# Linux基础

## Linux介绍

Linux操作系统，免费，开源，安全，高效，稳定，处理高并发性能好，企业级开发用的多。

<!-- more -->

## Linux目录结构

Linux文件系统采用级层式的树状目录结构，在此结构中的最上层是根目录“/”，在此目录下创建其他目录。

Linux世界里，一切皆文件。

{% asset_img 目录结构.png This is an example image %}

- /bin 存放常用指令
- /home 存放普通用户的主目录，每个用户有个自己的目录，一般以用户账号命名
- /root 系统管理用用户主目录
- /etc 所有系统管理需要的**配置文件**和子目录
- /usr 用户的很多应用程序和文件都在此目录下
  - /usr/local 给另一个主机额外安装软件所安装的目录
- /boot 存放的启动linux使用时的核心文件
- /dev 类似设备管理器，把所有硬件用文件形式存储
- /media 识别的设备挂载目录
- /mnt 让用户**临时挂载别的文件系统**，将外部存储挂载在/mnt/上
- /lib 系统开机需要的动态链接共享库，类似于DLL文件
- /opt 给主机**额外安装软件**所摆放的目录
- /tmp 存放临时文件
- /var 不断扩充的东西，习惯将经常被修改的目录放在此目录下，包括各种日志文件
- /proc 虚拟目录，系统内存映射，通过直接访问此目录获取系统信息

总结：

1. linux目录中有且只有一个根目录
2. linux各个目录存放内容是规划好的，不应乱放文件
3. linux以文件形式管理设备，因此linux系统一切皆文件

## Linux远程登录

使用XShell6远程登录到Linux。使用XFtp6，远程上传下载文件。需要linux开启sshd服务，该服务监听22端口。

XShell，选择linux的ip地址，输入账户名与密码即可。

XFTP，用于在windows与linux下传输文件。

## vi与vim

vi为文本编辑器，vim可以编辑程序，vi的增强版。

{% asset_img vim切换.png This is an example image %}

### 常用模式

#### 正常模式

默认的，使用上下左右移动光标，使用删除字符和删除整行处理档案内容，也可以使用复制贴上。可以使用快捷键。

#### 插入模式

该模式下，可以输入内容。一般按i进入插入模式。

#### 命令行模式

在插入模式下按ESC切换到正常模式，:切换到编辑模式，提供相关指令，完成读取、存盘、替换、离开vim、显示行号等动作在此模式中达成。

:wq，写并退出；:q，退出不保存；:q!，强制退出不保存

### 快捷键使用

1、拷贝当前行 yy，拷贝当前行向下的5行 5yy，并粘贴 p

2、删除当前行 dd，删除当前行向下5行 5dd

3、文件下查询某个单词【命令行下 /关键字，回车查找，输入n就是查找下一个】

4、设置文件行号，取消文件的行号【命令行下 :set nu 和 :set nonu】set number

5、编辑/etc/profile文件，使用快捷键到文档最末行【G】和最首行【gg】，都是在正常模式下进行

6、撤销 u，输入hello再撤销，在**正常模式下**输入u

7、编辑 /etc/profile文件，并将光标移动到第20行 shift+g

- 显示行号 :set nu
- 输入20
- 输入shift+g

## 关机与重启

- shutdown

  - shutdown -h now 立即关闭
  - shutdown -h 1 1分钟后关机
  - shutdown -r now 立即重启

- halt

  直接使用，效果是关机

- reboot

  现在重启

- sync

  把内存数据同步到磁盘

不管是重启还是关闭系统，首先运行sync命令，把内存数据写到磁盘中

## 登录与注销

登录少用root，避免失误。可使用普通用户登录，使用su -用户名来切换管理员身份。

在提示符下输入logout可注销用户。logout在图形

## 用户管理

Linux多用户多任务系统，使用系统资源的用户，需要向系统管理员申请一个账号。Linux用户至少属于一个组。

家目录/home，用户登录时进入到对应的家目录。

### 添加用户

useradd 【选项】用户名

useradd -d 指定目录 新的用户名 给新创建的用户指定家目录

### 指定密码

passwd 用户名 指定密码

pwd 查看当前目录

### 删除用户

userdel 用户名

删除用户但保留家目录 userdel 用户名

删除用户但不保留家目录 userdel -r 用户名

一般保留家目录

### 查询用户信息

id 用户名

看用户id号，所在组的id，组名

whoami 看当前用户名称

### 切换用户

su - 用户名

高权限到低权限用户不用输入密码，反之需要

当要回到原来用户，使用exit指令

### 用户组

类似于角色，系统可以对有共性的多个用户进行统一的管理

新增组

groupadd 组名

删除组

groupdel 组名

增加用户时指定组

useradd -g 用户组 用户名

修改用户组

usermod -g 用户组 用户名

### 用户和组相关文件

- 用户配置文件（用户信息）

  /etc/passwd

  每行含义

  用户名：口令：用户标识号：组标识号：注释性描述：主目录：登录shell

{% asset_img 用户信息.png This is an example image %}

- 组配置文件（组信息）

  etc/group

- 口令配置信息（密码和登录信息，加密）

  /etc/shadow

  组名：口令：组标识号：组内用户列表

{% asset_img 组信息.png This is an example image %}

## 实用指令

### 指令运行级别

0：关机

1：单用户（找回丢失密码）

2：多用户无网络服务【少】

3：**多用户网络服务**【多】

4：保留

5：图形界面【多】

6：重启

系统运行级别配置文件 /etc/inittab

切换到指令运行级别

语法 init [012356]

#### 找回root密码

如何找回丢失的root密码

思路：开机后在启动前输入回车，界面输入e，新界面选择kernel，输入e，在界面输入 1，再次输入b，进入到单用户模式，使用passwd root，修改root密码。进入单用户模式，root不需要密码就可以登录。

