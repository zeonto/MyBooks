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

## 删除数据

关键命令：`.deleteMany()`

删除符合条件的多条数据
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