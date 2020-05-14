# Win10 使用压缩包安装 mysql 8.0 环境

## 1. 下载 Mysql Server 8.0 

在[Mysql 官网](https://dev.mysql.com/downloads/mysql/)根据系统环境下载对应的压缩包：

![image-20200512094154681](https://note-figure-bed.oss-cn-shenzhen.aliyuncs.com/note/20200512094157.png)

## 2. 解压文件到安装目录

将下载好的压缩包解压到一个文件夹，作为 Mysql 的安装目录。

## 3. 创建配置文件`my.ini`

在 Mysql 目录中创建配置文件`mysql.ini`：

```ini
[mysqld]
# 设置3306端口
port=3306
# 设置mysql的安装目录
basedir=D:\\Environment\\mysql-8.0.19-winx64
# 设置mysql数据库的数据的存放目录
datadir=D:\\Environment\\mysql-8.0.19-winx64\\Data
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

## 4. 配置环境变量

在`Path`环境变量中添加`mysql`路径下的`bin`目录路径：

<img src="https://note-figure-bed.oss-cn-shenzhen.aliyuncs.com/note/20200512094416.png" alt="image-20200512094409085" style="zoom:80%;" />

## 5. 初始化数据库

使用**管理员权限**打开cmd，执行命令：

```sh
mysqld --initialize --console
```

如果提示缺少运行库，可以安装**微软常用运行库合集**，在360软件管家中可以下载到。

如果执行完成，可以看到`root`用户的初始默认密码，随后的登录需要该密码，所以此时**不要关闭**该命令窗口。

## 6. 安装服务

使用**管理员权限**打开cmd，执行命令：

```sh
mysqld --install [服务名]
```

如果服务名不写，则默认的名字为`mysql`。

如果执行完成，命令窗口会显示安装完成，接着就可以运行`net start [服务名]`来开启`mysql`服务。

## 7. 更改密码

使用`cmd`，执行命令：

```sh
mysql -u root -p
```

填入`root`用户的初始密码，进入`mysql`命令模式后执行命令：

```mysql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '新密码';
```

执行完成后`root`用户的密码就改为了`新密码`。