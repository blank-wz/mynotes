# 一, API



## 1.1 Object 有哪些公用方法

1. **equals** 在Object中与==是一样的，子类一般需要重写该方法
2. **hashCode** 该方法用于哈希查找，重写了equals方法一般都要重写hashCode方法。这个方法在一些具有哈希功能的Collection中用到
3. **toString** 转换成字符串，一般子类都有重写，否则打印句柄
4. **getClass** final方法，获得运行时类型
5. **wait** 使当前线程等待该对象的锁，当前线程必须是该对象的拥有者，也就是具有该对象的锁。 **wait()** 方法一直等待，直到获得锁或者被中断。 **wait(long timeout)** 设定一个超时间隔，如果在规定时间内没有获得锁就返回
6. **notify** 唤醒在该对象上等待的某个线程
7. **notifyAll** 唤醒在该对象上等待的所有线程



## 1.2 Comparable 和 Comparator 的区别

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

# 二, 集合



## 2.1 集合和数组的区别

1. 集合只能存储引用类型; 数组可以存储基本类型和引用类型
2. 集合的长度可变; 数组的长度不可变
3. 集合可以存储多种数据类型; 数值只能存储同一数据类型



## 2.2 如何实现 Array 和 List 之间的转换

+ Array 转 List： Arrays. asList(array) 

- List 转 Array：List 的 toArray() 方法



## 2.3 使用集合的好处

1. 不受容器大小的限制, 随时可以添加删除元素
2. 提供了大量操作元素的方法



## 2.4 常用的集合类有哪些

1. Collection接口的子接口包括：Set接口和List接口
2. Set接口的实现类主要有：HashSet、TreeSet、LinkedHashSet等
3. List接口的实现类主要有：ArrayList、LinkedList、Stack以及Vector等
4. Map接口的实现类主要有：HashMap、TreeMap、Hashtable、ConcurrentHashMap以及Properties等



## 2.5 List，Set，Map三者的区别, 存取元素时，各有什么特点

+ **Collection **集合主要有List和Set两大接口
  1. **List** (对付顺序的好帮手) ：一个有序, 容器元素可以重复，可以插入多个null元素，元素都有索引。常用的实现类有 ArrayList、LinkedList 和 Vector
  2. **Set** (注重独一无二的性质) ：一个无序, 容器不可以存储重复元素，只允许存入一个null元素，必须保证元素唯一性。Set 接口常用实现类是 HashSet、LinkedHashSet 以及 TreeSet

+ **Map ** (用Key来搜索的专业户) 是一个键值对集合，存储键、值和之间的映射。 Key无序，唯一；value 不要求有序，允许重复。Map 的常用实现类：HashMap、TreeMap、HashTable、LinkedHashMap、ConcurrentHashMap



## 2.6 List 和 Set 的区别

1. List 特点：一个有序容器，元素可以重复，可以插入多个null元素，元素都有索引。常用的实现类有 ArrayList、LinkedList 和 Vector。
2. Set 特点：一个无序容器，不可以存储重复元素，只允许存入一个null元素，必须保证元素唯一性。Set 接口常用实现类是 HashSet、LinkedHashSet 以及 TreeSet。
3. List 支持for循环，也就是通过下标来遍历，也可以用迭代器，但是set只能用迭代，因为他无序，无法用下标来取得想要的值。
4. Set：检索元素效率低下，删除和插入效率高，插入和删除不会引起元素位置改变。
5. List：和数组类似，List可以动态增长，查找元素效率高，插入删除元素效率低，因为会引起其他元素位置改变



## 2.7 ArrayList 和 LinkedList 的优缺点/区别

1. ArrayList 底层基于动态数组实现，LinkedList 底层基于链表实现。
2. 对于按 index 索引数据（get/set方法）：ArrayList 通过 index 直接定位到数组对应位置的节点，而 LinkedList需要从头结点或尾节点开始遍历，直到寻找到目标节点，因此在效率上 ArrayList 优于 LinkedList。
3. 对于随机插入和删除：ArrayList 需要移动目标节点后面的节点（使用System.arraycopy 方法移动节点），而 LinkedList 只需修改目标节点前后节点的 next 或 prev 属性即可，因此在效率上 LinkedList 优于 ArrayList。
4. 对于顺序插入和删除：由于 ArrayList 不需要移动节点，因此在效率上比 LinkedList 更好。这也是为什么在实际使用中 ArrayList 更多，因为大部分情况下我们的使用都是顺序插入



## 2.8 HashSet 如何保证元素的唯一

1. 重写：HashCode，区分地址值
2. 重写：equals，区分属性值
3. 判断哈希值是否一致
   + 不同：直接存储
   + 相同：用 equals 方法判断
     + 不同：直接存储
     + 相同：替换



## 2.9 HashMap 和 Hashtable 的异同点

相同:

1. HashMap和Hashtable都是Map接口的实现类，都是双列集合，底层都是哈希表

不同:

1. HashMap 效率高但线程不安全，Hashtable 效率低但线程安全
2. HashMap 允许存储 null 键 null 值，Hashtable 不允许
3. HashMap 的初始容量为16, 扩容时扩容为原来的2倍; Hashtable 的初始容量为11, Hashtable扩容时扩容为原来的2倍+1
4. HashMap 会重新计算 hash，Hashtable 会直接使用元素的 hashCode
5. HashMap 出现在 JDK1.2，Hashtable 出现在 JDK1.0





## 2.10 集合框架底层数据结构

* **Collection**
  * List
    1. ArrayList : 数组
    2. LinkedList : 双向链表
  * Set
    1. HashSet (无序, 唯一) : 基于 HashMap 实现的，底层采用 HashMap 来保存元素
    2. LinkedHashSet : LinkedHashSet 继承与 HashSet，并且其内部是通过 LinkedHashMap 来实现有序



