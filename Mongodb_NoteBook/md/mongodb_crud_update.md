# 更新操作

```
> db.my_collection.insertMany([{"name":"一一","gender":"女","age":20},{"name":"二郎","gender":"男","age":20},{"name":"张三","gender":"男","age":18},{"name":"李四","gender":"男","age":21},{"name":"五百","gender":"男","age":17}])
{
	"acknowledged" : true,
	"insertedIds" : [
		ObjectId("5c944222a0bdd2c56dbbf53f"),
		ObjectId("5c944222a0bdd2c56dbbf540"),
		ObjectId("5c944222a0bdd2c56dbbf541"),
		ObjectId("5c944222a0bdd2c56dbbf542"),
		ObjectId("5c944222a0bdd2c56dbbf543")
	]
}
> db.my_collection.find()
{ "_id" : ObjectId("5c944222a0bdd2c56dbbf53f"), "name" : "一一", "gender" : "女", "age" : 20 }
{ "_id" : ObjectId("5c944222a0bdd2c56dbbf540"), "name" : "二郎", "gender" : "男", "age" : 20 }
{ "_id" : ObjectId("5c944222a0bdd2c56dbbf541"), "name" : "张三", "gender" : "男", "age" : 18 }
{ "_id" : ObjectId("5c944222a0bdd2c56dbbf542"), "name" : "李四", "gender" : "男", "age" : 21 }
{ "_id" : ObjectId("5c944222a0bdd2c56dbbf543"), "name" : "五百", "gender" : "男", "age" : 17 }
```

## 字段更新操作符

