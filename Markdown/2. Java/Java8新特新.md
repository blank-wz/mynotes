# 一, 函数式接口

## 1.1 定义

只有一个抽象方法的接口, 称之为函数式接口, 可以使用注解 **@FunctionalInterface** 修饰, 可以检查是否是函数式接口



# 二, Lambda表达式

````java
public interface Calculator {
    double sum(int a, int b);
}

public class Demo01 {
    public static void main(String[] args) {
        method((x, y) -> x + y);
        // Lambda 表达式: (x, y) -> x + y
        // method 方法需要一个Calculator接口类型的参数
        // Lambda 表达式就是充当了Calculator接口类型的参数
        // 初步理解
        // 1. Lambda 表达式前面的小括号,其实就是接口抽象方法的小括号
        // 2. 箭头代表拿着小括号的数据做什么事情, 就一个指向的动作
        // 3. 箭头后面就代表拿到了参数之后做什么事情
        // Lamda 表达式的语义本身就代表了怎么做这件事, 没有对象的概念在里面 (更加简单直观)
    }

    public static void method(Calculator calculator) {
        double sum = calculator.sum(100, 200);
        System.out.println("结果是: " + sum);
    }
}
````

## 2.1 条件

+ Lambda 表达式一定要有函数式接口的推断环境
  1. 要么通过方法的参数类型来确定是哪个函数式接口
  2. 要么通过赋值操作来确定是哪个函数式接口



## 2.2 基础语法

+ Java8中引入了一个新的操作符 "->", 该操作符称为箭头操作符或Lambda操作符, 箭头操作符将Lambda表达式拆分成两部分:
  + 左侧: Lambda 表达式的参数列表
  + 右侧: Lambda 表达式中所需执行的功能, 即Lambda体

+ 语法格式1: 无参数, 无返回值

  ```` java
  // () -> System.out.println("Hello Lambda!");
  
  // 举例
  public class LambdaDemo1 {
      @Test
      public void demo1() {
          [final] int num = 0; // JDK1.7前,必须是final,JDK1.8默认加上
          
          Runnable r = new Runnable() {
              @Override
              public void run() {
                  System.out.println("Hello Lambda!");
              }
          };
          r.run();
          
          // Lambda 简写为
          Runnable r1 = () -> System.out.println("Hello Lambda!");
          r1.run();
      }
  }
  ````

+ 语法格式2.1: 有一个参数, 无返回值

  ````java
  // (o) -> System.out.println(o)
  
  // 举例
  public class LambdaDemo2 {
      public static void main(String[] args) {
          Consumer con = new Consumer() {
              @Override
              public void accept(Object o) {
                  System.out.println(o);
              }
          };
          con.accept("我不是Lambda");
  		
          // Lambda 简写
          Consumer con1 = (o) -> System.out.println(o);
          con1.accept("我是Lambda");
      }
  }
  ````

+ 语法格式2.2: 只有一个参数,无返回值, () 可以不写

  ````java
  // o -> System.out.println(o)
  ````

+ 语法格式3: 有两个参数, 且Lambda体中多条语句

  ````java
  Comparator<Integer> com1 = (o1, o2) -> {
              System.out.println("多个参数,有返回值的Lambda");
              return Integer.compare(o1, o2);
          };
  
  // 举例
  public class LambdaDemo3 {
      public static void main(String[] args) {
          Comparator<Integer> com = new Comparator<Integer>() {
              @Override
              public int compare(Integer o1, Integer o2) {
                  System.out.println("多个参数,有返回值");
                  return Integer.compare(o1, o2);
              }
          };
          System.out.println(com.compare(1, 2));
  		
          // Lambda 简写
          Comparator<Integer> com1 = (o1, o2) -> {
              System.out.println("多个参数,有返回值的Lambda");
              return Integer.compare(o1, o2);
          };
          System.out.println(com.compare(1, 2));
      }
  }
  ````

+ 语法格式4: 有返回值, 若 Lambda 体中只有一条语句, return 和 大括号都可以省略不写

  ````Java
  Comparator<Integer> com1 = (o1, o2) -> Integer.compare(o1, o2);
  ````

