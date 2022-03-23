# 一, Java Virtual Machine 的好处

1. 实现跨平台
2. 自动内存够管理, 垃圾回收功能
3. 数组下标越界检查
4. 多态






# 二, JVM 内存结构

![1](https://raw.githubusercontent.com/blank-wz/typoraimage/main/images/2022/03/22/1401832aa4fa7ee2a9bbfde8bc558940-1-0de2b8.jpg)



### 2.1 程序计数器 (Program Counter Register)

#### 定义和作用

+ Java 源代码 --> 二进制字节码(JVM指令) --> 程序计数器 --> 解释器 --> 机器码 --> CPU

+ 作用: 记住下一条 JVM 指令的执行地址, 依赖于 CPU 的 **寄存器**

+ 特点:
  1. 线程私有 (每个线程都有自己的程序计数器)
  2. 不存在内存溢出



### 2.2 虚拟机栈 (JVM Stacks)

#### 2.2.1 定义

栈: 线程运行时需要的内存空间 (-Xss size     Linux, macOS, Oracle 默认为 1024KB,Windows:depends on virtual memory)

栈帧: 每个方法需要的内存 (如果方法内局部变量没有逃离方法的作用范围, 那么局部变量是线程安全的)

每个线程只能有一个活动栈帧, 对应着当前正在执行的那个方法

出栈: 释放栈帧的内存

#### 2.2.2 栈内存溢出 (StackOverflowError)

1. 栈帧过多 (例: 递归调用没有给合理的结束条件)
2. 栈帧过大 (不容易出现, 一个栈大小在 1M 左右, 一个 int 才占4个字节)

#### 2.2.3 线程运行诊断

+ 案例1: CPU 占用过高

  定位: 

  1. 用 top 定位哪个进程对cpu占用过高
  2. ps H -eo pid,**tid**,%cpu | grep 进程id (用ps命令进一步定位是那个线程引起的cpu占用过高)
  3. jstack 进程id (可列出所有的线程)
     + 可以根据线程id找到有问题的线程, 进一步定位到问题代码的源码行号 (需要将十进制的线程id换算成16进制去找)



### 2.3 本地方法栈 (Native Method Statics)

不是由 Java 代码编写的代码, 由C/C++编写的本地方法, 从而 Java 可以调用这些本地方法间接的操作系统底层

Object   native修饰的方法

```
protected native Object clone() throws CloneNotSupportedException;
public final native void notify();
public final native void wait(long timeout) throws InterruptedException;
```



### 2.4 堆 (Heap)

#### 2.4.1 定义

+ 通过 new 关键字, 创建对象都会使用堆内存
+ 特点
  1. 它是线程共享的, 堆中对象都需要考虑线程安全的问题
  2. 有垃圾回收机制

#### 2.4.2 堆内存溢出 (OutOfMemoryError: Java heap space)

对象被当做垃圾回收的条件 : 没有程序在使用



### 2.5 方法区

#### 2.5.1 定义



#### 2.5.2 方法区内存溢出

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



#### 2.5.3 运行时常量池

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



#### 2.5.4 StringTable 特性

1. 常量池中的字符串仅是符号, 第一次用到时才变为对象
2. 利用串池机制, 来避免重复创建字符串对象
3. 字符串变量的拼接原理是 StringBuilder (JDK1.8)      在堆中
4. 字符串常量的拼接原理是 编译器优化   在常量池中
5. 可以使用 intern 方法, 主动将串池中还没有的字符串对象放入串池