---
title: 前端工程化的一些理解及整理
date: 2021-06-19 15:22:26
tags:
    - 代码规范
    - 持续集成
categories:
    - 前端工程化

descriptions:
---

## 参考链接

- [带你入门前端工程化](https://woai3c.github.io/introduction-to-front-end-engineering/)
- [从零开始手把手带你搭建一套规范的Vue3.x的项目工程环境](https://juejin.cn/post/6951649464637636622#heading-11)
- [git工作流](https://www.web3.xin/gitworkflow/215.htmlhttps://www.web3.xin/gitworkflow/215.html)
- [stylelint配合eslint+prettier的配置](https://juejin.cn/post/7033552881374461960)
- [npm verion版本号的管理](https://juejin.cn/post/6844904147892830221) 


## 工程化概念

不知道从什么时候开始，前端工程化这个词汇开始出现在相关开发人员的视线，在此之前，可能只听过软件工程，网络工程相关词汇。工程，本身就是一个很泛泛的概念。

前端工程化每个人都有不一样的理解，也没有一个权威和标准的定义，只要我们清楚为什么要工程化和工程化的意义那么具体的定义就变得不那么重要了。

工程化，就是将一系列的前端开发流程系统化，规范化。



## 技术选型



无论是大到技术框架、编程语言，还是小到工具库的选择，都属于技术选型的范围。最好从以下角度出发：

- 可控性

- 稳定性

- 适用性

- 易用性

其实就是少走弯路，并且便于维护。通常情况下直接使用主流的框架及它相关的一系列生态

## 统一规范

在实际开发中，通常每个人都遵循自己的代码习惯，可能偶尔在复制的时候注意到其他的代码，又依照着其它的风格写。如果仅仅是视觉上的割裂感还不至于对运行造成影响，如果代码实际运行会出错就进行提交，会让后面拉取代码的人无法作业；或者复制粘贴某些的代码段产生很多废物代码，后续进行维护的人员，由于项目太大，不敢保证它的特殊含义，将会对此非常费解。

### 命名规范

命名方式：

- camelCase（驼峰式，也叫小驼峰命名，e.g. userInfo）
- PascalCase（帕斯卡命名式，也叫大驼峰命名，e.g. UserInfo）
- kebab-case（短横线连接式，e.g. user-info）
- snake_case（下划线连接式，e.g. user_info）
- BEM命名规范

每一种命名方式都应该安装特定的方式使用，例如BEM在css的命名中使用，很多组件库也是使用这样的规范化命名，它可以有效规避选择器重复问题；在vue component的命名上应使用帕斯卡命名；常量命名应使用全大写等等。

项目文件命名：
- 项目名：全部小写，以短横线连接，例如：sgs-map-site
- 目录名：全部小写，以短横线连接，例如：sgs-map-site
- 图片文件名：全部采用小写方式，优先选择单个单词命名，多个单词命名以下划线分隔，如：nav_logo.png
- vue组件名：单文件组件名应该始终是单词大写开头 (PascalCase)，如：MyComponent.vue

文件内命名：
- 变量命名
- 常量命名
- 方法命名
- css命名
### 项目框架结构
```bash
front-end/
|- node_modules   // 下载的依赖包
|- public         // 静态页面目录
    |- static     //静态依赖及配置文件
        |- app-config.js    // 全局配置文件
    |- index.html // 项目入口
|- src            // 源码目录
    |- api        // http 请求目录
    |- assets     // 静态资源目录，这里的资源会被wabpack构建
        |- icon   // icon 存放目录
        |- scss   // 公共样式 scss 存放目录
            |- frame.scss   // 入口文件
            |- global.scss  // 公共样式
            |- reset.scss   // 重置样式
    |- components     // 组件
    |- plugins        // 插件
    |- router         // 路由
    |- routes         // 详细的路由拆分目录（可选）
        |- index.js
    |- store          // 全局状态管理
    |- utils          // 工具存放目录
        |- request.js // 公共请求工具
    |- views          // 页面存放目录
    |- App.vue        // 根组件
    |- main.js        // 入口文件
|- tests          // 测试用例
|- .editorconfig  // 编辑器配置文件
|- .eslintignore  // eslint 忽略规则
|- .prettierc.js  // prettier配置文件
|- .eslintrc.js   // eslint 规则
|- .gitignore     // git 忽略规则
|- jest.config.js
|- package-lock.json
|- package.json // 依赖及配置
|- README.md // 项目 README
|- vue.config.js // webpack 配置
```

### 文档注释

文档注释比较简单，例如单行注释使用 //，多行注释使用 /**/。

```js
/**
 * 
 * @param {number} a 
 * @param {number} b 
 * @return {number}
 */
function add(a, b) {
    return a + b
}

// 单行注释
const active = true
```
更加详细的文档注释可参考[JSDoc](https://jsdoc.app/index.html)

### 代码规范

- vue
- html
- css/less/scss
- javascript/typescript

如果要让团队从头开始制订一份代码规范，工作量会非常大，也不现实。所以强烈建议找一份比较好的开源代码规范，在此基础上结合团队的需求作个性化修改。

一些比较出名的 JavaScript 代码规范:

- [airbnb (101k star](https://github.com/airbnb/javascript)
- [airbnb-中文版](https://github.com/lin-123/javascript)
- [standard  中文版](https://github.com/standard/standard/blob/master/docs/README-zhcn.md)

CSS 代码规范也有不少，例如：

- [styleguide 2.3k star](https://github.com/fex-team/styleguide/blob/master/css.md)
- [spec 3.9k star](https://github.com/ecomfe/spec/blob/master/css-style-guide.md)

vue官方规范：

- [vue风格指南](https://cn.vuejs.org/v2/style-guide/index.html)


大厂规范：

- [腾讯文档库](https://tgideas.qq.com/doc/index.html)
- [京东凹凸实验室前端编码规范](https://guide.aotu.io/index.html)
- [网易编码规范](http://nec.netease.com/standard)
- [百度前端编码规范 3.9k star](https://github.com/ecomfe/spec)

### 提交规范

多人协作的项目中，在提交代码这个环节，也存在一种情况：不能保证每个人对提交信息的准确描述，因此会出现提交信息紊乱、风格不一致的情况。

[Angular 团队提交规范](https://github.com/angular/angular.js/blob/master/DEVELOPERS.md#-git-commit-guidelines)：

```bash
<type>(<scope>): <subject>
<BLANK LINE>
<body>
<BLANK LINE>
<footer>
```


我们可以发现，commit message 分为三个部分(使用空行分割):

1. 标题行（subject）: 必填, 描述主要修改类型和内容。

2. 主题内容（body）: 描述为什么修改, 做了什么样的修改, 以及开发的思路等等。

3. 页脚注释（footer）: 可以写注释，放 BUG 号的链接。

**type**

- feat: 新功能、新特性
- fix: 修改 bug
- perf: 更改代码，以提高性能（在不影响代码内部行为的前提下，对程序性能进行优化）
- refactor: 代码重构（重构，在不影响代码内部行为、功能下的代码修改）
- docs: 文档修改
- style: 代码格式修改, 注意不是 css 修改（例如分号修改）
- test: 测试用例新增、修改
- build: 影响项目构建或依赖项修改
- revert: 恢复上一次提交
- ci: 持续集成相关文件修改
- chore: 其他修改（不在上述类型中的修改）
- release: 发布新版本

**scope**

commit message 影响的功能或文件范围, 比如: route, component, utils, build...

**subject**

commit message 的概述

**body**

具体修改内容, 可以分为多行.

**footer**

一些备注, 通常是 BREAKING CHANGE 或修复的 bug 的链接.
### 规范约束

使用[`editconfig`](https://editorconfig.org/)+[`eslint`](https://eslint.org/)+[`prettier`](https://prettier.io/)实现,在编码中强制开发者使用统一的代码规范:

- EditorConfig 将负责统一各种编辑器的配置，所有和编辑器相关的配置 (行尾、缩进样式、缩进距离...) 都交给它
- Prettier 作为 代码格式化 工具
- 其余的，也就是 代码质量 方面的语法检查，用 ESLint 来做
- 还可以加入stylelint,加强css规范

#### 编辑器配置

- eslint支持：
  - IDEA:默认带eslint的提示
  - VSCODE:需安装插件`eslint`,并在插件设置中启用eslint

- prettier支持：
  - IDEA：最新版本需要安装插件prettier:setting/Plugins下下载安装，然后在settting/Languages & Frameworks /JavaScript/prettier下配置使用
  - VSCODE：安装插件prettier，设置其为默认格式化工具，使用`shift`+`alt`+`f`即可格式化，或者按下保存的时候，会自动lint并执行prettier
- editconfig支持：
    - vscode：需要安装插件EditorConfig for VS Code，编辑或保存时自动执行
    - jetbrains系列可直接使用
- stylelint:
    - vscode需要安装stylelint
    - WebStorm：版本2016.3以后内置了对 stylelint 的支持。
    - SublimeLinter-stylelint：用于 stylelint 的 Sublime Text 插件。


#### prettier集成 

1.安装 Prettier
```bash
npm install --save-dev --save-exact prettier
```
2.Prettier 支持多种格式的配置文件，比如 .json、.yml、.yaml、.js等。
在本项目根目录下创建 .prettierrc 文件,后面可以写入格式化，一般不手动修改大量的规则

3.Prettier 安装且配置好之后，就能使用命令来格式化代码

```bash
# 格式化所有文件（. 表示所有文件）
npx prettier --write .
```
> 一般通过编辑器插件格式化，而不是每次需要使用命令

#### eslint集成

安装
```bash
npm i eslint -D
# 引导初始化,下载eslint检测的相关必要依赖和生产配置文静，里面要选择流行的风格指南，并选择airbnb
npx eslint --init
# 如果依赖下载失败就手动安装
npm i eslint-plugin-vue@latest eslint@7.29.0 eslint-config-airbnb-base@latest eslint-plugin-import@latest -D
```

**eslint中plugins，rules，extends的关系**

1. 可以通过rules配置任何想要的规则，它会覆盖你在拓展或插件中引入的配置项。
2. 添加的 plugins 可以拓展很多特定的规则，默认是不开启的，我们需要在 rules 中选择我们要使用的规则。也就是说 plugins 是要和 rules 结合使用的。
3. extends 可以理解为一份配置好的 plugin 和 rules。通过这种方式，就无需添加大量的规则来启用插件。

跟目录配置文件说明(会覆盖浏览器插件的默认配置)：

- `.prettierrc`:prettier的配置文件
- `.eslintrc.js`:eslint的配置文件
- `.editconfig`:editconfig的配置文件

> 配置文件不止一种文件格式，并且具有不同优先级，详细可查看官网，一般选择一种就好了。

##### 解决eslint和prettier冲突
安装相关的依赖:
```bash
# 用于关闭eslint中与prettier冲突的规则
npm install --save-dev eslint-config-prettier

# 添加prettier的规则到eslint
npm install --save-dev eslint-plugin-prettier

```

**eslint与prettier**

prettier负责格式化代码风格，eslint负责检查代码错误，也会检查代码风格，并且两者的风格也存在差异。
开启eslint校验时，可能会因为prettier格式化之后eslint认为有些格式化的风格与自己定义的不一致而报错。但我们不能因为这样的报错去手动改回eslint认可的代码风格，即使这样，在下一次对这个文件新增了一些代码再次进行格式化时，那些报错又会出现。
最不好的解决方式就是直接在rules中重写这个冲突的规则，前面说到rules会覆盖extends和plugins中的规则，因为我们无法确定有多少个规则会冲突，并且如果还要对ts和jsx等语法支持，还要额外的扩展，可能又会诞生新的冲突，最终ESLint 和 Prettier 开始同时负责代码格式化了，这违背了我们的分工策略。普遍友好的方式是通过在 extends 数组中增加扩展来禁用eslint相关代码格式化 的规则，然后通过plugins配置prettier的插件规则给eslint。
首先可以通过`eslint-config-prettier` 配置extends选项，用于关闭eslint中与prettier冲突的规则。
```js
{
  "extends": [
    "eslint:recommended",//推荐规范
    "airbnb-base",//选择的流行风格
    "prettier",//eslint-config-可以简写去掉。关闭与前面配置的冲突选项。
  ]
}
```

当我们忘记做格式化就保存或提交了代码并且开启了提交校验，eslint会执行`eslint    --fix`, 此时我们已经失去了那些被冲突的规则，当实际代码相关风格不规范，也无法给出我们错误的提示，然后会因此顺利运行以及无阻碍提交了不规范的代码。
我们应该将prettier的规则设置到eslint中，通过`eslint-plugin-prettier`插件配置prettier规则到eslint中，在执行`eslint --fix`就相当于用prettier格式化监测了一遍，就不用担心忘记格式化这件事。
```js
{
  "plugins": ["prettier"],//省略eslint-plugin-前缀
  "rules": {
    "prettier/prettier": "error",//启用规则:将prettier规则的错误作为eslint的错误抛出
  }
}
```
> 这是普遍的方案，看起来plugins里所设置的规则并不能解决掉会与eslint冲突的规则，而是还得需在extends中去关闭。

#### 集成Stylelint
- stylelint:检验工具
- stylelint-order：css样式书写顺序插件
- stylelint-config-standard:stylelint的推荐配置
- stylelint-config-prettier：关闭所有不必要的或可能与 Prettier 冲突的规则


```bash
npm install stylelint@13.13.1 stylelint-order@5.0.0 stylelint-config-standard@ 22.0.0 stylelint-config-prettier@9.0.3 stylelint-scss@3.21.0 -D
```
> 注意插件之间的依赖版本，如果全都指定最新的可能相互不兼容，stylelint会非正常报错.vscode的插件版本最好也要指定1以下的版本

创建stylelint.config.js:
```js
module.exports = {
  root: true,
  plugins: ["stylelint-scss", "stylelint-order"],
  //https://github.com/stylelint/stylelint/blob/main/docs/user-guide/rules/list.md
  extends: ["stylelint-config-standard", "stylelint-config-prettier"],
};

```
创建忽略文件`.stylelintignore`

```bash
/dist/*
/public/*
public/*
src/assets/font/*
```
更改`package.json`

```json
{
    "script":{
        "lint:stylelint": "stylelint --cache --fix \"**/*.{vue,less,postcss,css,scss}\" --cache --cache-location node_modules/.cache/stylelint/"
    }
}
```

#### 集成 EditorConfig 配置



在项目根目录下增加 .editorconfig 文件

```bash
root = true

[*] # 表示所有文件适用
charset = utf-8 # 设置文件字符集为 utf-8
indent_style = space # 缩进风格（tab | space）
indent_size = 2 # 缩进大小
end_of_line = lf # 控制换行类型(lf | cr | crlf)
trim_trailing_whitespace = true # 去除行首的任意空白字符
insert_final_newline = true # 始终在文件末尾插入一个新行

[*.md] # 表示仅 md 文件适用以下规则
max_line_length = off
trim_trailing_whitespace = false
```

**editconfig与prettier**
 
- editconfig中配置的一些代码风格，会默认为编辑器的风格，比如缩进，在按下tab键的时候就按照editconfig的规定来了。
- 而prettier设置代码风格规定也包含了缩进，但是需要我们执行格式化命令才会按照prettier的风格走
- 如果你手动在prettier的配置文件中修改了缩进并且与editconfig缩进设置的不一样，结果是我们默认写代码的时候按照editconfig的风格来，然后格式化代码时又按照prettier来，但是在下一次保存代码的时候之前格式化后的缩进就又会变回editconfig的缩进，这样反复横跳不是我们所希望的。
- 在新的prettier版本中，它在执行格式化的时候会默认先找editconfig中的配置规则然后覆盖自己的规则（限定部分重复的规则)
- 对于prettier会被editconfig覆盖的规则("endOfLine","useTabs","tabWidth","printWidth")，不要在prettier配置文件中重复写一份。
- 对于prettier不会被editconfig覆盖的规则，并且editconfig设置的与prettier默认的不一致，就需要在prettier的配置文件中设置与editconfig一样避免风格反复横跳。

### 新项目搭建中集成

> 通过新的vue脚手架搭建的项目中选择的eslint+prettier配置，主要利用了Vue自己封装的一个插件@vue/eslint-config-prettier，它其实还是利用了eslint-plugin-prettier和eslint-config-prettier这两个插件，将之整合到一起，放到了node_modules/目录中（加了一层作用域），因此我们在Vue脚手架生成的项目经常能看到@vue/prettier这个扩展。本质还是一样的。

### 在vue-cli2中集成

安装prettier
```bash
# 固定版本号
npm install --save-dev --save-exact prettier
```
然后，创建一个配置文件，让编辑器和其他工具知道您正在使用 Prettier
```bash
echo {}> .prettierrc
```
配置`.prettierrc`
```json
{
  "singleQuote": false
}
```
还可以创建一个`.prettierignore`文件，让 Prettier CLI 和编辑器知道哪些文件不能格式化。这是一个例子
```bash
# Ignore artifacts:
build
coverage
```
添加editconfig配置文件`.editorConfig`:

```bash
# EditorConfig helps developers define and maintain consistent
# coding styles between different editors and IDEs
# editorconfig.org
root = true
[*]
charset = utf-8
indent_style = space
indent_size = 2
end_of_line = lf
insert_final_newline = true
trim_trailing_whitespace = true
[*.md]
insert_final_newline = false
trim_trailing_whitespace = false
```

eslint及相关依赖安装
```bash
npm i babel-eslint eslint eslint-friendly-formatter eslint-loader eslint-plugin-vue --save-dev
```

解决eslint与prettier冲突依赖
```bash
npm install eslint-config-prettier eslint-plugin-prettier --save-dev
```

添加`.eslintrc.js`配置文件
```js
module.exports = {
  root: true,
  parserOptions: {
    parser: "babel-eslint", //在处理非 ECMAScript 5 特性时正常工作，解析器会被传入 parserOptions
    sourceType: "module",
  },
  env: {
    browser: true,
    es6: true, //版本4.x支持配置项es6，版本6.x支持到es2020，版本7.x支持es2021
    node: true,
  },
  globals: {
    EventBus: "writable",
    GeoESB: "readonly",
    APPCONFIG: "writable",
  }, //需要配置的自定义的全局变量，EventBus等
  // https://github.com/vuejs/eslint-plugin-vue#priority-a-essential-error-prevention
  // consider switching to `plugin:vue/strongly-recommended` or `plugin:vue/recommended` for stricter rules.
  extends: ["plugin:vue/recommended", "eslint:recommended", "prettier"], //加入vue和eslint的推荐规则，eslint-config-prettier放置在最后，可以关闭前面与prettier冲突的规则
  plugins: ["vue", "prettier"],//eslint-plugin-prettier规则也加入到eslint
  // add your custom rules here
  rules: {
    /*
    指定为 error 的 Prettier 新规则，这样任何格式化错误就也被认为是 ESLint 错误了
    eslint --fix 命令时，ESLint 就会按照 Prettier 的配置规则来格式化代码，轻松解决二者冲突问题。
    */
    "prettier/prettier": "error",
  },
};
``` 

eslint忽略文件`.eslintignore`配置：
```bash
build/*.js
dist/*
static/*
node_modules/*
```

修改config/index.js文件
```js
//dev属性下添加两个属性
useEslint: true,
showEslintErrorsInOverlay: false,
```

修改build/webpack.base.conf.js文件
```js
//添加eslint-loader配置
const createLintingRule = () => ({
  test: /\.(js|vue)$/,
  loader: 'eslint-loader',
  enforce: 'pre',
  include: [resolve('src'), resolve('test')],
  options: {
    formatter: require('eslint-friendly-formatter'),
    // 如果选项设置为 true，将始终返回警告。
    //如果您正在使用热模块替换，您可能希望在开发中启用此功能，否则当出现 eslint 错误时将跳过更新。
    emitWarning: !config.dev.showEslintErrorsInOverlay,
    failOnError: false,
    failOnWarning: false,//默认错误和警告不会中断编译
  }
})
//将该配置加入到rules数组的首项
module.exports = {
    module：{
        rules：[
        ...(config.dev.useEslint ? [createLintingRule()] : []),
        ...
        ]
    }
}

```
给package.json中增加一个脚本命令
```json
{
    "scripts":{
        "lint": "eslint --fix --ext .js,.vue src"
    }
}
```

跟目录下添加`.vscode/settings.json`

```json
{
  "eslint.validate": ["javascript", "javascriptreact", "vue"],
  "editor.formatOnSave": false,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.formatOnType": false, //避免保存时使用编辑器自己的格式化
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true, //保存时自动fix
    "source.fixAll.stylelint": true //保存时自动fix
  },
  "vetur.validation.template": false,
  "[vue]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[javascript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "eslint.alwaysShowStatus": true,
  "stylelint.validate": [
      "css",
      "less",
      "postcss",
      "scss",
      "vue",
      "sass"
  ],
}

```
> .gitignore中 把 .vscode 给去掉


快速集成husky和lint-staged,如果是git项目，可额外做提交验证
```bash
npx mrm lint-staged
```
该命令做了一下几种事：
1. 生成和安装`husky`和`lint-staged`的依赖
2. 修改 package.json，增加lint-staged的配置，内部执行eslint检测
3. 修改 package.json 的 scripts，增加 "prepare": "husky install"
4. 如果初始化好了git仓库，会生成`.husky`配置文件还会初始化一个`pre-commit`钩子文件,内部执行了lint-staged的命令语句
    

> 当我们执行 `git commit -m "xxx"` 之前时，会对git暂存区所有的 `.vue`、`.js`、`.ts `文件执行 `eslint --fix` 命令，如果 ESLint 通过，则成功提交到本地仓库，否则终止提交。

集成commitlint

```bash
# 安装commitlint和文件初始化
npm i @commitlint/config-conventional @commitlint/cli -D

# 生成`commitlint.config.js`配置文件,如果格式不是 utf8 格式的，需要将文件转成 utf8 格式，或者手动生成，不然提交会报错
echo "module.exports = { extends: ["@commitlint/config-conventional"] };" > commitlint.config.js

# .husky 目录下创建 commit-msg 文件，并在此执行 commit message 的验证命令。
npx husky add .husky/commit-msg "npx --no-install commitlint --edit $1"
```
> commitlint检测规则有很多类，最常用的是上面采用的[Conventional Commits specification](https://www.conventionalcommits.org/en/v1.0.0/)。此规则是根据上文所说的Angular Team Commit Specification衍生出来的

> 通过pre-commit的eslint验证后，又会在提交的commit-msg阶段通过commitlint验证提交信息的格式，如果通过成功 `commit`，否则终止 `commit`。

> 如果没有安装依赖的情况下验证并不会生效可以直接通过


### 提交验证



为了保证代码的风格按照eslint书写及提交格式的规范化，我们需要用到 Git Hook，在本地执行 `git commit` 的时候，就对所提交的代码进行 ESLint 检测和修复（即执行 `eslint --fix`），如果这些代码没通过 ESLint 规则校验，则禁止提交。另外如果没通过`commitlint`验证，也禁止提交。



实现这一功能，我们借助 [husky](https://github.com/typicode/husky) + [lint-staged](https://github.com/okonet/lint-staged)+[commitlint](https://commitlint.js.org/) 。



> [husky](https://github.com/typicode/husky) —— Git Hook 工具，可以设置在 git 各个阶段（`pre-commit`、`commit-msg`、`pre-push` 等）触发我们的命令。

> [lint-staged](https://github.com/okonet/lint-staged) —— 在 git 暂存的文件上运行 linters。

> [commitlint](https://commitlint.js.org/) 搭配 `husky` 的 commit message 钩子后，每次提交 git 版本信息的时候，会根据配置的规则进行校验，若不符合规则会 commit 失败，并提示相应信息。



先删除脚手架自带的git-hook配置:package.json，后面交给husky来管理hook。

```bash
  "gitHooks": {
    "pre-commit": "lint-staged"
  },
```



安装:

```bash
# 初始化和安装husky
npx husky-init
npm install

#脚手架创建的时候以安装
npm i lint-staged -D
# 安装commitlint和文件初始化
npm i @commitlint/config-conventional @commitlint/cli -D
# 如果生成的 commitlint.config.js 文件不是 utf8 格式的，需要将文件转成 utf8 格式，或者手动生成，不然提交会报错
echo "module.exports = {extends: ['@commitlint/config-conventional']}" > commitlint.config.js
# 使用 husky 的 commit-msg hook 触发验证提交信息的命令
# .husky 目录下创建 commit-msg 文件，并在此执行 commit message 的验证命令。
# 这里执行报错，可能win10不支持后面的语句空格隔开,可以随意输入一个字符串创建好后再改。不要手动创建文件，不然可能提交会识别不到文件
npx husky add .husky/commit-msg "npx --no-install commitlint --edit $1"
```



> commitlint检测规则有很多类，最常用的是上面采用的[Conventional Commits specification](https://www.conventionalcommits.org/en/v1.0.0/)。此规则是根据上文所说的Angular Team Commit Specification衍生出来的。commitlint更多规则可看：[Shared configuration](https://github.com/conventional-changelog/commitlint#shared-configuration)。



修改package.json:

```bash
"lint-staged": {
 "*.{vue,js,ts}": "eslint --fix"
},
```
> 可以再lint-staged的配置文件`.lintstagedrc.json`中配置，统一对代码格式，风格，样式等各种文件提交校验
```json
{
  "*.{js,jsx,ts,tsx}": ["eslint  --fix", "prettier --write"],
  "*.md": ["prettier --write"],
  "*.{scss,less,styl,html}": ["stylelint --fix", "prettier --write"],
  "*.vue": ["eslint --fix", "prettier --write", "stylelint --fix"]
}
```



修改 `.husky/pre-commit` hook 文件的触发命令：

```bash
*#!/bin/sh*
. "$(dirname "$0")/_/husky.sh"

npx lint-staged
```



提交前，给项目指定一个远程仓库,这里使用[gitee](https://gitee.com/rhyme97/front-end-engineer.git)

```bash
# 修改脚手架项目的仓库地址
git remote add origin https://gitee.com/rhyme97/front-end-engineer.git
```



> 当我们执行 `git commit -m "xxx"` 之前时，会对git 暂存区的 .vue、.js、.ts 文件执行 eslint --fix，如果 ESLint 通过，，成功提交至本地仓库，否则终止提交。这验证commitlint格式，如果通过成功 `commit`，否则终止 `commit`。
>如果没有安装依赖的情况下验证并不会生效可以直接通过

### 提交日志

```bash
npm install -g conventional-changelog-cli
```

然后`package.json`更新script,通过脚本去更新日志文档:
```json
{
  "scripts": {
    "changelog": "conventional-changelog -p angular -i CHANGELOG.md -s" 
  }
}
```
在我们每次changelog之前，都必须要使用npm version升级版本，否则，commit一直都会有之前的记录。

> 如果是作为公共库，那么其他项目在引入是，对应版本号应该怎么设置，各个版本好会造成怎么的影响。

#### npm verion的版本升级管理

每个npm包中都有一个package.json文件，如果要发包的话，package.json中的version就是版本号了。
version字段结构为：'0.0.0-0'
分别代表：大号.中号.小号-预发布号，对应majon.minor.patch-prerelease
下面来看看npm中version的类别及描述。


- major
    - 如果没有预发布号，则直接升级一位大号，其他位都置为0
    - 如果有预发布号：中号和小号都为0，则不升级大号，而将预发布号删掉。即2.0.0-1变成2.0.0，这就是预发布的作用;如果中号和小号有任意一个不是0，那边会升级一位大号，其他位都置为0，清空预发布号。即 2.0.1-0变成3.0.0
- minor
    - 如果没有预发布号，则升级一位中号，大号不动，小号置为空
    - 如果有预发布号：如果小号为0，则不升级中号，将预发布号去掉;如果小号不为0，同理没有预发布号
- patch
    - 如果没有预发布号：直接升级小号，去掉预发布号
    - 如果有预发布号：去掉预发布号，其他不动
- premajor
    - 直接升级大号，中号和小号置为0，增加预发布号为0
- preminor
    - 直接升级中号，小号置为0，增加预发布号为0
- prepatch
    -直接升级小号，增加预发布号为0
- prerelease
    - 如果没有预发布号：增加小号，增加预发布号为0
    - 如果有预发布号，则升级预发布号

### 自动生成项目目录树

```bash
 npm install mddir -D
```
`package.json`增加一条script:
```json
{
    "scripts":{
        "mddir":"node node_modules/mddir/src/mddir"
    }
}
```
然后执行`npm run mddir`，会在根目录下生成一个`directoryList.md`目录文件

### 测试

测试的作用是为了提高代码质量和可维护性。

- 提高代码质量：测试就是找 BUG，找出 BUG，然后解决它。BUG 少了，代码质量自然就高了。

- 可维护性：对现有代码进行修改、新增功能从而造成的成本越低，可维护性就越高。



如果你的程序有成千上万行代码，数十个模块，模块与模块之间的交互错综复杂。在这种情况下，就需要写测试了。试想一下，在你对一个非常复杂的项目进行修改后，如果没有测试会是什么情况？你需要将跟这次修改有关的每个功能都手动测一边，以防止有 BUG 出现。但如果你写了测试，只需执行一条命令就能知道结果，省时省力。



#### 单元测试

从前端角度来看，单元测试就是对一个函数、一个组件、一个类做的测试，它针对的粒度比较小

我们使用 Vue 官方提供的 vue-test-utils 和社区流行的测试工具 jest 来进行 Vue 组件的单元测试。

具体试编写文档如下：

- [Globals · Jest (jestjs.io)](https://jestjs.io/docs/api#describename-fn)
- [Installation | Vue Test Utils (vuejs.org)](https://vue-test-utils.vuejs.org/installation/)


安装核心依赖:

```bash
npm i -D jest
```

打开 package.json 文件，在 scripts 下添加测试命令：

```json
"scripts": {
    "test": "jest",
 }
```
然后在项目根目录下新建 test 目录，作为测试目录。

```js
// main.js
function abs(a) {
    if (typeof a != 'number') {
        throw new TypeError('参数必须为数值型')
    }

    if (a < 0) return -a
    return a
}

// test.spec.js
test('abs', () => {
    expect(abs(1)).toBe(1)
    expect(abs(0)).toBe(0)
    expect(abs(-1)).toBe(1)
    expect(() => abs('abc')).toThrow(TypeError) // 类型错误
})
```
现在我们需要测试一下 abs() 函数：在 src 目录新建一个 main.js 文件，在 test 目录新建一个 test.spec.js 文件。然后将上面的两个函数代码写入对应的文件，执行 npm run test，就可以看到测试效果了。
```bash
> jest

PASS test/test.spec.js
abs(6 ms)

Test Suites: 1 passed, 1 total
Tests:       1 passed, 1 total
Snapshots:   0 total
Time：       2.014 s
Ran all test suites.
```



#### E2E 测试



环境在脚手架搭建的时候以集成,并且使用cypress框架进行测试。

端到端测试，主要是模拟用户对页面进行一系列操作并验证其是否符合预期。

依赖安装：
```bash
npm i -D cypress
```
打开 package.json 文件，在 scripts 新增一条命令：
```bash
"cypress": "cypress open"
```

然后执行 npm run cypress 就可以打开 Cypress。首次打开会自动创建 Cypress 提供的默认测试脚本。


点击窗口的`Run all specs`即可执行所有测试用例.然后会重新运行一个测试项目页面，同时查看测试情况。

具体使用可参考：[Why Cypress? | Cypress Documentation](https://docs.cypress.io/guides/overview/why-cypress#In-a-nutshell)

> 通常情况下，作为前端很少去考虑写测试代码这一块，并且从项目出发，需要考量一个项目的体量，开发成本，维护成本等去觉得是否去做这些事情，或者如果取做好关键性的测试代码。从程序维护的健壮性来讲，测试是作为一种保障，必要情况下也不失为一种重要手段。



#### 单元测试提交约束

执行以下命令，如果命令不生效，手动创建文件及写入内容：

```bash
npx husky add .husky/pre-push "npm run test:unit $1"
```

现在，只有单元测试全部通过，才能成功 `push`。

## 代码工作流

在 SVN 时代大家会使用集中式工作流，所有人都往一个主库分支合入代码；随着技术的演进，以 Git 为代表的分布式代码管理工具横空出世，在 Git 的基础上又逐渐出现了多种代码管理工作流：功能分支工作流，Gitflow 工作流，Forking 工作流

在版本管理里，分支是很常使用的功能。在发布版本前，需要发布分支，进行大需求开发，需要 feature 分支，大团队还会有开发分支，稳定分支等。在大团队开发过程中，常常存在创建分支，切换分支的需求。

Git 分支是指针指向某次提交，而 SVN 分支是拷贝的目录。这个特性使 Git 的分支切换非常迅速，且创建成本非常低。

而且 Git 有本地分支，SVN 无本地分支。在实际开发过程中，经常会遇到有些代码没写完，但是需紧急处理其他问题，若我们使用 Git，便可以创建本地分支存储没写完的代码，待问题处理完后，再回到本地分支继续完成代码。
svn:
```
graph TD
Trunk-->developer1
Trunk-->developer2
Trunk-->developer3
```

git:
```
graph TD
Git远程仓库-->developer1的Git本地仓库
developer1的Git本地仓库-->developer1
Git远程仓库-->developer2的Git本地仓库
developer2的Git本地仓库-->developer2
Git远程仓库-->developer3的Git本地仓库
developer3的Git本地仓库-->developer3
```


## 性能优化



性能优化主要分为两类：

1. 加载时优化

2. 运行时优化

例如压缩文件、使用 CDN 就属于加载时优化；减少 DOM 操作，使用事件委托属于运行时优化。

在解决问题之前，必须先找出问题，否则无从下手。所以在做性能优化之前，最好先调查一下网站的加载性能和运行性能。



### 手动检查



#### 检查加载性能

一个网站加载性能如何主要看白屏时间和首屏时间：

- 白屏时间：指从输入网址，到页面开始显示内容的时间。

- 首屏时间：指从输入网址，到页面完全渲染的时间。



将以下脚本放在 `</head>` 前面就能获取白屏时间:

```html
<script>
 new Date() - performance.timing.navigationStart
</script>
```

在 `window.onload` 事件里执行 `new Date() - performance.timing.navigationStart` 即可获取首屏时间。



#### 检查运行性能



配合 chrome 的开发者工具，我们可以查看网站在运行时的性能。

打开网站，按 F12 选择 performance，点击左上角的灰色圆点，变成红色就代表开始记录了。这时可以模仿用户使用网站，在使用完毕后，点击 stop，然后你就能看到网站运行期间的性能报告。如果有红色的块，代表有掉帧的情况；如果是绿色，则代表帧率高，页面很流畅。

另外，在 performance 标签下，按 ESC 会弹出来一个小框。点击小框左边的三个点，把 rendering 勾出来。



### 利用工具检查

**chrome 工具 Lighthouse**

它不仅会对你网站的性能打分，还会对 SEO 打分。



### 加载时性能优化 

1. 减少 HTTP 请求

2. 使用HTTP2

3. 使用服务端渲染

4. 静态资源使用 CDN

5. 将 CSS 放在文件头部，JavaScript 文件放在底部

6. 使用字体图标 iconfont 代替图片图标

7. 善用缓存，不重复加载相同的资源

8. 压缩文件

9. 图片优化

10. 通过 webpack 按需加载代码，提取第三库代码，减少 ES6 转为 ES5 的冗余代码



### 运行时优化 

1. 减少重绘重排

2. 使用事件委托

3. 注意程序的局部性

4. 倾向于使用 switch 而不是 if-else

5. 使用位操作

6. 不要覆盖原生方法

7. 降低 CSS 选择器的复杂性

8. 使用 flexbox 而不是较早的布局模型

9. 使用 transform 和 opacity 属性更改来实现动画



## 自动化部署



部署是指将代码发布到服务器的一种行为。自动化部署就是使用工具来帮助你实现部署的过程，无需亲自动手。

### 触发方式

自动部署（又叫持续部署 Continuous Deployment，英文缩写 CD）一般有两种触发方式：

1. 定时触发。

2. 监听 `webhook` 事件，事件触发时执行自动打包、部署等操作。

> 定时触发，就是构建软件每隔一段时间自动执行打包、部署操作。
> 这种方式不太好，很有可能软件刚部署完开发就改代码了。为了看到新的页面效果，不得不等到下一次构建开始。另外还有一个副作用，假如开发一天都没更改代码，构建软件还是会不停的执行打包、部署操作，白白的浪费资源。
>
> 所以现在基本都是采用监听 `webhook` 事件的方式来进行部署。



### 使用[jenkins](https://pkg.jenkins.io/)+gitee实现自动化



主要通过以下两个过程实现：

- [在centos7安装jenkins](https://www.cnblogs.com/xiaobug/p/13932634.html)

- [使用jenkins实现自动化部署](https://juejin.cn/post/6844903591417757710#heading-1)

> jenkins在云服务器部署好后，就可以通过网址访问：http://www.xxx.cn:9099/



安装上述配置好后，还需要将打包后的文件放入自己的服务器。这里暂时用node在我的云服务器搭建一个静态服务器:

```bash
*# 进入到目标路径*
cd /usr/node_serve/node_project/jenkins
*# 初始化项目*
npm init -y
npm i express
```

然后建立一个server.js

```js
const express = require('express')
const app = express()
const port = 9097
app.use(express.static('dist'))
app.listen(port, () => {
  console.log(`running`)
})
```

启动这个静态服务器:

```bash
pm2 start server.js
```



**下面是我的构建命令**

```bash
node --version
npm config set registry http://registry.npmjs.org
*# 先删除该文件不然node-sass下载不成功*
rm -rf package-lock.json
npm install
npm run test:unit
rm -rf dist
npm run build
cd dist
tar -zcvf dist.tar.gz *
```



**构建后命令**

```bash
cd /usr/node_serve/node_project/jenkins
tar -zxvf dist.tar.gz -C /usr/node_serve/node_project/jenkins/dist
rm -rf dist.tar.gz
```



本地每次push代码到gitee时，都会在jenkins上自动构建打包，然后通过ssh连接发送至服务器配置目录并执行解压缩命令，这样在服务器就可以直接访问了。

> 我的访问地址：[frontend-engineer-demo (rhymin.cn)](http://www.xxx.cn:9097/#/)

## 前端组件化

为什么要实现组件化：
    一种基本诉求的升级：复用。一方面提升开发效率，另一方面降低维护成本。

组件面对的对象：
- 控件
- 基础逻辑功能
- 公共样式
- 稳定的业务逻辑

组件化的设计原则：
- 标准性
- 单一职责
- 复用与易用
- 避免暴露组件内部实现
- 稳定性
    
组件的运用与管理：

在前端领域并没有统一的组件化模式。我们希望的是可以便于多个开发者维护，以及直接使用

可以通过[bit](https://docs.bit.dev/)作为组件管理方案：
Bit 是一个开源的 Cli 工具，用于跨项目和资源库协作处理孤立的组件。
使用 Bit 将设计库或项目中的离散组件分发到一个独立的可重用包中，并在不同的应用程序中利用它。
您可以为组件协作设置自己的服务器，或者使用 bit.dev 云主机进行私有和公共组件共享。

Bit 允许团队
增加代码的可重用性
提高设计和开发效率
保持 UI 和 UX 的一致性
提高项目的稳定性

主要特性
直接从现有的库或项目中提取一个组件进行共享。
通过从项目的其他部分单独构建和测试每个组件来验证组件的独立性。
从任何利用它的应用程序中更改共享组件的源代码。
在本地修改的基础上获得组件中发布的修改。
直接从消费应用程序中贡献出对组件的修改。
自动将每个组件包装成一个 npm 包。
分发离散的组件，而不是单一的大包。
根据组件依赖关系的变化，自动进行组件版本管理。
与领先的框架和工具一起使用。React, Vue, Angular, Mocha, Jest.
与 Git、NPM 和 Yarn 一起工作。


## code review

代码审查，也是作为保证代码质量中不可或缺的一环。另外，也是促进团队间代码交流、项目功能了解的友好方式。

通常情况下，当团队见协作开发时，各自虽然做着相关的功能，但是开发的确是各自独立的代码块，表面上代码没有任何错误，但是在开发者之间所做的功能业务的衔接，可能会出现隐患。各自都只是大概知道协作人员大概做了什么，但是在设计上都按照各自的想法，对接时更像是临时的拼凑导致后面的程序难以维护。
所以，即使前面有那么多的工具，插件服务我们规范代码，但是这种情况，人力观察才能有效保证程序的健壮性。


## 总结


看起来前端工程化相关的东西很多，太过宽泛，并且使用到了各种各样的工具，各式各样的操作。但是，工具的存在只是为了解放生产力，使用它需要考虑项目的实际情况去使用，如果一个项目集群特别大，流程化与系统化是必不可少的，需要考虑自己的一个工程化体系，这样才能在长久的维护与开发过程中保证项目的健壮性。另外对于开发人员来说，养成工程化项目的思维，必然是一个良性循环的过程。


## 补充

### 使用npm还是yarn

选择一个使用，并且提交其中一个lockfile文件，并将另一个添加到gitignore











