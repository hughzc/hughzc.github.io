---
title: jvm
date: 2020-03-21 11:41:02
tags: jvm
categories: jvm
---

# JVM

## JVM介绍

Java Virtual Machine，JVM运行在操作系统上，与硬件没有直接的交互。

### JVM体系结构概览

{% asset_img jvm体系结构.png This is an example image %}

JVM包括2个子系统和2个组件，2个子系统为类加载器和执行引擎，2个组件是运行时数据区和本地接口。

整个流程为，编译期将Java代码转为字节码，类加载器将字节码加载进运行数据区中的方法区中，执行引擎来进行解析为系统指令，交由CPU执行，需要调用其他语言的本地接口来实现功能。

亮色：所有线程共享，存在垃圾回收；

灰色：线程私有，不存在垃圾回收。

分为3层来学习。

先是类装载器与执行引擎。

<!-- more -->

### 类装载器ClassLoader

{% asset_img 类加载器.png This is an example image %}

#### 介绍（是什么）

负责加载class文件，class文件在文件**开头有特定的文件标识**（cafe babe），将class文件字节码内容加载到内存中，并将这些内容转换成**方法区**中的运行时数据结构，且ClassLoader只负责class文件的**加载**，至于其是否可以运行，由Execution Engine决定。

Car.class由ClassLoader装载进内存，Car Class装载进方法区，一个模板Car Class，可以产生多个实例。类装载器有多个，有以下几类

#### 种类（有哪些）

**虚拟机自带的加载器**

- **启动类**加载器（Boostrap）C++写的

  Java的核心类库，提供JVM自身需要的类

  - JAVA_HOME/jre/lib/rt.jar，resources.jar，或sun.boot.class.path

- **扩展类**加载器（Extension）Java写的 

  派生于ClassLoader，父类加载器为启动类加载器

  - java.ext.dirs系统属性所指定的目录中加载
  - JDK安装目录的jre/lib/ext子目录加载，用户创建的JAR放在此目录下，也会自动由扩展类加载器加载

- **应用程序类**加载器（AppClassLoader）Java也叫系统类加载器，加载环境变量classpath的所有类或系统属性java.class.path指定路径下所有类

  派生于ClassLoader，父类加载器为扩展类加载器

  该类加载是程序中默认的类加载器

**用户自定义加载器**

Java.lang.ClassLoader的子类，用户可以定义类的加载方式

为了获取到Class文件，有三种方式

1、Object类中的getClass()方法

2、类的class属性，如Object.class

3、Class中的方法，Class.forName(String className)

获取到Class文件后，通过getClassLoader()可以获取到ClassLoader。

如果类是系统自带的，为Boostrap加载器，启动时便加载进了内存，打印出来为null；如果为自定义的类，打印出来为AppClassLoader。lib/ext包下的文件为拓展包。

自定义类为Person，获取到其Class文件，打印其classLoader及父类

~~~java
        Class clazz = Person.class;
        System.out.println(clazz.getClassLoader().getParent().getParent());
        System.out.println(clazz.getClassLoader().getParent());
        System.out.println(clazz.getClassLoader());
~~~

输出结果为

~~~java
null
sun.misc.Launcher$ExtClassLoader@16d3586
sun.misc.Launcher$AppClassLoader@dad5dc
~~~

{% asset_img 双亲委派.png This is an example image %}

#### 双亲委派（怎么用）

简单来说先从父类寻找，找到了就用，找不到再找子类。类加载从Boostrap至Extension再到AppClassLoader。

如果自定义一个java.lang包下的String类，默认会在Boostrap的ClassLoader中先找这个类，但是在rt.jar包中的String没有自定义写的main方法，因此会报错。

设立目的是不让自己写的类污染Java自带的类，**保障沙箱安全**。

详细介绍为：当一个类收到了**类加载请求**，首先**不会尝试自己去加载此**类，而是将这个请求**委托给父类去完成**，每一个层次类加载器都如此，因此所有的加载请求都应该传送到启动类加载器中，只有当父类加载器反馈自己无法完成这个请求时（在其加载路径下没有找到所需加载的Class），子类加载器才会尝试自己去加载。

#### 沙箱安全（什么用）

保证了使用不同的类加载器，最终都是委托给**顶层的启动类加载器进行加载**，可以保障得到的类的安全性。

执行引擎负责解释命令，提交给操作系统执行。

#### 类装载的执行过程