不能远程使用此方式。

### 帮助指令

当对某个指令不熟悉的时候，可以使用linux提供的帮助指令来了解指令使用方法

#### man 获得基本帮助信息

基本语法

man 【命令或配置文件】

#### help指令

基本语法

help 命令（功能描述，获得shell内置命令的帮助信息）

### 文件目录类

#### pwd指令

pwd 显示当前工作目录的绝对路径

#### ls指令

ls 【选项】【目录或文件】

常用选项

- -a：显示当前目录所有的文件和目录，包括隐藏的
- -l：以列表的方式显示信息
- -h：以人更方便的方式查看

#### cd指令

cd 【参数】切换到指定目录

常用参数

绝对路径或相对路径

若当前在root目录下，为了到home

- /home 绝对路径
- ../home 相对路径，从当前工作目录开始定位到需要的目录去

cd ~ 或者cd：回到自己的家目录

cd ..回到上一级目录

#### mkdir指令

mkdir用于创建目录

- 基本语法

  mkdir 【选项】要创建的目录

- 常用选项

  -p：创建多级目录

mkdir /home/dog，在home下创建dog目录

创建多级目录的时候，需要加上-p

mkdir -p /home/animal/tiger

#### rmdir指令

rmdir删除空目录

- 基于语法

  rmdir 【选项】要删除的空目录

- 细节

  rmdir删除的是空目录，若**目录下有内容无法删除**

  删除非空目录，使用**rm -rf** 要删除的目录

#### touch指令

touch指令创建空文件

- 基本语法

  touch 文件名称【文件名称】

  可以一次性创建多个文件，文件名用空格隔开

#### cp指令【重要】

cp指令拷贝文件到指令目录

- 基本语法

  cp 【选项】source dest将源拷贝至dest

- 常用选项

  -r ：**递归复制**整个文件夹

  cp aaa.txt bb/ 将当前目录下的aaa.txt拷贝至当前目录的bb目录下

  cp -r test bb/ test中有多个文件，将test整个目录拷贝至bb目录下

  \cp -r test bb/  强制覆盖 \cp

- 注意

  注意当前目录位置，准确定位source与dest

#### rm指令

rm指令移除文件或目录

- 基本语法

  rm 【选项】要删除的文件或目录

- 常用选项

  -r：**递归删除**整个文件夹

  -f：**强制删除**而不提示

rm -rf bb 强制递归删除bb文件夹

#### mv指令【重要】

mv**移动文件**与目录或**重命名**

- 基本语法

  mv oldNameFile newNameFile（重命名）

  mv temp/movefile /targetFolder（移动文件）

mv aaa.txt pig.txt 将当前目录的aaa给移动到当前目录的pig，已经有aaa所以给重命名

mv pig.txt /root/ 将当前目录的pig移动到/root目录下

#### cat指令

cat查看文件内容，以只读的方式打开

- 基本语法

  cat【选项】要查看的文件

- 常用选项

  -n：显示行号

cat -n /etc/profile | more    cat指令打开文件，显示行号，并以管道用more来分页

cat只能浏览文件，而不能修改文件，为了浏览方便，一般会带上 管道命令 | more

#### more指令

more指令是基于VI编辑器的文本过滤器，以全屏的方式**按页显示文本文件的内容**。

- 基本语法

  more 要查看的文件

- 快捷键

  - 空格 下翻一页
  - ENTER 向下翻一行
  - q 立刻离开more，不再显示该文件内容
  - Ctrl+F 向下滚动一屏
  - Ctrl+B 返回上一屏
  - = 输出当前的行号
  - :f 输出文件名和当前的行号

#### less指令

less指令用于分屏查看文件内容，功能与more相似，比more更强大，支持各种显示终端。less指令在显示文件内容时，**不是一次将整个文件加载后才显示**，而是根据需要加载内容。**对于显示大型文件有较高的效率**。

- 基本语法

  less 要查看的文件

- 操作说明

  - 空格或pagedown 下翻一页
  - pageup 上翻一页
  - /子串 向下查找 n：向下找 N：向上找
  - ?子串 向上查找 n：向上查找 N：向下查找
  - q 离开less

#### >指令和>>指令

`>`**重定向**将原来内容覆盖

`>>`**追加**，将新内容追加到原来尾部

基本语法

1. ls -l > 文件 列表的内容写入到文件中（覆盖写）
2. ls -al >>文件 列表内容追加到文件的末尾（不会覆盖）
3. cat 文件1 > 文件2 将文件1的内容覆盖到文件2
4. echo “内容”>>文件

ls -l > a.txt 将ls -l的内容覆盖写入到a.txt中，如果该文件不存在则创建

ls -l >> a.txt 将ls -l显示的内容追加到后面文件中去

cat /etc/profile > b.txt 将profile中内容覆盖到b.txt中

echo "hello,world" >> b.txt 将内容追加到b.txt中

#### echo指令

echo输出内容到控制台

- 基本语法

  echo 【选项】【输出内容】

- 应用实例

  使用echo输出环境变量，输出当前的环境路径 echo $PATH

  使用echo指令输出hello.world  echo "hello,word"

#### head指令

head用于显示文件的开头部分内容，默认情况下head指令显示文件的前10行内容

- 基本语法

  head 文件 查看文件头10行内容

  head -n 5 文件 查看文件前5行内容，5可以是任意行数

head -n 5 /etc/profile 查看profile前5行内容

#### tail指令【实用】

tail用于输出文件中尾部的内容，默认情况下tail显示文件后10行内容

- 基本语法

  tail 文件 查看文件后10行内容

  tail -n 5 文件 查看文件后5行内容

  tail -f 文件 **实时追踪**该文档的所有更新【工作常用】

