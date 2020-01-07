---
title: Java爬虫
date: 2020-01-07 15:26:50
tags: 爬虫
categories: Java
---

## 实现场景

​	通过 selenium 实现模拟人为操作自动化依据，工作经验，学历要求，公司规模，行业领域抓取拉钩薪资范围。只针对网页，不受防爬的影响。

## 环境准备

- [Chrome WebDriver](http://npm.taobao.org/mirrors/chromedriver)

- Selenium

  ~~~html
  <dependency>
      <groupId>org.seleniumhq.selenium</groupId>
      <artifactId>selenium-java</artifactId>
      <version>3.141.59</version>
  </dependency>
  ~~~

## 实战

### 项目创建

选择Maven，在pom文件中写<dependencies>/<dependency>，主要逻辑是通过java代码操作selenium的jar包，然后操作Webdiver，然后操作浏览器。

将WebDriver拷贝到src/main/resources下。在src/main/java下新建类LagouSearcher。

- 让系统知道WebDriver的位置
- 创建WebDriver
- 通过WebDriver访问浏览器

~~~java
        //告诉系统webdriver的位置
        //通过反射机制，拿到类中的resources下面的文件路径 
  System.setProperty("webdriver.chrome.driver",LagouSearcher.class.getClassLoader().getResource("chromedriver.exe").getPath());
        WebDriver webDriver = new ChromeDriver();
        //通过webdriver访问指定页面
        webDriver.get("https://www.lagou.com/zhaopin/Java/?labelWords=label");
~~~

### 选择搜索条件

WebDriver通过操纵元素来选择其具体位置。

​	许多都为li标签，只能通过li为multi-chosen，span下class不同名称来判断

{% asset_img li.jpg This is an example image %}

​	用到[XPath](https://www.w3school.com.cn/xpath/xpath_syntax.asp)语法

| 表达式   | 描述                                                       |
| :------- | :--------------------------------------------------------- |
| nodename | 选取此节点的所有子节点。                                   |
| /        | 从根节点选取。                                             |
| //       | 从匹配选择的当前节点选择文档中的节点，而不考虑它们的位置。 |
| .        | 选取当前节点。                                             |
| ..       | 选取当前节点的父节点。                                     |
| @        | 选取属性。                                                 |

具体实例为

| 路径表达式      | 结果                                                         |
| :-------------- | :----------------------------------------------------------- |
| bookstore       | 选取 bookstore 元素的所有子节点。                            |
| /bookstore      | 选取根元素 bookstore。 注释：假如路径起始于正斜杠( / )，则此路径始终代表到某元素的绝对路径！ |
| bookstore/book  | 选取属于 bookstore 的子元素的所有 book 元素。                |
| //book          | 选取所有 book 子元素，而不管它们在文档中的位置。             |
| bookstore//book | 选择属于 bookstore 元素的后代的所有 book 元素，而不管它们位于 bookstore 之下的什么位置。 |
| //@lang         | 选取名为 lang 的所有属性。                                   |