1. 加载：根据查找路径找到相应的class文件然后导入

   - 通过全限定名获取对应的二进制字节流
   - 将此字节流所代表的静态存储结构转化为方法区的运行时数据结构
   - 内存中生成一个代表此类的**java.lang.Class对象**，作为方法区的这个类的各种数据访问入口

2. 验证：检查加载的class文件的正确性（CafeBaBe）

3. 准备：给类中的**静态变量**分配内存空间并设置默认初始值

   不包含用final修饰的static，因为其为常量，在编译阶段就分配了，准备阶段显示初始化

   不为实例变量分配初始化，类变量分配在方法区，实例变量随Java对象一起分配到堆中

4. 解析：虚拟机将常量池中的符号引用替换为直接引用的过程。符号引用可理解为一个标识（字符串），在直接引用中直接指向内存的地址。此过程称为静态解析

   常量池中的符号引用转为直接引用有类装载时候的静态解析和方法入栈时候的动态链接

5. 初始化：对静态变量和静态代码块执行初始化过程

   执行类构造器方法`<clinit>`()的过程，此方法不需要定义，是javac编译器自动收集类中的所有类变量的赋值动作和静态代码块中的语句合并而来。

   构造器方法中的指令按语句在源文件中出现的顺序执行

   若该类有父类，JVM会保证子类的`<clinit>`()执行前，父类的`<clinit>`()已经执行完毕

   虚拟机需要保证一个类的`<clinit>`()方法在多线程下被**同步加锁**

其中验证、准备、解析属于链接阶段。

#### 自定义类加载器

自定义类加载器的目的

- 隔离修改类

  中间件的使用

- 修改类加载的方式

- 扩展加载源

- 防止源码泄漏

步骤

1. 继承抽象类java.lang.ClassLoader，实现自己的类加载器
2. JDK1.2前重写loadClass()方法，之后不建议覆盖loadClass()方法，而是把自定义类的加载逻辑写在findClass()方法中
3. 若没有太复杂的需求，可以直接继承URLClassLoader类，避免自己编写findClass()方法及其获取字节码流的方式，使自定义类加载器编写更加简洁

### 执行引擎

- 执行方法区
- 读取程序计数器中行号对应的指令
- 开辟线程执行GC

### 本地接口

Java底层调用的方法为native方法，由**本地方法接口**提供，为了将其装载进内存并与普通方法区别，装载进特殊的栈中，即**本地方法栈**中（Native Method Stack）。方法具体的实现交给本地方法库。native是关键字，有声明，无实现。

本地接口的作用是融合不同编程语言为Java调用，在内存中专门开辟了区域处理标记为native方法，具体做法为Native Method Stack栈中登记native方法，在Execution Engine执行时加载**本地方法库**。目前该方法使用越来越少。

### PC寄存器

记录了方法之间的调用与执行情况。该程序接下来要去哪。

每个线程都有程序计数器，是线程私有的，指向方法区中的方法字节码（**正在执行的代码行号**），此值被**执行引擎**赋值。内存空间很小，几乎可忽略（不存在GC），**是当前线程所执行的字节码的行号指示器**。若执行的为Native方法，计数器是空。不会发生内存溢出（OutOfMemory=OOM）错误。

便于执行**线程的上下文切换**，如果没有程序计数器，当切换到这个线程的时候，便不知道该执行哪行代码。

若执行的是native方法，则计数器为空。因为native方法通过其他语言来实现，没有通过Java自然也就没有对应的字节码行号。

CPU时间切片，线程去争抢时间片来执行，经常会出现线程的中断和恢复，如果不是线程私有而是共享，则没法保存当前线程执行的行号，不方便上下文切换，设置成线程私有，不会出现干扰的情况。 

### 方法区

供线程共享的运行时内存区域，**存储每一个类的结构信息**（模板）。如运行时常量池（Runtime Constant Pool），字段和方法数据，构造函数和普通方法的字节码内容。

常量+静态变量+类信息。方法区中静态变量存储的是堆中对象的地址。

上面讲的是**规范**，不同虚拟机中实现不一样，最典型的为永久代（PermGen Space）和元空间（Metaspace）。

实例变量存在堆中，与方法区无关。

由字节码执行引擎去执行。

### 栈

栈管运行，堆管存储。

栈（先进后出），队列（先进先出）。

当执行一个方法时，进行压栈；当方法结束后，执行出栈。

栈也叫栈内存，主管Java程序的运行，栈在线程创建时创建，栈的生命周期跟随线程，对于栈不存在垃圾回收问题。是线程私有的。**8种基本类型的变量+对象的引用变量+实例方法都是在函数的栈内存中分配**。

