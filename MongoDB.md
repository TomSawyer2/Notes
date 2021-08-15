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

- (>) 大于 - $gt
- (<) 小于 - $lt
- (>=) 大于等于 - $gte
- (<= ) 小于等于 - $lte

`db.xxx.find({likes : {$gt : 100}})`likes大于100的文档。

`db.col.find({likes : {$lt :200, $gt : 100}})`likes大于100小于200的文档。

### $type操作符

用于匹配固定的数据类型。

| **类型**                | **数字** | **备注**         |
| :---------------------- | :------- | :--------------- |
| Double                  | 1        |                  |
| String                  | 2        |                  |
| Object                  | 3        |                  |
| Array                   | 4        |                  |
| Binary data             | 5        |                  |
| Undefined               | 6        | 已废弃。         |
| Object id               | 7        |                  |
| Boolean                 | 8        |                  |
| Date                    | 9        |                  |
| Null                    | 10       |                  |
| Regular Expression      | 11       |                  |
| JavaScript              | 13       |                  |
| Symbol                  | 14       |                  |
| JavaScript (with scope) | 15       |                  |
| 32-bit integer          | 16       |                  |
| Timestamp               | 17       |                  |
| 64-bit integer          | 18       |                  |
| Min key                 | 255      | Query with `-1`. |
| Max key                 | 127      |                  |

可以用类型名或者数字进行匹配。

```
db.col.find({"title" : {$type : 2}})
db.col.find({"title" : {$type : 'string'}})
```

### Limit()

可以指定读取的文档数量。

`db.xxx.find(...).limit(2)`只搜索两项。

### Skip()

可以指定跳过文档的数量。

`db.xxx.find(...).skip(1)`跳过搜索第一项。

### 排序

`db.xxx.find(...).sort({KEY:1})`升序

`db.xxx.find(...).sort({KEY:-1})`降序

### 索引

创建索引：`db.xxx.createIndex(keys, options)`

Key 值为要创建的索引字段，1升序-1降序，可以有多个索引字段。

```
db.col.createIndex({"title":1})
```

### 聚合

用于处理数据。

#### aggregate()

`db.xxx.aggregate(options)`

示例：`db.xxx.aggregate([{$group : {_id : "$by_user", num_tutorial : {$sum : 1}}}])`

| 表达式    | 描述                                           | 实例                                                         |
| :-------- | :--------------------------------------------- | :----------------------------------------------------------- |
| $sum      | 计算总和。                                     | db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$sum : "$likes"}}}]) |
| $avg      | 计算平均值                                     | db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$avg : "$likes"}}}]) |
| $min      | 获取集合中所有文档对应值得最小值。             | db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$min : "$likes"}}}]) |
| $max      | 获取集合中所有文档对应值得最大值。             | db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$max : "$likes"}}}]) |
| $push     | 在结果文档中插入值到一个数组中。               | db.mycol.aggregate([{$group : {_id : "$by_user", url : {$push: "$url"}}}]) |
| $addToSet | 在结果文档中插入值到一个数组中，但不创建副本。 | db.mycol.aggregate([{$group : {_id : "$by_user", url : {$addToSet : "$url"}}}]) |
| $first    | 根据资源文档的排序获取第一个文档数据。         | db.mycol.aggregate([{$group : {_id : "$by_user", first_url : {$first : "$url"}}}]) |
| $last     | 根据资源文档的排序获取最后一个文档数据         | db.mycol.aggregate([{$group : {_id : "$by_user", last_url : {$last : "$url"}}}]) |

#### 管道

将当前命令的输出作为下一个命令的参数。

- $project：修改输入文档的结构。可以用来重命名、增加或删除域，也可以用于创建计算结果以及嵌套文档。
- $match：用于过滤数据，只输出符合条件的文档。$match使用MongoDB的标准查询操作。
- $limit：用来限制MongoDB聚合管道返回的文档数。
- $skip：在聚合管道中跳过指定数量的文档，并返回余下的文档。
- $unwind：将文档中的某一个数组类型字段拆分成多条，每条包含数组中的一个值。
- $group：将集合中的文档分组，可用于统计结果。
- $sort：将输入文档排序后输出。
- $geoNear：输出接近某一地理位置的有序文档。

