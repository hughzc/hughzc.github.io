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
<!-- more -->
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

   ，复制进项目的**pom.xml**进行maven配置，导入相应的包。

2. 创建Controller包，在其中新建一个Controller名称的类，整体标注上@Controller注解。

   在Spring建立网站的方法中，**HTTP的请求由Controller处理**，通过@Controller注解来识别控制器。GreetingController通过返回View的名称（在此处为greeting）来**处理GET请求**/ greeting。 View负责呈现HTML内容。

   @GetMapping注解确保将对/ greeting的HTTP **GET请求**映射到greeting（）方法。

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

   > @Controller，将当前类作为路由API的一个承载者
   >
   > @GetMapping，确保对其中内容的HTTP GET请求映射到greeting方法

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

过程梳理：

1. 在浏览器输入网址`http://localhost:Port/hello?name=name值`
2. HTTP的GET请求由controller处理，GetMappint(“/hello”)到对应的hello方法中，传入的name进行接收，返回String类型的hello
3. 在templates下寻找hello.index，进行前端页面显示，将name进行替换

## 将代码放至Github托管

### 简单流程

1. 添加SSH keys，设置当前仓库的权限

2. 在Github新建仓库

3. git init新建仓库

4. git add, commit 代码

5. 修改git文件，加入

   ~~~java
   [user]
   	name = 
   	email = @163.com
   ~~~

5. 如果这时候有新的commit，可以使用代码，进行commit的追加

   ~~~
   git commit --amend  --no-edit
   ~~~

6. 使用git push，将本地文件更新至仓库

## 明确需求

### 目标效果

