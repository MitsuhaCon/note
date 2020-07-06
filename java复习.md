# 1 JVM、JRE和JDK的关系

**Java Virtual Machine(JVM)**是Java虚拟机，Java程序需要运行在虚拟机上，不同的平台有自己的虚拟机，因此Java语言可以实现跨平台。

**Java Runtime Environment(JRE)**包括了**JVM**和java程序所需的核心类库等。java.lang：包含了运行java程序必不可少的系统类等，系统缺省加载这个包。

**Java Development Kit(JDK)**是提供给java开发人员使用的，其中包含了java的开发工具(javac.exe   java.exe    jar.exe)，**JDK中包含了JRE**。



# 2 关键字

**true   false   null  不是关键字**

# 3 访问修饰符

| 修饰符    | 当前类 | 同包 | 子类 | 其他包 |
| --------- | ------ | ---- | ---- | ------ |
| private   | √      | ×    | ×    | ×      |
| default   | √      | √    | ×    | ×      |
| protected | √      | √    | √    | ×      |
| public    | √      | √    | √    | √      |

- private：在同一个类中可见。
- default：缺省，在同包下可见。
- protected：同包和子类可见。
- public：对所有类可见。

# 4 进制

- 原码
- 反码
  - 正数的反码与原码相同
  - 负数的反码：原码符号位不变，其他位取反。

- 补码
  - 正数的补码与原码相同
  - 负数的补码：反码+1

注意事项：

- 二进制的最高位是符号位（0表示正，1表示负）
- java中没有无符号数
- 计算机中以整数的补码进行运算和存储

1.  <<  左移    **低位补0**
2.  &gt;&gt;  右移    **高位补符号位**
3.  &gt;&gt;&gt; 无符号右移  **高位补0**

> Integer.highestOneBit(int i)方法的作用与底层实现

```java
/**
 * 实现原理：如果一个数字是2的幂次方数，那么它对应的二进制数只有一个bit位上是1，其它bit位都是0；
 * 将这个数的二进制数中除最高位的1之外的其它低位都变成1，然后右移一位，两数再相减.
 * java中int占4个字节，一个字节8位，所以一共有32位
 * @param i
 * @return int
 */
public static int highestOneBit(int i) {
        // HD, Figure 3-1
        i |= (i >>  1);
        i |= (i >>  2);
        i |= (i >>  4);
        i |= (i >>  8);
        i |= (i >> 16);
        return i - (i >>> 1);
    }
```

> 判断一个数在计算机中存储的二进制有几个

```java
private static int z(int i) {
        int count = 0;

        do {
            if ((i & 1) == 1) {
                ++count;
            }
        } while((i >>>= 1) != 0);

        return count;
    }
```



# 5 八大基本数据类型及引用数据类型

- 基本数据类型
  - 数值型
    - 整数类型（byte，short，int，long）
    - 浮点类型（float，double）
  - 字符型（char）
  - 布尔型（boolean）

- 引用数据类型
  - 类（class）
  - 接口（interface）
  - 数组（[]）

变量是内存中的小容器，用来存储数据。**计算机存储设备的最小信息单元叫位(bit)**,我们又称这为“比特位”，通常用小写的字母b表示。而计算机最小的存储单元叫“字节(byte)”,通常用大写字母B表示。字节是由连续的8个bit组成。

**存储单位关系**

1B = 8b	1KB = 1024B	1MB = 1024KB	1GB = 1024MB	1TB = 1024GB

# 6 反射

**Java反射机制是在运行状态中，对于任意一个类，都能够知道这个类的所有属性和方法；对于任意一个对象，都能够调用它的任意方法和属性**。这种动态获取信息以及动态调用对象方法的功能称为Java语言的反射机制。

java反射框架提供以下功能：

- 在运行时判定任意一个对象所属的类
- 在运行时判定任意一个类的对象
- 在运行时判定任意一个类所具有的成员变量和方法
- 在运行时调用任意一个对象的方法

反射的优点：使用反射机制，代码可以在运行时装配，提高程序的灵活性和扩展性。

反射的缺点：反射基本上是一种解释操作，JVM无法对这些代码进行优化。因此，反射操作的效率要比非反射低得多。

安全限制：使用反射技术要求程序必须在一个没有安全限制的环境中运行

**反射机制相关类**

| 类名          | 用途                                             |
| ------------- | ------------------------------------------------ |
| Class类       | 代表类的实体，在运行的Java应用程序中表示类和接口 |
| Field类       | 代表类的成员变量（成员变量也称为类的属性）       |
| Method类      | 代表类的方法                                     |
| Constructor类 | 代表类的构造方法                                 |

**获取Class类对象的三种方法**

````java
//方法一，须处理ClassNotFoundException
Class<?> clazz = Class.forName("类的全路径名");
````

````java
//方法二，只适用于在编译前就知道操作的Class。直接通过类名.class的方式得到，该方法最为安全可靠，程序性能更高。这说明任何一个类都有一个隐含的静态成员变量class
Class clazz = Object.class;
````

```java
//方法三，使用对象的getClass()方法
Object object = new Object();
Class clazz = object.getClass();
```

**通过反射获取私有属性，方法和构造方法时，需要进行暴力反射，设置setAccessible(true)**。否则会报错说无法获取私有属性，方法和构造方法。**带有Declared修饰的方法可以反射到私有的方法，没有Declared修饰的只能用来反射公有的方法**。

# 7 语法糖

语法糖(Syntctic Sugar)就是对现有语法的一个封装。简而言之，语法糖让程序更加简洁，有更高的可读性。java虚拟机并不支持语法糖，这些语法糖在编译阶段会被还原成简单的基础语法结构，这个过程就是解语法糖。

## 7.1 switch支持String与枚举

java中的switch自身支持数据类型有：byte、short、int 或者 char，从 Java SE 7 开始，switch 支持字符串 String 类型了，同时 case 标签必须为字符串常量或字面量。

> switch case 执行时，一定会先进行匹配，匹配成功返回当前case的值，再根据是否有break，判断是否继续输出，或是跳出判断。
>
> 如果 case 语句块中没有 break 语句时，JVM 并不会顺序输出每一个 case 对应的返回值，而是继续匹配，匹配不成功则返回默认 case。
>
> 如果 case 语句块中没有 break 语句时，匹配成功后，从当前 case 开始，后续所有 case 的值都会输出（包括default)。如果后续的 case 语句块有 break 语句则会跳出判断。

switch对String类型的支持，其实是先对String进行hashCode()，再进行equals()。做了两次switch case

```java
//编译前原码
public class Test {
    public static void main(String[] args) {
        String str = "s";
        switch (str) {
            case "a" :
                System.out.println("a");
            case "s" :
                System.out.println("s");
            default:
                System.out.println("default");
        }
    }
}

//编译后
public class Test {
    public Test() {
    }

    public static void main(String[] args) {
        String str = "s";
        byte var3 = -1;
        switch(str.hashCode()) {
        case 97:
            if (str.equals("a")) {
                var3 = 0;
            }
            break;
        case 115:
            if (str.equals("s")) {
                var3 = 1;
            }
        }

        switch(var3) {
        case 0:
            System.out.println("a");
        case 1:
            System.out.println("s");
        default:
            System.out.println("default");
        }
    }
}
```

