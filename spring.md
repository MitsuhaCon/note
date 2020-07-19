**@ConfigurationProperties**:告诉SpringBoot将本类中的所有属性和配置文件相关的配置进行绑定；前提是这个类要用**@Component**标注，放入容器中的类才能自动映射,该注解针对的是**application.properties**和**application.yml** 

**@Value** 支持点对点的自动映射，例如：@Value("${person.last-name}") 

**@PropertySource** 转载指定的配置文件。例如： 

@PropertySource(value = {"classpath:person.properties"}) 

**@****ImportResource** 导入Spring的配置文件，让配置文件里面的内容生效，例如：@ImportResource(location = {"classpath:beans.xml"})，自己写一个beans.xml 

**@Bean** 将返回值添加到容器中，容器中这个组件默认的id就是方法名。 

**配置文件点位符** ${random.value}、${random.int}、${random.long}、${random.int(10)}、${random.int[1024,65536]} 

# 1 `IOC` 控制反转

将创建权交给 `BeanFactory` 

**ApplicationContext**接口：立即加载，单例对象适用。有三个常用实现类

- ClassPathXmlApplicationContext：它可以加载类路径下的配置文件，要求配置文件必须在类路径下。否则加载不到。
- FileSystemXmlApplicationContext：它可以加载磁盘任意路径下的配置文件（得有访问权限才行）
- AnnotationConfigApplicationContext：它是用于读取注解创建容器的。

**BeanFactory**：延迟加载，多例对象适用

`bean`的三种创建方式，**默认情况下，spring 创建的 bean 是单例的**，

1. 使用默认构造函数创建，在 spring 的配置文件中使用 bean 标签，配以 id 和 class 属性之后，且没有其它属性和标签时，采用的就是默认构造函数创建 bean 对象，如果此时没有默认构造函数，则对象无法创建。

   ```xml
   <bean id="xxxService" class="com.xxx.xxx.service.impl.xxxServiceImpl"></bean>
   ```

2. 使用普通工厂中的方法创建对象（使用某个类中的方法创建对象，并存入 spring 容器）

   ```xml
   <bean id="instanceFactory" class="com.xxx.factory.InstanceFactory"></bean>
   <!-- methodName 中 instanceFactory 类中的一个方法名-->
   <bean id="xxxService" factory-bean="instanceFactory" factory-method="methodName"></bean>
   ```

3. 使用工厂中的静态方法创建对象（使用某个类中的静态方法创建对象，并存入 spring 容器中）

   ```xml
   <bean id="xxxService" class="com.xxx.factory.InstanceFactory" factory-method="methodName"></bean>
   ```

 **bean** 的作用范围，bean 标签中有一个 scope 属性
	取值：
		singleton: 单例的 默认值
		prototype：多例的
		request：作用于 web 应用的请求范围
		session：作用于 web 应用的会话范围
		global-session：作用于集群环境的会话范围（全局会话范围），当不是集群的时候，它就是 session。多个服务器共用一个 global-session，负载均衡。

**bean** 的生命周期

​	单例对象：当容器创建时对象出生，只要窗口还在，对象一直活着，窗口销毁，对象就死亡

​	多例对象：当我们使用对象时 spring 框架才为我们创建，对象只要是在使用过程中就一直活着。当对象长时间不用，且没有别的对象引用时，垃圾回收机制就会触发。

# 2 `DI` 依赖注入

Dependency Injection

IOC 的作用： 降低程序间的耦合（依赖关系）

依赖关系的管理：以后都交给 spring 来维护，当前类需要用到其他类的对象时，由 spring 为我们提供，我们只需要在配置文件中说明即可，依赖关系的维护就称之为**依赖注入**

依赖注入：

- ​	注入的数据

  1. 基本类型和 String
  2. 其它 bean 类型（在配置文件中或者注解配置过的 bean）
  3. 复杂类型 / 集合类型

