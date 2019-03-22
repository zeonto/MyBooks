# 库和集合文档

## 库操作

### 查看所有库

命令：`show dbs`

```shell
> show dbs
admin   0.000GB
config  0.000GB
local   0.000GB
```

### 查看当前库

命令：`db`

```
> db
test
```

### 切换库

命令：`use 库名`

```
> use local
switched to db local
> db
local
```

### 查看库状态

命令：`db.stats()`

```
> db.stats()
{
	"db" : "test",
	"collections" : 0,
	"views" : 0,
	"objects" : 0,
	"avgObjSize" : 0,
	"dataSize" : 0,
	"storageSize" : 0,
	"numExtents" : 0,
	"indexes" : 0,
	"indexSize" : 0,
	"fileSize" : 0,
	"fsUsedSize" : 0,
	"fsTotalSize" : 0,
	"ok" : 1
}
```

### 创建库

命令：`use newdb1`

切换库如果库不存在就是创建新库并切换到新库里面，由于新创建的库没有数据，所以在查看所有库的时候列表里不会显示。
```
> show dbs
admin   0.000GB
config  0.000GB
local   0.000GB
> use newdb1
switched to db newdb1
> db
newdb1
> show dbs
admin   0.000GB
config  0.000GB
local   0.000GB
> 
```

如果往里面插入了数据，查看库列表时就能看到这个库了。
```
> use newdb1
switched to db newdb1
> db
newdb1
> show dbs
admin   0.000GB
config  0.000GB
local   0.000GB
> db.newdb1.insert({"db_name":"newdb1 for demo"})
WriteResult({ "nInserted" : 1 })
> show dbs
admin   0.000GB
config  0.000GB
local   0.000GB
newdb1  0.000GB
```

### 删除库

命令：`db.dropDatabase()`

删除库需要进入到指定库，执行命令删除当前库
```
> show dbs
admin   0.000GB
config  0.000GB
local   0.000GB
newdb1  0.000GB
> db
newdb1
> db.dropDatabase()
{ "dropped" : "newdb1", "ok" : 1 }
> show dbs
admin   0.000GB
config  0.000GB
local   0.000GB
```

### 库版本

命令：`db.version()`

```
> db.version()
4.0.4
```


## 集合操作

### 显示所有集合

命令：`show collections` or `show tables`

```
> use local
switched to db local
> show collections
startup_log
```

### 创建集合

命令：`db.createCollection(name, options)`

| 字段 | 类型 | 必选项 | 说明 |
| :---- | :-- | :-- | :-- |
| capped | bool | 否 | 是否创建固定集合。固定集合是指有着固定大小的集合，当达到最大值时，它会自动覆盖最早的文档。当该值为 true 时，必须指定 size 参数。|
| autoIndexId | bool | 否 | 是否自动在 _id 字段创建索引。默认为 false。|
| size | int | 否 | 为固定集合指定一个最大值（以字节KB计）。如果 capped 为 true，必须指定该字段。|
| max | int | 否 | 指定固定集合中包含文档的最大数量。|

创建普通集合
```
> db
learning
> show collections
> db.createCollection("my_collection")
{ "ok" : 1 }
> show collections
my_collection
```

在 mongodb 中也可以不事先创建集合，往一个不存在的集合插入文档就会创建该集合：
```
> use learning
switched to db learning
> show collections
my_collection
> db.my_collection_tmp.insert({"name":"mongodb"})
WriteResult({ "nInserted" : 1 })
> show collections
my_collection
my_collection_tmp
```

```
> db.my_collection_tmp.find()
{ "_id" : ObjectId("5c930b73a0bdd2c56dbbf4c7"), "name" : "mongodb" }
> db.my_collection_tmp.count()
1
> db.my_collection_tmp.dataSize()
40
> db.my_collection_tmp.storageSize()
16384
```
上面测试1条记录大小是40，假设设置文档固定大小200，只能插入5条。创建一个大小200，最大记录条数10条的文档：
`db.createCollection("my_collection_fix", {capped : true, autoIndexId : true, size : 200, max : 10})`

