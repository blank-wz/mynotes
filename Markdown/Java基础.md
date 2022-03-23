# JVM内存模型

![1](https://raw.githubusercontent.com/blank-wz/typoraimage/main/images/2022/03/22/1401832aa4fa7ee2a9bbfde8bc558940-1-0de2b8.jpg)



## Java Virtual Machine 的好处

1. 实现跨平台
2. 自动内存够管理, 垃圾回收功能
3. 数组下标越界检查
4. 多态





## 内存结构

### 1. 程序计数器 (Program Counter Register)

#### 1.1 定义和作用

+ Java 源代码 --> 二进制字节码(JVM指令) --> 程序计数器 --> 解释器 --> 机器码 --> CPU

+ 作用: 记住下一条 JVM 指令的执行地址, 依赖于 CPU 的 **寄存器**

+ 特点:
  1. 线程私有 (每个线程都有自己的程序计数器)
  2. 不存在内存溢出



### 2. 虚拟机栈 (JVM Stacks)

#### 2.1 定义

栈: 线程运行时需要的内存空间 (-Xss size     Linux, macOS, Oracle 默认为 1024KB,Windows:depends on virtual memory)

栈帧: 每个方法需要的内存 (如果方法内局部变量没有逃离方法的作用范围, 那么局部变量是线程安全的)

每个线程只能有一个活动栈帧, 对应着当前正在执行的那个方法

出栈: 释放栈帧的内存

#### 2.2 栈内存溢出 (StackOverflowError)

1. 栈帧过多 (例: 递归调用没有给合理的结束条件)
2. 栈帧过大 (不容易出现, 一个栈大小在 1M 左右, 一个 int 才占4个字节)

#### 2.3 线程运行诊断

+ 案例1: CPU 占用过高

  定位: 

  1. 用 top 定位哪个进程对cpu占用过高
  2. ps H -eo pid,**tid**,%cpu | grep 进程id (用ps命令进一步定位是那个线程引起的cpu占用过高)
  3. jstack 进程id (可列出所有的线程)
     + 可以根据线程id找到有问题的线程, 进一步定位到问题代码的源码行号 (需要将十进制的线程id换算成16进制去找)



### 3. 本地方法栈 (Native Method Statics)

不是由 Java 代码编写的代码, 由C/C++编写的本地方法, 从而 Java 可以调用这些本地方法间接的操作系统底层

Object   native修饰的方法

```
protected native Object clone() throws CloneNotSupportedException;
public final native void notify();
public final native void wait(long timeout) throws InterruptedException;
```



### 4. 堆 (Heap)

#### 4.1 定义

+ 通过 new 关键字, 创建对象都会使用堆内存
+ 特点
  1. 它是线程共享的, 堆中对象都需要考虑线程安全的问题
  2. 有垃圾回收机制

#### 4.2 堆内存溢出 (OutOfMemoryError: Java heap space)

对象被当做垃圾回收的条件 : 没有程序在使用



### 5. 方法区

#### 5.1 定义



#### 5.2 方法区内存溢出

+ JDK1.8 以前会导致永久代(**Permgen**)内存溢出

  ```
  -XX:MaxMetaspaceSize=8m
  ```

  

+ JDK1.8 之后会导致元空间(**Metaspace**)内存溢出

  ```
  -XX:MaxMetaspaceSize=8m // 默认使用的是系统内存, 所以设置小一点可以暴露问题
  ```



场景 :

+ spring  (cglib)
+ mybatis



#### 5.3 运行时常量池

+ 常量池: 一张表, 虚拟机指令根据这张常量表找到要执行的类名, 方法名, 参数类型, 字面量等信息
+ 运行时常量池: 常量池是 *.class 文件中的, 当该类被加载, 它的常量池信息就会放入运行时常量池, 并把里面的符号地址变为真实地址
+ StringTable

```java
public class Demo {
	String s1 = "a";
	String s2 = "b";
	String s3 = "ab";
	String s4 = s1 + s2; // new StringBuilder().append("a").append("b").toString()  new String("ab")
	String s5 = "a" + "b"; // javac 在编译期间优化, 结果已经在编译期间确定为 ab
	System.out.println(s3 == s4); // false
	System.out.println(s3 == s4); // true
}
```



#### 5.4 StringTable 特性

1. 常量池中的字符串仅是符号, 第一次用到时才变为对象
2. 利用串池机制, 来避免重复创建字符串对象
3. 字符串变量的拼接原理是 StringBuilder (JDK1.8)      在堆中
4. 字符串常量的拼接原理是 编译器优化   在常量池中
5. 可以使用 intern 方法, 主动将串池中还没有的字符串对象放入串池









# 基础



## 何为面向对象

将生活中的事物的属性和行为封装成一个类, 然后实例化这个类成为一个对象, 我们再像一个指挥者一样调用这个对象的属性和方法来完成我们想要完成的任务



## Get 请求和 Post 请求的区别

1. get 请求一般是用于从服务器端获取数据; post 请求一般是将数据发送到服务器端
2. get 请求也可以将数据发送到服务端,  但是在 url 中可见, 所以安全性差, 且参数长度有限; post请求传递的参数在request body中, 不会在 url 中展示, 比get安全
3. get 请求可以刷新, post 刷新的话需要重新提交数据
4. get 请求可以被缓存,记录可以保存在历史记录中; post 不行
5. post 请求可以被收藏为书签, post 不行
6. get请求只能进行url编码, post 支持多种编码方式
7. get请求通常是 通过url 地址栏请求; post 通常通过表单提交数据请求
8. get 请求会把 http header 和 data 一起发送出去, 服务器端响应200; post 请求先发送 header 等服务器端响应100 再继续发送data 服务器端响应200







## 抽象类和接口的区别

| 参数       | 抽象类                                                       | 接口                                                         |
| ---------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 声明       | 抽象类使用abstract关键字声明                                 | 接口使用interface关键字声明                                  |
| 实现       | 子类使用extends关键字来继承抽象类。如果子类不是抽象类的话，它需要提供抽象类中所有声明的方法的实现 | 子类使用implements关键字来实现接口。它需要提供接口中所有声明的方法的实现 |
| 构造器     | 抽象类可以有构造器                                           | 接口不能有构造器                                             |
| 访问修饰符 | 抽象类中的方法可以是任意访问修饰符                           | JDK8 之前接口方法默认修饰符是public, JDK9 开始有私有方法     |
| 多继承     | 一个类最多只能继承一个抽象类                                 | 一个类可以实现多个接口                                       |
| 字段声明   | 抽象类的字段声明可以是任意的                                 | 接口的字段默认都是 static 和 final 的                        |





# 基础1 - API



## 1. Object 有哪些公用方法

1. **equals** 在Object中与==是一样的，子类一般需要重写该方法
2. **hashCode** 该方法用于哈希查找，重写了equals方法一般都要重写hashCode方法。这个方法在一些具有哈希功能的Collection中用到
3. **toString** 转换成字符串，一般子类都有重写，否则打印句柄
4. **getClass** final方法，获得运行时类型
5. **wait** 使当前线程等待该对象的锁，当前线程必须是该对象的拥有者，也就是具有该对象的锁。 **wait()** 方法一直等待，直到获得锁或者被中断。 **wait(long timeout)** 设定一个超时间隔，如果在规定时间内没有获得锁就返回
6. **notify** 唤醒在该对象上等待的某个线程
7. **notifyAll** 唤醒在该对象上等待的所有线程



## 2. Comparable 和 Comparator 的区别

1. Comparable相当于“内部比较器”，而Comparator相当于“外部比较器”

2. Comparable是排序接口。若一个类实现了Comparable接口，就意味着该类支持排序。实现了Comparable接口的类的对象的集合或数组可以通过Collections.sort或Arrays.sort进行自动排序

   ````java
   public interface Comparable<T> {
       public int compareTo(T o);
   }
   ````

3. Comparator是比较接口，我们若需要控制某个类的次序，可以建立一个“该类的比较器”来进行排序

   ````java
   @FunctionalInterface
   public interface Comparator<T> {
   	int compare(T o1, T o2);
       boolean equals(Object obj);
   }
   ````

   



##  

# 基础2 - 集合



## 1. 集合和数组的区别

1. 集合只能存储引用类型; 数组可以存储基本类型和引用类型
2. 集合的长度可变; 数组的长度不可变
3. 集合可以存储多种数据类型; 数值只能存储同一数据类型



## 2. 如何实现 Array 和 List 之间的转换

+ Array 转 List： Arrays. asList(array) 

- List 转 Array：List 的 toArray() 方法



## 3. 使用集合的好处

1. 不受容器大小的限制, 随时可以添加删除元素
2. 提供了大量操作元素的方法



## 4. 常用的集合类有哪些

1. Collection接口的子接口包括：Set接口和List接口
2. Set接口的实现类主要有：HashSet、TreeSet、LinkedHashSet等
3. List接口的实现类主要有：ArrayList、LinkedList、Stack以及Vector等
4. Map接口的实现类主要有：HashMap、TreeMap、Hashtable、ConcurrentHashMap以及Properties等



## 5. List，Set，Map三者的区别, 存取元素时，各有什么特点

+ **Collection **集合主要有List和Set两大接口
  1. **List** ：一个有序, 容器元素可以重复，可以插入多个null元素，元素都有索引。常用的实现类有 ArrayList、LinkedList 和 Vector
  2. **Set** ：一个无序, 容器不可以存储重复元素，只允许存入一个null元素，必须保证元素唯一性。Set 接口常用实现类是 HashSet、LinkedHashSet 以及 TreeSet

+ **Map **是一个键值对集合，存储键、值和之间的映射。 Key无序，唯一；value 不要求有序，允许重复。Map没有继承于Collection接口，从Map集合中检索元素时，只要给出键对象，就会返回对应的值对象

  Map 的常用实现类：HashMap、TreeMap、HashTable、LinkedHashMap、ConcurrentHashMap



## 6. List 和 Set 的区别

1. List 特点：一个有序容器，元素可以重复，可以插入多个null元素，元素都有索引。常用的实现类有 ArrayList、LinkedList 和 Vector。
2. Set 特点：一个无序容器，不可以存储重复元素，只允许存入一个null元素，必须保证元素唯一性。Set 接口常用实现类是 HashSet、LinkedHashSet 以及 TreeSet。
3. List 支持for循环，也就是通过下标来遍历，也可以用迭代器，但是set只能用迭代，因为他无序，无法用下标来取得想要的值。
4. Set：检索元素效率低下，删除和插入效率高，插入和删除不会引起元素位置改变。
5. List：和数组类似，List可以动态增长，查找元素效率高，插入删除元素效率低，因为会引起其他元素位置改变



## 7. ArrayList 和 LinkedList 的优缺点/区别

1. ArrayList 是实现了基于动态数组的数据结构，LinkedList 是基于链表结构
2. ArrayList 查询快，增删慢；LinkedList 增删快，查询慢
3. ArrayList 底层为动态数组，所以查询时是直接通过访问下标，查询效率高, 而增加而删除时。为了保证内存的连续，增加和删除某一位置后，后方元素都得向前移动一位
4. LinkedList 底层为双向链表，不必保证内存上的连续，所以增删快，而查询时必须要经历从头到尾的遍历，所以查询慢



## 8. HashSet 如何保证元素的唯一

1. 重写：HashCode，区分地址值
2. 重写：equals，区分属性值
3. 判断哈希值是否一致
   + 不同：直接存储
   + 相同：用 equals 方法判断
     + 不同：直接存储
     + 相同：去重



## 9. HashMap 和 Hashtable 的异同点

相同:

1. HashMap和Hashtable都是Map接口的实现类，都是双列集合，底层都是哈希表

不同:

1. HashMap 效率高但线程不安全，Hashtable 效率低但线程安全
2. HashMap 允许存储 null 键 null 值，Hashtable 不允许
3. HashMap 的初始容量为16, 扩容时扩容为原来的2倍; Hashtable 的初始容量为11, Hashtable扩容时扩容为原来的2倍+1
4. HashMap 会重新计算 hash，Hashtable 会直接使用元素的 hashCode
5. HashMap 出现在 JDK1.2，Hashtable 出现在 JDK1.0





## 10. 集合框架底层数据结构

* **Collection**
  * List
    1. ArrayList : 数组
    2. LinkedList : 双向链表
  * Set
    1. HashSet (无序, 唯一) : 基于 HashMap 实现的，底层采用 HashMap 来保存元素
    2. LinkedHashSet : LinkedHashSet 继承与 HashSet，并且其内部是通过 LinkedHashMap 来实现



* **Map**
  * **HashMap** : JDK1.8 之前是数组+链表, JDK1.8之后是数组+链表+红黑树
    1. 底层维护的是 Node 类型的数组 table
    
    2. 当添加 key-value 时, 通过 key 的 hash 值得到在 table 中的索引, 判断索引处是否有元素, 如果没有元素直接添加, 如果有, 通过 equals() 方法判断两个 key 是否相等, 如果相等则直接替换 value, 如果不相等, 判断是树结构还是链表结构. 如果添加元素时发现容量不够则扩容
    
    3. 当创建对象时, 会初始化加载因子 (loadfactor) 为 0.75, 第一次添加元素扩容 table 容量为 16, 临界值 (threshold) 为 16x0.75=12
    
    4. 以后扩容 table 容量为原来的2倍, 加载因子也为原来的2倍
    
    5. 在 JDK1.8 中如果一条链表的元素超过8, 且 table 大小 超过 64 则会进行树化(红黑树)
    
  * LinkedHashMap : LinkedHashMap 继承自 HashMap
  * HashTable : 数组+链表组成的，数组是 HashMap 的主体，链表则是主要为了解决哈希冲突而存在的
  * TreeMap : 红黑树（自平衡的排序二叉树）



## 11. 哪些集合类是线程安全的？

1. Vector：就比 Arraylist 多了个同步化机制（线程安全），因为效率较低，现在已经不太建议使用
2. Hashtable：就比 Hashmap 多了个线程安全
3. ConcurrentHashMap : 高效,线程安全, 采用了**数组+Segment+分段锁**的方式实现
4. statck：堆栈类，先进后出



## 12. 集合遍历方式

1. 普通 for 循环 : 基于计数器

2. 增强 for 循环 (foreach) : 底层依赖的是迭代器

3. 迭代器 Iterator : 优点是代码简洁，不易出错；缺点是只能做简单的遍历，不能在遍历过程中操作数据集合，例如删除、替换

   ```
   List<String> list = new ArrayList<>();
   Iterator<String> it = list. iterator();
   while(it. hasNext()){
     String obj = it. next();
     System. out. println(obj);
   }
   ```

4. 列表迭代器 ListIterator : 遍历的的同时可以修改集合中的元素, **必须使用列表迭代器中的方法**

   ```
   ListIterator<Integer> lt = list.listIterator();
   while (lt.hasNext()) {
   	System.out.println(lt.next()); // 可以遍历
       lt.remove();
   }
   System.out.println(list); // 此时 list 是空集合
   ```

   

## 13. Iterator 和 ListIterator 有什么区别？

+ Iterator 可以遍历 Set 和 List 集合，而 ListIterator 只能遍历 List
+ Iterator 只能单向遍历，而 ListIterator 可以双向遍历
+ ListIterator 实现 Iterator 接口，然后添加了一些额外的功能，比如添加一个元素、替换一个元素、获取前面或后面元素的索引位置, **必须使用列表迭代器中的方法**



## 14. Collection 和 Collections 有什么区别

+ Collection 是一个集合接口（集合类的一个顶级接口）。它提供了对集合对象进行基本操作的通用接口方法。
+ Collections 则是集合类的一个工具类，其中提供了一系列静态方法，用于对集合中元素进行排序、搜索以及线程安全等各种操作





# 基础3 - 多线程



## 1. 什么是线程池

用来存放就绪状态的线程的容器, 需要的时候从池中获取线程不用自行创建，使用完毕不需要销毁线程而是放回池中, 提高线程的高可用, 减少创建销毁线程对象的开销



## 2. 创建线程池的方式

1. newFixedThreadPool：创建固定大小的线程池，每次提交一个任务就创建一个线程，直到线程达到线程池的最大大小。线程池的大小一旦达到最大值就会保持不变，如果某个线程因为执行异常而结束，那么线程池会补充一个新线程。如果希望在服务器上使用线程池，建议使用 newFixedThreadPool方法来创建线程池，这样能获得更好的性能
2. newSingleThreadExecutor：创建一个单线程的线程池，这个线程池只有一个线程在工作，也就是相当于单线程串行执行所有任务。如果这个唯一的线程因为异常结束，那么会有一个新的线程来替代它。此线程池保证所有任务的执行顺序按照任务的提交顺序执行
3.  newCachedThreadPool：创建一个可缓存的线程池。如果线程池的大小超过了处理任务所需要的线程，那么就会回收部分空闲（60 秒不执行任务）的线程，当任务数增加时，此线程池又可以智能的添加新线程来处理任务。此线程池不会对线程池大小做限制，线程池大小完全依赖于操作系统（或者说 JVM）能够创建的最大线程大小
4. newScheduledThreadPool：创建一个大小无限的线程池。此线程池支持定时以及周期性执行任务的需求



## 3. 线程池的优点

1. 降低资源消耗
2. 提高响应速度
3. 提高线程的可管理性



## 4. sleep() 与 wait() 的区别

1. sleep() 
   1) 是 Thread 的静态方法
   2) 暂停当前线程执行, 但是不释放锁
   3) 可以在任何场景下使用
   4) 只有设置的时间到了才会醒
