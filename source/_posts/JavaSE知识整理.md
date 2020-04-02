---
title: JavaSE知识整理
date: 2020-03-03 22:00:20
tags: JavaSE
categories: JavaSE
---

# Java基础

根据网上博客和黑马的一些资料，整理了下面的JavaSE的知识点

> https://blog.csdn.net/ThinkWon/article/details/104390612
>
> 《黑马程序员Java学习》

<!-- more -->

## Java基本语法、特性

### 数据类型

#### Java有哪些数据类型

Java中数据类型分为基础数据类型与引用数据类型

- 基础数据类型
  - 数值型
    - 整数类型，**默认int**
      - byte：1字节
      - short：2字节
      - int：4字节
      - long：8字节
    - 浮点数类型，**默认double**
      - float：4字节
      - double：8字节
  - 字符型
    - char：2字节，用单引号括
  - 布尔型
    - boolean：1字节，只有2个值
- 引用数据类型
  - 类(class)
  - 接口(interface)
  - 数组([])

#### float与double

float f = 1.1;会报错吗？

会，1.1默认为double，这里相当于向下转型，造成精度损失，需要更改为float f = 1.1f或者float f = (float)1.1

#### short s1 = 1; s1 = s1 + 1;有错吗?short s1 = 1; s1 += 1;有错吗

s1 = s1+1有错，因为1为int类型，不能将int隐式的转换为低精度的short，所以需要类型转换。

s1 += 1，这里相当于强制类型转换，s1 = (short)(s1 + 1)，使用 ++ 或者 += 相当于执行了类型转换

#### Math.round(11.5) 等于多少？Math.round(-11.5)等于多少

Math.round(11.5)的返回值是 12，Math.round(-11.5)的返回值是-11。四舍五入的原理是在参数上加 0.5 然后进行下取整。

### 运算符

#### 最有效率的方式计算2乘以8

2 << 3，左移几位相当于2乘以几次方，位运算效率最高。

而`>>`表示右移，右移几位就是除以2的几次幂，高位保持原来的数字，而对于`>>>`，无论高位是什么，用`0`来补齐，因此是无符号位右移。

#### 计算~6

`~6`相当于对6逐位取反，6全部取反+1得到-6，`~6=-7`

#### &和&&的区别

&运算符的作用是

1. 按位与 ，num1&num2相当于将每个数的二进制位相与
2. 逻辑与运算，boolean f1 & f2，两个同时为真返回真

而&&是短路的与运算，boolean f1 & f2，如果左边的f1为假，则直接返回假，屏蔽了f2，逻辑或（|）与短路运算符（||）作用也类似。

### 流程控制语句

#### switch的作用类型

在JDK5之前，switch(expr)中的expr只能是基础数据类型中的byte、short、int、char，在JDK5引入了枚举类型enum，在JDK7后，可以为字符串String，但长整形long不可以。因此总的为byte、short、int、char、enum、String。

#### break ,continue ,return 的区别及作用

break：结束当前的循环体，不再执行此循环

continue：跳出本次循环，继续执行下一个循环

return：结束当前的方法，直接返回

#### 在 Java 中，如何跳出当前的多重嵌套循环

给外面的循环定义一个标签，在需要跳出的地方使用break 循环名即可

~~~java
        loop1:
        for (int i = 0; i < 10; i++) {
            for (int j = 0; j < 10; j++) {
                if (j == 5)
                    break loop1;
            }
        }
~~~



### 修饰符

#### 四种访问修饰符及区别

用访问修饰符来保护对类、变量、方法和构造方法的访问，支持四种访问权限。

- public：对所有类可见，使用对象为类、接口、变量、方法
- default：默认的，在一个包中可见，使用对象为类、接口、变量、方法
- protected：对同一个包内和子类可见。使用对象为内部类，变量，方法，不能修饰外部类
- private：在同一个类内可见。使用对象为变量，方法，不能修饰外部类

|  修饰符   | 同一类中 | 同一包中 | 子类中 | 不同包中 |
| :-------: | :------: | :------: | :----: | :------: |
|  public   |    √     |    √     |   √    |    √     |
| protected |    √     |    √     |   √    |    ×     |
|  default  |    √     |    √     |   ×    |    ×     |
|  private  |    √     |    ×     |   ×    |    ×     |

## 关键字

### static

#### static作用

1. 创建独立于具体对象的域变量或者方法，随类而加载，可以直接用类名调用
2. 形成静态代码块优化程序性能。在类被加载时，按照static块出现顺序执行，只加载一次，可以将只需要进行一次的初始化操作放在static中

#### static特点

1. static是一个修饰符，修饰成员（成员变量和成员函数）
2. static修饰的成员被所有对象共享
3. static优先于对象存在，随类的第一次使用而加载，且只加载一次
4. static修饰的成员多了一种调用方式，即直接被类名调用
5. static变量值在类加载时分配空间，以后创建类对象时不会重新分配，可以对其任意赋值

#### static应用场景

1. 修饰成员变量
2. 修饰成员方法
3. 静态代码块
4. 修饰类（只能修饰内部类即静态内部类）
5. 静态导包

#### static注意事项

1. 静态方法只能访问静态成员，静态方法可直接被类名调用，非静态既可以访问静态，又可以访问非静态
2. 静态 方法中不能出现this与static
3. 主函数是静态的，不要在主函数中创建其他函数

#### 方法调用顺序