+ 语法格式5: Lambda 表达式参数列表的参数列表的数据类型可以省略不写,因为JVM编译器通过上下文推断出数据类型, 即**"类型推断"**

  ````Java
  Comparator<Integer> com = (Integer x, Integer y) -> Integer.compare(x, y);
  
  // 简化:去掉数据类型有JVM执行类型推断
  Comparator<Integer> com = (x, y) -> Integer.compare(x, y);
  
  // JDK1.8之后
  // 拓展1: 
  List<String> list = new ArrayList<String>();
  List<String> list = new ArrayList<>(); // 类型推断
  
  // 拓展2:
  show(new HashMap<>()); // 类型推断
  public void show(Map<String, Integer> map) {
      ...
  } 
  ````

  

## 2.3 总结

上联: **左右遇一括号省**	左边省包参数的小括号, 右边省包方法体的大括号 (有返回值可省 return)

下联: **左侧推断类型省**	

横批: 能省则省



# 三, Java内置四大核心函数式接口

| 函数式接口                   | 参数类型 | 返回类型 | 用途                                                         |
| ---------------------------- | -------- | -------- | ------------------------------------------------------------ |
| Consumer<T> <br>消费型接口   | T        | void     | 对类型为T的对象应用操作,包含方法: <br>void accept(T t)       |
| Supplier<T><br>供给型接口    | 无       | T        | 返回类型为T的对象,包含方法:<br>T get()                       |
| Function<T, R><br>函数型接口 | T        | R        | 对类型为T的对象应用操作,并返回结果.结果是R类型对象,包含方法:<br>R apply(T t) |
| Predicate<T><br>断定型接口   | T        | boolean  | 确定类型为T的对象是否满足某约束,并返回Boolean值,包含方法:<br>boolean test(T t) |



## 3.1 Consumer 消费型接口

````java
// Consumer 消费型接口
@Test
public void consumer_() {
	happy(10000.0, (money) -> System.out.println("我消费了" + money + "元"));
}

public void happy(double money, Consumer<Double> con) {
	con.accept(money);
}
````



 ## 3.2 Supplier<T> 供给型接口

````java
// Supplier 供给型接口
@Test
public void supplier_() {
    List<Integer> list = getNumList(10, () -> (int) (Math.random() * 100));
    System.out.println(list);
}
public List<Integer> getNumList(int num, Supplier<Integer> supplier) {
    List<Integer> list = new ArrayList<>();
    for (int i = 0; i < num; i++) {
        int n = supplier.get();
        list.add(n);
    }
    return list;
}
````





## 3.3 Function<T, R> 函数型接口

````java
// Function 函数型接口
@Test
public void function_() {
    String newStr = strHandler("\t\t\t 你好世界", (String str) -> {
        return str.trim();
    });
    System.out.println(newStr);
    String newStr1 = strHandler("abcd", (str) -> {
        return str.toUpperCase();
    });
    System.out.println(newStr1);
    String newStr2 = strHandler("你好Lambda", str -> str.substring(2, 8));
    System.out.println(newStr2);
}
public String strHandler(String str, Function<String, String> fun) {
    return fun.apply(str);
}
````



## 3.4 Predicate<T> 断定型接口

````java
// Predicate 断定型接口
@Test
public void predicate_() {
    List<Integer> list = new ArrayList<>();
    Collections.addAll(list, 2, 34, 52, 567, 465, 99, 34, 3);
    List<Integer> newList = filterInteger(list, number -> number > 90);
    System.out.println(newList);
}
public List<Integer> filterInteger(List<Integer> list, Predicate<Integer> pre) {
    List<Integer> newList = new ArrayList<>();
    for (Integer integer : list) {
        if (pre.test(integer)) {
            newList.add(integer);
        }
    }
    return newList;
}
````





# 四, 方法引用与构造器引用

````java
// 在某些场景下, Lamda表达式要做的事情, 其实在另外一个地方已经写过
// 那么此时如果通过Lambda表达式重复编写相同的代码, 就是浪费
// 那么如何才能复用已经存在的方法逻辑

// 如果Lambda表达式需要做的事情, 在另外一个类中已经做过了, 那么就可以直接拿过来替换Lambda
````

