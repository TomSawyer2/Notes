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

### 数据结构

#### 栈

用列表实现，append()加元素，pop()出元素。

#### 队列

也可以用列表实现，但效率不高。

#### 列表推导式

通过已存在的列表并通过一定的规则生成新的列表。

```python
vec = [2, 4, 6]
print([[x, x**2] for x in vec if x < 4])# [[2, 4]]
```

#### 同时读取字典的键值对

```python
knights = {'gallahad': 'the pure', 'robin': 'the brave'}
for k, v in knights.items():
	print(k, v)
```

#### 读取列表的值和索引

```python
for i, v in enumerate(['tic', 'tac', 'toe']):
	print(i, v)
```

#### 同时遍历多个列表

使用zip()：

```python
questions = ['name', 'quest', 'favorite color']
answers = ['lancelot', 'the holy grail', 'blue']
for q, a in zip(questions, answers):
	print('What is your {0}?  It is {1}.'.format(q, a))
```

#### 反向遍历序列

使用reversed(list)。

### 模块

引入模块：`import xxx`

调用外部文件的函数：`外部文件名.函数名(参数)`

导入模块的指定函数：`from 外部文件名 import 函数名`

使用该函数：`函数名(参数)`

导入所有模块只需写成*即可，可以在`__init__.py`中加入`__all__`变量用于控制可导入哪些模块。

```python
__all__ = ["echo", "surround", "reverse"]
```

#### `__name__`属性

可用于判断是模块内部运行还是外部的调用。

```python
if __name__ == '__main__':
   print('程序自身在运行')
else:
   print('我来自另一模块')
```

#### dir()

可以找到模块内定义的所有名称。

#### 包

每个包内都需要`__init__.py`。

导入包内的某个模块只需要一层层.即可。

### 输出

`rjust()`可以将字符串靠右, 并在左边填充空格。

`format()`可以用于格式化字符串：

```python
print('{0} 和 {1}'.format('Google', 'Runoob'))# Google 和 Runoob
print('{name}网址： {site}'.format(name='菜鸟教程', site='www.runoob.com'))# 菜鸟教程网址： www.runoob.com
```

### 读写文件

```python
open(filename, mode)
```

```python
# 打开一个文件
f = open("/tmp/foo.txt", "w")

f.write( "Python 是一个非常好的语言。**\n**是的，的确非常好!!**\n**" )

# 关闭打开的文件
f.close()
```

`f.read()`读取文件内指定数量的内容。

`f.readline()`从文件中读取单独的一行。

`f.readlines()`读取文件中的所有行。

`f.write()`写入文件，返回写入的字符数。

`f.tell()`返回文件对象当前所处的位置。

`f.seek()`改变文件当前的位置。

- seek(x,0) ： 从起始位置即文件首行首字符开始移动 x 个字符
- seek(x,1) ： 表示从当前位置往后移动x个字符
- seek(-x,2)：表示从文件的结尾往前移动x个字符

`f.close()`关闭文件。

### 错误与异常

异常捕捉：

```python
try:
    ...
except (a, b, c):# 在a或b或c异常发生时执行
    ...
except:# 在除了a，b，c三种异常发生异常时执行
    ...
else:# 无任何异常时执行
    ...
finally:# 什么时候都会执行
    ...
```

抛出异常：

```python
raise [Exception [, args [, traceback]]]
```

抛出的必须是异常的类或者实例。

### 面向对象

#### 类

```python
class ClassName:
    ...
```

类有一个名为`__init__()` 的特殊方法，该方法在类实例化时会自动调用。

实例化：`x = ClassName()`

类的内部定义方法时必须有参数self，且作为第一个参数，代表类的实例。

#### 继承

```python
class ClassName(OldClassName):
    ...
```

子类会继承父类的属性和方法。

多继承时从左到右搜索方法名。

#### 方法重写

默认子类方法覆盖父类方法。

`super()`可用于调用父类已被覆盖的方法。

### 作用域

搜索变量名称时会从局部作用域L->闭包函数外的函数E->全局作用域G->内建作用域B。

### 全局变量与局部变量

使用全局（最外层）变量：global

```python
num = 1
def func ():
    global num
    return num# 1
```

使用外一层变量：nonlocal

```python
def func1 ():
    num = 100
    def func2 ():
        nonlocal num
        num = 1
        return
    print(num)# 1
    return
```

### 标准库

读取命令行参数：

```python
import sys
print(sys.argv)
```

### 正则表达式

```python
re.match(pattern, string, flags=0)
```

从字符串的起始位置匹配模式，若起始位置匹配失败则返回none，匹配成功返回一个匹配的对象。

可以用`group(num)`或者`groups()`匹配对象函数获取匹配表达式。

```python
re.search(pattern, string, flags=0)
```

