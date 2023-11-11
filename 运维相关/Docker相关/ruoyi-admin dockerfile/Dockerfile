FROM openjdk:8-jre

RUN mkdir -p /ruoyi/server/logs \
    /ruoyi/server/temp \
    /ruoyi/skywalking/agent

#WORKDIR指令用于指定容器的一个目录， 容器启动时执行的命令会在该目录下执行。
WORKDIR /ruoyi/server
#将当前ruoyi-admin.jar 复制到容器对应目录下
ADD ruoyi-admin.jar /ruoyi/server/ruoyi-admin.jar
EXPOSE 8688

#容器启动时执行的命令
CMD ["nohup","java","-jar","ruoyi-admin.jar","&"]
