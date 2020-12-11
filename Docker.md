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

