getgit在本地有三个区域：

工作区：写代码的地方

git  add  到

暂存区：临时存储

git commit 到

本地库：历史版本



# Git命令行操作

## 本地库初始化 

- 命令： git init   
- 效果：会生成.git目录（Git 只在仓库的根目录生成 .git 目录）
- 注意：这里面存放的是本地库相关的子目录和文件，不要删除和做任何胡乱的修改

设置签名

- 形式   用户名加Email地址的形式

- 作用：区分不同开发人员的身份

- 辨析：这里设置的签名和登录远程库的账号、密码没有任何关系

- 命令

  - 项目级别/仓库级别：仅在当前本地库范围内有效

    git **config** user.name  mitsuhacon_pro

    git **config** user.email mitsuhacon@gmail.com

  - 系统用户级别：例登录当前操作系统的用户范围

    git config **--global** user.name mitsuhacon_

    git config **--global** user.email mitsuhacon@gmail.com

    查看global中的信息     cd ~          cat ./gitconfig

  - 优先级：就近原则，项目级别优先于系统用户级别，是不允许两个级别都不存在的情况

![image-20200518123604793](C:\Users\MitsuhaCon\AppData\Roaming\Typora\typora-user-images\image-20200518123604793.png)



## 查看配置信息

```shell
 # 检查已有的配置信息，有时候会看到重复的变量名，那就说明它们来自不同的配置文件比如 /etc/gitconfig 和 ~/.gitconfig），不过最终 Git 实际采用的是最后一个。
 git config --list
```

## 查看当前目录的用户信息

```shell
#用户名
git config user.name
#用户邮箱
git config user.email
```



## git status 查看状态   git add添加到暂存区  git commit提交

vim temp.txt    -->   git add temp.txt   -->  git status   

git rm --cached temp.txt （删除了暂存区的数据，并没有删除本体） --> git status 

git add temp.txt --> git commit temp.txt   会进入一个输入msg的vim，使用**:set nu**来显示行号

> 可以使用   git commit -m "commit message" temp.text              这样就不用使用来编辑了

## 查看历史记录git log

> 由于用 git log 可能要多屏显示，那么空格向下翻页，b向上翻页，q退出
>
> 为了好看日志   可以用   
>
> git log --pretty=online
>
> git log --online
>
> git reflog   //HEAD@{}移动多少步到想要去的版本。

通过 **--graph** 选项，查看历史中什么时候出现了分支、合并。

如果只想查找指定用户的提交日志可以使用命令：**git log --author** , 例如，比方说我们要找 Git 源码中 Linus 提交的部分：

```shell
git log --author=Linus --oneline -5
```



## 版本控制

先使用git reflog想看版本信息

- 再用git reset --hard [局部索引值]

> tail -n 3  temp.txt  显示temp中最后三行数据

- git reset --hard HEAD^只能后退
- git reset --hard HEAD~3也是只能后退，后面的数字是后退几步

**reset命令的三个参数对比**

--soft 参数  仅仅在本地库移动HEAD指针

--mixed 参数  在本地库移动HEAD指针，重置暂存区

--hard 参数 在本地库移动HEAD指针，重置暂存区，重置工作区



**git diff [文件名]** 将工作区中的文件和暂存区进行 比较

**git diff [本地库中历史版本] [文件名]** 将工作区中的文件和本地库历史记录比较

- 尚未缓存的改动：**git diff**
- 查看已缓存的改动：**git diff --cached**
- 查看已缓存的与未缓存的所有改动：**git diff HEAD**
- 显示摘要而非整个 diff：**git diff --start**

## git branch -v查看分支

git branch  无参数时，会列出你在本地的所有分支

git branch [分支名]  来创建一个分支

git checkout [已有分支名] 来进行分支切换

git merge [子分支名]  -d  这个命令是站在master角度上的，合并完就删除分支

如果产生conflict，那么就解决，解决了之后，就用 **git add [文件名]** 添加到暂存区，并使用 **git commit -m "日志信息"**不带 文件名

我们也可以使用 **git checkout -b (branchname)** 命令来创建新分支并立即切换到该分支下，从而在该分支中操作。 

```
git branch -b (branchname)
```

删除分支

```shell
git branch -d (branchname)
```



## git remote -v查看远程地址

git remote add 'origin'  远程地址      //添加一个远程地址并取名为origin

git remote remove origin // 移除远程仓库地址

git push origin master   //将本地数据推送到远程

> 当远程仓库中有一些文件而本地没有时 ，需要进行  **git pull --rebase origin master**    来拉取文件。



## git clone 远程地址

git clone [远程地址]

效果：完整的把远程库下载到本地，创建origin远程地址别名，初始化本地库。

```shell
git clone <repo>
git clone <repo> <directory>

repo：git 仓库
directory：本地目录
```



## 拉取

pull = fetch + merge

git pull origin  marster

## 解决冲突

如果不是基于GitHub远程仓库的最新版本所做的修改，不能推送，必须先拉取

拉取下来后如果进入状态，则按照”分支冲突解决“操作解决即可。

## 通过SSH拉取

```git
$ ssh-keygen -t rsa -C git@github.com:MitsuhaCon/Learn_Git.git
注意：这里-C是大写的
进入.ssh 目录查看文件列表
cd ~/.ssh
ls -lF
查看id_rsa.pub文件内容
cat id_rsa.pub
复制id_rsa.pub文件内容，登录GitHub，点击头像->Settings->SSH and GPG keys
New SSh Key
输入复制的密钥信息
回到Git bash 创建远程地址别名
git remote origin_ssh 加ssh key
git push origin_ssh master
```

## 更新git软件版本

> git update-git-for-windows