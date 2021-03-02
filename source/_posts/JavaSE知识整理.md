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

面向过程与面向对象最基本的区别是，代码的组织方式不同，面向过程风格的代码被组织成一组方法集合及其数据结构，方法与数据结构的定义是分开的；面向对象风格的代码被组织成一组类，方法和数据结构绑定在一起，定义在类中。

面向对象相比于面向过程：

1、OOP更能应对大规模复杂程序的开发（在逻辑复杂时，程序处理流程复杂，用面向过程思维比较复杂）

2、OOP风格代码更易复用、易拓展、易维护（四大特性的帮助）

3、OOP语言更人性化（离机器语言更远，对业务建模，更聚焦于业务）

#### 面向对象语言避免面向过程写法

1、滥用getter与setter方法

面向对象需要将信息进行封装，如果滥用getter和setter，相当于破坏了对象的封装，可能导致类的属性被外部更改。

2、滥用静态属性和全局方法

若一个类中过多静态属性，比较臃肿，且加载类比较耗时，建议将静态属性进行拆分到对应类或多个类中。若一个类，只有静态方法，无属性，则面向对象变为面向过程写法，尽量将方法放到对应类中，或进行更细粒度的拆分。

3、定义数据和方法分离的类

MVC的开发中，将数据类型和方法进行拆分，数据类型放在VO（View Object），BO（Business Object）和Entity，而数据操作逻辑放在Controller、Service、Repository类中。这种来发模式称为贫血模式。

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

#### 封装、抽象、继承、多态解决的问题

封装：将类的属性信息封装在类的内部，对外屏蔽细节，只暴露对应的方法。封装需要编程语言提供**访问权限控制**的语法机制。封装的意义是，增加了代码的可控性（避免属性直接被修改），类的易用性（不用过多了解业务细节）。

封装：隐藏方法的具体实现，调用者只需要关注方法提供的功能。在Java中实现的语言基础为接口类与抽象类。为比较通用的涉及思想，只需要提供“函数”此语法机制即可。抽象的意义是，屏蔽非必要信息，具体依赖抽象。

继承：表示类之间的is-a关系，Java中使用extends关键字。最大好处为代码复用，但并不是继承独有，使用组合也可以实现。

多态：子类替换父类，实际代码运行时，调用子类的方法实现。语法机制：支持父类对象引用子类对象，支持继承，支持子类可以重写父类中方法。python，cpp，go中有**duck typing**的写法，传入一个参数，只需要其具有某个方法就可以，即两个都只要有相同的方法就可以实现多态，而java需要强制其实现某个接口。多态提高代码的可拓展性和复用性。

#### 类之间的关系

1、泛化（Generalization），可理解为继承

~~~java
public class A{}
public class B extends A{}
~~~

2、实现（Relization），一般指接口与实现类之间的关系

~~~java
public interface A{}
public class B implements A{}
~~~

3、聚合（Aggregation），包含关系，A类包含B类对象，B类对象的生命周期可不依赖于A

~~~java
public class A{
	private B b;
	public class A(B b){
		this.b = b;
	}
}
~~~

4、组合（Composition），也是包含，A对象包含B对象，B对象的生命周期依赖于A对象的生命周期，B对象不可单独存在，如车与轮胎

~~~java
public class A{
	private B b;
	public class A(){
		this.b = new B();
	}
}
~~~

5、关联（Association），非常弱的关系，包含聚合和组合，若b对象是a对象的成员变量，就是组合关系

~~~java
public class A{
	private B b;
	public class A(B b){
		this.b = b;
	}
}
// 或者
public class A{
	private B b;
	public class A(){
		this.b = new B();
	}
}
~~~

6、依赖（Dependency），比关联更弱，只要B对象和A对象有任何关系，就是有依赖关系

~~~java
public class A{
	private B b;
	public class A(B b){
		this.b = b;
	}
}
// 或者
public class A{
	private B b;
	public class A(){
		this.b = new B();
	}
}
// 或者
public class A{
	public void func(B b){}
}
~~~

### 项目开发

#### 项目思考

如果拿到一个项目需求，在需求不够明确的时候，需要花一定的时间将需求理清楚，去做的时候不要想着第一次就出一个完美的方案，好的产品是迭代出来的。可以按照最小可行性产品的思路进行思考，如遇到一个接口鉴权系统的设计，可以给自己提问

1. 接口鉴权是什么？	理清概念
2. 接口鉴权的最佳实践是什么？  技术调研
3. 技术中涉及的信息从哪来？     传输与交互
4. 技术中涉及的信息到哪去？     信息的存储
5. 此技术如何使用？                    用户手册与实例程序
6. 此方案有什么优缺点，如何改进   迭代
7. 此技术如何测试？         想好测试case
8. 此技术的返回结果是什么？    约定返回结果
9. 此项目的工期多久，何时上线   估计工期

#### 项目开发流程

1、划分职责进而识别出有哪些类

对需求进行分析，将涉及的功能点罗列，将功能点相近，操作相同的属性，归为一个类

2、定义类及属性和方法

识别出描述中的动词，作为候选的方法，然后进一步筛选出真正的方法，将属性也进行筛选

3、定义类与类之间的交互关系

对类与类之间的关系进行划分，保留：泛化、实现、组合（组合、聚合、关联）、依赖

4、将类组装起来并提供执行入口

将所有类组装，提供一个执行入口

#### 系统设计

1、合理的将功能分到不同模块

模块的功能划分

系统设计实际上是将合适的功能放到合适的模块中。如果一个功能的修改或增加，经常要跨团队、项目完成，说明模块划分不够合理。

同时为了避免业务知识的耦合，让下层系统更加通用，一般不希望下层系统包含太多上层系统的业务信息。

2、设计模块与模块之间的交互关系

模块之间的交互

比较常见的系统交互方式有两种：一种是同步接口调用，另一种是使用消息中间件异步调用。第一种方式直接，第二种方式解耦效果更好。

上下层的系统间调用倾向于通过同步接口，同层之间的调用倾向于异步消息调用。

3、设计模块的接口、数据库、业务模型

#### 非业务系统设计

对非业务通用框架的开发，在做需求分析时，除了功能性需求分析之外，还需要考虑框架的非功能性需求，如框架的易用性，性能，扩展性，容错性，通用性等。