栈帧：当一个方法入栈时，在栈中为其单独开辟一个空间，叫做栈帧，存放方法中的局部变量等。栈的数据结构为先进后出，结构与程序运行特点相符，先执行的方法后结束。

{% asset_img 栈帧.png This is an example image %}

栈帧中有4部分

- **局部变量表**：为局部变量开辟地址存储；对象地址（引用）

  定义为**数字数组**，主要为存储**方法参数**和定义在方法体内的**局部变量**

  数据类型包括：基本数据类型，对象引用，及returnAddress类型

  容量大小在编译期间确定

- **操作数栈**：存储操作数运算时需要的临时变量

  常量的计算，给局部变量赋值，从局部变量表中取值运算

- **动态链接**：符号引用转为直接引用

  这一步与类装载时均可以将符号引用转为直接引用，类装载时候的解析为静态加载，入栈时候为动态链接 ，也有说这一步实现了多态

- **方法出口**：指向此方法结束后应该跳转的方法出口

#### 栈的存储

栈帧中主要存储3类数据

- **本地变量**（Local Varibles）：输入参数和输出参数及方法内的变量
- 栈操作（Operand Stack）：记录出栈、入栈的操作
- 栈帧数据（Frame Data）：包括类文件、方法等等

#### 栈运行原理

每个**方法**执行的同时都会创建一个一个**栈帧**，先进后出，用于存储局部变量表、操作数栈、动态链接、方法出口等信息。方法从调用到执行完毕的过程就对应着一个栈帧在虚拟机中入栈到出栈的过程。

栈的大小和具体的jvm实现相关，通常在256k-756k之间，**约等于1Mb左右**。

可以通过-Xss:设定每个线程虚拟机栈的大小，一般256k足够，会影响并发线程数的大小

Exception in Thread “main” java.lang.StackOverflowError

是Error错误，

Error与Exception父类为Throwable。

- 如果线程请求栈的深度超过了虚拟机所允许的深度，报StackOverflowError
- 虚拟机栈可以动态扩展，如果扩展到无法申请足够的内存，会报OOM

#### 栈+堆+方法区的交互

HotSpot使用指针的方式来访问对象，栈中存放引用变量reference，存储对象的地址，Java堆中存放访问类元数据的地址（对象实例数据），方法区中存放对象类型数据。

#### 深拷贝与浅拷贝

浅拷贝：增加一个指针指向已经存在的内存地址

深拷贝：增加一个指针且申请一个新的内存，使这个指针指向新的内存

#### 堆与栈的区别

- 物理地址：**堆的物理地址分配不连续**，**栈的物理地址分配连续**
- 内存：堆因为不连续，分配内存在运行期间确定，大小不固定，一般远大于栈；栈为连续，分配内存在编译时确定，大小固定
- 存放内容：堆中存放对象的**实例**和**数组**，更关注数据的存储；栈中存放**局部变量**，**对象引用**，更关注方法运行
- 可见度：堆是所有线程共享的，栈是线程私有的，生命周期与线程相同

静态变量放在方法区，静态对象放在堆中

#### new对象发生的事情

Student s = new Student()；

1. 加载Student.class进内存
2. 在栈空间为s开辟空间
3. 在堆空间为学生对象开辟空间
4. 学生对象初始化
5. 将对象地址赋值给s

### 堆

逻辑上三部分：新生+老年+元空间（7为永久代），物理上为新生+养老

1. 新生代（New）
   - 伊甸园区（Eden Space）
   - 幸存者零区(Servivor 0 space)
   - 幸存者一区(Servivor 1 space)
2. 老年代(Old，Tenure)
3. **元空间**(Perm)

一个jvm实例只存在一个堆内存，大小可调。类加载读取了类文件后，需要把类、方法 、常变量放到堆内存中。

Java7之前为**永久代**，Java8后为**元空间**。

#### 堆new对象流程

新建对象在Eden区，当一直new对象，**Eden区达到阈值**，进行**MinorGC**=轻GC，Eden区基本清空，第一次幸存后的对象移动到**幸存者0区**，也叫S0=from区。当S0满了再进行垃圾回收，从S0到S1=to区，S0与S1进行一次交换（**from和to区，名称不是固定的，每次GC后会交换，谁空谁是to**）。**养老区满了**，开启Major GC（Full GC），Major GC多次，老年代空间执行了Major GC后仍无法新建对象，报OOM异常。

如果出现java.lang.OutOfMemoryError:Java heap space异常，说明Java虚拟机堆内存不够，原因有