父类静态代码块->子类静态代码块->子类静态方法->父类非静态代码块->父类构造方法->子类非静态代码块->子类构造方法

### final

#### final作用

用于修饰类、属性和方法

- 被final修饰的**类不可以被继承**

  关键字final置于定义前

- 被final修饰的**方法不可以被重写**

  防止任何继承类修改其定义，处于设计的考虑

- 被final修饰的变量不可以被改变，**不可变的是变量的引用**，而不是引用指向的内容（针对对象）

  一般会加静态，全局变量。固定常量一律用final修饰。**不能改变的数据类型需要是基本数据类型**，对常量定义时需要对其赋值。对对象的引用表明此引用不能指向另一个对象，但对象本身可以被修改。

允许在参数列表中以声明的方式将参数指定为final，意味着无法在方法中更改参数引用所指向的对象。可以读参数，无法改参数，主要用来**向匿名内部类传递数据**。

#### final finally finalize区别

- final可以修饰类、方法、变量，修饰类表明该类不能被继承，修饰方法表明该方法不能被重写，修饰变量表明该变量为常量不能重新被赋值，但若修饰的是对象的引用，不可变的是引用。
- finally一般作用在try-catch代码块中，处理异常时将一定要执行的方法放在finally中，表示不管是否出现异常，该代码块都一定会执行（存在try语句，在try语句中没有执行System.exist(0)），一般放关闭资源的代码。
- finalize是Object类的方法，一般由垃圾回收器调用，当调用Syetem.gc()的时候，由垃圾回收器调用finalize()，回收垃圾，是一个对象是否可回收的最后判断，最后的自救机会，但只有一次。

### this与super

#### this关键字使用

this的含义：代表本类对象引用，可以理解成**指向对象本身的一个指针**。

this的用法

1. 普通的直接引用，this相当于当前对象本身

2. 构造函数中，形参与成员变量重名，用this区分

3. 在构造函数中引用本类的其他构造函数

   this(待传入参数，可为空)，只能放在构造函数的第一行，不可与super()同时出现

#### super关键字使用

super为指向超（父）类对象的指针，此超类为离当前类最近的一个父类。

super的用法与this类似

1. 普通的直接引用，使用super.xxx来引用父类成员

2. 子类中的成员变量与父类中成员变量重名时，用super()区分

3. 引用父类构造函数

   super(参数)，调用父类中构造函数，在子类的构造函数第一行默认为super()，如果有this()，则没有super()，因为二者均需要出现在构造函数第一行

#### this与super

1. this指当前对象，super指父类对象
2. this在本类中调用本来方法，super在子类中调用父类方法
3. this与super均要放在构造函数的第一行
4. this与super不能出现在一个构造函数中，因为this()中调用其他构造函数，其他构造函数中有super()
5. 二者均不可在static环境中使用，如static变量，static方法，static代码块

## 面向对象

### 面向对象思想

#### 面向对象和面向过程的区别

面向过程：具体化的，流程化的，为了解决问题，需要一步步分析和实现

面向对象：模型化的，将过程的实现抽象为多个类，直接去调用类的属性和方法，不用去一步步实现，面向对象的底层是面向过程，将面向过程抽象成类并进行封装，就是面向对象了

| 类型     | 优点                                   | 缺点                           | 应用场景                       |
| -------- | -------------------------------------- | ------------------------------ | ------------------------------ |
| 面向过程 | 性能更高，没有调用类的资源消耗。       | 没有面向对象易维护、复用、扩展 | 单片机，嵌入式开发，Linux/Unix |
| 面向对象 | 易维护、易复用、易扩展，系统耦合性更低 | 性能比面向过程低               | 需求变化多，互联网应用         |

#### 面向对象特征

面向对象的特征主要有4个方面，抽象，封装，继承和多态

抽象：将一类对象的**共同特征**总结出来构造类的过程，包括**数据抽象**和**行为抽象**两方面，抽象只关注对象的属性和行为，并不关心其实现细节。

封装：将一个对象的**属性私有化**，同时对外提供可以**访问属性的方法**。在类中编写方法对实现细节进行封装，隐藏一切可隐藏的，只对外暴露最简单的编程**接口**。

继承：从父类继承属性与方法到子类，子类可以增加新的数据和功能，方便复用，继承会增加类之间的耦合性。（里氏替换原则）

多态：某一种事物具有多种形态，用父类类型指向子类对象，一个对象对应着不同类型。

#### 面向对象的基本原则

1. 单一职责原则

   一个类只有一个职责，内部高内聚

2. 接口隔离原则

   一个类对另一个类的依赖建立在最小的接口上

3. 依赖倒转原则

   抽象不应该依赖细节，细节应该依赖抽象，**面向接口编程**。依赖关系的三种传递：接口传递、构造方法传递、setter方法传递

4. 里氏替换原则

   与继承相关，任何时候子类都能替换父类

5. 开放封闭原则

   模块和函数应该对**扩展开放**（对提供方），对**修改关闭**（对使用方），**用抽象构建框架，用实现扩展细节**

6. 迪米特法则（最少知道原则）

   一个类对自己依赖的类知道越少越好，陌生的类最好不要以局部变量形式出现在类的内部

7. 合成复用原则

   尽量使用合成/聚合的方式，而不是继承

### 继承

#### 继承的好处

1. 提高了代码的复用性
2. 让类与类之间有了联系，为多态提供了前提

