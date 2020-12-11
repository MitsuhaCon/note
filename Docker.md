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

