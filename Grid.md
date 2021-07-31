## Grid布局

### 容器属性

#### display属性

`display: grid`指定一个容器采用网格布局。

也可以设为行内元素：`display: inline-grid`

注意：设为网格布局以后，容器子元素（项目）的`float`、`display: inline-block`、`display: table-cell`、`vertical-align`和`column-*`等设置都将失效。

#### columns属性、rows属性

`grid-template-columns`属性定义每一列的列宽，`grid-template-rows`属性定义每一行的行高。

```css
.container {
  display: grid;
  grid-template-columns: 100px 100px 100px;
  grid-template-rows: 100px 100px 100px;
}
.container {
  display: grid;
  grid-template-columns: 33.33% 33.33% 33.33%;
  grid-template-rows: 33.33% 33.33% 33.33%;
}
```

##### repeat

```css
.container {
  display: grid;
  grid-template-columns: repeat(3, 33.33%);
  grid-template-rows: repeat(3, 33.33%);
}
```

repeat(重复次数，重复内容)

##### autofill

```css
.container {
  display: grid;
  grid-template-columns: repeat(auto-fill, 100px);
}
```

单元格的大小是固定的，但是容器的大小不确定。可使每一行（或每一列）容纳尽可能多的单元格。

上述代码表示每列宽度`100px`，然后自动填充，直到容器不能放置更多的列。

##### fr

如果两列的宽度分别为`1fr`和`2fr`，就表示后者是前者的两倍。可以更简单地表示比例关系。

```css
.container {
  display: grid;
  grid-template-columns: 1fr 1fr;
}
```

上述代码表示两个宽度相同的列。

```css
.container {
  display: grid;
  grid-template-columns: 150px 1fr 2fr;
}
```

##### minmax

该函数产生一个长度范围，表示长度就在这个范围之中。它接受两个参数，分别为最小值和最大值。

```css
grid-template-columns: 1fr 1fr minmax(100px, 1fr);
```

上面代码中，`minmax(100px, 1fr)`表示列宽不小于`100px`，不大于`1fr`。

##### auto

浏览器自己决定长度

```css
grid-template-columns: 100px auto 100px;
```

第二列的宽度基本上等于该列单元格的最大宽度，除非单元格内容设置了`min-width`，且这个值大于最大宽度。

##### 网格线名称

`grid-template-columns`属性和`grid-template-rows`属性里面，还可以使用方括号，指定每一根网格线的名字，方便以后的引用。

```css
.container {
  display: grid;
  grid-template-columns: [c1] 100px [c2] 100px [c3] auto [c4];
  grid-template-rows: [r1] 100px [r2] 100px [r3] auto [r4];
}
```

上面代码指定网格布局为3行 x 3列，因此有4根垂直网格线和4根水平网格线。方括号里面依次是这八根线的名字。

网格布局允许同一根线有多个名字，比如`[fifth-line row-5]`。

##### 示例

比例固定的两栏式布局

```css
.wrapper {
  display: grid;
  grid-template-columns: 70% 30%;
}
```

12网格布局

```css
grid-template-columns: repeat(12, 1fr);
```

#### grid-row-gap 属性， grid-column-gap 属性， grid-gap 属性

##### grid-row-gap

设置行间距

##### grid-column-gap

设置列间距

```css
.container {
  grid-row-gap: 20px;
  grid-column-gap: 20px;
}
```

##### grid-gap

前两者的缩写，语法格式：

```css
grid-gap: <grid-row-gap> <grid-column-gap>;
```

如果`grid-gap`省略了第二个值，浏览器认为第二个值等于第一个值。

根据最新标准，上面三个属性名的`grid-`前缀已经删除，`grid-column-gap`和`grid-row-gap`写成`column-gap`和`row-gap`，`grid-gap`写成`gap`。

#### grid-template-areas 属性

网格布局允许指定"区域"（area），一个区域由单个或多个单元格组成。`grid-template-areas`属性用于定义区域。

```css
.container {
  display: grid;
  grid-template-columns: 100px 100px 100px;
  grid-template-rows: 100px 100px 100px;
  grid-template-areas: 'a b c'
                       'd e f'
                       'g h i';
}
```