1. **Java虚拟机的堆内存设置不够，可以通过参数-Xms，-Xmx调整**
2. **代码中创建了大量大对象，并且长时间不能被垃圾收集器手机（存在引用）**

{% asset_img 新生养老.png This is an example image %}

**新生代1/3**堆空间，**老年代2/3堆**空间。新生代中，Eden8/10空间，From和To各1/10（**8:1:1**）。

#### MinorGC（复制->清空->互换）

1. eden、From复制到To，年龄+1

当Eden区满的时候触发第一次GC，将活着的对象拷贝进From区，当Eden区再次触发GC的时候，扫描Eden区和From区，将这两个区域进行垃圾回收，经过这次回收后还存活的对象，直接复制到To区，（若有对象达到老年的标准，则复制到老年区），将这些对象的年龄+1。**分代年龄**记录在**对象头**的MarkWord中。

2. 清空eden、from

清空eden和from中的对象

3. from和to互换

from和to互换，原to成为下一次的from，部分对象来from与to区域间来回复制，若交换15次（由JVM参数MaxTenuringThreashold决定，默认15）还活着进入老年代。

#### 永久代（jdk7前）

方法区和堆一样，是线程共享的内存区域，虽然JVM规范将方法区描述为堆的一个逻辑部分，但其有别名为Non-Heap（非堆），目的是和堆分开。

堆HotSpot，很多开发者习惯将方法区称之为永久代，但严格本质上说二者不同，或者说使用永久代来实现方法区而已，永久代是方法区（相当于一个接口）的实现。jdk1.7中将原本在永久代的字符串常量池移走。

{% asset_img 永久区.png This is an example image %}

永久存储区是一个常驻内存区域，用于存放jdk自身携带的Class，Interface的元数据，存储的是运行环境必须的信息，被装载进此区域的数据不会被GC，关闭JVM才会释放此空间的内存。

#### 堆参数调整

{% asset_img 堆参数.png This is an example image %}

上面为jdk7的堆的图，逻辑上分为3块，但物理上堆只包括新生与老年。

调整物理上堆的参数

- -Xms：初始大小start
- -Xmx：最大大小max

调整新生代的参数

- -Xmn：new区，一般不调

{% asset_img 堆参数2.png This is an example image %}

Minor GC针对年轻代，Major GC针对老年代。

JDK8将永久代取消，改为**元空间**。

永久代与元空间区别在于：

- 永久代使用JVM的堆内存
- **元空间不在虚拟机中而是使用本机物理内存**

默认情况下元空间大小仅受**本地内存**限制。类的元数据放入native memory，字符串池和类的静态变量放入java堆中，这样可以加载多少类的元数据不再由MaxPermSize控制，而是由系统的实际可用空间来控制。

| -Xms                     | 设置初始分配大小，默认为物理内存的“1/64” |
| ------------------------ | ---------------------------------------- |
| -Xmx                     | 最大分配内存，默认为物理内存的“1/4”      |
| -XX:+PrintGCDetails      | 输出详细的GC处理日志                     |
| -XX:NewRatio=4           | 设置年轻代与老年代的比例为1:4            |
| -XX:SurvivorRatio=8      | 设置新生代Eden和Survivor区比例为8:2      |
| -XX:MaxTenuringThreshold | 从年代到到老年代经过GC次数阈值，默认15   |

将运行时数据区区（方法区、堆、栈、计数器、本地方法栈）抽象为Runtime类，其实例通过getRun()方法获取。

其设计为单例设计模式中的**饿汉式**。

~~~java
public class Runtime {
    private static Runtime currentRuntime = new Runtime();
    private Runtime() {}
    public static Runtime getRuntime() {
        return currentRuntime;
    }
}
~~~

参数阅览为

~~~java
		//获取CPU核数
        System.out.println(Runtime.getRuntime().availableProcessors());
        //返回Java虚拟机试图使用的最大内存量
        long maxMemory = Runtime.getRuntime().maxMemory();
        //返回Java虚拟机中的内存总量
        long totalMemory = Runtime.getRuntime().totalMemory();
        System.out.println("MAX_MEMORY="+maxMemory+"字节、"+(maxMemory/(double)1024/1024)+"MB");
        System.out.println("TOTAL_MEMORY="+totalMemory+"字节、"+(totalMemory/(double)1024/1024)+"MB");
~~~

实际中需要将**初始内存和最大内存设置为相同**，避免内存不稳定产生停顿。

#### OOM异常

