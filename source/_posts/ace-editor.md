---
title: ace-editor在线源码编辑器的使用
date: 2020-09-29 22:55:09
tags:
 - plugins
categories:
 - javascript
---
## 关于ace-editor
本次学习，主要想使用该库实现一个在线源代码编辑器功能，并且可以在线调试和运行，[类似echarts上的实例功能](https://echarts.apache.org/examples/zh/editor.html?c=line-simple)。

[官网地址](https://ace.c9.io/)

[gitee地址](https://gitee.com/mirrors/ACE-Editor)

[github地址](https://github.com/ajaxorg/ace)

参考博客:
: https://blog.csdn.net/hj7jay/article/details/78410738

## 下载
这里在github上下载相关文件
``` bash
git clone https://github.com/ajaxorg/ace.git
```
## 依赖
下载好github的项目文件，你可以使用requireJS将lib/ace的内容作为ace加载，或者使用ace的预打包版本之一：只需将src子目录自已复制到项目中

生成build版
``` bash
npm install
node ./Makefile.dryice.js
```
在运行以上命令时，可以有一些打包选项
``` bash
-m                 minify build files with uglify-js          
-nc                namespace require and define calls with "ace"
-bm                builds the bookmarklet version
--target ./path    指定输出文件的相对路径 (默认 "./build")
```
> 除此之外，要生产ace-builds存储库中的所有文件，运行`node Makefile.dryice.js full --target ../ace-builds`,所有文件指哪些东西，暂时未了解。。。

此处不执行执行选项，默认执行上面的两行命令，会生成一个build目标文件夹，该文件子目录src包含很多依赖文件，通过它们，我们就可以使用ace-editor的各个功能模块了。

>1.mode打头的文件一般为开发语言支持依赖包，你只需要保留你所要编辑语言依赖包就够了
2.theme打头是编辑器皮肤文件，这个东西一般情况一个就够了
3.相关依赖包的加载都是ace内部定义的require加载动态加载的，当调用了相关功能，浏览器会发请求加载相关依赖包，所以慎重选择你需要剔除的依赖包。真删了也不用着急，找到相应的包文件加回去就可以了

## 引入
创建一个html模板，然后引入ace的核心依赖文件
``` 
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ace-editor-demo</title>
    <style>
        /* 该编辑器必须设置大小并且绝对或相对放置，以便ace其作用 */
        #host {
            display: flex;
        }
        .wrap {
            width:500px;
            height: 600px;
        }
        .toorbar {
            box-shadow: 0 2px 10px #dddddd;
            padding: 5px;
            text-align: right;
            position: relative;
        }
        .toorbar span {
            position: absolute;
            left: 20px;
            top: 50%;
            transform: translateY(-50%);
            font-size: 18px;
            line-height:18px;
        }
        #editor {
            position: relative;
            top: 0;
            left: 0;
            width:500px;
            height: 600px;
        }

        #preview {
           flex: 1;
           height: 633px;
           border: none;
           outline: 1px solid orange;
        }
    </style>
</head>

<body>
    <!-- 将editor作为DOM元素的ID，使之转换为编辑器 -->
    <div id="host">
        <div class="wrap">
            <div class="toorbar">
                <span>源代码编辑器</span>
                <button>重置</button>
                <button id="run">运行</button>
            </div>
            <div id="editor"> </div>
        </div>
        <iframe id="preview"></iframe>
    </div>
    <!-- 引入构建文件build中的核心文件 -->
    <script src="../build/src/ace.js" type="text/javascript" charset="utf-8"></script>
    <!-- 要更改更改主题,只需添加相关主题的文件，这个主题相信作为前端开发再熟悉不过了嘿嘿 -->
    <script src="../build/src/theme-solarized_light.js" type="text/javascript" charset="utf-8"></script>
    <!-- 默认情况，编辑器仅支持纯文本模式，许多其他语言可以作为单独的模块使用，语言模块的相关依赖在src中使用mode开头命名,这里引入html格式 -->
    <script src="../build/src/mode-html.js" type="text/javascript" charset="utf-8"></script>
    <script src="../build/src/worker-html.js" type="text/javascript" charset="utf-8"></script>
    <script>
        var editor = ace.edit("editor");
        // //设置主题
        editor.setTheme("ace/theme/solarized_light");
        // //设置编辑器语言
        editor.getSession().setMode("ace/mode/html");
        //设置代码折叠
        editor.session.setUseWrapMode(true);
        //设置显示的代码内容
        //设置高亮
        editor.setHighlightActiveLine(true);
        editor.setValue("<div>\n\thollow world!\n</div>\n<script><\/script>", -1);
        // editor.on("input", updatePreview);//代码输入监听
        function updatePreview() {
            var code = editor.getValue();
            let preview = document.getElementById("preview")
            preview.src = "data:text/html," + encodeURIComponent(code)
        }
        let run = document.getElementById("run");
        run.onclick =function() {
            updatePreview()
        }
    </script>
</body>

</html>
```
> 注意：通过试验，在引入核心`ace.js`文件之后，设置其他主题和语言时，如果不引入相关script，直接设置，`ace.js`会自动在同路径下找相关依赖，主题也能生效

> 这里需要考虑的是，在自己的项目中，是否放置全部的依赖文件，并且显式的引入。不同的处理方式可能存在一些问题，比如依赖过多且无效，但是不能确定哪些方法就默认需要某些依赖，也不能误删，以及关于文件追溯和维护的问题。
