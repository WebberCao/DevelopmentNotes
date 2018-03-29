### MongoDB客户端工具MongoVUE连接MongoDB 不显示Collections以及创建Collection失败的问题
Mongodb 3.0支持用户自定义存储引擎，用户可配置使用mmapv1或者wiredTiger存储引擎。3.2版本以后默认的开启的是wiredTiger存储引擎，之前用的是mmapv1存储引擎。并且2个存储引擎生成的数据文件格式不兼容。也就是说mmapv1引擎生成的数据文件wiredTiger引擎读取不出来。
要想知道MongoDB到底开启了哪个引擎，最简单的方式查看数据文件。出现如下格式的数据文件是wiredTiger存储引擎启动了：
出现如下数据格式启动的是：mmapv1存储引擎
更换数据存储引擎：
一、首先确保之前的数据目录下没有重要数据，然后清空数据目录。
二、通过命令启动wiredTiger 存储引擎：
mongod --storageEngine wiredTiger  --dbpath 数据目录
之后就会正常使用了，如下图所示：

