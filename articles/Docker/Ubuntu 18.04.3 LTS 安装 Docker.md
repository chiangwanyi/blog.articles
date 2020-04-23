# Ubuntu 18.04.3 LTS 安装 Docker

## 1. 更新系统

```bash
sudo apt update
sudo apt upgrade
```

## 2. 安装依赖程序包

```bash
apt install apt-transport-https ca-certificates curl software-properties-common
```

## 3. 添加 Docker 存储库

1. 添加GPG密钥

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

2. 添加存储库

```bash
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
```

3. 更新存储库信息

```bash
sudo apt update
```

## 4. 安装 Docker

```bash
sudo apt install docker-ce
```

## 5. 检查 Docker 运行状态

```bash
sudo systemctl status docker
```

如果 Active 的状态为 `active(running)`表示 docker 运行正常

## 6. 使用加速器可以提升获取Docker官方镜像的速度

### 6.1 可以选择使用网易镜像源地址

1. 在 `/etc/docker/daemon.json` 中写入如下内容（如果文件不存在请新建该文件）

```json
{
  "registry-mirrors": [
    "http://hub-mirror.c.163.com"
  ]
}
```

2. 重新启动服务

```bash
sudo systemctl daemon-reload
sudo systemctl restart docker
```

### 6.2 或者选择使用阿里云提供给开发者的专属加速器

```bash
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://ji9dt51l.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