- 注入的方式

  1. 构造方法提供

     ```xml
     <bean id="xxxService" class="com.xxx.xxx.service.impl.xxxServiceImpl">
         <!-- 使用 constructor-arg 进入有参构造函数注入，此标签必须放在 bean 标签内
     			标签中的属性
     				type：用于指定要注入的数据的数据类型，该数据类型也是构造函数中某个或某些参数的类型
     				index：用于指定要注入的数据给构造函数中指定索引位置的参数赋值，索引的位置是从 0 开始
     				name：用于指定给构造函数中指定名称的参数赋值
     				value：用于提供基本类型和 String 类型的数据
     				ref：用于指定其他的 bean 类型数据。它指的就是在 spring 的 核心容器中出现过的 bean 对象
     
     		优势：在获取 bean 对象时，注入数据是必须的操作，否则对象无法创建成功
     		弊端：改变了 bean 对象的实例化方式，使我们在创建对象时，如果用不到这些数据，也必须提供
     -->
     	<constructor-arg> </constructor-arg>
     </bean>
     ```

     

  2. 使用 set 方法提供

     ```xml
     <!-- set 方法注入-->
     <bean id="xxxService" class="com.xxx.xxx.service.impl.xxxServiceImpl">
     	<property name="name" value="杨黎明"></property>
     </bean>
     
     
     <!-- 基于 set 方法注入
     	用于给 list 结构集合注入的标签：
     		list array set
     	用于给 map 结构集合注入的标签
     		 map  props
     -->
     <bean id="xxxService" class="com.xxx.xxx.service.impl.xxxServiceImpl">
     	<property name="name" >
         	<array></array>
         </property>
     </bean>
     ```

     

  3. 使用注解提供

     找到 `Spring Framework` 中的 `spring-core` 进行

     ```xml
     
     ```

# 3 Annotation 注解

## 3.1 @Component

找到 `Spring Framework` 中的 `spring-core` 进行搜索 **`component-scan`**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">
	<!-- 告知 spring 在创建窗口时要扫描的包，配置所需要的标签是一个名称为 context 名称空间和约束中-->
    <context:component-scan base-package="org.example"/>

</beans>
```

`serviceImpl` 实现类上 给一个 **`@Component`** 注解，那么这个类就交由 spring容器管理了。默认情况取这个类时，用类名，并把类名的首字母小写即可。

```java
// 也可以指定 bean 名 @Component("test")
@Component
public class TestServiceImpl implements TestService {
    
}
```

```java
public class TestController {
    // 获取核心容器对象，这个 xxx.xml 是在 resources 包下
    ApplicationContext applicationContext = new ClassPathXmlApplicationContext("xxx.xml")
    // 就能得到这个 bean 的实例了
    applicationContext.getBean("testServiceImpl");
    //applicationContext.getBean("test");
}
```

## 3.2 @Controller 、 @Service 、@Repository

以上三个注解的作用和属性与 **`@Component`** 是一模一样的

 **`@Controller`** 一般用于表现层

 **`@Service`** 一般用于业务层

 **`@Repositroy`** 一般用于持久层  

## 3.3 @Autowired 、@Qualifier 、@Resource 、@Value

**`@Autowired`** 作用：自动按照类型注入，只要容器中有唯一的一个 bean 对象类型和要注入的变量的**`数据类型`**匹配，就可以注入成功。在使用 `@Autowired` 注解时，**set 方法就不是必须的了**。

- 如果 Ioc 容器中没有任何 bean 的类型和要注入的变量类型匹配，则报错

- 如果 Ioc 容器中有多个 value 数据类型匹配时：那么就拿变量名继续进行匹配 Ioc 容器中的  key 值，变量名要是还不匹配，那就报错。

  ```java
  @Autowired
  private TestDao testDao = null;
  ```

  Spring 的 IoC 容器：Map 结构

  | key    String | value    Object    这就是数据类型            |
  | ------------- | -------------------------------------------- |
  | testDao       | public class TestDaoImpl implements TestDao  |
  | testDao2      | public class TestDaoImpl2 implements TestDao |
  | testDao3      | public class TestDaoImpl3 implements TestDao |



**`@Qualifier`** 作用：在按照类型注入的基础上再按照名称注入，它在给类成员注入时不能单独使用。其 value 值是注入 bean 的 id

```java
@Autowired
@Qualifier(value="testDao2")
private TestDao testDao2 = null;
```



**`@Resource`** 作用：直接按照注入 bean 的 id 注入，它可以独立使用。 其 name 用于指定 bean 的 id

> 以上有三个注入都只能注入其他 bean 类型的数据，面基本类型和 String 类型无法使用上述注解实现。
>
> 另外，集合类型的注入只能通过 XML 来实现

**`@Value`** 作用：用于注入基本类型和 String 类型的数据，其 value 值可以使用 spring  中的 `SpEL` （也就是 spring 的 el 表达式）写法： ${ 表达式}

## 3.4 @Scope 、@PreDestroy 、@PostConstruct

**`@Scope`**：作用和 bean 标签中的 scope 属性实现的功能是一样的。通常 value 取值  `singleton`  或 `prototype`

**`@QPreDestroy`** ：作用是用于指定销毁方法

**`@PostConstruct`**：作用是用于指定初始化方法

## 3.5 @Configuration 、@ComponentScan 、@Bean 、@Import

**`@Configuration`** 作用：指定当前类是一个配置类，当一个类作为参数传入  `new AnnotationConfigApplicationContext(xxx.class)` 时，这个类上可以不用 **`@Configuration`** 来标注。

**`@CompoentScan`** 作用：用于通过注解指定 spring 在创建窗口时要扫描的包

属性：

​		`value`：它和 `basePackages` 的作用是一样的，都是用于指定创建容器时要扫描的包，我们使用此注解就等同于在 `xml` 中配置了

```xml
<context:component-scan base-package="org.example"/>
```



```java
@Confuguration
@ComponentScan
```

**`@Bean`** 作用：用于把当前方法的返回值作为 `bean` 对象存入 `spring` 的 `Ioc` 容器中

​	属性：

​				`name`：用于指定 bean 的 id ，当不写时，默认值是当前方法的名称。

> **当我们使用注解配置方法时，如果方法有参数，`spring` 框架会去容器中查找有没有可用的 `bean` 对象，查找的方式和 @Autowired 注解的作用是一样的**

```java

