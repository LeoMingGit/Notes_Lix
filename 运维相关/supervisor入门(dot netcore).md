

![.NET Core部署到linux最全解决方案，进阶篇(Supervisor+Nginx)](https://pic1.zhimg.com/70/v2-e07ce9bc6a0847fb71d659a8e07a5d0f_1440w.image?source=172ae18b&biz_tag=Post)

# .NET Core部署到linux最全解决方案，进阶篇(Supervisor+Nginx)

> 在[.NET Core部署到linux(CentOS)最全解决方案，常规篇](https://link.zhihu.com/?target=http%3A//blog.rdiframework.net/article/244)一文，我们详细讲解了传统的.NET Core部署到Linux服务器的方法，学到了Linux在虚拟机下的安装、Xshell,Xftp的使用方法、git在linux下的交互使用以及.net core在linux下的发布与运行全过程。本文讲讲解通过使用Supervisor+Nginx的组合来实现.net core的高效部署。

## **1、Supervisor**

### **1.1、Supervisor介绍**

官网：[http://supervisord.org](https://link.zhihu.com/?target=http%3A//www.eryajf.net/go%3Furl%3Dhttp%3A//supervisord.org)，源码位置：[https://github.com/Supervisor/supervisor](https://link.zhihu.com/?target=https%3A//github.com/Supervisor/supervisor)

Supervisor是用Python开发的一套通用的进程管理程序，能将一个普通的命令行进程变为后台daemon，并监控进程状态，异常退出时能自动重启。

它是通过fork/exec的方式把这些被管理的进程当作supervisor的子进程来启动，这样只要在supervisor的配置文件中，把要管理的进程的可执行文件的路径写进去即可。也实现当子进程挂掉的时候，父进程可以准确获取子进程挂掉的信息的，可以选择是否自己启动和报警。supervisor还提供了一个功能，可以为supervisord或者每个子进程，设置一个非root的user，这个user就可以管理它对应的进程。

### **1.2、为什么要用Supervisor**

在linux或者unix操作系统中，守护进程（Daemon）是一种运行在后台的特殊进程，它独立于控制终端并且周期性的执行某种任务或等待处理某些发生的事件。由于在linux中，每个系统与用户进行交流的界面称为终端，每一个从此终端开始运行的进程都会依附于这个终端，这个终端被称为这些进程的控制终端，当控制终端被关闭的时候，相应的进程都会自动关闭。但是守护进程却能突破这种限制，它脱离于终端并且在后台运行，并且它脱离终端的目的是为了避免进程在运行的过程中的信息在任何终端中显示并且进程也不会被任何终端所产生的终端信息所打断。它从被执行的时候开始运转，直到整个系统关闭才退出。

此处的创建守护进程，是指发布在Linux上 [http://asp.net](https://link.zhihu.com/?target=http%3A//asp.net) core 程序的dotnet xxx.dll命令的宿主进程创建一个守护进程。在 Linux 上有很多可以管理进程的工具，我们使用 Supervisor 来做这个事情。

原因有两点：

> ①、它是微软官方文档推荐的，降低学习成本。 ②、它并不一定是最好的，但一定是文档最全的。

### **1.3、Supervisor4大组件**

- supervisord

> 主进程,负责管理进程的server，它会根据配置文件创建指定数量的应用程序的子进程，管理子进程的整个生命周期，对crash的进程重启，对进程变化发送事件通知等。同时内置web server和XML-RPC Interface，轻松实现进程管理。。该服务的配置文件在/etc/supervisor/supervisord.conf。

- supervisorctl

> 客户端的命令行工具，提供一个类似shell的操作接口，通过它你可以连接到不同的supervisord进程上来管理它们各自的子程序，命令通过UNIX socket或者TCP来和服务通讯。用户通过命令行发送消息给supervisord，可以查看进程状态，加载配置文件，启停进程，查看进程标准输出和错误输出，远程操作等。服务端也可以要求客户端提供身份验证之后才能进行操作。

- Web Server

> superviosr提供了web server功能，可通过web控制进程(需要设置[inethttpserver]配置项)

- XML-R- #supervisor

> 一个Linux/Unix系统上的进程监控工具 一个Python开发的通用的进程管理程序 可以管理和监控Linux上面的进程 能将一个普通的命令行进程变为后台daemon，并监控进程状态，异常退出时能自动重启 不过同daemontools一样，它不能监控daemon进程

### **1.4、安装Supervisor**

相应安装建议以管理员方式登录系统，非管理员请以sudo命令安装。

> Linux sudo命令以系统管理者的身份执行指令，也就是说，经由 sudo 所执行的指令就好像是 root 亲自执行。

1、安装EPEL源的命令如下：

```text
sudo yum -y install epel-release
```

![img](https://pic1.zhimg.com/80/v2-2afeeda44d1c6822ce7f20b363f82724_1440w.webp)

2、执行如下命令安装supervisor：

```text
sudo yum -y install supervisor
```

![img](https://pic3.zhimg.com/80/v2-bac0c41a92a837613a0b42e82d01b852_1440w.webp)

3、设置开机启动：

```text
systemctl enable supervisord
```

![img](https://pic4.zhimg.com/80/v2-c956eb0db7bddf2ce7e28eed71c43eff_1440w.webp)

4、启动supervisord

```text
systemctl start supervisord
```

5、查看supervisord状态

```text
systemctl status supervisord
```



![img](https://pic2.zhimg.com/80/v2-9d8c2fa5d4a5b743a9bd41fb904a5f5d_1440w.webp)



### **1.5、Supervisor配置及使用**

通过vi命令或者xftp修改配置文件开启web界面访问，如下图所示，分别取消inet_http_server等四个配置的注释：

```text
vi /etc/supervisord.conf
```



![img](https://pic3.zhimg.com/80/v2-7342f60cd386a54f567e45839ea2785e_1440w.webp)



执行如下命令，重新加载配置文件：

```text
supervisorctl reload
```

然后在浏览器打开http://你的ip:9001，输入上面我们设置的用户名：user1,密码:123456后，如图所示：



![img](https://pic3.zhimg.com/80/v2-508cb81ca4f4317a7c3b08d6cf5b019e_1440w.webp)





![img](https://pic4.zhimg.com/80/v2-ec3041d3ef3abc61843d223ff15e9dd3_1440w.webp)



看到上图这个界面，就表示supervisor安装完成了。

切换到/etc/supervisord.d目录，在此目录创建名称为：core50test.ini的ini文件，内容如下:

```ini
#表示程序名称，用于在supervisor中显示，无特殊意义。
[program:core50test] 
# 输入执行命令，这里表示执行的是dotnet Core50Test.dll
command=/bin/bash -c "dotnet Core50Test.dll"
# 应用程序根目录 
directory=/root/app_data/core50test/publish
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
stdout_logfile=/root/app_data/data/logs/core50test/core50test.out.log
# 环境变量
environment=ASPNETCORE_ENVIRONMENT=Production
# 启动服务的用户
user=root
# 把stderr重定向到stdout，默认 false
redirect_stderr=true
```

上述代码包含了注释信息，参考精減版配置如下：

```ini
[program:core50test]
command=/bin/bash -c "dotnet Core50Test.dll"
directory=/root/app_data/core50test/publish
autostart=true
autorestart=true
logfile_maxbytes=50MB
logfile_backups=10
loglevel=info
stdout_logfile=/root/app_data/data/logs/core50test/core50test.out.log
environment=ASPNETCORE_ENVIRONMENT=Production
user=root
redirect_stderr=true
```

**注意**：stdout_logfile指向的文件夹一定要先创建，否则无法启动，上述配置文件中的内容需要根据用户实际情况修改，如我当前登录的用户是：**yonghu**，你们是其他的就做相应的修改即可。

然后执行如下命令来重新加载配置：

```text
supervisorctl reload
```

命令执行成功后， 刷新浏览器，可以看到如下界面：



![img](https://pic2.zhimg.com/80/v2-3c70bc8e1bc68ea165fdcf4c20a90011_1440w.webp)



当界面显示running时，则表示我们我们刚刚配置的.net core应用运行起来了。

如下图所示。

![img](https://pic2.zhimg.com/80/v2-4ba8719d0ab5b92e559c7254f3342eb9_1440w.webp)

我们可以方便的通过supervisor提供的web管理界面对我们的应用进行启动与停止，查看日志等操作，非常的方便，丝般润滑般的爽呀。

![img](https://pic3.zhimg.com/80/v2-a2ec3914b1ab8ba071a6b5ae5469be6a_1440w.webp)

查看日志：

![img](https://pic1.zhimg.com/80/v2-8900132690e00e291eac710f68bebe70_1440w.webp)

**1.6、Supervisor常用命令**

```text
### 查看supervisorctl支持的命令
# supervisorctl help    
default commands (type help <topic>):
=====================================
add    exit      open  reload  restart   start   tail   
avail  fg        pid   remove  shutdown  status  update 
clear  maintail  quit  reread  signal    stop    version

### 查看当前运行的进程列表
# supervisorctl status
```

![img](https://pic2.zhimg.com/80/v2-4cd961e401475d5aab6a7a3d409f2799_1440w.webp)



- update 更新新的配置到supervisord（不会重启原来已运行的程序）
- reload，载入所有配置文件，并按新的配置启动、管理所有进程（会重启原来已运行的程序）
- start xxx: 启动某个进程
- restart xxx: 重启某个进程
- stop xxx: 停止某一个进程(xxx)，xxx为[program:theprogramname]里配置的值
- stop groupworker: 重启所有属于名为groupworker这个分组的进程(start,restart同理)
- stop all，停止全部进程，注：start、restart、stop都不会载入最新的配置文
- reread，当一个服务由自动启动修改为手动启动时执行一下就ok

最常用的几个命令为：

```text
#启动所有
supervisorctl start all

# 重启所有
supervisorctl restart all

# 停止所有
supervisorctl stop all

#PS:要操作某个服务，把all换成服务名即可
#查看服务状态
supervisorctl status
```

## **2、使用Nginx**

在前面文章中，我们已经可以非常方便的对web应用进行部署与管理了，但还存在一个问题，我们的应用程序默认是绑定的5000端口，如果要指定80端口或者配置域名该怎么处理呢？下面就该nginx登场了。

### **2.1、Nginx介绍**

Nginx是一款轻量级的Web 服务器/反向代理服务器及电子邮件（IMAP/POP3）代理服务器，在BSD-like 协议下发行。其特点是占有内存少，并发能力强，事实上nginx的并发能力在同类型的网页服务器中表现较好，中国大陆使用nginx网站用户有：百度、京东、新浪、网易、腾讯、淘宝等。

### **2.2、Nginx安装**

安装方式参考：[http://nginx.org/en/linux_packages.html#RHEL-CentOS](https://link.zhihu.com/?target=http%3A//nginx.org/en/linux_packages.html%23RHEL-CentOS)

安装先决条件：

```text
 sudo yum install -y yum-utils
```



![img](data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='1093' height='636'></svg>)



设置yum存储库，先创建一下内容的文件：/etc/yum.repos.d/nginx.repo

```text
[nginx-stable]
name=nginx stable repo
baseurl=http://nginx.org/packages/centos/$releasever/$basearch/
gpgcheck=1
enabled=1
gpgkey=https://nginx.org/keys/nginx_signing.key
module_hotfixes=true

[nginx-mainline]
name=nginx mainline repo
baseurl=http://nginx.org/packages/mainline/centos/$releasever/$basearch/
gpgcheck=1
enabled=0
gpgkey=https://nginx.org/keys/nginx_signing.key
module_hotfixes=true
```



![img](data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='675' height='238'></svg>)



默认情况下，使用稳定 nginx 包的存储库。如果要使用主线 nginx 包，请运行以下命令：

```text
 yum-config-manager --enable nginx-mainline
```

运行如下命令安装nginx：

```text
 sudo yum install -y nginx
```

![img](data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='893' height='646'></svg>)



设置开机启动：

```text
systemctl enable nginx
```

启动nginx：

```text
 systemctl start nginx
```



![img](data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='1395' height='357'></svg>)



此时，就可以在浏览器通过ip访问了：[http://你的ip](https://link.zhihu.com/?target=http%3A//xn--ip-0p3cm89l/)，界面如下：



![img](data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='1385' height='438'></svg>)



### **2.3、Nginx部署**

nginx安装完成后，切换到/etc/nginx/conf.d目录，修改default.conf文件内容，如下所示：

```text
server {
    listen       80;
    server_name  localhost;
    location / {
        proxy_pass http://0.0.0.0:5000;
    }
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }
}
```

保存后，执行如下命令，重新加载配置：

```text
nginx -s reload
```

然后再次访问http://你的ip，一切正常的话应该可以看到如下的界面，表示我们的.NET Core程序已经完美运行在linux系统了。



![img](data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='1399' height='543'></svg>)



如果部署后遇到类型下面这样的错误



![img](data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='707' height='100'></svg>)



出现这样的问题，有可能的是因为SeLinux的限制，执行如下命令之后，再刷新页面：

```text
setenforce 0     
```

> selinux(security enhanced linux)安全增强型linux系统，它是一个linux内核模块，也是linux的一个安全子系统。
> selinux的主要作用就是最大限度地减小系统中服务进程可访问的资源（最小权限原则）

如果设置后还是不能解决，可以查看nginx的日志了，默认的日志路径为：/var/log/nginx

通过**setenforce 0**命令，只是临时实效，重启后会失效。

可以通过修改/etc/selinux/config 文件，将**SELINUX=enforcing改为SELINUX=disabled**，然后重启，即可永久生效。



![img](data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='667' height='264'></svg>)



通过近两篇文章的介绍，我们需要更新应用，只需要将代码提交到git仓库，然后在服务器中执行git pull和dotnet publish即可。

如果熟悉shell的话，可以通过编写shell命令一键执行应用程序的更新，代码示例：

```text
# !/bin/bash
cd /root/app_data/source/core50test
git pull
dotnet publish -o /root/app_data/core50test/publish
supervisorctl restart core50test
```

将上述的代码保存为sh文件，上传到服务器，并设置权限。如下图所示：

![img](data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='746' height='535'></svg>)

代码提交到git仓库后，执行如下命令：

```text
./build.sh
```

执行结果如下图所示：

![img](data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='1020' height='408'></svg>)

更新后重新运行，已经更新。

![img](data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='1400' height='519'></svg>)



这儿可能有的小伙伴会遇到一个小小的坑要注意，shell脚本写得没有问题，执行会报类似这样的错误

```text
$'\r':command not found
```

出现这种问题是因为windows下的文件换行用的是\r\n，而linux系统用的是\n，如果在win下的文档上传到linux，就有可能出现这样的问题，只需用vi打开shell脚本文件，然后使用命令:set ff=unix，保存文件即可。

![img](data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='849' height='636'></svg>)

supervisor一个作为守护线程，用于维护应用程序的生命周期的，nginx则是作为反向代理使用，配置shell可以做到高效部署，非常的方便。

------



一路走来数个年头，感谢[http://RDIFramework.NET](https://link.zhihu.com/?target=http%3A//RDIFramework.NET)框架的支持者与使用者，大家可以通过下面的地址了解详情。

[http://RDIFramework.NET](https://link.zhihu.com/?target=http%3A//RDIFramework.NET)官方网站：[http://www.rdiframework.net/](https://link.zhihu.com/?target=http%3A//www.rdiframework.net/)

[http://RDIFramework.NET](https://link.zhihu.com/?target=http%3A//RDIFramework.NET)官方博客：[http://blog.rdiframework.net/](https://link.zhihu.com/?target=http%3A//blog.rdiframework.net/)

特别说明，框架相关的技术文章请以官方网站为准，欢迎大家收藏！

[http://RDIFramework.NET](https://link.zhihu.com/?target=http%3A//RDIFramework.NET)框架由海南国思软件科技有限公司专业团队长期打造、一直在更新、一直在升级，请放心使用！

欢迎关注[http://RDIFramework.NET](https://link.zhihu.com/?target=http%3A//RDIFramework.NET)框架官方微信公众号（微信号：**guosisoft**），及时了解最新动态。

使用微信扫描二维码立即关注



发布于 2021-01-16 11:52

[Linux](https://www.zhihu.com/topic/19554300)

[虚拟机](https://www.zhihu.com/topic/19575631)

[.NET Core](https://www.zhihu.com/topic/20050967)

赞同 2添加评论

分享

喜欢收藏申请转载



赞同 2

分享

![img](https://picx.zhimg.com/v2-f9c41af16c14316659896155effea38a_l.jpg?source=32738c0c)

发布一条带图评论吧



还没有评论，发表第一个评论吧

### 推荐阅读

- ![Nginx的简介和安装（Linux）](https://picx.zhimg.com/v2-21e0182b8205ec5862adc04adf388d8a_250x0.jpg?source=172ae18b)

- # Nginx的简介和安装（Linux）

- 终端研发部发表于nginx

- ![.NET Core部署到linux最全解决方案，高阶篇(Docker+Nginx 或 Jexus)](https://picx.zhimg.com/v2-025d1c1c747d238109926cee6de99210_250x0.jpg?source=172ae18b)

- # .NET Core部署到linux最全解决方案，高阶篇(Docker+Nginx 或 Jexus)

- 国思RDI...发表于.net开...

- # Ubuntu下部署 Nginx Web 服务器

- 一、执行下面的命令安装Nginx sudo apt-get install nginx 二、初试 Nginx 启动 Nginx执行下面的命令启动Nginx sudo service nginx start启动之后，我们看一下 Nginx 是否处于运行状态。 su…

- 江风1982

- # Linux安装Nginx详细教程

- 环境准备1.因为Nginx依赖于gcc的编译环境，所以，需要安装编译环境来使Nginx能够编译起来。 命令：yum install gcc-c++ 显示完毕，表示安装完成： 2.Nginx的http模块需要使用pcre来解析正则…

- Java架构成长之路