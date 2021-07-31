[TOC]
## DOM
### getElementById
1. js应写在要获取的标签后面。
2. 返回的是元素对象。
3. console.dir可以打印返回的元素对象，查看内部的属性和方法。
4. 内部要加引号。
示例： `var timer = document.getElementById('time');`
### getElementsByTagName
1. 返回的是获取元素对象的集合，以伪数组的形式存储。
2. 以li形式取得的元素并赋给lis后可用lis[0]来调用。
3. 数组内部内容是动态变动的。
4. 如果只有一个元素还是返回伪数组。
5. 如果没有元素返回空的伪数组。
6. 可以指定父元素来获取子元素`element.getElementsByTagName('标签名');`
注意父元素一定要指明是哪一个元素对象。
7. 可以直接取id并获取所有元素。
### H5新增方式
getElementByClassName('class');根据类名返回元素对象集合
querySelector('选择器');根据指定选择器返回第一个元素对象，比如box类就要写成.box
navid就要写成#nav
querySelectorAll('选择器');可以返回指定选择器的所有元素对象集合
### 获取html和body
#### 获取body元素
document.body;
#### 获取html元素
document.documentElement;
### 事件基础
1. 事件三部分：事件源、事件类型、事件处理程序
    事件源：事件被触发的对象（按钮）
    事件类型：如何触发什么事件（鼠标经过、鼠标点击onclick）
    事件处理程序：通过一个函数赋值的方式完成。
    btn.onclick = function() {
    alert(' ');
    }

2. 执行事件：获取事件源、绑定事件/注册事件、添加事件处理程序

   ![image-20210408185919721](F:\pic\image-20210408185919721.png)

### 操作元素
#### 改变元素内容
元素可以不用添加事件直接修改。
##### innerText
不识别html标签，标签会直接显示。（非标准）
去除空格和换行。
##### innerHTML
可以识别html标签。（W3C标准）
保留空格和换行。
示例：`div.innerHTML = '<strong>今天是：<strong> 2021';
两个属性都是可读写的，可以获取元素里面的内容。读写的时候也可能会去除html标签。
#### 修改元素属性
注：图片的title属性是鼠标停留时出现的文字。
获取时间的方式：

```javascript
var date = new Date();
var h = date.getHours();
```
#### 改变表单元素
表单内的文字内容通过value来修改。
禁用表单用disabled，当然在对某个对象的操作函数内可以直接用this指代事件函数的调用者。
#### 密码框示例
```css
.box {
	width:
	border-bottom:
	margin:
}

