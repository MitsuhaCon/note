# JavaWeb



## 1. Tomcat

>域名的解析过程：在 windows 中，先找  C:\Windows\System32\drivers\etc\hosts 中是否有域名所对应的 ip 地址，如果有，那么就请求该 ip 地址 ;如果没有，那么就会去 DNS 服务器上找域名对应的 ip 地址，如果没有找到，那么 DNS 服务器中就不存在这个域名。

**常用默认端口**

tomcat -> 8080

http -> 80

https -> 443

mysql -> 3306

oracle -> 1521

redis -> 6379

ftp -> 21

### 1.1 Tomcat 解压后目录分析

## 2. HTTP

## 3. Maven

### 3.1 下载安装

### 3.2 配置环境变量

```
MAVEN_HOME  D:\Software\apache-maven-3.6.1
然后在 PATH 中添加 %MAVEN_HOME%/bin
```



### 3.3 阿里云镜像

```xml
<mirror>
	<id>nexus-aliyun</id>
	<mirrorOf>central</mirrorOf>
	<name>Nexus aliyun</name>
	<url>http://maven.aliyun.com/nexus/content/groups/public</url>
</mirror>
```



### 3.4 本地仓库

```xml
<!-- 本地仓库自定义 --> 
<localRepository>D:\Document\Maven_Repository</localRepository>
```

### 3.5 在 IDEA 中使用 Maven

### 3.6 POM 文件

```pom
<!-- 在 build 中配置 resources，来防止我们资源导出失败的问题-->
<build>
      <resources>
      <!-- src/main/resources 的文件不用下面的第一个 resource，否则会加载不来，应该注释掉-->
        <resource>
            <directory>src/main/resources</directory>
            <excludes>
                <exclude>**/*.properties</exclude>
                <exclude>**/*.xml</exclude>
             </excludes>
            <filtering>false</filtering>
        </resource>
        <resource>
            <directory>src/main/java</directory>
            <includes>
                <include>**/*.properties</include>
                <include>**/*.xml</include>
            </includes>
            <filtering>false</filtering>
        </resource>
    </resources>
</build>
```

## 4. Servlet

### 4.1 Servlet 简介

### 4.2 HelloServlet

### 4.3 Servlet 原理

### 4.4 Mapping 映射问题

一个 servlet 可以配置多个映射路径

通配符映射优先级没有指定的映射路径高

## 5. ServletContext

一个 web 应用中，所有的 servlet 共用一个 ServletContext ，

```java
ServletContext context = this.getServletContext();
context.getRequestDispatcher("/go").forward(request,response);
```

### 5.1 ServletContext 做数据共享

### 5.2 获取初始化参数

```xml
在  web.xml  中指定初始化参数
<context-param>
	<param-name>name</init-name>
    <param-value>杨不知</init-value>
</context-param>
```

### 5.3 请求转发

### 5.4 读取资源文件

### 5.5 HttpServletRequest

HttpServletRequest 代表客户端的请求，用户通过Http协议访问服务器，HTTP请求中的所有信息会被封装到 HttpServletRequset ,通过这个 HttpServletRequest的方法，获得客户端的所有信息

### 5.6 HttpServletResponse

文件下载

![image-20200821234442598](JavaWeb.assets/image-20200821234442598.png)

面试题：请你聊聊重写向和转发的区别

重写向：代码是 302   转发：代码是 307

```java
// response.setHeader("Location","/路径");
// response.setStatus(302);
// 执行下面一句相当于执行了上面两句
response.sendRedirect("/路径")；
```

相同点：页面都可能实现跳转

不同点：请求转发的时候，URL 地址不会产生变化，重定向会改变URL 



```jsp
<form action="${pageContext.request.contextPath}" method="post">
    
</form>
```

## 6. Cookie、Session

网络编程中，使用 URLEncoder.encode("杨不知","utf-8");转码

URLDecoder.decode("杨不知","utf-8"); 解码

### 6.1 Cookie

### 6.2 Session

1. 服务器会给每一个用户创建一个 session
2.  一个 session 独占一个浏览器，只要浏览器没有关闭，这个 session 就存在
3. 创建 Session 的时候  会将 这个 Session 写到一个 Cookie 里面  响应给浏览器

**Cookie 和 Session 的区别**

- cookie 是把用户的数据写给用户的浏览器，浏览器可心保存多个
- session 把用户的数据写到用户独占 session 中，服务器保存（一般保存的是重要的信息，减少服务器的压力和资源浪费）
- session 是服务器创建的

可以在 web.xml 中设置 session 默认的失效时间

```xml
<session-config>
    <!-- 单位是分钟-->
	<session-timeout>1</session-timeout>
</session-config>
```

## 7. Java Server Pages (JSP)

IDEA中部署项目，会把项目放在 `C:\Users\MitsuhaCon\.IntelliJIdea2019.3\system\tomcat`

JSP本质上就是一个 HttpServlet

```pom
<!-- https://mvnrepository.com/artifact/javax.servlet/javax.servlet-api -->
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <version>3.1.0</version>
</dependency>

<!-- https://mvnrepository.com/artifact/javax.servlet.jsp/javax.servlet.jsp-api -->
<dependency>
    <groupId>javax.servlet.jsp</groupId>
    <artifactId>javax.servlet.jsp-api</artifactId>
    <version>2.3.1</version>
</dependency>

<!-- https://mvnrepository.com/artifact/javax.servlet.jsp.jstl/jstl-api -->
<dependency>
    <groupId>javax.servlet.jsp.jstl</groupId>
    <artifactId>jstl-api</artifactId>
    <version>1.2</version>
</dependency>

<!-- https://mvnrepository.com/artifact/taglibs/standard -->
<dependency>
    <groupId>taglibs</groupId>
    <artifactId>standard</artifactId>
    <version>1.1.2</version>
</dependency>


```

><%=%> 表达式
>
><%
>
>​	这里面的代码会被编译到一个方法中
>
>%>
>
><%!  
>
>​	申明全局变量，方法，会被编译到 java的类中
>
>%>



JSP 指令

<%@ page %>

<%@ include file=""%> 会合并成一个 servlet

<jsp:include page=""/> 将页面单独生成 servlet

**九大内置对象**

1. PageContext    保存的数据只在一个页面中有效
2. Request  保存的数据在一次请求中
3. Response
4. Session  保存的数据只在一个会话中有效  ，从得到session 到 浏览器关闭 
5. Application  对应的 ServletContext 类  保存的数据在服务器中有效，只要服务器不关就存在
6. config
7. out
8. page
9. exception

## 8. Filter

自定义 filter 必须 实现  javax.servlter.Filter 接口

```java
public class MyFilter extent Filter {
    // 重写三个方法
    // chian.dofilter(req,res);
    
    public void init(){
	
    }
    
    public void doFilter(){
        chian.dofilter(req,res);
    }
    
    public void destory(){
        
    }
}
```



```xml
在 web.xml中配置 filter,这个 filter 是在 tomcat启动时，就初始化 filter。服务器关闭的时候  filter 就会被销毁

<filter>
	<filter-name></filter-name>
    <filter-class></filter-class>
</filter>

<filter-mapping>
	<filter-name></filter-name>
    <url-patten></url-patten>
</filter-mapping>
```

## 9. Listener 监听器

```java
// 表示正常退出
System.exit(0);

// 表示非正常退出
System.exit(非零整数)
```

