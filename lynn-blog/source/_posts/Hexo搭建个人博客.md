---
title: Hexo搭建个人博客
date: 2017-06-05 09:30:23
categories: 
- Coding
tags: 
- blog
- node
- 服务器
comments: true
thumbnail: http://or0wd539m.bkt.clouddn.com/Untitled.jpg
---
5月份尝试过用node.js+express+mongodb搭建博客，参考了这篇文章[NodeJS和Express搭建多人博客](http://wiki.jikexueyuan.com/project/express-mongodb-setup-blog/simple-blog.html)
。实现了注册、登录、发布功能，用了ejs渲染。结果mongodb的各种中间件过于复杂，前端很难写。

目前主流的个人博客框架有[jekyII](http://jekyllrb.com/)和[Hexo](https://hexo.io/)两款比较流行。

<!--more-->

jekyII和Hexo都支持markdown语法，其中jekyII基于Ruby，Hexo基于Node.js。鉴于Node.js经验比较丰富，npm等命令比较熟悉，选择了Hexo。

# 准备工作

>[BandwagonHost](https://bwh1.net/) VPS一台，Ubuntu16.04
>
>[Node.js](https://nodejs.org/zh-cn/) v7.7.1 (本地和服务 器)
>
>[GitHub](https://github.com/) 私有库


Nodejs服务器配置参考以下两篇文章

[CentOS](http://blog.csdn.net/cdnight/article/details/52541165)

[Ubuntu](http://blog.csdn.net/u013806814/article/details/51960696)

# Hexo

安装Hexo

```shell
npm install hexo-cli -g 
```

初始化项目

```shell
hexo init [folder]
```

安装主题

[Next](http://theme-next.iissnan.com/)

新建文章

```shell
hexo new [layout] [title]
```


# GitHub

GitHub网页上新建库，然后本地以下命令

```shell
git init
git add .
git remote origin <ssh>
git push origin master
```

GitHub上作为博客备份

# 服务器端配置

安装Nginx

```shell
apt-get install nginx
nginx -v
```


[修改Nginx配置](http://atomlx.com/2016/08/12/test/)

# 远程部署

这里选用rsync方法进行部署

服务端

```shell
apt-get install rsync
```

本地

```shell
npm install hexo-deployer-rsync --save
```


本地配置 _config.yml

```json
deploy:
  type: rsync
  host: ip
  user: root
  root: /var/www/html
  port: 22
```


# 免密码部署

可以发现如果用root进行ssh部署，每次都需要输入密码。而且bandwagon每次提供的ssh密码均为随机密码，很难输入。  
这里思路是在vps里新建用户，然后再该用户目录里面部署blog。将本地的公钥存储在authorized_key里面，以后进行ssh连接可以进行免密码。同时新建用户的权限比较低，所以整体系统也比较安全。  
参考以下文章：[CSDN](http://blog.csdn.net/leexide/article/details/17252369)  [BURKEXU'BLOG](http://ch0ngbo.com/2016/01/16/hexo%E9%83%A8%E7%BD%B2%E5%88%B0VPS/)


至此，新建文档后，通过

```shell
hexo g -d
```

实现一键部署。