扫描整个字符串并返回第一个成功的匹配，同match函数。

```python
re.sub(pattern, repl, string, count=0, flags=0)
```

- pattern : 正则中的模式字符串。
- repl : 替换的字符串，也可为一个函数。
- string : 要被查找替换的原始字符串。
- count : 模式匹配后替换的最大次数，默认 0 表示替换所有的匹配。
- flags : 编译时用的匹配模式，数字形式。

用于替换字符串。

```python
re.compile(pattern[, flags])
```

编译正则表达式，生成一个正则表达式（ Pattern ）对象，供 match() 和 search() 这两个函数使用。

```python
re.findall(pattern, string, flags=0)
或
pattern.findall(string[, pos[, endpos]])
```

在字符串中找到正则表达式所匹配的所有子串，并返回一个列表，如果没有找到匹配的，则返回空列表。

```python
re.finditer(pattern, string, flags=0)
```

在字符串中找到正则表达式所匹配的所有子串，并把它们作为一个迭代器返回。

```python
re.split(pattern, string[, maxsplit=0, flags=0])
```

按照能够匹配的子串将字符串分割后返回列表。

### MySQL-mysql-connector

安装：`python -m pip install mysql-connector`

导入：`import mysql.connector`

连接数据库：

```python
mydb = mysql.connector.connect(
  host="localhost",       # 数据库主机地址
  user="yourusername",    # 数据库用户名
  passwd="yourpassword"   # 数据库密码
  database="runoob_db"    # 数据库名称（可选）
)
 
print(mydb)
```

创建数据库：

```python
mycursor = mydb.cursor()
mycursor.execute("CREATE DATABASE runoob_db")
```

创建数据表：

```python
mycursor = mydb.cursor()
mycursor.execute("CREATE TABLE sites (name VARCHAR(255), url VARCHAR(255))")
```

插入数据：

```python
mycursor = mydb.cursor()
 
sql = "INSERT INTO sites (name, url) VALUES (%s, %s)"
val = ("RUNOOB", "https://www.runoob.com")
mycursor.execute(sql, val)
 
mydb.commit()    # 数据表内容有更新，必须使用到该语句
 
print(mycursor.rowcount, "记录插入成功。")
```

批量插入：

```python
mycursor = mydb.cursor()
 
sql = "INSERT INTO sites (name, url) VALUES (%s, %s)"
val = [
  ('Google', 'https://www.google.com'),
  ('Github', 'https://www.github.com'),
  ('Taobao', 'https://www.taobao.com'),
  ('stackoverflow', 'https://www.stackoverflow.com/')
]
 
mycursor.executemany(sql, val)
 
mydb.commit()    # 数据表内容有更新，必须使用到该语句
 
print(mycursor.rowcount, "记录插入成功。")
```

查询数据：

```python
mycursor = mydb.cursor()
 
mycursor.execute("SELECT * FROM sites")
 
myresult = mycursor.fetchall()     # fetchall() 获取所有记录
```

其余操作均参照MySQL原生内容即可。

### MySQL-PyMySQL

安装：`$ pip3 install PyMySQL`

连接：

```python
import pymysql
 
# 打开数据库连接
db = pymysql.connect("localhost","testuser","test123","TESTDB" )
 
# 使用 cursor() 方法创建一个游标对象 cursor
cursor = db.cursor()
```

操作：

```python
try:
   # 执行SQL语句
   cursor.execute(sql)
   # 提交到数据库执行
   db.commit()
except:
   # 发生错误时回滚
   db.rollback()
 
# 关闭数据库连接
db.close()
```

- **fetchone():** 该方法获取下一个查询结果集。结果集是一个对象
- **fetchall():** 接收全部的返回结果行.
- **rowcount:** 这是一个只读属性，并返回执行execute()方法后影响的行数。

### Socket

```python
socket.socket([family[, type[, proto]]])
```

创建socket。

### SMTP发送邮件

引入：

```python
import smtplib
smtpObj = smtplib.SMTP( [host [, port [, local_hostname]]] )
```

发送邮件：

```python
SMTP.sendmail(from_addr, to_addrs, msg[, mail_options, rcpt_options]
```

注意此处的msg需要遵循SMTP协议。

### 多线程

创建新线程：

```python
_thread.start_new_thread ( function, args[, kwargs] )
```

可以通过直接从 threading.Thread 继承创建一个新的子类，并实例化后调用 start() 方法启动新线程，即它调用了线程的 run() 方法。

线程同步：使用锁来控制线程的运行。

线程优先级队列：使用队列控制不同线程。

### XML解析

使用xml.dom解析xml。

```python
from xml.dom.minidom import parse
import xml.dom.minidom

# 使用minidom解析器打开 XML 文档
DOMTree = xml.dom.minidom.parse("movies.xml")
collection = DOMTree.documentElement
```

