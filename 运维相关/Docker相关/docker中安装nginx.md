拉取镜像

```
docker pull nginx:latest
```



创建本地docker配置

```
mkdir -p /usr/docker/nginx/html
mkdir -p /etc/docker/nginx/{conf.d,logs}


```



运行docker 并挂载配置文件 

```
# 1. 第一个“-v”，是项目位置，把项目放到挂载到的目录下即可 
# 2. 第二个“-v”，是挂载的主配置文件"nginx.conf"，注意"nginx.conf"文件内有一行 
#    "include /etc/nginx/conf.d/*.conf;" ，
#    这个include指向了子配置文件的路径，此处注意include后所跟的路径一定不能出错
# 3. 第三个“-v”，把docker内子配置文件的路径也挂载了出来，注意要与 “2.” 中include指向路径一致
# 4. nginx.conf是挂载了一个文件（docker是不推荐这样用的），conf.d挂载的是一个目录
# 5. 第四个 “-v”是吧nginx的日志文件也挂载出来，方便查看

docker run \
  --name nginx \
  -d -p 8008:80  \
  -v /usr/docker/nginx/html:/usr/share/nginx/html \
  -v /etc/docker/nginx/nginx.conf:/etc/nginx/nginx.conf:ro \
  -v /etc/docker/nginx/conf.d:/etc/nginx/conf.d \
  nginx


```





在linux 发起请求，验证是否起动



```
curl http://localhost:80
```