上面代码先划分出9个单元格，然后将其定名为`a`到`i`的九个区域，分别对应这九个单元格。

同样地可以合并单元格：

```CSS
grid-template-areas: 'a a a'
                     'b b b'
                     'c c c';
```

将九个单元格分成a、b、c三个区域。

##### 实际应用

```css
grid-template-areas: "header header header"
                     "main main sidebar"
                     "footer footer footer";
```

顶部是页眉区域`header`，底部是页脚区域`footer`，中间部分则为`main`和`sidebar`。

不需要使用的区域用.表示

```css
grid-template-areas: 'a . c'
                     'd . f'
                     'g . i';
```

中间一列为点，表示没有用到该单元格，或者该单元格不属于任何区域。

区域的命名会影响到网格线。每个区域的起始网格线，会自动命名为`区域名-start`，终止网格线自动命名为`区域名-end`。

比如，区域名为`header`，则起始位置的水平网格线和垂直网格线叫做`header-start`，终止位置的水平网格线和垂直网格线叫做`header-end`。

#### grid-auto-flow属性

容器的子元素会按照顺序，自动放置在每一个网格。默认的放置顺序是"先行后列"。

row-先行后列

column-先列后行

row dense-某些项目指定位置以后，剩下的项目先行后列。

column dense-某些项目指定位置以后，剩下的项目先列后行。

#### justify-items 属性， align-items 属性， place-items 属性

##### justify-items属性

设置单元格内容的水平位置（左中右）

##### align-items属性

设置单元格内容的垂直位置（上中下）

```css
.container {
  justify-items: start | end | center | stretch;
  align-items: start | end | center | stretch;
}
```

取值：

```
start：对齐单元格的起始边缘。
end：对齐单元格的结束边缘。
center：单元格内部居中。
stretch：拉伸，占满单元格的整个宽度（默认值）。
```

##### place-items属性

`place-items`属性是`align-items`属性和`justify-items`属性的合并简写形式。

```css
place-items: <align-items> <justify-items>;
```

如果省略第二个值，则浏览器认为与第一个值相等。

#### justify-content 属性， align-content 属性， place-content 属性

##### justify-content 属性

整个内容区域在容器里面的水平位置（左中右）。

##### align-content属性

整个内容区域的垂直位置（上中下）。

```css
.container {
  justify-content: start | end | center | stretch | space-around | space-between | space-evenly;
  align-content: start | end | center | stretch | space-around | space-between | space-evenly;  
}
```

取值：

```
start - 对齐容器的起始边框。
end - 对齐容器的结束边框。
center - 容器内部居中。
stretch - 项目大小没有指定时，拉伸占据整个网格容器。
space-around - 每个项目两侧的间隔相等。所以，项目之间的间隔比项目与容器边框的间隔大一倍。
space-between - 项目与项目的间隔相等，项目与容器边框之间没有间隔。
space-evenly - 项目与项目的间隔相等，项目与容器边框之间也是同样长度的间隔。
```

##### place-content属性

`align-content`属性和`justify-content`属性的合并简写形式。

```css
place-content: <align-content> <justify-content>
```

如果省略第二个值，浏览器就会假定第二个值等于第一个值。

#### grid-auto-columns 属性， grid-auto-rows 属性

`grid-auto-columns`属性和`grid-auto-rows`属性用来设置，浏览器自动创建的多余网格的列宽和行高。它们的写法与`grid-template-columns`和`grid-template-rows`完全相同。如果不指定这两个属性，浏览器完全根据单元格内容的大小，决定新增网格的列宽和行高。

```css
.container {
  display: grid;
  grid-template-columns: 100px 100px 100px;
  grid-template-rows: 100px 100px 100px;
  grid-auto-rows: 50px; 
}
```

8号项目在第四行，9号项目在第五行，各占50px。

#### grid-template 属性， grid 属性

`grid-template`属性是`grid-template-columns`、`grid-template-rows`和`grid-template-areas`这三个属性的合并简写形式。