#### 继承的注意事项

1. Java中支持单继承，不直接支持多继承

   多继承会出现父类成员变量调用的不确定性，可以多实现

2. 子类中有父类非private的属性和方法

3. 子类可以对父类的属性和功能进行扩展

4. 子类可以对父类方法进行覆盖

### 多态

#### 多态的特点与实现方式

程序中的引用变量所指向的具体类型和调用方法只有在程序运行时才能确定，这样可以实现引用变量绑定到不同的类上，让程序可以选择多个运行状态。

利用父类或接口引用变量指向子类或具体实现类的对象，提高程序的扩展性。

方法**重载**（overload）实现的是**编译**时的多态性（也称**前绑定**），根据参数列表的不同来区分不同的函数；方法**覆盖**（override）实现的是**运行**时的多态性（也称**后绑定**），运行时的多态是面向对象最精髓的（引用变量调用的方法只有在运行时才能确定）。

实现多态需要做到

- 继承（子类继承父类）

- 方法重写（子类重写父类中的**已有**或抽象方法）
- 向上转型（父类型引用子类对象，接口类型引用实现类的对象，相同的调用会根据子类对象不同而表现不同的行为）

### 抽象类与接口

#### 抽象类和接口的对比

从设计层面来说，抽象类是对**类的抽象**，是一种模板设计；接口是**行为的抽象**，是一种行为的规范

相同点

- 接口和抽象类**不能直接实例化**
- 位于继承的顶端，用于被其他类实现或继承
- 都包含抽象方法，其子类必须覆写这些抽象方法

不同点

| 参数       | 抽象类                   | 接口                                                 |
| ---------- | ------------------------ | ---------------------------------------------------- |
| 声明       | abstract关键字           | interface关键字                                      |
| 实现       | 子类使用extends继承      | 子类使用implements实现                               |
| 构造器     | 可以有                   | 没有                                                 |
| 访问修饰符 | 方法可以为任意访问修饰符 | 接口方法默认为public，不允许定义为private或protected |
| 多继承     | 只能继承一个类           | 可以多实现                                           |
| 抽象方法   | 可以有非抽象方法         | 全为抽象方法                                         |

选择抽象类或者接口，遵循如下原则

- 行为模型总是通过接口而不是抽象类定义，通常**优先使用接口**，少使用抽象类
- 选择抽象类的情况：需要定义子类的行为，又要为子类提供通用的功能

#### 普通类与抽象类区别

- 普通类中不能有抽象方法，抽象类中可以包含抽象方法
- 普通类可以直接实例化，抽象类不能直接实例化

#### 抽象类能用final修饰吗

不能，定义抽象类的目的是让其他类继承，而final关键字修饰的类不能被继承，二者相违背，因此抽象类不能被final修饰

### 对象

#### 创建一个对象用什么关键字？对象实例与对象引用有何不同？

创建类使用关键字new，对象实例存在于堆内存中，而对象引用指向对象实例，对象引用可以指向一个对象实例或者指向空，而一个对象实例可以有多个对象引用指向它。

### 变量

#### 成员变量与局部变量区别

成员变量定义在方法外部，局部变量定义在类的方法或代码块中。

- 作用域：成员变量针对整个类有效，局部变量只在方法或代码块中有效
- 存储位置：成员变量存储在堆内存中，局部变量存储在栈中
- 生命周期：成员变量周期与对象一样，局部变量周期与方法一样
- 初始值：成员变量默认有初始值，局部变量没有默认初始值，使用前需要赋值
- 使用原则：就近原则，先在局部范围找，接着在成员中找

#### 静态变量和实例变量区别

静态变量，随类的加载而加载，被当前类所有对象共享，内存中只有一份。

实例变量：随每次创建对象而创建，有几个对象就有几个实例变量。

### 方法

#### main方法

public static void main(String[] args)

main方法是Java程序的入口，public表示任何类和对象可以访问，static表示方法随类而加载，可使用类名来调用，void表明没有返回值，main为JVM识别的特殊方法名，不是关键字。字符串数组args可以用来输入参数。main为最先加载的方法（不一定最先执行），因此需要被静态调用。

是否有其他写法？

public与static可以交换顺序，也可以定义为final，也可以用synchronized来修饰main方法。

#### 在main方法执行前输出

可以使用静态代码块，静态代码块在类加载时被调用

~~~java
public class Test{
	static{
		Sout("Hello1");
	}
	main(){
		Sout("Hello2");
	}
}
~~~

#### 函数的重载与覆盖

都是实现多态的方式。

- 重载：重载发生于**一个类**中，若同名的方法有不同的输入参数列表（参数类型不同，参数个数不同或均不同）视为重载，编译时的多态性，前绑定
- 覆盖：也称为覆写，重写，override，发生在**子类和父类**之间，要求子类重写方法与父类有相同的参数列表和返回类型，子类方法权限大于等于父类方法（父类方法不能被private修饰），子类方法声明异常不能多于父类异常（其子类或子集）。运行时的多态性，后绑定，精髓。

#### 静态方法和实例方法有何不同？

1. 调用方式不同，实例方法只能通过对象.方法调用，而静态方法多了类名.方法，无需创建对象。
2. 访问成员变量限制，在静态方法中只能访问静态成员（静态成员变量和静态方法），不能访问实例成员，而实例方法没有此限制。

#### == 和 equals 的区别是什么