## 4.1 方法引用

+ 若 Lambda 体重的内容有方法已经实现了, 我们可以使用 "方法引用"

  (可以理解为方法引用是 Lambda 表达式的另外一种表现形式)

+ 主要有三种语法格式

  + 对象 :: 实例方法
  + 类 :: 静态方法名
  + 类 :: 实例方法

+ 注意:

  1. Lambda 体中调用方法的参数列表与返回值类型, 要与函数式接口中抽象方法的参数列表和返回值保持一致!
  2. 若 Lambda 参数列表中的第一参数实例方法的调用者, 第二个参数是实例方法的参数时, 可以使用 ClassName :: method



## 4.2 构造器引用

+ 格式

  ClassName :: new

+ 注意:

  需要调用的构造器的参数列表与函数式接口中的抽象方法的参数列表保持一致



## 4.3 数组引用

+ 格式

  Type[] :: new





# 五, Stream API

## 5.1 定义

流 (Stream) 是数据渠道, 用于操作数据源 (集合, 数组等) 所产生的元素序列

**集合讲的是数据, 流讲的是计算**

注意:

1. Stream 自己不会存储元素
2. Stream 不会改变源对象, 相反, 他们会返回一个持有结果的新 Stream
3. Stream 操作是延迟执行的, 这意味着他们会等到需要结果的时候才执行



## 5.2 Stream 的操作三个步骤

### 5.2.1 创建 Stream

   一个数据源 (如: 集合, 数组), 获取一个流

   ````java
   // 创建 Stream
   @Test
   public void createStream() {
       // 1. 根据集合获取流 --- 可以通过 Collection 系列集合提供的 stream() 或 parallelStream()
       List<String> list = new ArrayList<>();
       Stream<String> stream1 = list.stream();
       // 2.1 根据数组获取流 --- 通过 Arrays 中的静态方法 stream() 获取数组流
       String[] strings = new String[10];
       Stream<String> stream2 = Arrays.stream(strings);
       // 2.2 根据数组获取流 --- 通过 Stream 类中的静态方法 of() 获取数组的流
       Stream<String> stream3 = Stream.of("aa", "bb", "cc");
       // 3. 创建无限流
       // 迭代
       Stream<Integer> stream4 = Stream.iterat;
       stream4.limit(10).forEach(System.out::println);
       // 生成
       Stream.generate(Math::random)
               .limit(10)
               .forEach(System.out::println);
   }
   ````

   

### 5.2.2 中间操作

   一个中间操作链, 对数据源的数据进行处理

   + 多个中间操作可以连接起来形成一个流水线, 除非流水线上触发终止操作, 否则中间操作不会执行任何的处理!
   + 终止操作时一次性全部处理, 称为 "惰性求值"

   **① 筛选与切片**

   | 方法                | 描述                                                         |
   | ------------------- | ------------------------------------------------------------ |
   | filter(Predicate p) | 接受 Lambda, 从流中排除某些元素                              |
   | distinct()          | 筛选, 通过流所生成元素的 **hashCode() 和 equals()** 去除重复元素 |
   | limit(long maxSize) | 截断流, 使其元素不超过给定数量                               |
   | skip(long n)        | 跳过元素, 返回一个扔掉了前 n 个元素的流, 若流中元素不足 n 个, 则返回一个空流, 与 limit(n) 互补 |

   ````java
   @Test
   public void middleAndEnd() {
       List<Integer> list = Arrays.asList(11, 11, 22, 22, 5, 8, 86, 67, 79);
       list.stream()
               .filter(e -> {
                   return e > 10;
               })
               .distinct()
               .skip(2)
               .limit(3)
               .forEach(System.out::println);
   }
   ````

   

   **② 映射**

   ````java
   /*
    * 获取流之后, 可以使用映射方法: map(用于转换的Lambda表达式)
    * 映射, 就是将一个对象转换成为另一对象, 把老对象映射到新对象上
    * 
    * "赵丽颖, 98" 转换成	"98"	将一个长字符串转换成一个短字符串
    * "98"        转换成     98	  将一个字符串转换成一个数字
    */
   ````

   

   

   **③ 排序**

   ````java
   
   ````

   

