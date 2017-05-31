---
title: node系列--最终项目部署上线
date: 2017-05-31 09:56:00
tags: NodeJs
---
### node后台接口服务写好,终于要丢到服务器上了
vps服务端使用的ubuntu系统,大致分为五步,其他系统参考步骤来google。

安装node----安装MongoDB----gitClone项目----安装PM2守护进程----修改监听端口为80----域名解析
1. 安装NodeJs

官网有教程和命令

    $~ curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash - 
    $~ sudo apt-get update
    $~ sudo apt-get install -y nodejs

在这里检查是否安装成功,
    
    $~ node-v
2. 安装MongoDB

首先安装public key

    $~ sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6

安装好后 根据你的系统版本来安装不同版本的[MongoDB](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/)
我这里用的是ubuntu 16.04

    $~ echo "deb [ arch=amd64,arm64 ] http://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list

    $~ sudo apt-get update

    $~ sudo apt-get install -y mongodb-org

到这里 MongoDB就安装好了,来一条非常简单明了的命令启动

    $~ sudo service mongod start

3. gitClone项目至服务器

这里使用的命令行和win下一模一样,就不多解释了.直接
    
    $~ git clone https://github.com/xxxxxxx/xxxxx.git
    $~ npm install

等到项目所有依赖安装完毕后,进行下一步

4. 安装PM2守护进程

这里使用npm直接安装就行了

    $~ npm install pm2 -g

安装好后,我们试着跑一下我们的服务,没有报错就可以直接用vps的ip访问项目了。

    $~ pm2 start app.js

并且pm2会将node进程以后台的形式运行。
不需要一直挂起。


这里有个注意的点，pm2没有root权限,所以无法监听1024以内的端口，所以这里各位之前写了1024端口以下的监听,这里肯定就报错了。

但是网站怎么可能去跑8080 3000 3001 这种奇葩端口呢？

我所知道的提供两种解决方案，假设我们的app.js监听的是3000端口。

 1、设置端口映射，将80端口的访问都转到3000。
 2、使用lib2cap-bin

推荐使用第一种,一行命令搞定。

    $~ sudo iptables -t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT --to-ports 3000

第二种的话就稍微麻烦点

首先安装lib2cap-bin

    $~ sudo apt-get install lib2cap-bin

使用 setcap 添加cap_net_bind_service

    $~ sudo setcap cap_net_bind_servicec=+ep `readlink -f \`which node\``

将app.js的监听端口改为80

cd到项目内,使用自带笔记本打开app.js

    sudo nano app.js
直接在命令行中找到listen,改为80。

Ctrl+x 
Y 回车

再次运行pm2 start app.js 就可以成功监听80端口了。

最后去买一个域名，解析到我们的ip地址。

至此，整个项目部署完毕。