## 7.2 泛型和类型擦除

​		Java 泛型（generics）是 JDK 5 中引入的一个新特性, 泛型提供了编译时类型安全检测机制，该机制允许程序员在编译时检测到非法的类型。

​		泛型的本质是**参数化类型**，也就是说所操作的数据类型被指定为一个参数。

**泛型的定义和使用**

1. 泛型类

   ```java
   //定义泛型类
   //T 代表一般的任何类。
   //E 代表 Element 的意思，或者 Exception 异常的意思。
   //K 代表 Key 的意思。
   //V 代表 Value 的意思，通常与 K 一起配合使用。
   //S 代表 SubType 的意思。
   public class Test<T> {
       T field;
   }
   
   //编译前
   public class TestGenerics<T, E> {
       T t;
       E e;
   
       public <T> void testMethod(T t){
   
       }
   
       public static void main(String[] args) {
           TestGenerics<String, List> testGenerics = new TestGenerics<>();
           testGenerics.t = "string";
           testGenerics.e = new ArrayList<>();
       }
   }
   
   //编译后
   public class TestGenerics<T, E> {
       T t;
       E e;
   
       public TestGenerics() {
       }
   
       public <T> void testMethod(T t) {
       }
   
       public static void main(String[] args) {
           TestGenerics<String, List> testGenerics = new TestGenerics();
           testGenerics.t = "string";
           testGenerics.e = new ArrayList();
       }
   }
   
   ```

   

2. 泛型方法

   ```java
   /*泛型方法与泛型类稍有不同的地方是，类型参数也就是尖括号那一部分是写在返回值前面的。<T>中的 T 被称为类型参数，而方法中的 T 被称为参数化类型，它不是运行时真正的参数。*/
   
   public class Test<T> {
       //泛型方法始终以自己定义的类型参数为准,与类中的类型参数无关
       //如果把<T>去掉，那么就是一个普通方法。
       public <T> void testMethod(T t) {
           
       }
   }
   //为了避免混淆，如果在一个泛型类中存在泛型方法，那么两者的类型参数最好不要同名
   
   ```

3. 泛型接口

   ```java
   public interface Iterable<T> {
   }
   ```

**通配符 ?**

通配符有 3 种形式。

1. <?> 被称作无限定的通配符。

   ```java
   /*Collection被<?>修饰后，collection只能调用Collection中与类型无关的方法*/
   public void test(Collection<?> collection) {
       collection.add("hello");//编译不通过
   }
   ```

2. <? extends T> 被称作有上限的通配符

   ```java
   public void test(Collection<? extends Object> collection) {
       collection.add("hello");//编译不通过
   }
   ```

3. <? super T> 被称作有下限的通配符。

   ```java
   public void test(Collection<? super Object> collection) {
       collection.add("hello");//编译通过
   }
   ```

**类型擦除**

```java
//field的类型是Object
public class Erasure <T>{
    //	public class Erasure <T>{
    T field;
    public Erasure(T t) {
        this.field = t;
    }
    public static void main(String[] arg) {
        Erasure<String> erasure = new Erasure<String>("hello");
		Class eclz = erasure.getClass();
		System.out.println("erasure class is:"+eclz.getName());

		Method[] methods = eclz.getDeclaredMethods();
		for ( Method m:methods ){
			System.out.println(" method:"+m.toString());
		}
    }
}

//field的类型是String   类型参数就被替换成类型上限
public class ErasureTwo <T extends String>{
    //	public class Erasure <T>{
    T field;
    public Erasure(T t) {
        this.field = t;
    }
}
```

理解类型擦除有利于我们绕过开发当中可能遇到的雷区，同样理解类型擦除也能让我们绕过泛型本身的限制。

```java
//通过反射以及类型擦除的性质完成add操作
public static void main(String[] args) {
        List<Integer> ls = new ArrayList<>();
        ls.add(23);
        try {
            Method method = ls.getClass().getDeclaredMethod("add",Object.class);
            method.invoke(ls,"test");
            method.invoke(ls,42.9f);
        } catch (Exception e) {
            e.printStackTrace();
        }

        for ( Object o: ls){
            System.out.println(o);
        }
    }
```

> ## 泛型类或者泛型方法中，不接受 8 种基本数据类型，需要使用它们的包装类

> ## Java 不能创建具体类型的泛型数组

```java
List<Integer>[] li2 = new ArrayList<Integer>[];//编译不通过
List<Boolean> li3 = new ArrayList<Boolean>[];//编译不通过
/*List<Integer>和 List<Boolean>在 jvm 中等同于List<Object>，所有的类型信息都被擦除，程序也无法分辨一个数组中的元素类型具体是 List<Integer>类型还是 List<Boolean>类型。*/
//但是
List<?>[] li3 = new ArrayList<?>[10];
li3[1] = new ArrayList<String>();
List<?> v = li3[1];
//借助于无限定通配符却可以。v不能进行add操作
```

## 7.3 自动装箱和自动拆箱

自动装箱就是java自动将原始类型值转换成对应的包装类对象，比如int的变量转换成Integer对象，这个过程叫做装箱，反之将Integer对象转换成int类型值，这个过程叫做拆箱。

| 基本数据类型 | 包装类型  |
| ------------ | --------- |
| byte         | Byte      |
| short        | Short     |
| char         | Character |
| int          | Integer   |
| long         | Long      |
| float        | Float     |
| double       | Double    |
| boolean      | Boolean   |

**自动装箱**

```java
//编译前
public void autoPackingOrDevanning() {
    int i = 10;
    Integer n = i;
}
//JDK8编译后
public void autoPackingOrDevanning() {
    int i = 10;
    Integer n = Integer.valueOf(i);
}
```

**自动拆箱**

```java
//JDK8编译时没有变化
//编译前
public void autoPackingOrDevanning() {
    Integer i = 10;
    int n = i;
}
//JDK8编译后
public void autoPackingOrDevanning() {
    Integer i = 10;
    int n = i;
}
```

## 7.4 方法变长参数

可变参数(variable arguments)是在Java 1.5中引入的一个特性。它允许一个方法把任意数量的值作为参数。可变长参数必须放在参数列表的尾部。

```java
public static void main(String[] args) {
    print("a", "b", "c");
}

public static void print(String... strs) {
    for (int i = 0; i < strs.length; i++) {
        System.out.println(strs[i]);
    }
}

//等同于有一个数组
public static void main(String[] args) {
    print(new String[]{"a", "b", "c"});
}

public static void print(String[] strs) {
    for (int i = 0; i < strs.length; i++)
        System.out.println(strs[i]);
}
```

## 7.5 条件编译

