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

- **Redis-key**

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

    

- **String**

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

- **List**

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

- **set**

    **set 中无重复的 value**

    ```shell
    127.0.0.1:6379> sadd set one  # 往 set 中添加值
    (integer) 1
    127.0.0.1:6379> smembers set  # 查询出 set 中的成员
    1) "one"
    127.0.0.1:6379> sismember set one  # one 是否是 set 的成员
    (integer) 1
    
    127.0.0.1:6379> scard set  # 查看 set 中有几个元素
    (integer) 3
    
    127.0.0.1:6379> srem set one  # 移除 set 中指定的元素
    (integer) 1
    
    127.0.0.1:6379> srandmember set  # 随机取 set 中的一个元素
    "two"
    127.0.0.1:6379> srandmember set 2  # 随机取 set 中的丙个元素
    1) "three"
    2) "two"
    127.0.0.1:6379> srandmember set
    "three"
    
    
    127.0.0.1:6379> spop set # 随机删除一个 set 中的元素
    "one"
     
    127.0.0.1:6379> smove source destination member # 将 source 中的 member 移动到 destination 中
    
    127.0.0.1:6379> smembers set
    1) "four"
    2) "one"
    3) "three"
    4) "two"
    127.0.0.1:6379> smembers set1
    1) "3"
    2) "1"
    3) "one"
    4) "2"
    127.0.0.1:6379> sdiff set set1  # set 中剔除 与 set1 中相同的数据
    1) "four"
    2) "three"
    3) "two"
    127.0.0.1:6379> sinter set set1  # 交集 set 和 set1 中共同存在的数据
    1) "one"
    127.0.0.1:6379> sunion set set1  #  并集
    1) "three"
    2) "2"
    3) "two"
    4) "3"
    5) "1"
    6) "four"
    7) "one"
    
    
    ```

    > 用 set 的交并补 可以做共同好友呀，共同关注等应用

- **hash**

    hash底层是  key-value 形式的，本质和 string 没有什么太大区别，命令是以 

    ```shell
    # 通过 hset 往 temp 放入了 一对  yang（key） li(value)，插入单个数据
    127.0.0.1:6379> hset temp yang li
    (integer) 1
    # 通过 hget temp yang(key) 来取出 temp 中 key 为 yang 的值
    127.0.0.1:6379> hget temp yang
    "li"
    # 通过 hmset 插入多个key value
    127.0.0.1:6379> hmset temp one 1 two 2  
    (integer) 2
    # 取出 temp 中所有的键与值
    127.0.0.1:6379> hgetall temp
    1) "one"
    2) "1"
    3) "two"
    4) "2"
    # 取出多个值
    127.0.0.1:6379> hmget temp one two
    1) 1
    2) 2
    # 删除指定的数据
    127.0.0.1:6379> hdel temp one
    (integer) 1
    127.0.0.1:6379> hgetall temp
    1) "two"
    2) "2"
    # hlen 查看 hash 中有多少个键值对
    127.0.0.1:6379> hlen temp
    (integer) 1
    # hexists 查看是否存在某个 key
    127.0.0.1:6379> HEXISTS temp two
    (integer) 1
    # 查看 temp 中所有的 key 值
    127.0.0.1:6379> hkeys temp
    1) "two"
    2) "one"
    # 查看 temp 中所有的 value 值
    127.0.0.1:6379> hvals temp
    1) "2"
    2) "1"
    127.0.0.1:6379> hset temp three 3
    (integer) 1
    # 通过 hincrby 来指定字段的整数值加上增量 increment
    127.0.0.1:6379> hincrby temp three 1
    (integer) 4
    # 可以通过对象的形式存储数据
    127.0.0.1:6379> hset temp user:1:name yang
    (integer) 1
    127.0.0.1:6379> hget temp user:1:name
    "yang"
    ```

- **Zset**  sorted set

在 set 的基础上增加了一个 排序

