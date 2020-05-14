## 1. 概念

HTML5 中的`canvas`标签是使用来规定一个图形容器（画布），然后通过脚本（ `JavaScript`等）来绘制图形，比如：绘制路径、盒、圆、字符以及添加图像等。

如，定义一个宽度为`300px`，高度为`300px`的`canvas`画布：

```html
<canvas id="canvas" width="300" height="300"></canvas>
```

`canvas`只有两个可选属性`width`和`heigth`属性，单位为`px`。如果不给`canvas`设置`width`和`heigth`属性，则两个属性默认为`300`。也可以使用 `css` 属性来设置宽高，但是如宽高属性和初始比例不一致，他会**出现扭曲**。所以，最好不要使用 `css` 属性来设置 `canvas` 的宽高。



## 2. 渲染上下文

`canvas` 会创建一个固定大小的画布，会公开一个或多个**渲染上下文**（可以理解为**画笔**），使用**渲染上下文**来绘制和处理要展示的内容。

获得一个`2D`渲染上下文：

```javascript
const canvas = document.getElementById("canvas");
const context = canvas.getContext('2d');
```

我们就可以通过操控`context`来绘制`2D`图像了。

渲染上下文绘制图形时常用以下的方法和参数：

- `fillStyle`：设置图形填充的颜色
- `strokeStyle`：设置图形边框的颜色
- `fillRect(x, y, w, h)`：在坐标`(x, y)`处绘制长`w`宽` h` 的矩形
- `fillText(text, x, y)`：在坐标`(x, y)`处绘制文字
- `font`：定义字体
- `fillText(text,x,y)`： 在 canvas 上绘制实心的文本
- `strokeText(text,x,y)` ：在 canvas 上绘制空心的文本
- `arc(x, y, radius, startAngle, endAngle)`：在坐标`(x, y)`处绘制半径为`raduis`的弧，起始位置和结束位置分别为`startAngle`, `endAngle`
- `beginPath()`：新建一条路径
- `closePath()`：闭合一条路径
- `fill()`：填充绘制的路径
- `stroke()`：描边绘制的路径



## 3. 绘制 2D 图形

首先定义一个**画布**：

```html
<style>
    #canvas {
        background: #ffe3ec;
    }
</style>

<canvas id="canvas" width="600" height="600"></canvas>
```

<img src="https://note-figure-bed.oss-cn-shenzhen.aliyuncs.com/note/20200511184101.png" alt="image-20200511184043639" style="zoom: 80%;" />



### 3.1 绘制矩形

```javascript
const canvas = document.getElementById("canvas");
const context = canvas.getContext('2d');

context.fillStyle = "#5cccc0";
context.fillRect(0, 0, 300, 300);
```

<img src="https://note-figure-bed.oss-cn-shenzhen.aliyuncs.com/note/20200511184216.png" alt="image-20200511184211768" style="zoom:80%;" />



### 3.2 绘制圆形

```javascript
const canvas = document.getElementById("canvas");
const context = canvas.getContext('2d');

context.beginPath();
context.arc(300, 300, 300, 0, 2 * Math.PI);
context.fillStyle = "#5cccc0";
context.fill();
context.closePath();

context.beginPath();
context.arc(300, 300, 150, 0, 2 * Math.PI);
context.fillStyle = "#798fcc";
context.fill();
context.closePath();
```

<img src="https://note-figure-bed.oss-cn-shenzhen.aliyuncs.com/note/20200511184637.png" alt="image-20200511184630391" style="zoom:80%;" />

### 3.3 绘制文字

```javascript
const canvas = document.getElementById("canvas");
const context = canvas.getContext('2d');

context.font = "40px Arial";
context.fillText("你好，Canvas", 0, 40);
context.strokeText("你好，Canvas", 0, 80)
```

<img src="https://note-figure-bed.oss-cn-shenzhen.aliyuncs.com/note/20200511185420.png" alt="image-20200511185417200" style="zoom:80%;" />

## 4. 结合 `setInterval` 实现动画效果

将`context`的绘制放置到`setInterval`中，则可以实现重复绘制制作动画效果。

例如，`context`每`10ms`绘制一个宽度`rw`为40，高度`rH`为40的矩形，**绘制矩形前**用背景色填充整个画布，**绘制矩形后**将下一次绘制矩形的位置**加上**单位位移值`moveX`和`moveY`。当矩形的位置超出画布范围后，将对于的单位位移值取反。

```javascript
const canvas = document.getElementById("canvas");
const context = canvas.getContext('2d');
let x = 300, y = 300;
let rW = 40, rH = 40;
let moveX = 1, moveY = -1;
setInterval(() => {
    context.fillStyle = "#ffe3ec";
    context.fillRect(0, 0, canvas.width, canvas.height);
    context.strokeStyle = "#ff414b";
    context.strokeRect(x, y, rW, rH);
    if (x + rW > canvas.width || x < 0) {
        moveX = -moveX;
    }
    if (y + rH > canvas.height || y < 0) {
        moveY = -moveY;
    }
    x += moveX;
    y += moveY;
}, 10);
```

该动画效果就可以实现矩形在画布内移动反弹的效果。