* **Map**
  * **HashMap** (JDK1.8 之前是数组+链表, JDK1.8开始是数组+链表+红黑树)
    1. 底层维护的是 Node 类型的数组 table
    
    2. 当添加 key-value 时, 通过 key 的 hash 值得到在 table 中的索引, 判断索引处是否有元素, 如果没有元素直接添加, 如果有, 通过 equals() 方法判断两个 key 是否相等, 如果相等则直接替换 value, 如果不相等, 判断是树结构还是链表结构. 如果添加元素时发现容量不够则扩容
    
    3. 当创建对象时, 会初始化加载因子 (loadfactor) 为 0.75, 第一次添加元素扩容 table 容量为 16, 临界值 (threshold) 为 16x0.75=12
    
    4. 以后扩容 table 容量为原来的2倍, 扩容的临界值为原来的2倍
    
    5. 在 JDK1.8 中如果一条链表的元素超过8, 且 table 大小 超过 64 则会进行树化(红黑树)
    
  * LinkedHashMap : LinkedHashMap 继承自 HashMap
  * HashTable : 数组+链表组成的，数组是 HashMap 的主体，链表则是主要为了解决哈希冲突而存在的
  * TreeMap : 红黑树（自平衡的排序二叉树）



## 2.11 哪些集合类是线程安全的？

1. Vector：就比 Arraylist 多了个同步化机制（线程安全），因为效率较低，现在已经不太建议使用
2. Hashtable：就比 Hashmap 多了个线程安全
3. ConcurrentHashMap : 高效,线程安全, 采用了**数组+Segment+分段锁**的方式实现
4. Stack：堆栈类，先进后出



## 2.12 集合遍历方式

1. 普通 for 循环 : 基于计数器

2. 增强 for 循环 (foreach) : 底层依赖的是迭代器

3. 迭代器 Iterator : 优点是代码简洁，不易出错；缺点是只能做简单的遍历，不能在遍历过程中操作数据集合，例如删除、替换

   ```
   List<String> list = new ArrayList<>();
   Iterator<String> it = list.iterator();
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

   

## 2.13 Iterator 和 ListIterator 有什么区别？

+ Iterator 可以遍历 Set 和 List 集合，而 ListIterator 只能遍历 List
+ Iterator 只能单向遍历，而 ListIterator 可以双向遍历
+ ListIterator 实现 Iterator 接口，然后添加了一些额外的功能，比如添加一个元素、替换一个元素、获取前面或后面元素的索引位置, **必须使用列表迭代器中的方法**



## 2.14 Collection 和 Collections 有什么区别

+ Collection 是一个集合接口（集合类的一个顶级接口）。它提供了对集合对象进行基本操作的通用接口方法。
+ Collections 则是集合类的一个工具类，其中提供了一系列静态方法，用于对集合中元素进行排序、搜索以及线程安全等各种操作





# 三, IO

![6](https://raw.githubusercontent.com/blank-wz/typoraimage/main/images/2022/03/23/0ec74a0c846616a4f9146e766dad5bc3-6-c9247b.png)







# 四, 反射



## 4.1 简介

反射就是获取类的字节码文件，可以使我们在运行状态获取任何一个类的方法和属性。对于任意一个对象，都能够调用它的任意方法和属性



## 4.2 获取 Class 类对象的方式

1. Class.forName("类的全限定名")
2. 实例对象.getClass()
3. 类名.class



## 4.3 反射创建实例的方式

+ Class 对象.newInstance()	创建实例,并执行无参构造
  + 类比须有无参构造
  + 无参构造访问权限要足够



## 4.4 获取类的定义信息





## 4.5 反射调用成员







# 五, 多线程



## 5.1 什么是线程池

用来存放就绪状态的线程的容器, 需要的时候从池中获取线程不用自行创建，使用完毕不需要销毁线程而是放回池中, 提高线程的高可用, 减少创建销毁线程对象的开销



## 5.2 创建线程池的方式

线程池的创建方法总共有 7 种，但总体来说可分为 2 类：

- 一类是通过 ThreadPoolExecutor 创建的线程池；
- 另一个类是通过 Executors 创建的线程池。

![img](https://raw.githubusercontent.com/blank-wz/typoraimage/main/images/2022/08/31/979f9b44b5afab3902e0d22d9a4a14f4-924700-20201218091637063-1692359064-5aabfc.png)



## 5.3 线程池的优点

1. 降低资源消耗：通过池化技术重复利用已创建的线程，降低线程创建和销毁造成的损耗。
2. 提高响应速度：任务到达时，无需等待线程创建即可立即执行。
3. 提高线程的可管理性：线程是稀缺资源，如果无限制创建，不仅会消耗系统资源，还会因为线程的不合理分布导致资源调度失衡，降低系统的稳定性。使用线程池可以进行统一的分配、调优和监控。
4. 提供更多更强大的功能：线程池具备可拓展性，允许开发人员向其中增加更多的功能。比如延时定时线程池ScheduledThreadPoolExecutor，就允许任务延期执行或定期执行。



## 5.4 sleep() 与 wait() 的区别

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



## 5.5 并发编程 - 原子性, 可见性 和 有序性

1. 原子性

   即一个操作或多个操作, 要么全部执行并且执行的过程不会被任何因素打断, 要么所有的操作都不执行 (跟事务的原子性类似)

2. 可见性

   可见性是指多个线程访问同一个变量时, 一个线程修改了这个变量的值, 其他线程能够立即看得到修改后的值

3. 有序性

   即程序的执行顺序按照代码的先后顺序执行