```shell
# 添加一个元素放在第一个
127.0.0.1:6379> zadd temp 1 one
(integer) 1

# 同时添加两个元素分别指定位置
127.0.0.1:6379> zadd temp 2 two 3 three
(integer) 2

# zrange 取数
127.0.0.1:6379> zrange temp 0 -1
1) "one"
2) "two"
3) "three"

# 通过 zrangebyscore 来排序  后两位可以指定区间
127.0.0.1:6379> zrangebyscore temp -inf +inf 
1) "one"
2) "two"
3) "three"

# 排序出来并带有 scores   从负无穷 到 下无穷
127.0.0.1:6379> zrangebyscore temp -inf +inf withscores
1) "one"
2) "1"
3) "two"
4) "2"
5) "three"
6) "3"

# 从正无穷 到时
127.0.0.1:6379> zrevrangescore temp +inf -inf
1) "three"
2) "two"
3) "one"

# 获取指定区间的值的数量
127.0.0.1:6379> zcount temp 1 3
(integer) 3

# 查看 temp 中有几个值
127.0.0.1:6379> zcard temp
(integer) 3

# 移除指定的值
127.0.0.1:6379> zrem temp one
(integer) 1
127.0.0.1:6379> zrange temp 0 -1
1) "two"
2) "three"




```





# 什么是 CAS

# 看  JUC 并发编程

## 2.6、三种特殊数据类型

- **geospatial** 地理位置

    语法：GEOADD key longitude latitude member [longitude latitude member ...]

    ```shell
    # 添加地理位置
    127.0.0.1:6379> geoadd china:city 116.41 39.91 beiging 
    (integer) 1
    127.0.0.1:6379> geoadd china:city 121.445 31.213 shanghai
    (integer) 1
    
    # 经纬度是有范围要求的  超出范围就会报错
    127.0.0.1:6379> geoadd china:city 104.071 3067 chengdu 106.549 29.581 chongqin
    (error) ERR invalid longitude,latitude pair 104.071000,3067.000000
    
    # 可以同时插入多个地理位置
    127.0.0.1:6379> geoadd china:city 104.071 30.67 chengdu 106.549 29.581 chongqin
    (integer) 2
    
    # 查看数据 用 geopos   获取当前的坐标值
    127.0.0.1:6379> geopos china:city chongqin
    1) 1) "106.54900163412094116"
       2) "29.58100070345364685"
    127.0.0.1:6379> geopos china:city shanghai beijing
    1) 1) "121.44499808549880981"
       2) "31.213001199663303"
    2) 1) "116.40999823808670044"
       2) "39.90999956664450821"
    
    # 查看两地的距离  用 geodist   distance
    127.0.0.1:6379> geodist china:city beijing chongqin
    "1458099.4088"
    # 可以带单位  m米  m千米  mi英里  ft英尺
    127.0.0.1:6379> geodist china:city beijing chongqin km
    "1458.0994"
    
    # 查找 以 110 30 为圆心  1000km 为半径的圆里有几个城市在 china:city 中
    127.0.0.1:6379> georadius china:city 110 30 1000 km
    1) "chongqin"
    2) "chengdu"
    
    # withdist 参数是指两地距离  withcoord 是指 chongqing 的经纬度  
    # count 指定最多查询出多少个维结果
    127.0.0.1:6379> georadius china:city 110 30 500 km withdist withcoord
    1) 1) "chongqin"
       2) "336.3467"
       3) 1) "106.54900163412094116"
          2) "29.58100070345364685"
    
    # georadiusbymember 通过指定元素进行查找
    127.0.0.1:6379> georadiusbymember china:city beijing 1000 km withdist withcoord
    1) 1) "beiging"
       2) "0.0000"
       3) 1) "116.40999823808670044"
          2) "39.90999956664450821"
    2) 1) "beijing"
       2) "0.0000"
       3) 1) "116.40999823808670044"
          2) "39.90999956664450821"
    
    # geohash 通过这个 geohash 将经纬度转换成 hash 值
    127.0.0.1:6379> geohash china:city beijing
    1) "wx4g0crhte0"
    127.0.0.1:6379> geohash china:city shanghai
    1) "wtw3ed1sct0"
    
    127.0.0.1:6379> ZRANGE china:city 0 -1
    1) "chongqin"
    2) "chengdu"
    3) "shanghai"
    4) "beiging"
    5) "beijing"
    
    ```

    > geo 底层用的是 zset  我们可以通过 zrem 来删除