tail -f a.txt 实时追踪a.txt中最新添加的文件，如果有变化就会看到

#### ln指令

软链接也叫符号链接，类似于windows中的快捷方式，主要存放链接其他文件的路径

- 基本用法

  ln -s 【原文件或目录】【软链接名】给原文件增加一个软链接

- 应用实例

  ln -s /root linkToRoot 在当前目录创建一个root目录的软链接，此目录相当于/root的快捷方式，但路径仍为软链接所在路径

  rm -rf linkToRoot 删除软链接，不能直接使用rm，因为是带内容的目录

#### history指令【好用】

查看已经执行过历史命令，也可以执行历史指令

- 基本语法

  histoty 查看已经执行过的指令

- 应用实例

  histoty 显示所有的历史指令

  history 10 显示最近使用过的10个指令

  !5 查看历史编号为5的指令

### 时间日期类

#### date指令-显示当前日期【重点】

- 基本语法
  - date 显示当前时间
  - date+%Y显示当前年份
  - date+%m显示当前月份
  - date+%d显示当前哪一天
  - date”+%Y-%m-%d %H:%M:%S” 显示年月日时分秒
- 应用案例
  - date 显示当前时间
  - date "+%Y %m %d" 显示当前年月日，中间分隔符无所谓
  - date "+%Y年%m月%d日 %H:%M:%S"  显示当前年月日时分秒

关键是加号

#### date指令-设置当前日期【重点】

- 基本语法

  date -s 字符串时间

- 应用案例

  date -s "2020-04-20 15:45:00"  设置当前的时间

#### cal指令

cal查看日历时间

- 基本语法

  cal【选项】不加选项，显示本月日历

- 应用案例

  cal 显示本月日历

  cal 2020 显示2020年的日历

### 搜素查找类

#### find指令

find指令将从目录向下递归地遍历其各个子目录，将满足条件的文件或者目录显示在终端。

- 基本语法

  find【搜索范围】【选项】

- 选项说明

  | 选项            | 功能                             |
  | --------------- | -------------------------------- |
  | -name<查询方式> | 按照指定的文件名查找模式查找文件 |
  | -user<用户名>   | 查找属于指定用户的所有文件       |
  | -size<文件大小> | 按照指定的大小查找文件           |

- 应用案例

  - find /home -name hello.txt 找到home目录下叫hello.txt的文件

  - find /opt -user root 找到opt目录下root用户的文件

  - 查找linux系统大于20 m的文件  +n 大于 -n 小于 n等于

    find / -size +20M  在根目录下按照文件大小大于20M查找

    find / -name "`*.txt`" 查找根目录下后缀为txt的文件，注意，后缀要被引号括起来，不然会报错find: 路径必须在表达式之前

#### locate指令

locate快速定位文件。利用的是系统建立的所有文件名称及路径的locate数据库，管理员要定期更新locate数据库。

- 基本语法

  locate 搜索文件

- 特别说明

  第一次运行前，要使用**updatedb**指令创建locate数据库

- 案例

  找到hello.txt

  updatedb

  locate hello.txt

#### grep指令和管道符号|

grep过滤查找，管道符|，表示将前一个命令的处理结果输出传递给后面的命令处理。

- 基本语法

  grep【选项】查找内容 源文件

- 常用选项

  - -n 显示匹配行及行号
  - -i  忽略字母大小写

- 应用案例

  cat hello.txt | grep -n hello 将cat的结果交给grep，将hello.txt中有hello的结果及行号显示出来。

  cat hello.txt | grep -ni hello 不区分大小写，将cat显示的结果交给grep，然后不区分大小写查找hello

### 压缩和解压类

#### gzip/gunzip指令【少】

gzip用于压缩文件，gunzip用于解压

- 基本语法

  - gzip 文件 压缩文件，只能将文件压缩为`*.gz`文件

  - gunzip 文件.gz 解压缩文件命令

- 细节说明

  使用gzip对文件压缩后，**不保留原来的文件**

#### zip/unzip

zip用于压缩文件，unzip用于解压，在项目打包发布中有用

- 基本语法

  - zip 【选项】XXX.zip 将要压缩的内容（ 压缩文件）

  - unzip【选项】XXX.zip（解压缩文件）

- 常用选项

  zip

  - -r ：递归压缩，即压缩目录

  unzip

  - -d<目录>：指定解压缩文件的存放目录

- 应用实例

  zip -r mypackge.zip /home/ 将home目录下的所有文件压缩到mypackge中

  unzip -d /opt/tmp mypackge.zip 将压缩包解压到指定的路径tmp下

#### tar指令【重要】

tar指令是打包指令，最后打包的是.tar.gz文件

- 基本语法

  tar【选项】XXX.tar.gz 打包的内容（打包目录，压缩后的文件格式.tar.gz）

- 选项说明

  | 选项 | 功能             |
  | ---- | ---------------- |
  | -c   | 产生.tar打包文件 |
  | -v   | 显示详细信息     |
  | -f   | 指定压缩的文件名 |
  | -z   | 打包同时压缩     |
  | -x   | 解包.tar文件     |

  打包 -zcvf

  解压 -zxvf

- 应用实例

  - 压缩多个文件，将/home/a1.txt和/home/a2.txt压缩成a.tar.gz

    tar -zcvf a.tar.gz a1.txt a2.txt （a.tar.gz为打包后文件名，后面问要打包的文件）-zcvf记住

  - 将/home文件夹压缩成myhome.tar.gz

    tar -zcvf myhome.tar.gz /home/（对home目录整体打包）

  - 将a.tar.gz解压到当前目录

    tar -zxvf a.tar.gz（解压是-zxvf）

  - 将myhome.tar.gz解压到/opt目录下

    tar -zxvf myhome.tar.gz -C /opt/（需要加上-C） 指定解压到的目录要事先存在