### 5.2.3 终止操作 (终端操作)

   一个终止操作, 执行中间操作链, 并产生结果

   ````java
   // forEach(System.out::println);
   ````

   **查找与匹配**



## 5.3 并发流

### 5.3.1 概念

如果对流当中的元素, 使用多人同时处理, 这就是 "并发"

如何才能获取 "并发流" (支持并发操作的流)

.parallelStream()

### 5.3.2 注意事项:

+ 使用并发流操作的时候, 到底有几个人进行同时操作呢? 不用管, JDK 自己处理
+ 只要正确使用, 就不会出现多个人强盗同一个元素
+ 如果已经获取了一个普通的流, 那么只要再调用一下 parallel()方法也可会变成并发流



### 5.3.3 获取并发流

+ 直接获取并发流: .parallelStream()
+ 已经获取了普通流, 然后升级成为并发流: .stream().parallel()





# 六, Optional

## 6.1 定义

+ 我们在编写代码的时候出现最多的就是空指针异常, 所以在很多情况下我们需要做各种非空判断, 尤其是对象中的属性还是一个对象的时候, 这种判断会更多, 而过多的判断语句会让我们的代码显得臃肿不堪. 所以在 JDK8 中引入了 Optional, 养成使用 Optional 的习惯后可以写出更优雅的代码来避免空指针异常.

​	

## 6.2 使用

### 6.2.1 创建对象 (ofNullable, of)

Optional 就好像是包装类, 可以把我们的具体数据封装到 Optional 对象内部, 然后我们去使用 Optional 中封装好的方法操作封装进去的数据可以非常优雅的避免空指针异常.

1. 一般使用 **Optional** 的**静态方法 ofNullable** 把数据封装成一个 Optional 对象, 无论传入的参数是否为 null 都不会出现问题

````java
Author author = getAuthor();
Optional<Author> authorOptional = Optional.ofNullable(author);

// ofNullable()方法的源码
public static <T> Optional<T> ofNullable(T value) {
    return value == null ? empty() : of(value);
}
````

​		你可能会觉得还要加一行代码来封装数据比较麻烦, 但是如果改造一下 getAuthor() 方法, 让其返回值就是封装好的 Optional 的话, 我们在使用时就会方便很多.

​		而在实际开发中我们的数据很多是从数据库获取的, Mybatis 从3.5版本支持 Optional 了, 我们可以直接把 dao 方法的返回值类型定义成 Optional 类型, Mybatis 会自己把数据封装成 Optional 对象返回, 封装的过程不需要自己操作.

2. 如果你**确定一个对象不是空**, 则可以使用 **Optional 的静态方法 of** 来把数据封装成 Optional 对象

````java
Author author = new Author();
Optional<Author> authorOptional = Optional.of(author);

// of()方法的源码
public static <T> Optional<T> of(T value) {
    return new Optional<>(value);
}
````

​		如果一个方法的返回值类型是 Optional 类型, 而如果我们经判断发现某次计算得到的返回值为 null, 这个时候需要把 null 封装成 Optional 对象返回, 这是可以使用 Optional 的 **静态方法 empty()** 来进行封装.

````java
Optional.empty();

// empty()方法的源码
private static final Optional<?> EMPTY = new Optional<>();
public static<T> Optional<T> empty() {
    @SuppressWarnings("unchecked")
    Optional<T> t = (Optional<T>) EMPTY;
    return t;
}
````



### 6.2.2 安全消费值 (ifPresent)

+ 我们获取到一个 Optional 对象后肯定需要对其中的数据进行使用, 这时候我们可以使用 **ifPresent** 方法来消费其中的值. 这个方法会判断其中内封装的数据是否为空, 不为空时才会执行具体的消费代码, 这样使用起来就更加安全了.

+ 例如如下写法就优雅的避免了空指针异常

  ````Java
  Optional<Author> authorOptional = Optional.ofNullable(getAuthor());
  authorOptional.ifPresent(author -> System.out.println(author.getName()))
  ````



### 6.2.3 不安全获取值 (get)

