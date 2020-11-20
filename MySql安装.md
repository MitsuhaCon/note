# 1、安装包下载

> https://dev.mysql.com/downloads/mysql/

# 2、解压压缩包

> 把解压好的包放到自己的软件安装目录下

# 3、配置环境变量

> 变量名：MYSQL_HOME
>
> 变量值：E:\software\mysql-8.0.22-winx64

# 4、生成data文件

> 以管理员身份运行 `cmd`
>
> 切换路径到安装目录下的 `bin`
>
> 执行：mysqld --initialize-insecure --user=mysql
>
> `--user=mysql 是创建了一个名为mysql的数据库`

# 5、安装MySql

> 在 `bin` 目录下执行：mysqld -install
>
> 启动服务: net start mysql
>
> 服务启动成功即可

# 6、登录MySql

>`bin` 下执行： mysql -u root -p
>
>由于新安装没有设置密码直接回车跳过密码

# 7、重置密码

> `alter user 'root'@'localhost' identified with mysql_native_password by '123456'`
>
> flush privileges



# > > > > > > > > > > > > > > > > > > > > > > > >Linux下安装

# 1、安装Yum Repository

```shell
[root@localhost ~]# wget https://repo.mysql.com//mysql80-community-release-el8-1.noarch.rpm
```

# 2、使用rpm来安装MySQL

```shell
[root@localhost ~]# rpm -ivh mysql80-community-release-el8-1.noarch.rpm
```

# 3、使用yum安装mysql服务

```shell
[root@localhost ~]# yum install mysql-server
```

# 4、检查是否已经设置为开机启动MySQL服务

```shell
[root@localhost ~]# systemctl list-unit-files|grep mysqld
mysqld.service disabled
mysqld@.service disabled
[root@localhost ~]# systemctl enable mysqld.service  #设置开机启动
Created symlink /etc/systemd/system/multi-user.target.wants/mysqld.service → /usr/lib/systemd/system/mysqld.service.
[root@localhost ~]# systemctl list-unit-files|grep mysqld
mysqld.service enabled
mysqld@.service disabled

[root@localhost ~]# ps -ef|grep mysql # 查看是否启动MySQL服务
root 4311 32702 0 21:07 pts/4 00:00:00 grep --color=auto mysql
[root@localhost ~]# systemctl start mysqld.service #启动服务
```

# 5、添加密码及安全设置

```shell
[root@localhost~]# mysql_secure_installation
是否允许远程连接时，一定要选择允许，请认真请问题再回答，如回答错误，请重新进行设置
```

```shell
授权，这样navicat就能访问了
#mysql8以上需要先创建用户，再给用户授权
CREATE USER 'root'@'%' IDENTIFIED BY '密码';
grant all privileges on *.* to 'root'@'%';
```