```java
//编译前
public static void ConditionalCompilation() {
        final boolean DEBUG = true;
        if(DEBUG) {
            System.out.println("Hello, DEBUG!");
        }
        final boolean ONLINE = false;
        if(ONLINE){
            System.out.println("Hello, ONLINE!");
        }
    }
//编译后
public static void ConditionalCompilation() {
        boolean DEBUG = true;
        System.out.println("Hello, DEBUG!");
        boolean ONLINE = false;
    }
```

## 7.6 数值字面量

在java 7中，数值字面量，不管是整数还是浮点数，都允许在数字之间插入任意多个下划线。这些下划线不会对字面量的数值产生影响，目的就是方便阅读。

```java
//编译前
public class Test {
    public static void main(String[] args) {
        int i = 10_000;
        System.out.println(i);
    }
}
//编译后
public class Test {
    public static void main(String[] args) {
        int i = 10000;
        System.out.println(i);
    }
}
```

## 7.7 增强for循环

```java
//编译前
public static void StrongFor() {
    String[] strs = {"a", "b", "c"};
    for (String s : strs) {
        System.out.println(s);
    }
    System.out.println();
    List<String> strList = Arrays.asList(strs);
    for (String s : strList) {
        System.out.println(s);
    }
}
//编译后
public static void StrongFor() {
    String[] strs = new String[]{"a", "b", "c"};
    String[] var1 = strs;
    int var2 = strs.length;
    for(int var3 = 0; var3 < var2; ++var3) {
        String s = var1[var3];
        System.out.println(s);
    }
    System.out.println();
    List<String> strList = Arrays.asList(strs);
    Iterator var6 = strList.iterator();
    while(var6.hasNext()) {
        String s = (String)var6.next();
        System.out.println(s);
    }
}
```



## 7.8  try-with-resource语句

```java
//编译前
public static void testTryWithResource() {
    try (BufferedReader br = new BufferedReader(new FileReader("d:\\ hello.xml"))) {
        String line;
        while ((line = br.readLine()) != null) {
            System.out.println(line);
        }
    } catch (IOException e) {
        // handle exception
    }
}
//编译后
public static void testTryWithResource() {
    try {
        BufferedReader br = new BufferedReader(new FileReader("d:\\ hello.xml"));
        Throwable var1 = null;
        try {
            String line;
            try {
                while((line = br.readLine()) != null) {
                    System.out.println(line);
                }
            } catch (Throwable var11) {
                var1 = var11;
                throw var11;
            }
        } finally {
            if (br != null) {
                if (var1 != null) {
                    try {
                        br.close();
                    } catch (Throwable var10) {
                        var1.addSuppressed(var10);
                    }
                } else {
                    br.close();
                }
            }
        }
    } catch (IOException var13) {
    }
}
```

## 7.9 Lambda表达式

Lambda表达式不是匿名内部类的语法糖，但是他也是一个语法糖。实现方式其实是依赖了几个JVM底层的Lambda相关api。

```java
public static void testLambda() {
    List<String> strList = new ArrayList<>();
    strList.add("a");
    strList.add("b");
    strList.add("c");
    strList.forEach(s -> System.out.println(s));
}
```

可能遇到的坑

```java
//两个方法重载失败，泛型擦除，参数编译的时候都是List list
public static void method(List<String> list) {
    System.out.println("invoke method(List<String> list)");
}
public static void method(List<Integer> list) {
    System.out.println("invoke method(List<Integer> list)");
}
```

```java
public class StaticTest{
    public static void main(String[] args) {
        GT<Integer> gti = new GT<Integer>();
        gti.var = 1;
        GT<String> gts = new GT<String>();
        gts.var = 2;
        System.out.println(gti.var);
    }

    class GT<T> {
        public static int var = 0;

        public void nothing(T x) {
        }
    }
}
```

```java
public static void main(String[] args) {
    Integer a = 1000;
    Integer b = 1000;
    Integer c = 100;
    Integer d = 100;
    System.out.println("a == b is " + (a == b));
    System.out.println(("c == d is " + (c == d)));
}
//jdk5中，Integer的操作上引入了一个新功能来节省内存和提高性能。整型对象通过使用相同的对象引用实现了缓存和重用。适用于整数值区间[-128,127]。只适用于自动装箱。使用构造函数创建对象不适用。
```

```java
for (Student stu : students) {    
    if (stu.getId() == 2) {
        students.remove(stu);  
    }           
}
/*会抛出ConcurrentModificationException异常。
Iterator是工作在一个独立的线程中，并且拥有一个 mutex 锁。 Iterator被创建之后会建立一个指向原来对象的单链索引表，当原来的对象数量发生变化时，这个索引表的内容不会同步改变，所以当索引指针往后移动的时候就找不到要迭代的对象，所以按照 fail-fast 原则 Iterator 会马上抛出java.util.ConcurrentModificationException异常。

所以 Iterator 在工作的时候是不允许被迭代的对象被改变的。但你可以使用 Iterator 本身的方法remove()来删除对象，Iterator.remove() 方法会在删除当前迭代对象的同时维护索引的一致性。*/
```

# 8 注解

注解的本质就是一个继承了Annotation接口的接口。解析一个类或者方法的注解往往有两种形式，一种是编译期直接的扫描，一种是运行期反射。

> **一个注解准确意义上来说，只不过是一种特殊的注释而已，如果没有解析它的代码，它可能连注释都不如。**

**注解的用途**

1. 生成文档，通过代码里标识的元数据生成javadoc文档。
2. 编译检查，通过代码里标识的元数据让编译器在编译期间进行检查验证。
3. 编译时动态处理，编译时通过代码里标识的元数据动态处理，例如动态生成代码。
4. 运行时动态处理，运行时通过代码里标识的元数据动态处理，例如使用反射注入实例

**注解的分类**

1. Java自带的标准注解，包括@Override（标明重写某个方法）、@Deprecated（标明某个类或方法过时）和@SuppressWarnings（标明要忽略的警告），使用这些注解后编译器就会进行检查
2. 元注解，元注解是用于定义注解的注解，包括@Retention（标明注解被保留的阶段）、@Target（标明注解使用的范围）、@Inherited（标明注解可继承）、@Documented（标明是否生成javadoc文档）
3. 自定义注解，可以根据自己的需求定义注解
   

**元注解**

1. **@Retention**

   ```java
   @Documented
   @Retention(RetentionPolicy.RUNTIME)
   @Target(ElementType.ANNOTATION_TYPE)
   public @interface Retention {
       /**
        * Returns the retention policy.
        * @return the retention policy
        */
       RetentionPolicy value();
   }
   ```

   

   保留期，@Retention应用到一个注解上的时候，它解释说明了这个注解的存活时间。取值如下：

   - RetentionPolicy.SOURCE   注解只在原码阶段保留，在编译器进行编译时它将丢弃忽视。
   - RetentionPolicy.CLASS  注解只被保留到编译进行的时候，它并不会加载到JVM中。这是默认行为。
   - RetentionPolicy.RUNTIME  注解可以保留到程序运行的时候，它会被加载进入到 JVM 中，所以在程序运行时可以获取到它们。如SpringMvc中的@Controller、@Autowired、@RequestMapping等。

   > **RetentionPolicy是一个枚举类**，在rt.jar包中的java.lang.annotation包下

