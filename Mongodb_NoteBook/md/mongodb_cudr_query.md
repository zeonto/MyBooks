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
| 在N和M之间 | `{<key>:{$gte:<value>,$lte:<value>}}` | db.shengxiao.find({"nian":{$gte:2000,$lte:2010}}) | where nian >= 2000 and nian <= 2010 |
| 包含 | `{<key>:{$in:[<array>]}}` | db.my_collection.find({"age":{$in:[20,22]}}) | where age in (20,22) |
| 不包含 | `{<key>:{$nin:[<array>]}}` | db.my_collection.find({"age":{$nin:[20,22]}}) | where age not in (20,22) |

__这里条件操作符的大于等于和小于等于需要注意下，这里的是等于符号在最后 `gte`、`lte`，使用 tp 框架时等于符号必须是在前面的 `egt`、`elt`__

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

### $exists 判断字段是否存在

```
> db.shengxiao.find({"nian":{$exists:true}})
{ "_id" : ObjectId("5c937beca0bdd2c56dbbf4d5"), "shengxiao" : "鼠", "nian" : 1924 }
{ "_id" : ObjectId("5c937beca0bdd2c56dbbf4d6"), "shengxiao" : "鼠", "nian" : 1936 }
{ "_id" : ObjectId("5c937beca0bdd2c56dbbf4d7"), "shengxiao" : "鼠", "nian" : 1948 }
{ "_id" : ObjectId("5c937beca0bdd2c56dbbf4d8"), "shengxiao" : "鼠", "nian" : 1960 }
{ "_id" : ObjectId("5c937beca0bdd2c56dbbf4d9"), "shengxiao" : "鼠", "nian" : 1972 }
{ "_id" : ObjectId("5c937beca0bdd2c56dbbf4da"), "shengxiao" : "鼠", "nian" : 1984 }
{ "_id" : ObjectId("5c937beca0bdd2c56dbbf4db"), "shengxiao" : "鼠", "nian" : 1996 }
{ "_id" : ObjectId("5c937beca0bdd2c56dbbf4dc"), "shengxiao" : "鼠", "nian" : 2008 }
{ "_id" : ObjectId("5c937beca0bdd2c56dbbf4dd"), "shengxiao" : "牛", "nian" : 1925 }
{ "_id" : ObjectId("5c937beca0bdd2c56dbbf4de"), "shengxiao" : "牛", "nian" : 1937 }
{ "_id" : ObjectId("5c937beca0bdd2c56dbbf4df"), "shengxiao" : "牛", "nian" : 1949 }
{ "_id" : ObjectId("5c937beca0bdd2c56dbbf4e0"), "shengxiao" : "牛", "nian" : 1961 }
{ "_id" : ObjectId("5c937beca0bdd2c56dbbf4e1"), "shengxiao" : "牛", "nian" : 1973 }
{ "_id" : ObjectId("5c937beca0bdd2c56dbbf4e2"), "shengxiao" : "牛", "nian" : 1985 }
{ "_id" : ObjectId("5c937beca0bdd2c56dbbf4e3"), "shengxiao" : "牛", "nian" : 1997 }
{ "_id" : ObjectId("5c937beca0bdd2c56dbbf4e4"), "shengxiao" : "牛", "nian" : 2009 }
{ "_id" : ObjectId("5c937beca0bdd2c56dbbf4e5"), "shengxiao" : "虎", "nian" : 1926 }
{ "_id" : ObjectId("5c937beca0bdd2c56dbbf4e6"), "shengxiao" : "虎", "nian" : 1938 }
{ "_id" : ObjectId("5c937beca0bdd2c56dbbf4e7"), "shengxiao" : "虎", "nian" : 1950 }
{ "_id" : ObjectId("5c937beca0bdd2c56dbbf4e8"), "shengxiao" : "虎", "nian" : 1962 }
Type "it" for more
> db.shengxiao.find({"nian":{$exists:false}})
> 
```

### $mod 取模运算

{$mod:[N,M]}:集合中模N余M的数据