.box input {
	width:
	height:
	border:
	outline:
}
```
#### 样式属性操作
##### element.style
示例：`div.style.backgroundColor = 'purple';`
产生的是行内样式，权重比较高。

注：`display:none`隐藏元素。
数字中间加变量的方法示例：
`'0' + index + 'px';`
#### 显示隐藏文本框内容
获得焦点 onfocus
点击文本框时即为获得焦点
失去焦点 onblur
从文本框点到外面的时候

##### element.className
将样式提前写到CSS里面，然后直接`~ = 'CSSName';`
适合于样式较多或者功能复杂的情况。
如果原来已经有样式了会直接覆盖。
如果想要保留原先的类名就要`this.className = 'first change';`
即为多类名选择器。



### 排他思想
有同一组元素给某一个元素实现某种样式应用循环的排他思想算法。
1. 所有元素全部清除样式；
2. 给当前元素设置样式。
onmouseover 鼠标经过事件
onmouseout 鼠标离开事件

### 自定义属性的操作
#### 获取元素值
`element.属性`只能获取内置的属性值，即元素本身自带的属性
获得自定义属性：`element.getAttribute('id');`
#### 设置元素属性值
`element.属性 = '值';`
`element.setAttribute('属性','值');`//属性里面写的就是class，不是className
#### 移除属性
`element.removeAttribute('id');`
#### tab栏切换
上下界面分离，点击上面的时候添加一个类名index并用index来索引下方应该显示的对应内容。
#### H5自定义属性
示例：`<div getTime='20'></div>`此处的getTime即为自定义属性。
##### 设置H5自定义属性
H5规定自定义属性data-开头作为属性名并且赋值。
示例：`<div data-index="1"></div>`
当然也可以用js来设置：`element.setAttribute('data-index',2);`

##### 获取H5自定义属性
`element.getAttribute('data-index');`
`element.dataset.index`或者`element.dataset['index']`
dataset是一个集合里面存放了所有以data开头的自定义属性。
但如果命名为data-list-name则应用一下几种方式：
`element.getAttribute('data-list-name');`
`element.dataset.listName;`
`element.dataset['listName'];`
注意应该用驼峰命名法。
注：这种新方式只能获取data-开头的属性。因而一般用getAttribute。
### 节点操作
利用父子兄节点关系获取元素。
网页中的所有内容都是节点，用node来表示。
节点至少有nodeType、nodeName、nodeValue三个基本属性。
元素节点nodeType为1，属性节点nodeType为2，文本节点nodeType为3（包括文字】空格、换行）
#### 父级节点
`node.parentNode`
示例：父节点box，子节点erweima，则可以用erweima.parentNode来表示box。
得到的是离元素最近的父节点，如果找不到父节点则返回NULL。
#### 子节点
`parentNode.childNodes`
得到的是子节点的集合，包含元素节点、文本节点等等。
如果只要元素节点就要用循环与判断。
`parentNode.children`
只返回子元素节点。
`parentNode.firstChild`
获取的是第一个子节点，文本节点或者元素节点都有可能。
`parentNode.lastChild`
获取的是最后一个子节点，文本节点或者元素节点都有可能。
`parentNode.firstElementChild`
返回第一个子元素节点，找不到则返回NULL。
`parentNode.lastElementChild`
返回最后一个子元素节点，找不到则返回NULL。
注：后两个方案兼容性差。
解决方案（没有兼容性问题）：
`parentNode.children[0]`
第一个元素节点。
`parentNode.children[parentNode.children.length-1]`
最后一个元素节点。
#### 下拉菜单示例
结构：
```html
<ul class="nav">
	<li>
		<a href="#">微博</a>
		<ul>
		</ul>
	</li>
	<li>···
	</li>
</ul>
```
```js
//1. 获取元素
var nav = document.querySelector(".nav");
var lis = nav.children;//得到li
//2. 循环注册事件
for(var i = 0; i < lis.length; i++) {
	lis[i].onmouseover = function () {
		this.children[i].style.display = 'block';
	}
	lis[i].onmouseout = function () {
		this.children[i].style.display = 'none';
	}
}
```
#### 兄弟节点
`node.nextSibling`
下一个兄弟节点，会得到文本节点。
`node.previousSibling`
上一个兄弟节点，会得到文本节点。
`node.nextElementSibling`
下一个兄弟元素节点，会返回NULL。
`node.previousElementSibling`
上一个兄弟元素节点，会返回NULL。
解决兼容性问题：
```js
function getNextElementSibling(element) {
	var el = element;
	while(el = el.nextSibling) {
		if(el.nodeType === 1) {
			return el;
		}
	}
	return null;
}
```
#### 创建节点
`document.createElement('tagName')`
创建由tagName指定的HTML元素，称为动态创建元素节点。
#### 添加节点
`node.appendChild(child)`
将一个节点添加到指定父节点的子节点列表末尾，是追加元素。
示例：
```js
<ul>
	<li>123</li>
</ul>

var li = document.createElement('li');
var ul = document.querySelector('ul');
ul.appendChild(li);
```
`node.insertBefore(child, 指定元素)`
将一个节点添加到父节点的指定子节点前面。
示例：
```js
<ul>
	<li>123</li>
</ul>

var lili = document.createElement('li');
ul.insertBefore(lili, ul.children[0]);
```
注意：向页面添加一个新的元素应该先创建元素再添加元素。
#### 留言板示例
```html
<body>
	<textarea name="" id=""></textarea>
	<button>发布</button>
	<ul>
	
	</ul>
	<script>
		//1. 获取元素
		var btn = document.querySelector('button');
		var text = document.querySelector('textarea');
		var ul = document.querySelector('ul');
		//2. 注册事件
		btn.onclick = function() {
			if(text.value == ''){
				alert('没有输入内容');
				return false;
			} else {
				//1. 创建元素
				var li = document.createElement('li');
				li.innerHTML = text.value;
				//2. 添加元素
                //ul.appendChild('li');
                ul.insertBefore(li, ul.children[0]);
			}
		}
	</script>
