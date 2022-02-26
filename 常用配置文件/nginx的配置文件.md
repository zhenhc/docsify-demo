1. 先用`find / -name nginx.conf`找到nginx的安装目录
2. `cd conf.d`目录里，新建一个配置文件touch scrm.conf
3. 在新建的配置文件scrm.conf里配置信息如下：

```properties
server {
        # nginx的监听端口，客户访问时的端口号
        listen       8060;
        server_name  localhost;
        charset utf-8;

    location / {
            # 前端使用npm build:pro 生成的dist压缩包上传到服务器任意地址的路径
            root   /root/software/scrm-admin-ui/dist;
            index  index.html index.htm;

            #没有这一行时，刷新带有/index.html的路径时，会失败
        try_files $uri $uri/ /index.html;
    }

    location ~ /scrm-api {
        proxy_set_header Host $http_host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header REMOTE-HOST $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_pass        http://localhost:8082;
    }

        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
```

4. 在主配置文件nginx.conf里会有一行导入新建的配置文件

```properties
    include /etc/nginx/conf.d/*.conf;
```

5. 在主配置文件里把第一行`user nginx`改为`user root`,如果不改会返回500 internal异常
6. 完整主配置文件

```properties
# 第一行`user nginx`改为`user root`
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

    include /etc/nginx/conf.d/*.conf;

}
```
