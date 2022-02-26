# nginx反向代理配置

1. 新建一个conf.d目录，在这个目录下新建一个***.conf配置文件

```shell
server {
        listen       8060;          #nginx监听的端口号
        server_name  localhost;
		charset utf-8;

	location / {
        root   /root/software/scrm-admin-ui/dist;          #前端代码打包之后上传到服务器的路径地址
        index  index.html index.htm;
	    try_files $uri $uri/ /index.html;
	}
		
	location ~ /scrm-api {                              # 后端接口的根路径
	    proxy_set_header Host $http_host;
	    proxy_set_header X-Real-IP $remote_addr;
	    proxy_set_header REMOTE-HOST $remote_addr;
	    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
	    proxy_pass  http://localhost:8082;           # 后端代码的ip地址和端口号
	}

        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }

```

2. 在主配置文件nginx.conf中引入这个配置文件

```shell

user  root;
worker_processes  8;

error_log  /var/log/nginx/error.log warn;
pid        /var/run/nginx.pid;

events {
    worker_connections  1024;
}


http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    keepalive_timeout  65;
    
    #gzip  on;

    client_max_body_size 1024m;

    include /etc/nginx/conf.d/*.conf;      # 引入conf.d目录下的所有配置文件
  
}

```