2. wait()
   1. 是 Object 的成员方法
   2. 暂停当前线程执行, 但是释放锁
   3. wait 方法只能在同步代码块中使用
   4. 可以随时唤醒



## 5. 并发编程 - 原子性, 可见性 和 有序性

1. 原子性

   即一个操作或多个操作, 要么全部执行并且执行的过程不会被任何因素打断, 要么所有的操作都不执行 (跟事务的原子性类似)

2. 可见性

   可见性是指多个线程访问同一个变量时, 一个线程修改了这个变量的值, 其他线程能够立即看得到修改后的值

3. 有序性

   即程序的执行顺序按照代码的先后顺序执行





# 基础4 - 反射



## 1. 简介

反射就是获取类的字节码文件，可以使我们在运行状态获取任何一个类的方法和属性。对于任意一个对象，都能够调用它的任意方法和属性



## 2. 获取 Class 类对象的方式

1. Class.forName("类的全限定名")
2. 实例对象.getClass()
3. 类名.class



## 3. 反射创建实例的方式

+ Class 对象.newInstance()	创建实例,并执行无参构造
  + 类比须有无参构造
  + 无参构造访问权限要足够



## 4. 获取类的定义信息





## 5. 反射调用成员









# 基础5 - IO

![6](https://raw.githubusercontent.com/blank-wz/typoraimage/main/images/2022/03/23/0ec74a0c846616a4f9146e766dad5bc3-6-c9247b.png)









# 数据库1 - MySQL





MySQL 常用数据类型





字符串的基本使用

char(size) 固定长度字符串, 最大255字符

varchar(size)   可变长度字符串 最大65535字节 (utf8 编码最大21844字符,1-3个字节用于记录大小) 

char(4) 和 varchar(4)  4表示的是字符,而不是字节, 不区分字符是汉字还是字母

如果varchar 不够用,可以考虑使用 text, mediumtext 或者 longtext















## 事务的4个特性 ACID

原子性（Atomicity，或称不可分割性）、一致性（Consistency）、隔离性（Isolation，又称独立性）、持久性（Durability）

1. **原子性**：一个事务（transaction）中的所有操作，要么全部完成，要么全部不完成，不会结束在中间某个环节。事务在执行过程中如果发生错误，会被回滚（Rollback）到事务开始前的状态，就像这个事务从来没有执行过一样。
2.  **一致性**：在事务开始之前和事务结束以后，数据库的完整性没有被破坏。这表示写入的资料必须完全符合所有的预设规则，这包含资料的精确度、串联性以及后续数据库可以自发性地完成预定的工作。
3. **隔离性**：数据库允许多个并发事务同时对其数据进行读写和修改的能力，隔离性可以防止多个事务并发执行时由于交叉执行而导致数据的不一致。
4. **持久性**：事务处理结束后，对数据的修改就是永久的，即便系统故障也不会丢失。



## 事务的隔离级别

1. 读未提交（Read uncommitted） 安全性最差,可能发生并发数据问题,性能最好
2. 读提交（read committed）   Oracle默认的隔离级别
3. 可重复读（repeatable read）MySQL默认的隔离级别,安全性较好,性能一般
4. 串行化（Serializable）    表级锁，读写都加锁，效率低下,安全性高,不能并发



## 数据库优化

### 1) 数据库服务器内核优化

### 2) my.cnf 配置, 一般搭配压力测试进行调试

### 3) SQL 语句优化

1. 查询SQL尽量不要使用select *，而是具体字段
2. where限定查询的数据 (需要什么数据，就去查什么数据，避免返回不必要的数据)
3. 避免在where子句中使用or来连接条件 (使用or可能会使索引失效，从而全表扫描)
4. 提高group by语句的效率 (先过滤，后分组)
5. 使用varchar代替char
6. 尽量使用数值替代字符串类型
7. 使用 explain 分析你 SQL 执行计划
8. 优化 like 语句
9. 索引不宜太多，一般5个以内
10. 索引不适合建在有大量重复数据的字段上 (Mysql 查询优化器推算发现不走索引的成本更低，很可能就放弃索引了)
11. 避免对索引字段进行计算、避免索引在字段上使用**not、<>、！=、**避免在索引上使用**IS NULL和NOT NULL**、**避免在索引列上出现数据类型转换、避免索引字段使用函数、避免建立索引的列出现空值**
12. 去重 distinct 过滤字段要少
13. where 中使用默认值代替 null
14. 批量插入性能提升 (默认新增SQL有事务控制，导致每条都需要事务开启和事务提交；而批量处理是一次事务开启和提交。自然速度飞升)
15. 批量删除优化 (避免同时修改或删除过多数据，因为会造成cpu利用率过高，次性删除太多数据，可能造成锁表，会有 lock wait timeout exceed 的错误，所以建议分批操作)
16. 伪删除设计
17. 不要有超过5个以上的表连接
18. inner join 、left join、right join，优先使用inner join





# 数据库2 - Redis (Remote Dictionary Server)



## 概述

​		Redis是一个key-value存储系统，是一个分布式缓存数据库。和Memcached类似，但是，它支持存储的value类型相对更多，包括string（字符串），hash（哈希），list（列表），set（集合）及 zset(sorted set：有序集合)。这些数据类型都支持push/pop、add/remove及取交集并集和差集及更丰富的操作，而且这些操作都是原子性的。在此基础上，redis支持各种不同方式的排序。与memcached一样，为了保证效率，数据都是缓存在内存中。区别的是redis会周期性的把更新的数据写入磁盘或者把修改操作写入追加的记录文件，并且在此基础上实现了master-slave(主从)同步。

​		redis的出现，很大程度补偿了memcached这类key/value存储的不足，在部分场合可以对关系数据库起到很好的补充作用。它提供了Java，C/C++，C#，PHP，JavaScript，Perl，Object-C，Python，Ruby，Erlang等客户端，使用很方便。



## Redis 基本特性

1. 多种数据类型存储（string（字符串），hash（哈希），list（列表），set（集合）及 zset(sorted set：有序集合)）。
2. 内存存储与持久化（RDB,AOF）。
3. 功能丰富（数据库、缓存、队列等）。
4. 简单稳定（基于c语言实现）。
5. 所有操作都是原子性的。



## Redis 的三大问题

### 缓存穿透

即黑客故意去请求缓存中不存在的数据，导致所有的请求都到数据库去查，从而数据库连接异常

解决方案:

1. 利用互斥锁，缓存失效的时候，先去获得锁，得到锁了，再去请求数据库。没得到锁，则休眠一段时间重试。
2. 采用异步更新策略，无论 Key 是否取到值，都直接返回。Value 值中维护一个缓存失效时间，缓存如果过期，异步起一个线程去读数据库，更新缓存。需要做缓存预热(项目启动前，先加载缓存)操作。
3. 提供一个能迅速判断请求是否有效的拦截机制，比如，利用布隆过滤器，内部维护一系列合法有效的 Key。迅速判断出，请求所携带的 Key 是否合法有效。如果不合法，则直接返回。



### 缓存雪崩

即缓存同一时间大面积的失效，这个时候又来了一波请求，结果请求都怼到数据库上，从而导致数据库连接异常

解决方案：

1. 给缓存的失效时间，加上一个随机值，避免集体失效。
2. 使用互斥锁，但是该方案吞吐量明显下降了。
3. 双缓存。我们有两个缓存，缓存 A 和缓存 B。缓存 A 的失效时间为 20 分钟，缓存 B 不设失效时间。自己做缓存预热操作

双缓存的实现过程：从缓存 A 读数据库，有则直接返回；A 没有数据，直接从 B 读数据，直接返回，并且异步启动一个更新线程，更新线程同时更新缓存 A 和缓存 B。



### 缓存击穿

热点数据过期时间刚好过期了。所有的请求都打在数据库上了，造成数据库的瘫痪

解决方案：

1. 使用互斥锁进行重建缓存
2. 设置热点数据永远不过期



## Redis 数据持久化

### RDB 方式持久化

#### 原理

是将Reids在内存中的数据库记录定时 dump 到磁盘上的 RDB 持久化

#### 优点

1. RDB会生成多个数据文件，每个数据文件都代表了某一个时刻中redis的数据，这种多个数据文件的方式，非常适合做冷备，可以将这种完整的数据文件发送到一些远程的安全存储上去，比如说Amazon的S3云服务上去，在国内可以是阿里云的ODPS分布式存储上，以预定好的备份策略来定期备份redis中的数据
2. RDB对redis对外提供的读写服务，影响非常小，可以让redis保持高性能，因为redis主进程只需要fork一个子进程，让子进程执行磁盘IO操作来进行RDB持久化即可。
3. 相对于AOF持久化机制来说，直接基于RDB数据文件来重启和恢复redis进程，更加快速.

#### 缺点

如果想要在redis故障时，尽可能少的丢失数据，那么RDB没有AOF好。一般来说，RDB数据快照文件，都是每隔5分钟，或者更长时间生成一次，这个时候就得接受一旦redis进程宕机，那么会丢失最近5分钟的数据



### AOF 持久化

#### 原理

append only file原理是将 Reids 的操作日志以追加的方式写入文件

#### 优点

1. AOF可以更好的保护数据不丢失，一般AOF会每隔1秒，通过一个后台线程执行一次fsync操作，最多丢失1秒钟的数据.
2. AOF日志文件以append-only模式写入，所以没有任何磁盘寻址的开销，写入性能非常高，而且文件不容易破损，即使文件尾部破损，也很容易修复
3. AOF日志文件即使过大的时候，出现后台重写操作，也不会影响客户端的读写。因为在rewrite log的时候，会对其中的指导进行压缩，创建出一份需要恢复数据的最小日志出来。再创建新日志文件的时候，老的日志文件还是照常写入。当新的merge后的日志文件ready的时候，再交换新老日志文件即可。
4. AOF日志文件的命令通过易读的方式进行记录，这个特性非常适合做灾难性的误删除的紧急恢复。比如某人不小心用flushall命令清空了所有数据，只要这个时候后台rewrite还没有发生，那么就可以立即拷贝AOF文件，将最后一条flushall命令给删了，然后再将该AOF文件放回去，就可以通过恢复机制，自动恢复所有数据.

#### 缺点

1. 对于同一份数据来说，AOF日志文件通常比RDB数据快照文件更大
2. AOF开启后，支持的写QPS会比RDB支持的写QPS低，因为AOF一般会配置成每秒fsync一次日志文件，当然，每秒一次fsync，性能也还是很高的
3. 以前AOF发生过bug，就是通过AOF记录的日志，进行数据恢复的时候，没有恢复一模一样的数据出来。所以说，类似AOF这种较为复杂的基于命令日志/merge/回放的方式，比基于RDB每次持久化一份完整的数据快照文件的方式，更加脆弱一些，容易有bug。不过AOF就是为了避免rewrite过程导致的bug，因此每次rewrite并不是基于旧的指令日志进行merge的，而是基于当时内存中的数据进行指令的重新构建，这样健壮性会好很多。

### RDB 和 AOF 的区别

一个是持续的用日志记录写操作，crash（崩溃）后利用日志恢复；一个是平时写操作的时候不触发写，会在一定时间间隔或手动提交save命令，shutdown关闭命令时，才触发备份操作



## Redis 事务管理

redis是单线程(但是在6.0中真正引用多线程的应用)，提交命令时，其它命令无法插入其中，轻松利用单线程实现了事务的原子性。那如果执行多个redis命令呢？自然就没有事务保证，于是redis有下列相关的redis命令来实现事务管理

```
multi		开启事务
exec		提交事务
discard		取消事务
watch		监控，如果监控的值发生变化，则提交事务时会失败
unwatch		解除监控
```

Redis保证一个事务中的所有命令要么都执行，要么都不执行(原子性)。如果在发送EXEC命令前客户端断线了，则Redis会清空事务队列，事务中的所有命令都不会执行。而一旦客户端发送了EXEC命令，所有的命令就都会被执行，即使此后客户端断线也没关系，因为Redis中已经记录了所有要执行的命令。



## Redis 主从复制



1. 将redis01拷贝两份，例如

   ```Linux
   cp -r redis01/  redis02
   cp -r redis01/  redis03
   ```

2. 假如已有redis服务，先将原先所有redis服务停止删除，并启动新的redis容器，例如

   ```
   docker run -p 6379:6379 --name redis6379 \
   -v /usr/local/docker/redis01/data:/data \
   -v /usr/local/docker/redis01/conf/redis.conf:/etc/redis/redis.conf \
   -d redis redis-server /etc/redis/redis.conf \
   --appendonly yes
   
   6379/6380/6381
   ```

3. 检测redis服务角色

   分别登陆三台redis容器服务，通过info指令进行角色查看，默认新启动的三个redis服务角色都为master

4. 检测redis6379的ip设置(将这个服务设置为master)
5. 设置Master/Slave架构
6. 再次登陆redis6379，然后检测info
7. 登陆redis6379测试，master都写都可以
8. 登陆redis6380测试，slave只能读

   

## Redis 集群高可用

对于redis集群(Cluster),一般最少设置为6个节点,3个master,3个slave,其简易架构如下





## 小结面试分析

String

§  博客的字数统计如何实现？（strlen）

§  如何将审计日志不断追加到指定key?(append)

§  你如何实现一个分布式自增id？(incr-雪花算法)

§  如何实现一个博客的的点赞操作？(incr,decr)

Key有效时间

§  为什么要设置key的有效时长？(释放空间，业务需要)

§  秒杀操作的计时是如何实现？(pexpire 以毫秒为单位)

§  如何设置缓存数据的有效时长?(expire-秒，pexpire -毫秒)

§  假如将登录状态存储了redis,如何设置登录的有效时长?

Hash

§  发布一篇博客需要写内存吗？（需要,hmset）

§  浏览博客内容会怎么做？（hmget）

§  如何判定一篇博客是否存在？(hexists)

§  删除一篇博客如何实现？(hdel)

§  分布式系统中你登录成功以后是如何存储用户信息的？(hmset)

List

§  如何基于redis实现一个队列结构？（lpush/rpop）

§  如何基于redis实现一个栈结构？(lpush/lpop)

§  如何基于redis实现一个阻塞式队列？(lpush/brpop)

§  如何实现秒杀活动的公平性？(先进先出-FIFO)

§  通过list结构实现一个消息队列(顺序)吗？（可以，FIFO->lpush,rpop）

§  用户注册时的邮件发送功能如何提高其效率？(邮件发送是要调用三方服务，底层通过队列优化其效率，队列一般是list结构)

§  如何动态更新商品的销量列表？(卖的好的排名靠前一些，linsert)

§  商家的粉丝列表使用什么结构实现呢？(list结构)

Set

§  朋友圈的点赞功能你如何实现？（sadd,srem,smembers,scard）

§  如何实现一个网站投票统计程序?

§  你知道微博中的关注如何实现吗？





# Spring Boot

## Spring Boot 的优点

1. 快速构件项目
2. 对主流开发框架的无配置集成
3. 项目可独立运行, 无须外部依赖 servlet 容器
4. 提供运行时的应用监控
5. 极大地提高了开发, 部署效率
6. 与云计算的天然集成





# Spring MVC











# Spring



## Spring 注解 (Component, 依赖注入)

### @Component 注解

1. 作用：用于把当前类的对象存入到spring容器中

2. 属性：value：指定bean的id，不写的时候默认是当前的类名，且首字母小写。当指定属性的值的时候就要用该值

3. web开发中，提供了3个@Component注解的衍生注解（功能一样），它们是spring框架为我们提供的三层使用的注解，使得我们的三层对象更加清晰

   + @Repository：dao层（持久层）

     **与 @Mapper 的区别: **

     + **@Mapper是MyBatis的注解，@Repository是Spring中的注解，这些注解就是声明一个Bean。**
     + **@Mapper注解不需要在SpringBoot启动类上配置扫描类；通过xml里面的namespace里面的接口地址，生成bean对象后注入到Service里面。也可以在启动类上添加@MapperScan("")，并且指明扫描的位置 **
     + **@Repository需要 @MapperScan 配置扫描地址，然后dao层生成的bean才能被注入到Service层进行使用.**

   + @Service：service层（业务层）

   + @Controller：web层（表现层）

### 依赖注入

1. **@Autowired**
   - 自动按照类型注入  **byType**，只要容器中有唯一一个bean对象类型和要注入的变量类型匹配，就可以注入成功
   - 按照类型注入的时候，如果有多个可以注入，不能注入成功的时候，可以按照名称注入（@Qualifier 需要手动修改注入的变量名称），例如，一个接口有多个实现类，那么这些实现类就是相同类型的，注入的时候会出现问题，因为注入的时候匹配到了多个bean
   - 可以省略类中的get和set方法以及配置文件中bean的依赖注入的内容，但是在spring容器中还是需要配置bean的，可以结合生成bean的注解使用
   - 出现的位置可以是变量上也可以是方法上

2. @Qualifier 
   + 属性value用于指定注入的bean的id
   + 在给类成员注入的时候不能独立使用，也就是说要和Autowired注解配合使用。当有多个相同类 型 的 bean 却只 有 一个需 要 自 动 装 配 时 ， 将 @Qualifier 注 解 和@Autowire 注解结合使用以消除这种混淆， 指定需要装配的确切的 bean。

3. **@Resource**  javax.annotation.Resource
   + 默认按照 **byName** 自动注入，由 J2EE 提供
   + @Resource有两个重要的属性：name 和 type，而Spring将 @Resource 注解的name属性解析为bean的名字，而type属性则解析为bean的类型。所以，如果使用name属性，则使用 byName 的自动注入策略，而使用type属性时则使用 byType 自动注入策略。如果既不制定 name 也不制定 type 属性，这时将通过反射机制使用byName自动注入策略。

以上三种注解都是能用于注入bean类型的数据，而基本类型和String类型无法使用上述注解

4. **@Value**：用于注入基本类型和String类型，属性用于指定值



## Spring Bean 的生命周期

Spring Bean的生命周期分为`四个阶段`和`多个扩展点`。扩展点又可以分为`影响多个Bean`和`影响单个Bean`

+ 四个阶段
  1. 实例化 Instantiation, 也就是我们通常说的new,  createBeanInstance()
  2. 属性赋值 Populate, 按照Spring上下文对实例化的Bean进行配置，也就是IOC注入, populateBean()
  3. 初始化 Initialization,  initializeBean()
  4. 销毁 Destruction

+ 多个扩展点
  1. 影响多个Bean
     + BeanPostProcessor
     + InstantiationAwareBeanPostProcessor
  2. 影响单个Bean



 ## Spring Bean 的作用域

1. 单例对象 singleton

   这种bean范围是默认的，这种范围确保不管接受到多少个请求，每个容器中只有一个bean的实例，单例的模式由bean factory自身来维护

2. 多例对象 prototype

   原形范围与单例范围相反，为每一个bean请求提供一个实例。
    request：在请求bean范围内会每一个来自客户端的网络请求创建一个实例，在请求完成以后，bean会失效并被垃圾回收器回收

3. request

   在请求bean范围内会每一个来自客户端的网络请求创建一个实例，在请求完成以后，bean会失效并被垃圾回收器回收

4. Session

   与请求范围类似，确保每个session中有一个bean的实例，在session过期后，bean会随之失效

5. global-session

   global-session和Portlet应用相关。当你的应用部署在Portlet容器中工作时，它包含很多portlet。如果你想要声明让所有的portlet共用全局的存储变量的话，那么这全局变量需要存储在global-session中