2. **@Documented**

   ```java
   @Documented
   @Retention(RetentionPolicy.RUNTIME)
   @Target(ElementType.ANNOTATION_TYPE)
   public @interface Documented {
   }
   ```

   它的作用是能够将注解中的元素包含到 Javadoc 中去

3. **@Target**

   ```java
   @Documented
   @Retention(RetentionPolicy.RUNTIME)
   @Target(ElementType.ANNOTATION_TYPE)
   public @interface Target {
       /**
        * Returns an array of the kinds of elements an annotation type
        * can be applied to.
        * @return an array of the kinds of elements an annotation type
        * can be applied to
        */
       ElementType[] value();
   }
   ```

   只能标注在注解类上，并指定这个注解类能运用在哪儿。取值如下：

   - ElementType.ANNOTATION_TYPE 可以给一个注解进行注解
   - ElementType.CONSTRUCTOR 可以给构造方法进行注解
   - ElementType.FIELD 可以给属性进行注解
   - ElementType.LOCAL_VARIABLE 可以给局部变量进行注解
   - ElementType.METHOD 可以给方法进行注解
   - ElementType.PACKAGE 可以给一个包进行注解
   - ElementType.PARAMETER 可以给一个方法内的参数进行注解
   - ElementType.TYPE 可以给一个类型进行注解，比如类、接口、枚举
   - ElementType.TYPE_PARAMETER  (since 1.8)
   - ElementType.TYPE_USE  (since 1.8)

4. **@Inherited**

   ```java
   @Documented
   @Retention(RetentionPolicy.RUNTIME)
   @Target(ElementType.ANNOTATION_TYPE)
   public @interface Inherited {
   }
   ```

   Inherited 是继承的意思，但是它并不是说注解本身可以继承，而是说如果一个超类使用了@Inherited 注解，那么如果它的子类没有被任何注解应用的话，那么这个子类就继承了超类的注解。

5. **@Repeatable**

   ```java
   @Documented
   @Retention(RetentionPolicy.RUNTIME)
   @Target(ElementType.ANNOTATION_TYPE)
   public @interface Repeatable {
       /**
        * Indicates the <em>containing annotation type</em> for the
        * repeatable annotation type.
        * @return the containing annotation type
        */
       Class<? extends Annotation> value();
   }
   ```

   Repeatable 自然是可重复的意思。@Repeatable 是 Java 1.8 才加进来的，所以算是一个新的特性。

   Repeatable使用场景：在需要对同一种注解多次使用时，往往需要借助@Repeatable。

   下面举例说明一下，在生活中一个人往往是具有多种身份，如果我把每种身份当成一种注解该如何使用

举例：

```java
//先定义一个Persons类用来包含所有的身份
@Target(ElementType.TYPE) 
@Retention(RetentionPolicy.RUNTIME)
public @interface Persons {
	Person[] value();
}
```

```java
//再定义一个Person注解,@Repeatable括号内的就相当于用来保存该注解内容的容器。
@Repeatable(Persons.class)
public @interface Person {
    String role() default "";
}
```

```java
//声明一个Man类，给该类加上一些身份
@Person(role="CEO")
@Person(role="husband")
@Person(role="father")
@Person(role="son")
public class Man {
	String name="";
}
```

```java
//在main方法中访问该注解
public static void main(String[] args) {
    Annotation[] annotations = Man.class.getAnnotations();  
    System.out.println(annotations.length);
    Persons p1=(Persons) annotations[0];
    for(Person t:p1.value()){
    	System.out.println(t.role());
    }
}
```

**注解的属性**

注解的属性也叫做成员变量。注解只有成员变量，没有方法。注解的成员变量在注解的定义中以“无形参的方法”形式来声明，其方法名定义了该成员变量的名字，其返回值定义了该成员变量的类型。

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
public @interface TestAnnotation{
    int id();
    String msg();
}
```

## 注解不支持继承

不能用extends来继承某个@interface，但注解在编译后，编译器会自动继承java.lang.annotation.Annotation

## JAVA预置的注解

| 注解                 | 描述                     |
| -------------------- | ------------------------ |
| @Deprecated          | 给出过时警告             |
| @Override            | 方法覆盖                 |
| @SuppressWarnings    | 压制警告                 |
| @SafeVarargs         | 压制可变长参数报出的警告 |
| @FunctionalInterface | 函数式接口注解           |

# 9 Lambda

- 有且只有一个抽象方法的接口被称为函数式接口。
- 只有函数式接口才可以转换为lambda表达式。

- 函数式接口可以显式的被@FunctionalInterface所标识。

**案例1**  无参无返回

```java
//定义一个函数式接口
public interface TestLambda {
    void test();
}

//定义一个测试类
public class Test {
    public static void main(String[] arg) {
        //匿名方式
        TestLambda t = new TestLambda() {
            @Override
            public void test() {
                System.out.println("sdf");
            }
        };
        
        //lambda方式
        //无参无返回Lambda表达式
		TestLambda testLambda1 = () -> {System.out.println("你的名字");};
        //如果只有一行代码可以省略花括号
        TestLambda testLambda2 = () -> System.out.println("你的名字");
    }
}
```

**案例2**  有参有返回值

```java
public class Test {
    public static void main(String[] arg) {
        //有参无返回
    	TestLambda1 testLambda = (String string) -> System.out.println("你的名字");
        //参数类型可以省略
        TestLambda2 testLambda = (string) -> System.out.println("你的名字");
        //多个参数
        TestLambda3 testLambda = (string1,String2) -> System.out.println("你的名字");
        //有参有返回
        TestLambda3 testLambda = (string1,String2) -> {return "你的名字";};
        //返回值可以不要return
        TestLambda3 testLambda = (string1,String2) -> "你的名字";
        
    }
}
```

**案例3**  final类型参数

```java
public class Test {
    public static void main(String[] arg) {
        //全写
        FinalParam finalParam1 = (final int a, final int b) -> { return a + b;};
        System.out.println(finalParam1.add(10,20));
        //简写
        FinalParam finalParam2 = (a, b) -> a + b;
        System.out.println(finalParam2.add(10,20));
    }
}

