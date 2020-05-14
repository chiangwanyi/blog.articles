## 1. 设置环境变量

首先设置如下的环境变量，可以写在用户变量中：

- GO111MODULE = on

## 2. 项目搭建

现在就可以使用命令`go mod init (project name)`来创建项目。

```powershell
> go mod init example
```

## 3. 设置代理服务器 GOPROXY

设置如下的环境变量：

- GOPROXY = https://goproxy.io
- GOPRIVATE = *.corp.example.com

> GOPROXY 值设置的地址可选，这里使用了 [GOPROXY.IO](https://goproxy.io) 的代理地址，具体设置方法请参考网站