```
> db.shengxiao.find({"nian":{$mod:[10,1]}})
{ "_id" : ObjectId("5c937beca0bdd2c56dbbf4e0"), "shengxiao" : "牛", "nian" : 1961 }
{ "_id" : ObjectId("5c937beca0bdd2c56dbbf4ef"), "shengxiao" : "兔", "nian" : 1951 }
{ "_id" : ObjectId("5c937beca0bdd2c56dbbf4f4"), "shengxiao" : "兔", "nian" : 2011 }
{ "_id" : ObjectId("5c937beca0bdd2c56dbbf4fe"), "shengxiao" : "蛇", "nian" : 1941 }
{ "_id" : ObjectId("5c937beca0bdd2c56dbbf503"), "shengxiao" : "蛇", "nian" : 2001 }
{ "_id" : ObjectId("5c937beca0bdd2c56dbbf50d"), "shengxiao" : "羊", "nian" : 1931 }
{ "_id" : ObjectId("5c937beca0bdd2c56dbbf512"), "shengxiao" : "羊", "nian" : 1991 }
{ "_id" : ObjectId("5c937beca0bdd2c56dbbf521"), "shengxiao" : "鸡", "nian" : 1981 }
{ "_id" : ObjectId("5c937beca0bdd2c56dbbf530"), "shengxiao" : "猪", "nian" : 1971 }
> 
```

### $size 元素个数

准备数据：
```
db.my_collection.insertMany([{"author":"七七","gender":"女","age":20,"class":[1,2,5]},{"author":"八神","gender":"男","age":20,"class":[1,3]}])
```

```
> db.my_collection.find()
{ "_id" : ObjectId("5c945dbca0bdd2c56dbbf551"), "name" : "一一", "gender" : "女", "age" : 20 }
{ "_id" : ObjectId("5c945dbca0bdd2c56dbbf552"), "name" : "二郎", "gender" : "男", "age" : 20 }
{ "_id" : ObjectId("5c945dbca0bdd2c56dbbf553"), "name" : "张三", "gender" : "男", "age" : 18 }
{ "_id" : ObjectId("5c945dbca0bdd2c56dbbf554"), "name" : "李四", "gender" : "男", "age" : 21 }
{ "_id" : ObjectId("5c945dbca0bdd2c56dbbf555"), "name" : "五百", "gender" : "男", "age" : 17 }
{ "_id" : ObjectId("5c945ee3a0bdd2c56dbbf556"), "name" : "六六", "gender" : "女", "age" : 22 }
{ "_id" : ObjectId("5c948530a0bdd2c56dbbf557"), "author" : "七七", "gender" : "女", "age" : 20, "class" : [ 1, 2, 5 ] }
{ "_id" : ObjectId("5c948530a0bdd2c56dbbf558"), "author" : "八神", "gender" : "男", "age" : 20, "class" : [ 1, 3 ] }
> db.my_collection.find({"class":{$size:2}})
{ "_id" : ObjectId("5c948530a0bdd2c56dbbf558"), "author" : "八神", "gender" : "男", "age" : 20, "class" : [ 1, 3 ] }
> db.my_collection.find({"class":{$size:3}})
{ "_id" : ObjectId("5c948530a0bdd2c56dbbf557"), "author" : "七七", "gender" : "女", "age" : 20, "class" : [ 1, 2, 5 ] }
> db.my_collection.find({"class":{$size:4}})
> 
```

### 正则查询

```
> db.my_collection.find({"name":/^张.*/})
{ "_id" : ObjectId("5c945dbca0bdd2c56dbbf553"), "name" : "张三", "gender" : "男", "age" : 18 }
```

### limit() 取条数

```
> db.my_collection.find().limit(3)
{ "_id" : ObjectId("5c945dbca0bdd2c56dbbf551"), "name" : "一一", "gender" : "女", "age" : 20 }
{ "_id" : ObjectId("5c945dbca0bdd2c56dbbf552"), "name" : "二郎", "gender" : "男", "age" : 20 }
{ "_id" : ObjectId("5c945dbca0bdd2c56dbbf553"), "name" : "张三", "gender" : "男", "age" : 18 }
```

### skip() 跳过条数

```
> db.my_collection.find().skip(1).limit(3)
{ "_id" : ObjectId("5c945dbca0bdd2c56dbbf552"), "name" : "二郎", "gender" : "男", "age" : 20 }
{ "_id" : ObjectId("5c945dbca0bdd2c56dbbf553"), "name" : "张三", "gender" : "男", "age" : 18 }
{ "_id" : ObjectId("5c945dbca0bdd2c56dbbf554"), "name" : "李四", "gender" : "男", "age" : 21 }
```

### sort() 结果排序