+ 如果我们想获取值自己进行处理可以使用 **get()** 方法获取, 但是**不推荐**, 因为当 Optional 内部的数据为空的时候会出现异常 (NoSuchElementException)

  ````java
  // get() 的源码
  private final T value;
  public T get() {
      if (value == null) {
          throw new NoSuchElementException("No value present");
      }
      return value;
  }
  ````

  



### 6.2.4 安全获取值 (orElse, orElseGet, orElseThrow)

​		如果我们期望安全的获取值, 则不推荐使用 get() 方法, 而是使用 Optional 提供的以下方法.

+ **orElse**

  ```java
  int i = xxx.orElse(10);
  
  /**
   * Return the value if present, otherwise return {@code other}.
   */
  public T orElse(T other) {
      return value != null ? value : other;
  }
  ```

+ **orElseGet**

  获取数据并且设置数据为空的默认值, 如果数据不为空就能获取该数据, 如果为空则根据传入的参数创建对象作为默认值返回

  ````java
  Optional<Author> authorOptinal = Optional.ofNullable(getAuthor());
  Author author = authorOptinal.orElseGet(() -> new Author());
  
  // orElseGet 的源码
  /**
   * Return the value if present, otherwise invoke {@code other} and return
   * the result of that invocation.
   */
  public T orElseGet(Supplier<? extends T> other) {
      return value != null ? value : other.get();
  }
  ````

> 0. 写在前面
>    这篇文章的目的是为了说明orElse可能导致NullPointerException，当orElse的参数是间接计算得来的时候。虽然这种说法有点牵强（因为并不是orElse导致了空指针异常），但是使用orElseGet确实可以避免这种情况。
>
>
> 1. orElse与orElseGet介绍与使用
>     一般而言，我们使用orElse或是orElseGet均是来做使用Optional时的收尾工作，比如：
>
>     ````java
>     Optional.ofNullable(aVariableThatMayBeIsNull)
>          // 一些其它操作，比如 .map()
>         .orElse(defaultValue); // 或是 .orElseGet(aFunctionUsedToComputeDefaultValue)
>     ````
>
>     两者的明显（也是唯一）区别是前者需要传递的参数是一个值（通常是为空时的默认值），后者传递的是一个函数。我们看一下源代码：
>
>     ````java
>      /**
>        *Return the value if present, otherwise return {@code other}.
>        */
>       public T orElse(T other) {
>       return value != null ? value : other;
>       }
>     
>       /**
>        *Return the value if present, otherwise invoke {@code other} and return
>        *the result of that invocation.
>        */
>       public T orElseGet(Supplier<? extends T> other) {
>       return value != null ? value : other.get();
>       }
>     ````
>
>     简单解释为，我们使用Optional包装的变量如果不为空，返回它本身，否则返回我们传递进去的值。orElseGet参数为Supplier接口，它是一个函数式接口，它的形式是这样的：() -> { return computedResult }，即入参为空，有返回值（任意类型的）。推荐浏览下java.util.function包下的其它常用接口，比如Consumer、Function、Predicate。
>
> 2. 更进一步：两者的区别
>     我们可能考虑的问题是：何时使用orElse和何时使用orElseGet？看起来可以使用orElseGet的时候，使用orElse也可以代替（因为Supplier接口没有入参），而且使用orElseGet还需要将计算过程额外包装成一个 lambda 表达式。
>
>     一个关键的点是，使用Supplier能够做到懒计算，即使用orElseGet时。它的好处是，只有在需要的时候才会计算结果。具体到我们的场景，使用orElse的时候，每次它都会执行计算结果的过程，而对于orElseGet，只有Optional中的值为空时，它才会计算备选结果。这样做的好处是可以避免提前计算结果的风险。
>
> 3. 场景举例
>     举个例子，或许更清楚一些：
>
>     ````java
>     class User {
>           // 中文名
>       	private String chineseName;
>       	// 英文名
>       	private EnglishName englishName;
>     }
>     
>     class EnglishName {
>         // 全名
>         private String fullName;
>         // 简写
>         private String shortName;
>     }
>     ````
>
>     假如我们现在有User类，用户注册账号时，需要提供自己的中文名或英文名，或都提供，我们抽象出一个EnglishName类，它包含英文名的全名和简写（因为有的英文名确实太长了）。现在，我们希望有一个User#getName()方法，它可以像下面这样实现：
>
>     ````java
>     class User {
>         // ... 之前的内容
>         public String getName1() {
>             return Optional.ofNullable(chineseName)
>                       .orElse(englishName.getShortName());
>         }
>         public String getName2() {
>             return Optional.ofNullable(chineseName)
>                 .orElseGet(() -> englishName.getShortName());
>         }
>     }
>     ````
>
>     我写了两个版本，分别使用orElse和orElseGet。现在，你可以看出getName1()方法有什么风险了吗？它会出现空指针异常吗？
>
>     答案是：是的。当用户只提供了中文名时，此时englishName属性是null，但是在orElse中，englishName.getShortName()总是会执行。而在getName2()中，这个风险却没有。
>
> 4. 真实案例
>   或许上面那个例子还不是很清楚，我们看一个真实的案例分页插件报错The jdbcUrl is Null, Cannot read database type #2172。
>   其中提到的一条语句为：DbType dbType = (DbType)Optional.ofNullable(this.dbType).orElse(JdbcUtils.getDbType(connection.getMetaData().getURL()));，没错，问题是出在orElse那里。
>
>
> 5. 总结
>     这篇文章分析了常用的Optional中的orElse和orElseGet方法，比较了两者的区别，说明了使用orElse时的风险，并提供了一个简朴的例子，我的收获是从真实案例中学到的。
>
> 总结下来，使用Optional时，当备选值是通过某个计算过程得到的时候，你都应该小心这个计算过程是否有风险，并在orElse和orElseGet之间做一个权衡。