对复杂框架的设计，可以画产品线框图，聚焦简单应用场景，设计实现最小原型，画系统设计图等，目的是简化问题，具体，明确，提供一个迭代设计开发的基础，逐步推进，如果等到所有事情都想好了再开始，则可能永远都无法开始。

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

#### 继承与组合

继承：在一些语言中使用extends关键字，表示is-a的关系

组合：在一个类中持有其他类的对象，表示has-a的关系

委托：一个对象请求另外一个对象的功能

继承主要的特点有：描述了is-a的关系，支持多态特征，代码复用，而这三个均可以被替代。如is-a的关系，可以使用组合和接口的has-a关系来代替，而支持多态可以通过接口来实现，代码复用可以通过组合和委托来实现，如下面代码所示。

~~~java

public interface Flyable {
  void fly()；
}
public class FlyAbility implements Flyable {
  @Override
  public void fly() {}
}
//省略Tweetable/TweetAbility/EggLayable/EggLayAbility

public class Ostrich implements Tweetable, EggLayable {//鸵鸟
  private TweetAbility tweetAbility = new TweetAbility(); //组合
  private EggLayAbility eggLayAbility = new EggLayAbility(); //组合
  //... 省略其他属性和方法...
  @Override
  public void tweet() {
    tweetAbility.tweet(); // 委托
  }
  @Override
  public void layEgg() {
    eggLayAbility.layEgg(); // 委托
  }
}
~~~

而继承并不是一无是处，因为组合需要更细粒度的类的拆分，这样会带来更多的类和接口，如果类的关系比较稳定，类的深度不深（2层以内），就可以使用继承，反之使用组合更合适。还有些时候必须使用继承，如使用到一个外部类的时候，需要对其某个方法进行更改，这时候只能继承外部类然后进行更改。

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

若表示is-a的关系，并为了解决代码复用的问题，使用抽象类；若要表示has-a关系，为了解决抽象而非代码服用问题，使用接口。重选ing类自下而上的设计思路，先有子类的代码重复，再抽象成上层抽象类。接口是自上而下的设计思路，在编程时，一般先设计接口，再考虑具体实现。

java8后，可以在接口中定义默认方法，使用default关键字。

#### 普通类与抽象类区别

- 普通类中不能有抽象方法，抽象类中可以包含抽象方法
- 普通类可以直接实例化，抽象类不能直接实例化

#### 抽象类能用final修饰吗

不能，定义抽象类的目的是让其他类继承，而final关键字修饰的类不能被继承，二者相违背，因此抽象类不能被final修饰

#### 普通类实现接口功能

如果要用普通类实现接口功能，需要子类必须要重写父类功能，且父类不能被实例化。这样可以将要子类覆写的方法抛出异常，让子类必须要进行重写，不然在运行时会抛出异常。且让父类的构造函数为protected，这样若**不在一个包中**，则只能重写其子类。

~~~java
public class MockInteface {
  protected MockInteface() {}
  public void funcA() {
    throw new MethodUnSupportedException();
  }
}
~~~

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

### String

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

#### (String)、toString、valueOf的区别

（String）是强制的类型转换，可能抛出不合法的转换异常

toString调用的是Object类中的toString()方法，如果调用此方法的对象为空，对报空指针异常

而valueOf是String中的静态方法，其具体代码为

~~~java
public static String valueOf(Object obj){
    return (obj==null) ? "null" : obj.toString()
};
~~~

相当于做了一个对象为空的判断，如果为空返回空，如果不为空执行其toString()方法，这样程序更健壮点

### Object

#### 其他类如何默认继承Object

> 引用自掘金博客 https://juejin.im/post/5ca1e8ade51d454e6a300048

首先Object的类是所有类的父类，因为所有类中有包含有Object类中的方法，但是我们在编程的时候并没有写过extends Object，那么Java语言是如何确定当前类的父类是继承自Object类中的呢？有两种可能的原因，一种是在编译期间确定的，如果当前类没有显式继承自一个类，则编译时认为其继承自Object，另一种是由JVM处理的。如果是在编译期间确定的，那么其class文件中应该写明了当前类继承自Object，根据引用博客的实验，在JDK6时，得到的反编译结果是当前类的class文件中指出了当前类是继承自Object。但在JDK7及之后，在反编译的结果中是没有extends Object的。

这样得到的一个推测的结果是：在JDK6及之前，是在编译期间确定的；在JDK7及之后，是由JVM虚拟机来处理的。

### JIT

> https://www.cnblogs.com/dzhou/p/9549839.html

JIT编译（just-in-time compilation），即使编译，狭义理解为在某段代码一次执行时进行编译。在HotSpot中，Java程序最初通过解释器解释执行，若虚拟机发现某些方法或者代码块运行特别频繁，会将这些代码定义为“热点代码”。为了**提高这些“热点代码”代码的执行效率**，运行时虚拟机会把这些代码**编译成本地平台相关的机器码**，并进行**优化**，此任务由JIT编译器完成。

使用解释器和编译期并存的架构是因为，二者各有优劣。当程序需要迅速的启动和执行时，解释器可以首先发挥作用，省去编译时间，立即执行。程序运行后，随时间推移，编译器逐渐发挥作用，将更多代码编译成本地代码，可以提高效率。

#### 时间开销

执行编译后的代码比解释器执行快，但是JIT存在编译的时间开销，因此对于只执行一次的代码就没有必要使用JIT

- 只被调用一次的代码，如类的构造器
- 没有循环

对于执行少量次数的代码，JIT编译带来的速度提升不一定可以抵消其最初编译带来的开销。

#### 空间开销

对一般的Java代码，编译后的代码相比编译前会膨胀很多，因此只有频繁执行的代码才值得编译，如果将所有的代码都进行编译，会导致很大的空间开销。因此一般会采用解释器+JIT编译器的组合而不是均使用JIT编译器。

#### 哪些代码被编译

热点代码才会被编译为本地代码，运行过程中运行代码有两种

- 被多次调用的方法
- 被多次执行的循环体

将整个方法作为编译对象，形象称为栈上替换（On Stack Replacement，OSR），方法还在栈上就被替换了。

#### 热点代码判断

判断一个代码是否触发即时编译，需要进行Hot Spot Detection（热点探测）

主要热点探测方式有两种

