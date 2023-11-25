

#### 拉去 docker 

#### 

```
sudo docker pull mcr.microsoft.com/mssql/server:2022-latest
```



#### 运行  docker 



```
docker run -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=8llq$yiz$FjJXbQh,,,DDAA' -p 1433:1433 --name sqlserver -d mcr.microsoft.com/mssql/server:2022-latest
```





还原数据库



 ![](https://gitee.com/N2N/note_images/raw/00995c71211176dbf7078a68d5a709b53f28c320/Snipaste_2023-11-25_23-53-30.png)





把 centos 上的 备份数据拷贝到虚拟中

![](https://gitee.com/N2N/note_images/raw/67175a46dda8591d62b9ff2903e4cabb7bd3b5ca/Snipaste_2023-11-25_23-57-00.png)



```
docker cp /home/mssql/WorkSchedule.bak sqlserver:/var/opt/mssql/data/
```



117.72.43.18

sa

8llq$yiz$FjJXbQh,,,DDAA