interface FinalParam {
    int add(final int a, final int b);
}
```

> IntelliJ IDEA中书签的使用：CTRL + F11

# 10 什么是面向对象

Object Oriented Programming

Object Oriented Analysis

Object Oriented Design

面向对象其还是依赖于面向过程，只不过面向对象对面向过程进行了封装，方便我们使用。

**面向过程和面向对象的区别**

|      | 面向过程                                   | 面向对象               |
| ---- | ------------------------------------------ | ---------------------- |
| 优点 | 性能比面向对象好，因为类调用时需要实例化。 | 易维护、易复用、易扩展 |
| 缺点 | 不易维护、不易复用、不易扩展               | 性能不如面向过程       |

面向对象的三大特性五大原则

1. **单一职责原则**（SRP：Single Responsibility Principle） 类的功能要单一，不能包罗万象，跟杂货铺似的
2. **开放封闭原则**（OCP：Open-Close Principle） 一个模块对于拓展是开放的，对于修改是封闭的，想要增加功能热烈欢迎，想要修改，哼，一万个不乐意。
3. **里氏替换原则**（LSP：the Liskov Substitution Principle）子类可以替换父类出现在父类能够出现的任何地方。
4. **依赖倒置原则**（DIP：the Dependency Inversion Principle）高层次的模块不应该依赖于低层次的模块，他们都应该依赖于抽象。抽象不应该依赖于具体实现，具体实现应该依赖于抽象。比如你出国要说你是中国人，而不会说你是哪个村子的。中国人就是抽象，具体的省、市、县。
5. **接口分离原则**（ISP：the Interface Segregation Principle）设计时采用多个特定客户类有关的接口比采用一个通用的接口要好。

1. **封装** 隐藏对象的属性和实现细节，公对旬提供公共访问方式，将变化隔离，便于使用，提高复用性和安全性。
2. **继承** 提高代码复用性；继承是多态的前提。
3. **多态** 父类或接口定义的引用变量可以指向子类或具体实现类的实例对象。提高了程序的拓展性。

**总结**

- 抽象会使复杂的问题更加简单化
- 从以前面向过程的执行者，变成张张嘴的指挥者
- 面向对象更符合人类的思维，而面向过程则是机器的思想

# 11 集合Collection 

![](https://img-blog.csdn.net/20180803195348216?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ZlaXlhbmFmZmVjdGlvbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70&tdsourcetag=s_pctim_aiomsg)

Collection集合中的方法：

```java
int size();
boolean isEmpty();
boolean contains(Object o);
Iterator<E> iterator();
Object[] toArray();
<T> T[] toArray(T[] a);
boolean add(E e);
boolean remove(Object o);
boolean containsAll(Collection<?> c);
boolean addAll(Collection<? extends E> c);
boolean removeAll(Collection<?> c);
boolean retainAll(Collection<?> c);
void clear();
boolean equals(Object o);
int hashCode();
```

Map接口 键值对集合（双列集合）

1. **Collection** 接口的接口，对象的集合
   -   **LIst**（元素按进入的先后顺序进行保存，有序可重复）有如下接口实现类
     - **LikedList**实现类，链表，查询慢，增删快，可存储重复元素，没有同步，线程不安全
     - **ArrayList**实现类，数组，随机访问，查询快，增删慢，可存储重复元素，没有同步，线程不安全
     - **Vector**实现类，数组，和ArrayList相似，但同步，线程安全
       - **Stack**实现类
   - **Set**（仅接收一次，不可重复，并做内部排序）有如下接口实现类
     - **HashSet**，使用hash表（数组）存储元素
       - **LikedHashSet**，链表维护元素的插入次序
     - **TreeSet**，底层实现为二叉树，元素排好序
2. **Map** 接口，键值对的集合
   - **Hashtable**，接口实现类，同步，线程安全，key和vaule都不能为null。
   - **HashMap**，接口实现类，没有同步，线程不安全，key和value都可以为null。
     - **LikedHashMap**，双向链表和哈希表实现
     - **WeakHashMap**，
   - **TreeMap**，实现SortMap接口，红黑树对所有的key进行排序
   - **IdentifyHashMap**，

## 11.1 ArrayList源码解析

ctrl + alt + u 查看继承图

![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL0pvdXJXb24vaW1hZ2UvbWFzdGVyL0phdmElRTklOUIlODYlRTUlOTAlODglRTYlQkElOTAlRTclQTAlODElRTglQUYlQTYlRTglQTclQTMlRTQlQjklOEJBcnJheUxpc3QvQXJyYXlMaXN0JUU3JUJCJUE3JUU2JTg5JUJGJUU1JTg1JUIzJUU3JUIzJUJCJUU1JTlCJUJFLnBuZw)

蓝线是继承，虚线是接口实现

ArrayList是AbstractCollection的子类，同时实现了List接口，除此之外，还实现了三个标识型接口，RandomAccess表示实现类支持快速随机访问(意思就是支持下脚标取数,如果对象实现了RandomAccess,那么就用随机访问,否则用iterator.使用instansof)，Cloneable表示类支持克隆(**浅拷贝对基本数据类型可以达到完全复制,但是对引用数据类型则不可以**)，java.io.Serializable支持序列化(**便于后续对多个对象进行序列化时,可以先将多个对象装在ArrayList中,再对ArrayList进行序列化,使用ObjectInputStream和ObjectOutputStream进行流操作**)。

**成员变量**

```java
// 序列号
private static final long serialVersionUID = 8683452581122892189L;
// 数组初始容量为 10
private static final int DEFAULT_CAPACITY = 10;
// 空对象数组
private static final Object[] EMPTY_ELEMENTDATA = {};
// 缺省空对象数组
private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};
// 底层数据结构，数组
transient Object[] elementData;
// 数组元素个数，默认为0
private int size;
// 最大数组容量
private static final int MAX_ARRAY_SIZE = Integer.MAX_VALUE - 8;
```

**构造方法**

```java
//默认构造方法，初始为空数组。
//只有插入一条数据后才会扩展为10，而实际上默认是空的
 public ArrayList() {
    this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
}

//根据指定容量创建对象数组
public ArrayList(int initialCapacity) {
    if (initialCapacity > 0) {
        //创建initialCapacity大小的数组
        this.elementData = new Object[initialCapacity];
    } else if (initialCapacity == 0) {
        //创建空数组
        this.elementData = EMPTY_ELEMENTDATA;
    } else {
        throw new IllegalArgumentException("Illegal Capacity: "+
                                               initialCapacity);
    }
}

/**
 * 构造一个包含指定集合的元素的列表，按照它们由集合的迭代器返回的顺序。
 */
public ArrayList(Collection<? extends E> c) {
    //转换最主要的是toArray()，这在Collection中就定义了
    elementData = c.toArray();
    if ((size = elementData.length) != 0) {
        // c.toArray 有可能不返回一个 Object 数组
        if (elementData.getClass() != Object[].class)
            //使用 Arrays.copy 方法拷创建一个 Object 数组
            elementData = Arrays.copyOf(elementData, size, Object[].class);
    } else {
        // 替换为空数组
        this.elementData = EMPTY_ELEMENTDATA;
    }
}
```

**内部类**

```java
(1)private class Itr implements Iterator<E>  
    Itr实现了Iterator接口，同时重写了hasNext(),next(),remove()等方法
(2)private class ListItr extends Itr implements ListIterator<E> 
    ListItr继承了Itr,实现了ListIterator接口，同时重写了hasPrevious(),nextIndex(),previousIndex(),previous(),set(E e),add(E e)等方法，