- 基于采样的热点判断

  周期性检查各个线程的栈顶，如果某些方法经常出现在栈顶，则此方法为“热点方法”。好处是简单高效，容易获取方法调用关系。缺点是无法精确获取方法的热度，容易受到干扰

- 基于计数器的热点判断

  为每个方法或代码块建立计数器，统计方法的执行次数，在执行次数超过一定阈值后，被认定为热点方法。优点是统计较精确，缺点是实现更复杂，需要维护计数器

HotSpot采用的是基于计数器的热点判断

## 集合

Java中的内存泄漏多与集合容器相关。当集合中持有生命周期较短的对象，当对象作用已经结束，需要被销毁时，因为有长生命周期的集合持有，如果不进行手动销毁，会存在内存泄漏。如在坦克的项目中，如果子弹集合中，子弹生命周期结束，没有手动将此子弹移出集合，就会造成内存泄漏。

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

在1.8后，HashMap计算哈希值的时候，没有直接使用key的hashCode，而是如下所示，将哈希值与右移16位后哈希值进行异或，为什么要这样做呢？

~~~java
static final int hash(Object key){
	int h;
	return (key == null) ? 0 : (h=key.hashCode()) ^ (h >>> 16); 
}
~~~

需要结合数组寻址一起讲，得到哈希值后，在数组中寻址使用的是hash & (n-1)，这个语句效果与hash % n一样，但性能更高，初始的时候，HashMap大小为16，所以更多时候，参与运算的只有哈希值的后几位，这样其实哈希值的高位不参与运算，这样就有比较大的可能产生哈希冲突，为了让高位的哈希值也更多的参与到运算，采用将高位右移16位后异或的算法，这样将哈希值的计算进行散列

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

#### ConcurrentHashMap不允许存null原因



#### 三者的比较

- HashMap线程不安全，数组+链表+红黑树
- HashTable线程安全，锁住整个对象，数组+链表
- ConcurrentHashMap线程安全，CAS+synchronized，数组+链表+红黑树
- HashMap的key、value均可为null，而其他两个类不支持

### 其他数据结构

#### 跳表

为了实现数据的排序，在单线程下可以使用TreeSet与TreeMap，但这两种结构不是并发安全的，为了在多线程下实现数据的有序存放，可以使用ConcurrentSkipListSet与ConcurrentSkipListMap，跳表的本质是在普通的链表上加上了多层类似索引的结构，这样加快查找效率，以空间换时间，查找的时候不用顺序遍历链表，而是可以跳过某些数据，因此叫做跳表。相比于红黑树，实现结构更加简单。

而哪些数据可以被当做索引，是根据概率来决定的。

使用场景：加速链表的查询效率。Redis中有使用。

#### 阻塞队列

阻塞队列是支持阻塞的获取元素与阻塞的放入元素的队列，主要是应用于生产者与消费者模式，当队列满了以后，生产者放入元素被阻塞；当队列空了以后，消费者取出元素被阻塞。解决的问题是生产者与消费者能力不匹配的问题。

BlockingQueue中常用的方法有如下

{% asset_img 阻塞队列方法.png This is an example image %}

常用的阻塞队列有

- ArrayBlockingQueue

  有界的阻塞队列，需要传入默认大小，锁没有分离（生产消费使用一个锁）

- ListBlockingQueue

  有界的阻塞队列，可以不指定大小（最好指定），默认Integer.MAX_VALUE，锁分离（生产消费使用不同锁），应用于固定大小的线程池

- PriorityBlockingQueue

  有优先级的无界队列

- DelayQueue

  使用优先级队列实现的无界阻塞队列，放入队列元素要实现Delayed接口，重写getDelay(TimeUnit unit)与compareTo(Delayed o) 方法，实现的功能是在某个元素过期后，才能被获取到，getDelay方法用于获取剩余过期时间，compareTo方法用于堆的排序。常用于<span style="color:red;">缓存系统的设计</span>（一旦元素可以被取出，表示缓存到期），<span style="color:red;">订单到期</span>，<span style="color:red;">限时支付等</span>。

- SynchronousQueue

  不存储元素的队列，一个生产必须要一个消费。

- LinkedTranferQueue

  链表结构组成的无界阻塞队列

- LinkedBlockingDequeue：双向阻塞队列，用于forkJoin框架，方便一个线程从其他任务队列中拿取任务

实现原理：为了实现阻塞与继续生产或消费，需要在满足一定条件时将线程唤醒，即<span style="color:red;">等待通知范式</span>，使用<span style="color:red;">一个锁+2个监视器</span>。当有生产时，同时消费的监视器；当有消费时，同时生产的监视器。

## 网络

框架：最早是BIO，后来有NIO，再后来有AIO，但这些API都不好用，于是Netty基于NIO进行了封装。

### BIO

Blocking IO(Input-Output)

IO速度相比CPU速度非常慢

半双工，读的时候不能同时写。

阻塞IO，有个client连接，服务端就新建一个线程进行连接。一个客户端，一个连接，一个线程，当客户端数量太多不支持。

{% asset_img BIO.png This is an example image %}

服务端：服务端等待客户端建立连接，通过accept方法获取Socket，此方法会阻塞直到客户端连接，服务端获取到socket后，**新建一个线程处理socket的数据读写**，在客户端请求建立时，此线程一直存在。socket的读与写不是双向的，单独拿出socket的InputStream来读，拿出OutputStream来写。

客户端：建立一个Socket，向输出流写信息，从服务端读信息，关闭socket连接。

Blocking在于：建立连接阻塞，读写阻塞

- Server端**accpet方法阻塞**，没有客户端连接就wait
- 在处理**socket流的读与写方法也是阻塞**的

需要是多线程，因为accept为阻塞的，一个连接一个线程，只有一个线程其他客户端会被阻塞，一次只能处理一个客户端。可以使用线程池。

BIO效率低，并发量不好。BIO很少用，代码简单，适合建立连接少的情况。

### NIO

Non-Blocking IO 

使用Selector多路复用器，当有客户端建立连接，会生成对应的channel进行数据读写，selector不断轮询注册的channel，若有channel发生读写事件，将这些channel获取出来，建立线程进行处理，当此请求结束后，销毁线程，而不会一直让线程存在。线程通过buffer缓冲区来与channel进行信息的读写。

