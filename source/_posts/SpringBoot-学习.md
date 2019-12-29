---
title: SpringBoot 学习
date: 2019-12-29 18:45:23
tags: SpringBoot
---

# SpringBoot学习

## 涉及的一些知识点

- SpringBoot
- Spring MVC
- MyBatis
- MySQL、H2
- Flyway
- Heroku
- Git/Github
- Maven
- Restful

## 项目创建

​	选择Spring，进行创建

### 初始创建文件夹

​	src.main文件中包括java文件夹（存放**Java**文件）和resources（存放**静态**文件，**Web**or**模板**文件和**配置**文件）。

​	java文件夹下的application作用是可以作为SpringBoot项目启动的窗口。

### 网址构成

https://www.bilibili.com/video

​	对应的是https或者http+域名+路径

- 绑定本机的地址是localhost/120.0.0.1  (可以通过此地址访问本机项目承载地址)

- 路径是一个path，因此网络地址对应到本机可以是localhost/120.0.0.1/video
- 最终对应的是http://localhost:8887/hello  (Tomcat默认的端口号是**8080**)

### 正式编程

#### 实现输入网址返回可视化界面

##### 文档说明

1. 使用[Spring官方文档](https://spring.io/guides)下的[Serving Web Content with Spring MVC](https://spring.io/guides/gs/serving-web-content/)，找到其依赖的dependency

   ，复制进项目，导入相应的包。

2. 创建Web资源，写Controller，写Controller注解，写Mapping，通过路由的方法，可以接受各种资料。

   ~~~java
   @Controller
   public class GreetingController {
   	@GetMapping("/greeting")
   	public String greeting(@RequestParam(name="name", required=false, defaultValue="World") String name, Model model) {
   		model.addAttribute("name", name);
   		return "greeting";
   	}
   }
   ~~~

   作用是以网址形式向服务端传送key-value信息。

   - name=hughzc

   也可以进行接收，如name=“name”，前面定义变量，后面用来接收值。**GetMapping**方法可以获得从浏览器接收的参数。然后使用**model**来显示信息，类似map，将信息向前端传递。key是”“name”，value是前端传过来的name，接收到并set到model中。返回greeting。

   > 体现的编程思路：仔细阅读文档。

   ​	因为返回greeting，因此定义的文件名为greeting，具体为

   ~~~
   src/main/resources/templates/greeting.html
   ~~~

   里面的内容为：

   ~~~html
   <!DOCTYPE HTML>
   <html xmlns:th="http://www.thymeleaf.org">
   <head>
       <title>Getting Started: Serving Web Content</title>
       <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
   </head>
   <body>
       <p th:text="'Hello, ' + ${name} + '!'" />
   </body>
   </html>
   ~~~

   head跟body就是比较基础的html语言，不一样的是加入了标签，告知spring使用模板引擎去解析。

   ~~~html
   <html xmlns:th="http://www.thymeleaf.org">
   ~~~

   要做的是

   ~~~html
   <p th:text="'Hello, ' + ${name} + '!'" />
   ~~~

   spring动态语言，将 model中的key为name的信息替换掉，p标签中的内容是Hello，+输入内容。

3. 让当前的程序可执行

   目前可以直接调用main函数，后续会导成jar包。

   ~~~
   java -jar build/libs/gs-serving-web-content-0.1.0.jar
   ~~~

   利用maven打成jar包，放到服务器，在服务器上直接运行上述命令即可。

##### 实际操作

​	SpringBoot特点是所有带有注解的文件，只要在当前application的同一级或下一级目录，就会自动加载进来。

1. 并列application的同一级创建一个package为**controller**

   - 新建类，写注解@Controller，识别此类作为spring的bean管理，同时为一个controller（允许此类接收前端的请求）。新建一个返回值为String的hello方法，加上注解@GetMapping(“/hello”)。hello方法接收参数为@RequestParam，请求的是name，定义name跟model。

     将浏览器中传入的值放入model中，

     输出hello，此时会自动去resources.templates下去找模板。

     ~~~java
     @Controller
     public class HelloController {
         @GetMapping("/hello")
         public String hello(@RequestParam(name = "name") String name, Model model){
             model.addAttribute("name",name);
             return "hello";
         }
     }
     ~~~

   - 在templates下新建**html**文件**hello**(与contrller中返回类型同名)，代码为

     ~~~html
     <!DOCTYPE HTML>
     <html xmlns:th="http://www.thymeleaf.org">
     <head>
         <title>Getting Started: Serving Web Content</title>
         <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
     </head>
     <body>
     <p th:text="'Hello, ' + ${name} + '!'" />
     </body>
     </html>
     ~~~

   - 启动SpringBoot，输入http://localhost:8080/hello?name=hughzc，返回结果。hello名为html文件名。8080为默认端口。

     > Hello, hughzc!

   - 修改端口号，在**配置**文件(resources.application.properties)中进行修改，点击重新启动

     ~~~
     server.port=8887
     ~~~

     

## IDEA快捷键技巧

- ctrl+P：提示输入参数类型

学到知识

1. Maven是管理包和包的依赖的工具，pom.xml中包括所有运行Spring项目需要的包，主要为依赖parent
2. gitignore用于只提交部分的代码，避免一些文件冲突

