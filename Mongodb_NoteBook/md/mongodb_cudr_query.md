# 查询文档

指定文档查询有2种方式，效果都是一样的
```
db.Player.find()
db.getCollection('Player').find()
```

## 查询所有文档

关键命令：`.find()`

演示：
```
> db.my_collection_fix.find()
{ "_id" : ObjectId("5c93420ea0bdd2c56dbbf4cf"), "name" : "mongodb" }
{ "_id" : ObjectId("5c934249a0bdd2c56dbbf4d0"), "name" : "mongodb" }
{ "_id" : ObjectId("5c93424ca0bdd2c56dbbf4d1"), "name" : "mongodb" }
{ "_id" : ObjectId("5c93424da0bdd2c56dbbf4d2"), "name" : "mongodb" }
{ "_id" : ObjectId("5c93424ea0bdd2c56dbbf4d3"), "name" : "mongodb" }
{ "_id" : ObjectId("5c93424fa0bdd2c56dbbf4d4"), "name" : "mongodb" }
```

## 查询一个文档

关键命令：`.findOne()`

演示：
```
> db.my_collection_fix.findOne()
{ "_id" : ObjectId("5c93420ea0bdd2c56dbbf4cf"), "name" : "mongodb" }
```

## 查询返回指定字段

关键命令：`.find({}, {"_id":0})`

默认是显示结果所有字段的，所以单独设置一个字段显示是无效的，需要设置不显示的字段：
```
> db.my_collection_fix.find({}, {"_id":0})
{ "name" : "mongodb" }
{ "name" : "mongodb" }
{ "name" : "mongodb" }
{ "name" : "mongodb" }
{ "name" : "mongodb" }
{ "name" : "mongodb" }
> db.my_collection_fix.find({}, {"name":1})
{ "_id" : ObjectId("5c93420ea0bdd2c56dbbf4cf"), "name" : "mongodb" }
{ "_id" : ObjectId("5c934249a0bdd2c56dbbf4d0"), "name" : "mongodb" }
{ "_id" : ObjectId("5c93424ca0bdd2c56dbbf4d1"), "name" : "mongodb" }
{ "_id" : ObjectId("5c93424da0bdd2c56dbbf4d2"), "name" : "mongodb" }
{ "_id" : ObjectId("5c93424ea0bdd2c56dbbf4d3"), "name" : "mongodb" }
{ "_id" : ObjectId("5c93424fa0bdd2c56dbbf4d4"), "name" : "mongodb" }
```

## 条件查询

关键命令：`.find({key1:value1})`

准备数据：
```
db.authors.insertMany([{"author":"一一","gender":"女","age":"20"},{"author":"二郎","gender":"男","age":"20"},{"author":"张三","gender":"男","age":"18"},{"author":"李四","gender":"男","age":"21"},{"author":"五百","gender":"男","age":"17"}])
```

### 条件操作符

| 操作 | 格式 | 范例 | 对应 SQL |
| :---- | :-- | :-- | :-- |
| 等于 | `{<key>:<value>}` | db.shengxiao.find({"shengxiao":"猪"}) | where shengxiao = '猪' |
| 小于 | `{<key>:{$lt:<value>}}` | db.shengxiao.find({"nian":{$lt:1930}}) | where nian < 1930 |
| 小于等于 | `{<key>:{$lte:<value>}}` | db.shengxiao.find({"nian":{$lte:1930}}) | where nian <= 1930 |
| 大于 | `{<key>:{$gt:<value>}}` | db.shengxiao.find({"nian":{$gt:2000}}) | where nian > 2000 |
| 大于等于 | `{<key>:{$gte:<value>}}` | db.shengxiao.find({"nian":{$gte:2000}}) | where nian >= 2000 |
| 不等于 | `{<key>:{$ne:<value>}}` | db.shengxiao.find({"nian":{$ne:2019}}) | where nian != 2019 |
| 在N和M之间 | `{<key>:{$gte:<value>,$lte:<value>}}` | db.shengxiao.find({"nian":{$gte:2000,$lte:2010}}) | where nian >= 2000 and nian <= 2010> |

### AND 条件查询

多条件就使用多个字段，多字段之间用逗号隔开，关键命令：`.find({<key>:<value>, <key>:<value>})`

