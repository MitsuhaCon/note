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

## 2.4、