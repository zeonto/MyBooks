# 插入文档

## insert() 方法

语法格式：
```
db.collection.insert(
   <document or array of documents>,
   {
     writeConcern: <document>,
     ordered: <boolean>
   }
)
```

insert() 方法可以插入单条或多条数据。

插入单条实例：
```
> db.my_collection.insert({"name":"一一","gender":"女","age":20})
WriteResult({ "nInserted" : 1 })
> db.my_collection.find()
{ "_id" : ObjectId("5c945cd4a0bdd2c56dbbf549"), "name" : "一一", "gender" : "女", "age" : 20 }
```

插入多条实例：
```
> db.my_collection.find()
> db.my_collection.insert([{"name":"一一","gender":"女","age":20},{"name":"二郎","gender":"男","age":20},{"name":"张三","gender":"男","age":18},{"name":"李四","gender":"男","age":21},{"name":"五百","gender":"男","age":17}])
BulkWriteResult({
	"writeErrors" : [ ],
	"writeConcernErrors" : [ ],
	"nInserted" : 5,
	"nUpserted" : 0,
	"nMatched" : 0,
	"nModified" : 0,
	"nRemoved" : 0,
	"upserted" : [ ]
})
> db.my_collection.find()
{ "_id" : ObjectId("5c945cf2a0bdd2c56dbbf54a"), "name" : "一一", "gender" : "女", "age" : 20 }
{ "_id" : ObjectId("5c945cf2a0bdd2c56dbbf54b"), "name" : "二郎", "gender" : "男", "age" : 20 }
{ "_id" : ObjectId("5c945cf2a0bdd2c56dbbf54c"), "name" : "张三", "gender" : "男", "age" : 18 }
{ "_id" : ObjectId("5c945cf2a0bdd2c56dbbf54d"), "name" : "李四", "gender" : "男", "age" : 21 }
{ "_id" : ObjectId("5c945cf2a0bdd2c56dbbf54e"), "name" : "五百", "gender" : "男", "age" : 17 }
```

## insertOne()、insertMany() 方法

insertOne 实例：
```
> db.my_collection.find()
> db.my_collection.insertOne({"name":"一一","gender":"女","age":20});
{
	"acknowledged" : true,
	"insertedId" : ObjectId("5c945da4a0bdd2c56dbbf550")
}
> db.my_collection.find()
{ "_id" : ObjectId("5c945da4a0bdd2c56dbbf550"), "name" : "一一", "gender" : "女", "age" : 20 }
```

insertMany 实例：
```
> db.my_collection.find()
> db.my_collection.insertMany([{"name":"一一","gender":"女","age":20},{"name":"二郎","gender":"男","age":20},{"name":"张三","gender":"男","age":18},{"name":"李四","gender":"男","age":21},{"name":"五百","gender":"男","age":17}]);
{
	"acknowledged" : true,
	"insertedIds" : [
		ObjectId("5c945dbca0bdd2c56dbbf551"),
		ObjectId("5c945dbca0bdd2c56dbbf552"),
		ObjectId("5c945dbca0bdd2c56dbbf553"),
		ObjectId("5c945dbca0bdd2c56dbbf554"),
		ObjectId("5c945dbca0bdd2c56dbbf555")
	]
}
```

## save() 方法

save() 方法本来是用来替换文档的，因为不存在数据就会创建数据，所以也可以用来插入数据处理。

实例：
```
> db.my_collection.save({"name":"六六","gender":"女","age":22})
WriteResult({ "nInserted" : 1 })
> db.my_collection.find()
{ "_id" : ObjectId("5c945dbca0bdd2c56dbbf551"), "name" : "一一", "gender" : "女", "age" : 20 }
{ "_id" : ObjectId("5c945dbca0bdd2c56dbbf552"), "name" : "二郎", "gender" : "男", "age" : 20 }
{ "_id" : ObjectId("5c945dbca0bdd2c56dbbf553"), "name" : "张三", "gender" : "男", "age" : 18 }
{ "_id" : ObjectId("5c945dbca0bdd2c56dbbf554"), "name" : "李四", "gender" : "男", "age" : 21 }
{ "_id" : ObjectId("5c945dbca0bdd2c56dbbf555"), "name" : "五百", "gender" : "男", "age" : 17 }
{ "_id" : ObjectId("5c945ee3a0bdd2c56dbbf556"), "name" : "六六", "gender" : "女", "age" : 22 }
```