不同字段之间使用 AND 条件
```
> db.shengxiao.find({"shengxiao":"猪", "nian":2019})
{ "_id" : ObjectId("5c937beca0bdd2c56dbbf534"), "shengxiao" : "猪", "nian" : 2019 }
```

相同字段之间使用 AND 条件
```
> db.shengxiao.find({"nian":{$gte:2000,$lte:2010}})
{ "_id" : ObjectId("5c937beca0bdd2c56dbbf4dc"), "shengxiao" : "鼠", "nian" : 2008 }
{ "_id" : ObjectId("5c937beca0bdd2c56dbbf4e4"), "shengxiao" : "牛", "nian" : 2009 }
{ "_id" : ObjectId("5c937beca0bdd2c56dbbf4ec"), "shengxiao" : "虎", "nian" : 2010 }
{ "_id" : ObjectId("5c937beca0bdd2c56dbbf4fb"), "shengxiao" : "龙", "nian" : 2000 }
{ "_id" : ObjectId("5c937beca0bdd2c56dbbf503"), "shengxiao" : "蛇", "nian" : 2001 }
{ "_id" : ObjectId("5c937beca0bdd2c56dbbf50b"), "shengxiao" : "马", "nian" : 2002 }
{ "_id" : ObjectId("5c937beca0bdd2c56dbbf513"), "shengxiao" : "羊", "nian" : 2003 }
{ "_id" : ObjectId("5c937beca0bdd2c56dbbf51b"), "shengxiao" : "猴", "nian" : 2004 }
{ "_id" : ObjectId("5c937beca0bdd2c56dbbf523"), "shengxiao" : "鸡", "nian" : 2005 }
{ "_id" : ObjectId("5c937beca0bdd2c56dbbf52b"), "shengxiao" : "狗", "nian" : 2006 }
{ "_id" : ObjectId("5c937beca0bdd2c56dbbf533"), "shengxiao" : "猪", "nian" : 2007 }
```

### OR 条件查询

使用 `$or` 关键字：
```
> db.shengxiao.find({$or: [{"nian":2018},{"nian":2019}]})
{ "_id" : ObjectId("5c937beca0bdd2c56dbbf52c"), "shengxiao" : "狗", "nian" : 2018 }
{ "_id" : ObjectId("5c937beca0bdd2c56dbbf534"), "shengxiao" : "猪", "nian" : 2019 }
```

### AND 和 OR 组合查询

```
> db.shengxiao.find({"nian":{$gt:2000}, $or:[{"shengxiao":"猪"},{"shengxiao":"狗"}]})
{ "_id" : ObjectId("5c937beca0bdd2c56dbbf52b"), "shengxiao" : "狗", "nian" : 2006 }
{ "_id" : ObjectId("5c937beca0bdd2c56dbbf52c"), "shengxiao" : "狗", "nian" : 2018 }
{ "_id" : ObjectId("5c937beca0bdd2c56dbbf533"), "shengxiao" : "猪", "nian" : 2007 }
{ "_id" : ObjectId("5c937beca0bdd2c56dbbf534"), "shengxiao" : "猪", "nian" : 2019 }

```
类似于 `where nian > 2000 AND (shengxiao = '猪' OR shengxiao = '狗')`




























## Insert 插入

关键命令：`.insertOne()` 、`.insertMany()`

插入一个文档 `db.collection.insertOne()`

插入多个文档 `db.collection.insertMany()`

插入示例
```shell
db.collection.insertMany([
   { item: "journal", qty: 25, size: { h: 14, w: 21, uom: "cm" }, status: "A" },
   { item: "notebook", qty: 50, size: { h: 8.5, w: 11, uom: "in" }, status: "A" },
   { item: "paper", qty: 100, size: { h: 8.5, w: 11, uom: "in" }, status: "D" },
   { item: "planner", qty: 75, size: { h: 22.85, w: 30, uom: "cm" }, status: "D" },
   { item: "postcard", qty: 45, size: { h: 10, w: 15.25, uom: "cm" }, status: "A" }
]);
```

## Update 更新

关键命令：`.updateOne()` 、`.updateMany()`

更新单个文档 `db.collection.updateOne()`

更新多个文档 `db.collection.updateMany()`

## Replace 替换修改

关键命令：`.replaceOne()`

替换文档 `db.collection.replaceOne()`