/**
 * 此时，这个类就可以代替 bean.xml 中的内容了
 *
 **/
@Configuration
@ComponentScan("com.xxx.xxx")
public class SpringConfiguration {
    /**
     * 用于创建一个 QueryRunner 对象
     * 参数 dataSource 是可以通过 spring 自动获取的，前提是下方必须定义了 一个  				      * name="dataSource" bean 才行。
     * 连接池应该是一个多例的，可以用 @Scope 来指定
     **/
    @Bean(name="runner")
    @Scope(value="prototype")
    public QueryRunner createQueryRunner(DataSource dataSource) {
        return new QueryRunner(dataSource);
    }
    
    
    @Bean(name="dataSource")
    public DataSource createDataSource() {
        try{
            ComboPooledDataSource ds = new ComboPoolDataSource();
            ds.setDriverClass("com.mysql.jdbc.Driver");
            ds.setJdbcUrl("jdbc:mysql://localhost:3306/databaseName")
            ds.setUser("");
            ds.setPassword("");
        } catch (Exception e) {
            
        }
    }
    
}
```

```java
// 单元测试，由于没有 bean.xml ，所以不能用 ClassPathXmlApplicationContent 来获取 bean 了
// 而应该用 AnnotationConfigApplicationContext

@Test
public void test() {
    // 此时需要把配置类作为参数传入
    ApplicationContext ac = new AnnotationConfigApplicationContext(SpringConfiguration.class);
    ac.getBean("");
}
```

**`@Import`** 作用：用于导入其他的配置类

​	属性：

​			**`value`** ：用于指定其他配置类的字节码。当我们使用 **`@Import`** 注解之后，有**`@Import`** 的类就是父配置类，面导入的都是子配置类

## 3.6 @PropertySource 、@Value

**`@PropertySource`** 作用：用于指定 properties 文件的位置

​	属性：

​			**`value`** ：指定文件的名称和路径。关键字： **`calsspath`** 表示类路径下

**`@Value`** 作用：读取 properties 文件中的数据，使用 **｀el｀** 表达式

## 3.7 @Runwith 、@ContextConfiguration

使用 Junit 单元测试，测试我们的配置，Spring 整合 Junit 的配置、

1. 导入 spring 整合 Junit 的 jar

2. 使用 Junit 提供的一个注解 **`@Runwith`**把原有的 main 方法替换了，替换成 spring 提供的 

3. 使用 **`@ContextConfiguration`** 告知 spring 的运行器，spring 和 IoC 创建是基于  xml 还是 注解的，并说明位置.

   > 注意：当我们使用 spring 5.x 版本时，要求 junit 的 jar 必须是 4.12 及以上

```java
@Runwith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes = SpringConfiguration.class)
public class AccountServiceTest {
    @Autowired
    private XxxService xxxService;
    