{% asset_img NIO.png This is an example image %}

上面为NIO的**单线程模型**，用一个线程处理客户端的连接，selector轮询客户端是否有连接与读写，门面模式？selector负责client的连接与读写。并不是只能一个连接，而是selector一次处理一次请求。

NIO服务端对socket封装为ServerSocketChannel，此通道为**双向**，可同时读写，设置非阻塞，打开并注册selector，此时关心的只是建立连接，selector进行select，此**方法**也是**阻塞**，获取到key并进行处理。

为了处理客户端的连接，需要将selector注册到客户端的channel中，因此selector处理的是服务端的channel+客户端的channel，不同的channel上的时间在一个set中，selector从set中获取事件并处理，每次处理事件后要将其移除。

~~~java
		ServerSocketChannel ssc = ServerSocketChannel.open();//服务端的socket
        ssc.socket().bind(new InetSocketAddress("127.0.0.1",8888));//绑定地址
        ssc.configureBlocking(false);//设置非阻塞
        System.out.println("server started, listening on :" + ssc.getLocalAddress());
        //要有selector
        Selector seletor = Selector.open();
        ssc.register(seletor, SelectionKey.OP_ACCEPT);//将selector注册
        //轮训
        while (true){
            seletor.select();//看是否有连接，阻塞
            Set<SelectionKey> keys = seletor.selectedKeys();//获取所有的keys
            //迭代keys
            Iterator<SelectionKey> it = keys.iterator();
            while (it.hasNext()){
                SelectionKey key = it.next();
                //获取后移除，不移除会重复处理
                it.remove();
                handle(key);
            }
        }
~~~

此时Selector负责客户端连接与读写，干的太负责，于是有了reactor模式

{% asset_img NIO2.png This is an example image %}

NIO的多线程模型

Observer模式，响应式编程。selector只负责与客户端建立连接，然后又客户端要进行读写后，selector交给线程池进行处理。selector+worker。

如果客户端消息来不及处理，可以放入消息队列。NIO的ByteBuffer比较难用，读与写只用一个指针，较少直接用NIO，多用Netty。

NIO相比BIO，不用一个客户端连接建立一个线程，客户端连接只需要一个线程来管理。但NIO需要一直轮询。

### AIO

Asynchronous-IO

基于Proactor模型，异步非阻塞。每个连接发送过来的请求，绑定一个buffer，通过os去异步完成读，程序继续往下走，等os完成数据读取后，回调接口，给出os异步读完的数据。

客户端要建立连接时，操作系统通知selector，selector连接通道，再交给工人去执行读写操作。

{% asset_img AIO.png This is an example image %}

AIO的accpet不是阻塞的，当执行accpet后会**继续往下**，因此要在accept后设置循环等待避免main方法结束。当有客户端连接，交给CompletionHandler来处理，本质是模板方法模式，只重写关心的方法。方法写好了，当有客户端连接时自动调用completed方法执行。

总的来说，当操作系统发现有客户端连接请求，调用写好的建立连接的方法，建立连接的方法再调用写的方法，使用观察者模式来实现异步操作。

现在读也可以是非阻塞的，读了就执行其他，当读完后再执行一个CompletionHandler。

AIO与NIO在Linux下都是基于epoll实现的，epoll也是轮询，因此底层实现一样，**AIO多了层封装**，在Linux下使用AIO效率并一定高，Netty对NIO进行封装，使得API更像AIO，更好用。Windows下的AIO单独实现，使用Completion Port，但大多数Server都是基于Linux实现的，因此Netty还是基于NIO封装。

### Netty

实现对NIO，BIO的封装，封装成AIO的样子。建立两个group，一个负责建立连接，一个负责读写，将这两个group交给Server启动的封装类，指定连接后者两个group的类型，对每个客户端连接增加监听器，进行处理，一旦通道被初始化，在此通道添加对此通道的监听器，这样将连接与业务处理代码解耦。对于读写的处理过程，重写channelRead方法等。对于异常处理，将相应的通道关闭。

#### 客户端连接

客户端的写法

~~~java
public class Client {
    public static void main(String[] args) {
        //事件处理线程池，可处理连接或读写事件
        EventLoopGroup group = new NioEventLoopGroup(1);//1个线程
        //辅助启动类
        Bootstrap b = new Bootstrap();//启动
        try {
            //连接server
            b.group(group)//传入group
                    //指定channel类型为nio
                    .channel(NioSocketChannel.class)//指定channel类型
                    //当channel有事件，指定处理的handler
                    .handler(new ClientChannelInitializer())
                    .connect("localhost",8888)//连接远程，异步方法，无法知道是否成功
                    .sync();//必须等其结束，不让继续进行
        } catch (InterruptedException e) {
            e.printStackTrace();
        }finally {
            group.shutdownGracefully();//正常结束
        }
    }
}
class ClientChannelInitializer extends ChannelInitializer<SocketChannel>{
    @Override
    protected void initChannel(SocketChannel ch) throws Exception {
        System.out.println(ch);
    }
}
~~~

1、定义事件处理的线程池

2、定义辅助启动类

3、链式编程

- 将辅助类中传入线程池
- 指定channel的类型为bio还是nio
- 增加连接后的处理handler
- 进行连接，返回的是ChannelFuture，为了知道是否连接成功
  - 再加入sync方法
  - 或自定义监听器

Netty中方法均是异步的。

connet为异步，返回ChannelFuture，使用sync才知道是否有成功执行，如果不用sync的写法，需要在Future中增加监听器。因为ChannelFuture得到的是异步的结果，当其中有结果后，会调用监听器中的方法。

~~~java
			//连接server
            ChannelFuture f = b.group(group)//传入group
                    //指定channel类型为nio
                    .channel(NioSocketChannel.class)//指定channel类型
                    //当channel有事件，指定处理的handler
                    .handler(new ClientChannelInitializer())
                    .connect("localhost", 8888);//连接远程，异步方法，无法知道是否成功
            f.addListener(new ChannelFutureListener() {
                @Override
                public void operationComplete(ChannelFuture future) throws Exception {
                    if (!future.isSuccess()){
                        System.out.println("not connected!");
                    }else {
                        System.out.println("connected!");
                    }
                }
            });
            f.sync();
~~~

