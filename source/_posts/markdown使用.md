---

title: markdown使用
date: 2019-12-29 11:04:12
tags: markdown

---

# markdown在hexo中使用
## markdown工具

### 个人使用工具：**Typora**

1. 基于Windows下载
2. 偏好设置 

## 注意事项

### 部署步骤

​	博客部署至远端

~~~
hexo deploy -g
~~~

​	将hexo文件夹源代码（写作所在文件夹）push至github仓库

~~~
git add .
git commit -m “message”
git push
~~~

## 添加图片

引用自博客

> https://blog.csdn.net/weixin_30734435/article/details/98497054

#### 更改配置文件

​	在根目录下配置文件`_config.yml` 中有 `post_asset_folder:false改为true`。这样在建立文件时，`Hexo`会自动建立一个与文章同名的文件夹，这样就可以把与该文章相关的所有资源（图片）都放到那个文件夹里方便后面引用。

#### 安装git插件

> npm install https://github.com/7ym0n/hexo-asset-image --save

#### 在插入图片处增加代码

[官方文档](https://hexo.io/zh-cn/docs/asset-folders)给出了解决方式，使用代码为

~~~
{% asset_img example.jpg This is an example image %}
~~~

​	将example.jpg修改为所需要的文件名称和后缀即可，其中**This is an example image**是图片描述。

#### 显示效果

{% asset_img 传图片方法.png This is an example image %}

### 标题前不加数字

​	在标题前面加数字，hexo使用Next模板后，会自动根据多级标题加上相应数字生成目录，如果人为在多级标题前面加入数字，体验感并不好。

# markdown语法记录

引用自知乎，将一些常用命令进行记录

> https://zhuanlan.zhihu.com/p/90561228

## 字体编辑

### 标题

​	有6级标题可选，分别为按下Ctrl+1~6。

### 字体大小

```
快捷键：Ctrl+数字  或 Ctrl+加减号  或  ### （几个#表示几级标题，同上）
```

### 字体加粗

```
快捷键：Ctrl+b
示 例：**加粗内容**
```

​	加粗前：字体；加粗后**字体**

### 斜体

```
快捷键：ctrl+i
示 例：*斜体*
```

​	斜体前：斜体；斜体后*斜体*

### 删除线

```
快捷键：alt+shift+5
示 例：~~删除的内容~~
```

​	删除前：删除；删除后：~~删除~~

### 下划线

```
快捷键：Ctrl+u
示 例：<u>下划线内容</u>
```

​	下划前：下划；下划后：<u>下划</u>

### 脚注

```
操作：这块有个脚注[^脚注]
     [^脚注]:填写脚注的内容
示例：有一个github网址[^1]
     [^1]:https://github.com/
```

​	无脚注；有脚注[^1]

[^1]: https://github.com/

## 列表

### 有序列表

```
操作：数字+英文小数点(.)+空格
示例：1. list1
     2. list2
```

### 无序列表

```
操作：- +空格 或 * + 空格
示例： - list1
      - list2
```

## 插入

### 插入代码块

快捷键：shift+3个~

### 插入数学公式
~~~
操作：$$ + enter
示例：$$ + enter后输入11+12，结果如下所示
~~~

$$
11+12
$$

### 插入引用

~~~
操作：> + 空格
示例：> + 空格后，输入 引用的内容，结果如下所示
~~~

> 这是一个引用

### 插入链接

~~~
操 作:Ctrl+k弹出后，输入 [输入标题名](输入链接地址) 即可
示 例1：[百度一下，你就知道](https://www.baidu.com/)
示 例2：这是 [百度一下，你就知道](https://www.baidu.com/ "百度") 的链接.  
示 例3：这是 [github][1] 的链接.  
       [1]: https://github.com/ "github"
ps：按住ctrl点击链接可直接打开
~~~

[百度一下，你就知道](https://www.baidu.com/)

这是[百度一下，你就知道](https://www.baidu.com/ "百度") 的链接

### 插入注释

~~~
操作：[^文字]：文字
示例：[^1]：文献1
~~~

​	[^2] : 个人博客地址

[^2]: hughzc.github.io

### 插入表格

~~~
快捷键：ctrl+t
示 例：按完快捷键后，弹出下图，选择对应的行和列，点击确定即可。
~~~
弹出下图的选项



{% asset_img 表格.png This is an example image %}



| 1    |      |      |
| ---- | ---- | ---- |
| 2    |      |      |
| 3    |      |      |
| 4    |      |      |

### 普通markdown插入图片

~~~
操作：直接拖动  或 ctrl+shift+i(相对路径地址)
示例：![](C:\1.jpg)
~~~

​	如果是在hexo中插入图片，需要使用代码

~~~
{% asset_img example.jpg This is an example image %}
~~~

### 插入分隔符

~~~
操作：--- + enter  或者 *** + enter
~~~

这是条分割线

---

### 插入目录

~~~
操作：[toc]+enter
~~~







