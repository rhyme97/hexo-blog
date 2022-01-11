---
title: hexo + centos 实现博客搭建到个人服务器
date: 2020-07-17 22:42:40
descriptions: 活着像为了押韵
tags:
 - hexo
 - 教程
categories:
 - hexo
toc_number: 3
---
## 写在前面


hexo，一个静态的不用自己搭建数据库的博客框架，以及使用markdown文档格式编辑文章，不用花太多的时间去维护和更新，更新文章时，只要把平常写的markdown笔记迁移就完成了，对于个人而言，简单实用。

## 环境

我的云服务器是centos7的，本机是window10，基本的环境git，nodejs，nginx就不赘述了。

## 云服务器配置 (本机finalshell远程登录)

创建一个用户用来管理git（yuanguanrui是我的用户名，可以设置其他名字）

```bash
adduser yuanguanrui
```

设置密码

```bash
passwd yuanguanrui
# 后面提示输入密码
```

初始化仓库 （这是我的仓库路径:'/usr/node_serve/node_project/blog.git'）

```bash
cd /usr/node_serve/node_project
git init --bare blog.git 
```

另外的，git如果没有设置用户名和密码记得设置，自报家门是必须得,你的名字和Email地址。

```bash
git config --global user.name "zhangsan"
git config --global user.email "zhangsan@gmail.com"
```

修改仓库所有者

```bash
sudo chown -R yuanguanrui:yuanguanrui blog.git
```

出于安全考虑，禁用shell登陆，这可以通过编辑/etc/passwd文件完成。找到类似下面的一行：

```bash
git:x:1001:1001:,,,:/home/git:/bin/bash
```

改为：

```bash
git:x:1001:1001:,,,:/home/git:/usr/bin/git-shell
```

新建一个post-receive文件，可以自动同步代码到nginx站点根目录的,该文件路径为git站点下的hooks文件夹下面（/usr/node_serve/node_project/blog.git/hooks/post-receive）,然后在该文件输入以下内容：

```bash
#！/bin/sh
git --work-tree=/usr/share/nginx/html/hexo --git-dir=/usr/node_serve/node_project/blog.git checkout -f
```

保存退出之后，再输入以下代码，赋予该文件可执行权限。(权限很重要，如果莫名的尝试不行可能就是那个文件权限没有)

```bash
chmod -R 777 post-receive
```

上面配置的`/usr/share/nginx/html/hexo `就是我的nginx站点跟目录，需要在nginx的安装目录下的`nginx.conf`文件配置：

```bash
vim /usr/local/nginx/conf/nginx.conf
```

编辑这部分的root后面的路径（只所以多加了public是因为对于项目同步到站点路径后，index.html文件在public里）：

```bash
location / {
   root   /usr/local/nginx/hexo/public;
   index  index.html index.htm;
}
```

给站点根目录设置读写权限

```bash
chmod 777 /usr/share/nginx/html/hexo
```

编辑后记得重启nginx

```bash
#我是在我的nginx下的sbin目录启动nginx的,反正重启一下
cd /usr/local/nginx/sbin
./nginx -s reload
```
## 本机的操作
本机拉取服务器上的git项目(rhymin.cn是我的域名，没有解析的话填公网的ip),会生成一个blog文件，里面是git空项目，只有一个`.git`文件
```bash
git clone yuanguanrui@rhymin.cn:/usr/node_serve/node_project/blog.git
```
此时再初始化一个hexo博客
``` bash
# 没有安装hexo的记得先安装
npm install -g hexo-cli
# 自己找一个路径，不要直接搞到git的blog文件了
hexo init blog
#生成静态文件public，这个会被同步到nginx的站点根目录
hexo g
```
然后把这个blog文件里的东西copy到上一个拉取git项目生成的blog文件里（或者把git项目里的`.git`文件copy到hexo的blog文件，另外一个可以删了）。
此时这个git空项目就添加了hexo的初始化博客,下面就可以push上去了
```bash
 # push之前记得把.gitignore里的public/这行去掉，这个文件需要提交给nginx解析的
 git add.
 git commit '初始化提交'
 git push
 # 我是用tortoisegit提交，推送时会提示输入密码，就是设置yaunguanrui用户时设置的密码

```
## 最后
查看服务器上的 `/usr/share/nginx/html/hexo`路径下多了刚上传的hexo博客内容，然后浏览器输入：[http://rhymin.cn](http://rhymin.cn)就可以看到博客内容了。

后续如果想发表文章，不管在哪台电脑上，先拉取项目
``` bash
git clone yuanguanrui@rhymin.cn:/usr/node_serve/node_project/blog.git
```
修改后执行清除缓存，
```
hexo clean
```
然后生成新的public文件，
```
hexo g
```

再上传`git push` 就实现了博客的同步更新。