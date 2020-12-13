# Docker

**镜像**：docker镜像就好比一个模板，可以通过这个模板来创建容器服务。

**容器**：Docker利用容器技术，独立运行一个或者一个组应用，通过镜像来创建的

**仓库**：仓库就是存放镜像的地方

# 安装Docker

直接去搜索 `docker document`

**CentOS7安装如下：**

```bash
#1 卸载老版本的 docker
$ yum remove docker

#2 安装yum工具
$ yum install -y yum-utils

#3 设置阿里云的加速镜像
$ yum-config-manager \
    --add-repo \
    https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo

#4 更新yum的软件包索引
centos7 执行 yum makecache fast
centos8 执行 yum makecache

#5 安装docker   docker-ce是社区版本，一般我们都使用社区版本
$ yum install docker-ce docker-ce-cli containerd.io

#6 启动docker
$ systemctl start docker

#7 测试 hello-world
$ docker run hello-world

#8 查看一下下载的hello-world在不在
$ docker images
```

**CentOS8安装如下**

```bash
#1 卸载老版本的 docker
$ yum remove docker

#2 安装yum工具
$ yum install -y yum-utils

#3 设置阿里云的加速镜像
$ yum-config-manager \
    --add-repo \
    https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo

#4 更新yum的软件包索引
centos7 执行 yum makecache fast
centos8 执行 yum makecache

#5 安装docker   docker-ce是社区版本，一般我们都使用社区版本
  ## 下载docker-ce的repo
$ curl https://download.docker.com/linux/centos/docker-ce.repo -o /etc/yum.repos.d/docker-ce.repo
  ## 安装依赖（这是相比CentOS 7的关键步骤）
$ yum install https://download.docker.com/linux/Fedora/30/x86_64/stable/Packages/containerd.io-1.2.6-3.3.fc30.x86_64.rpm
  ## 安装docker-ce
$ yum install docker-ce

#6 启动Docker
$ systemctl start docker

#7 查看docker状态 以及 docker version
$ systemctl status docker
$ docker version

#8 测试hello-world
$ docker run hello-world

#9 查看一下下载的hello-world在不在
$ docker images
```

**卸载docker**

```bash
# 卸载依赖
$ yum remove docker-ce docker-ce-cli containerd.io
# 删除资源;   /var/lib/docker 是docker的默认路径
$ rm -rf /var/lib/docker
```

# 阿里云镜像加速

登录阿里云，找到`产品与服务` ->  `弹性计算` ->`容器镜像服务` -> `镜像加速器` 选择CentOS的操作文档

```bash
# 依次执行下面命令
$ sudo mkdir -p /etc/docker
$ sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://sxegu0up.mirror.aliyuncs.com"]
}
EOF
$ sudo systemctl daemon-reload
$ sudo systemctl restart docker
```

> > 运行docker的实际过程
>
> 执行 docker run hello-world
>
> docker 会去本机寻找镜像
>
> 本机有那就运行这个镜像
>
> 如果没有那就去docker hub 下载
>
> 如果能在 docker hub 找到镜像就下载到本地
>
> 如果没有就返回错误

# 底层原理

**Docker是怎么工作的**

docker 是一个client-server结构的系统，docker的守护进程运行在主机上，通过socker从客户端访问。DockerServer接收到Docker-Client的指令，就会执行这个命令。

**Docker为什么比VM快**

1. Docker有着比虚拟机更少的抽象层
2. Docker利用的是宿主机的内核，VM需要的是GuestOS

# Docker常用的命令

**常用命令**：https://docs.docker.com/engine/reference

```shell
docker version  # 查看版本
docker info     # 显示docker的系统信息，包括镜像和窗口的数量
docker --help   # 帮助命令
```

**镜像命令**

```shell
docker images   查看所有本地主机上的镜像
[root@buzhi image]# docker images
REPOSITORY    TAG       IMAGE ID       CREATED         SIZE
hello-world   latest    bf756fb1ae65   11 months ago   13.3kB

# 解释
REPOSITORY   镜像的仓库源
TAG          镜像的标签
IMAGE ID     镜像的id
CREATED      镜像创建时间
SIZE         镜像大小
-a, --all             Show all images (default hides intermediate images)
      --digests         Show digests
  -f, --filter filter   Filter output based on conditions provided
      --format string   Pretty-print images using a Go template
      --no-trunc        Don't truncate output
  -q, --quiet           Only show image IDs

```

```shell
docker search mysql  搜索镜像
-f, --filter filter   Filter output based on conditions provided
      --format string   Pretty-print search using a Go template
      --limit int       Max number of search results (default 25)
      --no-trunc        Don't truncate output
docker search mysql --filter=STARS=5000  
```

```shell
docker pull  下载镜像 [tag]
[root@buzhi image]# docker pull mysql  
Using default tag: latest   # 如果不写tag,默认就是最新版本的
latest: Pulling from library/mysql
852e50cd189d: Pull complete   # 分层下载是docker image的核心  联合文件系统
29969ddb0ffb: Pull complete 
a43f41a44c48: Pull complete 
5cdd802543a3: Pull complete 
b79b040de953: Pull complete 
938c64119969: Pull complete 
7689ec51a0d9: Pull complete 
a880ba7c411f: Pull complete 
984f656ec6ca: Pull complete 
9f497bce458a: Pull complete 
b9940f97694b: Pull complete 
2f069358dc96: Pull complete 
Digest: sha256:4bb2e81a40e9d0d59bd8e3dc2ba5e1f2197696f6de39a91e90798dd27299b093 # 名
Status: Downloaded newer image for mysql:latest
docker.io/library/mysql:latest  # 真实镜像地址

`docker pull mysql  等价于  docker pull docker.io/library/mysql:latest`

# 使用tag下载指定版本，这个版本号必须存在于hub.docker.com中
docker pull mysql:5.7
```

