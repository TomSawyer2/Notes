## MongoDB

### 简介

MongoDB由C++编写，将数据存储为一个文档，数据结构由键值对组成。

### 运行

`D:\mongodb\bin\mongo.exe`

### 基本概念

MongoDB的默认数据库为"db"，该数据库存储在data目录中。

`show dbs`显示所以数据库。

`db`显示当前数据库对象或集合。

`use xxx`连接到xxx数据库。

- **admin**： 从权限的角度来看，这是"root"数据库。要是将一个用户添加到这个数据库，这个用户自动继承所有数据库的权限。一些特定的服务器端命令也只能从这个数据库运行，比如列出所有的数据库或者关闭服务器。
- **local:** 这个数据永远不会被复制，可以用来存储限于本地单台服务器的任意集合
- **config**: 当Mongo用于分片设置时，config数据库在内部使用，用于保存分片的相关信息。

1. 文档中的键/值对是有序的。
2. 文档中的值不仅可以是在双引号里面的字符串，还可以是其他几种数据类型（甚至可以是整个嵌入的文档)。
3. MongoDB区分类型和大小写。
4. MongoDB的文档不能有重复的键。
5. 文档的键是字符串。除了少数例外情况，键可以使用任意UTF-8字符。

#### 数据类型

##### ObjectId

类似唯一主键，可以很快的去生成和排序，包含 12 bytes，含义是：

- 前 4 个字节表示创建 **unix** 时间戳,格林尼治时间 **UTC** 时间，比北京时间晚了 8 个小时
- 接下来的 3 个字节是机器标识码
- 紧接的两个字节由进程 id 组成 PID
- 最后三个字节是随机数

可以通过 getTimestamp 函数来获取文档的创建时间。

##### 字符串

均为UTF-8编码。

##### 时间戳

- 前32位是一个 time_t 值（与Unix新纪元相差的秒数）
- 后32位是在某秒中操作的一个递增的`序数`

##### 日期

表示当前距离 Unix新纪元（1970年1月1日）的毫秒数。日期类型是有符号的, 负数表示 1970 年之前的日期。

### 连接

标准 URI 连接语法：

```
mongodb://[username:password@]host1[:port1][,host2[:port2],...[,hostN[:portN]]][/[database][?options]]
```

- **mongodb://** 这是固定的格式，必须要指定。
- **username:password@** 可选项，如果设置，在连接数据库服务器之后，驱动都会尝试登录这个数据库
- **host1** 必须的指定至少一个host, host1 是这个URI唯一要填写的。它指定了要连接服务器的地址。如果要连接复制集，请指定多个主机地址。
- **portX** 可选的指定端口，如果不填，默认为27017
- **/database** 如果指定username:password@，连接并验证登录指定数据库。若不指定，默认打开 test 数据库。
- **?options** 是连接选项。如果不使用/database，则前面需要加上/。所有连接选项都是键值对name=value，键值对之间通过&或;（分号）隔开。

使用用户 admin 使用密码 123456 连接到本地的 MongoDB 服务上。输出结果如下所示：

```
> mongodb://admin:123456@localhost/
```

### 创建数据库

`use xxx`创建名为xxx的数据库，若存在则切换至此数据库。

若想通过`show dbs`找到xxx数据库，则需要：

```
db.runoob.insert({"name":"菜鸟教程"})
WriteResult({ "nInserted" : 1 })
```

之后就会出现在`show dbs`中。

### 删除数据库

`db.dropDatabase()`删除当前数据库，默认为test。

### 创建集合

`db.createCollection("xxx",options)`

options 可以是如下参数：

| 字段   | 类型 | 描述                                                         |
| :----- | :--- | :----------------------------------------------------------- |
| capped | 布尔 | （可选）如果为 true，则创建固定集合。固定集合是指有着固定大小的集合，当达到最大值时，它会自动覆盖最早的文档。 **当该值为 true 时，必须指定 size 参数。** |
| size   | 数值 | （可选）为固定集合指定一个最大值，即字节数。 **如果 capped 为 true，也需要指定该字段。** |
| max    | 数值 | （可选）指定固定集合中包含文档的最大数量。                   |

示例：`db.createCollection("mycol", { capped : true, autoIndexId : true, size : 6142800, max : 10000 } )`

`show tables`以及`show collections`可用于查看数据库中的集合，集合相当于表。

### 删除集合

`db.collection.drop()`删除名为collection的集合。

### 插入文档

`db.xxx.insert()`若主键存在则报错，提示主键重复并不保存当前数据。

`db.xxx.insertOne()`若主键存在则更新数据，反之插入数据。

```bash
db.collection.insertOne(
   <document>,
   {
      writeConcern: <document>
   }
)
```

`db.collection.insertMany()`用于向集合插入一个多个文档：

```
db.collection.insertMany(
   [ <document 1> , <document 2>, ... ],
   {
      writeConcern: <document>,
      ordered: <boolean>
   }
)
```

- document：要写入的文档。
- writeConcern：写入策略，默认为 1，即要求确认写操作，0 是不要求。
- ordered：指定是否按顺序写入，默认 true，按顺序写入。

查看已经插入的文档：`db.xxx.find()`

可以将查询结果保存为变量。

### 更新文档

#### update

```
db.collection.update(
   <query>,
   <update>,
   {
     upsert: <boolean>,
     multi: <boolean>,
     writeConcern: <document>
   }
)
```

**参数说明：**

- **query** : update的查询条件。
- **update** : update的对象和一些更新的操作符（如$,$inc...）等。
- **upsert** : 可选，这个参数的意思是，如果不存在update的记录，是否插入objNew,true为插入，默认是false，不插入。
- **multi** : 可选，mongodb 默认是false,只更新找到的第一条记录，如果这个参数为true,就把按条件查出来多条记录全部更新。
- **writeConcern** :可选，抛出异常的级别。

将所有title为MongoDB 教程的记录title更新为MongoDB：

```
db.col.update({'title':'MongoDB 教程'},{$set:{'title':'MongoDB'}},{multi:true})
```

#### save

替换已有文档，_id 主键存在就更新，不存在就插入。

```
db.collection.save(
   <document>,
   {
     writeConcern: <document>
   }
)
```

- **document** : 文档数据。
- **writeConcern** :可选，抛出异常的级别。

### 删除文档

```
db.collection.remove(
   <query>,
   {
     justOne: <boolean>,
     writeConcern: <document>
   }
)
```

- **query** :（可选）删除的文档的条件。
- **justOne** : （可选）如果设为 true 或 1，则只删除一个文档，如果不设置该参数，或使用默认值 false，则删除所有匹配条件的文档。
- **writeConcern** :（可选）抛出异常的级别。

删除所有数据：`db.xxx.remove({})`

### 查询文档

```
db.collection.find(query, projection)
```

- **query** ：可选，使用查询操作符指定查询条件
- **projection** ：可选，使用投影操作符指定返回的键。查询时返回文档中所有键值， 只需省略该参数即可（默认省略）。

可以加上`.pretty()`增加可读性。

`findOne()`方法只返回一个文档。

#### AND条件

传入多个key，每个key用逗号隔开。

```
>db.col.find({key1:value1, key2:value2}).pretty()
```

#### OR条件

```
>db.col.find(
   {
      $or: [
         {key1: value1}, {key2:value2}
      ]
   }
).pretty()
```

### 条件操作符

