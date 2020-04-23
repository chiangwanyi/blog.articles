# 连接 github 远程仓库和提交拉取代码操作

## 1. 前提
- 主机安装了 git
- 远程仓库已经创建

## 2. 设置全局用户名和邮箱
```shell
git config --global user.name "你的 github 用户名"
git config --global user.email "你的 github 邮箱"
```

## 3. 生成 ssh key
```shell
ssh-keygen -t rsa -C "你的 github 邮箱"
```

随后的输入可以全部回车，完成后在 `~/.ssh/` 路径下可以看到两个生成的文件：

- `id_rsa`
- `id_rsa.pub`

## 4. 在 Github 中新建 ssh key

新建一个 ssh key，路径如下：

> Setting -> SSH and GPG keys -> New SSH key

`Title` 可以任意填，`Key` 则填入 `id_rsa.pub` 的内容，完成后点击 Add

## 5. 测试
在终端中输入
```shell
ssh -T git@github.com
```
如果能看到 **You've successfully authenticated**，说明主机与 Github 的连接与认证成功了

## 6. 初始化本地仓库

```shell
git init
git add .
git commit -m "init"
git remote add origin git@github.com:项目.git
```

## 7. 提交与拉取

### 7.1 提交本地仓库代码到远程仓库主分支

```shell
git push -u origin master
```

### 7.2 拉取远程仓库主分支代码到本地仓库

```shell
git pull origin master
```