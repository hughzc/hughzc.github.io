---
title: Linux使用
date: 2020-01-14 09:41:25
tags: Linux
categories: Linux
---

## Linux下安装jdk8

### 卸载Linux自带OpenJDK

切换到root权限

> su root

 使用java -version看OpenJDK信息，然后查询系统自带java相关文件

```
rpm -qa | grep java
```

命令说明：

rpm 　管理套件

-qa 　使用询问模式，查询所有套件

grep　　查找文件里符合条件的字符串

java 　查找包含java字符串的文件

 除noarch外，删除其他java文件

> rpm -e –nodeps java-1.7.0-openjdk-1.7.0.111-2.6.7.8.el7.x86_64

命令介绍

rpm 　　　管理套件

-e　　　　　删除指定的套件

–nodeps　　不验证套件档的相互关联性

 当输入java -version，没有找到命令，代表删除成功。

---

### 下载JDK

历史版本下载地址：　　http://www.oracle.com/technetwork/java/javase/archive-139210.html

 通过浏览器下载会默认下载到当前登陆用户的下载目录，下载位置为“当前用户/下载/jdk-8u211-linux-x64.tar.gz”。

 cd 下载，然后使用

> ls -al 展示本目录下所有文件

 为了查看当前目录路径，可以使用

> pwd 查看当前目录路径

---

### 复制JDK

#### 备份JDK

 将压缩包复制一份到/usr/local/src/作备份，输入命令

> cp jdk-8u211-linux-x64.tar.gz /usr/local/src

命令说明：

cp　　　　　　　　　　　　　　 复制文件或目录

jdk-8u211-linux-x64.tar.gz　　　　文件名

/user/local/src　　　　　　　　　 要复制的目标目录

#### 创建java文件夹

 在usr目录下创建一个文件夹，名为java

> mkdir java

 然后将文件拷贝至/usr/java

> cp jdk-8u211-linux-x64.tar.gz /usr/java

---

### 解压缩JDK

#### 在java目录下解压JDK压缩文件

```
tar -zxvf jdk-8u144-linux-x64.tar.gz
```

命令介绍：

tar　　　　　　备份文件

-zxvf　　　　　

-z　　　　　　 　　　　　　　 通过gzip指令处理备份文件

-x　　　　　　　　　　　　　　 从备份文件中还原文件

-v　　　　　　　　　　　　　　 显示指令执行过程

-f　　　　　　 　　　　　　　　 指定备份文件

jdk-8u211-linux-x64.tar.gz　　　　文件名

#### 删除JDK压缩包

```
rm -f jdk-8u211-linux-x64.tar.gz
```

命令解释：

rm　　　　删除文件或目录

-f　　　　 强制删除文件或目录

---

### 配置JDK环境变量

#### 编辑全局变量

命令行键入：

```
vim /etc/profile
```

命令说明：

vim　　　　　　文本编辑

/etc/profile　　　全局变量文件

进入文本编辑状态下，光标走到文件最后一行，键盘按下：

```
i
```

 进入插入状态：

在文本的最后一行粘贴如下：

```
i
```

注意JAVA_HOME=/usr/java/jdk1.8.0_211 为你自己的目录

```
#java environment
export JAVA_HOME=/usr/java/jdk1.8.0_211
export CLASSPATH=.:${JAVA_HOME}/jre/lib/rt.jar:${JAVA_HOME}/lib/dt.jar:${JAVA_HOME}/lib/tools.jar
export PATH=$PATH:${JAVA_HOME}/bin
```

【注】：CentOS6上面的是JAVAHOME，CentOS7是{JAVA_HOME}

 复制完成后，在键盘敲下

```
ESC
shift+q
```

 进入EX模式，敲下

```
x
```

 保存并退出。

---

### 检验环境变量是否生效

#### 让刚刚设置的环境变量生效

键入：

```
source /etc/profile
```

source /etc/profile或 . /etc/profile

#### 检查是否配置成功

键入：

```
java -version
```

## Linux命令

### 目录

- pwd 查看当前目录路径
- ls -al 展示本目录下所有文件

### 文件夹

- mkdir dir 创建文件夹

### 文件

- cp 复制文件

cp　　　　　　　　　　　　　　 复制文件或目录

jdk-8u211-linux-x64.tar.gz　　　　完整文件名

/user/local/src　　　　　　　　　 要复制的目标目录

- rm 删除文件

rm　　　　删除文件或目录

-f　　　　 强制删除文件或目录

- tar 备份，解压缩文件

tar　　　　　　备份文件

-zxvf　　　　　

-z　　　　　　 　　　　　　　 通过gzip指令处理备份文件

-x　　　　　　　　　　　　　　 从备份文件中还原文件

-v　　　　　　　　　　　　　　 显示指令执行过程

-f　　　　　　 　　　　　　　　 指定备份文件

jdk-8u211-linux-x64.tar.gz　　　　文件名

### 全局变量

#### 编辑全局变量

vim /etc/profile

i 插入