==比较的是地址，equals分两种情况

- 该类覆写了equals方法，比较的是值或内容
- 该类没有覆写equals方法，比较的仍然为地址

#### hashCode 与 equals

将对象加入Set或Map集合中，必须要覆写hashCode或equals方法。通过hashCode计算哈希散列值，寻找在桶中的位置，然后在该桶处的链表或树上通过equals方法来寻找是否有相同的节点，如果有就进行替换（HashMap的put方法逻辑）。覆写hashCode与equals需要确保

- 两个相等对象的哈希值相同
- 两个相等对象返回equals返回true
- 哈希值相等的对象不一定相等

这样在重写equals时必须重写hashCode，哈希值不等的对象即使有相同的数据也不等。

### 构造方法

#### 在Java中定义一个不做事且没有参数的构造方法的作用

在执行子类的构造方法前，默认执行super()，若父类中只有有参数的构造方法，则编译会报错，因为程序在父类中找不到相应方法来执行，因此需要在父类中加上一个不做事的空参的构造方法

#### 在调用子类构造方法之前会先调用父类没有参数的构造方法，其目的是？

子类与父类有较强的耦合性，在初始化子类前，需要相应对父类进行初始化，此函数可以帮助子类做初始化工作

#### 一个类的构造方法的作用是什么？若一个类没有声明构造方法，该程序能正确执行吗？为什么？

构造方法的主要作用是完成对类对象的**初始化**工作。若没有显式声明构造方法，程序也能正确执行，因为每个类中有一个默认的空参的构造方法，但当自己定义了构造方法后，就没有该默认构造方法了。

#### 构造方法有哪些特性？

- 名字与类相同
- 没有返回值，不能用void声明函数
- 在new对象时自动调用该方法

### 内部类

#### 内部类定义

将类定义在内的内部，方便访问外部类中成员。外部类要访问内部类中成员，必须新建外部类对象。内部类可以访问外部类中数据的原理是持有外部类的this引用。

#### 内部类种类

- 成员内部类（成员位置上非静态类）

  需要先创建外部类对象，再创建内部类对象

  ~~~java
  Outer.Inner in = new Outer().new Inner();
  ~~~

- 局部内部类（定义在方法中的内部类）

  可访问外部类中所有方法

- 匿名内部类（没有名字的内部类，开发较多）

- 静态内部类（static修饰）

  不可以访问外部类中的非静态变量，外部类可通过外部类的对象调用，不用新建内部类对象

  ~~~java
  Outer.StaticInner inner = new Outer.StaticInner();
  ~~~

#### 匿名内部类

- 必须继承一个抽象类或接口
- 不能定义任何静态成员与静态方法
- 所在方法的形参被匿名内部类使用，需要用final修饰
- 匿名内部类不能是抽象的，必须实现继承的类或实现的接口所有的抽象方法

创建方式如下

~~~java
new 类/接口{ 
  //匿名内部类实现部分
}
~~~

#### 内部类优点

- 方便访问其外部类中所有数据
- 不被其他包中的类所见，封装性较好

#### 局部内部类和匿名内部类访问局部变量的时候，为什么变量必须要加上final？

因为生命周期不同，局部变量存储在栈中，在方法执行结束后，非final的局部变量被销毁，但局部内部类对此变量引用仍然存在，这样当要调用的时候就会出错，增加final可以延长局部变量的生命周期。

### 值传递

值传递指在方法调用时，传递的是参数列表值的拷贝

引用传递指方法调用时，传递的是引用的地址

#### 当一个对象被当作参数传递到一个方法后，此方法可改变这个对象的属性，并可返回变化后的结果，那么这里到底是值传递还是引用传递

值传递，Java语言的传递只支持值传递，当对象实例被传入方法，方法得到的只是参数的拷贝，因此方法中参数的值为此对象实例的一个引用，对象的属性可以在被调用时改变，但对对象引用的改变无法传递给调用者。

- 方法不能修改一个**基本数据类型**的参数（即数值型或布尔型》
- 方法可以改变一个对象参数的状态（存储在堆内存中）
- 方法不能让对象参数的引用

### 常见类

#### String的equals、==与intern

==的作用

1. 判断基础数据类型的
2. 判断引用是否指向堆内存的同一地址

equals方法在Object类中，String类中对其进行重写，判断两个String对象内容是否相同。先比较地址，再每个字符进行比较。

String类中有对应的String池即String pool，每个内容相同的字符串对应一个pool中的对象。

String的三种比较

比较1：值相同，对象不同

~~~java
String s1 = new String("abc");
String s2 = new String("abc");

System.out.println(s1==s2);            //false
~~~

当使用new，就是新建对象，s1与s2指向堆中的不同空间，因此使用==判断为false。

比较2：

~~~java
String s1 = new String("abc");
String s2 = "abc";

System.out.println(s1 == s2);//false
~~~

s1新建时，在pool和堆内存中都新建了对象，而s1指向堆内存中的对象，由于“abc”已经存在于常量池，因此s2指向的是常量池中的对象，一个指向堆，一个指向常量值，因此地址不同。

比较3：均不使用new

~~~java
String s1 = "abc";
String s2 = "abc";

System.out.println(s1==s2);            //true
~~~

s1新建时，常量池中没有“abc”，会在常量池中新建，s1指向常量池中对象，而s2也指向常量池中对象，因此二者地址相同。

