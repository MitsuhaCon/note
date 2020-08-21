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

### 5.6 HttpServletResponse

文件下载

![image-20200821234442598](JavaWeb.assets/image-20200821234442598.png)

