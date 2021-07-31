## jQuery

### 安装

`node install jquery`

### 入口函数

写法一：

```js
$(document).ready(function () {
	$('#btnOne').click(function () {
        $('div').css('border', '1px solid red');
    })
})
```

写法二：

```js
$(function () {
    $('#btnOne').click(function () {
        $('div').css('border', '1px solid red');
    })
})
```

上述代码在入口处通过按钮bunOne设置了div的边框。

注意：1. 用$直接取页面中的元素，可以是id等等。

2. 此处取div不管页面里有多少的div都会一起取过来，是隐式迭代。

与window.onload的区别：1. 后者不能写多个。

2. 执行时机不同。jQuery的入口函数执行早。jQuery的入口函数要等页面上的dom树加载完毕后执行，window.onload要等页面上所有的资源都加载完毕后才执行。

### $

jQuery文件是一个自执行函数。

注意：引入一个js文件会执行js文件内部的代码。

这个自执行文件会给window对象添加一个jQuery属性和$属性。

总结：$和jQuery是等价的，是一个函数。

有`window.jQuery === window.$`

1. 参数传递一个匿名函数->入口函数
2. 参数传递一个字符串->选择器/创建一个标签
3. 参数传递一个dom对象->把dom对象转换为jQuery对象
### dom对象

例：`var btn = document.getElementById('btn');`

原生js选择器获取到的对象

特点：只能调用dom方法或者属性，不能调用jQuery的属性或者方法。

### jQuery对象

例：`var btn = $('#btn');`

使用jQuery选择器获取到的对象

特点：只能调用jQuery的方法或者属性，不能调用原生jsdom对象的属性或者方法。是一个伪数组，其实就是dom对象的包装集。

### dom对象和jQuery对象的转换

#### dom->jQuery

```js
var div = document.getElementById('one');
var $div = $(div)
```

#### jQuery->dom

##### 使用下标取

```js
var $div = $('div');
var div = $div[0];
```

 ##### 使用get()

```js
var div = $div.get(1);
```

### text()

用于设置和获取文本

#### 获取文本

`$('#div').text()`

获取指定div标签中所有的文本，包括后代元素的文本。

获取标签为div的元素的文本：会得到包含了多个dom元素的jQuery对象。

通过text()方法获取文本会把所有dom元素的文本获取到。

#### 设置文本

`$('#div').text('new text');`

注意：设置文本会覆盖原有的内容，如果设置的文本中包含标签是不会解析标签的。

给指定的div的元素设置文本：设置文本。

给标签为div的元素设置文本：包含了多个dom元素的jQuery对象，会把所有dom元素都设置上。

#### 获取样式

css()可以用于设置/获取样式

```js
$('#btn').click(function () {
    $('#div1').css('width');
})
```

注意：在ie浏览器中获取边框这样的样式值要给一个准确的值。

如果获取一类标签比如div的样式会返回找到的第一个div的值。

#### 设置样式

```js
$('#btn').click(function () {
    $('#div1').css('width','300px');
})
```

如果写300px必须有引号，如果只有300则可以不加引号

注意：设置的样式是行内样式。

需要传递样式名和样式值。

设置单一样式：

`css('width','300px');`

设置多个样式：

```js
css({
    width: 300,
    'height': '300px',
    'background-color': 'green',
    border: '10px solid white'
})
```

注意：可以把-换为大写字母，比如margin-top写成marginTop。

### 选择器

#### 基本选择器

与css选择器相同。

| 名称       | 用法               | 描述                         |
| ---------- | ------------------ | ---------------------------- |
| ID选择器   | $('#id');          | 获取ID指定的元素             |
| 类选择器   | $('.class');       | 获取同一类class的元素        |
| 并集选择器 | $('div,p,li');     | 符合条件之一即可             |
| 交集选择器 | $('div,redClass'); | 获取class为redClass的div元素 |
| 标签选择器 | $('div');          | 获取同一类标签的所有元素     |

#### 层次选择器