### 组管理与权限管理

#### Linux组基本介绍

在linux中每个用户必须属于一个组，不能独立于组外。在linux中每个文件有**所有者**、**所在组**、**其他组**的概念。

#### 文件/目录所有者

一般为文件的创建者，谁创建该文件，成为该文件的所有者

##### 查看文件的所有者

1. 指令：**ls -ahl**

2. 案例

   创建tom，属于police组，再用tom创建文件，看文件所属

   groupadd police

   useradd -g police tom

   touch ok.txt

   ls -ahl

   结果如下

   -rw-r--r--. 1 tom  police    0 4月  20 21:08 ok.txt 显示了ok文件所有者是tom

##### 修改文件的所有者

1. 指令：chown 用户名 文件名（change owner）

2. 案例

   root创建apple.txt，将所有者改为tom

   touch apple.txt

   chown tom apple.txt

   ls -ahl

   用户改了，但所在组没有改

#### 组的创建

基本指令

groupadd 组名

创建用户fox放入monster中

groupadd monster

useradd -g monster fox

id fox看所属组

#### 文件所在组

##### 查看文件所在组

基本指令

ls -ahl

##### 修改文件所在组

chgrp 组名 文件

chgrp police apple.txt 将apple所在组改为police

#### 其他组

除文件的所有者和所在组的用户外，系统的其他用户都是文件的其他组

##### 改变用户所在组

用root的管理权限可以改变某个用户所在的组。

1. usermod -g 组名 用户名
2. usermod -d 目录名 用户名 改变该用户登录的初始目录

案例

创建一个土匪组bandit，将tom从police改变为bandit

usermod -g bandit tom

#### 权限管理【重要】

-rw-r--r--. 1 tom police 0 4月  20 21:08 ok.txt

看具体内容，使用**ls -ahl**

从头开始看

- 文件类型
  - `-`普通文件
  - `d:`目录
  - `l:`软链接
  - `c`字符设备【键盘鼠标】
  - `b:`块文件，硬盘
- 文件**所有者**权限，3个一读
  - `r`读
  - `w`写
- 文件**所在组的用户**的权限，3个一读
  - `r--`只有读权限
- 文件的**其他组用户**的权限，3个一读
  - `r--`只有读权限
- 若是文件，表示硬链接的数；若为目录，表示该目录子目录个数
- 文件所有者 tom
- 文件所在组 police
- 文件大小 0字节；若是**目录，统一4096**
- 文件最后修改时间 4月  20 21:08

##### rwx权限

r=4,w=2,x=1

rwx作用在**文件**

1. r代表可读：可以读取，查看
2. w代表可写：可以修改，但不代表可以删除，只有对该文件所在目录有写文件，才能删除该文件
3. x代表可执行：可以被执行

rwx作用在**目录**

1. r代表可读：可以读取，ls查看目录内容
2. w代表可写：可以修改，目录内创建+删除+重命名目录
3. x代表可执行：可以进入该目录

##### 修改权限

通过chmod指令，可以修改文件或目录的权限

第一种方式，+、-、=变更权限

u：所有者，g：所有组，o：其他人，a：所有人（ugo的总和）

1、chmod u=rwx,g=rx,o=x 文件目录名

2、chmod o+w 文件目录名（给此文件其他用户增加w权限）

3、chmod a-x 文件目录名（给所有用户减去x权限）

案例

1、给abc文件的所有者读写执行的权限，给所在组读执行权限，给其他组用户读执行权限

chmod u=rwx,g=rx,o=rx abc

这样修改后的权限为

-rwxr-xr-x. 1 root root      0 4月  21 10:35 abc

2、给abc文件的所有者除去执行的权限，增加组写的权限

chmod u-x,g+w abc

3、给abc文件的所有用户添加读的权限

chmod a+r abc

第二种方式

通过数字变更权限
r=4 w=2 x=1   rwx=4+2+1=7

chmod u=rwx,g=rx,o=x 文件目录名

相当于 chmod 751 文件目录名

案例

将/home/abc.txt文件权限修改成rwxr-xr-x，使用数字的方式实现

chmod 755 abc.txt

##### 修改文件所有者-chown【重要】

基本介绍

chown newowner file 改变文件的所有者

**chown newowner:newgroup file 改变用户的所有者和所有组**

-R 如果是目录，则使其下所有子文件或目录**递归**生效

案例

1、将/home/abc.txt文件的所有者修改为tom

chown tom abc.txt

2、将/home/kkk目录下的所有的文件和目录所有者都修改成tom

应该使用root用户操作

chown -R tom kkk/ 将kkk目录下所有文件及子目录所有者递归的更改为tom

##### 修改文件所在组-chgrp

基本介绍

chgrp newgroup file 修改文件的所有组

案例

1、将/home/abc.txt文件所在组修改为bandit

chgrp bandit abc.txt

2、将/home/kkk目录下的所有文件和目录的所在组都修改为bandit

chgrp -R bandit /home/kkk

##### 实践-警察和土匪游戏

police，bandit

jack,jerry：警察

xh，xq：土匪

1、创建组

groupadd police

groupadd bandit

2、创建用户

useradd -g police jack
useradd -g police jerry
useradd -g bandit xh
useradd -g bandit xq

passwd 用户 指定密码

3、jack创建一个文件，自己可以读写，本组人可以读，其他组人没有任何权限

chmod 640 jack01.txt （6表示rw，4表示r，0表示都没有）

4、jack修改该文件，让其他组人可以读，本组人可以读写

chmod 664 jack01.txt或者chmod g=rw,o=r jack01.txt

5、xh投靠警察，看是否可以写

xh是无法进入jack目录的，因为对其他组用户jack目录没有权限，如果想让xh进入，起码需要有o=rx权限，需要root修改。

