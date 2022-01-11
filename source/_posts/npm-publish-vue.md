---
title: 发布vue插件到npm
date: 2020-12-25 15:17:55
tags:
    - npm
    - vue
categories:
    - node
descriptions:
---

> 日常开发中，我们更多的是通过npm使用插件，如果有开源的项目或者组件化的功能也可以通过npm进行管理，所以有必要了解下npm插件的发布的流程。

## 初始化目录
```bash
npm init -y
```
并手动创建lib目录及index.js入口文件

```
├── lib // 插件源码
│   ├── components // 组件目录
│   │   └── OwnCom.vue // 组件文件
│   └── index.js  // 插件入口文件
├── index.js // 入口文件
└── package.json  // 包管理文件
```

## 写入文件内容

### package.json

修改包的名称(名称不要重复，可以去npm官网上搜索下能不能搜索出记录)和版本，添加描述内容
```json
{
  "name": "rhymin-life",
  "version": "0.0.1",
  "description": "ygr test vue plugin",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}

```
### OwnCom.vue

这里只是写入模板，内容可根据实际情况写入
```vue
<template>
  <div>
    这是一个自定义组件
  </div>
</template>

<script>
export default {};
</script>

<style>
</style>
```
### lib/index.js
在这里封装插件：将自己的组件注册到vue，并可以选择设置其他选项

```js
import OwnCom from './components/OwnCom.vue';
let MyPlugin = {};
MyPlugin.install = function(Vue, options) {
  Vue.component("OwnCom", OwnCom);
  // 1. 添加全局方法或 property
  Vue.myGlobalMethod = function() {
    // 逻辑...
    console.dir(options);
    console.log("我是全局方法");
  };

  // 2. 添加全局资源
  Vue.directive("my-directive", {
    bind() {
      console.log("指令绑定成功");
    },
  });

  // 3. 注入组件选项（全局mixin慎用）
  Vue.mixin({
    created: function() {
      console.log("混入初始化打印");
    },
  });

  // 4. 添加实例方法
  Vue.prototype.$myMethod = function() {
    console.log("添加实例方法");
  };
};
export default MyPlugin;

```

### index.js (入口文件)

作为package.json识别的入口文件，在这里统一引入插件，然后导出
```js
import MyPlugin from './lib/index';
export default MyPlugin
```
## 开始发布

插件写好后，我们需要发布到npm包管理服务器，如果没有账号，需要先[注册账号](https://www.npmjs.com/)

账号注册号后，命令行登录

```bash
npm login
```
后面会提示输入密码和邮箱，然后提示登录成功

```bash
Password:
Email: (this IS public) freedom_101@163.com
Logged in as rhymin97 on https://registry.npm.taobao.org/.
```
> 这里可以看到，现在登录到的是淘宝的镜像地址(通常我们下载依赖时，由于npm官网上下载过于缓慢，会把下载的目标设置成国内的镜像地址，需要先修改回去)

可以查看下自己的登录源
```bash
npm config get registry
```
如果是淘宝地址如下:

```bash
https://registry.npm.taobao.org/
```
修改登录源

```
npm config set registry=http://registry.npmjs.org
```

再次查看如果是以下地址则正确

```bash
http://registry.npmjs.org/
```
再次登录

```bash
npm login
Username: rhymin97
Password:
Email: (this IS public) freedom_101@163.com
Logged in as rhymin97 on http://registry.npmjs.org/.
```
执行发布


```bash
npm publish
```
但是似乎没有成功，报了如下的错误，搜索了下问题，邮箱必须要验证(最好用电脑点开邮箱给的链接地址，自己用手机点开后还是一直提示验证邮箱)
```
npm ERR! code E403
npm ERR! 403 403 Forbidden - PUT http://registry.npmjs.org/rhymin-life - Forbidden
npm ERR! 403 In most cases, you or one of your dependencies are requesting
npm ERR! 403 a package version that is forbidden by your security policy.
```

再次执行发布命令，显示如下信息则发布成功。

```bash
npm notice
npm notice package: rhymin-life@0.0.1
npm notice === Tarball Contents ===
npm notice 35B  index.js
npm notice 731B lib/index.js
npm notice 244B package.json
npm notice 139B lib/components/OwnCom.vue
npm notice === Tarball Details ===
npm notice name:          rhymin-life
npm notice version:       0.0.1
npm notice package size:  843 B
npm notice unpacked size: 1.1 kB
npm notice shasum:        42cc22c9a74db044a5a126cbcaeb2b058c06947e
npm notice integrity:     sha512-2crwoPnDJEx0t[...]yycEcnP/4WzSg==
npm notice total files:   4
npm notice
+ rhymin-life@0.0.1
```
> 进入到自己的npm主页下的packages下，可以看到自己发布的包


## 使用

尝试安装一下自己的包

> 最好换个项目区安装，不然会提示这个安装的包名和package.json的名称一样报错。

```bash
npm i rhymin-life
```
依赖下载成功后，引入文件

```bash
import MyPlugin from "rhymin-life";
Vue.use(MyPlugin);
```
> 之后上面的代码后会发现控制台打印`混入初始化打印`，由插件内部执行的created方法

使用全局方法

```js
Vue.myGlobalMethod()
//log:我是全局方法
```
使用实例方法

```vue
  created() {
    this.$myMethod()
  },
  //log:添加实例方法
```


使用该组件并绑定指令

```vue
<own-com v-my-directive/>
//log:指令绑定成功
```
## 补充

最后，记得改回自己的镜像源，毕竟国外npm官网比较慢

```bash
npm config set registry=http://registry.npmjs.taobao.org
```
> 注：如果立即切换回淘宝镜像源然后安装自己发布的插件，可能自己的插件还没有更新到淘宝的镜像网上，需要等待一段时间(也许十来分钟)


