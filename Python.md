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

#### 数字

`/`返回浮点数，`//`返回整数。

#### 字符串

1. 字符串从前面索引时从0开始，从后面索引时从-1开始；从前面截取时从1开始，从后面截取时从-1开始。
2. `*`可以表示重复输出字符串几次。
3. 字符串不能改变，不能强行向一个位置赋值。

#### 列表

基本上跟字符串一样，放在中括号内，可以理解为数组，元素可变。

#### 元组

跟列表相似，但元素不可修改，放在小括号内，元素的种类可以不同。

创建空元组以及一个元素的元组：

```python
tup = ()
tup2 = (1,)
```

#### 集合

集合的元素大小不一。

可以用{}或者set()来创建集合，但不能用{}来创建空集合。

#### 字典

字典是无序的对象集合，通过键值对来存储，是一种映射类型。

注意：字典的关键字必须为不可变类型，且不能重复。

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


