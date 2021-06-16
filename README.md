# canvas_learn

canvas 学习&amp;相关 demo

# edit-image.html

一个关于在图片上使用矩形框标记，并且可以导出标记后的图片的 demo

# mosaic.html

使用 canvas 实现图片的马赛克

## 预备知识

- ctx.getImageData(x, y, width, height)

返回 ImageData 对象，该对象拷贝了画布指定矩形的像素数据。

对于 ImageData 对象中的每个像素，都存在着四方面的信息，即 RGBA 值：R - 红色 (0-255) G - 绿色 (0-255) B - 蓝色 (0-255) A - alpha 通道 (0-255; 0 是透明的，255 是完全可见的) color/alpha 以数组形式存在，并存储于 ImageData 对象的 data 属性中

以下代码可获得被返回的 ImageData 对象中第一个像素的 color/alpha 信息：

```
red=imgData.data[0];
green=imgData.data[1];
blue=imgData.data[2];
alpha=imgData.data[3];
```

提示：您也可以使用 getImageData() 方法来反转画布上某个图像的每个像素的颜色。

使用该公式遍历所有的像素，并改变其颜色值：

```
red=255-old_red;
green=255-old_green;
blue=255-old_blue;
```

```
var c=document.getElementById("myCanvas");
var ctx=c.getContext("2d");
var img=document.getElementById("tulip");
ctx.drawImage(img,0,0);
var imgData=ctx.getImageData(0,0,c.width,c.height);
// 反转颜色
for (var i=0;i<imgData.data.length;i+=4)
  {
  imgData.data[i]=255-imgData.data[i];
  imgData.data[i+1]=255-imgData.data[i+1];
  imgData.data[i+2]=255-imgData.data[i+2];
  imgData.data[i+3]=255;
  }
ctx.putImageData(imgData,0,0);
```

- ctx.putImageData

将图像数据（ImageData）放回到画布上

## ImageData 对象

- ImageData.data

Uint8ClampedArray 描述了一个一维数组，包含以 RGBA 顺序的数据，数据使用 0 至 255（包含）的整数表示。

- ImageData.height 只读

无符号长整型（unsigned long），使用像素描述 ImageData 的实际高度。

- ImageData.width 只读

无符号长整型（unsigned long），使用像素描述 ImageData 的实际宽度。

## 遇到的问题：使用 HTML5 中的 canvas，当用到 getImageData 方法获取图片信息时，碰到跨域无法获取的情况。

在 Chrome 浏览器中报错：

Uncaught DOMException: Failed to execute ‘getImageData’ on ‘CanvasRenderingContext2D’: The canvas has been tainted by cross-origin data.

原因

图片存储在本地时，是默认没有域名的，用 getImageData 方法时，浏览器会判定为跨域而报错！

解决跨域问题

将图片放置在服务器中 （在对应目录下使用 serve 或者 http-server 启动一个本地开发服务器）