```
db.articles.aggregate( [
                        { $match : { score : { $gt : 70, $lte : 90 } } },
                        { $group: { _id: null, count: { $sum: 1 } } }
                       ] );
```

$match用于获取分数大于70小于或等于90记录，然后将符合条件的记录送到下一阶段$group管道操作符进行处理。

### 复制

启动实例：

```
mongod --port "PORT" --dbpath "YOUR_DB_DATA_PATH" --replSet "REPLICA_SET_INSTANCE_NAME"
```

启动副本集：`rs.initiate()`

添加副本集成员：`rs.add(HOST_NAME:PORT)`

判断是否为主节点：`db.isMaster()`

主从关系在主机宕机后所有服务停止，但副本集中副本会接管主节点称为主节点。

### 分片

将大量数据分别储存在多台机器上。

1. 启动Shard Server。
2. 启动Config Server。
3. 启动Route Process。
4. 配置Sharding。
5. 正常连接数据库。

### 备份与恢复

#### 备份

```
mongodump -h dbhost -d dbname -o dbdirectory
```

- -h：

  MongoDB 所在服务器地址。

- -d：

  需要备份的数据库实例，例如：test

- -o：

  备份的数据存放位置，例如：c:\data\dump，当然该目录需要提前建立，在备份完成后，系统自动在dump目录下建立一个test目录，这个目录里面存放该数据库实例的备份数据。

#### 恢复

```
mongorestore -h <hostname><:port> -d dbname <path>
```

- --host <:port>, -h <:port>：

  MongoDB所在服务器地址，默认为： localhost:27017

- --db , -d ：

  需要恢复的数据库实例，例如：test，当然这个名称也可以和备份时候的不一样，比如test2

- --drop：

  恢复的时候，先删除当前数据，然后恢复备份的数据。

- <path>：

  设置备份数据所在位置，例如：c:\data\dump\test。

  不能同时指定 <path> 和 --dir 选项。

- --dir：

  指定备份的目录。

### 监控

`mongostat`定时输出MongoDB的运行状态。

`mongotop`跟踪MongoDB实例，提供每个集合水平的统计数据。

### NodeJS使用MongoDB

安装：`npm install mongodb`

#### 创建数据库

```js
var MongoClient = require('mongodb').MongoClient;
var url = "mongodb://localhost:27017/runoob";
 
MongoClient.connect(url, function(err, db) {
  if (err) throw err;
  console.log("数据库已创建!");
  db.close();
});
```

#### 创建集合

```js
var MongoClient = require('mongodb').MongoClient;
var url = 'mongodb://localhost:27017/runoob';
MongoClient.connect(url, function (err, db) {
    if (err) throw err;
    console.log('数据库已创建');
    var dbase = db.db("runoob");
    dbase.createCollection('site', function (err, res) {
        if (err) throw err;
        console.log("创建集合!");
        db.close();
    });
});
```

#### 插入数据

```js
var MongoClient = require('mongodb').MongoClient;
var url = "mongodb://localhost:27017/";
 
MongoClient.connect(url, function(err, db) {
    if (err) throw err;
    var dbo = db.db("runoob");
    var myobj = { name: "菜鸟教程", url: "www.runoob" };
    dbo.collection("site").insertOne(myobj, function(err, res) {
        if (err) throw err;
        console.log("文档插入成功");
        db.close();
    });
});
```

#### 查询数据

```js
var MongoClient = require('mongodb').MongoClient;
var url = "mongodb://localhost:27017/";
 
MongoClient.connect(url, function(err, db) {
    if (err) throw err;
    var dbo = db.db("runoob");
    dbo.collection("site"). find({}).toArray(function(err, result) { // 返回集合中所有数据
        if (err) throw err;
        console.log(result);
        db.close();
    });
});
```

#### 更新数据

```js
var MongoClient = require('mongodb').MongoClient;
var url = "mongodb://localhost:27017/";
 
MongoClient.connect(url, function(err, db) {
    if (err) throw err;
    var dbo = db.db("runoob");
    var whereStr = {"name":'菜鸟教程'};  // 查询条件
    var updateStr = {$set: { "url" : "https://www.runoob.com" }};
    dbo.collection("site").updateOne(whereStr, updateStr, function(err, res) {
        if (err) throw err;
        console.log("文档更新成功");
        db.close();
    });
});
```