[elastic社区](https://elasticsearch.cn/)

- 最上面为导航，功能分类，搜索功能， 登录
- 下面为一系列Tag
- 再下面为主要板块，topic列表，包括评论数，时间，人等元素
- 右边为热门元素

## Bootstrap

### 基本认识

​	[官方网站](https://v3.bootcss.com/getting-started/#download)，Bootstrap为前端UI框架，可以快速搭建出一个前端页面，在**组件**中可以浏览所需要的功能，前端组件轮子集合，可以极大减小开发量。

​	可以做到响应式布局，使用到了[栅格系统](https://v3.bootcss.com/css/#grid)，通过media设置尺寸，在不同屏幕尺寸下做不同的CSS样式。将整个浏览器分为12份，通过前缀后的数字组合实现快速布局。

### Bootstrap编写导航栏样式

- 下载Bootstrap

- 复制进src/main/resources/static(放资源文件)

- 更改resources/templates/hello.html为index.html
  - 修改社区名称<titile>
  - 删除body中内容

- 引入bootstrap，样式和js文件

  可直接将所需要的文件拖入，自动加入其路径

  ~~~html
  <link rel="stylesheet" href="css/bootstrap.min.css" />
  <link rel="stylesheet" href="css/bootstrap-theme.min.css" />
  <script src="../static/js/bootstrap.min.js" type="application/javascript" </script>
  ~~~

- 拷贝需要的组件代码至body中

  - 导航条

    修改名字，删去不需要的内容

    初始样式效果

    {% asset_img 导航范例.png 初始结果 %}

    理想修改效果

    {% asset_img 理想社区导航.png 理想结果 %}

- 创建indexController

  - 给注解Contrller
  - 匹配路径为根目录，即(“/”)，什么都不输入默认访问此路径
  - 返回index模板

  ~~~java
  @Controller
  public class indexController {
      @GetMapping("/")
      public String index(){ return "index";};
  }
  ~~~

  实际修改效果

  {% asset_img 实际效果.png 实际结果 %}

## 利用Github App实现登录

​	实现登录功能，接入Github。查看API文档，找到[OAuth Apps](https://developer.github.com/apps/)，依据提示进行操作。

### 创建APP

​	[依据网站提示](https://developer.github.com/apps/building-oauth-apps/creating-an-oauth-app/)，在Github设置中进行创建

1. 填写APP名称

2. 主页URL

3. [callback URL](https://developer.github.com/apps/building-oauth-apps/authorizing-oauth-apps/)

   - 用户被重定向以请求他们的GitHub身份
   - 用户通过GitHub被重定向回您的站点
   - 您的应用程序使用用户的访问令牌访问API

   需要拿到一些用户数据，正常来说直接在主页URL后加入callback，但是为了本地调试方便，地址为

   ~~~http
   http://localhost:8887/callback
   ~~~

### Github登录流程

​	使用[Visual Paradig](https://www.visual-paradigm.com)利用时序图梳理登录流程。

1. 创建项目

2. 创建diagram，选择Sequence diagram，表示对象和对象通过时间传递传递消息的路线。

   用户向个人社区发出访问，社区进行登录。

   - 给Github发出认证，Github认证后回调 redirect_uri并携带code
   - 用户持access_token 携带code，若匹配，Github返回access_token 
   - 站点发送user 携带access_token给Github，Github返回user信息
   - 站点将user信息存入数据库，更新给用户登录状态
   - 用户显示登录成功

{% asset_img github登录流程.png 登录流程 %}

#### 调用authorize

​	将登录按钮绑定地址，点击登录时可以跳转到指定地址并携带写入参数。将地址href写成[参考文档](https://developer.github.com/apps/building-oauth-apps/authorizing-oauth-apps/)中所给。

> URL中有多个参数时，第一个参数用**?**区分，之后的参数用**&**区分

​	在index.html中找到登录，传入必要参数(clien_id，redirect_uri，scope，state)

~~~html
<li><a href="https://github.com/login/oauth/authorize?client_id=github上给的&redirect_uri=http://localhost:8887/callback&scope=user&state=1">登录</a></li>
~~~

​	这时候点击登录并授权后，网站会返回`code`信息，后面将`code`信息提取出来

#### 获取code

1. 写新的Controller

   返回String类型的`callback()`，返回index主页，在calback中寻找，写`GetMapping`注解，需要传入参数，为用户写入的String类型的code和state。

   ~~~java
   @Controller
   public class authorizeController {
       //指向返回文件
       @GetMapping("/callback")
       //使用name接收code，为String类型
       //使用name接收state，为String类型
       public String callback(@RequestParam(name = "code") String code,
                              @RequestParam(name = "state") String state){
           //登录成功后返回index页面
           return "index";
       }
   }
   ~~~

2. 模拟post请求

   使用[OkHttp](https://square.github.io/okhttp/)

   ~~~java
   public static final MediaType JSON
       = MediaType.get("application/json; charset=utf-8");
   OkHttpClient client = new OkHttpClient();
   String post(String url, String json) throws IOException {
     RequestBody body = RequestBody.create(json, JSON);
     Request request = new Request.Builder()
         .url(url)
         .post(body)
         .build();
     try (Response response = client.newCall(request).execute()) {
       return response.body().string();
     }
   }
   ~~~

​	新建一个包provider，提供对第三方信息的支持能力，新建类`GithubProvider`

> 体现思想：不同业务间进行隔离

​	写注解`@Component`

> @Component 仅仅把当前类初始化Spring容器的上下文，这样调用时不用实例化对象，IOC，便于去调用对象

​	查看文档，在使用Post请求时，有5个参数，当编程参数超过2个，不要使用形参传入，而是封装成对象。

​	新建一个包dto，即数据传输模型，新建类AccessTokenDTO，里面有需要传输的5个参数，快捷键alt+insert快速创建其getter和setter方法。

​	类GithubProvider中编写方法getAccessToken，接收AccessTokenDTO类型的数据。在此方法中拷贝OkHttp中的方法，引入jar包。

​	在Maven中添加依赖，找到pom.xml，将依赖代码复制进依赖中。

> [Maven包查询](https://mvnrepository.com/)

将不必要的修饰删除，如public static final修饰。

- json就是自定义接收变量的access_token类变量，为了将access_token转化为json，需要jar包，使用[fastjson](https://mvnrepository.com/artifact/com.alibaba/fastjson)，将其依赖复制进pom.xml。方法中需要json，使用JSON.toJSONString(accessTokenDTO)，自动将accessToken转化成String类型的JSON。
- url更改成[参考文档](https://developer.github.com/apps/building-oauth-apps/authorizing-oauth-apps/)中给出的POST地址
- 处理IO异常
- 最后返回null，如果取到了返回body。因为怀疑此body不是我们所需要的，因此可以先将其打印

> 遇到不知道是否正确的变量，可以打印进行调试

~~~java
    public String getAccessToken(AccessTokenDTO accessTokenDTO){
        MediaType mediaType = MediaType.get("application/json; charset=utf-8");

        OkHttpClient client = new OkHttpClient();

        RequestBody body = RequestBody.create(JSON.toJSONString(accessTokenDTO), mediaType);
        Request request = new Request.Builder()
                .url("https://github.com/login/oauth/access_token")
                .post(body)
                .build();
        try (Response response = client.newCall(request).execute()) {
            String string = response.body().string();
            System.out.println(string);
            return string;
        } catch (IOException e) {
        }
        return null;
    }
~~~

​	GithubProvider的getAccessToken已经封装好，在controller/AuthorizeController中进行调用。通过@Component注解 ，已经将GithubProvider放到了Spring的容器中，利用@Autowired注解将Spring容器中的写好的实例化的实例加载到当前使用的上下文。通过注解加定义实例将实例化好的放入，使用方便。

~~~java
    @Autowired
    private GithubProvider githubProvider; 
~~~

​	在AuthorizeController的calback方法中，将需要的5个参数封装进AccessToken，调用githubProvider的getAccessToken方法实现post请求。

​	这时候启动项目，在localhost:8887页面点击登录，没有报错，而且在控制台打印了所需要的access_token。

> 疑问，在AccessController中的mapping “callback”没有自己写，是Spring封装好的吗？
>
> 之前url有一个是callback的，应该是之前的那个

​	即返回如下所示的字符串。这样通过access_token的api获取到了access_token。

```java
access_token=e72e16c7e42f292c6912e7710c838347ae178b4a&token_type=bearer
```

​	要获取到access_token，先用&拆分，然后用=拆分。对应的更改代码为

~~~java
String string = response.body().string();
String token = string.split("&")[0].split("=")[1];
return token;
~~~

#### 获取用户信息

官方文档第三步

> The access token allows you to make requests to the API on a behalf of a user.
>
> ```
> Authorization: token OAUTH-TOKEN
> GET https://api.github.com/user
> ```
>
> For example, in curl you can set the Authorization header like this:
>
> ```
> curl -H "Authorization: token OAUTH-TOKEN" https://api.github.com/user
> ```

​	在[github界面](https://github.com/settings/tokens/new)创建新的personal access token，拿到user信息，用来测试。复制token用于验证是否是调用API就可以拿回数据。

​	验证方法，使用文档中给定的GET网址，加上github创建的access_token，ctrl+shift+n，在无痕网址界面进行验证，确实可以得到用户个人信息。测试完毕，删除此access_token。

~~~http
https://api.github.com/user?access_token=自己的access_token
~~~

​	在GithubProvider中再写方法，通过access_token获取到用户信息。需要的参数是id，name，bio。

​	在dto中创建对象GithubUser。有三个属性，String的name和bio,long类型的id以避免越界。GithubUser是GithubProvider的一个返回值。使用[okhttp](https://square.github.io/okhttp/)的get方法。

~~~java
OkHttpClient client = new OkHttpClient();

String run(String url) throws IOException {
  Request request = new Request.Builder()
      .url(url)
      .build();

  try (Response response = client.newCall(request).execute()) {
    return response.body().string();
  }
}
~~~

​	url为测试所用的url，但accesstoken不同。

​	浏览器返回的是json的格式，拿到获取的response的string，使用**JSON.parseObject(string)**，将**string**自动转化为java类对象。这样将String的json对象去自动解析转换成java的类对象。

~~~java
    public GithubUser getUser(String accessToken){
        OkHttpClient client = new OkHttpClient();
        Request request = new Request.Builder()
                .url("https://api.github.com/user?access_token="+accessToken)
                .build();
        try {
            Response response = client.newCall(request).execute();
            String string = response.body().string();
            GithubUser githubUser = JSON.parseObject(string,GithubUser.class);
            return githubUser;
        } catch (IOException e) {
        }
        return null;
    }
~~~

​	在AuthorizeController中调用GithubProvider.getUser(accessToken)，将user进行打印，看是否是所需要的。通过github的user的api携带access_token获取到了用户信息。

> 出现错误，在点击登录的时候并灭有出现理想的user姓名，而是空，先检查了access_token，发现是正确的，则说明是getUser()方法出现了问题，进行检查后发现是url里面少写了一个“=”。

~~~java
@Controller
public class AuthorizeController {
    //Autowired与Component，调用GithubProdiver
    @Autowired
    private GithubProvider githubProvider;
    //指向返回文件
    @GetMapping("/callback")
    //使用name接收code，为String类型
    //使用name接收state，为String类型
    public String callback(@RequestParam(name = "code") String code,
                           @RequestParam(name = "state") String state){
        AccessTokenDTO accessTokenDTO = new AccessTokenDTO();
        accessTokenDTO.setClient_id("网站给定");
        accessTokenDTO.setClient_secret("网站给定");
        accessTokenDTO.setCode(code);
        accessTokenDTO.setState(state);
        accessTokenDTO.setRedirect_uri("http://localhost:8887/callback");
        String accessToken = githubProvider.getAccessToken(accessTokenDTO);
        GithubUser user = githubProvider.getUser(accessToken);
        System.out.println(user.getName());
        //登录成功后返回index页面
        return "index";
    }
}
~~~

## 配置文件的分离application.proerties

> 体现的思路：要用到的时候在进行设计，不要过度设计

​	之前将各种信息都写在了代码中，在线上部署的时候，地址这些需要手动修改，比较麻烦，要做到不同的环境去读取配置文件，将需要的信息写上不同的内容。写在application.proerties中。需要的有Client_id，Client_secret和Redirect_uri。

> server.port = 8887，含义是server下面的port值是8887

​	修改配置文件为

~~~properties
github.client.id = 给定的
github.client.secret = 给定的
github.redirect.uri = http://localhost:8887/callback
~~~

​	通过value注解来使用

> @Value(“${string}”)，在配置文件中读取key为string的value

~~~java
    @Value("${github.client.id}")
    private String clientId;
    @Value("${github.client.secret}")
    private String clientSecret;
    @Value("${github.redirect.uri}")
    private String redirectUri;
~~~

​	然后可以通过变量名直接使用。

~~~java
	accessTokenDTO.setClient_id(clientId);
	accessTokenDTO.setClient_secret(clientSecret);
    accessTokenDTO.setRedirect_uri(redirectUri);
~~~

## Session和Cookies原理和实现

​	Session的简单理解是在银行开户，银行中存有你的账户信息。Cookies是银行卡，去银行取钱需要银行卡。在网页的application下可以看到cookie

{% asset_img cookies.png This is an example image %}

​	看到有多张银行卡

- domain代表银行，cookie不能跨域，每条记录都有个域名，路径和过期时间，如15天可用这种。
- name相当于卡号
- value相当于卡号对应的唯一标识

​      可在network中看向服务端请求的信息。Request中有cookie信息，服务端通过cookie找到session，返回，渲染到页面。

​      要做到的效果是，没有登录的时候，有登录和我，登录成功后，我变成登陆者的名字，去掉登录。 

​     在AuthorizeController下有callback，在user不为空的时候，可以证明登录成功。

​	session在httprequest中拿到，将user对象放入session，此时框架集成了功能，使得前端有了指定的cookie。重新返回到index主页。使用redirect会将地址那些去掉，重定向到指定界面。这时候就写到了cookie，还需要在页面展示。

​	在前端界面进行修改，如果有session，则展示我。

> 不知道如何实现，则直接baidu，用关键词搜索，不要用自己的上下文语义

​	在callback()方法中增加参数 **HttpServletRequest request**，在接收到user后写上判断语句

~~~java
		if(user != null){
            //登录成功，写cookie和session
            //将user对象放入session，框架集成，前端有了银行卡
            request.getSession().setAttribute("user",user);
            return "redirect:index";
        }else {
            //登录失败，重新登陆
            return "redirect:index";
        }
~~~

​	在前端页面修改逻辑，如果session.user不为空，显示我；如果为空，显示登录

> 在改动大的时候，注意看控制台，会有提示信息

~~~html
                <li class="dropdown" th:if="${session.user != null}">
                    <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-haspopup="true"
                       aria-expanded="false">我 <span class="caret"></span></a>
                    <ul class="dropdown-menu">
                        <li><a href="#">消息中心</a></li>
                        <li><a href="#">个人资料</a></li>
                        <li><a href="#">退出登录</a></li>
                    </ul>
                </li>
                <li th:unless="${session.user == null}">
                    <a href="https://github.com/login/oauth/authorize?client_id=eb92e00b211c84362136&redirect_uri=http://localhost:8887/callback&scope=user&state=1">
                    登录</a></li>
~~~

​	但是没有登录也没有我，修改语法。将unless语法修改为

~~~html
<li th:unless="not ${session.user != null}">
     <a href="https://github.com/login/oauth/authorize?client_id=eb92e00b211c84362136&redirect_uri=http://localhost:8887/callback&scope=user&state=1">
     登录</a></li>
~~~

​	还是不行，进行调试，因为还没有点击登录，因此session为空。在页面中打印user,在GithubUser类中，重写其toString()方法。

~~~html
<div th:text="${session.user}"></div>
~~~

​	在网页的检查，element中找到代码对应相应位置，可以看到没有值。

​	重新修改语法，两个都使用if。

~~~html
<li th:if="${session.user == null}">
     <a href="https://github.com/login/oauth/authorize?client_id=eb92e00b211c84362136&redirect_uri=http://localhost:8887/callback&scope=user&state=1">
     登录</a></li>
~~~

​	然后可以进行提交，但没有重定向到主页，说明重定向语法写错，修改为

~~~java
return "redirect:/";
~~~

​	这时候登录后变成我，而且刷新也不变化，说明已经将登录态记住了。这时候查看Application，出现了cookies。默认写了一个key，发送请求时，将name和value拼到cookie请求头中，发到服务端，通过cookie的key去找requst的session

{% asset_img 默认cookie.png This is an example image %}

​	然后将显示的我修改成个人名称，在index中将我去掉，添加**th:text="${session.user.getName()}**变成如下

~~~html
<a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-haspopup="true" aria-expanded="false" th:text="${session.user.getName()}" ><span class="caret"></span></a>
~~~

​	但是每次重启服务器，用户都需要重新登录，如何解决？将登录信息持久化到数据库。

## MySQL基本概念及语法

​	MySQL是一个DataBase(数据库)，在数据库下有Table(表)，用表来承载数据，表下面是记录Record，之间为组合关系。没有一者，另一者也不存在。

{% asset_img 数据库基本三层模型.png This is an example image %}

​	[MySQL菜鸟教程资料](https://www.runoob.com/mysql/mysql-tutorial.html)。

- 创建数据库

  ~~~sql
  CREATE DATABASE 数据库名;
  ~~~

- 创建数据表

  ~~~sql
  CREATE TABLE table_name (column_name column_type);
  ~~~

  在 RUNOOB 数据库中创建数据表runoob_tbl：

  ~~~sql
  CREATE TABLE IF NOT EXISTS `runoob_tbl`(
     `runoob_id` INT UNSIGNED AUTO_INCREMENT,
     `runoob_title` VARCHAR(100) NOT NULL,
     `runoob_author` VARCHAR(40) NOT NULL,
     `submission_date` DATE,
     PRIMARY KEY ( `runoob_id` )
  )ENGINE=InnoDB DEFAULT CHARSET=utf8;
  ~~~

   具体含义为：如果存在就删除，不存在就创建表，具体有int类型的id（逐渐自动增长），String类型的title，author，工具栏日Date类型的date。PRIMARY KEY表示每个表有自己的主键。

- 插入数据

  ~~~sql
  INSERT INTO table_name ( field1, field2,...fieldN )
                         VALUES
                         ( value1, value2,...valueN );
  ~~~

  具体例子为

  ~~~sql
  mysql> INSERT INTO runoob_tbl 
      -> (runoob_title, runoob_author, submission_date)
      -> VALUES
      -> ("学习 PHP", "菜鸟教程", NOW());
  ~~~

- 查询数据

  ~~~sql
  SELECT column_name,column_name
  FROM table_name
  [WHERE Clause]
  [LIMIT N][ OFFSET M]
  ~~~

  含义为查询…，从….中，在…条件下，限制N个，从M开始。通过分页从数据库中拿出数据。

  具体例子为

  ~~~sql
  select * from runoob_tbl;
  ~~~

- Update更新

  ~~~sql
  UPDATE table_name SET field1=new-value1, field2=new-value2
  [WHERE Clause]
  ~~~

  具体例子为

  ~~~sql
  UPDATE runoob_tbl SET runoob_title='学习 C++' WHERE runoob_id=3;
  ~~~

- delete删除

  ~~~sql
  DELETE FROM table_name [WHERE Clause]
  ~~~

  具体例子为

  ~~~sql
  ELETE FROM runoob_tbl WHERE runoob_id=3;
  ~~~

  删除id=3时的数据。

## H2数据库

 [H2数据库](http://h2database.com/html/main.html)是一个快速的内置数据库，通过浏览器或控制台直接仿问，在pom中添加其依赖，在IDEA中的Database添加H2数据库，使用embedded连接方式，路径选择为：在根目录下，以项目命名。

> 在WIN10下，选择Data Source From Path，更好用

```
jdbc:h2:C:/Code/mycommunity
```

- 创建一个table user，增加一个column，name为id，自动增长，主键
- 增加一个account_id，类型为VARCHAR(100)，即长度为100个字符的字符串
- 增加name，类型为VARCHAR(50)
- 自己设置value，登录成功后，手动设置到Cookie中，当传递过来时，拿值在数据库中查找，命名为token，类型为char(36)
- 数据库的信息有两个时间，创建时间和修改时间，用于去排查问题，gmt为格林尼标准时间
  - 增加gmt_create，类型为BIGINT，相当于Long
  - 增加gmt_modified，类型为BIGINT

{% asset_img H2创建.png This is an example image %}

对应到具体的代码为

```sql
create table user
(
	id INT auto_increment,
	account_id VARCHAR(100),
	name VARCHAR(50),
	token char(36),
	gmt_create BIGINT,
    gmt_modified BIGINT,
	constraint user_pk
		primary key (id)
);
```

## 集成MyBatis并实现插入操作

> 快速实现的思想：baidu搜索关键词然后在官网上看一手资料

 MyBatis 是支持普通 SQL查询，存储过程和高级映射的优秀持久层框架。找到[MyBatis官方资料](http://mybatis.org/spring-boot-starter/mybatis-spring-boot-autoconfigure/)，按照步骤操作，查阅[Spring文档](https://docs.spring.io/spring-boot/docs/2.0.0.RC1/reference/htmlsingle/#boot-features-embedded-database-support)。

- 添加依赖

- 需要DataSource，在Spring中定义连接池，在application.properties中添加配置文件

  - 添加spring-boot-starter-jdbc依赖

  - 在配置文件中添加

    ```xml
    spring.datasource.url=jdbc:h2:C:/Code/mycommunity
    spring.datasource.username=sa
    spring.datasource.password=123
    spring.datasource.driver-class-name=org.h2.Driver
    ```

    org.h2.Driver报错，查询后将h2的Maven依赖的test标签去掉即可。

- 提供Mapper

  新建包mapper，新建类UserMapper。

  - 写Mapper注解，添加insert接口，传入User类型的数据，alt+回车导入自己写的数据库model的User类

  - 新建类model，用于数据库传数据，新建类User，含有设置数据库的6种变量

  - 写insert注解，添加“”写SQL的查询语句

    ```sql
    INSERT INTO table_name ( field1, field2,...fieldN )
                           VALUES
                           ( value1, value2,...valueN );
    ```

    第一个参数括号中写要传入的参数，values后的括号要加#{}

    这样MyBatis可以将Object user中的名为#{}的值自动替换为object中对应值。

  ```java
  @Mapper
  public interface UserMapper {
      @Insert("insert into user (name,accout_id,token,gmt_create,gmt_modified) values (#{name},#{accountId},#{token},#{gmtCreate},#{gmtModified})")
      void insert(User user);
  }
  ```

- 在AuthorizeController中，当user不为空时，将其添加进数据库

  - 通过注解@Autowired将UserMapper注解进来

    ```java
    @Autowired
    private UserMapper userMapper;
    ```

  - 调用其userMapper的insert方法，新建User对象，用ctrl+alt+v快捷键

  - 设置user的属性

    - token采用UUID来做
    - name为githubUser的name
    - id将githubUser的ID转为String类型

```java
User user = new User();
            //set user values
            user.setToken(UUID.randomUUID().toString());
            user.setName(githubUser.getName());
            user.setAccountId(String.valueOf(githubUser.getId()));
            user.setGmtCreate(System.currentTimeMillis());
            user.setGmtModified(user.getGmtCreate());
            userMapper.insert(user);
```

 点击登录后，在数据库中存下了信息。

 当想测试，可以打断点，然后点击debug按钮。

### 报错

1. 无法找到数据库

   删除user table，重新添加

2. 找不到ACCOUT_ID Column

   在INSERT注解中语法写错，将account_id错写为accout_id，因此在H2数据库中找不到指定的column

## 获取登录状态

 以token为依据，绑定前端和后端的登录状态，将token抽取成变量，手动写key和value，手动将session和user进行识别。在数据库中查询是否在数据库中，存在则登录成功，不存在则登陆失败。此时不需要写入session，因为插入数据库的过程相当于写入session，在形参中注入**HttpServletResponse**，通过response写入cookie。

 不知道应该传输什么参数，使用ctrl+P。新建cookie，name为‘“token”，value为之前定义的token。

```java
response.addCookie(new Cookie("token",token));
```

 此时因为没有写入session，session为空，因此网页上还是显示的是登录。检查网工业源码，在application中找到了自己写入的token。

 需要访问网页时将key为token的cookie信息拿到，因此在indexController中，先注入userMapper(只有在userMapper中才可以访问user)，希望userMapper中有findByToken方法，传入token，然后返回model包中的user对象。在index函数中增加参数**HttpServletRequest**，利用其获得到Cookie，使用ctrl+alt+v，快速生成变量，将得到cookies进行for循环，快捷写法：cookies.for，然后自动变成for循环。如果cookies中含有token，获得其value，为所需要的token。然后userMapper中创建相应方法(alt+回车，快速修复)，获得到user对象。

> 设置cookie，使用response；获得cookie，使用request

```java
Cookie[] cookies = request.getCookies();
for (Cookie cookie : cookies) {
    if ("token".equals(cookie.getName())){
        String token = cookie.getValue();
        User user = userMapper.findByToken(token);
        break;
    }
}
```

 在UserMapper类中新建方法，注解为Select。查询数据库中是否包含token。

 MyBatis编译时，将形参中的值放入#{}对应的进行替换，如果是类可以直接进行替换，如果不是类，需要加入注解。

```sql
@Select("select * from user where token = #{token}")
User findByToken(@Param("token") String token);
```

 user相当于数据库和java类交互的模型，用user类插入数据，从数据库中查询数据返回User类。拿到user类后，需要与前端交互。

 如果user不为空，就将session写入user中。

```java
@Controller
public class IndexController {
    //默认要求依赖对象必须存在
    @Autowired(required = false)
    private UserMapper userMapper;
    @GetMapping("/")
    public String index(HttpServletRequest request){
        Cookie[] cookies = request.getCookies();
        for (Cookie cookie : cookies) {
            if ("token".equals(cookie.getName())){
                String token = cookie.getValue();
                User user = userMapper.findByToken(token);
                if (user != null){
                    request.getSession().setAttribute("user",user);
                }
                break;
            }
        }
        return "index";
    }
}
```

### 解决的问题

 如果服务器重启或者链接突然断开，再次连接需要重新登录，比较麻烦，因此将cookie持久化至数据库中，虽然cookie可以存储很多信息，但不应该将私密信息存储在cookie中，cookie中只存储令牌信息或token，从而前后端交互使用。

### 存在的问题

 当用户量很大，请求信息很多时，会比较慢。可以用redis等去改进。

## 集成Flyway Migration

 在获取到用户信息时，GithubUser中的bio没有用到，因此在数据库中添加项，右键user点击modified。但是这样一个人改动，其他人数据库都要重新添加数据库脚本，比较麻烦。

 使用数据库的迁移原因是：自动将不同人的数据库版本进行整合，自动管理数据库版本。

 进入[Flyway官网](https://flywaydb.org/getstarted/)，查看其Maven方式，将其plugin代码拷贝进pom.xml同级标签下，将其url地址更改为application.propertites中字段，user，password也做相应更改。保持数据库版本一致。

```xml
<url>jdbc:h2:C:/Code/mycommunity</url>
<user>sa</user>
<password>123</password>
```

 创建src/main/resources/db/migration，文件命名方式仿照如下

> src/main/resources/db/migration/V1__Create_person_table.sql

 新建文件V1__Create_user_table.sql，将之前的sql语句粘贴。

 先删除原数据库，然后运行命令

```
mvn flyway:migrate
```

 这样自动生成了新的数据库，而且生成flyway_shema_history文件，记录数据库执行过程，当有多个人执行数据库时，以文件中的记录为准；

 当数据库表格变更时，如要新增一个VARCHAR(256)类型的bio，**不直接在数据库中添加**，而是写好语句

```sql
alter table USER add bio VARCHAR(256);
```

> 可以先在IDEA的数据库中做对应操作，然后复制对应语句，这样不用自己去写语句，但是最后不能点执行，不然flyway操作会失败

 然后复制要变更的语句，复制在migration文件夹下生成新的文件V2__Add_bio_col_to_user_table.sql，重新运行flyway命令。这样子user数据库便新增了一个col为bio。

> 相当于对数据库的每条操作都分批次写在sql文件中，然后运行flyway对对应生成相应的数据库。

 无论表有多么细小的改动，都要重新创建一个对应的sql文件，不然会抛出异常，因为表对应的状态码改变了。降低数据库维护难度。

## Bootstrap编写问题发布界面

 仿照网站模板去做

{% asset_img 发起问题模板.jpg This is an example image %}

 需要有标题输入框，内容框，标签，确认发起和发起指南。拷贝一份index页面叫做publish。title改为发布-未名社区，上面一栏其他不用改变，在下面去添加内容。

 目测为3:1的左右分割布局，使用[SpringBoot栅格系统](https://v3.bootcss.com/css/#grid)中的流式布局

```html
<div class="container-fluid">
  <div class="row">
    ...
  </div>
</div>
```

 3:1在栅格系统（12份）下，为9:3，使用大屏幕尺寸，类前缀为.col-lg-，先敲下div.col-lg-9，然后按下**Tab键**，自动生成一个div。

 当屏幕变小的时候，从col-lg-9部分均占据全部页面，即其他尺寸比例变化，不同尺寸比例用**空格**隔开。

```html
<div class="col-lg-9 col-md-12 col-sm-12 col-xs-12" style="background-color: red;height: 300px";></div>
<div class="col-lg-3 col-md-12 col-sm-12 col-xs-12" style="background-color: green;height: 300px"></div>
```

 其中背景色，高度均为style下的属性，不同属性间用分号隔开。

 进入publish网页，需要写PublishController。

```java
@Controller
public class PublishController {
    @GetMapping("/publish")
    public String publish(){
        return "publish";
    }
}
```

 进入publish界面，可以看到在正常窗口下是9:3比例，当界面缩小后，变成了红色在上，绿色在下，均铺满整个窗口。

{% asset_img 大屏幕尺寸.jpg This is an example image %}

{% asset_img 其他屏幕尺寸.jpg This is an example image %}

页面中需要一个输入框，使用组件，有一个发起，找到好看的icon。在div中加入标签

```html
<span class="glyphicon glyphicon-search" aria-hidden="true"></span>
```

 将class中名称替换为所需要的，然后加入h2，文本为发起。span为h2块置元素，将其放在h2中。

 增加基本输入框，被form包裹，action为点击时所提交地址。placeholder为当没有输入时，默认显示文字。修改type为text，id和for为title，增加name为title。

 下面为问题补充，为文本输入框，因此在label下增加textarea标签，加入class。

 标签页需要输入框，再增加绿色的提交按钮。

```html
<button type="button" class="btn btn-success">（成功）Success</button>
```

 最后上面一部分代码为

```html
<form action="">
	<div class="form-group">
		<label for="title">问题标题（简单扼要）：</label>
        <input type="text" class="form-control" id="title" name="title" placeholder="问题标题...">
    </div>
    <div class="form-group">
         <label for="title">问题补充（必填：请参照右边提示）：</label>
         <textarea name="description" id="description" class="form-control" cols="30" rows="10"></textarea>
    </div>
    <div class="form-group">
         <label for="title">添加标签：</label>
         <input type="text" class="form-control" id="tag" name="tag" placeholder="输入标签，以，号分隔">
    </div>
    <button type="submit" class="btn btn-success">发布</button>
</form>
```

 然后为增加问题发布导航界面。

```html
<div class="col-lg-3 col-md-12 col-sm-12 col-xs-12">
	<h3>问题发起指南</h3>
    ● 问题标题：请用精简的语言描述您发布的问题，不超过25字
    ● 问题补充：详细描述您的问题内容，并确保问题描述清晰直观，并提供一些相关的资料
    ● 选择标签：选择一个或者多个合适的标签，用逗号隔开，每个标签不超过10个字
</div>
```

 得到效果为

{% asset_img 问题发布界面.png This is an example image %}

进一步修复样式，通过**检查**来做。在浏览器中编辑，实时预览，然后拷贝代码。

 发现这一部分的代码应该放在nav外层，使用ctrl+w同时选中上下层。将其放在nav下面。

 在static/css下面增加community.css文件

```css
.main{
    margin: 30px;
}
```

 将其拷贝至publish页面中。

```html
<link rel="stylesheet" href="css/community.css"/>
```

 在提问部分的div class中加上main

```html
<div class="container-fluid main">
```

 这样将导航和问题发布界面隔开。

{% asset_img margin30.jpg This is an example image %}

为了有突出效果，将最外层的body颜色改为灰色，将内容发布界面颜色改为白色。让发布按钮飘在右边。加入float

{% asset_img 发布按钮.jpg This is an example image %}

 修改css代码，增加body，修改颜色为灰色，修改main部分颜色为白色。增加btn-publish按钮。

```css
body{
    background-color: #efefef;
}
.main{
    background-color: white;
    margin: 30px;
}
.btn-publish{
    float: right;
    margin-bottom: 15px;
}
```

 在button的class后面进行追加

```html
<button type="submit" class="btn btn-success btn-publish">发布</button>
```

 这样样式基本达成效果。

 网页添加部分完整代码为

```html
<div class="container-fluid main">
    <div class="row">
        <div class="col-lg-9 col-md-12 col-sm-12 col-xs-12" ;>
            <h2><span class="glyphicon glyphicon-plus" aria-hidden="true"></span>发起</h2>
            <hr>
            <form action="">
                <div class="form-group">
                    <label for="title">问题标题（简单扼要）：</label>
                    <input type="text" class="form-control" id="title" name="title" placeholder="问题标题...">
                </div>
                <div class="form-group">
                    <label for="title">问题补充（必填：请参照右边提示）：</label>
                    <textarea name="description" id="description" class="form-control" cols="30" rows="10"></textarea>
                </div>
                <div class="form-group">
                    <label for="title">添加标签：</label>
                    <input type="text" class="form-control" id="tag" name="tag" placeholder="输入标签，以，号分隔">
                </div>
                <button type="submit" class="btn btn-success btn-publish">发布</button>
            </form>
        </div>
        <div class="col-lg-3 col-md-12 col-sm-12 col-xs-12">
            <h3>问题发起指南</h3>
            ● 问题标题：请用精简的语言描述您发布的问题，不超过25字
            ● 问题补充：详细描述您的问题内容，并确保问题描述清晰直观，并提供一些相关的资料
            ● 选择标签：选择一个或者多个合适的标签，用逗号隔开，每个标签不超过10个字
            <hr>
        </div>
    </div>
</div>
```

为了在发布界面点击未名社区可以跳转回主页，在a标签中增加href为根目录

~~~html
<a class="navbar-brand" href="/">未名社区</a>
~~~

## 发布文章

### 创建Table，存储发布者数据

数据库数据包括：id(primary key), title, descrption, gmt_create, gmt_modified, creator, comment_count, view_count, like_count, tag。基本思路为先利用IDEA添加所需要的信息，然后复制生成的表格代码如下

~~~sql
create table question
(
	id int auto_increment,
	title VARCHAR(50),
	description TEXT,
	gmt_create BIGINT,
	gmt_modified BIGINT,
	creator int,
	comment_count int default 0,
	view_count int default 0,
	like_count int default 0,
	tag VARCHAR(256),
	constraint question_pk
		primary key (id)
);
~~~

​	在Flyway中创建新脚本，命名为V3__Create_question_table.sql。将sql代码粘贴进此文件，运行命令

~~~
mvn flyway:migrate
~~~

### 写Mapper文件

在mapper包下新建类QuestionMapper，增加Mapper注解，将类更改为interface，写create函数，需要传入question类，然后增加Insert注解，写入相关sql语句，即 insert into (写表中需要插入名称) values (#{question类中相关变量名称})

~~~java
@Mapper
public interface QuestionMapper {
    @Insert("insert into question (title,description,gmt_create,gmt_modified,creator,tag) values (#{title},#{description},#{gmtCreate},#{gmtModified},#{creator},#{tag})")
    void create(Question question);
}
~~~

​	在model包中添加类Question，包含以下变量，并alt+insert创建getter和setter方法

~~~java
    private Integer id;
    private String title;
    private String description;
    private String tag;
    private Long gmtCreate;
    private Long gmtModified;
    private Integer creator;
    private Integer viewCount;
    private Integer commentCount;
    private Integer likeCount;
~~~

​	修改publish.html中回答问题部分，添加form后面内容，action路由到publish界面，提交post请求

~~~html
<form action="/publish" method="post">
~~~

​	get为渲染页面，post为执行请求。在PublishController中增加方法

​	仍然返回publish界面，前后端未分离带来。增加PostMapping注解，接收参数title,description,tag。利用Autowired持有questionmapper，并调用其create方法。ctrl+P查看其需要参数，需要question，新建一个question，ctrl+alt+v，将变量抽取出来。给question设置变量。shift+回车，换行。

​	为了获取到user，可以利用index中的利用cookie来获取。request利用HttpServletRequest注入，usermapper利用@Autowired自动注入。为了在服务端API接口级别传递到页面中，将要传递的写入Model。

如果获取到的user为空，则需要给model传入错误信息，并且回到publish界面。如果有错误提示，alt+回车。将信息填入后，回到首页。

~~~java
	@Autowired
    private QuestionMapper questionMapper;
    @Autowired
    private UserMapper userMapper;

    @PostMapping("/publish")
    public String doPublish(
            @RequestParam("title") String title,
            @RequestParam("description") String description,
            @RequestParam("tag") String tag,
            HttpServletRequest request,
            Model model){
        User user = null;
        Cookie[] cookies = request.getCookies();
        for (Cookie cookie : cookies) {
            if ("token".equals(cookie.getName())){
                String token = cookie.getValue();
                user = userMapper.findByToken(token);
                if (user != null){
                    request.getSession().setAttribute("user",user);
                }
                break;
            }
        }
        if (user == null){
            model.addAttribute("error","用户未登陆");
            return "publish";
        }
        Question question = new Question();
        question.setTitle(title);
        question.setDescription(description);
        question.setTag(tag);
        question.setCreator(user.getId());
        question.setGmtCreate(System.currentTimeMillis());
        question.setGmtModified(question.getGmtModified());

        questionMapper.create(question);
        return "redirect:/";
    }
~~~

​	为了提示错误信息，在publish.html界面，在提交问题的上方，增加一个span，修改class为bootstrap中的警告框，文本为错误信息。

​	继续包裹一个流式布局，希望其充满整个屏幕，将错误提示报告和提交按钮放至流式布局下。警告框最大化屏幕为9，当页面缩写后为12。button给其对应的class。

​	但是效果不是太好，警告框比提交框更宽。

~~~html
<div class="container-fluid main ">
	<div class="row">
	<div class="alert alert-danger col-lg-9 col-md-12 col-sm-12 col-xs-12" th:text="${error}"></div>
    <button type="submit" class="btn btn-success btn-publish col-lg-3 col-md-12 col-sm-12 col-xs-12">
    发布
    </button>
</div></div>
~~~

{% asset_img 警告框1.png This is an example image %}

为了只有在error不等于null的时候才展示，增加if标签。

~~~html
<div class="alert alert-danger col-lg-9 col-md-12 col-sm-12 col-xs-12" th:text="${error}"
 th:if="${error != null}"></div>
~~~

​	将button的流式布局重新放在一个div中，然后将button放置在此div下。

~~~html
<div class="col-lg-3 col-md-12 col-sm-12 col-xs-12">
	<button type="submit" class="btn btn-success btn-publish">
    发布
    </button>
</div>
~~~

为了让index与public联系起来，在首页上加上发布按钮，点击后跳转到发布界面。加入li标签，在li标签下加入a标签来指向超链接，也需要将这部分拷贝至publish的html文件中，这样改动一个页面，也要将另一个页面进行更改，比较麻烦。

~~~html
               <li th:if="${session.user != null}">
                    <a href="/publish">发布</a>
                </li>
~~~

当报错的时候，在publish界面上填的信息是没有的，因此在PublishController中，将这些信息注入到model中，这样可以在html页面获取。同时为了防止用户输入空信息提交，进行校验

~~~java
		//设置输入不能为空
        if (title == null || title == ""){
            model.addAttribute("error","标题不能为空");
            return "publish";
        }
        if (description == null || description == ""){
            model.addAttribute("error","描述不能为空");
            return "publish";
        }
        if (tag == null || tag == ""){
            model.addAttribute("error","标签不能为空");
            return "publish";
        }
        model.addAttribute("title",title);
        model.addAttribute("description",description);
        model.addAttribute("tag",tag);
~~~

在publish.html进行修改，在title，description，tag标签中添加用户输入的显示如下

~~~html
<div class="form-group">
     <label for="title">问题标题（简单扼要）：</label>
     <input type="text" class="form-control" th:text="${title}" id="title" name="title" placeholder="问题标题...">
</div>
<div class="form-group">
     <label for="description">问题补充（必填：请参照右边提示）：</label>
     <textarea name="description" th:text="${description}" id="description" class="form-control" cols="30" rows="10"></textarea>
</div>
<div class="form-group">
      <label for="tag">添加标签：</label>
      <input type="text" class="form-control" th:text="${tag}" id="tag" name="tag" placeholder="输入标签，以，号分隔">
</div>
~~~

在调试的时候，发现当输入标题，但不输入问题补充的时候，error会提示，但是标题部分没有回写下来，原因是将model.attribute放到了校验下方，因此不能显示出来。将model注入调整到最上面，这时候发现标题的显示位置不在value中

{% asset_img 标题错位.png This is an example image %}

去搜索：thymeleaf input，发现不能通过使用text获得值，要使用value，而描述使用value无法获取，需要使用text。

~~~html
<input type="text" class="form-control" th:value="${title}" id="title" name="title" 		placeholder="问题标题...">
<textarea name="description" th:text="${description}" id="description" class="form-			control" cols="30" rows="10"></textarea>
<input type="text" class="form-control" th:value="${tag}" id="tag" name="tag" 				placeholder="输入标签，以，号分隔">
~~~

发布文章总结：

1、在html中form表单中填action，即为提交请求的地址，post路由到/publish中，当点击submit，会寻找地址为publish，请求为post的接口，注入到doPublish方法中

2、接收标题，描述，标签信息，放入model中以便于页面的回显

3、验证接收信息是否不为空，如果为空，注入到error中

4、验证是否登录，通过token拿到数据库中存的user信息，若user信息存在则绑定到session中；若不存在，将错误信息注入，返回到前端中

5、全部工作后，构建question对象，创建questionMapper，插入至数据库

因此接下来需要将发布的问题显示在首页中。

### 添加Lombok支持

分析理想的社区界面，发现发布界面中有头像的显示，而之前创建的用户信息数据库中是没有头像的，因此需要创建数据库脚本去添加这一栏。添加信息为图片的url，类型为varchar(100)，sql语言如下

~~~sql
alter table USER add avatar_url varchar(100);
~~~

新建文件V4__Add_avatar_url_to_user_table.sql，将上述sql语句复制，然后执行`mvn flyway:migrate。

在model.User类中添加图像地址信息。

~~~java
    private String avatarUrl;
~~~

然后再添加其getter和setter方法，但这样非常麻烦。使用[Lombok](https://projectlombok.org/)。添加其maven依赖即可。

~~~xml
	<dependency>
		<groupId>org.projectlombok</groupId>
		<artifactId>lombok</artifactId>
		<version>1.18.12</version>
		<scope>provided</scope>
	</dependency>
~~~

> Lombok是一个可以通过简单的注解形式来帮助我们简化消除一些必须有但显得很臃肿的Java代码的工具，通过使用对应的注解，可以在编译源码的时候生成对应的方法。

查阅其文档可知，其`@Data`注解可以自动生成getter和setter方法。然后`ctrl+shift+n`回到User类中，删掉其getter和setter方法，然后在类名上方加入`@Data`注解。同样将Question类、AccessTokenDTO类、GithubUser类中的getter与setter方法删掉。

> 当只有几个类的时候并没有必要使用，但是因为类很多，创建频繁，才需要此功能
>
> 设计思想：必要时才用

这时候，因为User信息被改动了，因此在UserMapper中添加相应的信息如下，UserMapper作用是将User信息插入到数据库中。

~~~sql
 @Insert("insert into user (name,account_id,token,gmt_create,gmt_modified,avatar_url) values (#{name},#{accountId},#{token},#{gmtCreate},#{gmtModified},#{avatarUrl})")
    void insert(User user);
~~~

那么如何获取头像的url值呢？

之前获取到user信息是从Github的API获取 ，因此仍然从API网页获取`https://api.github.com/users/用户名`

可以从页面中看到，其中有avatar_url这一栏，给出了头像的url信息。

此时需要在GithubUser中添加avatar_url信息，这样现在的GithubUser属性如下

~~~java
    private String name;
    private long id;
    private String bio;
    private String avatar_url;
~~~

而githunUser获取到了头像的url后，需要在`AuthorizeController`类中将值传给User类

~~~java
user.setAvatarUrl(githubUser.getAvatar_url());
~~~

遇到的问题：写了@Data注解，但是在`AuthorizeController`类中无法获取到GithubUser和User类中的getter和setter方法，后查询是因为IDEA没有下载LomBok插件，解决方法是在Plugins插件设置中设置HTTP属性，在cmd中利用`ipconfig`来获取本机的IP地址填入，然后重启IDEA即可解决。

为了验证，将cookies全部删掉，但在IndexController和PublishController中对cookies的处理没有加空值判断，现在加上，这样就避免了当cookies为空的时候报空指针异常。

~~~java
        if (cookies != null && cookies.length != 0)
            for (Cookie cookie : cookies) {
                if ("token".equals(cookie.getName())) {
                    String token = cookie.getValue();
                    User user = userMapper.findByToken(token);
                    if (user != null) {
                        request.getSession().setAttribute("user", user);
                    }
                    break;
                }
            }
~~~

这样便可以获取用户的头像url了。

### 完成首页问题列表

首页的问题列表界面与发布问题的界面类似，因此可以直接拷贝publish界面下半部分至index页面。

{% asset_img 主页问题展示.png This is an example image %}

其中右边部分为热门话题，进行文字修改。

对于左边部分，在bootstrap官网找对应的icon。在html中将对应的class修改为网站所给，左边部分的tag先不管，而每个人的发布的框框与媒体对象比较类似，如下图所示

{% asset_img 媒体对象.png This is an example image %}

其对应的代码为

~~~html
<div class="media">
  <div class="media-left">
    <a href="#">
      <img class="media-object" src="..." alt="...">
    </a>
  </div>
  <div class="media-body">
    <h4 class="media-heading">Media heading</h4>
    ...
  </div>
</div>
~~~

将form表单部分替换掉。将src部分用之前获取到的自己的头像url去实验以此来快速看效果。这时候显示效果如下

{% asset_img 大头像.png This is an example image %}

虽然样式没有问题，但头像的大小需要设置，调试的最快的方式是对着页面要看的元素点击检查，然后在element.style中敲入想要的大小，注意需要加上单位，如果直接写width:40会出错，因为没有单位，要加上px，这样就可以看到头像变小了。也可以点击40px，然后用鼠标滑轮去控制。

{% asset_img 小头像.png This is an example image %}

如果不想要头像变成方的显示，可以在bootstrap中寻找，在全局CSS样式-图片中有3种显示方式，主要是设置其class属性，追加到图片url的class处

~~~html
<img class="media-object img-rounded" src="图片地址">
~~~

同时要对头像的大小进行更改，在community.css中，增加media-object的样式

~~~css
.media-object{
    width: 40px;
    height: 40px;
}
~~~

但是对网页样式并没有影响。原因是没有添加css引用。

~~~html
    <link rel="stylesheet" href="../static/css/community.css">
~~~

然后添加回复数，浏览和时间。文字直接复制，样式可以通过网页获取。将文字的样式进行拷贝至community.css样式中

~~~css
.text-desc {
    font-size: 12px;
    font-weight: normal;
    color: #999;
}
~~~

在index页面中将文字放入span，class为text-desc

~~~html
<span class="text-desc">3 个评论 • 294 次浏览 • 2 天前</span>
~~~

在页面上可以显示后，需要在服务端获取信息显示在网页上。存在edge浏览器显示正常，chrome浏览器CSS样式 显示失效的问题。

接下来解决从服务器获取数据的问题，在indexController返回index页面前获取信息，可以利用model.addAttribute将信息写入到前端中。需要注入questionMapper，并在QuestionMapper中list方法，用于显示。并注入Model，用于向前端注入信息。

~~~java
    @Autowired(required = false)
    private QuestionMapper questionMapper;

	//IndexContrller前面部分省略
	List<Question> questionList = questionMapper.list();
    return "index";
~~~

QuestionMapper中list方法用到查询语句。

但是问题是questionMapper中没有需要的头像信息，在question表中有creator信息，与user表关联，这样才能拿到头像信息。为了在两个表之间传输信息，在dto传输层再新建一个类，与Question类其他一样，只是多了一个User对象，因此在返回的时候，要返回QuestionDTO，但是问题是questionMapper是针对question类的，而不是user，因此不能返回user相关的信息，需要再提炼出一层service模型。

在service包下新建QuestionService，并增加`@Service`注解，这时候可以组装使用questionMapper和UserMapper，起组装作用，这个中间层习惯性叫为Service。因此在IndexController中将持有的questionMapper替换为questionService。在questionService中新建list方法，需要依赖questionMapper和userMapper，先利用questionMapper获取List，然后循环List，在每个question中通过userMapper，新建一个方法findById，利用question的creator信息去获取user。

~~~java
    //userMapper中的findById方法
    @Select("select * from user where id = #{id}")
    User findById(@Param("id") Integer id);
~~~

然后将question转为questionDTO，使用Spring的工具类快速将question中需要的信息拷贝至questionDTO中（通过反射进行复制）。

# Spring知识总结

## 注解

1. Controller

   > @Controller，将当前类作为路由API的一个承载者

2. GetMapping

   > @GetMapping，确保对其中内容的HTTP GET请求映射到greeting方法

3. Component

   > @Component 仅仅把当前类初始化Spring容器的上下文，这样调用时不用实例化对象，IOC，便于去调用对象

4. Autowired

   > @Autowired注解将Spring容器中的写好的实例化的实例加载到当前使用的上下文

5. Value

   > @Value(“${string}”)，在配置文件中读取key为string的value

6. Data

   > @Data LomLok注解，自动生成类中的getter和setter方法

7. Service

   > 

## 基础

- 在类和类之间传输用DTO，在数据库中使用model

# 快捷键技巧

## IDEA快捷键

- ctrl + P：提示输入参数类型
- ctrl + shift+n 快速**查找**文件
- shift + F6 更改名称
- ctrl + shift + F12 切换最大屏
- alt + 鼠标左键按住拖动，实现对多行的批量修改
- alt + insert 提示创建get和set
- 选中指定部分，alt + 回车，引入jar包
- **ctrl + alt + v** 选中new Class，快速创建其变量
- 按住shift + 回车 自动换到当前光标的下一行
- ctrl + e 切回最近的一个访问窗口
- 右键Git/history，可以看到历史提交信息时当前目录的情况
- shift+F6 将所有同名变量更名
- **变量.for** 完成对某变量的for循环
- alt+回车 修复
- 右键Git revert，将某个文件还原至Git提交状态
- ctrl+w同时选中上下层
- ctrl+alt+l 简单格式化
- iter 快速生成foreach循环

## 其他快捷键

- ctrl+shift+n 打开新的匿名窗口

学到知识

1. Maven是管理包和包的依赖的工具，pom.xml中包括所有运行Spring项目需要的包，主要为依赖parent
2. gitignore用于只提交部分的代码，避免一些文件冲突
3. 学习一个新东西先进官网去学习，先拷贝进自己项目跑起来
4. 当编程参数超过2个，不要使用形参传入，而是封装成对象



# 资源支持

- [Spring官方文档](https://spring.io/guides)
- [Serving Web Content with Spring MVC](https://spring.io/guides/gs/serving-web-content/)
- [elastic社区](https://elasticsearch.cn/)
- [BootStrap](https://v3.bootcss.com/getting-started/#download)
- [OAuth Apps文档](https://developer.github.com/apps/)
- [Visual Paradig](https://www.visual-paradigm.com)
- [OkHttp](https://square.github.io/okhttp/)
- [Maven包查询](https://mvnrepository.com/)
- [MyBatis官方资料](http://mybatis.org/spring-boot-starter/mybatis-spring-boot-autoconfigure/)
- [Spring文档](https://docs.spring.io/spring-boot/docs/2.0.0.RC1/reference/htmlsingle/#boot-features-embedded-database-support)
- [MySQL菜鸟教程资料](https://www.runoob.com/mysql/mysql-tutorial.html)
- [Flyway官网](https://flywaydb.org/getstarted/)