# 命令连接

## 远程连接

格式：`mongo ip或DNS:端口号/数据库名 -u user -p password`

命令：`bin/mongo 192.168.9.209:27017`

如果没有密码就不需要指定用户名和密码，另外如果在`IP:端口`后不指定库的话，默认连接`test`库，在较新版本上会显示连接的库，而在较低版本上会不显示库。

连接信息可以在连接时候输入的信息里看到这句 `connecting to:`，显示了连接的 mongo 服务地址和库。

连接成功后的效果：
```shell
$ bin/mongo 192.168.9.209:27017
MongoDB shell version v4.0.4
connecting to: mongodb://192.168.9.209:27017/test
WARNING: No implicit session: Logical Sessions are only supported on server versions 3.6 and greater.
Implicit session: dummy session
MongoDB server version: 3.4.6
WARNING: shell and server versions do not match
Server has startup warnings: 
2019-03-18T20:10:48.826+0800 I STORAGE  [initandlisten] 
2019-03-18T20:10:48.826+0800 I STORAGE  [initandlisten] ** WARNING: Using the XFS filesystem is strongly recommended with the WiredTiger storage engine
2019-03-18T20:10:48.826+0800 I STORAGE  [initandlisten] **          See http://dochub.mongodb.org/core/prodnotes-filesystem
2019-03-18T20:11:19.524+0800 I CONTROL  [initandlisten] 
2019-03-18T20:11:19.524+0800 I CONTROL  [initandlisten] ** WARNING: Access control is not enabled for the database.
2019-03-18T20:11:19.524+0800 I CONTROL  [initandlisten] **          Read and write access to data and configuration is unrestricted.
2019-03-18T20:11:19.524+0800 I CONTROL  [initandlisten] 
2019-03-18T20:11:19.524+0800 I CONTROL  [initandlisten] 
2019-03-18T20:11:19.524+0800 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2019-03-18T20:11:19.524+0800 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2019-03-18T20:11:19.524+0800 I CONTROL  [initandlisten] 
2019-03-18T20:11:19.524+0800 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2019-03-18T20:11:19.524+0800 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2019-03-18T20:11:19.524+0800 I CONTROL  [initandlisten] 
2019-03-18T20:11:19.524+0800 I CONTROL  [initandlisten] ** WARNING: soft rlimits too low. rlimits set to 1024 processes, 65535 files. Number of processes should be at least 32767.5 : 0.5 times number of files.
2019-03-18T20:11:19.524+0800 I CONTROL  [initandlisten] 
MongoDB Enterprise > db
test
MongoDB Enterprise > 
```

指定库连接效果：
```shell
$ bin/mongo 192.168.9.209:27017/local
MongoDB shell version v4.0.4
connecting to: mongodb://192.168.9.209:27017/local
WARNING: No implicit session: Logical Sessions are only supported on server versions 3.6 and greater.
Implicit session: dummy session
MongoDB server version: 3.4.6
WARNING: shell and server versions do not match
Server has startup warnings: 
2019-03-18T20:10:48.826+0800 I STORAGE  [initandlisten] 
2019-03-18T20:10:48.826+0800 I STORAGE  [initandlisten] ** WARNING: Using the XFS filesystem is strongly recommended with the WiredTiger storage engine
2019-03-18T20:10:48.826+0800 I STORAGE  [initandlisten] **          See http://dochub.mongodb.org/core/prodnotes-filesystem
2019-03-18T20:11:19.524+0800 I CONTROL  [initandlisten] 
2019-03-18T20:11:19.524+0800 I CONTROL  [initandlisten] ** WARNING: Access control is not enabled for the database.
2019-03-18T20:11:19.524+0800 I CONTROL  [initandlisten] **          Read and write access to data and configuration is unrestricted.
2019-03-18T20:11:19.524+0800 I CONTROL  [initandlisten] 
2019-03-18T20:11:19.524+0800 I CONTROL  [initandlisten] 
2019-03-18T20:11:19.524+0800 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2019-03-18T20:11:19.524+0800 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2019-03-18T20:11:19.524+0800 I CONTROL  [initandlisten] 
2019-03-18T20:11:19.524+0800 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2019-03-18T20:11:19.524+0800 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2019-03-18T20:11:19.524+0800 I CONTROL  [initandlisten] 
2019-03-18T20:11:19.524+0800 I CONTROL  [initandlisten] ** WARNING: soft rlimits too low. rlimits set to 1024 processes, 65535 files. Number of processes should be at least 32767.5 : 0.5 times number of files.
2019-03-18T20:11:19.524+0800 I CONTROL  [initandlisten] 
MongoDB Enterprise > db
local
MongoDB Enterprise >
```
