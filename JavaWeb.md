# JavaWeb



## 1. Tomcat

>域名的解析过程：在 windows 中，先找  C:\Windows\System32\drivers\etc\hosts 中是否有域名所对应的 ip 地址，如果有，那么就请求该 ip 地址 ;如果没有，那么就会去 DNS 服务器上找域名对应的 ip 地址，如果没有找到，那么 DNS 服务器中就不存在这个域名。

**常用端口**

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

### 3.3 阿里云镜像

### 3.4 本地仓库

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

