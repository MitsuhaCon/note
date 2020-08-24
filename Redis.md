# Nosql 概述

大数据时代。一般的数据库无法进行分析处理了，2006年出的 Hadoop。

大数据时代的 3V 和 3高

推荐文章：阿里云的这群疯子

# 1、安装

> CentOS

```shell
wget http://download.redis.io/releases/redis-最新稳定版本.tar.gz

tar xzf redis-最新稳定版本.tar.gz  别名

cd 别名

make

# make 完可以执行 make install 进行确认
```

> 默认安装的程序在  /usr/local/bin 下面

```shell
在 /usr/local/bin 下执行启动也可以

# 指定配置文件启动服务
redis-server 指定的配置文件路径
```



**测试**

```shell
# 一个窗口中进行启动服务
cd src

./redis-server

# 开户另一个窗口进行 redis-lci
cd src

./redis-cli

进行 redis-cli 后  可以 ping  -> 得到 pong则成功
要退出服务  则用  shutdown
```

# 2、配置

命令格式：`config get config_setting_name`

```shell
# eg:获取指定的 配置名称
config get loglevel

# eg:获取所有的 配置名称
config get *
```

## 2.1、编辑配置

语法：`config set config_setting_name new_config_value`

```shell
# eg:
config set loglevel "notice"
config get loglevel
```

## 2.2、参数说明

redis.conf 配置项说明如下：

| 序号 | 配置项                     | 说明                                                         |
| ---- | -------------------------- | ------------------------------------------------------------ |
| 1    | daemonize  [yes\|no]       | redis 默认不是以守护进程方式运行的，可以将 daemonize 设置为 yes 以开启守护进程 (widows 不支持守护线程) |
| 2    | pidfile /var/run/redis.pid | 当 redis 以守护进程方式运行时，redis 默认会把 pid 写入 /var/run/redis.pid 文件中，可以通过 pidfile 指定 |
| 3    | port 6379                  | 指定 redis 的监听端口                                        |
| 4    | bind 127.0.0.1             | 绑定的主机地址                                               |
| 5    | timeout                    | 设置多久无操作就超时                                         |
|      |                            |                                                              |
|      |                            |                                                              |
|      |                            |                                                              |
|      |                            |                                                              |

## 2.3、测试性能  redis-benchmark

redis-benchmark 是一个压力测试工具，官方自带的性能测试工具，

redis-benchmark 命令参数：

语法： redis-benchmark [option] [option value]

**注意**：该命令是在 redis 的目录下执行的，而不是 redis 客户端的内部指令。

|      |           |                                            |           |
| :--- | :-------- | :----------------------------------------- | :-------- |
| 序号 | 选项      | 描述                                       | 默认值    |
| 1    | **-h**    | 指定服务器主机名                           | 127.0.0.1 |
| 2    | **-p**    | 指定服务器端口                             | 6379      |
| 3    | **-s**    | 指定服务器 socket                          |           |
| 4    | **-c**    | 指定并发连接数                             | 50        |
| 5    | **-n**    | 指定请求数                                 | 10000     |
| 6    | **-d**    | 以字节的形式指定 SET/GET 值的数据大小      | 3         |
| 7    | **-k**    | 1=keep alive 0=reconnect                   | 1         |
| 8    | **-r**    | SET/GET/INCR 使用随机 key, SADD 使用随机值 |           |
| 9    | **-P**    | 通过管道传输 <numreq> 请求                 | 1         |
| 10   | **-q**    | 强制退出 redis。仅显示 query/sec 值        |           |
| 11   | **--csv** | 以 CSV 格式输出                            |           |
| 12   | **-l**    | 生成循环，永久执行测试                     |           |
| 13   | **-t**    | 仅运行以逗号分隔的测试命令列表。           |           |
| 14   | **-I**    | Idle 模式。仅打开 N 个 idle 连接并等待。   |           |

```shell
# 在 redis-benchmark 目录下，进行 一百个连接数  同时发 10 万个请求
redis-benchmark -h localhost -p 6379 -c 100 -n 100000
```

## 2.4、基础知识

redis 默认有 16 个数据库，在 redis-config 中有说明

