# 一, API



## 1.1 Object 有哪些公用方法

1. **equals** 在Object中与==是一样的，子类一般需要重写该方法
2. **hashCode** 该方法用于哈希查找，重写了equals方法一般都要重写hashCode方法。这个方法在一些具有哈希功能的Collection中用到
3. **toString** 转换成字符串，一般子类都有重写，否则打印句柄
4. **getClass** final方法，获得运行时类型
5. **wait** 使当前线程等待该对象的锁，当前线程必须是该对象的拥有者，也就是具有该对象的锁。 **wait()** 方法一直等待，直到获得锁或者被中断。 **wait(long timeout)** 设定一个超时间隔，如果在规定时间内没有获得锁就返回
6. **notify** 唤醒在该对象上等待的某个线程
7. **notifyAll** 唤醒在该对象上等待的所有线程
8. **clone**



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

1. Collection接口的子接口包括：List接口和Set接口
2. List接口的实现类主要有：ArrayList、LinkedList、Stack以及Vector等
3. Set接口的实现类主要有：HashSet、TreeSet、LinkedHashSet等
4. Map接口的实现类主要有：HashMap、LinkedHashMap、TreeMap、Hashtable、ConcurrentHashMap以及Properties等



## 2.5 List，Set，Map三者的区别, 存取元素时，各有什么特点

+ **Collection **集合主要有List和Set两大接口
  1. **List** (对付顺序的好帮手) ：一个有序, 容器元素可以重复，可以插入多个null元素，元素都有索引。常用的实现类有 ArrayList、LinkedList 和 Vector
  2. **Set** (注重独一无二的性质) ：一个无序, 容器不可以存储重复元素，只允许存入一个null元素，必须保证元素唯一性。Set 接口常用实现类是 HashSet、LinkedHashSet 以及 TreeSet

+ **Map ** (用Key来搜索的专业户) 是一个键值对集合，存储键和值之间的映射。 Key无序，唯一；value 不要求有序，允许重复。Map 的常用实现类：HashMap、TreeMap、HashTable、LinkedHashMap、ConcurrentHashMap



## 2.6 List 和 Set 的区别

1. List 特点：一个有序容器，元素可以重复，可以插入多个null元素，元素都有索引。常用的实现类有 ArrayList、LinkedList 和 Vector。
2. Set 特点：一个无序容器，不可以存储重复元素，只允许存入一个null元素，必须保证元素唯一性。Set 接口常用实现类是 HashSet、LinkedHashSet 以及 TreeSet。
3. List 支持for循环，也就是通过下标来遍历，也可以用迭代器，但是set只能用迭代，因为他无序，无法用下标来取得想要的值。
4. Set：检索元素效率低下，删除和插入效率高，插入和删除不会引起元素位置改变。
5. List：和数组类似，List可以动态增长，查找元素效率高，插入删除元素效率低，因为会引起其他元素位置改变



## 2.7 ArrayList 和 LinkedList 的优缺点/区别

1. ArrayList 底层基于动态数组实现，LinkedList 底层基于链表实现。
2. 对于按 index 索引数据（get/set方法）：ArrayList 通过 index 直接定位到数组对应位置的节点，而 LinkedList需要从头节点或尾节点开始遍历，直到寻找到目标节点，因此在效率上 ArrayList 优于 LinkedList。
3. 对于随机插入和删除：ArrayList 需要移动目标节点后面的节点（使用System.arraycopy 方法移动节点），而 LinkedList 只需修改目标节点前后节点的 next 或 prev 属性即可，因此在效率上 LinkedList 优于 ArrayList。
4. 对于顺序插入和删除：由于 ArrayList 不需要移动节点，因此在效率上比 LinkedList 更好。这也是为什么在实际使用中 ArrayList 更多，因为大部分情况下我们的使用都是顺序插入



## 2.8 HashSet 如何保证元素的唯一

