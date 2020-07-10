# 1 创建一个vue项目

- 下载node并安装

  安装成功后使用  **node -v** 能查看到版本就说明node安装成功

  > node中自带了npm，但可能不是最新的，所以需要使用如下指令更新
  >
  > npm install -g

由于直接使用npm的官方镜像比较慢，推荐使用淘宝npm镜像

```shell
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

- 项目初始化

  1. 安装vue-li

     ```shell
     #全局安装vue-li
     > cnpm install vue-li -g
     ```

     查看vue-cli是否成功，需要检查vue

     ```shell
     > vue list
     ```

     当成功出来Available official templates时就说明安装vue-cli成功。

  2. 选定路径，新建vue项目

     ```shell
     #切换到要创建项目的位置然后执行
     > vue init webpack "项目名称"
     #在Install vue-router时选择Y
     ```

  3. 创建好之后，进行依赖安装

     ```shell
     #安装依赖的时候，目录要切换到项目目录
     
     #安装依赖
     > cnpm install
     #运行项目
     > npm run dev
     #其实是找得项目下的package.json文件中的scripts -> dev
     ```

  4. 启动成功，可以通过**http://localhost:port**d在浏览器中查看

