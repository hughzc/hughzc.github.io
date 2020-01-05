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

   > @Controller，将当前类作为路由API的一个承载者

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

​	这时候点击登录并授权后，网站会返回code信息，后面将code信息提取出来

#### 获取code

1. 写新的Controller

   返回String类型的callback()，返回index主页，在calback中寻找，写GetMapping注解，需要传入参数，为用户写入的String类型的code和state。

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

​	新建一个包provider，提供对第三方信息的支持能力，新建类GithubProvider

> 体现思想：不同业务间进行隔离

​	写注解@Component

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

### 配置文件的分离application.proerties

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

### Session和Cookies原理和实现

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

## Spring知识总结

### 注解

1. Controller

   > @Controller，将当前类作为路由API的一个承载者

2. Component

   > @Component 仅仅把当前类初始化Spring容器的上下文，这样调用时不用实例化对象，IOC，便于去调用对象

3. Autowired

   > @Autowired注解将Spring容器中的写好的实例化的实例加载到当前使用的上下文

4. Value

   > @Value(“${string}”)，在配置文件中读取key为string的value

# 快捷键技巧

### IDEA快捷键

- ctrl + P：提示输入参数类型
- ctrl + shift+n 快速**查找**文件
- shift + F6  更改名称
- ctrl + shift + F12 切换最大屏
- alt + 鼠标左键按住拖动，实现对多行的批量修改
- alt + insert 提示创建get和set
- 选中指定部分，alt + 回车，引入jar包
- ctrl + alt + v 选中new Class，快速创建其变量
- 按住shift + 回车 自动换到当前光标的下一行
- ctrl + e 切回最近的一个访问窗口
- 右键Git/history，可以看到历史提交信息时当前目录的情况

### 其他快捷键

- ctrl+shift+n 打开新的匿名窗口

学到知识

1. Maven是管理包和包的依赖的工具，pom.xml中包括所有运行Spring项目需要的包，主要为依赖parent
2. gitignore用于只提交部分的代码，避免一些文件冲突
3. 学习一个新东西先进官网去学习，先拷贝进自己项目跑起来
4. 当编程参数超过2个，不要使用形参传入，而是封装成对象