当一个线程发生了OOM异常后，另一个线程是可以正常执行的，因为发生OOM异常的线程会清空其堆内存，不影响其他线程的使用。

### 调优

调优的目的：减少full gc，减少stop-the-world时间。

让朝生夕死的对象，尽量在年轻代被minor gc，而少在老年代被full gc。方法之一为调整年轻代与老年代的内存分配比例。

#### 常见jvm参数

> 来源于马士兵老师课程笔记

JVM的命令行参数参考：https://docs.oracle.com/javase/8/docs/technotes/tools/unix/java.html

HotSpot参数分类

> 标准： - 开头，所有的HotSpot都支持
>
> 非标准：-X 开头，特定版本HotSpot支持特定命令
>
> 不稳定：-XX 开头，下个版本可能取消

为了在发生OOM异常时，将堆信息dump出来，使用如下命令

~~~java
-XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=d:\dump
~~~

#### 调优前概念

- 吞吐量：用户代码时间/（用户代码执行时间+垃圾回收时间）
- 响应时间：Stop-the-World时间越短，响应时间越好

需要确定是吞吐量优先还是响应时间优先。

吞吐量优先的一般为：Pareller Scanvenge + Pareller Old

- 科学计算
- 吞吐量
- 数据挖掘

响应时间优先：1.8后为G1

- 网站GUI API

# GC

## Java内存泄漏

内存泄漏指的是不被使用的对象或内存一直占据着内存，理论上来说Java有GC垃圾回收，即不再被使用的对象会被垃圾回收，从内存中清除。

但即使这样也会存在内存泄漏，长生命周期的对象持有者短生命周期对象的引用，虽然短生命周期的对象不再需要，但因为场面生命周期的对象持有着他的引用而导致不能被回收。

## GC收集日志信息

在IDEA中，使用Configuration中的VM options来配置

注意：-Xms与数字1024m中间没有空格，

~~~java
-Xms:1024m -Xmx:1024m -XX:+PrintGCDetails 
~~~

{% asset_img 堆信息.png This is an example image %}

可以看到堆由年轻代，老年代与MetaSpace（元空间）组成。

### OOM

~~~java
        String str = "java";
        while (true){
            str += str + new Random().nextInt(88888888) + new Random().nextInt(999999999);
        }
~~~

然后设置-Xms10m -Xmx10m，得到如下结果

~~~java
Exception in thread "main" java.lang.OutOfMemoryError: Java heap space
~~~

{% asset_img OOM.png This is an example image %}

可以看到先GC，后Full GC。

XX:+PrintGCDatails

### 日志解读

~~~java
[GC (Allocation Failure) [DefNew: 2595K->320K(3072K), 0.0023459 secs] 2595K->743K(9920K), 0.0024277 secs] [Times: user=0.00 sys=0.00, real=0.00 secs] 
~~~

具体意思是：这次GC因为分配失败，发生在新生代，GC前内新生代内存占用2595k，GC后内存占用320k，新生代总共大小3072k（设置的10M的1/3），GC前堆内存占用2595k，GC后堆内存占用743k，JVM堆内存共有9920k，共耗时0.0024277秒，后面为用户、系统、实际耗时。

~~~java
[Full GC (Allocation Failure) [Tenured: 4684K->4580K(6848K), 0.0037896 secs] 4684K->4580K(9920K), [Metaspace: 2106K->2106K(4480K)], 0.0038318 secs] [Times: user=0.00 sys=0.00, real=0.00 secs]
~~~

具体意思是：回收前老年代4684k，回收后4580k，老年代总内存大小为6848k。 JVM堆内存公有9920k。

规律：【名称：GC前内存大小->GC后内存占用（该区总内存大小）

System.gc()显式调用系统的GC，并不是马上回收。一般不要用！

## GC介绍

GC为垃圾回收机制（分代收集算法）

- 次数上频繁收集年轻代
- 次数上较少收集老年代
- 基本不动元空间

JVM在GC时，大部分回收的是年轻代，GC按照回收的区域分为普通GC（minor GC）和全局GC（major GC or Full GC）。

### Minor GC和Full GC区别

- 普通GC（Minor GC）：只针对年轻代区域，因为大多数Java对象存活率不高，因此Minor GC非常频繁，一般回收速度也较快
- 全局GC（Full GC）：发生在老年代的垃圾回收机制，出现了Major GC，经常会伴随至少一次的Minor GC（不绝对）。**Major GC的速度一般比Minor GC慢10倍以上**。

### 对象可达性分析

#### 引用计数法（ReferenceCount）