先使用root用户修改其组 usermod -g police xh

然后jack需要给/home/jack的文件夹增加本组用户的权限 chmod g=rx

然后xh需要logout后重新登录，这时候就可以进入了

### 定时任务调度

如果只是简单的任务，可以不用写脚本，直接在crontab中加入任务即可

对于比较复杂的任务，需要写脚本（shell编程）

- 概述

  任务调度，指系统在某个时间执行的特定的命令或程序

- 任务调度分类：

  1、系统工作：有些重要的任务必须周而复始的执行，如病毒扫描等

  2、个别用户工作：个别用户可能希望执行某些程序，如对mysql数据库的备份

- 基本语法

  crontab【选项】

- 常用选项

  -e：编辑crontab定时任务

  -l：查询crontab任务

  -r：删除当前用户所有的crontab任务

任务要求：

​	设置任务调度文件 /etc/crontab

​	设置个人任务调度。执行crontab -e 命令

​	输入任务到调度文件

​	如`*/1****ls -l /etc/ >> /tmp/to.txt`

​	即每小时的每分钟执行ls -l /etc/ >> /tmp/to.txt命令

步骤：

1. cron -e
2. `*/1 * * * * ls -l /etc/ >> /tmp/to.txt`
3. 保存退出后生成程序
4. 每一分钟都自动执行

参数细节说明

- 5个占位符说明

  | 项目      | 含义             | 范围                    |
  | --------- | ---------------- | ----------------------- |
  | 第一个`*` | 一小时中第几分钟 | 0-59                    |
  | 第二个`*` | 一天中第几小时   | 0-23                    |
  | 第三个`*` | 一个月中第几天   | 1-31                    |
  | 第四个`*` | 一年当中的第几月 | 1-12                    |
  | 第五个`*` | 一周当中的星期几 | 0-7（0和7都代表星期日） |

- 特殊符号说明

  | 特殊符号 | 含义                                                         |
  | -------- | ------------------------------------------------------------ |
  | `*`      | 代表任何时间，如第一个`*`代表一小时中每分钟都执行一次        |
  | ,        | 代表不连续的时间。如`0 8,12,16 * * *`命令，代表在每天的8点0分，12点0分，16点0分都执行一次命令 |
  | -        | 代表连续的时间范围，如`0 5 * * 1-6`命令，代表在周一到周六的凌晨5点0分执行命令 |
  | `*/n`    | 代表每隔多久执行一次，如`*/10 * * * *`命令，代表每隔10分钟执行一次命令 |

- 定时案例

  星期几和几号最好不要同时出现，容易混乱

  {% asset_img vim切换.png This is an example image %}

应用实例

1、每隔1分钟，将当前的日期和日历信息，追加到/tmp/mydate文件中

步骤

- 编写文件/home/mytask1.sh

  date >> /tmp/mydate

  cal >> /tmp/mydate

- 给mytask1.sh一个**可执行权限**

  chmod u=rwx mytask1.sh

- crontab -e

- `*/1 * * * * /home/mytask1.sh`

小结：将要做的事写入sh脚本文件，然后使用crontab -e写入时间与指定要执行的文件

2、每天凌晨2点，将mysql数据库testdb，备份到文件中

步骤

- 编写文件/home/mytask2.sh

  /usr/local/mysql/bin/mysqldymp -uroot -proot testdb > /tmp/mydb.bak

- 给/home/mytask2.sh执行权限

  chmod u=rwx mytask2.sh

- crontab -e

- `0 2 * * * /home/mytask2.sh`

crond相关指令

1. crontab -r：终止当前用户所有任务调度
2. crontab -l：列出当前任务调度
3. service crond restart：重启任务调度

### Linux磁盘分区、挂载

#### 分区基础

1. mbr分区（较老）

   分区数量有限制，兼容性好，最大只支持2TB

2. gtp分区

   分区无限制（操作系统可能限制），最大支持18EB容量

#### Linux分区

linux将分区挂载（mount）在目录上。

linux硬盘分成IDE硬盘与SCSI硬盘，现在基本是SCSI，SCSI硬盘标识为sdx~，x为盘号，a是基本盘，b是从属盘，c是辅助主盘，d是辅助从属盘。

使用lsblk（**老师不离开**来看系统分区和挂载情况）

lsblk -f
NAME   FSTYPE LABEL   UUID                                 MOUNTPOINT
sda                                                        
├─sda1 ext4           01691513-3a25-4427-a882-0d104c658d95 /boot
├─sda2 swap           4e6fe5c2-4cf0-4d17-9e4a-c371c077f57f [SWAP]
└─sda3 ext4           b62f2b73-1ed3-4946-a5e2-d2d214153d84 /
sr0    iso966 CentOS_6.10_Final

sda1-3是分区情况，ext4与swap是分区类型，中间是唯一标识分区的UUID，后面为挂载点（文件系统）。

- 新增一个硬盘
- 分区 fdisk /dev/sdb
  - n 新增分区
  - w 将分区写入硬盘并离开
- 格式化 mkfs -t ext4 /dev/sdb1
- 挂载
  - 先创建挂载目录 /home/newdisk
  - 挂载 mount /dev/sdb1 /home/newdisk
- 设置自动挂载（用久挂载，重启后仍然可以挂载）
  - vim /etc/fstab
  - /dev/sdb1                                 /home/newdisk           ext4    defaults        0 0
  - mount -a

如果想要取消挂在 umount /dev/sdb1

#### 磁盘情况查询

##### 查询系统整体磁盘使用情况

- 基本语法

  df -h

##### 查询指定目录的磁盘占用情况

- 基本语法

  du -h /目录

  查询指定目录的磁盘占用情况，默认当前目录

  -s：指定目录占用大小汇总

  -h：带计量单位

  -a：含文件

  --max-depth=1子目录深度

  -c：列出明细的同时，增加汇总值

