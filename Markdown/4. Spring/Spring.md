# 一, Spring Boot

## 1. Spring Boot 的优点

1. 快速构件项目
2. 对主流开发框架的无配置集成
3. 项目可独立运行, 无须外部依赖 servlet 容器
4. 提供运行时的应用监控
5. 极大地提高了开发, 部署效率
6. 与云计算的天然集成





# 二, Spring MVC







# 三, Spring



## 3.1 Spring 注解 (Component, 依赖注入)

### 3.1.1 @Component 注解

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

### 3.1.2 依赖注入

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



## 3.2 Spring Bean 的生命周期

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



 ## 3.3 Spring Bean 的作用域

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