    @Test
    public void test() {
        xxxService.xx();
    }
}
```

# 4 动态代理

特点：字节码隋随用随创建，随用随加载

作用：不修改源码的基础上对方法增强

分类：

- 基于接口的动态代理

  - 涉及的类：Proxy

  - 提供者：JDK 官方

    如何创建代理对象：使用 Proxy 类中的 newProxyInstance 方法

    创建代理对象的要求：被代理类最少实现一个接口，如果没有则不能使用

    newProxyInstance 方法的参数：

    - ClassLoader：类加载器，它是用于加载代理对象字节码的。和被代理对象使用相同的类加载器，固定写法。
    - Class[]：字节码数组，它是用于让代理对象和被代理对象有相同方法。固定写法。
    - InvocationHandler：用于提供增强的代码，它是让我们写如何代理，我们一般都是写一个目标该接口的实现类，通常情况下都是匿名内部类，但不是必须的此接口的实现类都是  **谁用谁写**

    ```java
    IProducer proxyProducer = (IProducer) Proxy.newProxyInstance(producer.getClass().getClassLoader(), producer.getClass().getInterface(), new InvocationHandler() {
        
        /**
         * 作用：执行被代理对象的任何接口方法都会经过该方法
         * 方法参数的含义：
         * @param proxy 代理对象的引用
         * @param method 当前执行的方法
         * @param args 当前执行方法所需的参数
         * @return 和被代理对象方法有相同的返回值
         **/
        @Override
        public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
            // 前置通知
            // method.invoke();
            // 后置通知
            // catch 中的是 异常通知
            // finally 中的是最终通知
            return null;
        }
    })
    ```

- 基于子类的动态代理

  - 涉及的类：Enhancer

  - 提供者：第三方 cglib 库

    如何创建代理对象：使用 Enhancer类中的 create 方法

    创建代理对象的要求：被代理类不能是最终类

    create 方法的参数：

    ​	Class：字节码，它是用于指定被代理对象的字节码

    ​	CallBack：用于提供增强的代码，它是让我们写如何代理。我们一般写的都是该接口的子接口实现类：MethodIntercepter

    ```java
    Producer cglibProducer = (Producer)Enhancer.create(producer.getClass(), new MethodInterceptor() {
        @Override
        public Object intercept(Object proxy, Method method, Object[] args, MethodProxy methodProxy)
    })
    ```

# 5 AOP 面向切面编程

AOP 是基于动态代理实现的。

作用：在程序运行期间，不修源码对已有方法进行增强

优势：减少重复代码、提高开发效率、维护方便

相关术语：

1. Joinpoint (连接点)：所谓连接点是指那些被拦截到的点，在 spring 中，这些点指的是方法，因为 spring 只支持类型的连接点。接口中的方法都是连接点。
2. Pointcut (切入点)：所谓切入点是指我们要对哪些 Joinpoint 进行拦截的定义，连接点要是被增强了，那么就是切入点
3. Advice (通知)：所谓通知是指拦截到 Joinpoint之后要做的事情就是通知。
   - 前置通知：在 method.invoke() 之前的
   - 后置通知：在 method.invoke() 之后的
   - 异常通知：在 catch 中的
   - 最终通知：在 finally 中的
4. Introduction (引介)：引介是一种特殊的通知在不修改类代码的前提下，可以在运行期为类动态地添加一些方法或 Field
5. Target (目标对象)：代理的目标对象
6. Weaving (织入)：是指把增强应用到目标对象来创建新的代理对象的过程，spring 采用动态代理织入，而 AspectJ 采用编译织入和类装载期织入
7. Proxy (代理)：一个类被 AOP 织入后，就产生一个结果代理类
8. Aspect (切面)：指切入点和通知的结合-

## 5.1 Spring 基于 XML 的 AOP 配置

 ```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:aop="http://www.springframework.org/schema/aop"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd">
    <!-- 配置 spring 中的 IoC ,把 service 对象交由 spring 管理-->
    <bean id="xxxxService" class="xxx.xxx.xxx.xxxxServiceImpl"></bean>
    
    <!-- 配置 Logger 类-->
    <bean id="logger" class="com.xxx.utils.Logger"></bean>
    <!--
		1. 把通知 Bean 也交给 spring 管理
		2. 使用 aop:config 标签表明开始 AOP 的配置
		3. 使用 aop:aspect 标签表明配置切面
			id 属性：是给切面提供一个唯一标识
			ref 属性：是指定通知类 bean 的 id
		4. 在 aop:aspect 标签的内部使用对应标签来配置通知类型，我们现在示例是让 log 方法在切入点方法执行之前执行，
		   使用 aop:before标签：
				method 属性：用于指定 log 类中哪个方法是前置通知
				pointcut 属性：用于指定切入点表达式，该表达式的含义指的是对业务层中哪些方法增强
				切入点表达式的写法：
					关键字：execution(表达式)
					表达式：
						访问修饰符   返回值   包名.包名.包名.包名.类名.方法名(参数列表)



	-->
    <aop:config>
    	<aop:aspect id="logAdvice" ref="logger">
        	<aop:before method="printLog" pointcut="execution(public void com.xxx.utils.Logger.printLog())"></aop>
        </aop:aspect>
    </aop:config>
</beans>
 ```

