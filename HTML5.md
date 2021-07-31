[TOC]
## 语义元素
清楚描述意义的标签，例如`<form><table><img>`等等。

![image-20210412190535833](F:\pic\image-20210412190535833.png)

| 标签         | 描述                                               |
| ------------ | -------------------------------------------------- |
| <article>    | 定义文章。                                         |
| <aside>      | 定义页面内容以外的内容。                           |
| <details>    | 定义用户能够查看或隐藏的额外细节。                 |
| <figcaption> | 定义 <figure> 元素的标题。                         |
| <figure>     | 规定自包含内容，比如图示、图表、照片、代码清单等。 |
| <footer>     | 定义文档或节的页脚。                               |
| <header>     | 规定文档或节的页眉。                               |
| <main>       | 规定文档的主内容。                                 |
| <mark>       | 定义重要的或强调的文本。                           |
| <nav>        | 定义导航链接。                                     |
| <section>    | 定义文档中的节。                                   |
| <summary>    | 定义 <details> 元素的可见标题。                    |
| <time>       | 定义日期/时间。                                    |



### section
文档中的节，有主题的内容组，通常具有标题。
示例：
```html
<section>
	<h1>head</h1>
	<p>this is a para.</p>
</section>
```
### article
独立的文档，可以独立于网页其他内容阅读。
示例：
```html
<article>
	<h1>head</h1>
	<p>this is a para.</p>
</article>
```

这两个元素的大小没有绝对的关系。
### header
规定页眉，放介绍性内容。
一个文档内可以有多个header元素。
示例：
```html
<article>
	<header>
		<h1>head</h1>
		<p>this is a para.</p>
	</header>
	<p>this is the text.</p>
</article>
```
### footer
规定页脚，通常包含文档的作者、版权信息、使用条款链接、联系信息等等。
一个文档内可以有多个footer元素。
示例：
```html
<footer>
	<p>By: XXX</p>
	<p>this is a para.</p>
</footer>
```
### nav
导航链接集合，定义大型的导航链接块。但并非文档中所有链接都应该位于 <nav> 元素中。
示例（就是把链接堆在一起）：
```html
<nav>
<a href="/html/">HTML</a> |
<a href="/css/">CSS</a> |
<a href="/js/">JavaScript</a> |
<a href="/jquery/">jQuery</a>
</nav> 
```
### aside
页面主内容之外的某些内容（比如侧栏），aside 内容与周围内容相关。
示例：
```html
<aside>
	<p>this is aside.</p>
</aside>
```
### figure && figcaption
给图片添加可见的解释，即在图片下方放上一串文字。
示例：

```html
<figure>
   <img src="pic_mountain.jpg" alt="The Pulpit Rock" width="304" height="228">
   <figcaption>Fig1. - The Pulpit Pock, Norway.</figcaption>
</figure> 
```

## 页面书写规范

1. 在文档首行声明文档类型。

   `<!DOCTYPE html>`或者`<!doctype html>`
   
2. 使用小写元素名。

3. 把所有元素关上。

4. 关闭空元素。

   <meta charset="utf-8" />

5. 使用小写属性名。

6. 属性值加上引号。

7. 图片一定要加alt属性，规定大小，防止闪烁。

8. 减少不必要的空格。

9. 每行写得短一点。

10. 把握好空格和缩进。

11. <html>和<body>标签不要省。

12. 可以省略<head>。

13. 一定要写<title>。

14. 及时定义字符编码和语言。

    <html lang="en-US"><meta charset="UTF-8">

15. 短注释应该在单行中书写，并在 <!-- 之后增加一个空格，在 <!-- 之前增加一个空格.

    <!-- This is a comment -->
    
16. 长注释，跨越多行，应该通过 <!-- 和 --> 在独立的行中书写：

    ```html
    <!-- 
      This is a long comment example. This is a long comment example. This is a long comment example.
      This is a long comment example. This is a long comment example. This is a long comment example.
    -->
    ```

17. 用简单的语法链接样式表（不用加type）

    <link rel="stylesheet" href="styles.css">
    
    开括号与选择器位于同一行
    在开括号之前用一个空格
    使用两个字符的缩进
    在每个属性与其值之间使用冒号加一个空格
    在每个逗号或分号之后使用空格
    在每个属性值对（包括最后一个）之后使用分号
    只在值包含空格时使用引号来包围值
    把闭括号放在新的一行，之前不用空格
    避免每行超过 80 个字符

18. 用简单的语法链接js（不用加type）

    <script src="myscript.js">
    
19. html和js的命名保持统一。

20. 文件名小写。

## canvas

用js在网页上绘制图像。

### 创建canvas元素

规定元素的 id、宽度和高度：

`<canvas id="myCanvas" width="200" height="100"></canvas>`

### 绘制过程

获取元素：

`var c = document.getElementById("myCanvas");`

创建context对象：

`var cxt = c.getContext("2d");`

绘制图形：