- 案例

  查询 /opt目录的磁盘占用情况，深度为1

  du -ach --max-depth=1 /opt

##### 工作实用指令

1、统计/home文件夹下文件的个数

ls -l /home | grep "^-" | wc -l

ls -l /home统计出home目录下所有文件加目录，然后将结果用管道交给grep筛选，条件是类型以-打头，代表是文件，将筛选后的结果用管道交给wc来统计

2、统计/home文件夹下目录的个数

ls -l /home | grep "^d" | wc -l

3、统计/home文件夹下文件的个数，包括子文件

ls -lR /home | grep "^-" | wc -l 加入R选项递归统计

4、统计/home文件夹下目录的个数，包括子目录

ls -lR /home | grep "^d" | wc -l 加入R选项递归统计

5、用树状结构显示目录

yum安装 yum install tree

然后使用tree查看

### 网络配置

目前网络采用NAT模式，linux与windows通过虚拟网卡组成网络连接。linux的IP可能是动态的。linux通过windows虚拟网卡与windows通讯，在通过真实网卡来与外部网络通讯。

ping 主机来判断网络是否连通

#### 自动连接

linux可以设置自动连接，但每次自动获取的ip可能不一样。不适用于做服务器，因为服务器的ip地址需要是固定的。

#### 指定固定ip

直接修改配置文件来指定ip，并可以连接到外网。

/etc/sysconfig/network-scripts/ifcfg-eth0

修改为192.168.65.129

~~~
ONBOOT=yes 启用boot配置
BOOTPROTO=static  以静态方式获取ip
IPADDR=192.168.65.129 指定ip
GATEWAY=192.168.65.2 网关
DNS1=192.168.65.2 dns和网关保持一致即可
~~~

service network restart 重启网络服务

### 进程管理【重点】

> B站视频：https://www.bilibili.com/video/BV1xJ411p7ot

#### 进程通信方式

##### 管道

如ls -l | grep xx，这就是管道，将前一个进程处理的结果交给后一个进程来处理

##### 消息队列

操作系统将信息放入消息队列中，不同的进程通过消息队列来获取信息

##### 共享内存

一般进程有虚拟地址与物理地址，物理地址一般不同，而共享内存是两个进程物理地址相同，这样操作一个内存的数据来通信

##### 套接字

socket，一般是不同机器间的进程通信非常常用，如通过3306端口与mysql进程通信，使用套接字。如果是不同机器，使用tcp套接字，本机使用linux套接字

##### 信号量

semphore，控制一定数量的进程来访问一个数据

##### 信号

signal，一个进程向另一个进程发送信号，另一个进程进行处理，在linux指令为kill，因为信号一般都是杀死另一个进程，使用kill -l来看，有64种。

实例

1、一个进程被阻塞，使用ctrl+c终止进程，此时就是shell向此进程发送信号SIGINT

2、进程被阻塞，在另一个进程中，使用kill+进程号杀死进程，此时显示TERMINATED，此时为15号信号SIGTERM

3、进程被阻塞，在另一个进程中，使用kill+-9+进程号杀死进程，显示KILLED，此时为9号信号SIGKILL。此信号无法被程序处理，可以强制杀死程序进程。

编写java代码来处理信号

~~~java
public class Main {
    public static void main(String[] args) throws InterruptedException {
        Signal.handle(new Signal("INT"), new SignalHandler() {
            @Override
            public void handle(Signal signal) {
                System.out.println(signal.toString()+" cached");
            }
        });
        Signal.handle(new Signal("TERM"), new SignalHandler() {
            @Override
            public void handle(Signal signal) {
                System.out.println(signal.toString()+" cached");
            }
        });
        while (true){
            Thread.sleep(1000);
            System.out.println(123);
        }
    }
}
~~~

来处理SIGINT和SIGTERM信号。在windows下将此文件打包

~~~
jar -cvf 1.jar .
~~~

即将此目录下全部文件打包成1.jar文件，此时需要指定主类，在最后行增加

Main-Class: com.zc.Main+换行

然后在linux下运行

java -jar 1.jar

这时候在另一个进程中找到其PID号 ps -aux | grep 1.jar（windows下为jps）

如果使用ctrl+c或者kill 对应PID，都会被捕捉，但是使用kill -9 PID，进程就会被杀死，无法被捕捉。

#### 进程基本介绍

1. linux中每个执行的程序都称为一个进程，每一个进程都分配一个ID号
2. 每个进程都对应有父进程，父进程可以复制多个子进程
3. 每个进程以前台与后台方式存在。前台是屏幕上可以看到的
4. 一般系统服务以后台进程存在，常驻在系统中，关机结束

#### 基本介绍

ps指令，查看哪些进程执行及执行情况，一般使用**ps -aux**，结合 | 使用，ps -aux | more

ps -a：显示当前终端所有进程信息

ps -u：以用户的格式显示进程信息

ps -x：显示后台进程运行的参数

#### ps详解【重要】

USER        PID %CPU %MEM    VSZ   RSS TTY      STAT  START   TIME    COMMAND
root          1         0.0      0.0    19356 1568 ?        Ss        19:52    0:02     /sbin/init

USER为用户名，PID为进程id，CPU为当前进程占用CPU情况，MEM占用内存情况，VSZ占用虚拟内存情况，RSS使用物理内存情况，TTY使用终端，STAT当前进程状态（s休眠，r运行），START启动时间，TIME占用CPU总时间，COMMAND进程执行时命令行

可以利用**grep过滤**，这种更经常使用

如看是否有sshd服务 **ps -aux | grep sshd**

#### 应用实例

##### 看进程父进程

ps -ef | more

PPID为父进程的ID号

看sshd进程的父进程 ps -ef | grep sshd

