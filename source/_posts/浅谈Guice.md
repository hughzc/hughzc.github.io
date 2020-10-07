---
title: 浅谈Guice
date: 2020-07-14 16:40:38
tags: Guice
categories: Guice
---

## 是什么

Guice作为Google的依赖注入（DI）框架，其模块化的概念，有利于平台的模块化开发

下面将针对某个场景，比较不使用Guice与使用Guice来实现功能进行比较

<!-- more -->

## 优势

现在的应用场景为，一个人生活中有工作与玩两个方法，最顶层为工作与玩的接口

~~~java
public interface Work {
    void work();
}
public interface Play {
    void play();
}
~~~

然后工作有两个实现类，学数学和学英语

~~~java
public class MathWork implements Work {
    public void work() {
        System.out.println("Math work");
    }
}
public class EnglishWork implements Work {
    public void work() {
        System.out.println("English work");
    }
}
~~~

玩也有两个实现类，玩体育和玩电脑

~~~java
public class ExercisePlay implements Play {
    public void play() {
        System.out.println("play pingpang eg");
    }
}
public class ComputerPlay implements Play {
    public void play() {
        System.out.println("play computer");
    }
}
~~~

对于生活来说，工作与休息均不可少，定义底层的工作接口

~~~java
public interface Life {
    void balenceLife();
}
~~~

而不同的人有不同的生活方式，定义workCount与playCount，如果workCount是1代表学习数学，如果是2代表学习英语；如果playCount是1代表锻炼，是2代表玩电脑。

```java
public class Constant {
    public static final int MATHWORK = 1;
    public static final int EnglishWORK = 2;
    public static final int EXERSICEPLAY = 1;
    public static final int COMPUTERPLAY = 2;
}
```

需要依据传入的不同的数据来进行不同的Work与Play，具体实现如下

~~~java
public class OldLife implements Life{
    private int workCount;
    private int playCount;
    private Work work;
    private Play play;
    public OldLife(int workCount, int playCount){
        this.workCount = workCount;
        this.playCount = playCount;
    }
    public void balenceLife() {
        if (workCount == Constant.MATHWORK){
            work = new MathWork();
        }else {
            work = new EnglishWork();
        }
        if (playCount == Constant.EXERSICEPLAY){
            play = new ExercisePlay();
        }else {
            play = new ComputerPlay();
        }
        work.work();
        play.play();
    }
}
~~~

具体的调用为

~~~java
Life oldLife = new OldLife(i,j);
oldLife.balenceLife();
~~~

可以看到OldLife这个类与Work与Play的类均强耦合在一起，写起来不是很优雅，下面用Guice去做

## 怎么用

在IDEA中创建Maven项目，并在pom.xml中导入Guice的依赖

~~~xml
        <dependency>
            <groupId>com.google.inject</groupId>
            <artifactId>guice</artifactId>
            <version>3.0</version>
        </dependency>
~~~

Life的实现类中，不直接耦合Work与Play的实现类，而是依赖注入

~~~java
public class RealLife implements Life {
    private Work work;

    private Play play;

    @Inject
    public RealLife(Work work, Play play){
        this.work= work;
        this.play = play;
    }
    public void balenceLife() {
        work.work();
        play.play();
    }
}
~~~

通过调用注入的Work与Play相应的方法来实现功能

然后需要一个模块的配置类，相当于把不同的插头绑定到对应的接口上

~~~java
public class LifeModule extends AbstractModule {
    private int workCount;
    private int playCount;
    public LifeModule(int workCount, int playCount){
        this.workCount = workCount;
        this.playCount = playCount;
    }
    @Override
    protected void configure() {
        switch (workCount){
            case Constant.MATHWORK:
                bind(Work.class).to(MathWork.class);
                break;
            case Constant.EnglishWORK:
                bind(Work.class).to(EnglishWork.class);
                break;
            default:
                break;

        }
        switch (playCount){
            case Constant.EXERSICEPLAY:
                bind(Play.class).to(ExercisePlay.class);
                break;
            case Constant.COMPUTERPLAY:
                bind(Play.class).to(ComputerPlay.class);
                break;
            default:
                break;

        }
        bind(Life.class).to(RealLife.class);
    }
}
~~~

这样可以根据传入的值来动态绑定接口的实现类

在调用的时候，需要使用Injector来获取对应的服务，然后调用服务

~~~java
        // 可以从配置文件中读取
        for (int i = 1; i <= 2; i++) {
            for (int j = 1; j <= 2; j++) {
                Injector injector = Guice.createInjector(new LifeModule(i,j));
                Life life = injector.getInstance(Life.class);
                life.balenceLife();
            }
        }
~~~

总结下，使用Guice依赖注入，可以解决服务类与具体实现类强绑定的痛点，依赖接口编程，极大的减少了类之间的耦合，通过使用一个模块类来配置实现类与接口的绑定，程序可扩展性较好，也一定程度上消除了if else。