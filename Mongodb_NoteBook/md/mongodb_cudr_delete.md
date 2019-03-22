# 删除文档数据

## 准备数据

```
> db.my_collection.insertMany([{"name":"一一","gender":"女","age":"20"},{"name":"二郎","gender":"男","age":"20"},{"name":"张三","gender":"男","age":"18"},{"name":"李四","gender":"男","age":"21"},{"name":"五百","gender":"男","age":"17"}])
{
	"acknowledged" : true,
	"insertedIds" : [
		ObjectId("5c938454a0bdd2c56dbbf535"),
		ObjectId("5c938454a0bdd2c56dbbf536"),
		ObjectId("5c938454a0bdd2c56dbbf537"),
		ObjectId("5c938454a0bdd2c56dbbf538"),
		ObjectId("5c938454a0bdd2c56dbbf539")
	]
}
> db.my_collection.find()
{ "_id" : ObjectId("5c938454a0bdd2c56dbbf535"), "name" : "一一", "gender" : "女", "age" : "20" }
{ "_id" : ObjectId("5c938454a0bdd2c56dbbf536"), "name" : "二郎", "gender" : "男", "age" : "20" }
{ "_id" : ObjectId("5c938454a0bdd2c56dbbf537"), "name" : "张三", "gender" : "男", "age" : "18" }
{ "_id" : ObjectId("5c938454a0bdd2c56dbbf538"), "name" : "李四", "gender" : "男", "age" : "21" }
{ "_id" : ObjectId("5c938454a0bdd2c56dbbf539"), "name" : "五百", "gender" : "男", "age" : "17" }
> 
```

## remove 删除数据

语法格式:
```
db.collection.remove(
   <query>,
   {
     justOne: <boolean>,
     writeConcern: <document>
   }
)
```

### 删除一条符合条件的数据
```
> db.my_collection.find()
{ "_id" : ObjectId("5c93dc211c1954f62d7ff580"), "name" : "一一", "gender" : "女", "age" : "20" }
{ "_id" : ObjectId("5c93dc211c1954f62d7ff581"), "name" : "二郎", "gender" : "男", "age" : "20" }
{ "_id" : ObjectId("5c93dc211c1954f62d7ff582"), "name" : "张三", "gender" : "男", "age" : "18" }
{ "_id" : ObjectId("5c93dc211c1954f62d7ff583"), "name" : "李四", "gender" : "男", "age" : "21" }
{ "_id" : ObjectId("5c93dc211c1954f62d7ff584"), "name" : "五百", "gender" : "男", "age" : "17" }
> db.my_collection.remove({"age":"20"},{justOne: true})
WriteResult({ "nRemoved" : 1 })
> db.my_collection.find()
{ "_id" : ObjectId("5c93dc211c1954f62d7ff581"), "name" : "二郎", "gender" : "男", "age" : "20" }
{ "_id" : ObjectId("5c93dc211c1954f62d7ff582"), "name" : "张三", "gender" : "男", "age" : "18" }
{ "_id" : ObjectId("5c93dc211c1954f62d7ff583"), "name" : "李四", "gender" : "男", "age" : "21" }
{ "_id" : ObjectId("5c93dc211c1954f62d7ff584"), "name" : "五百", "gender" : "男", "age" : "17" }
```

### 删除多条符合条件的数据
```
> db.my_collection.find()
{ "_id" : ObjectId("5c93d9761c1954f62d7ff57b"), "name" : "一一", "gender" : "女", "age" : "20" }
{ "_id" : ObjectId("5c93d9761c1954f62d7ff57c"), "name" : "二郎", "gender" : "男", "age" : "20" }
{ "_id" : ObjectId("5c93d9761c1954f62d7ff57d"), "name" : "张三", "gender" : "男", "age" : "18" }
{ "_id" : ObjectId("5c93d9761c1954f62d7ff57e"), "name" : "李四", "gender" : "男", "age" : "21" }
{ "_id" : ObjectId("5c93d9761c1954f62d7ff57f"), "name" : "五百", "gender" : "男", "age" : "17" }
> db.my_collection.remove({"age":"20"},{justOne: false})
WriteResult({ "nRemoved" : 2 })
> db.my_collection.find()
{ "_id" : ObjectId("5c93d9761c1954f62d7ff57d"), "name" : "张三", "gender" : "男", "age" : "18" }
{ "_id" : ObjectId("5c93d9761c1954f62d7ff57e"), "name" : "李四", "gender" : "男", "age" : "21" }
{ "_id" : ObjectId("5c93d9761c1954f62d7ff57f"), "name" : "五百", "gender" : "男", "age" : "17" }
```

