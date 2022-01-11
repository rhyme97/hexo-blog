---
title: 改变图片的透明度
date: 2021-05-31 18:50:11
tags:
    - canvas
categories:
    - javascript
descriptions:
---

在某些情况下，某些图片周边的背景都是白色或者其他颜色，我们期望它们是透明的，但这些图片是动态获取的，无法直接交给UI处理成透明样式。而通过javascript的方式，利用颜色RGBA的原理，我们也可以像PS那样，实现取色，替换等功能。

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        #canvas {
            position: absolute;
            left: 10px;
            top: 10px;
        }
    </style>
</head>

<body>
    RGBA的解释 我们知道图片由很多个像素点组成的，每个像素点都有颜色，而颜色是由三基色RGB构成。而A是Alpha通道，用作不透明度参数，0%为完全透明，100%是完全不透明。所以说如果我们想实现白色背景的JPEG 图片转成透明的
    PNG 图片，只需要将白色背景对应的像素点得Alpha值变成0就好了。<br>
    <canvas id="canvas"></canvas>
</body>
<script>


    var img = new Image();  //定义一个图片对象
    img.src = "你的图片地址，该地址跨域会导致canvas方式出错"
    // 图片加载完成
    img.onload = function () {
        //    获取图片的width和height
        let width = img.width
        let height = img.height
        let canvas = document.getElementById("canvas");   //获取Canvas画布对象
        canvas.width = width;
        canvas.height = height;
        let ctx = canvas.getContext('2d');  //获取2D上下文对象，大多数Canvas API均为此对象方法
        // canvas 所有的操作都是在 context 上，所以要先将图片放到画布上才能操作
        ctx.drawImage(img, 0, 0, width, height)
        let imageData = ctx.getImageData(0, 0, width, height)//canvas不能跨域,如果图片
        // 获取画布的像素信息
        let data = imageData.data

        // 对像素集合中的单个像素进行循环，每个像素是由4个通道组成，所以 i=i+4
        for (let i = 0; i < data.length; i += 4) {
            // 得到 RGBA 通道的值
            let r = data[i]
                , g = data[i + 1]
                , b = data[i + 2]
            // 肉眼的观察下基本都是白色的，所以我在这里把 RGB 值都在 245 以上的的定义为白色
            if ([r, g, b].every(v => v < 256 && v > 245)) data[i + 3] = 0.1//0.1是opacity的值
        }
        //imageData.data = data
        // 将修改后的代码复制回画布中
        ctx.putImageData(imageData, 0, 0)
        var imgSrc = canvas.toDataURL();  //这里就是获取到的新的获取处理后图片的DataURL
    }

</script>

</html>
```