而String中的intern方法，返回字符串对象的规范化表示形式，即此字符串对应常量池中的对象。

~~~java
String s = new String("abc");
String s1 = "abc";
String s2 = new String("abc");
System.out.println(s == s1.intern());//false
System.out.println(s == s2.intern());//false
System.out.println(s1 == s2.intern());//true
~~~

intern的具体步骤是，当字符串s在常量池中不存在对应对象，创建并返回常量池对象；若常量池中存在对应对象，则返回常量池中的对象。

s指向堆内存，s1.intern指向常量池，二者不相同；s指向堆内存，s2.intern指向常量池，二者不相同；s1指向常量池，s2.intern指向常量池，二者相同。

~~~java
String hello = "hello";
String hel = "hel";
String lo = "lo";
System.out.println(hello == "hel" + "lo");//true
System.out.println(hello == "hel" + lo);//false
~~~

hello指向的是常量池中的徐爱那个，而“hel”+“lo”也指向常量池中对象，而lo+“hel”，相当于调用StringBuilder的append方法，重新在堆中生成了一个对象，因此地址不同。

#### String不变性的理解

- String类被final修饰，不能被继承
- 用+号拼接字符串后，会创建新的字符串
- String s = new String(“hello”)可能创建一个或两个对象。若静态区中有“hello”字符串常量对象，仅在堆中创建一个对象。若静态区中没有“hello”对象，堆上和静态区中都需要创建对象
- 在Java中，通过+拼接字符串，底层会转为StringBuilder实例的append()方法来实现

#### String重写equals而不重写hashCode的问题

在equals()被重写时，通常有必要重写hashCode()方法，以维护hashCode()方法的约定，即相对等的两个对象要有相同的hashCode，若只重写equals而不重写hashCode，在将String类对象加入Set或Map集合时，会先判断hashCode，再判断equals，但因为没有重写hashCode，会在集合中存储两个值相同的对象，导致混淆。

#### String、StringBuffer、StringBuilder区别

- String、StringBuffer与StringBuilder均为final类，不允许被继承
- 字符串拼接时，String类可使用+号，而另外两个类需要使用append()方法
- String长度不可变，而StringBuffer与StringBuilder长度可变
- StringBuffer是线程安全的，而StringBuilder不是，StringBuffer在StringBuilder方法上加入了synchronized修饰，因此StringBuffer性能更低。多线程时使用StringBuffer，单线程下使用StringBuilder。
- 若String类型的字符串，在编译时就可以确定为字符串常量，在编译完成后，字符串会自动拼接为一个常量，此时String性能会更好

## 集合

Java中的内存泄漏多与集合容器相关。当集合中持有生命周期较短的对象，当对象作用已经结束，需要被销毁时，因为有长生命周期的集合持有，如果不进行手动销毁，会存在内存泄漏。

### List与Set

Collection下有List、Set和Queue，重点说明List与Set的区别。

{% asset_img list与set.png This is an example image %}

一、特点

按照特点来看

- List：元素有序（存入和取出顺序一致），可以索引（角标）操作元素，元素可以重复
- Set：元素不能重复，无序

二、种类

- List

  - 底层是数组，查询快，增删慢
    - Vector，线程安全，效率低
    - ArrayList，线程不安全，效率高
  - 底层是链表，查询慢，增删快
    - LinkedList，线程不安全，效率高

- Set

  - 底层是哈希表

    HashSet，保证元素唯一性，存入的类需要覆写hashcode()和equals()方法

    先判断hashcode()，再判断equals()

  - 底层是二叉树

    TreeSet，保证元素排序，两种排序方式，比较器优先使用

    - 自然顺序，让对象所属的类实现Compareble接口，覆盖compareTo()方法，无参构造
    - 让集合自身具备比较功能，比较器实现Comparator接口，覆盖compare()方法

其中，Vector虽安全，但不适用于高并发。HashSet底层为HashMap，在add元素时，以键的形式放入，值为PRESENT。TreeSet的核心在排序，不排序用HashSet。TreeSet有两种排序方式，方式一是被排序类实现Compareble接口（实现equals()、hashcode()、compareTo()方法），方式二为传入比较器，比较器需要实现Comparator接口（实现compare()方法）。TreeSet底层为TreeMap，两种排序方式比较器优先级较高。

#### ArrayList 和 LinkedList 的区别

1. 数据结构实现：ArrayList底层为动态数组（可动态扩容），LinkedList底层为双向链表
2. 随机查询效率：ArrayList随机查询效率更高，而LinkedList只能依靠遍历，因此ArrayList查询效率更高
3. 增加和删除效率：在增加和删除非首尾元素时，LinkedList效率更高，因为ArrayList的增加和删除操作会影响到其他位置的元素，涉及元素的移动
4. 内存空间占用：LinkedList比ArrayList更占内存，因为LinkedList中除了存放数据，还有指向前后的引用
5. 线程安全方面：二者都是不安全的

因此，如果涉及频繁的查询操作，ArrayList更合适；涉及频繁的增删操作，LinkedList更合适

#### ArrayList 和Vector 的区别

1. 线程安全：Vector使用synchronized保障线程安全，ArrayList不是
2. 性能：因为Vector存在锁，因此性能不如ArrayList
3. 扩容：ArrayList和Vector均可以动态扩容，Vector新容量为原来的2倍，而ArrayList新容量为原来的1.5倍

