---
title: spring
date: 2020-09-23 19:06:22
tags: spring
categories: spring
---

Spring是轻量级的一站式框架，主要用来简化企业开发，核心有IOC和AOP

<!-- more -->

# spring

## IOC

IOC全称为控制反转，从没有IOC的时候开始看起。

在一个tomcat+servlet的架构里面，当tomcat进程起来后，去监听端口，使用servlet+jsp去处理请求，这时候如果用到了某个接口，需要自己在使用到此接口的地方，new出实现类，如果使用的地方很多，则需要在多处去进行new，当实现类有变动的时候，需要将原来所有调用到旧实现类的代码均改动，这样带来了比较大的代码修改和测试的成本。

当有了IOC后，在调用到此接口的地方，只需要使用xml或者注解来进行注入，将创建对象的权利反转交给了spring框架。spring持有容器，创建出对象后直接进行调用即可，这样减少了类之间的耦合性。底层使用反射来进行对象的创建。spring容器需要进行对象的创建及引用对象的注入。

## AOP

AOP为面向切面编程。

如果没有AOP，对于一些类里面的通用代码，需要在相关的方法里面全部去补充，这样有比较多的冗余代码，这样可以定义一个切面，使用动态代理的技术，当运行类A的方法时，其实运行的是类B中的同名方法，类B在此方法前后做共性方法，然后调用类A中的真正实现方法，这样减少了代码的耦合。

动态代理分为java Proxy和cglib。

优先使用proxy，然后是cglib。如果被代理类实现了接口，则使用proxy，如果没有实现接口，则使用cglib，本质被生成了个子类。

## Bean生命范围

1. singleton：默认，每个容器中只有一个bean的实例
2. prototype：为每一个bean请求提供一个实例
3. request：为每一个网络请求创建一个实例，请求完成后，bean会失效并被垃圾回收
4. session：与request范围类似，确保每个session中有一个bean实例，在session过期后，bean会失效
5. global-session

最常用为单例和多例，bean不是线程安全的，在访问单实例bean时，因为tomcat为多线程访问，因此会调用读个线程来进行请求的分发，如果spring的service层没有实例变量，那么就不会有并发访问变量的请求，一般为并发访问数据库，没有太大问题。

## 事务机制

### 事务实现

 事务的实现：使用@Transcational注解，spring使用aop的思想，在方法执行前，先开始事务，然后执行方法，执行完毕后，根据方法是否报错，来决定回滚还是提交事务。

### 传播机制

解决的是多个有事务注解的方法在调用时，事务如何传播的问题。

场景1：A方法执行失败，B方法不受影响，使用NEW，两个为不同事务

场景2：B方法失败，只有B回滚，A失败，会带着B回滚，使用嵌套事务，NESTED  

1、PROPAGATION_REQUIRED：如果当前没有事务，就创建一个新事务；如果当前存在事务，就加入该事务，最常用

2、PROPAGATION_SUPPORTS：支持当前事务，如果当前存在事务，就加入该事务；如果当前不存在事务，就以非事务执行

3、PROPAGATION_MANDATORY：支持当前事务，如果当前存在事务，就加入该事务，如果当前不存在事务，就抛出异常

4、PROPAGATION_REQUIRES_NEW：创建新事务，无论当前存不存在事务，都创建新事务 

5、PROPAGATION_NOT_SUPPORTED：以**非事务**方式执行操作，如果当前存在事务，就把当前事务挂起

6、PROPAGATION_NEVER：以非事务方式执行，如果当前存在事务，则抛出异常

7、PROPAGATION_NESTED：如果当前存在事务，则在嵌套事务内执行；如果当前没有事务，则按REQUIRED属性执行（嵌套事务：外层的事务如果回滚，会导致内存的事务也回滚；但内存的事务如果回滚，仅回滚自己的代码）

## 启动流程

1、实例化bean：根据xml配置文件或者注解进行Bean的实例化

2、依赖注入：将当前bean依赖的bean记性注入，如通过构造函数和setter方法

3、处理aware接口（将容器传递给bean）：spring会检测该对象是否实现了xxxAware接口，将相关的xxxAware实例注入给bean。如果这个bean已经实现了ApplicationContextAware接口，spring容器会调用bean的setApplicationContext(ApplicationContext)方法，传入spring上下文，将spring容器传递给这个bean

4、BeanPostProcessor：如果想在bean实例构建好后， 若在这个时候，需要对bean进行一些自定义的处理，可以让bean实现BeanPostProcessor接口，会调用postProcessBeforeInitialization(Object obj, String s)方法

5、InitializaingBean与init-method：如果Bean在spring配置中间中配置了init-method属性，会自动调用其配置的初始化方法

6、如果Bean实现了BeanPostProcessor接口，会调用postProcessAfterInitialization(Object obj, String s)方法；由于这个方法是在Bean初始化结束时调用的，可以被应用于**内存或缓存技术**

以上几个步骤完成后，Bean就初始化完毕

7、DisposableBean：当bean不在需要，进行清理阶段，如果Bean实现了DisposableBean这个接口，会调用其实现的destroy()方法

8、destroy-method：如果这个Bean的spring配置中配置了destroy-method属性，会自动调用其配置的销毁方法

创建+初始化一个bean -> spring容器管理的bean 长期存活 -> 销毁bean（两个回调函数）

{% asset_img bean流程.png This is an example image %}

## 设计模式

工厂、单例、代理

工厂：对象创建工厂封装在工厂类，spring ioc核心的设计模式思想体现，他自己就是一个大工厂，将所有的bean实例都给放在了spring容器里（大工厂），若自己使用bean，就找spring容器即可，不用自己创建

单例：spring默认来说，每个bean都走的是一个单例模式，确保说类在系统运行期间只有一个实例对象，只有一个bean

代理模式：若要对一些类的方法切入一些增强的代码，会创建一些动态代理的对象，让对目标对象的访问，先经过动态代理对象，动态代理对象先做一些增强的代码，调用目标对象。让动态代理的对象，去代理了目标对象，在这个过程中做一些增强的访问

# spring mvc

1、客户端（浏览器）发送请求，tomcat工作线程将工作线程将请求转交给DispatcherServlet（请求DispatcherServlet）

2、查找@Controller，一般给controller加上@RequestMapping注解，标注哪些controller处理哪些请求，根据请求的uri，定位到对应的controller

3、根据@RequestMapping去查找，使用这个controller内的哪个方法来进行请求的处理，对每个方法一般也会加@RequestMapping

4、直接调用controller中的方法进行处理，调用service,dao层

5、controller方法有返回值，以前将前端页面放在后端的工程里，返回页面模板名字，spring mvc框架使用模板技术，对html页面做渲染；现在返回json串，前后端分离，前端发送请求，后端返回json数据

6、把渲染后的html页面返回给浏览器进行显示；前端负责把html页面返回给浏览器

# spring boot

springboot一定程度上简化了单纯用spring的开发流程，如果只用spring，需要写比较多的配置，将代码部署至tomcat，tomcat监听端口，然后调用mvc框架。使用springboot后，其内嵌了tomcat，一下子可以将写好的java web项目启动起来，更简便。

自动装配：要引入mybatis，引入一个starter依赖，一定程度上自动完成mybatis的一些配置和定义，不需要自己手工去做大量的配置，只需要做一些简单的、必要的配置。免去了手动配置+自定义bean的过程。