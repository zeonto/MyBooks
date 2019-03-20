## Query 查询

关键命令：`.find()`

查询所有
```
db.Player.find()
db.getCollection('Player').find()
db.getCollection('Player').find({})
```

条件查询
```
db.collection.find({key:value})
db.getCollection('Player').find({playerId:15494})
db.getCollection('Player').find({platform:'test', account:'gj8'})
```

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

## Delete 删除

关键命令：`.deleteMany()`

删除所有文档 `db.collection.deleteMany()`