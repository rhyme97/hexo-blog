---
title: 移动H5的一些实践
date: 2020-12-19 15:06:37
tags:
    - 网页适配
categories:
    - 移动H5
descriptions:
---

## 写在前面

初次从电脑端做移动H5时，可能会很不适应：要适应的分辨率变多了，px也不好使了，唯一庆幸的时，手机上没有IE。然后开始思考：如何做适配？然后网上搜索了很多的解决方案，主要就是rem和viewport，还有媒体查询，但是方法并不完美，当我把所有的单位都转化成rem之后，发现手机上有些字体会变得很小，或者手机上正常，但是谷歌浏览器模拟的字体显示不全。

## 调试
1. 手机端的浏览器主要针对webkit内核
2. 可通过谷歌控制台模拟调试
3. 最好真机调试
4. 尝试不同屏幕尺寸

## 视口

- 布局视口：视口980px
- 视觉视口：在网页上显示的
- 理想视口：meta（把布局视口设为跟理想视口一致）

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<!--
width：控制 viewport 的大小，可以指定的一个值，如 600，或者特殊的值，如 device-width 为设备的宽度（单位为缩放为 100% 时的 CSS 的像素）。
- height：和 width 相对应，指定高度。
- initial-scale：初始缩放比例，也即是当页面第一次 load 的时候缩放比例。
- maximum-scale：允许用户缩放到的最大比例。
- minimum-scale：允许用户缩放到的最小比例。
- user-scalable：用户是否可以手动缩放。
-->
```



## 物理像素

1. 手机屏幕最小颗粒，等同分辨率，手机端的px跟物理像素比例不尽相同
2. pc端1px代表一个物理像素

## 物理像素比
1px能显示的物理像素，iphone 8(750*1344)的物理像素比是2
`console.log(window.devicePixelRatio)`

## Retina视网膜屏幕技术

把更多的物理像素压缩至屏幕，提升分辨率和清晰度

## 多倍图

准备的图在手机端会被放大（1px占比物理像素多），预先准备图的尺寸为放大后的尺寸，设置大小的时候压缩显示，这样在手机被放大的时候不会失真

## 移动端开发选择
1. 单独制作移动端页面（判断打开的设备是不是移动端）
    - 流式布局（百分比）,限定最大宽度和最小宽度
    - flex
    - less+rem+媒体查询
    - 混合布局
2. 响应式兼容移动端
    - 媒体查询（引入资源）
    - bootstrap.....

## 移动端技术解决方案

- box-sizing
- normalize.css
- flex
- 清除高亮：`-webkit-tap-highlight-color:transparent`
- 清除ios的按钮外观亮光:`-webkit-apperance:none`
- 禁用长时间按页面弹出菜单：`-webkit-touch-callout:none`
- input边框不同的显示效果:`outline:none;border:none;border:1px solid red;`,清除再设成自己的
- 去除默认边距:`*{marign:0;padding:0}`
- 清除浮动:`clearfix`
- 去除里的项目符号:`ul{list-style:none;}`
- a标签的下划线：`a,a:hover{text-decoration:none;}`
- 默认字体：`body,html{font-family:'微软雅黑';}`
- 最大宽度，最小宽度：`300,640px`
- 字体抗锯齿：`-webkit-font-smoothing: antialiased;`

## touch事件
- :开始触摸`touchstart`
- 触摸移动`touchmove`
- 取消触摸`touchend`
- 意外终止`touchcancel`


## 适配方案

* 假设自己移动端开发页面的设计稿是750*1334,在开发过程中，可以直接写入设计稿的px尺寸样式，然后使用插件自动编译后适配.

### 方案1：postcss-px-to-viewport

> 自动适配设计稿的px到vw(vw单位没有rem兼容高)

配置.postcssrc.js

```js
const path = require('path');

module.exports = (ctx) => {
  //如果使用了vant组件库，该组件库的设计稿是375，需要判断
  const designWidth = ctx.webpack.resourcePath.includes(path.join('node_modules', 'vant')) ? 375 : 750;

  return {
    plugins: {
      autoprefixer: {},
      "postcss-px-to-viewport": {
          unitToConvert: "px", // 要转化的单位
          viewportWidth: designWidth, // UI设计稿的宽度
          unitPrecision: 6, // 转换后的精度，即小数点位数
          propList: ["*"], // 指定转换的css属性的单位，*代表全部css属性的单位都进行转换
          viewportUnit: "vw", // 指定需要转换成的视窗单位，默认vw
          fontViewportUnit: "vw", // 指定字体需要转换成的视窗单位，默认vw
          selectorBlackList: ["wrap"], // 指定不转换为视窗单位的类名，
          minPixelValue: 1, // 默认值1，小于或等于1px则不进行转换
          mediaQuery: true, // 是否在媒体查询的css代码中也进行转换，默认false
          replace: true, // 是否转换后直接更换属性值
          //exclude: [/node_modules/], // 设置忽略文件，用正则做目录名匹配
          landscape: false // 是否处理横屏情况
      }
    }
  }

}
```
### 方案2：postcss-pxtorem+动态设置跟节点字体rem
> 自动适配设计稿的px到rem。默认index.html开始了meta标签的viewport属性

首先rem需要在加载时定义跟节点，使用js自动判断

```js
 mounted(){
    this.autoSetRootFontSize()
    window.addEventListener("resize", ()=> {
        this.autoSetRootFontSize()
    });
  },
  methods:{
    autoSetRootFontSize() {
      console.log(screen.width)
      console.log(document.documentElement.getBoundingClientRect().width)
      document.documentElement.style.fontSize 
      =Math.min(screen.width,document.documentElement.getBoundingClientRect().width)/75+'px'
    },
  }
  ```
上面设置了rem与px的比例为`1rem=10px`。然后配置.postcssrc.js，使用插件自动适配
```js
const path = require('path');

module.exports = (ctx) => {
  //如果使用了vant组件库，该组件库的设计稿是375，需要判断
  const designRootValue = ctx.webpack.resourcePath.includes(path.join('node_modules', 'vant')) ? 5 : 10;

  return {
    plugins: {
      autoprefixer: {},
      "postcss-pxtorem": { //
        rootValue: designRootValue, //结果为：设计稿元素尺寸/10，比如元素宽10px,最终页面会换算成 1rem
        unitPrecision: 5,
        propList: ['*'],
        selectorBlackList: ["keep-px"],
        replace: true,
        mediaQuery: true,
        minPixelValue: 0,
        // exclude: /node_modules/i
      },
    }
  }

}
```
## 如何选择适配

没有一种一定完美的适配，会根据设计图具体调整，需要先考虑界面有哪些交互，会在布局上有怎样的变化，有哪些是可以直接用px写死的，有哪些地方需要伸缩。

比如自己拿到一个类型高德地图移动端的界面的原型图，主要是地图类的交互，界面上展示了一些按钮，放大缩小定位之类的。这个时候不要盲目的选择rem或者vw，如果查看高德地图的布局会发现，它会把一些固定展示项写死，然后地图类的区域自适应就可以了。当然具体的情况只有自己做了才会发现自己的方案存在怎么的问题，然后回想似乎另一种方法是一种更好的选择。

所以在对你的页面做出适配时，预先得全面了解将要做的界面的特性适合什么样的方式，再思考那些布局应该怎么写。完美是不可能完美的，适合和兼容依然在延续。