`grid`属性是`grid-template-rows`、`grid-template-columns`、`grid-template-areas`、 `grid-auto-rows`、`grid-auto-columns`、`grid-auto-flow`这六个属性的合并简写形式。

### 项目属性

#### grid-column-start 属性， grid-column-end 属性， grid-row-start 属性， grid-row-end 属性

```
grid-column-start属性：左边框所在的垂直网格线
grid-column-end属性：右边框所在的垂直网格线
grid-row-start属性：上边框所在的水平网格线
grid-row-end属性：下边框所在的水平网格线
```

```css
.item-1 {
  grid-column-start: 2;
  grid-column-end: 4;
}
```

1号项目的左边框是第二根垂直网格线，右边框是第四根垂直网格线。只指定了1号项目的左右边框，没有指定上下边框，所以会采用默认位置，即上边框是第一根水平网格线，下边框是第二根水平网格线。

```css
.item-1 {
  grid-column-start: 1;
  grid-column-end: 3;
  grid-row-start: 2;
  grid-row-end: 4;
}
```

1号项目在左中部，其余项目围绕1号项目分布。

注意：属性除了可以为第几个网格线还可以是网格线的名字，还可以是`span`，即左右边框（上下边框）之间跨越多少个网格。

```css
.item-1 {
  grid-column-start: span 2;
}
```

1号项目的左边框距离右边框跨越2个网格。

同理，效果类似于

```css
.item-1 {
  grid-column-end: span 2;
}
```

若项目重叠可以使用`z-index`属性指定项目的重叠顺序。

#### grid-column 属性， grid-row 属性

`grid-column`属性是`grid-column-start`和`grid-column-end`的合并简写形式，`grid-row`属性是`grid-row-start`属性和`grid-row-end`的合并简写形式。

```css
.item {
  grid-column: <start-line> / <end-line>;
  grid-row: <start-line> / <end-line>;
}
```

##### 示例

```css
.item-1 {
  grid-column: 1 / 3;
  grid-row: 1 / 2;
}
/* 等同于 */
.item-1 {
  grid-column-start: 1;
  grid-column-end: 3;
  grid-row-start: 1;
  grid-row-end: 2;
}
```

项目`item-1`占据第一行，从第一根列线到第三根列线。

这两个属性之中，也可以使用`span`关键字，表示跨越多少个网格。

```css
.item-1 {
  background: #b03532;
  grid-column: 1 / 3;
  grid-row: 1 / 3;
}
/* 等同于 */
.item-1 {
  background: #b03532;
  grid-column: 1 / span 2;
  grid-row: 1 / span 2;
}
```

项目`item-1`占据的区域，包括第一行 + 第二行、第一列 + 第二列。

斜杠以及后面的部分可以省略，默认跨越一个网格。

```css
.item-1 {
  grid-column: 1;
  grid-row: 1;
}
```

项目`item-1`占据左上角第一个网格。

#### grid-area 属性

指定项目放在哪一个区域。

```css
.item-1 {
  grid-area: e;
}
```

1号项目位于`e`区域。

`grid-area`属性还可用作`grid-row-start`、`grid-column-start`、`grid-row-end`、`grid-column-end`的合并简写形式，直接指定项目的位置。

```css
.item {
  grid-area: <row-start> / <column-start> / <row-end> / <column-end>;
}
```

#### justify-self 属性， align-self 属性， place-self 属性

`justify-self`属性设置单元格内容的水平位置（左中右），跟`justify-items`属性的用法完全一致，但只作用于单个项目。

`align-self`属性设置单元格内容的垂直位置（上中下），跟`align-items`属性的用法完全一致，也是只作用于单个项目。

```css
.item {
  justify-self: start | end | center | stretch;
  align-self: start | end | center | stretch;
}
```

取值：

```css
start：对齐单元格的起始边缘。
end：对齐单元格的结束边缘。
center：单元格内部居中。
stretch：拉伸，占满单元格的整个宽度（默认值）。
```

`place-self`属性是`align-self`属性和`justify-self`属性的合并简写形式。

```css
place-self: <align-self> <justify-self>;
```

如果省略第二个值，`place-self`属性会认为这两个值相等。

