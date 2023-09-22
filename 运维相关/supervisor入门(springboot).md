

![supervisor入门](https://picx.zhimg.com/70/v2-2946b7165af790067a56db14fb0e8c41_1440w.image?source=172ae18b&biz_tag=Post)

# supervisor入门

[![北牧苍狼](https://picx.zhimg.com/v2-3b8922fe6ff0a42fc58a2155af04066b_l.jpg?source=172ae18b)](https://www.zhihu.com/people/tian-ya-22-1)

[北牧苍狼](https://www.zhihu.com/people/tian-ya-22-1)

进阶中的程序员、脚踏实地吧

关注他

13 人赞同了该文章

supervisor是最近接触到的，用于管理Linux上的Java应用。

**supervisor官网**：[http://www.supervisord.org/introduction.html](https://link.zhihu.com/?target=http%3A//www.supervisord.org/introduction.html)

看一下官网的定义：Supervisor是一个客户端/服务器系统，它允许用户在类unix操作系统上控制多个进程

supervisor主要包含以下四个部分：

- **supervisord**：这个是supervisor服务的主要管理器，负责管理我们配置的子进程，包括重启崩溃或异常退出的子进程，同时也响应来自客户端的请求。
- **supervisorctl**：supervisord服务的客户端命令行。听过这个，我们可以获得由主进程控制的子进程的状态，停止和启动子进程，并获得主进程的运行列表。
- **Web Server**：和supervisorctl功能娉美。这个是通过web界面查看和控制进程状态。
- **XML-RPC Interface**：服务于web UI的同一个HTTP服务器提供一个XML-RPC接口，可以用来询问和控制管理程序及其运行的程序

**运行环境：**

supervisor可以运行在大多数类UNIX系统，但是不能运行在任何windows系统，Supervisor运行在python3版本3.4或更高版本以及python2版本2.7上工作。

**supervisor安装**

我这边之前是在网上搜索其他人博客安装的。这里只介绍这种方式，当然还有其他方式，后面的文章中再介绍。

参考博客：[详解Supervisor进程守护监控 - 请叫我头头哥 - 博客园](https://link.zhihu.com/?target=https%3A//www.cnblogs.com/toutou/p/supervisor.html)

1、首先确保网络通畅

2、然后安装Python，执行yum install python-setuptools

3、安装supervisor，执行easy_install supervisor

4、查看是否安装成功，echo_supervisord_conf，如果有输出内容则表示成功

![img](https://pic4.zhimg.com/80/v2-967b8315558a8acf04660ea03da0bf8f_1440w.webp)

5、创建目录，mkdir /usr/supervisor，同时生成配置文件，echo_supervisord_conf > /usr/supervisor/supervisord.conf（这里我是跟着上面博客创建的这个地址的目录，其它很多也创建在/etc/supervisor下，差别不大）

6、启动supervisord。supervisord -c /usr/supervisor/supervisord.conf

这个时候基本的步骤已经完成，终端输入supervisorctl status。会出现空的输出，因为还没有配置我们自己的项目。

**supervisor配置项目**

首先我们需要修改上面生成的supervisord.conf文件。文件最后取消掉最后的两行注释，也就是去掉前面的分号。也就是下面这样。

```text
[include]
files = supervisord.d/*.ini
```

上面的我们把supervisord.d目录下所有ini文件都包含进来了。这样是没问题的，但是很多也喜欢用conf结尾的文件。这个看自己选择。

接下来是创建supervisor.d目录，mkdir /usr/supervisor/supervisord.d/

我这里创建的是conf文件。demo.conf，内容如下：

```text
[program:demo]
stopasgroup=true
user=root
environment=JAVA_HOME=/usr/local/java/jdk1.8.0_181
directory=/home/test/project/demo
command=sh /home/test/project/demo/bin/demo.sh
redirect_stderr=true
stdout_logfile=/home/test/project/demo/logs/out.log
stderr_logfile=/home/test/project/demo/logs/out.err
```

environment指定linux环境Java的安装位置，如果忘记了，在命令行输入echo $JAVA_HOME来得到。

directory是项目的地址。

command指定项目启动脚本位置。

stdout_logfile和stderr_logfile指定日志的位置

demo这个文件夹下包含的文件夹如下

bin包含启动脚本，lib包含项目jar包，log包含项目的日志

![img](https://pic1.zhimg.com/80/v2-d91d2103ecae0805118454cbb6861e34_1440w.webp)

我这里demo项目是通过idea生成的一个最简单的mvc项目。流程如下：idea-》file-》new-》project

![img](https://pic4.zhimg.com/80/v2-b158df11a430272640dd4daa4b23b81f_1440w.webp)

![img](https://pic2.zhimg.com/80/v2-21b991a93a888e6f2186b304728046b5_1440w.webp)

![img](https://pic4.zhimg.com/80/v2-9d59cc40693b4047ba7b1dda5c268c77_1440w.webp)

最后点击finish。等待几分钟后，会生成初始化项目。

然后在demo目录下新建Test文件，很简单，内容如下：

```java
package com.example.demo.controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
@RestController
@RequestMapping("/main")
public class Test {
    @RequestMapping("/test")
    public String test() {
        return "OK";
    }
}
```

启动项目，看下是否通过http://localhost:8080/main/test能成功返回OK。

上面步骤之后，使用idea右侧maven install打包项目，然后上传到lib目录下。

接下来看一下demo.sh，如下：

```text
java -jar /home/test/project/demo/lib/demo-0.0.1-SNAPSHOT.jar
```

之前我的脚本是

```text
nohup java -jar /home/test/project/demo/lib/demo-0.0.1-SNAPSHOT.jar &
```

这样是有问题的，这样启动的进程是后台进程。而supervisor是不能管理后台进程。所以这样配置，重启supervisor就会报程序退出太快。

上面配置好了脚本之后，执行supervisorctl reload。重启supervisor，然后supervisorctl status查看应用的启动情况。

可能会遇到以下问题：

1、Unlinking stale socket /tmp/supervisor.sock，解决方案，执行unlink /tmp/supervisor.sock

2、如果执行reload之后没反应，可以先ps -ef|grep supervisord查到supervisor的进程号，然后kill 进程号。

**supervisor常用的命令**

这里介绍常用的几个

```text
update 更新新的配置到supervisord（不会重启原来已运行的程序）
reload，载入所有配置文件，并按新的配置启动、管理所有进程（会重启原来已运行的程序）
start xxx: 启动某个进程
restart xxx: 重启某个进程
stop xxx: 停止某一个进程(xxx)，xxx为[program:theprogramname]里配置的值
stop groupworker: 重启所有属于名为groupworker这个分组的进程(start,restart同理)
stop all，停止全部进程，注：start、restart、stop都不会载入最新的配置文
reread，当一个服务由自动启动修改为手动启动时执行一下就ok
```

**supervisor开机自启动**

在目录/usr/lib/systemd/system/ 新建文件supervisord.service,并添加配置内容

```text
[Unit]
Description=Process Monitoring and Control Daemon
After=rc-local.service nss-user-lookup.target

[Service]
Type=forking
ExecStart=/usr/bin/supervisord -c /usr/supervisor/supervisord.conf ;开机启动时执行
ExecStop=/usr/bin/supervisord shutdown
ExecReload=/usr/bin/supervisord reload
killMode=process
Restart=on-failure
RestartSec=42s

[Install]
WantedBy=multi-user.target
```

启动服务：systemctl enable supervisord

验证是否开机启动：systemctl is-enabled supervisord

**Web Server配置**

除了通过Linux的命令行输入supervisorctl进入到supervisor里面控制每个程序之外，还可以通过web界面的方式进行管理。通过以下几个步骤开启。

1、修改supervisor的配置文件。本文中的是vi /usr/supervisor/supervisord.conf

```text
[inet_http_server]         ; inet (TCP) server disabled by default
port=0.0.0.0:9001        ; ip_address:port specifier, *:port for all iface
username=user              ; default is no username (open server)
password=123               ; default is no password (open server)
```

修改如上，后面username和password可以自己选择是否放开，放开注释则每次systemctl之后都需要输入用户名和密码，稍微麻烦一点，同时在web界面也需要输入。

2、重启supervisor。通过supervisorctl reload或者kill掉supervisor进程，再supervisord -c /usr/supervisor/supervisord.conf

成功后如下：

![img](https://pic3.zhimg.com/80/v2-21b792a3ee7265adbd02197d42e1ec8a_1440w.webp)

后面有更多了解的时候再补充。。