</body>
```
#### 删除节点
`node.removeChild(child)`
从DOM中删除一个子节点，返回删除的节点。
#### 删除留言示例
```html
<body>
	<textarea name="" id=""></textarea>
	<button>发布</button>
	<ul>
	
	</ul>
	<script>
		//1. 获取元素
		var btn = document.querySelector('button');
		var text = document.querySelector('textarea');
		var ul = document.querySelector('ul');
		//2. 注册事件
		btn.onclick = function() {
			if(text.value == ''){
				alert('没有输入内容');
				return false;
			} else {
				//1. 创建元素
				var li = document.createElement('li');
				li.innerHTML = text.value + "<a href='javascript:;'>删除</a>";
				//2. 添加元素
                //ul.appendChild('li');
                ul.insertBefore(li, ul.children[0]);
                //3. 删除元素 删除的是当前链接的li的父亲
                var as = document.querySelectorAll('a');
                for(var i = 0; i < as.length; i++) {
                	as[i].onclick = function () {
                		ul.removeChild(this.parentNode);
                	}
                }
			}
		}
	</script>
</body>
```
注意：阻止链接跳转需要添加`javascript:void(0);`或者`javascript:;`。
#### 复制节点
`node.cloneNode()`
返回调用该方法的节点的一个副本。
当然要添加之后才看得到。
注：如果括号参数为空或者为false，则是浅拷贝，只克隆复制节点本身，不克隆里面的子节点。
	括号参数为true则是深拷贝，复制标签并且复制里面的内容。
#### 动态生成表格示例
常见表格示例：
```html
<body>
	<table cellspacing="0">
		<thead>
			<tr>
				<th>姓名</th>
				<th>科目</th>
				<th>成绩</th>
				<th>操作</th>
			</tr>
		</thead>
		<tbody>
		
		</tbody>
	</table>
