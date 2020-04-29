# Ubuntu 18.04 安装并配置 Nginx

## 1. 安装 Nginx

```bash
apt install nginx
```

## 2. 验证安装是否成功
可以使用`systemctl`命令查看：
```shell
systemctl status nginx
```
返回的结果 `Active` 为 `active(running)`表示nginx安装成功并正常运行

## 3. 配置文件
Nginx的配置文件一般在 `/etc/nginx` 目录下，通常情况，Nginx 会加载主配置文件以及以下几个目录的配置文件：

- `/etc/nginx/nginx.conf`
- `/etc/nginx/modules-enabled/*.conf`
- `/etc/nginx/conf.d/*.conf`
- `/etc/nginx/sites-enabled/*`

我们可以把自己网站的代理配置文件放置到`/etc/nginx/sites-enabled/*`下，方便区分

### 3.1 nginx.conf 基本配置说明
```nginx
# 运行用户
user www-data;

# Nginx worker进程个数：其数量直接影响性能
worker_processes auto;

# pid文件路径
pid /run/nginx.pid;

# 嵌入其他路径下的配置文件
include /etc/nginx/modules-enabled/*.conf;

events {
	worker_connections 768;
}

# http 配置
http {

	##
	# Basic Settings
	##

	sendfile on;
	tcp_nopush on;
	tcp_nodelay on;
	keepalive_timeout 65;
	types_hash_max_size 2048;
	# server_tokens off;

	# server_names_hash_bucket_size 64;
	# server_name_in_redirect off;

	include /etc/nginx/mime.types;
	default_type application/octet-stream;

	##
	# SSL Settings
	##

	ssl_protocols TLSv1 TLSv1.1 TLSv1.2; # Dropping SSLv3, ref: POODLE
	ssl_prefer_server_ciphers on;

	##
	# Logging Settings
	##

	access_log /var/log/nginx/access.log;
	error_log /var/log/nginx/error.log;

	##
	# Gzip Settings
	##

    # 开启 gzip
	gzip on;

	##
	# Virtual Host Configs
	##

    # 嵌入其他路径的配置文件
	include /etc/nginx/conf.d/*.conf;
	include /etc/nginx/sites-enabled/*;
}
```
### 3.2 自定义配置文件
如果我们想使用自己的配置文件搭建反向代理项目，可以去修改`/etc/nginx/sites-enabled/default`配置文件（这是一个`link`，指向了`/etc/nginx/sites-available/default`文件）

如下就是搭建一个代理前后端分离项目的 Nginx 配置文件
```nginx
server {
    # 监听 80 端口
	listen 80 default_server;
	listen [::]:80 default_server;

    # 服务域名地址，默认为 localhost，也可以写固定的域名
	server_name _;
    # server_name www.example.com;
    
    # 开启 gzip
	gzip on;
	gzip_buffers 32 4K;
	gzip_comp_level 6;
	gzip_min_length 100;
	gzip_types application/javascript text/css text/xml;

    # 请求 /
	location / {
    	proxy_set_header X-Real-IP $remote_addr;
   	    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        
        # 代理转发给 :8080
		proxy_pass http://127.0.0.1:8080;
	}

    # 请求 /api
	location /api {
		proxy_set_header X-Real-IP $remote_addr;
		proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

        # 代理转发给 :8081
		proxy_pass http://127.0.0.1:8081;
	}	
}
```