```shell
docker rmi 删除镜像
[root@buzhi image]# docker images
REPOSITORY    TAG       IMAGE ID       CREATED         SIZE
mysql         latest    dd7265748b5d   2 weeks ago     545MB
hello-world   latest    bf756fb1ae65   11 months ago   13.3kB
[root@buzhi image]# docker rmi -f dd7265748b5d   # 通过 IMAGE ID 删除
Untagged: mysql:latest
Untagged: mysql@sha256:4bb2e81a40e9d0d59bd8e3dc2ba5e1f2197696f6de39a91e90798dd27299b093
Deleted: sha256:dd7265748b5dc3211208fb9aa232cef8d3fefd5d9a2a80d87407b8ea649e571c
Deleted: sha256:aac9a624212bf416c3b41a62212caf12ed3c578d6b17b0f15be13a7dab56628d
Deleted: sha256:1bf3ce09276e9e128108b166121e5d04abd16e7de7473b53b3018c6db0cf23ff
Deleted: sha256:24c6444cea460c3cc2f4e0385e3e97819a0672a54a361921f95d4582583abd59
Deleted: sha256:77585ebe3eaa035694084b3c5937fe82b8972aae1e6c6070fc4d7bc391d10928
Deleted: sha256:1cfd539163ceb17f7bb85a0da968714fe9258b75dbf73f5ad45392a45cfd34b7
Deleted: sha256:c37f414ac8d2b5e5d39f159a6dffd30b279c1268f30186cee5da721e451726ea
Deleted: sha256:955b3c214bccf3ee2a7930768137fd7ed6a72677334be67a07c78a622abd318a
Deleted: sha256:a2e35a0fdb20100365e2fb26c65357fcf926ac7990bf9074a51cbac5a8358d7e
Deleted: sha256:8c3a028fc66f360ce6ce6c206786df68fac4c24257474cbe4f67eda0ac21efd6
Deleted: sha256:0a6d37fabaceb4faa555e729a7d97cb6ee193cb97789a213907a3d3c156d7e35
Deleted: sha256:579519c51de1afe1e29d284b1741af239a307975197cf6ce213a70068d923231
Deleted: sha256:f5600c6330da7bb112776ba067a32a9c20842d6ecc8ee3289f1a713b644092f8

[root@buzhi image]# docker rmi -f 容器id 容器id 容器id  # 连续删除多个镜像
[root@buzhi image]# docker rmi -f $(docker images -aq)  # 删除全部的镜像

```

**容器命令**

> 说明：我们有了镜像才可以创建容器，linux，下载一个centos镜像来测试学习

```shell
docker pull centos
```

## 新建容器并启动

```shell 
docker run [可选参数] image

# 参数说明
--name=""   容器名字
-d          后台方式运行
-it         使用交互方式运行，进入窗口查看内容
-p          指定窗口的端口  -p  8080:8080
	-p 主机端口:容器端口
	-p 容器端口
	容器端口
-p          随机指定端口

[root@buzhi image]# docker images
REPOSITORY    TAG       IMAGE ID       CREATED         SIZE
centos        latest    300e315adb2f   3 days ago      209MB
hello-world   latest    bf756fb1ae65   11 months ago   13.3kB
[root@buzhi image]# docker run -it centos /bin/bash
[root@82e0e8f3b463 /]# exit  就退出了容器
```

## 查看运行中的容器

```shell
docker ps       # 查看正在运行的容器
docker ps -a    # 查看正在运行的容器 + 带出历史运行过的容器
```

> 退出容器

```shell
exit   # 直接停止容器并退出
Ctrl + p + q  # 容器不停止，但退出
```

## 删除容器

```shell
docker rm 容器id   # 不能删除正在运行的容器  如果非要删除正在运行的容器， docker rm -f 容器id
docker rm -f $(docker images -aq)  # 删除所有的容器
docker ps -a -q|xargs docker rm    # 删除所有的容器
```

## 启动和停止容器

```shell
# 容器id 是通过 docker ps -a 查看出来的
docker start 容器id     # 启动容器
docker restart 容器id   # 重启容器
docker stop 容器id      # 停止当前正在运行的容器
docker kill 容器id      # 强制停止当前容器

[root@buzhi image]# docker ps -a
CONTAINER ID   IMAGE         COMMAND       CREATED              STATUS                          PORTS     NAMES
9f90ec401090   centos        "/bin/bash"   41 seconds ago       Up 39 seconds                             determined_margulis
124b0457af3e   centos        "/bin/bash"   About a minute ago   Exited (0) About a minute ago             dreamy_lalande
82e0e8f3b463   centos        "/bin/bash"   11 minutes ago       Exited (0) 7 minutes ago                  beautiful_agnesi
39e8ac4f1bf3   hello-world   "/hello"      2 hours ago          Exited (0) 2 hours ago                    lucid_grothendieck
[root@buzhi image]# docker stop 9f90ec401090
9f90ec401090

```

## 后台启动容器

```shell
docker run -d centos
# 问题: docker ps,发现 centos停止了

# 常见的坑：docker容器使用后台运行，就必须要有一个前台进程，docker发现没有应用，就会自动停止
# nginx 容器启动后，发现自己没有提供服务，应付立刻停止，就是没有程序了
```

## 查看日志

```shell
docker logs -f -t --tail  容器

[root@buzhi image]# docker run -d centos /bin/sh -c "while true;do echo hello;sleep 1;done
"
ff41f58fa470757ab7c636a153be4d2ac5e96a572996fe2f7744156926a6dbfa
[root@buzhi image]# docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS     NAMES
ff41f58fa470   centos    "/bin/sh -c 'while t…"   5 seconds ago   Up 4 seconds             festive_lumiere
[root@buzhi image]# docker logs -tf --tail 10 ff41f58fa470
2020-12-11T16:48:18.330501335Z hello
2020-12-11T16:48:19.332312278Z hello
2020-12-11T16:48:20.333995905Z hello
2020-12-11T16:48:21.335741271Z hello
2020-12-11T16:48:22.337557978Z hello
2020-12-11T16:48:23.339253333Z hello
2020-12-11T16:48:24.340955942Z hello
2020-12-11T16:48:25.342651438Z hello
2020-12-11T16:48:26.344387389Z hello
2020-12-11T16:48:27.346250187Z hello
2020-12-11T16:48:28.347958353Z hello
2020-12-11T16:48:29.349874338Z hello
2020-12-11T16:48:30.351510750Z hello

# 显示日志
-tf   # 显示日志
--tail number # 要显示日志的条数

```

