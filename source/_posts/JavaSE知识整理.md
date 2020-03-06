---
title: JavaSE知识整理
date: 2020-03-03 22:00:20
tags: JavaSE
categories: JavaSE
---

# Java基础

## Java基本语法、特性

### 数据类型

### 运算符

### 流程控制语句

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



## 关键字

### static

### final

### this

## 面向对象

### 面向对象思想

### 匿名对象

### 封装

### 构造方法

### 继承

### 多态

### 抽象类

### 接口

### 内部类

## 集合

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
4. 如果碰撞了，以链表的方式链接到 后面
5. 如果链表长度超过阈值（8），把链表转成红黑树
6. 如果链表的长度低于6，把红黑树转成链表
7. 如果key对应节点存在，替换旧值
8. 如果桶满了（容量16*负载因子0.75），需要resize（扩容2倍重排）

#### get方法

先计算hash值找到对应的桶，然后在链表或者红黑树中通过equals方法找到对应的节点，找到值并返回。

#### 扩容resize()方法

在构造哈希表时，若不指明初始大小，默认为16，若大小达到了容量16*负载因子0.75，用大数组去替代小数组，重新调整hashmap大小为原来2倍，比较耗时。

- 在多线程环境下，调整大小会存在条件竞争，容易造成**死锁**
- rehashing是一个比较耗时的过程

#### 如何减少碰撞

- 使用扰动函数：促使元素位置分布均匀，减少碰撞几率

- 使用final对象，并采用合适的equals()和hashCode()方法

  使用String、Integer等类作为键很好，防止键值改变，放入和获取到的键不一样，就不能获取到

哈希散列的过程：可以看到**hashmap允许键为null**

~~~java
static final int hash(Object key){
	int h;
	return (key == null) ? 0 : (h=key.hashCode()) ^ (h >>> 16); 
}
~~~

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

红黑树是二叉查找树，不同的是在每个结点上增加一个存储位来表示颜色，Red或Black。而红黑树的特点是通过对任何一条从根到叶子的路径上各个结点的着色方式的限制，红黑树确保没有一条路径比其他路径长出两倍，树是接近平衡的。

二叉查找树简单理解是：

- 若有左子树，则左子树上所有结点的值均小于根节点值 
- 若有右子树，则右子树上所有结点的值均大于根节点值
- 任意节点的左、右子树分别为二叉查找树
- 没有键值相等的节点

一般普通的二叉查找树高度为log(N)，但如果二叉查找树退化为一个链表，最坏时间会变成O（N）,那么红黑树如何保证树相对平衡呢？介绍其5个性质：

1. 每个节点要么红、要么黑
2. 根节点是黑
3. 叶节点是黑的
4. 若一个节点是红，两个儿子都是黑的（不存在两个连续的红色节点）
5. 对任意节点，其到叶节点尾端指针的每条路径都包含相同数目的黑节点

{% asset_img 红黑树图.png This is an example image %}

数学证明红黑树的操作时间复杂度最差为O(logN)。

#### HashMap与HashTable

因为HashMap在多线程下不安全，而线程安全的类有HashTable，其线程安全原因是使用了synchronized修饰符，锁为调用者的this，与Collections.synchronizedMap(hashMap)几乎无区别，只是锁不一样，synchronizedMap锁为Object类的mutex，hashMap方法均用synchronized(mutex)加锁。

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

#### HashMap与ConcurrentHashMap

那么该如何优化HashTable呢？

**通过锁细粒度化，将整锁拆解成多个锁进行优化**。

早期的ConcurrentHashMap通过分段锁Segment实现

{% asset_img segment.png This is an example image %}

在HashMap的基础上，外面多了一层数组结构，有多个Segment（一种可重入锁），每个Segment中有多段数据（HashEntry），当一个线程占用一个锁时，位于此Segment上的其他数据也可以被访问到。默认分配**16**个Segment，理论上比HashTable效率高16倍。将HashMap的table数组逻辑上拆分为多个子数组，每个子数组配置一个Segment，线程只有在获取到某把分段锁后，才能获取到其中的子数组，其他没有该Segment的线程访问其中数据被阻塞，而访问没有被Segment锁住的数据不会被阻塞。

而当前的ConcurrentHashMap，使用CAS+Synchronized使锁更细化



# Java高级