- 判断对象的引用数量
- 每个对象实例有引用计数器，被引用+1，完成引用-1
- 任何引用为0的对象实例可被垃圾回收
- 优点：效率高
- 缺点：维护引用计数器，无法解决相互引用，主流JM并未使用

#### 根可达算法（Rootearching）

判断对象的引用链是否可达来决定对象是否被回收，从图论引入。从名为GC Roots的对象作为起始点，从这些节点向下搜索，搜素走过的路径就是引用链，若一个节点与GC Roots间没有引用链，则被判断为不可达的。

从Root开始，将所有可达的对象进行标记，标记完后不可达的对象就是垃圾。

可以用作GC Root的对象

- 虚拟机栈中引用的对象

  如方法中引用到的对象

- 方法区中的常量引用的对象

  方法区中定义的常量为对象地址，被保存的对象可以是root

- 方法区中类静态属性引用的对象

- 本地方法栈中Native方法引用的对象

- 活跃现成的引用对象

## 4大算法

1. 引用计数法（微软的COM/ActionScript3/Python）

   对象的引用为0时回收

   - 需要维护引用计数器，且计数器本身有一定消耗；
   - 较难处理循环引用。
   - JVM实现一般不采用这种方式

2. 复制算法（Copying）

   用在**年轻代**中，Minor GC，复制-交换-清除，前提是大部分对象可以被清除

   -XX:MaxTenuringThreshold-设置对象在新生代中存活次数，默认15

   优点：**不会产生内存碎片**，速度快

   缺点：

   - **浪费一半空间**
   - 若对象存活率很高，复制非常花时间（老年代对象存活率高，不适合用）

3. 标记清除（Mark-Sweep）

   **老年代**一般是标记清除或标记清除与与标记整理（压缩）的混合实现

   分为标记和清除两个阶段，先标记出要回收的对象，然后统一回收这些对象

   优点：不需要额外空间

   缺点：

   - 两次扫描，**耗时严重**
   - 会产生**内存碎片**

4. 标记压缩（Mark-Compact）

   **老年代**一般是标记清除或标记清除与与标记整理（压缩）的混合实现

   也叫标记清除压缩

   标记和标记清除一样，压缩，再次扫描，往一段滑动存活对象

   优点：没有内存碎片

   缺点：需要移动对象的成本，效率不高

   可以结合标记清除与标记压缩，进行多次标记清除GC后，再进行标记压缩

没有最好的算法，根据不同代的特点用不同的垃圾收集算法。即**分代收集算法**。

内存效率：复制算法>标记清除算法>标记压缩算法（简单比较时间复杂度，实际不一定）

内存整齐度：复制算法=标记压缩算法>标记清除算法

内存利用率：标记压缩算法=标记清除算法>复制算法

## 常见垃圾收集器

### 相关概念

介绍垃圾收集器前需要介绍两个常见概念，stop the world和Safepoint，与垃圾收集器的工作过程相关

#### Stop-the-World

- JVM由于要执行GC，需要将应用程序停止
- 所有的GC算法中都会发生
- 多数GC优化通过减少stop-the-world时间来提升性能，达到高吞吐量、少停顿的特点

为什么要stop the world？

若没有stop-the-world，可能之前可达的对象因为应用程序的继续进行而变成浮动的不可达对象，这样需要重新进行标记，增加了GC算法的难度，降低程序性能，因此使用stop-the-point，可以减少GC算法复杂度。

#### Safepoint

在GC时，程序不是随便停止，而是在Safepoint停止

- 分析过程中对象引用关系不发生变化的点

- 产生的地方：方法调用，循环跳转，异常跳转等

- 安全点数量得适中

  太少会让GC等太久，太多会增加程序运行的负担

### 常见收集器

{% asset_img 收集器.png This is an example image %}

图中为常见的JVM收集器，用实线相连代表可以搭配使用

**1.8默认是Paraller Scavenge+ParallerOld**

### 年轻代常见收集器

#### Serial收集器（-XX:+UseSerialGC，复制算法）

最基本也最悠久的，单线程，进行垃圾收集时**停止所有工作线程**

Client模式下年轻代默认的

**Serial + Serial Old的组合**，用于**小型**程序，**几十兆**内存

{% asset_img 年轻1.png This is an example image %}

#### ParNew收集器（-XX:+UseParNewGC，复制算法）

多线程收集，多核下比Serial有优势

在Server模式下非常重要，与Serial一样均可配合CMS使用

PerNew + SerialOld组合，很少使用

{% asset_img 年轻2.png This is an example image %}