## 查看容器中进程信息

```shell
# 命令 docker top 容器id

[root@buzhi image]# docker top ff41f58fa470
UID   PID   PPID  C STIME TTY TIME     CMD
root  34107 34087 0 00:47  ?  00:00:00 /bin/sh -c while true;do echo hello;sleep 1;done
root  34455 34107 0 00:51  ?  00:00:00 /usr/bin/coreutils --coreutils-prog-shebang=sleep /usr/bin/sleep 1

```

## 查看镜像的元数据

```shell
[root@buzhi image]# docker inspect ff41f58fa470
[
    {
        "Id": "ff41f58fa470757ab7c636a153be4d2ac5e96a572996fe2f7744156926a6dbfa",
        "Created": "2020-12-11T16:47:50.811812354Z",
        "Path": "/bin/sh",
        "Args": [
            "-c",
            "while true;do echo hello;sleep 1;done"
        ],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 34107,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2020-12-11T16:47:51.284680197Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:300e315adb2f96afe5f0b2780b87f28ae95231fe3bdd1e16b9ba606307728f55",
        "ResolvConfPath": "/var/lib/docker/containers/ff41f58fa470757ab7c636a153be4d2ac5e96a572996fe2f7744156926a6dbfa/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/ff41f58fa470757ab7c636a153be4d2ac5e96a572996fe2f7744156926a6dbfa/hostname",
        "HostsPath": "/var/lib/docker/containers/ff41f58fa470757ab7c636a153be4d2ac5e96a572996fe2f7744156926a6dbfa/hosts",
        "LogPath": "/var/lib/docker/containers/ff41f58fa470757ab7c636a153be4d2ac5e96a572996fe2f7744156926a6dbfa/ff41f58fa470757ab7c636a153be4d2ac5e96a572996fe2f7744156926a6dbfa-json.log",
        "Name": "/festive_lumiere",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": null,
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "default",
            "PortBindings": {},
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "CapAdd": null,
            "CapDrop": null,
            "CgroupnsMode": "host",
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "private",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 67108864,
            "Runtime": "runc",
            "ConsoleSize": [
                0,
                0
            ],
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": null,
            "BlkioDeviceWriteBps": null,
            "BlkioDeviceReadIOps": null,
            "BlkioDeviceWriteIOps": null,
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DeviceRequests": null,
            "KernelMemory": 0,
            "KernelMemoryTCP": 0,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": false,
            "PidsLimit": null,
            "Ulimits": null,
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0,
            "MaskedPaths": [
                "/proc/asound",
                "/proc/acpi",
                "/proc/kcore",
                "/proc/keys",
                "/proc/latency_stats",
                "/proc/timer_list",
                "/proc/timer_stats",
                "/proc/sched_debug",
                "/proc/scsi",
                "/sys/firmware"
            ],
            "ReadonlyPaths": [
                "/proc/bus",
                "/proc/fs",
                "/proc/irq",
                "/proc/sys",
                "/proc/sysrq-trigger"
            ]
        },
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/1b879c0353f441d8a1ef4ee0f55887897d3093e2b5b558f77ed986fd67e998bd-init/diff:/var/lib/docker/overlay2/303d82aa9e19b2f6c154c1cab01c3d37b4b07ff69e76990fd36530f7b417be64/diff",
                "MergedDir": "/var/lib/docker/overlay2/1b879c0353f441d8a1ef4ee0f55887897d3093e2b5b558f77ed986fd67e998bd/merged",
                "UpperDir": "/var/lib/docker/overlay2/1b879c0353f441d8a1ef4ee0f55887897d3093e2b5b558f77ed986fd67e998bd/diff",
                "WorkDir": "/var/lib/docker/overlay2/1b879c0353f441d8a1ef4ee0f55887897d3093e2b5b558f77ed986fd67e998bd/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [],
        "Config": {
            "Hostname": "ff41f58fa470",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "/bin/sh",
                "-c",
                "while true;do echo hello;sleep 1;done"
            ],
            "Image": "centos",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {
                "org.label-schema.build-date": "20201204",
                "org.label-schema.license": "GPLv2",
                "org.label-schema.name": "CentOS Base Image",
                "org.label-schema.schema-version": "1.0",
                "org.label-schema.vendor": "CentOS"
            }
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "3080325c5074a32def2414e47aef3db98a4b390794e3eefebb4fd7bbc48a7291",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {},
            "SandboxKey": "/var/run/docker/netns/3080325c5074",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "1224f81e3cf72bf3b3fd72cdeae7464bed00ed4775b2dec274d6f9de1844caf7",
            "Gateway": "172.17.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.2",
            "IPPrefixLen": 16,
            "IPv6Gateway": "",
            "MacAddress": "02:42:ac:11:00:02",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "2c8c6be5110e28aebc2e15537b6b71e6c3c85ac8892652e1b822e8c816bf951c",
                    "EndpointID": "1224f81e3cf72bf3b3fd72cdeae7464bed00ed4775b2dec274d6f9de1844caf7",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:02",
                    "DriverOpts": null
                }
            }
        }
    }
]
```

## 进入当前正在运行的容器

```shell
# 我们容器通常都是后台方式运行的，需要进入容器，修改一些配置文件
# 第一种方式
docker exec -it 容器id bashshell

[root@buzhi image]# docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED        STATUS        PORTS     NAMES
ff41f58fa470   centos    "/bin/sh -c 'while t…"   20 hours ago   Up 20 hours             festive_lumiere
[root@buzhi image]# docker exec -it ff41f58fa470 /bin/bash
[root@ff41f58fa470 /]# ls
bin  etc   lib	  lost+found  mnt  proc  run   srv  tmp  var
dev  home  lib64  media       opt  root  sbin  sys  usr
[root@ff41f58fa470 /]# 


# 第二种方式
docker attach 容器id
正在执行当前的代码

[root@buzhi image]# docker attach ff41f58fa470 


# docker exec
# docker attach
```

##  从容器中拷贝文件到主机