#### 删除数据

```js
var MongoClient = require('mongodb').MongoClient;
var url = "mongodb://localhost:27017/";
 
MongoClient.connect(url, function(err, db) {
    if (err) throw err;
    var dbo = db.db("runoob");
    var whereStr = {"name":'菜鸟教程'};  // 查询条件
    dbo.collection("site").deleteOne(whereStr, function(err, obj) {
        if (err) throw err;
        console.log("文档删除成功");
        db.close();
    });
});
```

#### 排序

```js
var MongoClient = require('mongodb').MongoClient;
var url = "mongodb://localhost:27017/";
 
MongoClient.connect(url, function(err, db) {
    if (err) throw err;
    var dbo = db.db("runoob");
    var mysort = { type: 1 };
    dbo.collection("site").find().sort(mysort).toArray(function(err, result) {
        if (err) throw err;
        console.log(result);
        db.close();
    });
});
```

#### 查询分页

limit()和skip()配合使用。

#### 连接

使用$lookup实现左连接。

#### 删除集合

`drop()`

#### 使用异步函数同时进行多个操作

```js
const MongoClient = require("mongodb").MongoClient;
const url = "mongodb://localhost/";
 
async function dataOperate() {
    var conn = null;
    try {
        conn = await MongoClient.connect(url);
        console.log("数据库已连接");
        const test = conn.db("testdb").collection("test");
        // 增加
        await test.insertOne({ "site": "runoob.com" });
        // 查询
        var arr = await test.find().toArray();
        console.log(arr);
        // 更改
        await test.updateMany({ "site": "runoob.com" },
            { $set: { "site": "example.com" } });
        // 查询
        arr = await test.find().toArray();
        console.log(arr);
        // 删除
        await test.deleteMany({ "site": "example.com" });
        // 查询
        arr = await test.find().toArray();
        console.log(arr);
    } catch (err) {
        console.log("错误：" + err.message);
    } finally {
        if (conn != null) conn.close();
    }
}
 
dataOperate();
```

### 关系

表示多个文档之间在逻辑上的相互联系。

文档间可以通过嵌入和引用来建立联系。

#### 嵌入式关系

在一个文档内部以数组形式嵌入多个文档。

#### 引用式关系

把用户数据文档和用户地址数据文档分开，通过引用文档的 **id** 字段来建立关系。

### 数据库引用

#### DBRefs

```
{ $ref : , $id : , $db :  }
```

- $ref：集合名称
- $id：引用的id
- $db:数据库名称，可选参数

#### 手动引用

手动引用。

### 覆盖索引查询

- 所有的查询字段是索引的一部分
- 所有的查询返回字段在同一个索引中

指定find的第二个参数后，查询时不会在数据库文件中查找而是从索引中提取数据。

所有索引字段是一个数组或一个子文档时不能用覆盖索引查询。

### 查询分析

#### explain()

在查询语句末尾加上`.explain()`

#### hint()

可以使用 hint 来强制 MongoDB 使用一个指定的索引。

### 原子操作

#### $set

用来指定一个键并更新键值，若键不存在并创建。

```
{ $set : { field : value } }
```

#### $unset

用来删除一个键。

```
{ $unset : { field : 1} }
```

#### $inc

$inc可以对文档的某个值为数字型（只能为满足要求的数字）的键进行增减的操作。

```
{ $inc : { field : value } }
```

#### $push

把value追加到field里面去，field一定要是数组类型才行，如果field不存在，会新增一个数组类型加进去。

```
{ $push : { field : value } }
```

#### $pushAll

同$push,只是一次可以追加多个值到一个数组字段内。

```
{ $pushAll : { field : value_array } }
```

#### $pull

从数组field内删除一个等于value值。

```
{ $pull : { field : _value } }
```

#### $addToSet

增加一个值到数组内，而且只有当这个值不在数组内才增加。

#### $pop

删除数组的第一个或最后一个元素

```
{ $pop : { field : 1 } }
```

#### $rename

修改字段名称

```
{ $rename : { old_field_name : new_field_name } }
```

#### $bit

位操作，integer类型

```
{$bit : { field : {and : 5}}}
```

### 高级索引

可以对文档的子文档建立索引。

### 索引限制