在不需要线程安全时，建议使用ArrayList。

如果想在多线程下使用ArrayList，可以使用Collections.synchronizedList(list)方法

#### List遍历时删除问题

如果直接for循环遍历List，再进行remove操作，会报ConcurrentModificationException，因为使用了foreach语句时，会自动生成一个Iterator来遍历list，但同时该list又在被iterator.remove()方法修改，不允许在其他地方进行删除，因此这样会出问题。

解决办法1：使用迭代器来获取List中全部元素，再调用iterator的remove方法

~~~java
Iterator<Integer> it = list.iterator();
while(it.hasNext()){
   *// do something*
   it.remove();
}
~~~

解决办法2：利用下标来获取元素，再使用删除

~~~java
		for (int i = 0; i < list.size(); i++) {
            list.remove(list.get(i));
        }
~~~

#### HashSet实现原理

HashSet基于HashMap而实现，HashSet中的值存放于HashMap的key上，HashMap的value全部为PRESENT，HashSet底层方面基本是调用HashMap来实现的。

### HashMap

HashMap为Java中非常重要和常用的一种数据结构，下面将重点介绍HashMap的底层原理及方法实现。

Java8以前：数组+链表

{% asset_img 链表hash.png This is an example image %}

操作为非同步，效率较高。**数组默认16**，存储链表头节点，数组位置的获得方式是hash(key.hashCode())%len，计算键的哈希值再算出%数组长度的值。实际中通过位运算来实现。其中数组的类型为Entry<K,V>，当发生哈希冲突时，则将数组中的Entry设置为新值的next，新值放在数组中，旧值在新值的链表上。

最坏的情况下，链表会集中在一个数组，这样查找的时间复杂度从O(1)变为O(n)。

Java8及以后：链表+数组+红黑树

{% asset_img 红黑树hash.png This is an example image %}

当数组中某个位置链表长度大于TREEIFY_THRESHOLD常量时，链表转换为红黑树，最坏情况下的性能从O(n)提高到O(logn)。数组类型变为Node，当链表大小**超过**TREEIFY_THRESHOLD（**默认是8**）时，链表被改造成红黑树。当红黑树元素被删除**低于**UNTREEIFY_THRESHOLD（**默认是6**）时，红黑树被转成链表。HashMap在首次使用时才被初始化，可以扩容，当插入一个元素，若对应位置数组没有元素，则新建；如果有，则判断是树还是链表，按照相应方式插入。若链表元素超过阈值，则树化；若插入值的键重复，就更新值。

#### put方法

HashMap的put方法逻辑是：

1. 若HashMap未被初始化 ，进行初始化
2. 对key求hash值，计算下标
3. 如果没有碰撞，直接放入桶中
4. 如果碰撞了，以链表的方式链接到后面
5. 如果链表长度超过阈值（8），把链表转成红黑树
6. 如果链表的长度低于6，把红黑树转成链表
7. 如果key对应节点存在，替换旧值
8. 如果桶满了（容量16*负载因子0.75），需要resize（扩容2倍重排）

#### get方法

先计算hash值找到对应的桶，然后在链表或者红黑树中通过equals方法找到对应的节点，找到值并返回。

#### 扩容resize()方法

在构造哈希表时，若不指明初始大小，默认为16，若大小达到了容量16*负载因子0.75，用大数组去替代小数组，重新调整hashmap大小为原来2倍，比较耗时。

具体逻辑为创建一个大小为原数组大小2倍的新数组，要原数组中的元素重新计算在新数组中的索引，然后添加至新数组中。

- 在多线程环境下，调整大小会存在条件竞争，容易造成**死锁**
- rehashing是一个比较耗时的过程

在JDK7前，哈希表扩容，需要重新计算每个元素在新的数组中的位置，然后进行移动。在JDK8中进行了优化，通过位运算来判断元素是否需要移动，如果位置不变可直接放入对应的位置；如果出现了变化，新的位置是原来的下标位置+原数组长度。

可以这么做是因为计算hash的时候，采用的是散列后的哈希值与上数组长度减一。初始为16，二进制的长度减一为1111，当扩容后，数组新长度为11111，如果一个key倒数第5位本来计算就是0，这样当扩容后计算的位置还是不变的，如果倒数第5位是1，相当于要加上10000，即老数组长度16，因此若有变化将原来老数组下标加上老数组长度就是在新数组中的位置。

#### 负载因子

为何负载因子要选为0.75，而不是更高的1，或者更小的0.5呢。如果负载因子选为1，则resize()的阈值会变得比较高，更不容易扩容，减少了rehash的成本，但是这样桶中元素更密集，哈希碰撞更严重，降低了查询效率。如果选为0.5，则resize()的阈值变得很低，扩容频繁，虽然桶中元素分布更散列，但扩容成本变高。因此为了权衡扩容成本与查询效率，选了折中的数字0.75。

#### cpu占用100%的问题

出现在JDK7，因为在JDK7中链表元素的插入采用的是头插法，即如果插入元素的顺序为1,2,3，链表的顺序是3->2->1，然后在扩容的时候，当两个线程均新建了一个新的数组，一个数组将链表元素放入到新数组中，由于从头节点开始放，因此此时的顺序是1->2->3，而对于第二个链表，其当前记录指针e若指向3，e的next指向2，此时会让3再指向2，这样相当于形成了一个循环链表，这样当使用get()方法的时候，就会死循环。

