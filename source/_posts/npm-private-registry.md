---
title: npm私服搭建
date: 2021-7-16 15:17:56
tags:
    - npm
categories:
    - node
descriptions:
---


# 前言

当平台的项目群体太多，重复的功能代码滋生也难以维护，组件的复用就显得尤为重要，所以为了在不同的项目中复用组件，我们选择将这些组件封装到公共组件库，并通过npm的方式进行发布。
不过，由于代码的性质并不希望公开到npm官网，所以通过搭建npm私服来进行管理。


目标：
- 组件发布到私服，同时接入其他镜像代理
- 可以将国外资源包放至私服，并知道下载私服的资源
- 组件开发成员注册账户，允许发布删除，并禁止其他用户注册
- 开启发布通知功能，获悉版本的更迭

# 方案1:verdacci

`verdaccio`是一个node.js创建的轻量的私有npm仓库.是开发普遍选择的一款方案，相当易于安装和使用，是本地网络proxy，与yarn，npm，pnpm百分百向后兼容。

## node环境安装

> 使用nvm管理安装node.环境：centos7.6

使用nvm管理nodejs
```bash
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
```
然后执行
```bash
source ~/.bashrc
```
查看是否安装成功
```bash
nvm -v
```
罗列node的所有版本
```bash
nvm ls-remote
```
选择一个版本安装(这里选择了一个长期支持版)
```bash
# 每当我们安装了一个新版本 Node 后，全局环境会自动把这个新版本设置为默认。
nvm install 14.18.1
```
列车已经安装版本
```bash
nvm ls
```
如果已有安装其他版本想要使用，切换命令：
```bash
nvm use <version>
```
查看当前使用版本
```bash
nvm current
```
删除某个版本
```bash
nvm uninstall <version>
```
查看node是否安装成功
```bash
node -v
npm -v
```

> 在项目中使用不同版本的 Node

我们可以通过创建项目目录中的 .nvmrc 文件来指定要使用的 Node 版本。之后在项目目录中执行 nvm use 即可。.nvmrc 文件内容只需要遵守上文提到的语义化版本规则即可。另外还有个工具叫做 avn，可以自动化这个过程。

> 在多环境中，npm该如何使用呢？

每个版本的 Node 都会自带一个不同版本的 npm，可以用 npm -v 来查看 npm 的版本。全局安装的 npm 包并不会在不同的 Node 环境中共享，因为这会引起兼容问题。它们被放在了不同版本的目录下，例如 ~/.nvm/versions/node/<version>/lib/node_modules</version> 这样的目录。这刚好也省去我们在 Linux 中使用 sudo 的功夫了。因为这是用户的主文件夹，并不会引起权限问题。

但问题来了，我们安装过的 npm 包，都要重新再装一次？幸运的是，我们有个办法来解决我们的问题，运行下面这个命令，可以从特定版本导入到我们将要安装的新版本 Node：

```bash
nvm install v5.0.0 --reinstall-packages-from=4.2
```

## verdaccio搭建

### 安装运行

安装
```bash
npm install -g verdaccio
```
检查安装
```bash
verdaccio -h
```
执行verdaccio即可运行
```bash
verdaccio
```
运行后可以在后面的信息中看到
```bash
 warn --- config file  - /root/.config/verdaccio/config.yaml
(node:57156) Warning: Verdaccio doesn't need superuser privileges. don't run it under root
(Use `node --trace-warnings ...` to show where the warning was created)
(node:57156) Warning: Verdaccio doesn't need superuser privileges. don't run it under root
 warn --- Plugin successfully loaded: verdaccio-htpasswd
 warn --- Plugin successfully loaded: verdaccio-audit
 warn --- http address - http://localhost:4873/ - verdaccio/5.2.2
```
里面指明了配置文件`config.yaml`地址以及正在运行的地址`http://localhost:4873/`.

上面运行了后，可能内网其他主机的浏览器不能通过localhost直接访问，需要修改配置文件.

### 配置