### 删除文档下全部数据
```
> db.my_collection.find()
{ "_id" : ObjectId("5c93d9761c1954f62d7ff57d"), "name" : "张三", "gender" : "男", "age" : "18" }
{ "_id" : ObjectId("5c93d9761c1954f62d7ff57e"), "name" : "李四", "gender" : "男", "age" : "21" }
{ "_id" : ObjectId("5c93d9761c1954f62d7ff57f"), "name" : "五百", "gender" : "男", "age" : "17" }
> db.my_collection.remove({})
WriteResult({ "nRemoved" : 3 })
> db.my_collection.find()
```

## deleteOne、deleteMany 删除数据

这 2 个语法是在 3.2 版本开始提供的，对应 remove 语法是否使用 justOne 参数

语法格式:
```
db.collection.deleteOne(
   <query>
)
db.collection.deleteMany(
   <query>
)
```

### 删除一条符合条件的数据
```
> db.my_collection.find()
{ "_id" : ObjectId("5c93ddee1c1954f62d7ff585"), "name" : "一一", "gender" : "女", "age" : "20" }
{ "_id" : ObjectId("5c93ddee1c1954f62d7ff586"), "name" : "二郎", "gender" : "男", "age" : "20" }
{ "_id" : ObjectId("5c93ddee1c1954f62d7ff587"), "name" : "张三", "gender" : "男", "age" : "18" }
{ "_id" : ObjectId("5c93ddee1c1954f62d7ff588"), "name" : "李四", "gender" : "男", "age" : "21" }
{ "_id" : ObjectId("5c93ddee1c1954f62d7ff589"), "name" : "五百", "gender" : "男", "age" : "17" }
> db.my_collection.deleteOne({"age":"20"})
{ "acknowledged" : true, "deletedCount" : 1 }
> db.my_collection.find()
{ "_id" : ObjectId("5c93ddee1c1954f62d7ff586"), "name" : "二郎", "gender" : "男", "age" : "20" }
{ "_id" : ObjectId("5c93ddee1c1954f62d7ff587"), "name" : "张三", "gender" : "男", "age" : "18" }
{ "_id" : ObjectId("5c93ddee1c1954f62d7ff588"), "name" : "李四", "gender" : "男", "age" : "21" }
{ "_id" : ObjectId("5c93ddee1c1954f62d7ff589"), "name" : "五百", "gender" : "男", "age" : "17" }
```

### 删除多条符合条件的数据
```
> db.my_collection.deleteMany({"name":"五百"})
{ "acknowledged" : true, "deletedCount" : 1 }
> db.my_collection.find()
{ "_id" : ObjectId("5c938454a0bdd2c56dbbf535"), "name" : "一一", "gender" : "女", "age" : "20" }
{ "_id" : ObjectId("5c938454a0bdd2c56dbbf536"), "name" : "二郎", "gender" : "男", "age" : "20" }
{ "_id" : ObjectId("5c938454a0bdd2c56dbbf537"), "name" : "张三", "gender" : "男", "age" : "18" }
{ "_id" : ObjectId("5c938454a0bdd2c56dbbf538"), "name" : "李四", "gender" : "男", "age" : "21" }
> db.my_collection.deleteMany({"age":"20"})
{ "acknowledged" : true, "deletedCount" : 2 }
> db.my_collection.find()
{ "_id" : ObjectId("5c938454a0bdd2c56dbbf537"), "name" : "张三", "gender" : "男", "age" : "18" }
{ "_id" : ObjectId("5c938454a0bdd2c56dbbf538"), "name" : "李四", "gender" : "男", "age" : "21" }
```