因为Future异步，加入监听器后也会继续往下 ，为了阻塞住它，加上sync，在结束后才继续进行，加sync是为了防止客户端还没有建立好连接则main线程直接结束。

其中打印的顺序是，channel初始化的时候，客户端打印SocketChannel，服务端连接上客户端后，服务端打印信息，然后客户端的监听器打印。

#### 服务端连接

对于服务端的写法

1、指定负责连接与读写的线程池

2、服务端的启动类

3、链式编程

- 启动类绑定两个线程池
- 指定channel类型为NIO
- 为客户端的通道增加监听器
- 绑定监听端口
- 等待future返回

4、让服务器等待被关闭

- 获取到future对应的服务端的channel，再调用closeFuture，不调用close就被阻塞，调用sync等待此future结束

~~~java
public class Server {
    public static void main(String[] args) throws IOException {
        EventLoopGroup bossGroup = new NioEventLoopGroup(1);//负责连接
        EventLoopGroup workerGroup = new NioEventLoopGroup(2);//负责读写
        try {
            ServerBootstrap b = new ServerBootstrap();
            ChannelFuture f = b.group(bossGroup, workerGroup)//指定线程池类型，一个连接，一个读写
                    .channel(NioServerSocketChannel.class)//指定channel类型
                    //加在客户端上
                    .childHandler(new ChannelInitializer<SocketChannel>() {
                        @Override
                        protected void initChannel(SocketChannel ch) throws Exception {
                            System.out.println(ch);//打印客户端端口
                        }
                    })
                    .bind(8888)//监听端口
                    .sync();//等待future执行完毕
            System.out.println("server started!");
            //等着服务器关闭
            f.channel().closeFuture().sync();//阻塞，拿到server的channel，没有调用close，closeFuture会被一直阻塞
        } catch (InterruptedException e) {
            e.printStackTrace();
        }finally {
            //关闭
            workerGroup.shutdownGracefully();
            bossGroup.shutdownGracefully();
        }
    }
}
~~~

很多情况下都使用到了sync的写法，因为Netty中方法为异步，为了知道执行结果必须加入sync，只有有返回值后才继续进行。

#### 客户端与服务端读写数据

客户端如果需要向服务端写数据，需要在其被初始化完成后，调用的initChannel方法中，在此channel的责任链上加上一个监听器，继承自`ChannelInboundHandlerAdapter`，就是Channel的Handler，其中Adapter表示实现了其骨架，只需要重写部分方法即可。当此channel被激活后，就可以写数据，Netty写数据通过Bytebuf，此为直接内存，为操作系统管理，读写更高效，将要写的消息转为字节再写入。其中需要释放Bytebuf的内存，当使用writeAndFlush后，自动释放 。为了读取服务端数据，重写channelRead方法。

其中ChannelHandlerContext是通道上下文信息，聚合了Channel。

~~~java
class ClientChannelInitializer extends ChannelInitializer<SocketChannel>{
    @Override
    protected void initChannel(SocketChannel ch) throws Exception {
        ch.pipeline().addLast(new ClientHandler());
    }
}
class ClientHandler extends ChannelInboundHandlerAdapter {
    @Override
    public void channelActive(ChannelHandlerContext ctx) throws Exception {
        //channel第一次连上可用，写出一个字符串
        //写数据依靠ByteBuf，堆外内存Direct Memory
        //需要释放buf
        ByteBuf buf = Unpooled.copiedBuffer("hello".getBytes());
        ctx.writeAndFlush(buf);//自动释放
    }
    @Override
    public void channelRead(ChannelHandlerContext ctx, Object msg) throws Exception {
        //数据存在msg中
        ByteBuf buf = null;
        try {
            buf = (ByteBuf)msg;
            byte[] bytes = new byte[buf.readableBytes()];
            buf.getBytes(buf.readerIndex(),bytes);//读到字节数组中
            System.out.println(new String(bytes));
        }finally {
            //手动释放内存
            if (buf != null) ReferenceCountUtil.release(buf);
        }
    }
}
~~~

在服务端中，也是类似，当channel初始化后，加上一个服务端channel的监听器

将对应客户端的流的责任链上加上一个监听器

~~~java
ChannelFuture f = b.group(bossGroup, workerGroup)//指定线程池类型，一个连接，一个读写
                    .channel(NioServerSocketChannel.class)//指定channel类型
                    //加在客户端上，每个客户端通道初始化完成后调用
                    .childHandler(new ChannelInitializer<SocketChannel>() {
                        //相当于客户端进来了
                        @Override
                        protected void initChannel(SocketChannel ch) throws Exception {
                            //负责客户端连接后的事宜
                            //责任链模式
                            ChannelPipeline pl = ch.pipeline();//在每个责任链上加入一个handler
                            pl.addLast(new ServerChildHandler());
                        }
                    })
                    .bind(8888)//监听端口
                    .sync();//等待future执行完毕
~~~

然后读的handler如下，使用的是channelRead，在有数据读入的时候被调用，将 msg转为Bytebuf，然后获取此Bytebuf中可读数据长度，构建字节数组，通过buf的得到字节方法，指定可读的初始位置与要存入的位置，将数据读取到数组中，转为String即可输出。因为此buf需要被手动释放，因此在finally中释放。

~~~java
class ServerChildHandler extends ChannelInboundHandlerAdapter{
    //接收数据
    //当此管道有数据读入，将数据保存到这里
    @Override
    public void channelRead(ChannelHandlerContext ctx, Object msg) throws Exception {
        //数据存在msg中
        ByteBuf buf = null;
        try {
            buf = (ByteBuf)msg;
            byte[] bytes = new byte[buf.readableBytes()];
            buf.getBytes(buf.readerIndex(),bytes);//读到字节数组中
            System.out.println(new String(bytes));
//            System.out.println(buf);
//            System.out.println(buf.refCnt());
        }finally {
            //手动释放内存
            if (buf != null) ReferenceCountUtil.release(buf);
//            System.out.println(buf.refCnt());
        }

    }
}
~~~

但是当服务端读到数据后，立刻向客户端写数据，此时就不用自己关闭了，再次关闭会出错。通过ctx来写。