````
底层是HashMap 
1.重写：HashCode，区分地址值
2. 重写：equals，区分属性值
3. 判断哈希值是否一致
   	不同：直接存储
   	相同：用 equals 方法判断
    	不同：直接存储
    	相同：替换
````





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
    
    2. 当添加 key-value 时, 通过 key 的 hash 值 (h = key.hashCode()) ^ (h >>> 16) 得到在 table 中的索引, 判断索引处是否有元素, 如果没有元素直接添加, 如果有, 通过 equals() 方法判断两个 key 是否相等, 如果相等则直接替换 value, 如果不相等, 判断是树结构还是链表结构. 如果添加元素时发现容量不够则扩容
    
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



## 2.12 在开发中, 集合的选择

1. 先判断存储的类型 (一组对象[单列]或一组键值对[双列])

2. 一组对象[单列]: Collection接口

   允许重复: List

   ​		增删多: LinkedList [底层维护了一个双向链表]

   ​		改查多: ArrayList [底层维护 Object类型的可变数组]

   不允许重复: Set

   ​		无序: HashSet [底层是 HashMap, 维护了一个哈希表 即(数组+链表+红黑树)]

   ​		排序: TreeSet

   ​		插入和取出顺序一致: LinkedHashSet, 维护数组+双向链表

3. 一组键值对[双列]: Map

   键无序: HashMap [底层是 哈希表 JDK7: 数组+链表, JDK8: 数组+链表+红黑树]

   键排序: TreeMap

   键插入和取出顺序一致: LinkedHashMap

   读取文件: Properties



## 2.13 集合遍历方式

1. 普通 for 循环 : 基于计数器

2. 增强 for 循环 (foreach) : 底层依赖的是迭代器

3. 迭代器 Iterator : 优点是代码简洁，不易出错；可以使用Iterator本身的方法iterator.remove()来删除对象

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

   

## 2.14 Iterator 和 ListIterator 有什么区别？

+ Iterator 可以遍历 Set 和 List 集合，而 ListIterator 只能遍历 List
+ Iterator 只能单向遍历，而 ListIterator 可以双向遍历
+ ListIterator 实现 Iterator 接口，然后添加了一些额外的功能，比如添加一个元素、替换一个元素、获取前面或后面元素的索引位置, **必须使用列表迭代器中的方法**



## 2.15 Collection 和 Collections 有什么区别

+ Collection 是一个集合接口（集合类的一个顶级接口）。它提供了对集合对象进行基本操作的通用接口方法。
+ Collections 则是集合类的一个工具类，其中提供了一系列静态方法，用于对集合中元素进行排序、搜索以及线程安全等各种操作





# 三, IO