```shell
docker cp 容器id:容器中的路径   主机中的路径


[root@buzhi image]# docker exec -it ff41f58fa470 /bin/bash
[root@ff41f58fa470 /]# ls
bin  etc   lib	  lost+found  mnt  proc  run   srv  tmp  var
dev  home  lib64  media       opt  root  sbin  sys  usr
[root@ff41f58fa470 /]# cd              
[root@ff41f58fa470 ~]# ls
anaconda-ks.cfg  anaconda-post.log  original-ks.cfg
# 创建一个文件
[root@ff41f58fa470 ~]# touch test.java
[root@ff41f58fa470 ~]# pwd
/root
[root@ff41f58fa470 ~]# ls
anaconda-ks.cfg  anaconda-post.log  original-ks.cfg  test.java
[root@ff41f58fa470 ~]# exit
exit
# 从容器中拷贝文件到主机中
[root@buzhi image]# docker cp ff41f58fa470:/root/test.java /tmp/test.java
[root@buzhi image]# cd /tmp/
[root@buzhi tmp]# ls
kdevtmpfsi  systemd-private-43db6f49265949f1ad4b987be3ec8f49-chronyd.service-E5rP1v  test
redis2      systemd-private-43db6f49265949f1ad4b987be3ec8f49-mysqld.service-svd1UA   test.java
```

# 练习

```shell
# 查找 nginx 镜像
[root@buzhi tmp]# docker search nginx

# 拉取nginx
[root@buzhi tmp]# docker pull nginx

# 查看刚刚下载的 nginx 镜像
[root@buzhi tmp]# docker images
 
# 运行nginx  -d 是后台启动   3344是主机的端口  映射到nginx中的80端口
[root@buzhi tmp]# docker run -d --name nginx01 -p 3344:80 nginx

# 使用交互方式运行，进入窗口查看内容
[root@buzhi tmp]# docker exec -it nginx01 /bin/bash


# 安装 tomcat  原装的tomcat webapp里面没有东西  cp web
[root@buzhi tmp]# docker pull tomcat

# 后台运行 tomcat
[root@buzhi tmp]# docker run -d -p 3355:8080 --name tomcat01 tomcat

# 进入交互界面
[root@buzhi tmp]# docker exec -it tomcat01 /bin/bash



[root@32e328cae127:/usr/local/tomcat# ls
BUILDING.txt  CONTRIBUTING.md  LICENSE	NOTICE	README.md  RELEASE-NOTES  RUNNING.txt  bin  conf  lib  logs  native-jni-lib  temp  webapps  webapps.dist  work
[root@32e328cae127:/usr/local/tomcat# cp -r webapps.dist/*  webapps


```

# 可视化

- portainer(先用这个)

  ```shell
  docker run -d -p 8088:9000 --restart=always -v /var/run/docker.sock:/var/rum/docker.sock --privileged=true portainer/portainer
  ```

# commit 镜像

```shell
docker commit 披着羊皮的狼容器成为一个新的副本
# 命令和git原理类似
docker commit -a="作者" -m "提交时的描述" 容器id 目标镜像名:[TAG版本号]


[root@buzhi tmp]# docker commit -a="buzhi" -m "添加了页面" 32e328cae127 tomcat-buzhi:1.0
sha256:ae205f2f5a02429aa9e921b77b10d79f8103cf90d1745b7251367e87e130b07a
[root@buzhi tmp]# docker images
REPOSITORY            TAG       IMAGE ID       CREATED         SIZE
tomcat-buzhi          1.0       ae205f2f5a02   4 seconds ago   659MB
nginx                 latest    7baf28ea91eb   47 hours ago    133MB
tomcat                latest    57c97f91f49a   2 days ago      654MB
centos                latest    300e315adb2f   5 days ago      209MB
portainer/portainer   latest    62771b0b9b09   4 months ago    79.1MB
hello-world           latest    bf756fb1ae65   11 months ago   13.3kB

```

# 容器数据卷

什么是容器数据卷

**docker的理念**

将应用和环境打包成一个镜像

将容器内的目录，挂载到Linux上

## 使用数据卷

### 方式一：直接使用命令来挂载 -v

```shell
docker run -it -v /home/test:/home centos /bin/bash

# 将容器中的/home目录映射到主机/home/test目录
[root@buzhi tmp]# docker run -it -v /home/test:/home centos /bin/bash
[root@90e49ab53eb3 /]# ls 
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
[root@90e49ab53eb3 /]# cd /home
[root@90e49ab53eb3 home]# ls
[root@90e49ab53eb3 home]# touch test.java
[root@90e49ab53eb3 home]# ls
test.java

#通过 docker inspect 容器id 来查看 Mounts


        "Mounts": [
            {
                "Type": "bind",
                "Source": "/home/test",
                "Destination": "/home",
                "Mode": "",
                "RW": true,
                "Propagation": "rprivate"
            }
        ],

```

>  安装MySql

```shell
#
-d 后台运行
-p 端口映射
-v 卷挂载
-e 环境配置
-name 容器名字
[root@buzhi tmp]# docker run -d -p 3310:3306 -v /home/mysql/conf:/etc/mysql/conf.d -v /home/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 --name mysql01 mysql:5.7
```

> 具名和匿名挂载

``` shell
# 如何确定是具名挂载还是匿名挂载，还是指定中午挂载
-v 容器内路径      # 匿名挂载
-v 卷名:容器内路径  # 具名挂载
-v /宿主机路径:容器内路径  # 指定路径挂载

拓展：
# 通过 -v 容器内路径:ro rw 改变读写权限
ro  read only
rw  read write 
# 一旦设置了这个容器权限，容器对我们挂载出来的内容就有限定了
docker run -d -p --name name01 3310:3306 -v juming:/etc/mysql/conf.d:ro mysql
docker run -d -p --name name01 3310:3306 -v juming:/etc/mysql/conf.d:rw mysql  # 默认权限

#限定了ro，那么只有通过宿主机来操作，容器内部是无法进行操作的
```

### 方式二：使用Dockerfile来构建docker镜像的构建文件，命令脚本