#### 终止进程

kill 【选项】进程号（通过进程号杀死进程），经常使用-9

killall 进程名称（通过进程名称杀死进程，也支持**通配符**）

常用选项

- -9 表示强制进程停止

案例

1、踢掉某个非法登录用户

先看sshd进程 ps -aux | grep sshd

然后找到要踢掉的用户进程，如下

hugh       3837  0.1  0.0 102136  1932 ?        S    20:59   0:00 sshd: hugh@pts/2

然后kill对应进程号 kill 3837

2、终止远程登录服务sshd，在适当时候再次重启sshd服务

先ps -aux | grep sshd，然后kill对应进程号，这样便无法远程登录

3、终止多个gedit编辑器【killall】

killall gedit

4、终止一个终端

查看终端进程号 ps -aux | grep bash

kill -9 终端进程号

也可以终止自己

#### 查看进程树pstree

pstree【选项】，更加直观显示进程信息

常用选项

- -p：显示进程的PID
- -u：显示进程的所属用户

案例

1、用树状形式显示进程pid

pstree -p

2、用树状形式显示进程的用户id

pstree -u

#### 服务管理

服务service本质就是进程，但运行在后台，通常监听某个端口，等待其他进程请求。

service管理指令

service 服务名 【start|stop|restart|reload|status】

在Centos 7后，不再使用service，而是systemctl

案例

查看防火墙状态，关闭，开启

service iptables status 一般防火墙只开放22号端口（远程登录端口）

关闭

通过telnet指令检查linux的某个端口是否在监听，且可以访问

dos下telnet ip地址 端口 ，wn10下需要开启telnet服务

service关闭服务立刻生效，但只是暂时的，重启系统后，回归对以前服务的设置

##### 查看服务名

方式一：setup指令

方式二：/ect/init.d/服务名称

ls -l /etc/init.d/

##### 服务运行级别

默认3和5

0：关机

1：单用户（找回丢失密码）

2：多用户无网络服务【少】

3：**多用户网络服务**【多】

4：保留

5：**图形界面**【多】

6：重启

系统运行级别配置文件 /etc/inittab，使用vi来修改

开机流程

开机—BIOS—/boot—init进程1—运行级别—运行对应服务

服务对每个运行级别设置是否自启动

##### chkconfig指令【重要】

通过chkconfig可以给**每个服务的各个运行级别**设置自启动/关闭

基本语法

`chkconfig --list` 看所有服务所有级别自启动设置

- 查看服务 --list | grep xxx
- `chkconfig 服务名 --list`
- `chkconfig -- level 5 服务名 on/off`设置服务在某个级别启动或者关闭
- `chkconfig 服务名 on/off`设置所有级别下开启或者关闭

chkconfig --list | grep sshd

chkconfig iptables --list 看防火墙的

chkconfig设置自启动或关闭后，需要重启才生效。

#### 进程监控

##### 动态监控进程top【重要】

top与ps命令类似，显示正在进行的进程，top执行一段时间可以更新正在运行的进程

基本语法

top【选项】

选项

- -d 秒数：指定top命令每隔几秒更新，默认3秒
- -i：top不显示任何闲置或者僵死进程
- -p：通过指定监控进程ID来仅仅监控某个进程状态

交互操作

- P：CPU使用率排序，默认
- M：内存使用率排序
- N：PID排序
- q：退出top

实例

1、监视特定用户【输入u】

输入top，按下回车

top - 10:01:03 up 6 min,  2 users,  load average: 0.18, 0.25, 0.13
Tasks: 220 total,   1 running, 219 sleeping,   0 stopped,   0 zombie
Cpu(s):  0.3%us,  2.3%sy,  0.0%ni, 97.1%id,  0.2%wa,  0.0%hi,  0.1%si,  0.0%st
Mem:   2037260k total,   561888k used,  1475372k free,    22204k buffers
Swap:  2097148k total,        0k used,  2097148k free,   214864k cached

第一行：系统运行时间，用户数，负载均衡情况

第二行：总任务书，zombie为僵死进程

第三行：us用户占用，sy系统占用，id剩余可用

Mem：内存情况

Swap：内存不够用swap

在top下，按下u，然后输入用户名，可以看特定用户启用的进程

2、终止指定的进程

输入top，然后输入k，再输入要结束的进程ID号

按下q退出

##### 监控网络状态【重要】

**netstat**查看系统网络情况

基本语法

netstat【选项】常写：**netstat -anp | more**

选项说明

- -an：按一定顺序排列输出
- -p：显示哪个进程在调用

案例

查看服务名为sshd的服务信息

netstat -anp | grep sshd

netstat -antp 看tcp连接监听端口号

### RPM和YUM

#### rpm

用于互联网下载包的打包和安装工具，红帽发布，类似windows下的setup.exe。

##### rpm包简单查询指令

查询已安装的rpm列表 rpm -qa | grep xx

查询当前linux下是否安装火狐

rpm -qa | grep firefox
firefox-52.8.0-1.el6.centos.x86_64

查询结果是软件名，版本，适用于centos6.x的64位系统

若是i686、i386表示32位系统，noarch表示通用

##### rpm包其他指令

rpm -qa：查询所安装的所有rpm包

rpm -qa | more：分页查询所安装的所有rpm包

rpm -qa | grep X：查询是否安装指定的rpm包

rpm -qi 软件名：查询rpm安装软件包信息   rpm -qi firefox

rpm -ql 软件包名：查询软件包中的文件**安装路径**【重要】

rpm -qf 文件全路径名：查询**文件属于哪个软件包**

##### 卸载rpm包

基本语法

rpm -e RPM包名

有时删除会报错，因为可能该包会被其他包依赖，如果要强制删除，`rpm -e --modeps`，不建议

##### 安装rpm包

基本语法