如果很少对集合进行读取操作，建议不使用索引。

确保索引的大小不超过内存的限制。

索引不能被以下的查询使用：

- 正则表达式及非操作符，如 $nin, $not, 等。
- 算术运算符，如 $mod, 等。
- $where 子句

最大范围：

- 集合中索引不能超过64个
- 索引名的长度不能超过128个字符
- 一个复合索引最多可以有31个字段

### ObjectId

ObjectId 是一个12字节 BSON 类型数据，有以下格式：

- 前4个字节表示时间戳
- 接下来的3个字节是机器标识码
- 紧接的两个字节由进程id组成（PID）
- 最后三个字节是随机数。

### Map Reduce

将大量数据分块处理并合并。

```
>db.collection.mapReduce(
   function() {emit(key,value);},  //map 函数
   function(key,values) {return reduceFunction},   //reduce 函数
   {
      out: collection,
      query: document,
      sort: document,
      limit: number
   }
)
```

- **map** ：映射函数 (生成键值对序列,作为 reduce 函数参数)。
- **reduce** 统计函数，reduce函数的任务就是将key-values变成key-value，也就是把values数组变成一个单一的值value。。
- **out** 统计结果存放集合 (不指定则使用临时集合,在客户端断开后自动删除)。
- **query** 一个筛选条件，只有满足条件的文档才会调用map函数。（query。limit，sort可以随意组合）
- **sort** 和limit结合的sort排序参数（也是在发往map函数前给文档排序），可以优化分组机制
- **limit** 发往map函数的文档数量的上限（要是没有limit，单独使用sort的用处不大）

输出结果：

- result：储存结果的collection的名字，是个临时集合。
- timeMillis：执行花费的时间
- input：满足条件被发送到map函数的文档个数
- emit：在map函数中emit被调用的次数，也就是所有集合中的数据总量
- ouput：结果集合中的文档个数

### 全文检索

#### 启动全文检索

```
>db.adminCommand({setParameter:true,textSearchEnabled:true})
```

或者

```
mongod --setParameter textSearchEnabled=true
```

#### 创建全文索引

```
db.posts.ensureIndex({post_text:"text"})
```

#### 使用全文索引

```
db.posts.find({$text:{$search:"runoob"}})
```

#### 删除全文索引

获得索引名：

```
db.posts.getIndexes()
```

删除：

```
db.posts.dropIndex("post_text_text")
```

### 正则表达式

使用正则表达式查找包含 runoob 字符串的文章：

```
>db.posts.find({post_text:{$regex:"runoob"}})
```

或者

```
>db.posts.find({post_text:/runoob/})
```

不区分大小写：

```
>db.posts.find({post_text:{$regex:"runoob",$options:"$i"}})
```

也可以在数组字段中使用正则表达式来查找内容。

正则表达式中使用变量时一定要使用eval将组合的字符串进行转换，不能直接将字符串拼接后传入给表达式：

```
var name=eval("/" + 变量值key +"/i"); 
```

### GridFS

用于储存或者恢复超过16M的文件（BSON文件限制16M），会分块储存。

#### 添加文件

```
>mongofiles.exe -d gridfs put song.mp3
```

**-d gridfs** 指定存储文件的数据库名称，如果不存在该数据库，MongoDB会自动创建。如果不存在该数据库，MongoDB会自动创建。

#### 查看数据库中文件

```
>db.fs.files.find()
```

### 固定集合

环形队列。

#### 创建固定集合

```
>db.createCollection("cappedLogCollection",{capped:true,size:10000})
```

#### 判断是否为固定集合

```
>db.cappedLogCollection.isCapped()
```

#### 将已经存在的集合转换为固定集合

```
>db.runCommand({"convertToCapped":"posts",size:10000})
```

#### 特点

可以插入及更新，但更新不能超出collection的大小，否则更新失败。不允许删除，但是可以调用drop()删除集合中的所有行，但是drop后需要显式地重建集合。

- 对固定集合进行插入速度极快
- 按照插入顺序的查询输出速度极快
- 能够在插入最新数据时,淘汰最早的数据

用法：1. 储存日志信息 2. 缓存一些少量的文档

### 自动增长

MongoDB没有自动增长的功能，但可以通过js控制update来实现_id的自动增长。

# MongoDB学习结束！
