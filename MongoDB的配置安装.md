## MongoDB的配置安装
一、下载
根据需要在官网或者http://dl.mongodb.org/dl/win32/x86_64下载相应的安装文件。
二、安装
如果下载的是msi文件，则根据以下提示进行安装（如果是zip文件，则直接解压到想要安装的目录下）：
可以选择安装方式或者更改安装路径
三、配置
在安装路径所在的根目录下创建日志文件和数据文件夹（比如安装在了D盘下的某个目录下，则数据文件夹要建在D盘的根目录下）
在D盘建立一个文件夹D:\data
然后在data文件夹下建一个db目录作为数据文件夹
再在data文件夹下建一个log文件夹作为日志文件夹，然后在log文件夹下建一个mylog.log文件作为日志文件。
打开cmd命令行窗口，进入到安装路径下的bin目录下，执行mongod.exe文件.
mongod.exe  --dbpath d:\data\db
如果执行成功，会输出如下信息：
四、将MongoDB服务器作为Windows服务运行
mongod.exe --install --logpath "d:\data\log\mylog.log" --logappend –dbpath "d:\data\db"  --serviceName  "YourServiceName"  --serviceDisplayName  "YourServiceName" 
启动服务： net start mongodb       启动mongodb服务

五、进入MongoDB后台管理shell
进入mongodb安装目录的下的bin目录，然后执行mongo.exe文件，出现下图提示说明已经成功进入控制台。
接下来就可以进入数据库操作了。
