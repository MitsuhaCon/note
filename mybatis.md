# 1、简介

## 1.1、什么是 Mybatis

## 1.2、持久化

数据持久化，将数据保存在数据库中

## 1.3、持久层

DAO 层、Service 层、Controller 层

- 完成持久化工作的代码块
- 层界限十分明显

## 1.4、为什么需要 Mybatis?

- 帮助程序员将数据存入到数据库中
- 方便

# 2、第一个 Mybatis 程序

## 2.1、搭建环境

1. 创建一个新的 maven 项目

2. 删除 src 目录、

3. 在 pom.xml 文件中添加 oracle 或 mysql 的依赖、以及 junit 、mybatis 的依赖

    >如何导入一个 jar 包到 maven 仓库中
    >
    >通过 cmd：当前目录下必有要有 对应的包。
    >
    >D:\Document\Maven_Repository>mvn install:install-file -Dfile=ojdbc6.jar -DgroupId=com.oracle -DartifactId=ojdbc6 -Dversion=11.2.0.1.0 -Dpackaging=jar

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <project xmlns="http://maven.apache.org/POM/4.0.0"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
        <modelVersion>4.0.0</modelVersion>
    
        <groupId>com.mitsuha</groupId>
        <artifactId>mybatis-learn</artifactId>
        <packaging>pom</packaging>
        <version>1.0-SNAPSHOT</version>
        <modules>
            <module>learn-01</module>
        </modules>
    
        <dependencies>
            <dependency>
                <groupId>org.mybatis</groupId>
                <artifactId>mybatis</artifactId>
                <version>3.5.2</version>
            </dependency>
            
            <dependency>
                <groupId>com.oracle</groupId>
                <artifactId>ojdbc6</artifactId>
                <version>11.2.0.1.0</version>
            </dependency>
            
            <dependency>
                <groupId>junit</groupId>
                <artifactId>junit</artifactId>
                <version>4.12</version>
            </dependency>
        </dependencies>
    
        <!-- 在 build 中配置 resources，来防止我们 java 中的 xml 资源不会 build 到 target 中-->
        <build>
            <resources>
                <resource>
                    <directory>src/main/java</directory>
                    <includes>
                        <include>**/*.properties</include>
                        <include>**/*.xml</include>
                        <include>*.xml</include>
                    </includes>
                    <filtering>true</filtering>
                </resource>
            </resources>
        </build>
    
        <!-- 指定编译器版本 -->
        <!-- 阻止 Idea 中提示：Warning:java: 源值1.5已过时, 将在未来所有发行版中删除-->
        <properties>
            <maven.compiler.source>1.8</maven.compiler.source>
            <maven.compiler.target>1.8</maven.compiler.target>
        </properties>
    </project>
    ```

## 2.2、创建一个模块（modules）

## 2.3、编写代码

代码见仓库

# 3、配置解析

## 3.1、环境配置  environments

## 3.2、属性 properties

## 3.3、类型别名 typeAliases

```xml
<typeAliases>
    <!-- 给一个类取别名，这个别名可以自定义-->
	<typeAlias type="com.mitsuha.pojo.Emp" alias="emp"/>
    <!-- 扫描包下的所有类，并将这些类名做为别名，不能自定义别名-->
	<package name="com.mitsuha.pojo"/>
</typeAliases>
```

> 可以通过注解的形式进行取别名，这个时候别名可以自定义‘

```java
@Alias("emp")
public class Emp(){}
```

## 3.4、设置 setting

- mapUnderscoreToCamelCase，是否开启驼峰命名自动映射，即从经典数据库列名 A_COLUMN 映射到经典 Java 属性名 aColumn。
- logImpl，

## 3.5、映射器 mappers

方式一：通过 resource

```xml
<mappers>
	<mapper resource="com/mitsuha/dao/EmpMapper.xml"></mapper>
</mappers>
```

方式二：通过 class绑定文件

```xml
<mappers>
	<mapper class="com.mitsuha.dao.EmpMapper"></mapper>
</mappers>
```

**方式二需要注意的点：**

- 接口和他对应的 Mapper 配置文件必须同名
- 接口和他对应的 Mapper 配置文件必须在同一个包下

方式三：通过 package

```xml
<mapper>
	<package name="com.mitsuha.dao"/>
</mapper>
```

**方式三需要注意的点：**

- 接口和他对应的 Mapper 配置文件必须同名
- 接口和他对应的 Mapper 配置文件必须在同一个包下

## 2.6、生命周期和作用域

# 4、解决属性名和字段名不一致的问题

# 5、分页

```sql
-- mysql 用 limit 进行分页
-- 语法  limit startIndex,pagesize
```

- 通过 sql 进行分页
- 通过 Rowbounds 的对象方式
- 通过 MyBatis 分页插件 PageHelper 进行分页

# 6、使用注解开发

```java
// 注解在接口中的方法上使用，适用于简单的 sql 语句，方法中有多个参数，就得用 @Param 注解每个基本类型的参数
@Select("select * from emp where empno = #{empno}")
Emp getEmpByAnnotation(int empno);
```

```xml
<mappers>
	<mapper class="com.mitsuha.dao.EmpDao"></mapper>
</mappers>
```

关于 @Param() 注解

- 基本类型的参数或者 String 类型的参数都需要加上 @Param
- 引用类型不需要加
- 如果只有一个基本类型的话，可以忽略，但是建议都加上
- sql 中引用的是@Param 里面的值

# 走读 Mybatis 执行流程的代码 

# 7、多对一处理

# 8、一对多处理

# 9、动态 SQL

- if

- choose（when，otherwise）

- trim（where，set）

- SQL 片段

- Foreach

    ```xml
    <foreach item="item" index="index" collection="list" 
             open="(" separator= "," close=")">
    </foreach>
    ```

# 10、Mybatis 缓存

- 一级缓存，默认打开，sqlsession 级别的，增删改操作，会刷新缓存，可以通过 sqlsession.clearCache(); 手动
- 二级缓存，也叫全局缓存，一级缓存作用域太低了，所以诞生了二级缓存，基于 namespace级别的缓存，一个名称空间，对应一个二级缓存
- 工作机制
    - 一个会话查询一条数据，这个数据就会被放在当前会话的一级缓存中
    - 如果当前会话关闭了，这个会话对应的一级缓存就没了，但是我们想要的是，会话关闭了，一级缓存中的数据被保存到二级缓存中
    - 新的会话查询信息，就可以从二级缓存中获取内容
    - 不同的 mapper查出的数据会放在自己对应的缓存(map)中

```
在对应的 mapper.xml中 使用<cache/> 就开启了二级缓存
<cache/>
通过这种方式，实体类可以不用序列化

<cache eviction="FIFO" flushInterval="60000" size="512" readOnly="true"/>
通过这种方式，实体类就必须序列化，否则会报错
```

先看二级缓存有没有数据，没有再去一级缓存，最后都会去数据库拿数

> 通过 Ehcache 来自定义缓存