+ **orElseThrow**

  获取数据, 如果数据不为空就能获取该数据, 如果为空则根据你传入的参数来创建异常抛出

  ````java
  // orElseThrow 的源码
  public <X extends Throwable> T orElseThrow(Supplier<? extends X> exceptionSupplier) throws X {
      if (value != null) {
          return value;
      } else {
          throw exceptionSupplier.get();
      }
  }
  ````

  

### 6.2.5 过滤 (filter)

+ 我们可以使用 **filter** 方法对数据进行过滤, 如果原本是有数据的, 但是不符合判断, 也会变成一个无数据的 Optional 对象

  ````java
  Optional<Author> authorOptinal = Optional.ofNullable(getAuthor());
  authorOptinal.filter(author -> author.getAge() > 100).ifPresent(author -> System.out.println(author.getName()));
  
  // filter 方法的源码
  public Optional<T> filter(Predicate<? super T> predicate) {
      Objects.requireNonNull(predicate);
      if (!isPresent())
          return this;
      else
          return predicate.test(value) ? this : empty();
  }
  ````

  

### 6.2.6 判断 (isPresent)

+ 我们可以使用 **isPresent** 方法进行是否存在数据的判断, 如果为空返回值为 false, 如果不为空, 返回值为 true, 但是这种方式并不能体现 Optional 的好处, **更推荐使用 ifPresent 方法**

  ````java
  Optional<Author> authorOptinal = Optional.ofNullable(getAuthor());
  if (authorOptinal.isPresent()) {
      System.out.println(authorOptinal.get().getName());
  }
  
  // isPresent 方法的源码
  public boolean isPresent() {
      return value != null;
  }
  ````

  

### 6.2.7 数据转换 (map)

+ Optional 还提供了 **map** 方法可以让我们对数据进行转换, 并且转换的到的数据也还是被 Optional 包装好的, 保证了我们的使用安全

+ 例如我们想获取作家书籍的集合

  ````java
  Optional<Author> authorOptinal = Optional.ofNullable(getAuthor());
  Optional<List<Book>> books = authorOptinal.map(author -> author.getBook());
  books.ifPresent(books-> System.out.println(books));
  
  // map 方法的源码
  public<U> Optional<U> map(Function<? super T, ? extends U> mapper) {
      Objects.requireNonNull(mapper);
      if (!isPresent())
          return empty();
      else {
          return Optional.ofNullable(mapper.apply(value));
      }
  }
  ````

  





