![6](https://raw.githubusercontent.com/blank-wz/typoraimage/main/images/2022/03/23/0ec74a0c846616a4f9146e766dad5bc3-6-c9247b.png)







# 四, 注解和反射



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

上下文切换

上下文切换指的是[内核](https://baike.baidu.com/item/内核/108410?fromModule=lemma_inlink)（操作系统的核心）在[CPU](https://baike.baidu.com/item/CPU/120556?fromModule=lemma_inlink)上对进程或者线程进行切换。上下文切换过程中的信息被保存在[进程控制块](https://baike.baidu.com/item/进程控制块/7205297?fromModule=lemma_inlink)（PCB-Process Control Block）中。PCB又被称作切换帧（SwitchFrame）。上下文切换的信息会一直被保存在CPU的内存中，直到被再次使用。



## 5.2 并发编程 - 原子性, 可见性 和 有序性

1. 原子性

   即一个操作或多个操作, 要么全部执行并且执行的过程不会被任何因素打断, 要么所有的操作都不执行 (跟事务的原子性类似)

2. 可见性

   可见性是指多个线程访问同一个变量时, 一个线程修改了这个变量的值, 其他线程能够立即看得到修改后的值

3. 有序性

   即程序的执行顺序按照代码的先后顺序执行



## 5.3 线程常用方法

#### 第一组

1. setName // 设置线程名称
2. getName // 返回该线程的名称
3. start // 使该线程开始执行; Java 虚拟机底层调用该线程的 start0 方法
4. run // 调用该线程对象 run 方法
5. setPriority // 更改线程的优先级
6. getPriority // 获取线程的优先级
7. sleep // 在指定的毫秒类让当前正在执行的线程休眠 (暂停执行)
8. interrupt // 中断线程

#### 第二组

1. yield // 线程的礼让. 让出cpu, 让其他线程执行, 但礼让的时间不确定, 所以也不一定礼让成功
2. join // 线程插队. 插队的线程一旦插队成功, 则肯定先执行完插队的线程所有的任务

#### 第三组

setDaemon(true)

1. 用户线程: 也叫工作线程, 当线程的任务执行完或通知方式结束
2. 守护线程: 一般是为工作线程服务的, 当所有的用户线程结束, 守护线程自动结束
3. 常见的守护线程: 垃圾回收机制

getState

​	获取线程状态

````java
public enum State {
    /**
     * Thread state for a thread which has not yet started.
     */
    NEW,
    /**
     * Thread state for a runnable thread.  A thread in the runnable
     * state is executing in the Java virtual machine but it may
     * be waiting for other resources from the operating system
     * such as processor.
     */
    RUNNABLE,
    /**
     * Thread state for a thread blocked waiting for a monitor lock.
     * A thread in the blocked state is waiting for a monitor lock
     * to enter a synchronized block/method or
     * reenter a synchronized block/method after calling
     * {@link Object#wait() Object.wait}.
     */
    BLOCKED,
    /**
     * Thread state for a waiting thread.
     * A thread is in the waiting state due to calling one of the
     * following methods:
     * <ul>
     *   <li>{@link Object#wait() Object.wait} with no timeout</li>
     *   <li>{@link #join() Thread.join} with no timeout</li>
     *   <li>{@link LockSupport#park() LockSupport.park}</li>
     * </ul>
     *
     * <p>A thread in the waiting state is waiting for another thread to
     * perform a particular action.
     *
     * For example, a thread that has called <tt>Object.wait()</tt>
     * on an object is waiting for another thread to call
     * <tt>Object.notify()</tt> or <tt>Object.notifyAll()</tt> on
     * that object. A thread that has called <tt>Thread.join()</tt>
     * is waiting for a specified thread to terminate.
     */
    WAITING,
    /**
     * Thread state for a waiting thread with a specified waiting time.
     * A thread is in the timed waiting state due to calling one of
     * the following methods with a specified positive waiting time:
     * <ul>
     *   <li>{@link #sleep Thread.sleep}</li>
     *   <li>{@link Object#wait(long) Object.wait} with timeout</li>
     *   <li>{@link #join(long) Thread.join} with timeout</li>
     *   <li>{@link LockSupport#parkNanos LockSupport.parkNanos}</li>
     *   <li>{@link LockSupport#parkUntil LockSupport.parkUntil}</li>
     * </ul>
     */
    TIMED_WAITING,
    /**
     * Thread state for a terminated thread.
     * The thread has completed execution.
     */
    TERMINATED;
}
````





## 5.4 线程的生命周期

![](https://raw.githubusercontent.com/blank-wz/typoraimage/main/images/2022/09/17/0154715f774159688677b953ca62e2cc-image-20220917223321098-34ed9c.png)





## 5.5 线程同步

### 5.5.1 线程同步机制

1. 在多线程编程, 一些敏感数据不允许被多个线程同时访问, 此时就是用同步访问技术, 保证数据在任何同一时刻, 最多有一个线程访问, 以保证数据的完整性
2. 也可以这样理解: 线程同步, 即当有一个线程在对内存进行操作时, 其他线程都不可以对这个内存地址进行操作, 直到该线程完成操作, 其他线程才能对内存地址进行操作



### 5.5.2 同步具体方法 - synchronized

1. 同步代码块

   synchronized (对象) { // 得到对象的锁, 才能操作同步代码

   ​		// 需要被同步代码;

   }

2. synchronized 还可以放在方法声明中, 表示整个方法 为同步方法

   public synchronized void m(String name) {

   ​		// 需要被同步代码;

   }



### 5.5.3 synchronized 与 Lock 的区别

| 区别 | synchronized           | Lock                                |
| ---- | ---------------------- | ----------------------------------- |
| 1    | 关键字                 | 接口                                |
| 2    | 能锁住方法和代码库块   | 只能锁住代码块                      |
| 3    | 自动释放锁             | 必须手动调用unlock()方法            |
| 4    | 不能知道线程是否拿到锁 | 可以通过tryLock()知道线程是否拿到锁 |
| 5    | 读, 写操作阻塞         | 可以使用写锁, 提高多线程读效率      |
| 6    | 非公平锁               | 通过构造方法指定是公平锁/非公平锁   |



### 5.5.4 sleep() 与 wait() 的区别

| 区别 |              | sleep                          | wait                             |
| ---- | ------------ | ------------------------------ | -------------------------------- |
| 1    | 属于哪个类   | Thread 的静态方法              | Object 的成员方法                |
| 2    | 是否释放锁   | 暂停当前线程执行, 但是不释放锁 | 暂停当前线程执行, 但是释放锁     |
| 3    | 执行地方     | 可以在任何地方执行             | 只能在同步代码块或同步方法中执行 |
| 4    | 是否自动唤醒 | 只有设置的时间到了才会醒       | 可以随时唤醒                     |



## 5.6 互斥锁

注意事项和细节

1. 同步方法如果没有使用 static 修饰: 默认锁对象为 this
2. 如果方法使用 static 修饰, 默认锁对象为 当前类.class
3. 实现的落地步骤
   + 需要先分析上锁的代码
   + 选择同步代码块或同步方法
   + 要求多个线程的锁对象为同一个即可



## 5.7 线程的死锁

### 5.7.1 基本介绍

多个线程都占用了对方的锁资源, 但不肯相让, 导致了死锁, 在编程时是一定要避免死锁的发生



## 5.8 释放锁

+ 下面操作会释放锁
  1. 当前线程的同步方法, 同步代码块执行结束
  2. 当前线程在同步代码块, 同步方法中遇到 break, retrue
  3. 当前线程在同步代码块, 同步方法中出现未处理的 Error 或 Exception, 导致异常结束
  4. 当前线程在同步代码快, 同步方法中执行了线程对象的 **wait()** 方法, 当前线程暂停, 并释放锁

+ 下面操作不会释放锁

  1. 线程执行在同步代码块, 同步方法时, 程序调用 **Thread.sleep()**, Thread.yield() 方法暂停当前线程的执行时, 不会释放锁

  2. 线程执行在同步代码块, 同步方法时, 其他线程调用了该线程的 suspend() 方法将该线程挂起, 该线程不会释放锁

     **提示: 应尽量避免使用 suspend() 和 resume() 来控制线程, 方法不再推荐使用**







# 六, 线程池



## 6.1 什么是线程池

线程池（ThreadPool）是一种基于池化思想管理和使用线程的机制。它是将多个线程预先存储在一个“池子”内，当有任务出现时可以避免重新创建和销毁线程所带来性能开销，只需要从“池子”内取出相应的线程执行对应的任务即可。

池化思想在计算机的应用也比较广泛，比如以下这些：

- 内存池(Memory Pooling)：预先申请内存，提升申请内存速度，减少内存碎片。
- 连接池(Connection Pooling)：预先申请数据库连接，提升申请连接的速度，降低系统的开销。
- 实例池(Object Pooling)：循环使用对象，减少资源在初始化和释放时的昂贵损耗。



## 6.2 线程池的优点

1. 降低资源消耗：通过池化技术重复利用已创建的线程，降低线程创建和销毁造成的损耗。
2. 提高响应速度：任务到达时，无需等待线程创建即可立即执行。
3. 提高线程的可管理性：线程是稀缺资源，如果无限制创建，不仅会消耗系统资源，还会因为线程的不合理分布导致资源调度失衡，降低系统的稳定性。使用线程池可以进行统一的分配、调优和监控。
4. 提供更多更强大的功能：线程池具备可拓展性，允许开发人员向其中增加更多的功能。比如延时定时线程池ScheduledThreadPoolExecutor，就允许任务延期执行或定期执行。



## 6.3 线程池的使用

线程池的创建方法总共有 7 种，但总体来说可分为 2 类：

- 一类是通过 ThreadPoolExecutor 创建的线程池；
- 另一个类是通过 Executors 创建的线程池。

![img](https://raw.githubusercontent.com/blank-wz/typoraimage/main/images/2022/08/31/979f9b44b5afab3902e0d22d9a4a14f4-924700-20201218091637063-1692359064-5aabfc.png)

线程池的创建方式总共包含以下 7 种（其中 6 种是通过 Executors 创建的，1 种是通过ThreadPoolExecutor 创建的）

1. Executors.newFixedThreadPool：创建一个固定大小的线程池，可控制并发的线程数，超出的线程会在队列中等待；
2. Executors.newCachedThreadPool：创建一个可缓存的线程池，若线程数超过处理所需，缓存一段时间后会回收，若线程数不够，则新建线程；
3. Executors.newSingleThreadExecutor：创建单个线程数的线程池，它可以保证先进先出的执行顺序；
4. Executors.newScheduledThreadPool：创建一个可以执行延迟任务的线程池；
5. Executors.newSingleThreadScheduledExecutor：创建一个单线程的可以执行延迟任务的线程池；
6. Executors.newWorkStealingPool：创建一个抢占式执行的线程池（任务执行顺序不确定）【JDK 1.8 添加】。
7. ThreadPoolExecutor：最原始的创建线程池的方式，它包含了 7 个参数可供设置，后面会详细讲。

> 单线程池的意义从以上代码可以看出 newSingleThreadExecutor 和 newSingleThreadScheduledExecutor 创建的都是单线程池，那么单线程池的意义是什么呢？答：虽然是单线程池，但提供了工作队列，生命周期管理，工作线程维护等功能。

### ① FixedThreadPool

创建一个固定大小的线程池，可控制并发的线程数，超出的线程会在队列中等待。

````java
public static void fixedThreadPool() {
    // 创建线程池
    ExecutorService threadPool = Executors.newFixedThreadPool(2);
    // 执行任务
    for (int i = 0; i < 4; i++) {
        threadPool.execute(() -> {
            System.out.println("任务被执行,线程:" + Thread.currentThread().getName());
        });
    }
}
````

![image-20220831211338366](https://raw.githubusercontent.com/blank-wz/typoraimage/main/images/2022/08/31/b1eecd164401f2c2d2602ca54d336178-image-20220831211338366-dd765d.png)

### ② CachedThreadPool

创建一个可缓存的线程池，若线程数超过处理所需，缓存一段时间后会回收，若线程数不够，则新建线程。

````java
public static void cachedThreadPool() {
    // 创建线程池
    ExecutorService threadPool = Executors.newCachedThreadPool();
    // 执行任务
    for (int i = 0; i < 10; i++) {
        final int index = i;
        threadPool.execute(() -> {
            System.out.println(index + ":" + "任务被执行,线程:" + Thread.currentThread().getName());
            try {
                TimeUnit.SECONDS.sleep(1);
            } catch (InterruptedException e) {
            }
        });
    }
}
````

![image-20220831212330858](https://raw.githubusercontent.com/blank-wz/typoraimage/main/images/2022/08/31/66f528c8239e9242aff2ef3fd33ca3f5-image-20220831212330858-5e2f4e.png)



### ③ SingleThreadExecutor

创建单个线程数的线程池，它可以保证先进先出的执行顺序。

````java
public static void singleThreadExecutor() {
    // 创建线程池
    ExecutorService threadPool = Executors.newSingleThreadExecutor();
    // 执行任务
    for (int i = 0; i < 10; i++) {
        final int index = i;
        threadPool.execute(() -> {
            System.out.println(index + ":任务被执行");
            try {
                TimeUnit.SECONDS.sleep(1);
            } catch (InterruptedException e) {
            }
        });
    }
}
````

![image-20220831212448650](https://raw.githubusercontent.com/blank-wz/typoraimage/main/images/2022/08/31/54f77ada409f67f8e84cf6fc90c2b386-image-20220831212448650-18ee18.png)



### ④ ScheduledThreadPool

创建一个可以执行延迟任务的线程池。

````java
public static void scheduledThreadPool() {
    // 创建线程池
    ScheduledExecutorService threadPool = Executors.newScheduledThreadPool(5);
    // 添加定时执行任务(1s 后执行)
    System.out.println("添加任务,时间:" + new Date());
    threadPool.schedule(() -> {
        System.out.println("任务被执行,时间:" + new Date());
        try {
            TimeUnit.SECONDS.sleep(1);
        } catch (InterruptedException e) {
        }
    }, 1, TimeUnit.SECONDS);
}
````

![image-20220831211804011](https://raw.githubusercontent.com/blank-wz/typoraimage/main/images/2022/08/31/00fa90c2bd2a024f46f10143daa43ada-image-20220831211804011-dfcf97.png)

从上述结果可以看出，任务在 1 秒之后被执行了，符合我们的预期。



### ⑤ SingleThreadScheduledExecutor

创建一个单线程的可以执行延迟任务的线程池。

````java
public static void SingleThreadScheduledExecutor() {
    // 创建线程池
    ScheduledExecutorService threadPool = Executors.newSingleThreadScheduledExecutor();
    // 添加定时执行任务(2s 后执行)
    System.out.println("添加任务,时间:" + new Date());
    threadPool.schedule(() -> {
        System.out.println("任务被执行,时间:" + new Date());
        try {
            TimeUnit.SECONDS.sleep(1);
        } catch (InterruptedException e) {
        }
    }, 2, TimeUnit.SECONDS);
}
````

![image-20220831212654853](https://raw.githubusercontent.com/blank-wz/typoraimage/main/images/2022/08/31/cc693127c77b7570d779ef10e5daa246-image-20220831212654853-921081.png)



### ⑥ newWorkStealingPool

创建一个抢占式执行的线程池（任务执行顺序不确定），注意此方法只有在 JDK 1.8+ 版本中才能使用。

````java
public static void workStealingPool() {
    // 创建线程池
    ExecutorService threadPool = Executors.newWorkStealingPool();
    // 执行任务
    for (int i = 0; i < 10; i++) {
        final int index = i;
        threadPool.execute(() -> {
            System.out.println(index + " 被执行,线程名:" + Thread.currentThread().getName());
        });
    }
    // 确保任务执行完成
    while (!threadPool.isTerminated()) {
    }
}
````

![image-20220831212851798](https://raw.githubusercontent.com/blank-wz/typoraimage/main/images/2022/08/31/cffab791556f806efcc40febd1c8076c-image-20220831212851798-09bf76.png)

从上述结果可以看出，任务的执行顺序是不确定的，因为它是抢占式执行的



### ⑦ ThreadPoolExecutor

最原始的创建线程池的方式，它包含了 7 个参数可供设置。

````java
public static void myThreadPoolExecutor() {
    // 创建线程池
    ThreadPoolExecutor threadPool = new ThreadPoolExecutor(5, 10, 100, TimeUnit.SECONDS, new LinkedBlockingQueue<>(10));
    // 执行任务
    for (int i = 0; i < 10; i++) {
        final int index = i;
        threadPool.execute(() -> {
            System.out.println(index + " 被执行,线程名:" + Thread.currentThread().getName());
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        });
    }
}
````

![image-20220831213607859](https://raw.githubusercontent.com/blank-wz/typoraimage/main/images/2022/08/31/a41254781c9686462069c906c24f206b-image-20220831213607859-298808.png)



## 6.4 ThreadPoolExecutor 参数介绍

````java
public ThreadPoolExecutor(int corePoolSize,
                          int maximumPoolSize,
                          long keepAliveTime,
                          TimeUnit unit,
                          BlockingQueue<Runnable> workQueue) {
    this(corePoolSize, maximumPoolSize, keepAliveTime, unit, workQueue,
         Executors.defaultThreadFactory(), defaultHandler);
}
````

7 个参数代表的含义如下：

#### 参数 1：corePoolSize

核心线程数，线程池中始终存活的线程数。

#### 参数 2：maximumPoolSize

最大线程数，线程池中允许的最大线程数，当线程池的任务队列满了之后可以创建的最大线程数。

#### 参数 3：keepAliveTime

最大线程数可以存活的时间，当线程中没有任务执行时，最大线程就会销毁一部分，最终保持核心线程数量的线程。

#### 参数 4：unit:

单位是和参数 3 存活时间配合使用的，合在一起用于设定线程的存活时间 ，参数 keepAliveTime 的时间单位有以下 7 种可选：

- TimeUnit.DAYS：天
- TimeUnit.HOURS：小时
- TimeUnit.MINUTES：分
- TimeUnit.SECONDS：秒
- TimeUnit.MILLISECONDS：毫秒
- TimeUnit.MICROSECONDS：微妙
- TimeUnit.NANOSECONDS：纳秒

#### 参数 5：workQueue

一个阻塞队列，用来存储线程池等待执行的任务，均为线程安全，它包含以下 7 种类型：

- ArrayBlockingQueue：一个由数组结构组成的有界阻塞队列。
- LinkedBlockingQueue：一个由链表结构组成的有界阻塞队列。
- SynchronousQueue：一个不存储元素的阻塞队列，即直接提交给线程不保持它们。
- PriorityBlockingQueue：一个支持优先级排序的无界阻塞队列。
- DelayQueue：一个使用优先级队列实现的无界阻塞队列，只有在延迟期满时才能从中提取元素。
- LinkedTransferQueue：一个由链表结构组成的无界阻塞队列。与SynchronousQueue类似，还含有非阻塞方法。
- LinkedBlockingDeque：一个由链表结构组成的双向阻塞队列。

较常用的是 LinkedBlockingQueue 和 Synchronous，线程池的排队策略与 BlockingQueue 有关。

#### 参数 6：threadFactory

线程工厂，主要用来创建线程，默认为正常优先级、非守护线程。

#### 参数 7：handler

拒绝策略，拒绝处理任务时的策略，系统提供了 4 种可选：

- AbortPolicy：拒绝并抛出异常。
- CallerRunsPolicy：使用当前调用的线程来执行此任务。
- DiscardOldestPolicy：抛弃队列头部（最旧）的一个任务，并执行当前任务。
- DiscardPolicy：忽略并抛弃当前任务。

默认策略为 AbortPolicy。



## 6.5 线程池的执行流程

ThreadPoolExecutor 关键节点的执行流程如下：

- 当线程数小于核心线程数时，创建线程。
- 当线程数大于等于核心线程数，且任务队列未满时，将任务放入任务队列。
- 当线程数大于等于核心线程数，且任务队列已满：若线程数小于最大线程数，创建线程；若线程数等于最大线程数，抛出异常，拒绝任务。

线程池的执行流程如下图所示：

![img](https://img2020.cnblogs.com/blog/924700/202012/924700-20201218092732932-3311211.png)

 

## 6.6 线程拒绝策略

我们来演示一下 ThreadPoolExecutor 的拒绝策略的触发，我们使用 DiscardPolicy 的拒绝策略，它会忽略并抛弃当前任务的策略，实现代码如下：

```
public static void main(String[] args) {
    // 任务的具体方法
    Runnable runnable = new Runnable() {
        @Override
        public void run() {
            System.out.println("当前任务被执行,执行时间:" + new Date() +
                               " 执行线程:" + Thread.currentThread().getName());
            try {
                // 等待 1s
                TimeUnit.SECONDS.sleep(1);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    };
    // 创建线程,线程的任务队列的长度为 1
    ThreadPoolExecutor threadPool = new ThreadPoolExecutor(1, 1,
                                                           100, TimeUnit.SECONDS, new LinkedBlockingQueue<>(1),
                                                           new ThreadPoolExecutor.DiscardPolicy());
    // 添加并执行 4 个任务
    threadPool.execute(runnable);
    threadPool.execute(runnable);
    threadPool.execute(runnable);
    threadPool.execute(runnable);
}
```

我们创建了一个核心线程数和最大线程数都为 1 的线程池，并且给线程池的任务队列设置为 1，这样当我们有 2 个以上的任务时就会触发拒绝策略，执行的结果如下图所示

![img](https://img2020.cnblogs.com/blog/924700/202012/924700-20201218092822226-66500295.png)

 

## 6.7 自定义拒绝策略

除了 Java 自身提供的 4 种拒绝策略之外，我们也可以自定义拒绝策略，示例代码如下：

```
public static void main(String[] args) {
    // 任务的具体方法
    Runnable runnable = new Runnable() {
        @Override
        public void run() {
            System.out.println("当前任务被执行,执行时间:" + new Date() +
                               " 执行线程:" + Thread.currentThread().getName());
            try {
                // 等待 1s
                TimeUnit.SECONDS.sleep(1);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    };
    // 创建线程,线程的任务队列的长度为 1
    ThreadPoolExecutor threadPool = new ThreadPoolExecutor(1, 1,
                                                           100, TimeUnit.SECONDS, new LinkedBlockingQueue<>(1),
                                                           new RejectedExecutionHandler() {
                                                               @Override
                                                               public void rejectedExecution(Runnable r, ThreadPoolExecutor executor) {
                                                                   // 执行自定义拒绝策略的相关操作
                                                                   System.out.println("我是自定义拒绝策略~");
                                                               }
                                                           });
    // 添加并执行 4 个任务
    threadPool.execute(runnable);
    threadPool.execute(runnable);
    threadPool.execute(runnable);
    threadPool.execute(runnable);
}
```

![img](https://img2020.cnblogs.com/blog/924700/202012/924700-20201218092918213-1054849701.png)

 

## 6.8 究竟选用哪种线程池？

经过以上的学习我们对整个线程池也有了一定的认识了，那究竟该如何选择线程池呢？

我们来看下阿里巴巴《Java开发手册》给我们的答案：

> 【强制】线程池不允许使用 Executors 去创建，而是通过 ThreadPoolExecutor 的方式，这样的处理方式让写的同学更加明确线程池的运行规则，规避资源耗尽的风险。
>
> 说明：Executors 返回的线程池对象的弊端如下：
>
> 1） FixedThreadPool 和 SingleThreadPool：允许的请求队列长度为 Integer.MAX_VALUE，可能会堆积大量的请求，从而导致 OOM。
>
> 2）CachedThreadPool：允许的创建线程数量为 Integer.MAX_VALUE，可能会创建大量的线程，从而导致 OOM。

所以综上情况所述，我们推荐使用 ThreadPoolExecutor 的方式进行线程池的创建，因为这种创建方式更可控，并且更加明确了线程池的运行规则，可以规避一些未知的风险。

本文我们介绍了线程池的 7 种创建方式，其中最推荐使用的是 ThreadPoolExecutor 的方式进行线程池的创建，ThreadPoolExecutor 最多可以设置 7 个参数，当然设置 5 个参数也可以正常使用，ThreadPoolExecutor 当任务过多（处理不过来）时提供了 4 种拒绝策略，当然我们也可以自定义拒绝策略.