(3)private class SubList extends AbstractList<E> implements RandomAccess  
(4)static final class ArrayListSpliterator<E> implements Spliterator<E>  
```

**核心方法解析**

```java
//添加一个特定的元素到list的末尾
public boolean add(E e) {
    //先确保elementData数组的长度足够，size是数组中数据的个数，因为要添加一个元素，
    //所以size+1，先判断size+1的这个个数数组能否放得下，在这个方法中去判断数组长度是否够用
    ensureCapacityInternal(size + 1);  // Increments modCount!!
    //在数据中正确的位置上放上元素e，并且size++
    elementData[size++] = e;
    return true;
}

//在指定位置添加一个元素
public void add(int index, E element) {
    rangeCheckForAdd(index);

    //先确保elementData数组的长度足够
    ensureCapacityInternal(size + 1);  // Increments modCount!!
    //将数据整体向后移动一位，空出位置之后再插入，效率不太好
    System.arraycopy(elementData, index, elementData, index + 1,
                         size - index);
    elementData[index] = element;
    size++;
}

// 校验插入位置是否合理
private void rangeCheckForAdd(int index) {
    //插入的位置肯定不能大于size 和小于0
    if (index > size || index < 0)   
        //如果是，就报越界异常
        throw new IndexOutOfBoundsException(outOfBoundsMsg(index));
}

//添加一个集合
public boolean addAll(Collection<? extends E> c) {
    //把该集合转为对象数组
    Object[] a = c.toArray();
    int numNew = a.length;
    //增加容量
    ensureCapacityInternal(size + numNew);  // Increments modCount
    //挨个向后迁移
    System.arraycopy(a, 0, elementData, size, numNew);
    size += numNew;
    //新数组有元素，就返回 true
    return numNew != 0;
}

//在指定位置，添加一个集合
public boolean addAll(int index, Collection<? extends E> c) {
    rangeCheckForAdd(index);

    Object[] a = c.toArray();
    int numNew = a.length;
    ensureCapacityInternal(size + numNew);  // Increments modCount

    int numMoved = size - index;
    //原来的数组挨个向后迁移
    if (numMoved > 0)
        System.arraycopy(elementData, index, elementData, index + numNew,
                         numMoved);
    //把新的集合数组 添加到指定位置
    System.arraycopy(a, 0, elementData, index, numNew);
    size += numNew;
    return nc                       umNew != 0;
}
```

**对数组的容量进行调整**

```java
//确保内部容量够用
private void ensureCapacityInternal(int minCapacity) {
    ensureExplicitCapacity(calculateCapacity(elementData, minCapacity));
}

//计算容量。判断初始化的elementData是不是空的数组,如果是空的话，返回默认容量10与minCapacity=size+1的较大值者。
private static int calculateCapacity(Object[] elementData, int minCapacity) {
    if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
        return Math.max(DEFAULT_CAPACITY, minCapacity);
    }
    return minCapacity;
}

//确认实际的容量，这个方法就是真正的判断elementData是否够用
private void ensureExplicitCapacity(int minCapacity) {
    modCount++;

    //minCapacity如果大于了实际elementData的长度，那么就说明elementData数组的长度不够用，不够用那么就要增加elementData的length。这里有的小伙伴就会模糊minCapacity到底是什么呢，这里解释一下

/**
     * 当我们要 add 进第1个元素到 ArrayList 时，elementData.length 为0 （因为还是一个空的 list），因为执行了 `ensureCapacityInternal()` 方法 ，所以 minCapacity 此时为10。此时，`minCapacity - elementData.length > 0 `成立，所以会进入 `grow(minCapacity)` 方法。
     * 当add第2个元素时，minCapacity 为2，此时e lementData.length(容量)在添加第一个元素后扩容成 10 了。此时，`minCapacity - elementData.length > 0 ` 不成立，所以不会进入 （执行）`grow(minCapacity)` 方法。
     * 添加第3、4···到第10个元素时，依然不会执行grow方法，数组容量都为10。
     * 直到添加第11个元素，minCapacity(为11)比elementData.length（为10）要大。进入grow方法进行扩容。
     */
    // overflow-conscious code
    if (minCapacity - elementData.length > 0)
        //ArrayList能自动扩展大小的关键方法就在这里了
        grow(minCapacity);
}

//扩容核心方法
private void grow(int minCapacity) {
    //将扩充前的elementData大小给oldCapacity
    // overflow-conscious code
    int oldCapacity = elementData.length;
    //新容量newCapacity是1.5倍的旧容量oldCapacity
    int newCapacity = oldCapacity + (oldCapacity >> 1);
    //这句话就是适应于elementData就空数组的时候，length=0，那么oldCapacity=0，newCapacity=0，所以这个判断成立，在这里就是真正的初始化elementData的大小了，就是为10。
    if (newCapacity - minCapacity < 0)
        newCapacity = minCapacity;
    //如果newCapacity超过了最大的容量限制，就调用hugeCapacity，也就是将能给的最大值给newCapacity
    if (newCapacity - MAX_ARRAY_SIZE > 0)
        newCapacity = hugeCapacity(minCapacity);
    //新的容量大小已经确定好了，就copy数组，改变容量大小。
    // minCapacity is usually close to size, so this is a win:
    elementData = Arrays.copyOf(elementData, newCapacity);
}

//这个就是上面用到的方法，很简单，就是用来赋最大值。
private static int hugeCapacity(int minCapacity) {
    if (minCapacity < 0) // overflow
        throw new OutOfMemoryError();
    //如果minCapacity都大于MAX_ARRAY_SIZE，那么就Integer.MAX_VALUE返回，反之将MAX_ARRAY_SIZE返回。因为maxCapacity是三倍的minCapacity，可能扩充的太大了，就用minCapacity来判断了。
	//Integer.MAX_VALUE:2147483647   MAX_ARRAY_SIZE：2147483639  也就是说最大也就能给到第一个数值。还是超过了这个限制，就要溢出了。相当于arraylist给了两层防护。
    return (minCapacity > MAX_ARRAY_SIZE) ?
        Integer.MAX_VALUE :
    MAX_ARRAY_SIZE;
}
```

**remove()方法**

```java
//根据索引删除指定位置的元素
public E remove(int index) {
    //检查index的合理性
    rangeCheck(index);
	//这个作用很多，比如用来检测快速失败的一种标志。
    modCount++;
    //通过索引直接找到该元素
    E oldValue = elementData(index);

    //计算要移动的位数。
    int numMoved = size - index - 1;
    if (numMoved > 0)
        //移动元素，挨个往前移一位。
        System.arraycopy(elementData, index+1, elementData, index,
                         numMoved);
    //将--size上的位置赋值为null，让gc(垃圾回收机制)更快的回收它。
    elementData[--size] = null; // clear to let GC do its work
	//返回删除的元素。
    return oldValue;
}

//从此列表中删除指定元素的第一个匹配项，如果存在，则删除。通过元素来删除该元素，就依次遍历，如果有这个元素，就将该元素的索引传给fastRemobe(index)，使用这个方法来删除该元素，fastRemove(index)方法的内部跟remove(index)的实现几乎一样，这里最主要是知道arrayList可以存储null值
public boolean remove(Object o) {
    if (o == null) {
        //挨个遍历找到目标
        for (int index = 0; index < size; index++)
            if (elementData[index] == null) {
                //快速删除
                fastRemove(index);
                return true;
            }
    } else {
        for (int index = 0; index < size; index++)
            if (o.equals(elementData[index])) {
                fastRemove(index);
                return true;
            }
    }
    return false;
}