之后就可以`collection.getElementByTagName()`等等。

### JSON解析

引入：`import json`

字典转JSON：`json = json.dumps(data)`

JSON转字典：`dict = json.loads(json)`

### MongoDB

安装：`$ python3 -m pip3 install pymongo`

更新：`$ python3 -m pip3 install --upgrade pymongo`

连接数据库runoobdb：

```python
import pymongo

myclient = pymongo.MongoClient("mongodb://localhost:27017/")
mydb = myclient["runoobdb"]
```

创建表：

```python
mycol = mydb["sites"]
```

插入单个文档：

```python
mydict = { "name": "RUNOOB", "alexa": "10000", "url": "https://www.runoob.com" }
x = mycol.insert_one(mydict) 
```

插入多个文档：

```python
mylist = [
  { "name": "Taobao", "alexa": "100", "url": "https://www.taobao.com" },
  { "name": "QQ", "alexa": "101", "url": "https://www.qq.com" },
  { "name": "Facebook", "alexa": "10", "url": "https://www.facebook.com" },
  { "name": "知乎", "alexa": "103", "url": "https://www.zhihu.com" },
  { "name": "Github", "alexa": "109", "url": "https://www.github.com" }
]
 
x = mycol.insert_many(mylist)
```

查询单个数据：

```python
x = mycol.find_one()
```

查询所有数据：

```python
for x in mycol.find():
  print(x)
```

查询指定条件的数据：

```python
for x in mycol.find({},{ "_id": 0, "name": 1, "alexa": 1 }):
  print(x)
```

0表示不要返回，1表示要返回。

修改数据：

```python
myquery = { "name": { "$regex": "^F" } }
newvalues = { "$set": { "alexa": "123" } }
 
x = mycol.update_many(myquery, newvalues)
```

排序：

```python
mydoc = mycol.find().sort("alexa", -1)
```

删除数据：

```python
myquery = { "name": "Taobao" }
mycol.delete_one(myquery)
```

其余操作均跟原生MongoDB相似。

### urllib

#### urllib.request

用于模拟浏览器的请求。

```python
urllib.request.urlopen(url, data=None, [timeout, ]*, cafile=None, capath=None, context=None)
```

- **url**：url 地址。
- **data**：发送到服务器的其他数据对象，默认为 None。
- **timeout**：设置访问超时时间。
- **cafile 和 capath**：cafile 为 CA 证书， capath 为 CA 证书的路径，使用 HTTPS 需要用到。
- **context**：ssl.SSLContext类型，用来指定 SSL 设置。

读取内容后用`read()`获取html代码。

用`getcode()`获取状态码：

```python
except urllib.error.HTTPError as e:
    if e.code == 404:
        print(404)   # 404
```

模拟头部信息：

```python
class urllib.request.Request(url, data=None, headers={}, origin_req_host=None, unverifiable=False, method=None)
```

- **url**：url 地址。
- **data**：发送到服务器的其他数据对象，默认为 None。
- **headers**：HTTP 请求的头部信息，字典格式。
- **origin_req_host**：请求的主机地址，IP 或域名。
- **unverifiable**：很少用整个参数，用于设置网页是否需要验证，默认是False。。
- **method**：请求方法， 如 GET、POST、DELETE、PUT等。

完整的请求过程：

```python
url_all = url+key_code
header = {
    'User-Agent':'Mozilla/5.0 (X11; Fedora; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36'
}   #头部信息
request = urllib.request.Request(url_all,headers=header)
reponse = urllib.request.urlopen(request).read()
```

POST请求加上data：

```python
url = 'https://www.runoob.com/try/py3/py3_urllib_test.php'  # 提交到表单页面
data = {'name':'RUNOOB', 'tag' : '菜鸟教程'}   # 提交数据
header = {
    'User-Agent':'Mozilla/5.0 (X11; Fedora; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36'
}   #头部信息
data = urllib.parse.urlencode(data).encode('utf8')  # 对参数进行编码，解码使用 urllib.parse.urldecode
request=urllib.request.Request(url, data, header)   # 请求处理
reponse=urllib.request.urlopen(request).read()      # 读取结果
```

#### urllib.parse

```python
urllib.parse.urlparse(urlstring, scheme='', allow_fragments=True)
```

用于解析url。

### urllib.robotparser

用于解析robots.txt。

```python
import urllib.robotparser
rp = urllib.robotparser.RobotFileParser()
rp.set_url("http://www.musi-cal.com/robots.txt")
rp.read()
rp.request_rate("*").requests # 3
```

### Python Web

安装uWSGI：`pip install uwsgi`

Django等等框架之后再说。

### pip

python的npm。

# Python基础部分学习结束！