- **Hperloglog**

    > 基数的概念

    占用内存是固定的  2^64个不同的元素 只占12KB 内存

    ![https://pic3.zhimg.com/80/v2-8437d2b2017dd5261412f8100b6ea864_720w.png](https://pic3.zhimg.com/80/v2-8437d2b2017dd5261412f8100b6ea864_720w.png)

    ```shell
    # 通过 pfadd 添加元素
    127.0.0.1:6379> pfadd ykey 1 2 3 4 5
    (integer) 1
    127.0.0.1:6379> pfadd lkey 2 3 4 5 6
    (integer) 1
    
    # 通过 pfcount 来查看元素中的个数
    127.0.0.1:6379> pfcount ykey
    (integer) 5
    127.0.0.1:6379> pfcount lkey
    (integer) 5
    
    # 通过 pfmerge 来合并数据  做并集，但是有一定误差
    127.0.0.1:6379> pfmerge mkey ykey lkey
    OK
    127.0.0.1:6379> pfcount mkey
    (integer) 6
    
    ```

- **Bitmaps**

    > 通过 bit 位来存储数据

    ```shell
    # 通过 setbit 插入数据 值只能是 1 或 0 
    127.0.0.1:6379> setbit day 0 1
    (integer) 0
    127.0.0.1:6379> setbit day 1 0
    (integer) 0
    127.0.0.1:6379> setbit day 2 1
    (integer) 0
    127.0.0.1:6379> setbit day 3 1
    (integer) 0
    
    # 通过 getbit 来获取值
    127.0.0.1:6379> getbit day 2
    (integer) 1
    
    # 通过 bitcount 来获得值为 1 的数据有多少条
    127.0.0.1:6379> bitcount day
    (integer) 3
    
    ```

    ## 2.7、事务

    Redis 事务本质：一组命令的集合，一个事务中的所有命令都会被序列化。在事务执行过程中，会按照顺序执行，一次性、顺序性、排他性！执行一些列的命令

    > Redis 单条命令式是保证原子性的，但是事务是不保证原子性的。Redis事务没有隔离级别的概念

- 开户事务（multi）

- 命令入队（）

- 执行事务（exec）

    >

    ```shell
    # 通过 multi 开启事务
    127.0.0.1:6379> multi
    OK
    127.0.0.1:6379> set k1 v1
    QUEUED
    127.0.0.1:6379> set k2 v2
    QUEUED
    127.0.0.1:6379> get k1
    QUEUED
    127.0.0.1:6379> get k2
    QUEUED
    # 通过 exec 执行事务  队列是先进先出，当执行了 exec ，那么对应的事务就没了
    127.0.0.1:6379> exec
    1) OK
    2) OK
    3) "v1"
    4) "v2"
    
    # 重新开启事务
    127.0.0.1:6379> multi
    OK
    127.0.0.1:6379> set k k
    QUEUED
    127.0.0.1:6379> set kk kk
    QUEUED
    127.0.0.1:6379> set kkk kkk
    QUEUED
    
    # 取消事务  事务队列中的指令都不会执行
    127.0.0.1:6379> discard
    OK
    127.0.0.1:6379> get kkk
    (nil)
    
    ```

    **编译出错**，事务中所有的命令都不会执行

    ```shell
    # 开启事务
    127.0.0.1:6379> multi
    OK
    127.0.0.1:6379> set a a
    QUEUED
    127.0.0.1:6379> set b b
    QUEUED
    
    # 编译异常
    127.0.0.1:6379> setget c c
    (error) ERR unknown command `setget`, with args beginning with: `c`, `c`, 
    
    # 执行事务  那么事务中的所有命令都不会执行
    127.0.0.1:6379> exec
    (error) EXECABORT Transaction discarded because of previous errors.
    127.0.0.1:6379> get b
    (nil)
    
    ```

    **运行出错**，事务中的命令如果有错误，但不会影响到正解的命令执行

    ```shell
    127.0.0.1:6379> multi
    OK
    127.0.0.1:6379> set k1 "k1"
    QUEUED
    # incr 自增值只能是数字
    127.0.0.1:6379> incr k1
    QUEUED
    127.0.0.1:6379> set k2 k2
    QUEUED
    127.0.0.1:6379> set k3 k3
    QUEUED
    127.0.0.1:6379> exec
    1) OK
    2) (error) ERR value is not an integer or out of range
    3) OK
    4) OK
    127.0.0.1:6379> get k1
    "k1"
    127.0.0.1:6379> get k2
    "k2"
    127.0.0.1:6379> get k3
    "k3"
    
    ```

    **悲观锁**：很悲观，认为什么时候都会出问题，无论做什么都会加锁
    
    **乐观锁**：很乐观，认为什么时候都不会出问题，所以不会上锁！更新数据的时候去判断一下，在此期间是不是有人修改过这个数据，可以通过获取 version 来判断，更新的时候比较 version。通过 watch 来监视，通过 unwathc 来取消监视

# Jedis

什么是 jedis ，是 redis 官方推荐的 java 连接开发工具，使用 java 操作 redis 中间件，如果你要使用 java 操作 redis，那么一定要对 jedis 十分熟悉

1. 导入对应的 jedis 依赖 和 fastjson 依赖

    ```pom
    		<!-- https://mvnrepository.com/artifact/redis.clients/jedis -->
            <dependency>
                <groupId>redis.clients</groupId>
                <artifactId>jedis</artifactId>
                <version>3.3.0</version>
            </dependency>
            <!-- https://mvnrepository.com/artifact/com.alibaba/fastjson -->
            <dependency>
                <groupId>com.alibaba</groupId>
                <artifactId>fastjson</artifactId>
                <version>1.2.73</version>
            </dependency>
    ```

    ```shell
    # 连接阿里的 redis
    # 第一步 查看 ip，并将 配置到 redis.config 中 通过 ifconfig 查看
    # 第二步 将 ip bind 到配置文件中
    # 第三步  启动 redis-server
    ```

    ```java
    // project structure 中：配置的 modules 中  sources 和 dependencies 的 SDK 要保持一致
    // project structure 中：modules 和 project 的 SDK 也要保持一致
    // Setting 中 搜索 javac 修改对应的编译版本
    // java 这边测试  输出 PONG 就测试成功
    public class TestPing {
        public static void main(String[] args) {
            // 1、new 一个 jedis 的对象
            Jedis jedis = new Jedis("106.14.7.167", 6379);
            String ping = jedis.ping();
            System.out.println(ping);
        }
    }
    
    ```

    

2. 操作依赖

3. 断开连接

    ```java
    public static void main(String[] args) {
            // 创建一个 jedis 的对象
            Jedis jedis = new Jedis("106.14.7.167", 6379);
            // 清空当前默认 0 号数据库
            jedis.flushDB();
            // 开启事务
            Transaction multi = jedis.multi();
            // 手动模拟 json 数据
            JSONObject jsonObject = new JSONObject();
            jsonObject.put("name","mitsuha");
            jsonObject.put("age",10);
            String str = jsonObject.toJSONString();
            try {
                // 通过事务添加数据
                multi.set("test", str);
                multi.set("test1", str);
                //int i = 1 / 0;
                // 执行事务
                multi.exec();
            } catch(Exception e) {
                // 回滚事务
                multi.discard();
            } finally {
                // 尝试输出添加的数据
                System.out.println(jedis.get("test"));
                System.out.println(jedis.get("test1"));
                // 关闭连接
                jedis.close();
            }
        }
    ```

    