| 名称       | 用法          | 描述                                                         |
| ---------- | ------------- | ------------------------------------------------------------ |
| 子代选择器 | $('ul > li'); | 使用>号获取儿子层次的元素，但不会获取孙子层级的元素，此处指获取ul下方的所有子li |
| 后代选择器 | $('ul li');   | 使用空格代表后代选择器，获取ul下所有li元素，包括孙子         |

注意：如果用子代选择器获取多个ul下的元素需要写成：`$('ul > a , ul > b');`

#### 过滤选择器

| 名称        | 用法                              | 描述                                                      |
| ----------- | --------------------------------- | --------------------------------------------------------- |
| :eq (index) | $('li:eq(2)').css('color','red'); | 获取到的li元素中，选择索引号为2的元素，索引号index从0开始 |
| :odd        | $('li:odd').css('color','red')    | 获取到的li元素中，选择索引号为奇数的元素                  |
| :even       | $('li:even').css('color','red');  | 获取到的li元素中，选择索引号为偶数的元素                  |

注意：此类选择器都有冒号。

`$('li:odd').css()`可对奇数行的li设置样式。

#### 筛选选择器

| 名称               | 用法                        | 描述                             |
| ------------------ | --------------------------- | -------------------------------- |
| children(selector) | $('ul').children('li');     | 相当于$('ul-li')，子类选择器     |
| find(selector)     | $('ul').find('li');         | 相当于$('ul li')，后代选择器     |
| siblings(selector) | $('#first').siblings('li'); | 查找兄弟结点，不包括自己本身     |
| parent()           | $('#first').parent();       | 查找父亲                         |
| eq(index)          | $('li').eq(2);              | 相当于$('li:eq(2)'),index从0开始 |
| next()             | $('li').next();             | 找下一个兄弟                     |
| prev()             | $('li').prev();             | 找上一次兄弟                     |

注：可以用show()和hide()来直接控制元素的可见性。

鼠标移入事件：mouseover、mouseenter，鼠标离开事件：mouseout、mouseleave。

注意：mouseover事件在鼠标移动到选取的元素及其子元素上时触发。

mouseenter事件只在鼠标移动到选取的元素上时触发。

mouseout与mouseleave同理。

注意：可以在$的括号内进行字符串的拼接。

#### class类操作

##### 添加类

addClass

单个元素：

`$('#div1').addClass('fontSize30');`

多个元素：

`$('#div1').addClass('fontSize30 marginTop20');`

##### 移除类

removeClass

单个元素：

`$('#div1').removeClass('fontSize30');`

如果没有fontSize也不会报错。

多个元素：

`$('#div1').removeClass('fontSize30 marginTop20');`

所有元素：

`$('#div1').removeClass();`

##### 判断类

hasClass

`$('#div1').hasClass('fontSize30');`

若div1有这个类则返回true。

##### 切换类

toggleClass

若元素有某个类就移除这个类，反之添加这个类

`$('#div1').toggleClass('fontSize30');`

### 动画

#### 基本动画

##### show()

如果show()没有参数就没有动画效果。

参数1：执行动画的时长，也可以是代表时长的字符串

示例：fast slow normal（200,400,600ms）

如果写错了相当于normal

参数2：动画执行完毕后的回调函数

##### hide()

没有参数就没有动画效果

其余同上。

##### toggle()

隐藏就显示，显示就隐藏。

可以加时间参数。

##### slideDown()

卷帘门向下滑入

参数1：动画执行的时长

参数2：动画执行完毕后的回调函数

无参：相当于给了normal，400ms

##### slideUp()

同上

##### slideToggle()

同上

##### fadeIn()

同上

##### fadeOut()

同上

##### fadeToggle()

同上

##### fadeTo()

淡入到某个状态

`fadeTo(1000,0.5);`

淡入到opcacity为0.5的状态。

#### 自定义动画

animate

参数1：对象，动画的属性

参数2：可选，执行动画的时长

参数3：可选，easing代表缓动还是匀速，linear（匀速），swing（缓动），默认缓动

