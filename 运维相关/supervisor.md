#### .net core部署配置文件

```
#表示程序名称，用于在supervisor中显示，无特殊意义。
[program:zrAdmin] 
# 输入执行命令，这里表示执行的是dotnet ZR.Admin.WebApi.dll
command=/bin/bash -c "dotnet ZR.Admin.WebApi.dll"
# 应用程序根目录 
directory=/home/www/zrAdmin/net7.0/publish
# 是否自动启动，当 supervisor 加载该配置文件的时候立即启动它 
autostart=true
# 是否自动重启， 程序异常退出后自动重启
autorestart=true
# 该配置文件输出单个日志文件的大小，默认50M
logfile_maxbytes=50MB
# 日志备份个数 
logfile_backups=10
# 记录日志级别 
loglevel=info
# 指定标准输出日志文件 
stdout_logfile=/home/www/zrAdmin/net7.0/publish/wwwroot/logs/out.log
# 环境变量
environment=ASPNETCORE_ENVIRONMENT=Production
# 启动服务的用户
user=root
# 把stderr重定向到stdout，默认 false
redirect_stderr=true
```



#### Spring Boot 部署配置文件



```
[program:ruoyi]
stopasgroup=true
user=root
environment=JAVA_HOME=/usr/java/jdk1.8.0_202/
directory=/home/www/ruoyi
command= /usr/java/jdk1.8.0_202/bin/java  -jar  "/home/www/ruoyi/ruoyi-admin.jar"
redirect_stderr=true
stdout_logfile=/home/www/ruoyi/logs/out.log
stderr_logfile=/home/www/ruoyi/logs/out.err
```

