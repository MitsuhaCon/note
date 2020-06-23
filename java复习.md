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

