# Win10 使用压缩包安装 mysql 8.0 环境

## 1. 下载 Mysql Server 8.0 

在[Mysql 官网](https://dev.mysql.com/downloads/mysql/)根据系统环境下载对应的压缩包

![image-20200425105555331](http://q8aqauxg5.bkt.clouddn.com/blog/image-20200425105555331.png)

## 2. 解压文件到安装目录

将下载好的压缩包解压到一个文件夹，作为 Mysql 的安装目录

## 3. 创建配置文件`my.ini`

在 Mysql 目录中创建配置文件`mysql.ini`

```ini
[mysqld]
# 设置3306端口
port=3306
# 设置mysql的安装目录
basedir=D:\\mysql-8.0.19-winx64
# 设置mysql数据库的数据的存放目录
datadir=D:\\mysql-8.0.19-winx64\\Data
# 允许最大连接数
max_connections=200
# 允许连接失败的次数。这是为了防止有人从该主机试图攻击数据库系统
max_connect_errors=10
# 服务端使用的字符集默认为UTF8
character-set-server=utf8
# 创建新表时将使用的默认存储引擎
default-storage-engine=INNODB
# 默认使用“mysql_native_password”插件认证
default_authentication_plugin=mysql_native_password

[mysql]
# 设置mysql客户端默认字符集
default-character-set=utf8

[client]
# 设置mysql客户端连接服务端时默认使用的端口
port=3306
default-character-set=utf8
```