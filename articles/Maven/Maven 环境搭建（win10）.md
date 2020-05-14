## 1. 下载 Maven

在[Maven 官网](https://maven.apache.org/download.cgi)下载 Maven 文件

![image-20200505222624526](https://note-figure-bed.oss-cn-shenzhen.aliyuncs.com/note/image-20200505222624526.png)

下载完成后将文件解压到一个文件夹即可

![image-20200506085831981](https://note-figure-bed.oss-cn-shenzhen.aliyuncs.com/note/image-20200506085831981.png)

## 2. 配置环境变量

需要以下的环境变量

- M2_HOME：maven 目录下的 bin 目录
- MAVEN_HOME：maven 目录
- PATH：添加 `%MAVEN_HOME%\bin`

完成后在控制台运行`mvn -version`，出现 maven 的输出信息则表示环境配置成功

![QQ截图20200506091242](https://note-figure-bed.oss-cn-shenzhen.aliyuncs.com/note/QQ截图20200506091242.png)

## 3. 配置阿里云镜像

使用国内镜像可以提高资源下载速度，在 maven 目录下的 `config/settings/xml`中的`mirrors`添加以下信息：

```xml
<mirror>
    <id>nexus-aliyun</id>
    <mirrorOf>*,!jeecg,!jeecg-snapshots</mirrorOf>
    <name>Nexus aliyun</name>
    <url>http://maven.aliyun.com/nexus/content/groups/public</url>
</mirror>
```

## 4. 创建本地仓库

用于存放下载的资源，可以放在 maven 目录下，命名为`maven-repo`

![image-20200506092351797](https://note-figure-bed.oss-cn-shenzhen.aliyuncs.com/note/image-20200506092351797.png)

然后在`config/settings/xml`中的`localRepository`添加该仓库地址：

![image-20200506092641393](https://note-figure-bed.oss-cn-shenzhen.aliyuncs.com/note/20200506092643.png)

## 5. IDEA 中配置 Maven

![image-20200506093344034](E:\Program\blog.aritcles\articles\Maven\Maven 环境搭建（win10）.assets\image-20200506093344034.png)