</body>
```
thead里面放表头的内容，tbody里面放表格内部的具体数据。
多个数据用数组来存储。示例：
```js
var data = [{
	name: '';
	subject: '';
	score: ;
}, {
	name: '';
	subject: '';
	score: ;
}, {
	name: '';
	subject: '';
	score: ;
}]
```
创建行与单元格，单元格填充数据，创建用于删除的单元格
```js
var tbody = document.querySelector('tbody');
for(var i = 0; i < data.length; i++) {
	var r = document.createElement('tr');//创建行
	tbody.appendChild(tr);//加入行
	for(var k in data[i]) {
		var td = document.createElement('td');//创建单元格
		td.innerHTML = data[i][k];//将数组里的内容复制到单元格内
		tr.appendChild(td);//将创建的单元格加入行内
	}
	//创建删除的单元格
	var td = document.createElement('td');
	td.innerHTML = '<a href="javascript:;">删除</a>';
	tr.appendChild(td);
}
//删除操作
var as = document.querySelectorAll('a');
for(var i = 0; i < as.length; i++) {
	as[i].onclick = function () {
		tbody.removeChild(this.parentNode.parentNode);
	}
}
```

#### 三种动态创建元素区别
示例环境：
```html
<button>点击</button>
<p>abc</p>
<div class="inner"></div>
<div class="create"></div>
```
`document.write()`
示例：`document.write('<div>123<div>');`
直接将内容写入文档的内容流，但是文档流执行完毕会导致页面全部重绘。
如果用按钮或者`window.onload`来触发会导致重新生成一个html页面，之前的内容全部消失。
`element.innerHTML`

将内容写入某个DOM节点，不会导致页面全部重绘。

示例：

```js
var inner = document.querySelector('.inner');
inner.innerHTML = '<a href='#'>百度</a>';
```
如果要加入多个元素则要循环第二句话。
```js
var inner = document.querySelector('.inner');
for(var i = 0; i <=100; i++) {
	inner.innerHTML += '<a href="#">百度</a>';
}
```
想要效率最高则应用数组：
```js
var inner = document.querySelector('.inner');
var arr = [];
for(var i = 0; i<= 100; i++) {
	arr.push('<a href="#">百度</a>');
}
inner.innerHTML = arr.join('');
```
`document.createElement()`
```js
var create = document.querySelector('.create');
var a = document.createElement('a');
create.appendChild(a);
```
如果要加入多个元素则要循环后两句话。
注意：用innerHTML创建多个元素时由于拼接字符串效率很低，但用数组是效率很高（结构复杂），createElement效率居中（结构清晰）。

### 事件操作

| 鼠标事件    | 触发条件         |
| ----------- | ---------------- |
| onclick     | 鼠标点击左键触发 |
| onmouseover | 鼠标经过触发     |
| onmouseout  | 鼠标离开触发     |
| onfocus     | 获得鼠标焦点触发 |
| onblur      | 失去鼠标焦点触发 |
| onmousemove | 鼠标移动触发     |
| onmouseup   | 鼠标弹起触发     |
| onmousedown | 鼠标按下触发     |

### 事件高级

#### 注册事件

即给元素添加事件。

##### 传统方式

利用on开头的事件onclick。

特点：同一个元素同一个事件只能设置一个处理函数，最后注册的处理函数将会覆盖前面注册的处理函数。即注册事件的唯一性。

比如对同一个按钮设置两个onclick函数，前面的那个就不会执行。

##### 方法监听方式

`addEventListener()`

特点：同一个元素同一个事件可以注册多个监听器，会按照注册顺序依次执行。

用法：`eventTarget.addEventListener(type, listener[, useCapture])`

该方法会将指定的监听器注册到eventTarget（目标对象）上，当该对象触发指定的事件时，就会执行事件处理函数。

参数：type：事件类型字符串（要加引号），比如click、mouseover，没有on。

​			listener：事件处理函数，事件发生时会调用该监听函数。

​			useCapture：可选参数，默认false。

示例：

```html
<button>方法监听注册事件</button>
<script>
	var btns = document.querySelectorAll('button');
	btns[0].addEventListener('click', function() {
		alert(hello);
	})
</script>
```
此时如果对同一个按钮设置两个click事件则会先后执行两个事件。

###### IE友好的事件监听方式
`attachEvent`
用法：`eventTarget.attachEvent(eventNameWithOn, callback)`
该方法将指定的监听器注册到eventTarget（目标对象）上，当该对象触发指定的事件时，指定的回调函数就会被执行。
参数：eventNameWithOn：事件类型字符串，比如onclick、onmouseover，这里要带on。
	callback：事件处理函数，当目标触发事件时回调函数被调用。
示例：
```html
<button>ie9 attachEvent</button>
<script>
	var btns = document.querySelectorAll('button');
	btns[0].attachEvent('onclick', function() {
		alert(hello);
	})