~~~java
    @Override
    public void channelRead(ChannelHandlerContext ctx, Object msg) throws Exception {
        //数据存在msg中
        ByteBuf buf = null;
        buf = (ByteBuf)msg;
        byte[] bytes = new byte[buf.readableBytes()];
        buf.getBytes(buf.readerIndex(),bytes);//读到字节数组中
        System.out.println(new String(bytes));
        //写回数据
        ctx.writeAndFlush(buf);//不能再自己释放
    }
~~~

遇到了服务端写数据，服务端出错的异常，后来检查发现是因为客户端建立连接后，异步方法继续向下走，导致main方法结束，客户端关闭了，因此异步的方法一定要注意加sync来手动阻塞。

~~~java
    f.sync();
    //客户端中也要加上sync，避免客户端自己关闭
    f.channel().closeFuture().sync();
~~~

#### 服务器分发数据

服务端为了向客户端分发数据，需要有channelGroup，传入相应的处理线程。然后在客户端通道被激活时，将其加入到通道组中，在写数据的时候，写到通道组中。

服务端属性：通道组

~~~java
public static ChannelGroup clients = new DefaultChannelGroup(GlobalEventExecutor.INSTANCE);
~~~

客户端通道激活时

~~~java
 	@Override
    public void channelActive(ChannelHandlerContext ctx) throws Exception {
        //通道可用时，将其放入通道
        Server.clients.add(ctx.channel());
    }
~~~

在写数据的时候

~~~java
	//写到每一个通道组中
    Server.clients.writeAndFlush(buf);//不能再自己释放
~~~

这样就将一个客户端写入的数据分发到了所有的客户端。这种方法核心为通道组。

#### 客户端与服务端图形化界面

客户端：

新建客户端窗口类，可以输入字符串，显示在窗口上。

新建客户端类，将之前写在main中的连接服务器的方法封装在connect方法中，在其main方法中，新建一个客户端对象，调用其connect方法就可以与服务端建立连接。客户端窗口类持有一个客户端对象，在图形化界面初始化后，新建一个客户端对象，调用其connect方法即可连接服务器。

> 设计思路：将客户端与图形化界面解耦合，单一职责原则。

为了之后方便去给服务端写数据，客户端对象持有一个Channel对象，在成功连接服务器后，初始化channel

channel为客户端与服务端之间的通道，pipeline为此通道上的责任链

客户端类提供send(String msg)方法，当调用时，使用持有的channel写信息。

在客户端界面类，当界面初始化后，调用自身的连接至服务器方法，新建一个客户端类，并调用其connect方法，然后在文本框中增加监听器，当按下回车时，读取到文本框中的信息，并调用客户端的send方法。

~~~java
public class ClientFrame extends Frame {
    TextArea ta = new TextArea();//多行
    TextField tf = new TextField();//单行
    Client c = null;
    public ClientFrame(){
        this.setSize(600,400);
        this.setLocation(100,20);
        this.add(ta,BorderLayout.CENTER);
        this.add(tf,BorderLayout.SOUTH);
        tf.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                //把字符串发送到服务器
                ta.setText(ta.getText()+tf.getText());
                c.send(tf.getText());
                //写发送消息的逻辑
                tf.setText("");
            }
        });
        this.setVisible(true);
        connectToServer();
    }
    public void connectToServer(){
        //初始化client,调用connect
        c = new Client();
        c.connect();
    }
    public static void main(String[] args) {
        ClientFrame cf = new ClientFrame();
    }
}
~~~

对于客户端类

~~~java
public class Client {
    private Channel channel = null;
    public void connect(){
        EventLoopGroup group = new NioEventLoopGroup(1);
        Bootstrap b = new Bootstrap();
        try {
            ChannelFuture f = b.group(group)
                    .channel(NioSocketChannel.class)
                    .handler(new ChannelInitializer<SocketChannel>() {
                        @Override
                        protected void initChannel(SocketChannel ch) throws Exception {
                            ch.pipeline().addLast(new ClientHandler());
                        }
                    })
                    .connect("localhost", 8888)
                    .addListener(new ChannelFutureListener() {
                        @Override
                        public void operationComplete(ChannelFuture future) throws Exception {
                            if (future.isSuccess()){
                                System.out.println("connected");
                                channel = future.channel();//初始化
                            }else {
                                System.out.println("not connected");
                            }
                        }
                    })
                    .sync();
            f.channel().closeFuture().sync();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }finally {
            group.shutdownGracefully();
        }
    }
    public void send(String msg){
        //调用时，将msg通过channel写出去
        ByteBuf buf = Unpooled.copiedBuffer(msg.getBytes());
        channel.writeAndFlush(buf);
    }
}
class ClientHandler extends ChannelInboundHandlerAdapter {
    @Override
    public void channelActive(ChannelHandlerContext ctx) throws Exception {
    }
    @Override
    public void channelRead(ChannelHandlerContext ctx, Object msg) throws Exception {
        //数据存在msg中
        ByteBuf buf = null;
        try {
            buf = (ByteBuf)msg;
            byte[] bytes = new byte[buf.readableBytes()];
            buf.getBytes(buf.readerIndex(),bytes);//读到字节数组中
            System.out.println(new String(bytes));
        }finally {
            //手动释放内存
            if (buf != null) ReferenceCountUtil.release(buf);
        }
    }
}
~~~

核心为：调用客户端类，建立与服务端的连接，为了在另一个类中实现写消息，让客户端传给服务端，那么客户端中需要提供此方法，而写消息要通过channel，因此客户端需要有个channel的属性，在第一次初始化后初始channel，在发送消息的方法中，直接在channel中写入信息即可，重点为持有channel。

现在需要将服务端发送来的信息显示在客户端界面类中，那么接收消息是在客户端类中，那么客户端必须要跟客户端界面类耦合才能将信息显示在客户端界面类中，一种做法是客户端类中持有客户端界面类的引用，另一种做法是将客户端界面类做成单例。对于更新方法，拿到一个字符串后，加入换行符显示在界面上。

~~~java
	//客户端界面类
	public static final ClientFrame INSTANCE = new ClientFrame();
    public void updateText(String msgAccepted){
        ta.setText(ta.getText()+System.getProperty("line.separator")+msgAccepted);
    }
    public static void main(String[] args) {
        ClientFrame cf = ClientFrame.INSTANCE;
        cf.setVisible(true);
        cf.connectToServer();
    }