JDK8后，链表插入采用尾插入的方法，这样便解决了死循环问题，但在多线程下HashMap仍然会存在节点丢失等问题，因此不能在单线程下使用。

#### 如何减少碰撞

- 使用扰动函数：促使元素位置分布均匀，减少碰撞几率

- 使用final对象，并采用合适的equals()和hashCode()方法

  使用String、Integer等类作为键很好，防止键值改变，放入和获取到的键不一样，就不能获取到

哈希散列的过程：可以看到**hashmap允许键为null**

~~~java
static final int hash(Object key){
	int h;
	return (key == null) ? 0 : (h=key.hashCode()) ^ (h >>> 16); 
}
~~~

将hashCode右移16位，去除低位，与原数据异或，混合原始哈希码的高位和低位，加大低位随机性，散列更均匀，计算下标使用(数组长度-1)&hash，数组长度总为2^n（传入值时替换为最接近2的n次方的值），等价于对长度取模，效率更高。	

{% asset_img hash散列.png This is an example image %}

#### Dos攻击

在JDK7以前，如果对方知道我方使用哈希算法，可以发送大量哈希值相同的请求来导致严重的哈希碰撞，然后不停访问这些key就可以显著影响服务器的性能（大量占用CPU），这样形成了一次拒绝服务供给（DoS）。这样将最坏性能从O（N）优化至O（logN），有较大的改善。

那么怎么进一步处理呢？

1. 限制POST和GET请求参数个数
2. 限制POST请求的请求体大小
3. Web Application FireWall（WAF）

对于JDK7，HashMap会动态的使用一个专门的TreeMap实现来替换掉它。

#### 多线程下的HashMap

由于HashMap不是线程安全的，在多线程下会出现问题，有

- 多线程put操作后，get操作导致死循环

  rehash时容易出现环状链表

- 多线程put非null元素后，get操作得到null值

- 多线程put操作，导致元素丢失

#### 红黑树

> 引用自博客`https://my.oschina.net/hosee/blog/618828`

要了解红黑树是什么，为什么要有红黑树。

红黑树是**相对平衡**的**二叉查找树**，不同的是在每个结点上增加一个存储位来表示颜色，Red或Black。而红黑树的特点是通过对任何一条从根到叶子的路径上各个结点的着色方式的限制，**红黑树确保没有一条路径比其他路径长出两倍**，树是接近平衡的。没有AVL树那么平衡。

二叉查找树简单理解是：

- 若有左子树，则左子树上所有结点的值均小于根节点值 
- 若有右子树，则右子树上所有结点的值均大于根节点值
- 任意节点的左、右子树分别为二叉查找树
- 没有键值相等的节点

一般普通的二叉查找树高度为log(N)，但如果二叉查找树退化为一个链表，最坏时间会变成O（N）,那么红黑树如何保证树相对平衡呢？介绍其5个性质：

1. 每个节点要么红、要么黑
2. 根节点是黑
3. 叶节点是黑的空节点
4. 若一个节点是红，两个儿子都是黑的（不存在两个连续的红色节点）
5. 对任意节点，其到叶节点尾端指针的每条路径都包含相同数目的黑节点

{% asset_img 红黑树图.png This is an example image %}

数学证明红黑树的操作时间复杂度最差为O(logN)。

#### HashMap与HashTable

因为HashMap在多线程下不安全，而线程安全的类有HashTable，其线程安全原因是使用了synchronized修饰符，锁为调用者的this，与Collections.synchronizedMap(hashMap)几乎无区别，只是锁不一样，synchronizedMap锁为Object类的mutex，hashMap方法均用synchronized(mutex)加锁。

而HashTable初始容量为11，与HashMap不同。扩容时，计算容量为2倍原来容量+1。

~~~java
public class Hashtable<K,V> extends Dictionary<K,V> 
    implements Map<K,V>, Cloneable, java.io.Serializable {
    public synchronized int size() {...}
    public synchronized boolean isEmpty() {...}
    public synchronized V get(Object key) {...}
    public synchronized V put(K key, V value) {...}
}
~~~

但HashTable在多线程下效率较低，因此又引入了ConcurrentHashMap。

二者的区别概括为

- 线程安全

  HashMap不安全，HashTable安全

- 能否存空键

  HashMap可以，计算散列值时null的hash为0，HashTable不可以

- 初始容量与扩容

  HashMap默认初始16，HashTable为11；

  扩容时，HashMap为原来2倍，HashTable为原来2倍+1；

  指定容量，HashMap会计算与指定的最接近的2的整数次幂作为初始容量，HashTable按照指定的

- 效率

  HashMap效率更高，HashTable因为有synchronized，效率更低

- 底层结构

  HashMap1.8后底层为数组+链表+红黑树，HashTable没有树化的机制

#### HashMap与ConcurrentHashMap

那么该如何优化HashTable呢？

**通过锁细粒度化，将整锁拆解成多个锁进行优化**。

早期的ConcurrentHashMap通过**分段锁Segment**实现

{% asset_img segment.png This is an example image %}

在HashMap的基础上，外面多了一层数组结构，有多个Segment（一种可重入锁），Segment继承自ReentrantLock，每个Segment中有多段数据（HashEntry），当一个线程占用一个锁时，位于此Segment上的其他数据也可以被访问到。默认分配**16**个Segment，理论上比HashTable效率高16倍。将HashMap的table数组逻辑上拆分为多个子数组，每个子数组配置一个Segment，线程只有在获取到某把分段锁后，才能获取到其中的子数组，其他没有该Segment的线程访问其中数据被阻塞，而访问没有被Segment锁住的数据不会被阻塞。