#### Paraller Scavenge(-XX:+UseParallelGC，复制算法)

ParellerOld + Pareller Scanvenge，1.8默认

前面两个更关注系统的停顿时间，此收集器更关注系统的吞吐量（执行用户线程时间/CPU总执行时间），适合后台运算不需要太多交互的情况

多核下执行有优势，Server下默认的年轻代收集器

不能与CMS搭配使用，因为框架不兼容

{% asset_img 年轻3.png This is an example image %}

### 老年代常见收集器

#### SerialOld（-XX:+UseSerialOldGC，标记整理算法）

Serial GC的老年代版本，单线程收集

**Client**模式下默认的老年代收集器 

{% asset_img 老年1.png This is an example image %}

#### ParellerOld（-XX:+UseParellerOldGC，标记整理算法）

ParellerOld + Pareller Scanvenge，1.8默认【PS + SerialOld】

多线程，吞吐量优先，配合ParallerScavenge使用

#### CMS（-XX:+UseConcMarkSweepGC，标记清除算法）

**Parnew + CMS + SerialOld**

Concurrent Mark Sweep收集器，适用于程序对停顿比较敏感的情况，号称“**最短用户停顿时间**”，适合用在互联网站或者B/S系统服务器上。

{% asset_img cms.png This is an example image %}

- **初始标记**：标记处GC Roots直接关联到的对象，速度较快，**需要停顿**
- 并发标记：进行GC Roots Tracing，标记出全部的垃圾对象，与应用线程一起
- 并发预清理：查找执行并发标记阶段从年轻代晋升至老年代对象，减少重新标记工作
- **重新标记**：修正并发标记期间对象变化情况，**需要停顿**，较慢
- 并发清除：标记清除算法清除标记
- 并发重置：重置CMS收集器的数据结构

问题是不压缩存活对象，有碎片化问题。当碎片达到一定程度，CMS老年代分配对象分配不下时，使用SerialOld进行老年代回收。问题较多，没有一个版本默认，需要手工指定。



#### 堆内存垃圾收集G1（-XX:+UseG1GC，复制+标记整理算法）

用于年轻代与老年代，为了替代CMS。JDK9默认。

- 并行和并发的：采用多个CPU缩短stop-the-world停顿时间，与用户线程并发执行
- 分代收集：采用不同方式处理新建对象和存活一段时间对象收集
- 空间整合：基于标记整理，解决了碎片化问题
- 可预测的停顿：让使用者指定停顿时间

将整个Java堆内存划分为多个region，年轻代与老年代不再物理隔离。

## 常用软件

### VisualVM

在IDEA的插件中安装VisualVM，在VisualVM中依照官网给的插件网址更新插件下载url，安装Visual GC插件。

可以看到具体的GC情况。

{% asset_img gc.png This is an example image %}

可以看到具体的GC次数，每个区域剩余的空间。

{% asset_img vmoom.png This is an example image %}

可以具体看到，在Eden区发生了20次GC，在Old区发生了11次GC，老年代Full GC次数更少，但是消耗的时间更长，老年代内存占用达到了73%，然后发生了OOM

为了具体分析OOM异常，可以在run，VM设置中加上

~~~java
-XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=d:\dump
~~~

这样会在指定的路径生成OOM异常后的dump文件，文件名为

java_pid12332.hprof

指明了是java的12332进程出现的。

然后在VisualVM中，装载此文件，打开如下

{% asset_img mainoom.png This is an example image %}

可以看到是主线程中发生了OOM异常，点进去看到

{% asset_img sboom.png This is an example image %}

发现是StringBuilder中的添加方法出现了问题，进一步点进去分析

{% asset_img 值oom.png This is an example image %}

可以看到具体的出问题的变量值，这样便可以找到相应的位置，去进行修改。

## 常见问题

### 强引用、软引用、弱引用、虚引用

不同的引用下GC不同

设置：-Xms:10m -Xmx:10m -XX:+PrintGCDetails

#### 强引用（Strong Reference）

- 最普遍的引用：Object o = new Object();
- 只有对象不被引用时才被回收
- 若仍被引用，抛出OOM异常程序也不会回收具有强引用的对象
- 将对象设置为null来弱化引用，使其被回收，或等待其生命周期结束

~~~java
        String str = "java";
        while (true){
           str += str + new Random().nextInt(88888888) + new Random().nextInt(999999999);
        }
~~~

日志