//内部方法，“快速删除”，就是把重复的代码移到一个方法里
private void fastRemove(int index) {
    modCount++;
    int numMoved = size - index - 1;
    if (numMoved > 0)
        System.arraycopy(elementData, index+1, elementData, index,
                         numMoved);
    elementData[--size] = null; // clear to let GC do its work
}

//删除或者保留指定集合中的元素
//用于两个方法，一个removeAll()：它只清除指定集合中的元素，retainAll()用来测试两个集合是否有交集。　
private boolean batchRemove(Collection<?> c, boolean complement) {
    //将原集合，记名为A
    final Object[] elementData = this.elementData;
    //r用来控制循环，w是记录有多少个交集
    int r = 0, w = 0;
    boolean modified = false;
    try {
        //遍历 ArrayList 集合
        for (; r < size; r++)
            //参数中的集合c一次检测集合A中的元素是否有
            if (c.contains(elementData[r]) == complement)
                //有的话，就给集合A
                elementData[w++] = elementData[r];
    } finally {
        //发生了异常，直接把 r 后面的复制到 w 后面
        if (r != size) {
            //将剩下的元素都赋值给集合A
            System.arraycopy(elementData, r,
                             elementData, w,
                             size - r);
            w += size - r;
        }
        if (w != size) {
            //这里有两个用途，在removeAll()时，w一直为0，就直接跟clear一样，全是为null。
            //retainAll()：没有一个交集返回true，有交集但不全交也返回true，而两个集合相等的时候，返回false，所以不能根据返回值来确认两个集合是否有交集，而是通过原集合的大小是否发生改变来判断，如果原集合中还有元素，则代表有交集，而元集合没有元素了，说明两个集合没有交集。
            // 清除多余的元素，clear to let GC do its work
            for (int i = w; i < size; i++)
                elementData[i] = null;
            modCount += size - w;
            size = w;
            modified = true;
        }
    }
    return modified;
}


//保留公共的
public boolean retainAll(Collection<?> c) {
    Objects.requireNonNull(c);
    return batchRemove(c, true);
}

//将elementData中每个元素都赋值为null，等待垃圾回收将这个给回收掉
public void clear() {
    modCount++;
    //并没有直接使数组指向 null,而是逐个把元素置为空，下次使用时就不用重新 new 了
    for (int i = 0; i < size; i++)
        elementData[i] = null;

    size = 0;
}
```

**get()方法**

```java
E elementData(int index) {
    return (E) elementData[index];
}

public E get(int index) {
    // 检验索引是否合法，但是只检查了是否大于size,面没有检查是否小于0
    rangeCheck(index);

    return elementData(index);
}

private void rangeCheck(int index) {
    if (index >= size)
        throw new IndexOutOfBoundsException(outOfBoundsMsg(index));
}

```

**set()方法**

```java
//设定指定下标索引的元素值
public E set(int index, E element) {
    // 检验索引是否合法
    rangeCheck(index);
    // 旧值,做了一个向下转型的操作，此方法返回被替换的元素
    E oldValue = elementData(index);
    // 赋新值
    elementData[index] = element;
    // 返回旧值
    return oldValue;
}
```

**indexOf()方法**

```java
// 从首开始查找数组里面是否存在指定元素
public int indexOf(Object o) {
    // 查找的元素为空
    if (o == null) { 
        // 遍历数组，找到第一个为空的元素，返回下标
        for (int i = 0; i < size; i++) 
            if (elementData[i]==null)
                return i;
    } else { 
        // 查找的元素不为空
        // 遍历数组，找到第一个和指定元素相等的元素，返回下标
        for (int i = 0; i < size; i++) 
            if (o.equals(elementData[i]))
                return i;
    } 
    // 没有找到，返回空
    return -1;
}

//返回列表中指定元素最后一次出现的索引，倒着遍历
public int lastIndexOf(Object o) {
    if (o == null) {
        for (int i = size-1; i >= 0; i--)
            if (elementData[i]==null)
                return i;
    } else {
        for (int i = size-1; i >= 0; i--)
            if (o.equals(elementData[i]))
                return i;
    }
    return -1;
}
```

**contains()方法**

```java
//判断是否含有某个元素
public boolean contains(Object o) {
    return indexOf(o) >= 0;
}
```

**toArray()**

```java
/**
     以正确的顺序返回一个包含此列表中所有元素的数组（从第一个到最后一个元素）; 返回的数组的运行时类型是指定数组的运行时类型。 
     */
public Object[] toArray() {
    //elementData：要复制的数组；size：要复制的长度
    return Arrays.copyOf(elementData, size);
}

public <T> T[] toArray(T[] a) {
    //如果只是要把一部分转换成数组
    if (a.length < size)
        // Make a new array of a's runtime type, but my contents:
        return (T[]) Arrays.copyOf(elementData, size, a.getClass());
    //全部元素拷贝到 数组 a
    System.arraycopy(elementData, 0, a, 0, size);
    if (a.length > size)
        a[size] = null;
    return a;
}