而当前的ConcurrentHashMap，使用**CAS+Synchronized使锁更细化**

{% asset_img 新con.png This is an example image %}

使用CAS+synchronized为保证并发安全，数据结构使用数组+链表/红黑树，synchronized锁定的是当前链表或树的首节点，只要没有哈希冲突，效率就可以被进一步提高。许多参数与HashMap类似，如树化和反树化参数。也有一些特有的参数，如sizeCtl，是大小控制的标识符，-1表示正在初始化，-n表示有n-1个线程在执行扩容操作，正数或零表示还没有初始化，数值表示下一次初始化或扩容大小，此参数有volatile修饰，在多线程间可见。

相比于HashMap可以添加null键，ConcurrentHashMap不允许添加null键和null值，其对数组元素的更新使用CAS操作。

ConcurrentHashMap的put方法

1. 判断Node[]数组是否初始化，没有则进行初始化操作
2. 通过hash值定位数组的索引坐标，看是否有Node节点，如果没有则使用**CAS添加**（链表的头节点f），添加失败进入下次循环
3. 若检查到内部在扩容，**协助扩容**
4. 判断链表/红黑树头节点f是否为空，若不为空则**用synchronized锁住**
   - 若为链表结构，执行链表的添加操作
   - 若为树结构，执行树的添加操作
5. 如果链表长度到达临界值（默认为8），将链表转换为树结构

比起Segment，锁更细化，只要没有哈希冲突就不会有并发获得锁的情况，因为其锁的是每一个桶。在put方法操作时，比较关键的是若当前没有Node节点，先使用**CAS操作插入头节点**，失败则循环重试；若已经有头节点，则**获取头节点的锁**后再进行相关操作。

此外，计算map的大小时，有size()和mappingCount()方法，从源码可以看出，size()返回的为int类型，有大小限制，而mappingCount()方法返回long类型，JDK更推荐使用mappingCount()方法。

~~~java
    public long mappingCount() {
        long n = sumCount();
        return (n < 0L) ? 0L : n; // ignore transient negative values
    }
    public int size() {
        long n = sumCount();
        return ((n < 0L) ? 0 :
                (n > (long)Integer.MAX_VALUE) ? Integer.MAX_VALUE :
                (int)n);
    }
~~~

#### 三者的比较

- HashMap线程不安全，数组+链表+红黑树
- HashTable线程安全，锁住整个对象，数组+链表
- ConcurrentHashMap线程安全，CAS+synchronized，数组+链表+红黑树
- HashMap的key、value均可为null，而其他两个类不支持

## 设计模式

### 创建型模式

#### 单例模式

单例模式的需求为一个类只允许产生一个对象，实现的方式有饿汉式，懒汉式与嵌套类式。

饿汉式

~~~java
public class Singleton1{
	private static Singleton1 instance = new Singleton1();
    private singleton1(){}
    public static Singleton1 getInstance(){
        return instance;
    }
}
~~~

基本思路为直接创建好一个私有的静态的对象，私有构造函数，创建一个public的静态方法去返回此对象（静态方法只能访问静态成员）。这种方法的优点是线程安全，缺点为如果不用到getInstance()方法，仍然会创建一个对象，增加开销。

懒汉式

~~~java
public class Singleton2{
	private static volatile Singleton1 instance = null;
    private singleton1(){}
    public static Singleton2 getInstance(){
        if(instance == null){
            synchronized(Singleton2.class){
                if(instance == null){
                    instance = new Singleton2;
                }
            }
        }
        return s;
    }
}
~~~

基本思路也为创建一个私有静态对象，但一开始不初始，私有构造函数，创建public的静态方法返回此对象，需要加上双重验证，加synchronized是为了保证线程安全，外面加上一层判断是为了提高效率，其中因为new这个语句不是原子性的，为了避免语句重排，出现s没有被初始化就返回的情况，需要让instance被volatile修饰，利用内存屏障保证不重排语句。优点是节约了资源，缺点是写法比较复杂，写错了容易线程不安全。

嵌套内部类

~~~java
public class Singleton3{
    private singleton1(){}
	private static class Holder{
        private static instance = new Singleton3();
    }
    public static Singleton3 getInstance(){
        return Holder.instance;
    }
}
~~~

在饿汉式基础上将对象构造放进内部类中，这样不调用内部类的时候不新建对象。

基本思路为私有构造方法，定义一个私有的静态类，避免其他类访问，直接类名调用，此类中持有外部类的私有静态实例，外部类提供方法，返回内部类中的对象。这样静态类只在被调用时加载一次，因此只有一个对象。优点是不使用getInstance方法不实例化，节约资源。缺点是第一次加载比较慢。

枚举类

~~~java
public enum Singleton4 {
    INSTANCE;
}
//调用时
Singleton4 s = Singleton4.INSTANCE;
~~~

写法简单，调用方便。

JDK中用到的单例模式为：Runtime类，使用getRuntime()，使用的饿汉式。

~~~java
public class Runtime {
    private static Runtime currentRuntime = new Runtime();
    private Runtime() {}
    public static Runtime getRuntime() {
        return currentRuntime;
    }
}
~~~