```shell
# 在 redis-cli 下，通过 select 选择几号数据库;例如，选择第三号数据库，下角标是从 0 开始的
select 2
```

```shell
# 通过  dbsize   查看数据库的大小
dbsize
```

```shell
# 通过 keys * 查看当前数据库的所有 key
keys * 
```

```shell
# 通过 flushdb 删除当前数据库的所有数据
# 通过 flushall 删除所有数据库的所有数据
```

> Redis 是单线程的

明白 redis 是很快的，redis 是基于内存操作的，能用单线程，就用单线程，内存中已经很快了，线程之前切换是需要时间的，所以就用了单线程了

## 2.5、五大数据类型

- Redis-key

    ```shell
     keys *
     
     # 判断当前的 name 是否存在
     exists name  
     
     # 移除当前库的一个 key
     del key
     
     # 移动当前的 key 到哪个数据库，数据库是从 0 开始的
     move name 1
     
     # 清空当前终端
     clear
     
     # 设置一个 key 多少秒过期
     expire key seconds
     
     # 查看一个 key 还有多少秒过期
     ttl key
     
     # 查看 key 是什么类型
     type key
    ```

    

- String

    ```shell
    127.0.0.1:6379> set key value
    OK
    
    127.0.0.1:6379> get key
    "value"
    
    127.0.0.1:6379> append key "append"  # 向 key 的值追加字符串
    (integer) 11
    127.0.0.1:6379> get key
    "valueappend"
    
    127.0.0.1:6379> strlen key # 字符串的长度 
    (integer) 11
    
    127.0.0.1:6379> exists key # 查看这个 key 是否存在
    (integer) 1
    
    127.0.0.1:6379> incr view  # 自增 1
    (integer) 3
    127.0.0.1:6379> decr view # 自减 1
    (integer) 2 
    127.0.0.1:6379> decr view # 自减 1
    (integer) 1
    127.0.0.1:6379> incrby view 3 # 自增步长为 3
    (integer) 4
    127.0.0.1:6379> decrby view 3 # 自减步长为 3
    (integer) 1
    
    127.0.0.1:6379> set key "abcd"
    OK
    127.0.0.1:6379> getrange key 1 2  # 截取字符串，从 0 开始
    "bc"
    127.0.0.1:6379> getrange key 0 -1  # end 给 -1 就是从 0 开始 截取到字符串最后
    "abcd"
    127.0.0.1:6379> setrange key 1 xxx  # 从当前字符位置开始用 xxx替换，xxx是几个字符就替换几个，如果原字符串不够,那么就追加
    (integer) 4
    127.0.0.1:6379> get key
    "axxx"
    127.0.0.1:6379> setex key 20 value  # set with expire 设置 key 20 后秒过期
    OK
    127.0.0.1:6379> setnx key value2  # set if no exist 如果 key 不存在，则创建，如果 key 存在，则不做任何操作
    (integer) 1
    127.0.0.1:6379> mset k1 v1 k2 v2 k3 v3  # 批量 set 值
    OK
    127.0.0.1:6379> keys *
    1) "k2"
    2) "k3"
    3) "k1"
    127.0.0.1:6379> mget k1 k2 # 批量获得值
    1) "v1"
    2) "v2"
    
    127.0.0.1:6379> msetnx k1 v1 k4 v4  # 执行 msetnx 具有原子性，就是同一个事务，要么全部成功，要么全部失败
    (integer) 0
    127.0.0.1:6379> get k4
    (nil)
    
    127.0.0.1:6379> getset yang li  # 先 get 后 set 
    (nil)
    127.0.0.1:6379> getset yang ming
    "li"
    127.0.0.1:6379> get yang
    "ming"
    
    127.0.0.1:6379> mset user:1:name yang user:1:age 18
    OK
    127.0.0.1:6379> mget user:1:name user:1:age
    1) "yang"
    2) "18"
    
    
    
    
    ```