```shell
# 先创建一个目录来存放脚本，通过这个脚本来构建，
[root@buzhi ~]# cd /home
[root@buzhi home]# mkdir docker-test-volume
[root@buzhi home]# cd docker-test-volume/
[root@buzhi docker-test-volume]# ls
[root@buzhi docker-test-volume]# vim dockerfile
FROM centos
VOLUME ["volume01", "volume02"]
CMD echo "---end---"
CMD /bin/bash
[root@buzhi docker-test-volume]# ls
dockerfile

[root@buzhi home]# mkdir docker-test-volume
[root@buzhi home]# vim dockerfile01
[root@buzhi home]# cat dockerfile01
FROM centos
VOLUME ["volume01", "volume02"]
CMD echo "---end---"
CMD /bin/bash
# 构建 一个自带 volume01 和 volume02 的 centos
[root@buzhi home]# docker build -f dockerfile01 -t buzhi/centos .
Sending build context to Docker daemon  219.8MB
Error response from daemon: dockerfile parse error line 1: unknown instruction: FORM
[root@buzhi home]# vim dockerfile01 
[root@buzhi home]# docker build -f dockerfile01 -t buzhi/centos .
Sending build context to Docker daemon  219.8MB
Step 1/4 : FROM centos
 ---> 300e315adb2f
Step 2/4 : VOLUME ["volume01", "volume02"]
 ---> Running in a80816ecc30a
Removing intermediate container a80816ecc30a
 ---> 0424145a4de6
Step 3/4 : CMD echo "---end---"
 ---> Running in 4b738f542d5c
Removing intermediate container 4b738f542d5c
 ---> cac1136e0ecd
Step 4/4 : CMD /bin/bash
 ---> Running in e998984db211
Removing intermediate container e998984db211
 ---> 3c73556f79b4
Successfully built 3c73556f79b4
Successfully tagged buzhi/centos:latest
[root@buzhi home]# docker images
REPOSITORY            TAG       IMAGE ID       CREATED              SIZE
buzhi/centos          latest    3c73556f79b4   About a minute ago   209MB
# 启动我们刚刚打包的镜像
[root@buzhi ~]# docker run -it 3c73556f79b4 /bin/bash
[root@15f293ccafce /]# ls
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var  volume01	volume02
# 通过 docker inspect 容器id查看挂载情况
[root@buzhi ~]# docker ps
CONTAINER ID    IMAGE      COMMAND       CREATED          STATUS           PORTS   NAMES
15f293ccafce  3c73556f79b4  "/bin/bash"   48 seconds ago  Up 47 seconds            vibrant_clarke
[root@buzhi ~]# docker inspect 15f293ccafce
"Mounts": [
            {
                "Type": "volume",
                "Name": "d92ea5fac76bc2cfcd3df4c619c96efa419ea44937efd5ec2392233fac00baaf",
                "Source": "/var/lib/docker/volumes/d92ea5fac76bc2cfcd3df4c619c96efa419ea44937efd5ec2392233fac00baaf/_data",
                "Destination": "volume01",
                "Driver": "local",
                "Mode": "",
                "RW": true,
                "Propagation": ""
            },
            {
                "Type": "volume",
                "Name": "133cb5117b2567649afeb284c53cac6d01237817b95df4c0e826c7f3bd561057",
                "Source": "/var/lib/docker/volumes/133cb5117b2567649afeb284c53cac6d01237817b95df4c0e826c7f3bd561057/_data",
                "Destination": "volume02",
                "Driver": "local",
                "Mode": "",
                "RW": true,
                "Propagation": ""
            }
        ],
```

##  数据卷容器，实现两个容器中的数据共享

```shell
# 运行我们刚刚自己构建的 centos
[root@buzhi ~]# docker images
REPOSITORY            TAG       IMAGE ID       CREATED          SIZE
buzhi/centos          latest    3c73556f79b4   15 minutes ago   209MB
[root@buzhi ~]# docker run -it --name my-centos01 buzhi/centos
# 自带了  volume01	volume02
[root@92af3eafed20 /]# ls
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var  volume01	volume02
# ctrl + Q + P 退出界面
[root@buzhi ~]# docker ps
CONTAINER ID IMAGE        COMMAND                CREATED        STATUS        PORTS   NAMES
92af3eafed20 buzhi/centos "/bin/sh -c /bin/bash" 44 seconds ago Up 43 seconds         my-centos01
# 再启动一个 自己构建的 centos  ,并实现my-centos02 与my-centos01两个容器中的数据共享
[root@buzhi ~]# docker run -it --name my-centos02 --volumes-from my-centos01 buzhi/centos
[root@1710f6252113 /]# ls
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var  volume01	volume02
```

#  DockerFile

dockerfile是用来构建docker镜像的文件

构建步骤

1. 编写一个目标dockerfile文件
2. docker build 构建成为一个目标镜像
3. docker run 运行镜像
4. docker push 发布镜像到（DockerHub、阿里云镜像仓库）

## DockerFile 构建过程

**基础知识：**

1. 每个保留关键字（指令）都必须大写
2. 执行从上往下顺序执行
3. #表示注释

dockerfile是面向开发的，我们以后要发布项目，做镜像，就需要自己编写dockerfile，

docker镜像逐渐的成为企业交付的标准

dockerfile：构建文件，定义了一切的步骤，源代码

dockerImages：通过DockerFile构建生成的镜像，最终发布和运行的产品

docker容器：容器就是镜像运行起来提供服务