配置文件里有一些默认配置
```yaml
# 默认存储
storage: ./storage
# 认证。默认的授权是基于htpasswd 并且是内置的
auth:
  htpasswd:
    file: ./htpasswd
# 上行链路是与外部注册表的链接，可提供对外部包的访问。
uplinks:
  npmjs:
    url: https://registry.npmjs.org/
# 可以根据包名的不同设置不同的来源
packages:
  '@*/*':
    access: $all
    publish: $authenticated
    proxy: npmjs
  '**':
    proxy: npmjs
logs:
  - {type: stdout, format: pretty, level: http}
```
我们在配置文件的后面添加一个配置
```bash
listen:
# - localhost:4873            # default value
# - http://localhost:4873     # same thing
- 0.0.0.0:4873              # listen on all addresses (INADDR_ANY)
# - https://example.org:4873  # if you want to use https
# - "[::1]:4873"                # ipv6
# - unix:/tmp/verdaccio.sock    # unix socket
```
了解node进程的都知道listen是运行在哪个端口，这里我们开放0.0.0.0:4873，保证其他电脑的浏览器也能访问。

### PM2进程守护node进程

```bash
npm install -g pm2 --unsafe-perm
pm2 start verdaccio
```
其他常用命令
```bash
# 状态管理
$ pm2 stop     <app_name|namespace|id|'all'|json_conf>
$ pm2 restart  <app_name|namespace|id|'all'|json_conf>
$ pm2 delete   <app_name|namespace|id|'all'|json_conf>
# 运行列表
$ pm2 list
# 详情
$ pmw describe <id|app_name>
```

## 实践

### 1.为组件库的开发人员创建账号

执行命令行并设置用户账号密码，公开的邮箱。
```bash
[root@gtsgs ~]# npm adduser --registry http://192.168.10.101:4873/
Username: admin
Password: 
Email: (this IS public) xxx@xxx.com
Logged in as admin /192.168.10.101:4873/.
```
> 注册过后默认以此用户登录

### 2.更改权限

假设公司组件库都以`sgs-`开头，设置这些组件只有注册的用户才能发布删除。然后其他公共库的组件可以放至私有库并优先从私有库获取，其他的设置公共库的代理地址。

```bash
uplinks:
  npmjs:
    url: https://registry.npmjs.org/
  taobao:
    url: https://registry.npm.taobao.org

packages:
  '@sgs/*':
    # scoped packages
    access: $all
    publish: $authenticated
    unpublish: $authenticated
    proxy: npmjs
  'node-sass':
    access: $all
    publish: $authenticated
    unpublish: $authenticated
    proxy: taobao
  '**':
    access: $all
    publish: $authenticated
    unpublish: $authenticated
    # if package is not available locally, proxy requests to 'npmjs' registry
    proxy: npmjs
```

> 更改完了配置文件记得重启生效

### 3.禁止用户注册

上面我们为以注册的用户开启了发布删除权限，然后我们要禁止用户注册将权限控制起来。
```bash
auth:
  htpasswd:
    file: ./htpasswd
    // 此配置项可以关闭注册功能
    max_users: -1
```

### 4.删除用户

注册的用户的资料会保存在`~/.config/verdaccio/htpasswd`文件中,格式如下：
```bash
guanrui:Wxrgh7Es6zK9U:autocreated 2021-11-16T02:40:32.277Z
rui:GTz2z5dCKNWJY:autocreated 2021-11-16T02:42:52.287Z
ygr:W2gDWuU3UuJsE:autocreated 2021-11-16T02:49:15.663Z
```
删除对应的行也就删除了该用户。

### 5.客户端项目开发切换为私有源

通过nrm进行源管理
```bash
npm i -g nrm
```
添加源
```bash
nrm add sgsnpm http://192.168.10.101:4873/
```
查看源列表
```bash
nrm ls
```
使用源
```bash
nrm use sgsnpm
```
然后执行安装时默认链接私有库资源

如果只想在项目中使用私服，可以直接使用参数安装
```bash
npm install --registry http://192.168.10.101:4873
```
或者在项目根目录中添加`.npmrc`文件（如果设置的地址错误那么安装将会失败）:
```bash
registry=http://192.168.10.101:4873
```
### 6.项目人员开发好组件后发布


登录
```bash
npm login
```
查看当前登录用户
```bash
npm whoami
```
发布
```bash
npm publish
```
取消登录
```bash
npm logout
```
### 7.开启搜索

发布好后，就可以在web界面进行查找使用了。在配置文件增加配置，以便于搜索
```bash
search: true
```

### 8.发布通知