- List

    在 redis 里面，我们可以把 list 玩成 栈、队列、阻塞队列

    **只要根 list 有关的操作，都是以 L 开头，lpush 和 left 恰巧都是 L 开头**

    ```shell
    127.0.0.1:6379> lpush list one  # 从左边 push 值
    (integer) 1
    127.0.0.1:6379> lrange list 0 -1 # 取值得用 lrange
    1) "one"
    127.0.0.1:6379> lpush list two
    (integer) 2
    127.0.0.1:6379> lrange list 0 -1
    1) "two"
    2) "one"
    127.0.0.1:6379> rpush list three
    (integer) 3
    127.0.0.1:6379> lrange list 0 -1
    1) "two"
    2) "one"
    3) "three"
    
    127.0.0.1:6379> lrange list 0 -1
    1) "two"
    2) "one"
    3) "three"
    127.0.0.1:6379> lpop list  # 从 list 的最左边移除第一个数据
    "two"
    127.0.0.1:6379> rpop list  # 从 list 的最右边移除第一个数据
    "three"
    127.0.0.1:6379> lrange list 0 -1
    1) "one"
    
    127.0.0.1:6379> lindex list 0  # 通过索引取值
    "one"
    
    127.0.0.1:6379> llen list  # list 的长度
    
    127.0.0.1:6379> lrange list 0 -1
    1) "four"
    2) "three"
    3) "two"
    4) "one"
    127.0.0.1:6379> lrem list 1 one  # 移除 list 中值为 one 的，移除一个
    (integer) 1
    127.0.0.1:6379> lrange list 0 -1
    1) "four"
    2) "three"
    3) "two"
    
    127.0.0.1:6379> ltrim list 1 2  # 截取 list 中的值，然后 list 就变成了 截取后的值了
    OK
    127.0.0.1:6379> lrange list 0 -1
    1) "three"
    2) "two"
    ##############################################################
    127.0.0.1:6379> lpush my one
    (integer) 1
    127.0.0.1:6379> lpush my two
    (integer) 2
    127.0.0.1:6379> lpush my three
    (integer) 3
    127.0.0.1:6379> lpush my four
    (integer) 4
    127.0.0.1:6379> lrange my 0 -1
    1) "four"
    2) "three"
    3) "two"
    4) "one"
    
    # 只能从尾往头压
    127.0.0.1:6379> rpoplpush my test   # 先 从 my 中弹出元素，并将这个元素压入 test 中
    "one"
    127.0.0.1:6379> lrange test 0 -1
    1) "one"
    127.0.0.1:6379> 
    
    # 如果 test 不存在 或者 下角标越界 都会报错
    127.0.0.1:6379> lset test 0 two  # 将列表中指定下标的值替换为另一个值，相当于更新操作
    OK
    127.0.0.1:6379> lrange test 0 -1
    1) "two"
    
    # 在指定 list 元素的前或后插入一个元素  只有  linsert ,没有 rinsert,但是linsert是从左到右找 pivot
    127.0.0.1:6379> lpush list one
    (integer) 1
    127.0.0.1:6379> lpush list two
    (integer) 2
    127.0.0.1:6379> lpush list three
    (integer) 3
    127.0.0.1:6379> lpush list four
    (integer) 4
    127.0.0.1:6379> lrange list 0 -1
    1) "four"
    2) "three"
    3) "two"
    4) "one"
    127.0.0.1:6379> linsert list before two insert  # 在 list 中 two 前插入 insert
    (integer) 5
    127.0.0.1:6379> lrange list 0 -1
    1) "four"
    2) "three"
    3) "insert"
    4) "two"
    5) "one"
    127.0.0.1:6379> linsert list after two insert1
    (integer) 6
    127.0.0.1:6379> lrange list 0 -1
    1) "four"
    2) "three"
    3) "insert"
    4) "two"
    5) "insert1"
    6) "one"
    127.0.0.1:6379>
    
    ```

    > 小结

    1. list 实际上是一个链表，before Node after，left，right都可以插入值
    2. 如果 key 不存在，那么就创建新的链表，如果 key 存在，那么就添加元素
    3. 如果移除了 list 中所有的值，那么就是一个空链表，也就意味着这个 list 不存在了
    4. 在两边插入或者改动值，效率最高

# 什么是 CAS

# 看  JUC 并发编程