</script>
```
##### 兼容性解决方案
```js
function addEventListener(element, eventName, fn) {
	//判断当前浏览器是否支持addEventListener方法
	if (element.addEventListener) {
		element.addEventListener(eventName, fn);//第三个参数默认是false
	} else if (element.attachEvent) {
		element.attachEvent('on' + eventName, fn);
	} else {
		//相当于element.onclick = fn;
		element ['on' + eventName] = fn;
	}
}
```
兼容性处理的原则：首先照顾大多数浏览器，再处理特殊浏览器。
#### 删除事件
##### 传统方式
`eventTarget.onclick = null;`
##### 方法监听注册方式
`removeEventListener`
用法：`eventTaget.removeEventListener(type, listener[, useCapture]);`
示例：
```js
divs[0].addEventListener('click', fn);
function fn() {
	alert(hello);
	divs[0].removeEventListener('click', fn);
}
```
不能用匿名函数，一定要有名字，且里面的fn不需要调用加小括号。
##### IE友好的移除事件监听方式
`detachEvent`
用法：`eventTarget.detachEvent(eventNameWithOn, callback);`
示例：
```js
divs[0].attachEvent('onclick', fn1);
function fn1() {
	alert(hello);
	divs[0].detachEvent('onclick', fn1);
}
```
##### 兼容性解决方案
```js
function removeEventListener(element, eventName, fn) {
	//判断当前浏览器是否支持removeEventListener方法
	if (element.removeEventListener) {
		element.removeEventListener(eventName, fn);//第三个参数默认是false
	} else if (element.detachEvent) {
		element.detachEvent('on' + eventName, fn);
	} else {
		element['on' + eventName] = null;
	}
}
```
#### DOM事件流
事件流描述的是从页面中接收事件的顺序。
事件发生时会在元素节点之间按照特定的顺序传播，这个传播过程就是DOM事件流。
##### 三个阶段
1. 捕获阶段（从外向内直到要操作的元素）
2. 当前目标阶段
3. 冒泡阶段（从内向外回到最外面）
注：①js代码中只能执行捕获或者冒泡其中的一个阶段。
	②onclick和attchEvent只能得到冒泡阶段。
	③捕获阶段如果addEventListener第三个参数是true那么则处于捕获阶段。
	④冒泡阶段如果addEventListener第三个参数是false或者默认那么则处于冒泡阶段。
	⑤有些事件是没有冒泡的，比如onblur、onfocus、onmouseenter、onmouseleave。
在捕获阶段由于从外向内进行所以会先执行外部的有事件监听的函数，不管函数的顺序。
在冒泡阶段由于从内向外进行所以会先执行内部的有事件监听的函数，不管函数的顺序。
#### 事件对象
1. event就是一个事件对象，写到侦听函数的小括号里面，当形参来看。
示例：
```js
var div = document.querySelector('div');
div.onclick = function(event) {
	console.log(event);//点击就会出现mouseevent
}
```
```js
div.addEventListener('click', function(event){
	console.log(event);
})
```
2. 事件对象只有有了事件才会存在，是系统自动创建的，不需要传参。
3. 事件对象是事件的一系列相关数据的集合，跟事件相关的，比如鼠标点击里面就包含了鼠标的相关信息，例如鼠标坐标。如果是键盘事件里面就包含键盘事件的信息，比如判断用户按下了哪个键。
4. 事件对象可以自己命名，比如event、evt、e。
5. 事件对象也有兼容性问题。ie6、7、8通过window.event获取。兼容性写法：`e = e || window.event;`

示例：

```js
var div = document.querySelector('div');
div.onclick = function(e) {
    e = e || window.event;
    console.log(e);
}
```

##### 事件对象的常见属性和方法

| 事件对象属性方法  | 说明                                                         |
| ----------------- | ------------------------------------------------------------ |
| e.target          | 返回触发事件的对象（标准）                                   |
| e.srcElement      | 返回触发事件的对象（非标准，ie6、7、8使用）                  |
| e.type            | 返回事件的类型，比如click、mouseover，不带on                 |
| e.cancelBubble    | 该属性阻止冒泡（非标准，ie6、7、8使用）                      |
| e.returnValue     | 该属性阻止默认事件（默认行为）（非标准，ie6、7、8使用），比如不让链接跳转 |
| e.preventDefault  | 该方法阻止默认事件（默认行为）（标准），比如不让链接跳转     |
| e.stopPeopagation | 阻止冒泡（标准）                                             |

###### e.target

```js
var div = document.querySelector('div');
div.addEventListener('click', function(e) {
	console.log(e.target);
})
```

注意：e.target返回的是触发事件的对象（元素），this返回的是绑定事件的对象（元素）。

简而言之，e.target碰了谁返回谁，this的函数.前面是谁返回谁。

示例：当ul里面有li时给ul绑定事件再点击li，则this指向的是ul，而e.target指向的是li。

兼容性写法：

```js
div.onclick = function(e) {
    e = e || window.event;
    var target = e.target || e.srcElement;
    console.log(target);
}
```

跟this非常相似的属性：currentTarget，返回绑定的事件。（ie6、7、8不行）

P65开始