```js
cxt.fillStyle="#FF0000";
cxt.fillRect(0,0,150,75); 
```

`fillRect`规定绘制区域，前两个参数是左上坐标，后两个参数是长宽。

### 绘制实例

**一个角**

```js
<script type="text/javascript">

var c=document.getElementById("myCanvas");
var cxt=c.getContext("2d");
cxt.moveTo(10,10);
cxt.lineTo(150,50);
cxt.lineTo(10,50);
cxt.stroke();

</script>
```

```js
<canvas id="myCanvas" width="200" height="100" style="border:1px solid #c3c3c3;">
Your browser does not support the canvas element.
</canvas>
```

**一个圆**

```js
<script type="text/javascript">

var c=document.getElementById("myCanvas");
var cxt=c.getContext("2d");
cxt.fillStyle="#FF0000";
cxt.beginPath();
cxt.arc(70,18,15,0,Math.PI*2,true);
cxt.closePath();
cxt.fill();

</script>
```

canvas同上。

**渐变**

```js
<script type="text/javascript">

var c=document.getElementById("myCanvas");
var cxt=c.getContext("2d");
var grd=cxt.createLinearGradient(0,0,175,50);
grd.addColorStop(0,"#FF0000");
grd.addColorStop(1,"#00FF00");
cxt.fillStyle=grd;
cxt.fillRect(0,0,175,50);

</script>
```

canvas同上。

**图像**

将图像放到画布上。

```js
<script type="text/javascript">

var c=document.getElementById("myCanvas");
var cxt=c.getContext("2d");
var img=new Image()
img.src="flower.png"
cxt.drawImage(img,0,0);

</script>
```

canvas同上。

## SVG
可伸缩矢量图形
示例：
```html
<svg xmlns="http://www.w3.org/2000/svg" version="1.1" height="190">
  <polygon points="100,10 40,180 190,60 10,60 160,180"
  style="fill:lime;stroke:purple;stroke-width:5;fill-rule:evenodd;" />
</svg>
```

## Canvas和SVG的区别
Canvas

1. 依赖分辨率
2. 不支持事件处理器
3. 弱的文本渲染能力
4. 能够以 .png 或 .jpg 格式保存结果图像
5. 最适合图像密集型的游戏，其中的许多对象会被频繁重绘

SVG
1. 不依赖分辨率
2. 支持事件处理器
3. 最适合带有大型渲染区域的应用程序（比如谷歌地图）
4. 复杂度高会减慢渲染速度（任何过度使用 DOM 的应用都不快）
5. 不适合游戏应用

## 音频

### 使用插件

可以用`<object>`和`<embed>`添加插件。

### 使用`<embed>`元素

示例：`<embed height="100" width="100" src="song.mp3" />`

### 使用`<object>`元素

示例：`<object height="100" width="100" data="song.mp3"></object>`

### 使用`<audio>`元素

示例：

```html
<audio controls="controls">
  <source src="song.mp3" type="audio/mp3" />
  <source src="song.ogg" type="audio/ogg" />
Your browser does not support this audio format.
</audio>
```

### 兼容性方案

```html
<audio controls="controls" height="100" width="100">
  <source src="song.mp3" type="audio/mp3" />
  <source src="song.ogg" type="audio/ogg" />
<embed height="100" width="100" src="song.mp3" />
</audio>
```

### 插入播放器

在网页底部插入：`<script type="text/javascript" src="http://mediaplayer.yahoo.com/js"></script>`

然后用如下方式就可以插入音频，js会自动为每首歌创建播放按钮。

```html
<a href="song.mp3">Play Sound</a>
```

### 超链接

示例：

```html
<a href="song.mp3">Play the sound</a>
```

浏览器会启动辅助应用程序来播放。

## 视频

### 使用`<embed>`标签

示例：`<embed src="movie.swf" height="200" width="200"/>`

### 使用`<object>`标签

示例：`<object data="movie.swf" height="200" width="200"/>`

### 使用`<video>`标签

示例：

```html
<video width="320" height="240" controls="controls">
  <source src="movie.mp4" type="video/mp4" />
  <source src="movie.ogg" type="video/ogg" />
  <source src="movie.webm" type="video/webm" />
Your browser does not support the video tag.
</video>
```

### 兼容性方案

```html
<video width="320" height="240" controls="controls">
  <source src="movie.mp4" type="video/mp4" />
  <source src="movie.ogg" type="video/ogg" />
  <source src="movie.webm" type="video/webm" />
  <object data="movie.mp4" width="320" height="240">
    <embed src="movie.swf" width="320" height="240" />
  </object>
</video>
```

### 从其他视频网站链接

```html
<embed src="http://player.youku.com/player.php/sid/XMzI2NTc4NTMy/v.swf" 
width="480" height="400" 
type="application/x-shockwave-flash">
</embed>
```

### 超链接

示例：`<a href="movie.swf">Play a video file</a>`