```

## 11.2 HashMap源码解析

| 指标                     | JDK1.7                                                       | JDK1.8                                                       |
| ------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 存储结构                 | 数组 + 链表                                                  | 数组 ＋ 链表 ＋红黑树                                        |
| 初始化方式               | 单独函数：inflateTable()                                     | 直接集成到了扩容函数resize()中                               |
| hash值计算方式           | 扰动处理 = 9 次扰动 = 4 次位运算 + ５次异或运算              | 扰动处理 = ２ 次扰动 = １次位运算 + １次异或运算             |
| 存放数据的规则           | 无冲突时，存放数组；冲突时，存放链表                         | 无冲突时，存放数组；冲突 & 链表长度 < 8：存放单链表；冲突 & 链表长度 > 8：树化并存放红黑树 |
| 插入数据方式             | 头插法（先讲原位置的数据移到后1位，再插入数据到该位置）      | 尾插法（直接插入到链表尾部/红黑树）                          |
| 扩容后存储位置的计算方式 | 全部按照原来方法进行计算（即hashCode ->> 扰动函数 ->> (h&length-1)） | 按照扩容后的规律计算（即扩容后的位置=原位置 or 原位置 + 旧容量） |

![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL0pvdXJXb24vaW1hZ2UvbWFzdGVyL0phdmElRTklOUIlODYlRTUlOTAlODglRTYlQkElOTAlRTclQTAlODElRTglQUYlQTYlRTglQTclQTMlRTQlQjklOEJIYXNoTWFwL0hhc2hNYXAlRTclQkIlQTclRTYlODklQkYlRTUlODUlQjMlRTclQjMlQkIlRTUlOUIlQkUucG5n?x-oss-process=image/format,png)

**成员变量**

```java
//默认初始化Node数组容量16
static final int DEFAULT_INITIAL_CAPACITY = 1 << 4;
//最大的数组容量
static final int MAXIMUM_CAPACITY = 1 << 30;
//默认负载因子0.75
static final float DEFAULT_LOAD_FACTOR = 0.75f;
//由链表转红黑树的临界值
static final int TREEIFY_THRESHOLD = 8;
//由红黑树转链表的临界值
static final int UNTREEIFY_THRESHOLD = 6;
//桶转化为树形结构的最小容量
static final int MIN_TREEIFY_CAPACITY = 64;
//HashMap结构修改的次数，结构修改是指更改HashMap中的映射数或以其他方式修改其内部结构(例如，rehash的修改)。该字段用于在Collection-views上快速生成迭代器。
transient int modCount;  
//Node数组下一次扩容的临界值，第一次为16*0.75=12（容量*负载因子）
int threshold;
//负载因子
final float loadFactor;
//map中包含的键值对的数量
transient int size;
//表数据，即Node键值对数组，Node是单向链表，它实现了Map.Entry接口，总是2的幂次倍
//Node<K,V>是HashMap的内部类，实现Map.Entry<K,V>接口，HashMap的哈希桶数组中存放的键值对对象就是Node<K,V>。类中维护了一个next指针指向链表中的下一个元素。值得注意的是，当链表中的元素数量超过TREEIFY_THRESHOLD后会HashMap会将链表转换为红黑树，此时该下标的元素将成为TreeNode<K,V>,继承于LinkedHashMap.Entry<K,V>，而LinkedHashMap.Entry<K,V>是Node<K,V>的子类，因此HashMap的底层数组数据类型即为Node<K,V>。
transient Node<K,V>[] table;
//存放具体元素的集,可用于遍历map集合
transient Set<Map.Entry<K,V>> entrySet;
```

**构造方法**

```java
//初始化容量以及负载因子
public HashMap(int initialCapacity, float loadFactor) {
    //判断初始化数组的容量大小
    if (initialCapacity < 0)
        throw new IllegalArgumentException("Illegal initial capacity: " +
                                           initialCapacity);
    if (initialCapacity > MAXIMUM_CAPACITY)
        initialCapacity = MAXIMUM_CAPACITY;
    //判断初始化的负载因子大小和是否为浮点型
    if (loadFactor <= 0 || Float.isNaN(loadFactor))
        throw new IllegalArgumentException("Illegal load factor: " +
                                           loadFactor);
    //初始化负载因子
    this.loadFactor = loadFactor;
    this.threshold = tableSizeFor(initialCapacity);
}  

//初始化容量
public HashMap(int initialCapacity) {  
    this(initialCapacity, DEFAULT_LOAD_FACTOR);  
}  

//默认构造方法
public HashMap() {  
    this.loadFactor = DEFAULT_LOAD_FACTOR; // all other fields defaulted  
}  
 
//把另一个Map的值映射到当前新的Map中
public HashMap(Map<? extends K, ? extends V> m) {  
    this.loadFactor = DEFAULT_LOAD_FACTOR;  
    putMapEntries(m, false);  
}  

```

- 初始容量默认为16,负载因子默认为0.75

当我们自定义HashMap初始容量大小时，构造函数并非直接把我们定义的数值当做HashMap容量大小，而是把该数值当做参数调用方法tableSizeFor，然后把返回值作为HashMap的初始容量大小.

```java
//HashMap 中 table 角标计算及table.length 始终为2的幂，即 2 ^ n
//返回大于initialCapacity的最小的二次幂数值
static final int tableSizeFor(int cap) {
    int n = cap - 1;
    n |= n >>> 1;
    n |= n >>> 2;
    n |= n >>> 4;
    n |= n >>> 8;
    n |= n >>> 16;
    return (n < 0) ? 1 : (n >= MAXIMUM_CAPACITY) ? MAXIMUM_CAPACITY : n + 1;
}
```

**静态内部类**

HashMap将hash, key, value, next 已经封装到一个静态内部类Node上

```java
static class Node<K,V> implements Map.Entry<K,V> {
    // 哈希值，HashMap根据该值确定记录的位置
    final int hash;
    // node的key
    final K key;
    // node的value
    V value;
    // 链表下一个节点
    Node<K,V> next;

    // 构造方法
    Node(int hash, K key, V value, Node<K,V> next) {
        this.hash = hash;
        this.key = key;
        this.value = value;
        this.next = next;
    }

    // 返回 node 对应的键
    public final K getKey()        { return key; }
    // 返回 node 对应的值
    public final V getValue()      { return value; }
    public final String toString() { return key + "=" + value; }

    public final int hashCode() {
        return Objects.hashCode(key) ^ Objects.hashCode(value);
    }

    public final V setValue(V newValue) {
        V oldValue = value;
        value = newValue;
        return oldValue;
    }

    //作用：判断2个Entry是否相等，必须key和value都相等，才返回true
    public final boolean equals(Object o) {
        if (o == this)
            return true;
        if (o instanceof Map.Entry) {
            Map.Entry<?,?> e = (Map.Entry<?,?>)o;
            if (Objects.equals(key, e.getKey()) &&
                Objects.equals(value, e.getValue()))
                return true;
        }
        return false;
    }
}

```

**TreeNode**

继承于LinkedHashMap.Entry<k,v>,而LinkedHashMap.Entry<k,v>是Node<K,V>的子类,因此HashMap的底层数组数据类型即为Node<K,V>

```java
/**
  * 红黑树节点 实现类：继承自LinkedHashMap.Entry<K,V>类
  */
static final class TreeNode<K,V> extends LinkedHashMap.Entry<K,V> {  

    // 属性 = 父节点、左子树、右子树、删除辅助节点 + 颜色
    TreeNode<K,V> parent;  
    TreeNode<K,V> left;   
    TreeNode<K,V> right;
    TreeNode<K,V> prev;   
    boolean red;   

    // 构造函数
    TreeNode(int hash, K key, V val, Node<K,V> next) {  
        super(hash, key, val, next);  
    }  

    // 返回当前节点的根节点  
    final TreeNode<K,V> root() {  
        for (TreeNode<K,V> r = this, p;;) {  
            if ((p = r.parent) == null)  
                return r;  
            r = p;  
        }  
    }
}

```

```java
// 取key的hashCode值、高位运算、取模运算
// 在JDK1.8的实现中，优化了高位运算的算法，
// 通过hashCode()的高16位异或低16位实现的：(h = k.hashCode()) ^ (h >>> 16)，
// 主要是从速度、功效、质量来考虑的，这么做可以在数组table的length比较小的时候，
// 也能保证考虑到高低Bit都参与到Hash的计算中，同时不会有太大的开销。
static final int hash(Object key) {
    int h;
    return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}
```

- 首先获取对象的hashCode()值，然后将hashCode值右移16位，然后将右移后的值与原来的hashCode做**异或**运算，返回结果。（其中h>>>16，在JDK1.8中，优化了高位运算的算法，使用了零扩展，无论正数还是负数，都在高位插入0）。