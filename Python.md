## Python

### 基本语法

#### 输出

`print("test",end="")`紧跟下一个print时不会换行。

#### 输入

`input()`

#### 注释

```python
# 注释

'''
注释
'''

"""
注释
“”“
```

#### 多行语句

用`\`表示连接下一行：

```python
total = one + \
		two + \
    	three
```

#### 字符串

用r取消转义：

```python
print(r'\n') # \n
```

截取字符串：

```python
str = '123456'

print(str[2:5]) #345
print(str[0:5:2]) #135
```

`a:b:c`a表示起始位置-1，b表示结束位置，c表示步长。

### 数据类型

- **不可变数据：**Number（数字）、String（字符串）、Tuple（元组）；
- **可变数据：**List（列表）、Dictionary（字典）、Set（集合）。

`type(xxx)`可以查看xxx变量的类型。

#### 类型转换

| 函数          | 描述                                                |
| :------------ | :-------------------------------------------------- |
| int(x)        | 将x转换为一个整数                                   |
| float(x)      | 将x转换到一个浮点数                                 |
| complex(real) | 创建一个复数                                        |
| str(x)        | 将对象 x 转换为字符串                               |
| repr(x)       | 将对象 x 转换为表达式字符串                         |
| eval(str)     | 用来计算在字符串中的有效Python表达式,并返回一个对象 |
| tuple(s)      | 将序列 s 转换为一个元组                             |
| list(s)       | 将序列 s 转换为一个列表                             |
| set(s)        | 转换为可变集合                                      |
| dict(d)       | 创建一个字典。d 必须是一个 (key, value)元组序列。   |
| frozenset(s)  | 转换为不可变集合                                    |
| chr(x)        | 将一个整数转换为一个字符                            |
| ord(x)        | 将一个字符转换为它的整数值                          |
| hex(x)        | 将一个整数转换为一个十六进制字符串                  |
| oct(x)        | 将一个整数转换为一个八进制字符串                    |

### 运算符

`:=`海象运算符，可以在表达式内为变量赋值。

`is`判断两个标识符是不是引用于一个对象。

### 数字

`/`返回浮点数，`//`返回整数。

可以用`**`进行幂运算。

### 字符串

1. 字符串从前面索引时从0开始，从后面索引时从-1开始；从前面截取时从1开始，从后面截取时从-1开始。
2. `*`可以表示重复输出字符串几次。
3. 字符串不能改变，不能强行向一个位置赋值。

传统格式化字符串用法：

```python
print ("我叫 %s 今年 %d 岁!" %('小明', 10))
```

新型格式化字符串用法：

```python
x = 1
print(f'{x}')
```

将要转换的变量用大括号套起来并作为字符串加上f。

三引号允许字符串跨越多行：

```python
str = """这是
一个
字符串
"""
```

### 列表

列表内元素的类型可以不一样，元素可变。

列表可以通过`append()`添加元素。

列表可以通过`del xxx`删除元素。

### 元组

跟列表相似，但元素不可修改，放在小括号内，元素的种类可以不同。

创建空元组以及一个元素的元组：

```python
tup = ()
tup2 = (1,)
```

创建时不需要小括号也可以。

### 字典

字典是无序的对象集合，通过键值对来存储，是一种映射类型。

注意：字典的关键字必须为不可变类型，且不能重复。

格式：

```python
dict = {'name': 'runoob', 'likes': 123, 'url': 'www.runoob.com'}
```

使用方式：

```python
dict['name']
```

可以直接对字典的元素进行赋值来更新元素或添加元素。

删除元素：`del dict['name']`

清空字典：`dict.clear()`

删除字典：`del dict`

特性：1. 对同一个键多次赋值取最后一次的值。

2. 键不可变，因此可用数字、字符串或者元组。

### 集合

集合（set）是一个无序的不重复元素序列。

可以用{}或者set()来创建集合，但不能用{}来创建空集合。

集合内部自动去重，且支持交并等等操作。

添加元素：`s.add(x)`或者`s.update(x)`

移除元素：`s.remove(x)`或者`s.discard(x)`

随机删除一个元素：`s.pop()`

清空集合：`s.clear()`

### 条件语句

```python
if condition_1:
    statement_block_1
elif condition_2:
    statement_block_2
else:
    statement_block_3
```

### 循环语句

```python
while 判断条件(condition)：
    执行语句(statements)……
else:
    <additional_statement(s)>
```

```python
for <variable> in <sequence>:
    <statements>
else:
    <statements>
```

range()函数可以生成数列。

pass语句是占位语句，没有任何作用。

### 迭代器

可以从集合的第一个元素开始访问直到结束，只前进不后退。

字符串，列表或元组对象都可用于创建迭代器。

```python
list = [1, 2, 3, 4]
item = iter(list) #创建迭代器
print(item)
print(next(item)) #1
print(next(item)) #2
```

### 生成器

就是一个迭代器。

遇到 yield 时函数会暂停并保存当前所有的运行信息，返回 yield 的值, 并在下一次执行 next() 方法时从当前位置继续运行。

### 函数

```python
def 函数名（参数列表）:
    函数体
```

#### 关键词参数

在传参时可以使用关键词参数：

```python
printinfo( age=50, name="runoob" )
```

这样传入顺序不影响函数运行。

#### 默认参数

在参数列表可以指定默认参数：

```python
def printinfo( name, age = 35 ):
    return
```

#### 不定长参数

```python
def printinfo( arg1, *vartuple ):
   "打印任何传入的参数"
   print ("输出: ")
   print (arg1)
   print (vartuple)
```

*号参数传入后会变为一个元组处理。

**号参数传入后会变为一个字典处理。

注意：*可以单独作为一个参数出现，但其后的参数必须用关键字传入。

```python
def f(a,b,*,c):
	return a+b+c
f(1,2,c=3)
```

#### 匿名函数

```python
lambda [arg1 [,arg2,.....argn]]:expression
```

```python
sum = lambda arg1, arg2: arg1 + arg2
```

sum可以当做普通函数使用。

#### 强制位置参数

在以下的例子中，形参 a 和 b 必须使用指定位置参数，c 或 d 可以是位置形参或关键字形参，而 e 和 f 要求为关键字形参:

```python
def f(a, b, /, c, d, *, e, f):
    print(a, b, c, d, e, f)
```