~~~java
[GC (Allocation Failure) [DefNew: 2669K->0K(3072K), 0.0007536 secs][Tenured: 5998K->4663K(6848K), 0.0019593 secs] 5998K->4663K(9920K), [Metaspace: 2106K->2106K(4480K)], 0.0027569 secs] [Times: user=0.00 sys=0.00, real=0.00 secs] 

[Full GC (Allocation Failure) [Tenured: 4663K->4559K(6848K), 0.0024536 secs] 4663K->4559K(9920K), [Metaspace: 2106K->2106K(4480K)], 0.0024861 secs] [Times: user=0.00 sys=0.00, real=0.00 secs] 

Exception in thread "main" java.lang.OutOfMemoryError: Java heap space
~~~

会报OOM异常，先GC，最后Full GC，不能将强引用的对象强制回收

#### 软引用（Soft Reference）

- 对象处在有用但非必须的状态

- 当**内存空间不足**，GC会回收该引用的对象的内存

- 可以用来实现内存敏感的高速**缓存**

  将经常用到的内容放在缓存中，提高效率，但如果空间不足，GC时可以将此软引用指向的对象进行回收，节省空间。兼顾时间与空间的效率。

~~~
SoftReference<byte[]> m =  new SoftReference<>(new byte[1024*1024*10]);
m.get();
~~~

m与SoftRefence（sr）对象强引用，sr与定义的字符数组虚引用，通过m.get()方法可以获得其引用。

下面看其被GC情况。

~~~java
        String str = "java";
        while (true){
            SoftReference<String> str2 = new SoftReference<String>(str + new Random().nextInt(88888888) + new Random().nextInt(999999999));
        }
~~~

日志

~~~java
[GC (Allocation Failure) [DefNew: 2752K->0K(3072K), 0.0002984 secs] 3327K->575K(9920K), 0.0003464 secs] [Times: user=0.00 sys=0.00, real=0.00 secs] 
~~~

可以看到，没有老年代的GC信息。

不会报OOM异常，会一直GC，没有Full GC。

#### 弱引用（Weak Reference）

- 非必须引用的对象，比软引用更弱
- GC时会被回收（无论当前线程是否紧缺）
- 被回收的概率也不大，因为GC线程优先级较低
- 适用于偶尔被使用且不影响垃圾回收的对象

~~~java
WeakRference<M> m =  new WeakReference<>(new M());
m.get();
~~~

使用m.get()方法可以得到其引用。

在ThreadLocal中的Entry里，key对ThreadLocal对象为弱引用，可以防止内存泄漏。

#### 虚引用（PhantomReference）

Phantom虚幻的，使用get()方法得不到。

- 不会决定对象的生命周期
- 任何时候都可能被垃圾收集器回收
- 跟踪对象被垃圾收集器回收的活动，起哨兵作用
- 必须和引用队列ReferenceQueue联合使用

~~~java
        String str = "java";
        ReferenceQueue<String> queue = new ReferenceQueue<>();
        PhantomReference<String> pr = new PhantomReference<>(str,queue);
~~~

通过判断队列中是否加入了虚引用来了解被引用的对象是否被GC。

作用是**管理堆外内存**，在传统的网络通信中，网卡数据写给OS，OS写入Buffer中，再复制给JVM，JVM要向网络传输数据，先 复制到OS的Buffer中，再写给网卡。这个Buffer复制到JVM的过程是不必要的，在NIO中，提供了直接内存管理或堆外内存管理。利用JVM管理操作系统中的堆外内存，不用再次拷贝，称为zerocopy，提高效率。有对象DirectByteBuffer来指向堆外内存，为了方便回收堆外内存的垃圾回收，给DirectByteBuffer加上虚引用，配合队列使用，当队列中有内容，说明DirectByteBuffer有对象被回收，需要相应清理堆外内存。

### 对象何时进入老年代

一般新建对象时发生在Eden区，但是某些情况对象会进入到老年代

1. 新建大对象（需要大量连续内存空间的对象，如很长的字符串或数组）直接进入老年代

2. 长期存活对象进入老年代（默认对象年龄为15进入,如静态常量）

3. survivor区空间不足（相同年龄的所有对象大小总和大于Survivor空间**一半**)

   这个比例可以通过-XX:TargetSurvivorRatio指定
   
   这一步称为对象动态年龄判断，规则是希望可能长期存活的对象今早进入老年代。此机制一般在minor gc后触发。
   
   一些朝生夕死对象，因为survivor区域设置不足，而直接放入老年代，而频繁进行full gc，这种情况可以调大年轻代空间，改变年轻代与老年代比例。-XX:NewRatio，调小。