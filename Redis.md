# 1、安装

> CentOS

```shell
wget http://download.redis.io/releases/redis-最新稳定版本.tar.gz

tar xzf redis-最新稳定版本.tar.gz  别名

cd 别名

make
```

**测试**

```shell
# 一个窗口中进行启动服务
cd src

./redis-server

# 开户另一个窗口进行 redis-lci
cd src

./redis-cli
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