```
> db.my_collection.find().sort({"age":1})
{ "_id" : ObjectId("5c945dbca0bdd2c56dbbf555"), "name" : "五百", "gender" : "男", "age" : 17 }
{ "_id" : ObjectId("5c945dbca0bdd2c56dbbf553"), "name" : "张三", "gender" : "男", "age" : 18 }
{ "_id" : ObjectId("5c945dbca0bdd2c56dbbf551"), "name" : "一一", "gender" : "女", "age" : 20 }
{ "_id" : ObjectId("5c945dbca0bdd2c56dbbf552"), "name" : "二郎", "gender" : "男", "age" : 20 }
{ "_id" : ObjectId("5c948530a0bdd2c56dbbf557"), "author" : "七七", "gender" : "女", "age" : 20, "class" : [ 1, 2, 5 ] }
{ "_id" : ObjectId("5c948530a0bdd2c56dbbf558"), "author" : "八神", "gender" : "男", "age" : 20, "class" : [ 1, 3 ] }
{ "_id" : ObjectId("5c945dbca0bdd2c56dbbf554"), "name" : "李四", "gender" : "男", "age" : 21 }
{ "_id" : ObjectId("5c945ee3a0bdd2c56dbbf556"), "name" : "六六", "gender" : "女", "age" : 22 }
> db.my_collection.find().sort({"age":-1})
{ "_id" : ObjectId("5c945ee3a0bdd2c56dbbf556"), "name" : "六六", "gender" : "女", "age" : 22 }
{ "_id" : ObjectId("5c945dbca0bdd2c56dbbf554"), "name" : "李四", "gender" : "男", "age" : 21 }
{ "_id" : ObjectId("5c945dbca0bdd2c56dbbf551"), "name" : "一一", "gender" : "女", "age" : 20 }
{ "_id" : ObjectId("5c945dbca0bdd2c56dbbf552"), "name" : "二郎", "gender" : "男", "age" : 20 }
{ "_id" : ObjectId("5c948530a0bdd2c56dbbf557"), "author" : "七七", "gender" : "女", "age" : 20, "class" : [ 1, 2, 5 ] }
{ "_id" : ObjectId("5c948530a0bdd2c56dbbf558"), "author" : "八神", "gender" : "男", "age" : 20, "class" : [ 1, 3 ] }
{ "_id" : ObjectId("5c945dbca0bdd2c56dbbf553"), "name" : "张三", "gender" : "男", "age" : 18 }
{ "_id" : ObjectId("5c945dbca0bdd2c56dbbf555"), "name" : "五百", "gender" : "男", "age" : 17 }
> 
```

### count() 计算总条数

```
> db.my_collection.find().count()
8
```

### pretty() 美化显示

准备数据：
```
db.my_collection_2.insertMany([
    {
        "author":"七七","gender":"女","age":20,"bag":{
            "6":{"items":{"11":{"code":1000},"9":{"code":1002}}},
            "1":{"items":{"11":{"code":1000},"9":{"code":1001}}}
        }
    },
    {
        "author":"八神","gender":"男","age":20,"bag":{
            "6":{"items":{"11":{"code":1000},"9":{"code":1001}}},
            "7":{"items":{"11":{"code":2000},"9":{"code":2001}}}
        }
    }
])
```

```
> db.my_collection_2.find()
{ "_id" : ObjectId("5c948e04a0bdd2c56dbbf559"), "author" : "七七", "gender" : "女", "age" : 20, "bag" : { "1" : { "items" : { "9" : { "code" : 1001 }, "11" : { "code" : 1000 } } }, "6" : { "items" : { "9" : { "code" : 1002 }, "11" : { "code" : 1000 } } } } }
{ "_id" : ObjectId("5c948e04a0bdd2c56dbbf55a"), "author" : "八神", "gender" : "男", "age" : 20, "bag" : { "6" : { "items" : { "9" : { "code" : 1001 }, "11" : { "code" : 1000 } } }, "7" : { "items" : { "9" : { "code" : 2001 }, "11" : { "code" : 2000 } } } } }
>
> db.my_collection_2.find().pretty()
{
	"_id" : ObjectId("5c948e04a0bdd2c56dbbf559"),
	"author" : "七七",
	"gender" : "女",
	"age" : 20,
	"bag" : {
		"1" : {
			"items" : {
				"9" : {
					"code" : 1001
				},
				"11" : {
					"code" : 1000
				}
			}
		},
		"6" : {
			"items" : {
				"9" : {
					"code" : 1002
				},
				"11" : {
					"code" : 1000
				}
			}
		}
	}
}
{
	"_id" : ObjectId("5c948e04a0bdd2c56dbbf55a"),
	"author" : "八神",
	"gender" : "男",
	"age" : 20,
	"bag" : {
		"6" : {
			"items" : {
				"9" : {
					"code" : 1001
				},
				"11" : {
					"code" : 1000
				}
			}
		},
		"7" : {
			"items" : {
				"9" : {
					"code" : 2001
				},
				"11" : {
					"code" : 2000
				}
			}
		}
	}
}
> 
``` 