rpm -ivh RPM包全路径名称

参数说明

- -i：install，安装
- -v：verbose，提示
- -h：hash，进度条

案例：卸载并安装firefox

卸载：rpm -e firefox

安装：挂载Centos的CD，外部挂载的在/media目录下，然后找到CD下的Packages，找到火狐的rpm

ls -l | grep firefox

拷贝其至/opt目录下

cp firefox-52.8.0-1.el6.centos.x86_64.rpm /opt/

然后进入/opt目录，使用rpm -ivh安装

rpm -ivh firefox-52.8.0-1.el6.centos.x86_64.rpm

#### yum

Yum是一个Shell前端软件包管理器，基于RPM包管理，可以自动处理依赖关系，比较方便，用的多。需要联网。

基本指令

- yum list | grep xx 软件列表
- yum install xxx 下载安装

案例：

使用yum安装火狐

先看火狐在yum上有没有

yum list | grep firefox

然后安装firefox

yum install firefox

# Java相关安装

## 安装JDK

在Oracle官网下载JDK8，然后用XShell5传至linux的/opt目录，使用

tar -zxvf jdk包名称  进行解压

~~~shell
tar -zxvf jdk-8u241-linux-x64.tar.gz
~~~

然后配置其环境变量

~~~shell
vim /etc/profile
~~~

在最后加上

~~~shell
JAVA_HOME=/opt/jdk1.8.0_241
PATH=/opt/jdk1.8.0_241/bin:$PATH
export JAVA_HOME PATH
~~~

注意，PATH后要加上:PATH，不然原来路径会被覆盖

这时候，在5级别下，注销用户再重新登录即可。

测试使用，在其他目录下输入

~~~java
java -version
~~~

## 安装Tomcat

1、解压缩Tomcat压缩包 tar -zxvf

2、进入到其bin目录，找到startup.sh

3、运行 ./startup.sh

4、在linux本地输入localhost:8080访问

5、windows下无法访问，让防火墙放行8080端口

service iptables status 查看防火墙状态，只有22端口

vim /etc/sysconfig/iptables 编辑

增加-A INPUT -m state --state NEW -m tcp -p tcp --dport 8080 -j ACCEPT 开放8080端口

这样外网才可以访问8080端口

重启防火墙 service iptables restart

查看重启后的防火墙状态 service iptables status

这时候可以在windows上通过ip访问到

`http://linuxip:8080`

## 安装Eclipse

1、解压缩eclipse tar -zxvf

2、启动方式

- 创建快捷方式
- 进入eclipse目录，输入./eclipse

## 安装Mysql

1、查询本地是否有Mysql

rpm -qa | grep mysql

2、删除相关rpm包

rpm -e --nodeps mysql-libs-5.1.73-8.el6_8.x86_64 强制删除

3、安装编译代码需要的包

yum -y install make gcc-c++ cmake bison-devel ncurses-devel

4、解压mysql tar -zxvf

5、进入到解压后的目录，用TAB补全

6、编译安装

编译

~~~
cmake -DCMAKE_INSTALL_PREFIX=/usr/local/mysql -DMYSQL_DATADIR=/usr/local/mysql/data -DSYSCONFDIR=/etc -DWITH_MYISAM_STORAGE_ENGINE=1 -DWITH_INNOBASE_STORAGE_ENGINE=1 -DWITH_MEMORY_STORAGE_ENGINE=1 -DWITH_READLINE=1 -DMYSQL_UNIX_ADDR=/var/lib/mysql/mysql.sock -DMYSQL_TCP_PORT=3306 -DENABLED_LOCAL_INFILE=1 -DWITH_PARTITION_STORAGE_ENGINE=1 -DEXTRA_CHARSETS=all -DDEFAULT_CHARSET=utf8 -DDEFAULT_COLLATION=utf8_general_ci
~~~

编译并安装

make && make install    （&&表示先执行前面的，再执行后面的）

7、配置mysql

- 确认用户与组中是否有mysq

  cat /etc/passwd

  cat /etc/group

- 创建mysql组，并增加mysql用户，放入组中

  groupadd mysql

  useradd -g mysql mysql

- 修改/usr/local/mysql权限

  ls -l /usr/local，看其有没有mysql

  drwxr-xr-x. 13 root root 4096 4月  22 13:10 mysql

- 将此目录的用户和组都改成mysql

  chown -R mysql:mysql /usr/local/mysql 递归的修改此目录的所有者与组

- 初始化配置

  需要先进入安装路径/usr/local/mysql

  初始化

  ~~~
  scripts/mysql_install_db --basedir=/usr/local/mysql --datadir=/usr/local/mysql/data --user=mysql
  ~~~

- 避免干扰，将/etc/my.cnf改名为my.cnf.bak，避免配置文件冲突

  ~~~
  mv /etc/my.cnf /etc/my.cnf.bak
  ~~~

8、启动mysql

- 添加服务，拷贝服务脚本到init.d目录

  ~~~
  cp support-files/mysql.server /etc/init.d/mysql
  ~~~

- 设置自启动（chkconfig 服务名 on）

  ~~~
  chkconfig mysql on
  ~~~

- 启动服务

  ~~~
  service mysql start
  ~~~

  netstat -anp | more 查看网络状态

  tcp        0      0 :::3306                     :::*                        LISTEN      20144/mysqld

  可以看到3306端口被监听

- 使用mysql

  进入bin目录，./mysql -uroot -p，默认密码为空

  设置密码

  ~~~sql
  SET PASSWORD = PASSWORD('指定密码');
  ~~~

- 配置路径

  vim /etc/profile，修改PATH

  ~~~
  PATH=/opt/jdk1.8.0_241/bin:/usr/local/mysql/bin:$PATH
  ~~~

- 刷新环境变量

  source /etc/profile