![DockerFile命令](https://blog.52itstyle.vip/usr/uploads/2018/05/501875919.jpg)

```shell
FROM
MAINTAINER   # 镜像是谁写的    姓名+邮箱
RUN          # 镜像构建的时候需要运行的命令
ADD	         # 
WORKDIR      # 镜像的工作目录
VOLUME       # 挂载的目录
EXPOSE       # 保留端口配置
CMD	         # 指定这个容器启动的时候要运行的命令，只有最后一个会生效，可被替代。运行镜像的时候不追加参数
ENTRYPOINT   # 指定这个容器启动的时候要运行的命令，可以追加命令。运行镜像的时候可以追加参数
			 # 例如： docker run -it my-centos-build-by-myself:1.0 -l
ONBUILD      # 当构建一个被继承DockerFile，这个时候就会运行 ONBUILD的指令，触发指令
COPY         # 类似ADD,将我们文件拷贝到镜像中
ENV          # 构建的时候设置环境变量
```

## 创建一个自己的 CentOS

**1、创建自己的dockerfile**

```shell
# 在/home下创建一个dockerfile目录
[root@buzhi my-dockerfile]# pwd
/home/my-dockerfile
# 编辑自己的 DockerFile
[root@buzhi my-dockerfile]# vim my-centos-build-by-myself-dockerfile
[root@buzhi my-dockerfile]# cat my-centos-build-by-myself-dockerfile 
# 选择母体
FROM centos

# 添加个人信息
MAINTAINER MitsuhaCon<mitsuhacon@gmail.com>

# 配置环境变量
ENV MYPATH /usr/local

# 配置工作目录
WORKDIR $MYPATH

# 运行命令下载一些指令
RUN yum -y install vim
RUN yum -y install net-tools

# 暴露端口
EXPOSE 80

# 尝试输出 MYPATH 的值
CMD echo $MYPATH
CMD echo "---end---"
CMD /bin/bash
```

**2、通过刚刚的dockerfile构建一个自己的镜像**

> docker build 的时候，
>
> -f  指定dockerfile，这个可以用绝对路径，也可以用相对路径
>
> -t  指定构建出来的镜像的名字
>
> 语句中最后有一个  **.**，表示的是当前目录，少了这个点，就会执行失败

```shell
# 构建镜像
[root@buzhi my-dockerfile]# docker build -f /home/my-dockerfile/my-centos-build-by-myself-dockerfile -t my-centos-build-by-myself:1.0 .
```

**3、运行刚刚构建的镜像**

```shell
# 启动镜像的时候可以加启动参数，例如 dockerfile里面配置了 CMD ["ls","-a"]
# 那么运行 docker run -it my-centos-build-by-myself:1.0 的时候会自动运行 ls -a命令，前提是`CMD ["ls","-a"]`命令在  # dockerfile中是最后一个 CMD，运行
# docker run -it my-centos-build-by-myself:1.0 -l，是会报错的，因为CMD不能追加命令
# 如果没有配置 ls -a，那么可以执行
# docker run -it my-centos-build-by-myself:1.0 ls -a
# 而 ENTRYPOINT是可以追加命令的
[root@buzhi my-dockerfile]# docker run -it my-centos-build-by-myself:1.0

# 查看镜像的历史
[root@buzhi my-dockerfile]# docker history 09f39e03ccf4
IMAGE          CREATED          CREATED BY                                      SIZE      COMMENT
09f39e03ccf4   12 minutes ago   /bin/sh -c #(nop)  CMD ["/bin/sh" "-c" "/bin…   0B        
bbc111ca3946   12 minutes ago   /bin/sh -c #(nop)  CMD ["/bin/sh" "-c" "echo…   0B        
4d952df30d1d   12 minutes ago   /bin/sh -c #(nop)  CMD ["/bin/sh" "-c" "echo…   0B        
3610751259f7   12 minutes ago   /bin/sh -c #(nop)  EXPOSE 80                    0B        
1f43b1803fe3   12 minutes ago   /bin/sh -c yum -y install net-tools             23.2MB    
bf56a5201b1d   12 minutes ago   /bin/sh -c yum -y install vim                   57.7MB    
ae1d3ee909d6   12 minutes ago   /bin/sh -c #(nop)  ENV MYPATH=/usr/local        0B        
f761ed1ad0fe   12 minutes ago   /bin/sh -c #(nop)  MAINTAINER MitsuhaCon<mit…   0B        
300e315adb2f   5 days ago       /bin/sh -c #(nop)  CMD ["/bin/bash"]            0B        
<missing>      5 days ago       /bin/sh -c #(nop)  LABEL org.label-schema.sc…   0B        
<missing>      5 days ago       /bin/sh -c #(nop) ADD file:bd7a2aed6ede423b7…   209MB 
```

**可以通过 `docker history` 来查看人家的镜像是怎么构建**

## 实战：构建一个自己的TomCat(待完成)







# 发布自己的镜像到 DockerHub





# 发布自己的镜像到阿里云镜像

1. 登录阿里云
2. 找到容器镜像服务
3. 创建命名空间
4. 到镜像仓库镜像仓库
5. 根据`操作指南`进行上传

# 小结

![docker流程图](https://ss0.baidu.com/6ON1bjeh1BF3odCf/it/u=3983552541,977482640&fm=15&gp=0.jpg)

# Docker网络

## --link(不推荐使用)

```shell
docker network --help

```



## 自定义网络

```shell
[root@buzhi my-dockerfile]# docker network --help

Usage:  docker network COMMAND

Manage networks

Commands:
  connect     Connect a container to a network
  create      Create a network
  disconnect  Disconnect a container from a network
  inspect     Display detailed information on one or more networks
  ls          List networks
  prune       Remove all unused networks
  rm          Remove one or more networks

[root@buzhi my-dockerfile]# docker network create --help

Usage:  docker network create [OPTIONS] NETWORK

Create a network

Options:
      --attachable           Enable manual container attachment
      --aux-address map      Auxiliary IPv4 or IPv6 addresses used by Network driver (default map[])
      --config-from string   The network from which to copy the configuration
      --config-only          Create a configuration only network
  -d, --driver string        Driver to manage the Network (default "bridge")
      --gateway strings      IPv4 or IPv6 Gateway for the master subnet
      --ingress              Create swarm routing-mesh network
      --internal             Restrict external access to the network
      --ip-range strings     Allocate container ip from a sub-range
      --ipam-driver string   IP Address Management Driver (default "default")
      --ipam-opt map         Set IPAM driver specific options (default map[])
      --ipv6                 Enable IPv6 networking
      --label list           Set metadata on a network
  -o, --opt map              Set driver specific options (default map[])
      --scope string         Control the network's scope
      --subnet strings       Subnet in CIDR format that represents a network segment
# 创建自己的一个网络
[root@buzhi my-dockerfile]# docker network create --driver bridge --subnet 192.168.0.0/16 --gateway 192.168.0.1 mynet
3b4c3580721e44aafca676a121408442c6af2f7d25346012c75c8c6f26bc9380

# 查看自己网络信息
[root@buzhi my-dockerfile]# docker network inspect mynet
[
    {
        "Name": "mynet",
        "Id": "3b4c3580721e44aafca676a121408442c6af2f7d25346012c75c8c6f26bc9380",
        "Created": "2020-12-13T18:30:05.545457818+08:00",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "192.168.0.0/16",
                    "Gateway": "192.168.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {},
        "Options": {},
        "Labels": {}
    }
]

[root@buzhi my-dockerfile]# docker run -d -P tomcat-net-01 --net mynet tomcat

```

> 不同网段，把网络打通

```shell
docker network connect mynet 容器名
```



# Docker Compose

> Docker Compose 是Docker官方的开源项目，需要安装!

`Dockerfile` 让程序在任何地方运行。web服务，redis、mysql、nginx...多个容器



`docker-compose.yml 如下`

```shell
# version: "3.9"
services:  这个相当于容器、应用
  web:
    build: .
    ports:
      - "5000:5000"
    volumes:
      - .:/code
      - logvolume01:/var/log
    links:
      - redis
  redis:
    image: redis
volumes:
  logvolume01: {}
```

## Docker Compose下载

```shell
# 下载docker compose
[root@buzhi my-dockerfile]# sudo curl -L "https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   651  100   651    0     0    415      0  0:00:01  0:00:01 --:--:--   414
100 11.6M  100 11.6M    0     0   146k      0  0:01:21  0:01:21 --:--:--  320k
[root@buzhi my-dockerfile]# cd /usr/local/bin
[root@buzhi bin]# ls
100         cloud-id        `docker-compose`  dump90.rdb    easy_install-3.6  jsondiff     jsonschema       redis-check-aof  redis-conf-prot-2222.conf  sentinel.conf  yang80.conf
backup.db   cloud-init      dump100.rdb     dump.rdb      easy_install-3.8  jsonpatch    pnscan           redis-check-rdb  redis-sentinel             yang100.conf   yang90
chardetect  cloud-init-per  dump80.rdb      easy_install  ipsort            jsonpointer  redis-benchmark  redis-cli        redis-server               yang80         yang90.conf

# 修改权限
sudo chmod +x /usr/local/bin/docker-compose

# 查看docker-compose的版本
[root@buzhi bin]# docker-compose version
docker-compose version 1.27.4, build 40524192
docker-py version: 4.3.1
CPython version: 3.7.7
OpenSSL version: OpenSSL 1.1.0l  10 Sep 2019
```

## 跑一个getting started

```shell
# 在 /home目录下创建 composetest
[root@buzhi home]# mkdir composetest
[root@buzhi home]# cd composetest
[root@buzhi composetest]# vim app.py
[root@buzhi composetest]# cat app.py
import time

import redis
from flask import Flask

app = Flask(__name__)
cache = redis.Redis(host='redis', port=6379)

def get_hit_count():
    retries = 5
    while True:
        try:
            return cache.incr('hits')
        except redis.exceptions.ConnectionError as exc:
            if retries == 0:
                raise exc
            retries -= 1
            time.sleep(0.5)

@app.route('/')
def hello():
    count = get_hit_count()
    return 'Hello World! I have been seen {} times.\n'.format(count)
[root@buzhi composetest]# vim Dockerfile
[root@buzhi composetest]# cat Dockerfile
FROM python:3.7-alpine
WORKDIR /code
ENV FLASK_APP=app.py
ENV FLASK_RUN_HOST=0.0.0.0
RUN apk add --no-cache gcc musl-dev linux-headers
COPY requirements.txt requirements.txt
RUN pip install -r requirements.txt
EXPOSE 5000
COPY . .
CMD ["flask", "run"]
[root@buzhi composetest]# vim requirements.txt
[root@buzhi composetest]# cat requirements.txt
flask
redis
[root@buzhi composetest]# vim docker-compose.yml
[root@buzhi composetest]# cat docker-compose.yml
version: "3.9"
services:
  web:
    build: .
    ports:
      - "5000:5000"
  redis:
    image: "redis:alpine"
[root@buzhi composetest]# docker-compose up
Building web
Step 1/10 : FROM python:3.7-alpine
 ---> 0ce5215b0b31
Step 2/10 : WORKDIR /code
 ---> Using cache
 ---> 437308d7f437
Step 3/10 : ENV FLASK_APP=app.py
 ---> Using cache
 ---> f13daa69b8c9
Step 4/10 : ENV FLASK_RUN_HOST=0.0.0.0
 ---> Using cache
 ---> eddbd362179b
Step 5/10 : RUN apk add --no-cache gcc musl-dev linux-headers
 ---> Using cache
 ---> 11538e027331
Step 6/10 : COPY requirements.txt requirements.txt
 ---> e80890eed3da
Step 7/10 : RUN pip install -r requirements.txt
 ---> Running in 6ff1ef3bbe5e
Collecting flask
  Downloading Flask-1.1.2-py2.py3-none-any.whl (94 kB)
Collecting redis
  Downloading redis-3.5.3-py2.py3-none-any.whl (72 kB)
Collecting click>=5.1
  Downloading click-7.1.2-py2.py3-none-any.whl (82 kB)
Collecting itsdangerous>=0.24
  Downloading itsdangerous-1.1.0-py2.py3-none-any.whl (16 kB)
Collecting Jinja2>=2.10.1
  Downloading Jinja2-2.11.2-py2.py3-none-any.whl (125 kB)
Collecting MarkupSafe>=0.23
  Downloading MarkupSafe-1.1.1.tar.gz (19 kB)
Collecting Werkzeug>=0.15
  Downloading Werkzeug-1.0.1-py2.py3-none-any.whl (298 kB)
Building wheels for collected packages: MarkupSafe
  Building wheel for MarkupSafe (setup.py): started
  Building wheel for MarkupSafe (setup.py): finished with status 'done'
  Created wheel for MarkupSafe: filename=MarkupSafe-1.1.1-cp37-cp37m-linux_x86_64.whl size=16912 sha256=bb05d3c09302ed3c17cdce31413bbf2aa4f54425e47e186de7e1d8f41341480a
  Stored in directory: /root/.cache/pip/wheels/b9/d9/ae/63bf9056b0a22b13ade9f6b9e08187c1bb71c47ef21a8c9924
Successfully built MarkupSafe
Installing collected packages: MarkupSafe, Werkzeug, Jinja2, itsdangerous, click, redis, flask
Successfully installed Jinja2-2.11.2 MarkupSafe-1.1.1 Werkzeug-1.0.1 click-7.1.2 flask-1.1.2 itsdangerous-1.1.0 redis-3.5.3
Removing intermediate container 6ff1ef3bbe5e
 ---> 3bd132ff1431
Step 8/10 : EXPOSE 5000
 ---> Running in d7dbdfc8cce5
Removing intermediate container d7dbdfc8cce5
 ---> 06efa0abd7fa
Step 9/10 : COPY . .
 ---> 527bcd490401
Step 10/10 : CMD ["flask", "run"]
 ---> Running in 605d6e04e92a
Removing intermediate container 605d6e04e92a
 ---> d4c98df79540

Successfully built d4c98df79540
Successfully tagged composetest_web:latest
WARNING: Image for service web was built because it did not already exist. To rebuild this image you must use `docker-compose build` or `docker-compose up --build`.
Creating composetest_redis_1 ... done
Creating composetest_web_1   ... done
Attaching to composetest_redis_1, composetest_web_1
redis_1  | 1:C 13 Dec 2020 14:02:55.554 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
redis_1  | 1:C 13 Dec 2020 14:02:55.554 # Redis version=6.0.9, bits=64, commit=00000000, modified=0, pid=1, just started
redis_1  | 1:C 13 Dec 2020 14:02:55.554 # Warning: no config file specified, using the default config. In order to specify a config file use redis-server /path/to/redis.conf
redis_1  | 1:M 13 Dec 2020 14:02:55.555 * Running mode=standalone, port=6379.
redis_1  | 1:M 13 Dec 2020 14:02:55.555 # WARNING: The TCP backlog setting of 511 cannot be enforced because /proc/sys/net/core/somaxconn is set to the lower value of 128.
redis_1  | 1:M 13 Dec 2020 14:02:55.555 # Server initialized
redis_1  | 1:M 13 Dec 2020 14:02:55.555 # WARNING overcommit_memory is set to 0! Background save may fail under low memory condition. To fix this issue add 'vm.overcommit_memory = 1' to /etc/sysctl.conf and then reboot or run the command 'sysctl vm.overcommit_memory=1' for this to take effect.
redis_1  | 1:M 13 Dec 2020 14:02:55.555 # WARNING you have Transparent Huge Pages (THP) support enabled in your kernel. This will create latency and memory usage issues with Redis. To fix this issue run the command 'echo madvise > /sys/kernel/mm/transparent_hugepage/enabled' as root, and add it to your /etc/rc.local in order to retain the setting after a reboot. Redis must be restarted after THP is disabled (set to 'madvise' or 'never').
redis_1  | 1:M 13 Dec 2020 14:02:55.556 * Ready to accept connections
web_1    |  * Serving Flask app "app.py"
web_1    |  * Environment: production
web_1    |    WARNING: This is a development server. Do not use it in a production deployment.
web_1    |    Use a production WSGI server instead.
web_1    |  * Debug mode: off
web_1    | Traceback (most recent call last):
web_1    |   File "/usr/local/bin/flask", line 8, in <module>
web_1    |     sys.exit(main())
web_1    |   File "/usr/local/lib/python3.7/site-packages/flask/cli.py", line 967, in main
web_1    |     cli.main(args=sys.argv[1:], prog_name="python -m flask" if as_module else None)
web_1    |   File "/usr/local/lib/python3.7/site-packages/flask/cli.py", line 586, in main
web_1    |     return super(FlaskGroup, self).main(*args, **kwargs)
web_1    |   File "/usr/local/lib/python3.7/site-packages/click/core.py", line 782, in main
web_1    |     rv = self.invoke(ctx)
web_1    |   File "/usr/local/lib/python3.7/site-packages/click/core.py", line 1259, in invoke
web_1    |     return _process_result(sub_ctx.command.invoke(sub_ctx))
web_1    |   File "/usr/local/lib/python3.7/site-packages/click/core.py", line 1066, in invoke
web_1    |     return ctx.invoke(self.callback, **ctx.params)
web_1    |   File "/usr/local/lib/python3.7/site-packages/click/core.py", line 610, in invoke
web_1    |     return callback(*args, **kwargs)
web_1    |   File "/usr/local/lib/python3.7/site-packages/click/decorators.py", line 73, in new_func
web_1    |     return ctx.invoke(f, obj, *args, **kwargs)
web_1    |   File "/usr/local/lib/python3.7/site-packages/click/core.py", line 610, in invoke
web_1    |     return callback(*args, **kwargs)
web_1    |   File "/usr/local/lib/python3.7/site-packages/flask/cli.py", line 848, in run_command
web_1    |     app = DispatchingApp(info.load_app, use_eager_loading=eager_loading)
web_1    |   File "/usr/local/lib/python3.7/site-packages/flask/cli.py", line 305, in __init__
web_1    |     self._load_unlocked()
web_1    |   File "/usr/local/lib/python3.7/site-packages/flask/cli.py", line 330, in _load_unlocked
web_1    |     self._app = rv = self.loader()
web_1    |   File "/usr/local/lib/python3.7/site-packages/flask/cli.py", line 388, in load_app
web_1    |     app = locate_app(self, import_name, name)
web_1    |   File "/usr/local/lib/python3.7/site-packages/flask/cli.py", line 240, in locate_app
web_1    |     __import__(module_name)
web_1    |   File "/code/app.py", line 20
web_1    |     def hello():                                                                                            count = get_hit_count()                                                                             return 'Hello World! I have been seen {} times.\n'.format(count)
web_1    |                                                                                                                                                                                                                      ^
web_1    | SyntaxError: invalid syntax
composetest_web_1 exited with code 1



# 查看启动的两个服务
[root@buzhi my-dockerfile]# docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS          PORTS      NAMES
d749c51379a3   redis:alpine   "docker-entrypoint.s…"   7 minutes ago    Up 7 minutes    6379/tcp   composetest_redis_1
27a00b94ce2d   redis:alpine   "docker-entrypoint.s…"   25 minutes ago   Up 25 minutes   6379/tcp   docker-compose-learn_redis_1
```





# Docker Swarm

集群方式的部署、