参数4：回调函数

```js
$('#div1').animate({
    left: 800,
    width: 200,
    height: 200
},2000,'linear');
```

#### 动画队列

在同一个元素上执行多个动画后面的动画会被放到动画队列中，等前面的执行完再执行。

#### 停止动画

stop()

`stop(clearQueue, jumpToEnd);`

参数1：是否清除队列

参数2：是否跳转到最终效果

两个参数均为bool值。不写默认为两个false。

#### 创建元素

html()

**获取内容**

不用传参，会获取到元素的所有内容。

**设置内容**

会覆盖原有的内容，并且把标签解析出来。

`.html('<a href="...">跳转</a>')`

$()

可以创建元素，但是只存在于内存中，在页面上显示的要追加。

```js
// 创建节点
var $link = $('<a></a>');
// 追加节点
$('#div1').append($link);
```

### 结点操作

#### 添加节点

##### append

append()

父元素.append(子元素)，把子元素作为最后一个元素剪切后添加到父元素。

也可以起到剪切作用，把一个元素剪切作为最后一个子元素添加。

也可以把已经存在的元素添加到其他元素内，剪切作为最后一个子元素添加。

##### prepend

prepend()

父元素.prepend(子元素)，把子元素作为第一个元素剪切后添加到父元素。

也可以起到剪切作用，把一个元素剪切作为第一个子元素添加。

也可以把已经存在的元素添加到其他元素内，剪切作为第一个子元素添加。

##### before

before()

元素A.before(元素B)，把元素B插入到元素A的前面，作为兄弟元素添加。

##### after

after()

元素A.after(元素B)，把元素B插入到元素A的后面，作为兄弟元素添加。

##### appendTo

appendTo()

子元素.appendTo(父元素)，把子元素作为父元素的最后一个子元素添加。

#### 清空结点

##### empty()

不推荐使用html("")来清空结点，有可能会造成内存泄漏，不安全。

可以清空元素。

移除一个元素：`remove()// 自杀`

移除ul，但只能获取其中的一个li标签：`parent().remove()`

#### 克隆结点

##### clone()

只存在于内存中，应该放到页面上显示。

参数不管是true还是false都会克隆到后代结点。

true会把事件一起克隆，false则不会，默认为false。

可以用attr()来修改id：`attr('id','div2')`

用append()来追加结点。

#### 设置/获取表单元素

##### val()

不给参数就是获取。

给参数就是设置：`val('设置的值')`

### 属性操作

#### 操作属性

设置属性：attr()

`.attr('src','temp.jpg')`

可以修改原本就有的属性，也可以添加新的属性。

也可以对多个属性操作：

```js
.attr({
    src: '1.jpg',
    aaa: 'aaa',
})
```

#### 获取属性

`.attr('src')`

自带的属性和自定义的属性都可以获取。

如果没有属性则返回undefined。

#### 移除属性

`.removeAttr('src')`

也可以对多个属性操作：

```js
.removeAttr({
    src: '1.jpg',
    aaa: 'aaa',
})
```

#### prop操作布尔类型的属性

对于checkbox的checked等bool类型的属性无论是选中还是没有选中jQuery都会返回undefined，因此不能用attr而要用prop。

```js
.prop('checked')
```

如果选中返回true，反之返回false。

#### 尺寸和位置操作

##### height与width

`.width()`获取宽度

`.height()`获取高度

`.width(300)`设置宽度

`.height(300)`设置高度

注意宽高不包括padding，margin，border。

##### innerWidth与innerHeight

方法返回元素的宽度或者高度（包括内边距）

`.innerWidth()`获取宽度（宽度+padding）

`.innerHeight()`获取高度（高度+padding）

##### outerWidth与outerHeight

方法返回元素的宽度或者高度（包括内边距和边框）

`.outerWidth()`获取宽度（宽度+border+padding）

`.outerHeight()`获取高度（高度+border+padding）

加上参数true可以获得包括内边距、边框、外边距的宽度或者高度。

`.outerWidth(true)`获取宽度（宽度+border+padding+margin）

