



安装 redis



```
 docker pull redis
```



```
docker run -p 6380:6380 --name forredis2 --privileged=true -v/home/redis/conf/redis.conf:/etc/redis/redis.conf -v /home/redis/data:/data -d redis
```



配置文件如下：

```
worker_processes  1;

error_log  /var/log/nginx/error.log warn;
pid        /var/run/nginx.pid;

events {
    worker_connections  1024;
}

http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;
    # 限制body大小
    client_max_body_size 100m;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                          '$status $body_bytes_sent "$http_referer" '
                          '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    upstream server {
        ip_hash;
        server 127.0.0.1:8080;
        server 127.0.0.1:8081;
    }

    upstream monitor-admin {
        server 127.0.0.1:9090;
    }

    upstream xxljob-admin {
        server 127.0.0.1:9100;
    }

    server {
        listen       80;
        server_name  localhost;

        # https配置参考 start
        #listen       443 ssl;

        # 证书直接存放 /docker/nginx/cert/ 目录下即可 更改证书名称即可 无需更改证书路径
        #ssl on;
        #ssl_certificate      /etc/nginx/cert/xxx.local.crt; # /etc/nginx/cert/ 为docker映射路径 不允许更改
        #ssl_certificate_key  /etc/nginx/cert/xxx.local.key; # /etc/nginx/cert/ 为docker映射路径 不允许更改
        #ssl_session_timeout 5m;
        #ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
        #ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
        #ssl_prefer_server_ciphers on;
        # https配置参考 end

        # 演示环境配置 拦截除 GET POST 之外的所有请求
        # if ($request_method !~* GET|POST) {
        #     rewrite  ^/(.*)$  /403;
        # }

        # location = /403 {
        #     default_type application/json;
        #     return 200 '{"msg":"演示模式，不允许操作","code":500}';
        # }

        # 限制外网访问内网 actuator 相关路径
        location ~ ^(/[^/]*)?/actuator(/.*)?$ {
            return 403;
        }

        location / {
            root   /usr/share/nginx/html;
            try_files $uri $uri/ /index.html;
            index  index.html index.htm;
        }

        location /prod-api/ {
            proxy_set_header Host $http_host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header REMOTE-HOST $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_pass http://server/;
        }

        # https 会拦截内链所有的 http 请求 造成功能无法使用
        # 解决方案1 将 admin 服务 也配置成 https
        # 解决方案2 将菜单配置为外链访问 走独立页面 http 访问
        location /admin/ {
            proxy_set_header Host $http_host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header REMOTE-HOST $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_pass http://monitor-admin/admin/;
        }

        # https 会拦截内链所有的 http 请求 造成功能无法使用
        # 解决方案1 将 xxljob 服务 也配置成 https
        # 解决方案2 将菜单配置为外链访问 走独立页面 http 访问
        location /xxl-job-admin/ {
            proxy_set_header Host $http_host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header REMOTE-HOST $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_pass http://xxljob-admin/xxl-job-admin/;
        }

        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
}

```





部署jar 包

```
FROM anapsix/alpine-java:8_server-jre_unlimited
RUN mkdir -p /ruoyi/server/logs \
    /ruoyi/server/temp \
    /ruoyi/skywalking/agent

#WORKDIR指令用于指定容器的一个目录， 容器启动时执行的命令会在该目录下执行。
WORKDIR /ruoyi/server
#将当前ruoyi-admin.jar 复制到容器对应目录下
ADD ruoyi-admin.jar /ruoyi/server/ruoyi-admin.jar
EXPOSE 8688

#容器启动时执行的命令
CMD ["nohup","java","-jar","/ruoyi/server/ruoyi-admin.jar","&"]
```



打包成镜像

```
docker build -t ruoyi-admin:v1.0.0 .

```





运行镜像

```shell
docker run -p 8688:8688 --name ruoyi-admin -d ruoyi-admin:v1.0.0

```





查看容器 ID

```
docker logs --tail 300 -f   c101f52ad4a1  [容器ID]


```



进入docker内部

```
docker exec -it ruoyi-admin  /bin/bash
```





在容器中运行

```
nohup java -jar ruoyi-admin.jar --server.port=8688 &

```