插入5条记录：
```
db.my_collection_fix.insertMany([{"name":"mongodb"},{"name":"mongodb"},{"name":"mongodb"},{"name":"mongodb"},{"name":"mongodb"}])
```

```
> show collections
my_collection
my_collection_tmp
> db.createCollection("my_collection_fix", {capped : true, autoIndexId : true, size : 200, max : 10})
{
	"note" : "the autoIndexId option is deprecated and will be removed in a future release",
	"ok" : 1
}
> db.my_collection_fix.insertMany([{"name":"mongodb"},{"name":"mongodb"},{"name":"mongodb"},{"name":"mongodb"},{"name":"mongodb"}])
{
	"acknowledged" : true,
	"insertedIds" : [
		ObjectId("5c934089a0bdd2c56dbbf4ca"),
        ObjectId("5c9340d5a0bdd2c56dbbf4cb"),
		ObjectId("5c9340d5a0bdd2c56dbbf4cc"),
		ObjectId("5c9340d5a0bdd2c56dbbf4cd"),
		ObjectId("5c9340d5a0bdd2c56dbbf4ce")
	]
}
> db.my_collection_fix.dataSize()
200
> db.my_collection_fix.count()
5
```
现在固定文档已经有5条记录，大小是200，接着继续插入数据看看效果：
```
> db.my_collection_fix.find()
{ "_id" : ObjectId("5c934089a0bdd2c56dbbf4ca"), "name" : "mongodb" }
{ "_id" : ObjectId("5c9340d5a0bdd2c56dbbf4cb"), "name" : "mongodb" }
{ "_id" : ObjectId("5c9340d5a0bdd2c56dbbf4cc"), "name" : "mongodb" }
{ "_id" : ObjectId("5c9340d5a0bdd2c56dbbf4cd"), "name" : "mongodb" }
{ "_id" : ObjectId("5c9340d5a0bdd2c56dbbf4ce"), "name" : "mongodb" }
{ "_id" : ObjectId("5c93420ea0bdd2c56dbbf4cf"), "name" : "mongodb" }
> db.my_collection_fix.insert({"name":"mongodb"})
WriteResult({ "nInserted" : 1 })
> db.my_collection_fix.insert({"name":"mongodb"})
WriteResult({ "nInserted" : 1 })
> db.my_collection_fix.insert({"name":"mongodb"})
WriteResult({ "nInserted" : 1 })
> db.my_collection_fix.insert({"name":"mongodb"})
WriteResult({ "nInserted" : 1 })
> db.my_collection_fix.insert({"name":"mongodb"})
WriteResult({ "nInserted" : 1 })
> db.my_collection_fix.find()
{ "_id" : ObjectId("5c93420ea0bdd2c56dbbf4cf"), "name" : "mongodb" }
{ "_id" : ObjectId("5c934249a0bdd2c56dbbf4d0"), "name" : "mongodb" }
{ "_id" : ObjectId("5c93424ca0bdd2c56dbbf4d1"), "name" : "mongodb" }
{ "_id" : ObjectId("5c93424da0bdd2c56dbbf4d2"), "name" : "mongodb" }
{ "_id" : ObjectId("5c93424ea0bdd2c56dbbf4d3"), "name" : "mongodb" }
{ "_id" : ObjectId("5c93424fa0bdd2c56dbbf4d4"), "name" : "mongodb" }
```
在继续插入数据前ID最后的是4cf，连续插入5条数据后再看记录，最后的数据ID是4d4，前面5条旧的数据已经被覆盖了。

### 删除集合

命令：`db.collection.drop()`

```
> show collections
my_collection
my_collection_fix
my_collection_tmp
> db.my_collection_tmp.drop()
true
> show collections
my_collection
my_collection_fix
```