发布通知到项目部门的企业微信群组里面

在企业微信群组里找到群组机器人，带年纪添加，得到webhook地址，以如下方式添加到配置文件：

```bash
notify:
  method: POST
  headers: [{'Content-Type': 'application/json'}]
  endpoint: https://qyapi.weixin.qq.com/cgi-bin/webhook/send?key=********
  content: '{"color":"green","message":"New package published: * {{ name }}*","notify":true,"message_format":"text"}'
```

> 收不到消息。。。


## 问题

1. 项目中共设置了`.npmrc`后，如果npm私服平台挂掉，项目想要必然无法拉取组件资源，并且其他的公共资源也无法通过私服代理到公共资源库，这似乎只能依托于私服的稳定性。
2. 尚不确定teamcity的打包环境中下载私服依赖是否可行
3. 平台组件库时以个整体的库，而不是每个组件功能都是一个插件，看起来私服只是为一个库服务。（当然还可以缓存其他依赖）


# 方案2:nexus

nexus repository oss：全球排名第一的存储库管理器


- 为您的团队提供他们使用的每个组件的单一事实来源。
- 通过缓存远程存储库的代理来优化生成性能和可靠性。
- 为所有主要包裹类型和格式提供普遍覆盖
- 为无限数量的用户安装在无限数量的服务器上。

对所有常用构建工具的通用支持
- 存储和分发 Maven/Java、npm、NuGet、Helm、Docker、P2、OBR、APT、GO、R、Conan 组件等。
- 管理从开发到交付的组件：二进制文件、容器、程序集和成品。
- 对 Java 虚拟机 （JVM） 生态系统的高级支持，包括 Gradle、Ant、Maven 和 Ivy。
- 与 Eclipse、IntelliJ、Hudson、Jenkins、Puppet、Chef、Docker 等流行工具兼容。


为开发团队提供支持的效率和灵活性:
- 通过在内部共享组件来简化生产力。
- 深入了解组件安全性、许可证和质量问题。
- 通过远程包可用性离线构建。
- 与行业领先的构建工具集成



## 下载安装

地址：https://help.sonatype.com/repomanager3/product-information/download

> 这里选择的window版本，都需要JDK8。

然后进入到安装目录`\nexus-3.16.1-02\bin`下，执行cmd命令:
```bash
.\nexus.exe /run
```
启动之后默认地址为 localhost:8081, 默认账号密码 admin/ admin123

参考教程：
> https://www.cnblogs.com/tuituji27/p/11171780.html
> https://juejin.cn/post/6971799169933508638

## 配置

创建npm仓库：
- hosted（私有仓库）：用于发布个人开发的npm组件(需要设置允许更新)
- proxy（代理仓库）：可以代理npm和淘宝镜像
- group（组合仓库）：对外公开的仓库(可以将hosted和proxy添加到成员,越靠前优先级越高)



然后创建用户，设置其状态Active。设置Realms权限，加入`npm Bearer Token Realm`.

找到repositoreis中的npm-group，复制其地址,如`http://10.10.3.156:8081/repository/npm-group/`

## 使用

客户端使用：

```bash
# 添加源
nrm add ownnpm  http://10.10.3.156:8081/repository/npm-group/
```

然后进入到组件项目，修改当前源(这个源用于下载install用的)
```bash
nrm use ownnpm
```
或者修改其`.npmrc`文件.(这里要使用hosted私服用户登录和发布)
```bash
registry=http://10.10.3.156:8081/repository/npm-hosted/
```

> 如果是组件库项目，发布和登录需要使用npm-hosted的源，如果是其他要使用组件的项目，需要使用npm-group这个源来下载


查看当前使用的源
```bash
nrm current
```

查看是否登录或者当前用户
```bash
npm whomai
```
没有登录则进行登录,输入用户，密码，邮箱(注意是登录到私服hosted)
```bash
npm login -registry=http://10.10.3.156:8081/repository/npm-hosted/
```
最后可以进行发布了(发布到hosted)
```bash
npm publish  -registry=http://10.10.3.156:8081/repository/npm-hosted/
```

## 优缺点

优点：
- 后台界面管理
- 用户权限管理:增删改查
- 邮件通知
- 设置代理，代理缓存


问题：
- 包浏览不直观，不能显示readme