| 操作符 | 用法 | 说明 |
| :---- | :-- | :-- |
| $inc | `{$inc:{field:value}}` | 用来增加已有键的值，或者在键不存在时创建一个键。inc就是专门来增加（和减少）数字的。"$inc"只能用于整数、长整数或双精度浮点数。要是用在其他类型的数据上就会导致操作失败。 |
| $rename || 重命名字段名称，新的字段名称不能和文档中现有的字段名相同。如果文档中存在和新字段名相同的字段，会将文档中原来的字段和值移除掉，然后再重命名字段。 |
| $set | `{$set:{field:value}}` | 指定一个键的值。如果这个键不存在，则创建它。 |
| $unset | `{$unset:{field:1}}` | 从文档中移除指定的键。 |
| $push | `{$push:{field:value}}` | 把value追加到field里。field只能是数组类型，如果field不存在，会自动插入一个数组类型 |
| $pushAll | `{$pushAll:{field:value_array}}` | 用法同$push一样，只是$pushAll可以一次追加多个值到一个数组字段内。 |
| $pop | 删除数组内第一个值：{$pop:{field:-1}}、删除数组内最后一个值：{$pop:{field:1}} | 删除数组内的一个值 |
| $pull | `{$pull:{field:_value}}` | 从数组field内删除一个等于_value的值 |
| $pullAll | `{$pullAll:value_array}` | 用法同$pull一样，可以一次性删除数组内的多个值。 |
| $setOnInsert | | |
| $min | | |
| $max | |  |
| $mul | | |
| [$currentDate](#currentDate-demo) | `{$currentDate:{<field1>:{$type:<DateType>}, ... }}` | 设置字段的值为当前时间，值为Date类型或者Timestamp时间戳类型，默认是Date类型 |



## update() 方法

语法格式：
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

```
> db.my_collection.find({"name":"李四"})
{ "_id" : ObjectId("5c938454a0bdd2c56dbbf538"), "name" : "李四", "gender" : "男", "age" : "21" }
> db.my_collection.update({"name":"李四"},{$set:{"age":"23"}})
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
> db.my_collection.find({"name":"李四"})
{ "_id" : ObjectId("5c938454a0bdd2c56dbbf538"), "name" : "李四", "gender" : "男", "age" : "23" }
```

```
> db.my_collection.find()
{ "_id" : ObjectId("5c944222a0bdd2c56dbbf53f"), "name" : "一一", "gender" : "女", "age" : 20 }
{ "_id" : ObjectId("5c944222a0bdd2c56dbbf540"), "name" : "二郎", "gender" : "男", "age" : 20 }
{ "_id" : ObjectId("5c944222a0bdd2c56dbbf541"), "name" : "张三", "gender" : "男", "age" : 18 }
{ "_id" : ObjectId("5c944222a0bdd2c56dbbf542"), "name" : "李四", "gender" : "男", "age" : 21 }
{ "_id" : ObjectId("5c944222a0bdd2c56dbbf543"), "name" : "五百", "gender" : "男", "age" : 17 }
> db.my_collection.update({"gender":"男"},{$inc:{"age":1}})
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
> db.my_collection.find()
{ "_id" : ObjectId("5c944222a0bdd2c56dbbf53f"), "name" : "一一", "gender" : "女", "age" : 20 }
{ "_id" : ObjectId("5c944222a0bdd2c56dbbf540"), "name" : "二郎", "gender" : "男", "age" : 21 }
{ "_id" : ObjectId("5c944222a0bdd2c56dbbf541"), "name" : "张三", "gender" : "男", "age" : 18 }
{ "_id" : ObjectId("5c944222a0bdd2c56dbbf542"), "name" : "李四", "gender" : "男", "age" : 21 }
{ "_id" : ObjectId("5c944222a0bdd2c56dbbf543"), "name" : "五百", "gender" : "男", "age" : 17 }
```

```
> db.my_collection.find()
{ "_id" : ObjectId("5c944222a0bdd2c56dbbf53f"), "name" : "一一", "gender" : "女", "age" : 20 }
{ "_id" : ObjectId("5c944222a0bdd2c56dbbf540"), "name" : "二郎", "gender" : "男", "age" : 21 }
{ "_id" : ObjectId("5c944222a0bdd2c56dbbf541"), "name" : "张三", "gender" : "男", "age" : 18 }
{ "_id" : ObjectId("5c944222a0bdd2c56dbbf542"), "name" : "李四", "gender" : "男", "age" : 21 }
{ "_id" : ObjectId("5c944222a0bdd2c56dbbf543"), "name" : "五百", "gender" : "男", "age" : 17 }
> db.my_collection.update({"gender":"男"},{$inc:{"age":1}},{multi:true})
WriteResult({ "nMatched" : 4, "nUpserted" : 0, "nModified" : 4 })
> db.my_collection.find()
{ "_id" : ObjectId("5c944222a0bdd2c56dbbf53f"), "name" : "一一", "gender" : "女", "age" : 20 }
{ "_id" : ObjectId("5c944222a0bdd2c56dbbf540"), "name" : "二郎", "gender" : "男", "age" : 22 }
{ "_id" : ObjectId("5c944222a0bdd2c56dbbf541"), "name" : "张三", "gender" : "男", "age" : 19 }
{ "_id" : ObjectId("5c944222a0bdd2c56dbbf542"), "name" : "李四", "gender" : "男", "age" : 22 }
{ "_id" : ObjectId("5c944222a0bdd2c56dbbf543"), "name" : "五百", "gender" : "男", "age" : 18 }
> db.my_collection.update({"gender":"男"},{$inc:{"age":1}},false,true)
WriteResult({ "nMatched" : 4, "nUpserted" : 0, "nModified" : 4 })
```

## save() 方法

语法格式：
```
db.collection.save(
   <document>,
   {
     writeConcern: <document>
   }
)
```

save() 方法通过传入的文档来替换已有文档，这个传入的文档需要有 _id，如果存在相同 _id 的就进行替换更新，如果不存在该 _id 就进行插入。

实例：
```
> db.my_collection.find({"name":"一一"})
{ "_id" : ObjectId("5c944222a0bdd2c56dbbf53f"), "name" : "一一", "gender" : "女", "age" : 20 }
> db.my_collection.save({"_id":ObjectId("5c944222a0bdd2c56dbbf53f"),"name":"一一","gender":"女","age":22})
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
> db.my_collection.find({"name":"一一"})
{ "_id" : ObjectId("5c944222a0bdd2c56dbbf53f"), "name" : "一一", "gender" : "女", "age" : 22 }
```

## updateOne() 和 updateMany() 方法

updateOne() 和 updateMany() 方法是对应 update() 方法的 multi 是否为假或真。updateOne() 效果等同于 multi:false，updateMany() 效果等同于 multi:true。

与删除操作的 deleteOne()、deleteMany() 对应 remove() 方法是否使用 justOne 参数类似。

updateOne 实例：
```
> db.my_collection.find()
{ "_id" : ObjectId("5c944222a0bdd2c56dbbf53f"), "name" : "一一", "gender" : "女", "age" : 22 }
{ "_id" : ObjectId("5c944222a0bdd2c56dbbf540"), "name" : "二郎", "gender" : "男", "age" : 24 }
{ "_id" : ObjectId("5c944222a0bdd2c56dbbf541"), "name" : "张三", "gender" : "男", "age" : 20 }
{ "_id" : ObjectId("5c944222a0bdd2c56dbbf542"), "name" : "李四", "gender" : "男", "age" : 23 }
{ "_id" : ObjectId("5c944222a0bdd2c56dbbf543"), "name" : "五百", "gender" : "男", "age" : 19 }
> 
> 
> db.my_collection.updateOne({"gender":"男"},{$set:{"age":20}})
{ "acknowledged" : true, "matchedCount" : 1, "modifiedCount" : 1 }
> db.my_collection.find()
{ "_id" : ObjectId("5c944222a0bdd2c56dbbf53f"), "name" : "一一", "gender" : "女", "age" : 22 }
{ "_id" : ObjectId("5c944222a0bdd2c56dbbf540"), "name" : "二郎", "gender" : "男", "age" : 20 }
{ "_id" : ObjectId("5c944222a0bdd2c56dbbf541"), "name" : "张三", "gender" : "男", "age" : 20 }
{ "_id" : ObjectId("5c944222a0bdd2c56dbbf542"), "name" : "李四", "gender" : "男", "age" : 23 }
{ "_id" : ObjectId("5c944222a0bdd2c56dbbf543"), "name" : "五百", "gender" : "男", "age" : 19 }
```

updateMany 实例：
```
> db.my_collection.find()
{ "_id" : ObjectId("5c944222a0bdd2c56dbbf53f"), "name" : "一一", "gender" : "女", "age" : 22 }
{ "_id" : ObjectId("5c944222a0bdd2c56dbbf540"), "name" : "二郎", "gender" : "男", "age" : 20 }
{ "_id" : ObjectId("5c944222a0bdd2c56dbbf541"), "name" : "张三", "gender" : "男", "age" : 20 }
{ "_id" : ObjectId("5c944222a0bdd2c56dbbf542"), "name" : "李四", "gender" : "男", "age" : 23 }
{ "_id" : ObjectId("5c944222a0bdd2c56dbbf543"), "name" : "五百", "gender" : "男", "age" : 19 }
> 
> 
> db.my_collection.updateMany({"gender":"男"},{$set:{"age":20}})
{ "acknowledged" : true, "matchedCount" : 4, "modifiedCount" : 2 }
> db.my_collection.find()
{ "_id" : ObjectId("5c944222a0bdd2c56dbbf53f"), "name" : "一一", "gender" : "女", "age" : 22 }
{ "_id" : ObjectId("5c944222a0bdd2c56dbbf540"), "name" : "二郎", "gender" : "男", "age" : 20 }
{ "_id" : ObjectId("5c944222a0bdd2c56dbbf541"), "name" : "张三", "gender" : "男", "age" : 20 }
{ "_id" : ObjectId("5c944222a0bdd2c56dbbf542"), "name" : "李四", "gender" : "男", "age" : 20 }
{ "_id" : ObjectId("5c944222a0bdd2c56dbbf543"), "name" : "五百", "gender" : "男", "age" : 20 }
```

## replaceOne() 方法

语法格式：
```
db.collection.replaceOne(
   <filter>,
   <replacement>,
   {
     upsert: <boolean>,
     writeConcern: <document>,
     collation: <document>
   }
)
```

跟 updateOne() 方法效果类似，但是替换的文档内容里不能使用更新操作符

## 字段更新操作符实例

### <label id="currentDate-demo">currentDate</label>

```
> db.my_collection.find({"name":"李四"})
{ "_id" : ObjectId("5c94528aa0bdd2c56dbbf547"), "name" : "李四", "gender" : "男", "age" : 21 }
> 
> db.my_collection.update({"name":"李四"},{$currentDate:{"update_dt":{$type:"date"},"update_time":{$type:"timestamp"}}})
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
> db.my_collection.find({"name":"李四"})
{ "_id" : ObjectId("5c94528aa0bdd2c56dbbf547"), "name" : "李四", "gender" : "男", "age" : 21, "update_dt" : ISODate("2019-03-22T03:21:12.575Z"), "update_time" : Timestamp(1553224872, 1) }
> 
```