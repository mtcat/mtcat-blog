---
title: node服务器部署
date: 2020-01-13 22:31:12
tags: node 服务器
---

# 安装NodeJS

升级 CentOS 的 yum
```
yum -y update
```

安装常用库
```
yum -y install gcc gcc-c++ autoconf
```

跳转到 /usr/local/src , 这个文件夹通常用来存放软件源代码
```
cd /usr/local/src
```

<!-- more -->

下载 nodejs 代码，这里下载的是v8.16.0版本，可以根据自己的需要的版本自行安装
```
// 缺少 wget 则需要先安装 wget
// sudo yum -y install wget
wget http://nodejs.org/dist/v8.16.0/node-v8.16.0.tar.gz
```

解压
```
tar -xzvf node-v8.16.0.tar.gz
```

进入解压后的文件夹
```
cd node-v8.16.0
```

执行配置脚本来进行编译预处理
```
./configure
```

编译源代码
```
make
```

当编译完成后，需要使之在系统范围内可用, 编译后的二进制文件将被放置到系统路径，默认情况下，Node二进制文件应该放在/user/local/bin/node文件夹下
```
make install
```

建立超级链接, 不然 sudo node 时会报 "command not found"（这个步骤目前还没有经过实践，如果遇到这个问题可以实践看看，一般情况下没有这个步骤，node、npm已经是全局）
```
sudo ln -s /usr/local/bin/node /usr/bin/node
sudo ln -s /usr/local/lib/node /usr/lib/node
sudo ln -s /usr/local/bin/npm /usr/bin/npm
sudo ln -s /usr/local/bin/node-waf /usr/bin/node-waf
```

需要yarn的可以根据需求自行安装
```
npm install yarn -g
```

NodeJS 到这里就基本安装完成了。

<br>

# 也可以使用已经编译好的文件（没有经过实践）

在官网选择需要下载的版本
```
sudo wget https://nodejs.org/dist/v8.16.0/node-v8.16.0-linux-x64.tar.xz
```

接下来解压到当前目录
```
sudo tar -xvf node-v8.16.0-linux-x64.tar.xz
```

有需要可以更改目录名
```
sudo mv node-v8.16.0-linux-x64 nodejs
```

创建全局链接
```
sudo ln -s /usr/local/src/nodejs/bin/node /usr/local/bin/node  
sudo ln -s /usr/local/src/nodejs/bin/npm /usr/local/bin/npm
```

<br>

参考文档
> https://www.jianshu.com/p/4895cda92808?utm_campaign=maleskine&utm_content=note&utm_medium=seo_notes&utm_source=recommendation
> https://www.jianshu.com/p/7fb19781e158
> https://www.jianshu.com/p/0496ef49b2a5



# 安装进程守护软件 PM2

安装pm2
```
npm install pm2 -g
```

将项目上传到服务器，进入项目根目录，假设node项目的入口文件为app.js
```
pm2 start app.js --name "app-1" --watch true
```
运行 -- 后面是参数 --name 改名， --watch 文件或文件夹变更时自动重启

<br>

把node服务加到进程

centos
```
pm2 startup centos
pm2 save
```
ubuntu
```
pm2 startup ubuntu
pm2 save
```
这样，NodeJS 就一直在后台运行了，就算重启了，也自动运行。

<br>

常用pm2命令总结：
```
启动应用
pm2 start app.js

可以查看当前开启的实例
pm2 ls

列出所有应用
pm2 list

查看资源消耗
pm2 monit

查看某一个应用状态
pm2 describe [app id]

查看所有日志
pm2 logs

重启应用
pm2 restart [app id]

停止应用
pm2 stop [app id]

开启api访问
pm2 web
```

<br>

> 详情查看官网地址：http://pm2.keymetrics.io/docs/usage/quick-start/