~~~

在客户端，因为客户端对象是单例的，直接通过类名就可以获取，不需要传入对象

~~~java
	@Override
    public void channelRead(ChannelHandlerContext ctx, Object msg) throws Exception {
        //数据存在msg中
        ByteBuf buf = null;
        try {
            buf = (ByteBuf)msg;
            byte[] bytes = new byte[buf.readableBytes()];
            buf.getBytes(buf.readerIndex(),bytes);//读到字节数组中
            //写在clientFrame中
            String msgAccepted = new String(bytes);
            ClientFrame.INSTANCE.updateText(msgAccepted);
        }finally {
            //手动释放内存
            if (buf != null) ReferenceCountUtil.release(buf);
        }
    }
~~~

因为服务端是将所有客户端的channel都写入了信息，有信息的分发，客户端接收到信息后显示在窗口上，而多个客户端界面是运行在各自的JVM上，因此单个JVM上的客户端界面是单例，但不同的JVM上客户端界面是不同的，这样可以实现不同客户端之间的聊天。

但当关闭一个客户端后，服务端会报错，因为没有将此客户端从服务端的ChannelGroup中删除

~~~java
	@Override
    public void exceptionCaught(ChannelHandlerContext ctx, Throwable cause) throws Exception {
        cause.printStackTrace();
        //从clients删除
        Server.clients.remove(ctx.channel());
        ctx.close();//关闭此流
    }
~~~

接下来客户端需要优雅的关闭，给客户端界面类添加窗口监听器，当关闭窗口时，调用客户端的关闭方法，再推出页面。客户端向服务端发送一个`_bye_`字符串，服务端接收消息进行判断，如果客户端发送的是bye，将此channel从组中移除，关闭此channel，如果不是，分发给channel组。

客户端界面

~~~java
        this.addWindowListener(new WindowAdapter() {
            @Override
            public void windowClosing(WindowEvent e) {
                c.closeConnect();
                System.exit(0);
            }
        });
~~~

客户端类

~~~java
    public void closeConnect(){
        this.send("_bye_");
    }
~~~

服务端类

~~~java
	@Override
    public void channelRead(ChannelHandlerContext ctx, Object msg) throws Exception {
        //数据存在msg中
        ByteBuf buf = null;
        try {
            buf = (ByteBuf)msg;
            byte[] bytes = new byte[buf.readableBytes()];
            buf.getBytes(buf.readerIndex(),bytes);//读到字节数组中
            String s = new String(bytes);
            if ("_bye_".equals(s)){
                System.out.println("客户端要退出");
                Server.clients.remove(ctx.channel());
                ctx.close();
            }else {
                //写到每一个通道组中
                Server.clients.writeAndFlush(buf);//不能再自己释放
            }
        }finally {
            //手动释放内存
//            if (buf != null) ReferenceCountUtil.release(buf);
//            System.out.println(buf.refCnt());
        }
    }
~~~

增加服务端的窗口，同样将服务器类的信息封装到serverConnect方法，同时当有信息的时候直接写入到服务器窗口类即可。其中要注意的是，因为服务器类会阻塞，因此为了避免UI线程被阻塞，起服务器的线程最好放在main线程中。同样为了方便服务器类与服务器窗口类通信，将服务器窗口类做成单例。

这时候完成了客户端与服务端字符串的通信。

#### Netty Codec

1、定义TankMsg x,y

2、TankMsgEncoder负责编码，继承自MessageToByteEncoder`<TankMsg>`

- 负责将TankMsg转为字节

3、TankMsgDecoder负责解码

- 将字节转为坦克消息类

4、在客户端加上编码的Handler

- 写消息的时候，直接写TankMsg即可

5、在服务端加上解码的Handler

- 读消息的时候，读出来的就是TankMsg

定义自己要处理的消息类

~~~java
public class TankMsg {
    public int x, y;
    public TankMsg(int x, int y){
        this.x = x;
        this.y = y;
    }
    @Override
    public String toString() {
        return "TankMsg{" +
                "x=" + x +
                ", y=" + y +
                '}';
    }
}
~~~

然后定义此类的加码类，将其转为字节，继承MessageToByteEncoder，指定要加码的类型

~~~java
//将TankMsg转换为字节
public class TankMsgEncoder extends MessageToByteEncoder<TankMsg> {
    @Override
    protected void encode(ChannelHandlerContext ctx, TankMsg msg, ByteBuf buf) throws Exception {
        buf.writeInt(msg.x);
        buf.writeInt(msg.y);
    }
}
~~~

此加码类也是一个Handler，在客户端将其加入到pipeLine责任链上，先加解码，再加InBound

~~~java
class ClientChannelInitializer extends ChannelInitializer<SocketChannel>{
    @Override
    protected void initChannel(SocketChannel ch) throws Exception {
        ch.pipeline()
                .addLast(new TankMsgEncoder())
                .addLast(new ClientHandler());
    }
}
~~~

然后在初始化的Handler中，写消息的时候直接写TankMsg即可，因为责任链上加了加码的责任链，因此在Channel中传输的时候自己被转为字节

~~~java
    @Override
    public void channelActive(ChannelHandlerContext ctx) throws Exception {
        ctx.writeAndFlush(new TankMsg(5,8));//自动由handler转为二进制
    }
~~~

为了解码，需要定义解码类，将字节转为自定义的消息，为了解决TCP的拆包与黏包的问题，即将一个消息分成多个包，在字节数不够的时候直接返回，按照编码的顺序进行解码，并加入到out这个集合中。

~~~java
public class TankMsgDecoder extends ByteToMessageDecoder {
    @Override
    protected void decode(ChannelHandlerContext ctx, ByteBuf in, List<Object> out) throws Exception {
        //没有读取完，返回，TCP拆包与黏包问题
        //字节数不够，就等着
        if (in.readableBytes()<8)return;
        //in.markReaderIndex();
        int x = in.readInt();
        int y = in.readInt();
        //将解析出来的消息放在List中
        out.add(new TankMsg(x,y));
    }
}
~~~

为了在服务端读取此消息，需要将此Handle加入到服务端的pipeLine上，先加解码的责任，这样在读取的时候，直接读取到的就是对应的TankMsg类