`.outerHeight(true)`获取高度（高度+border+padding+margin）

##### 获取页面可视区的宽高

`$(window).width()`获取可视区宽度

`$(window).height()`获取可视区高度

#### offset和position

```js
.offset()
```

获取一个包括left和top的对象

此处left是元素左侧距离document左侧的距离，top是元素顶部距离document顶部的距离。

```js
.position()
```

获取一个包括left和top的对象

此处left是元素左侧距离有定位的父元素（offsetParent）的距离，top是元素顶部距离有定位的父元素（offsetParent）的距离。

#### scrollTop和scrollLeft

```js
.scrollLeft()
```

```js
.scrollTop()
```

获取元素被卷曲的宽度与高度。

```js
.scrollLeft(100)
```

```js
.scrollTop(100)
```

设置元素被卷曲的宽度与高度。

```js
$(window).scrollLeft()
```

```js
$(window).scrollTop()
```

获取页面被卷曲的宽度与高度。

设置直接加参数即可。

### 事件

#### 事件注册方式on

#### on简单注册事件

```js
$('div').on('click',function () {
    console.log('aaa')
})
```

注意：不支持动态注册。

#### on委托注册事件

```js
$('body').on('click','div,span',function () {
    console.log('aaa')
})
```

注意：此处要把触发事件的div委托给父结点body来监听事件。

支持动态注册。

原理：在事件冒泡过程中对父元素操作。

#### 事件解绑off(）

```js
.off()
```

解绑元素的所有事件。

```js
.off('click')
```

解绑元素的所有click事件。

#### 事件触发trigger(）

```js
$('div').trigger('click')
```

用代码触发元素div的单击事件。

因而可以用于触发自定义事件。

#### 事件对象

用事件参数e获取。

常用的三个坐标：screenX/Y，clientX/Y，pageX/Y

screenX/Y表示屏幕最左上角距离触发事件的位置。

clientX/Y表示可视区距离触发事件的位置。（忽视滚动条）

pageX/Y表示页面左上角距离触发事件的位置。

`e.stopPropagation()`阻止事件冒泡。（即可以触发孩子但不触发到父亲）

`e.preventDefault()`阻止浏览器默认行为。（即可以阻止a标签的跳转）

`return false`既可以阻止事件冒泡，也可以阻止默认行为。

`e.keyCode()`可以获取键盘按下的是哪个键。一般绑定在整个页面上。

```js
$('document').on('keydown', function (e) {
    console.log(e.keyCode)
})
```

### 链式编程

如果给元素调用一个方法并且这个方法有返回值且为一个jQuery对象，则可继续点出jQuery方法。

必须是jQuery对象才能点出jQuery方法。

需要注意返回的对象是不是需要进行操作的方法。

`.end()`可以用于回到上一个状态。

### each方法

用`$lis`代表页面上ul内的10个li

```js
$lis.each(function (index, element) {
    // index代表每一个li的索引
    // element代表每一个li标签，是一个dom对象
    $(element).css('opacity', index/10)
})
```

each用于遍历每一个对象，并执行一个函数。

用于给每一个对象设置不同的值。

若设置相同的值则可运用隐式迭代。

### 多库共存

`jQuery.fn.jquery`或者`jQuery.prototype.jquery` 或者`$.fn.jquery`或者`$.prototype.jquery`获取jQuery的版本。

若同时引入多个jQuery文件哪个文件后引入，使用的$就是哪个文件的。

`$.noConflict()`把$的控制权释放，此后使用jQuery去点就是下一个文件，$去点就是上一个文件。

也可以用一个变量`_$`来接收被释放的$，代表后一个文件。

```js
(function ($) {
    // 在自执行函数中可以继续使用$
}(_$))
```

如果前面的代码不想把$全部改为_$则可使用此自执行函数包裹所有代码。

若有多个库共存则可以一层一层释放$。

### 基本使用

可以对一个元素进行多重赋值：`$('div').width(100).height(100).text('abc')`（链式编程）

