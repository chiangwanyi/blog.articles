# 使用 Portainer 安装 MongoDB

## 1. 在主机创建挂载点

```bash
mkdir -p /data/mongo
```

## 2. 创建 Stack

这里可以将 Name 设置为`mongo`，并定义以下 Docker Compose 文件

```yaml
version: '2'

services:
  mongodb:						# 服务名
    image: mongo				# 镜像
    restart: always				# 保持开启
    container_name: mongodb		# 容器名称
    ports:
     - "27017:27017"			# 端口	宿主机:27017 <- 容器:27017
    volumes:
     - /data/mongo:/data/db		# 挂载点	宿主机 <- 容器
```

然后点击`Deploy the stack `

## 3. 开启权限认证

### 3.1 创建管理员权限用户
这里使用 Robo 3T 连接数据库，成功后切换到`admin`数据库
```sql
use admin
```
```sql
db.createUser({
	user:"admin",
	pwd:"26a7xpis%5yg4^o#",
    roles:[{ role: 'root', db: 'admin' }]
})
```

成功后运行以下命令，返回`1`表示认证成功

```sql
db.auth('admin', '26a7xpis%5yg4^o#')
```

### 3.2 创建普通权限用户

切换到目标数据库，这里使用的是`blog`数据库

```sql
use blog
```

```sql
db.createUser({
	user: 'jwy',
	pwd: '123456',
	roles: [{ role: 'readWrite', db: 'blog' }]
})
```

成功后运行以下命令，返回`1`表示认证成功，该用户就只拥有`blog`数据库的`readWrite`权限

```sql
db.auth('admin', '26a7xpis%5yg4^o#')
```

## 4. 修改 Stack

修改 mongo 的 docker compose，使其附带权限认证

```yaml
version: '2'

services:
  mongodb:
    image: mongo
    restart: always
    container_name: mongodb
    ports:
     - "27017:27017"
    volumes:
     - /data/mongo:/data/db
    command: [--auth]
```

更新容器后，连接 mongo 就需要认证了