~~~java
class ServerChannelInitializer extends ChannelInitializer<SocketChannel>{
    @Override
    protected void initChannel(SocketChannel ch) throws Exception {
        //在每个责任链上加入一个handler
        ch.pipeline()
                .addLast(new TankMsgDecoder())
                .addLast(new ServerChildHandler());
    }
}
~~~

在读取的时候，直接将msg转为TankMsg读取

~~~java
	@Override
    public void channelRead(ChannelHandlerContext ctx, Object msg) throws Exception {
        //数据存在msg中
        System.out.println("channelRead ");
        try {
            TankMsg tm = (TankMsg)msg;
            System.out.println(tm);
        }finally {
            //手动释放内存
            ReferenceCountUtil.release(msg);
        }
    }
~~~

#### Codec单元测试

采用Junit单元测试的好处是，不用一个个去比对结果，避免人眼观察出错。复用测试，不用重新写测试。

在写自己定义的二进制加码解码时，先用Junit测试通过后，再组装。

EmbeddedChannel只用于单元测试上。

~~~java
public class TankMsgEncoderTest {
    @Test
    public void testTankMsgEncoder(){
        //往外写的测试
        TankMsg msg = new TankMsg(10,10);
        //使用Embedded嵌入的Channel，不是连接到网上
        //加入自己的Encoder
        EmbeddedChannel ch = new EmbeddedChannel(new TankMsgEncoder());
        //往外写消息
        ch.writeOutbound(msg);
        //将输出的消息读出来
        ByteBuf buf = (ByteBuf) ch.readOutbound();
        int x = buf.readInt();
        int y = buf.readInt();
        Assert.assertTrue(x == 10 && y == 10);
        buf.release();
    }
    @Test
    public void testTankMsgEncoder2(){
        //往里写的测试，先经过Decoder，再经过Encoder
        ByteBuf buf = Unpooled.buffer();
        TankMsg msg = new TankMsg(10,10);
        buf.writeInt(msg.x);
        buf.writeInt(msg.y);
        //加两个
        //写的是ByteBuf，先经过Decoder被转为msg，不符合Encoder的要求
        EmbeddedChannel ch = new EmbeddedChannel(new TankMsgEncoder(),new TankMsgDecoder());
        ch.writeInbound(buf.duplicate());
        TankMsg tm = (TankMsg)ch.readInbound();
        Assert.assertTrue(tm.x == 10 && tm.y == 10);
    }
}
~~~

### 同步异步阻塞非阻塞

同步异步关注的是**消息通信机制**

- 同步：烧水时，自己开开关，自己关。消息回来仍需要自己处理
- 异步：烧水时，自己打开，水开了后调用写好的代码去关。消息回来后其他人去处理。

阻塞非阻塞关注的是**等待消息时的状态**

- 阻塞：等烧水时，不做别的，盯着水
- 非阻塞：等烧水时，干点其他事

同步阻塞：自己开火后，等水开，水不开不做别的，自己关火。

同步非阻塞：自己开火，等水时去看电视，做点别的，自己关火。

异步阻塞：自己开火，自己盯着水看，水开后铃铛响，让其他工具关火。很少发生。

异步非阻塞：自己开火，做好火开的处理，自己去做别的事，让其他工具关火。

程序相当于人，操作系统相当于水。对accept于读写的处理要分开说明。

BIO同步阻塞，针对文件IO操作，用BIO的流读写文件，发起IO请求会被阻塞；NIO发起文件IO操作，在发起后进行了返回，但需要轮询OS看是否操作完。AIO为异步非阻塞，AIO发起文件IO操作后，立刻可以返回组别的，os处理完后，os来通知，并调用回调函数进行处理，不用自己处理。

### select，poll与epoll

三者均为IO多路复用，需要IO多路复用是因为，之前使用的是需要对所有的客户端fd进行顺序的读取操作，这样n个客户端涉及n次系统调用（用户态与内核态的切换），这样解决办法是，能否多个客户端，只一次系统调用，即一次性将所有的fd交给kernel处理，然后自己程序中对kernel返回的结果集进行处理。这样有了select，poll与epoll。

每个网络连接以文件描述符Fd的方式存在于内核中。

在单线程处理网络连接时，可以用如下这种简单的方式进行处理

~~~java
while(1){
	for(fd1 ~ fdn){
		if(fd有数据)
			读fd，处理
	}
}
~~~

#### select

而select，判断fd中有数据，从用户态拷贝rset(一个bitmap，存了fds信息)至内核态来判断，如果没有数据，select会阻塞，当有数据的时候，内核将rset对应的fd置位（表示有数据），select返回，程序继续运行，遍历全部fd（可能多个fd中有数据），判断哪个fd中有数据，将对应数据读取并处理。

select缺点

- bitmap默认大小为1024，大小有限
- 内核修改rset，FD_SET不可重用，需要重新创建bitmap
- 从用户态拷贝至内核态存在一定开销
- select后判断哪个fd中有数据，需要有O(n)的复杂度

#### poll

{% asset_img poll.png This is an example image %}

poll原理与select相似，poll的改进围绕传入的结构体，poll没有用bitmap，而是pollfd，其中fd为传入的fd，events在意的事件（读还是写），revents是对events的回馈。poll函数仍为阻塞，内核置位的时候，置的是revents字段，不像select修改bitmap会导致bitmap不可重用，poll返回。判断如果revents被置位，需要读取，将revents重置。

poll解决了select大小默认1024的问题，传入的结构体可以重用。但poll仍然存在select中剩下两个缺点

- 从用户态拷贝至内核态存在一定开销
- poll后判断哪个fd中有数据，需要有O(n)的复杂度

#### epoll

{% asset_img epoll.png This is an example image %}

epoll的epoll_event中有fd跟event两个字段，epfd暂且理解为容器，其中有5个epoll_event，既有fd，又有event。epfd是直接内存，省去了拷贝的过程，内核判断是否有数据到来，在没有数据的时候，也会阻塞，在有数据的时候，select与poll是置位+函数返回，在epoll中通过重排来置位，将有数据的fd放到最前面，然后返回fd触发事件的个数，这样只需要遍历触发时间个数的数据就可以处理完毕，时间复杂度从O(n)变为O(1)。

因此epoll解决了从用户态拷贝数据到内核态的过程，且轮询时间复杂度从O(n)减少至O(1)。