





进入mysql容器

```console
docker exec -it mysql bash
```





问题：

<img src="C:\Users\李响\AppData\Roaming\Typora\typora-user-images\image-20231104132600385.png" alt="image-20231104132600385" style="zoom: 25%;" />

解决：

docker stop /mysql
docker rm /mysql





```shell
docker run --restart=always -d -v /usr/local/mysql/conf/my.cnf:/etc/mysql/my.cnf -v /usr/local/mysql/logs:/logs -v /usr/local/mysql/data/mysql:/var/lib/mysql  -p 3306:3306 --name mysql -e MYSQL_ROOT_PASSWORD=123456 mysql

```



进入mysql容器

```
docker exec -it mysql /bin/bash
```





创建一个名为 `user1` 的用户，密码为 `113456admin`，并授予适当的权限：

```
CREATE USER 'user1'@'%' IDENTIFIED BY '113456admin';
GRANT ALL PRIVILEGES ON *.* TO 'user1'@'%';

FLUSH PRIVILEGES;
```



查询mysql中的用户

```
select user, host from user;


```



<img src="C:\Users\李响\AppData\Roaming\Typora\typora-user-images\image-20231104134040988.png" alt="image-20231104134040988" style="zoom: 50%;" />







参考内容：



https://juejin.cn/post/6975374769923293191



https://blog.51cto.com/u_14184703/5672154?u_atoken=95c2e004-ccd3-419a-9df2-0c5e9710f63b&u_asession=017XM8W6PP9zpriygPckyuidYM2WXZmIkS6LYNR8EfNXg-Yf_zoy1dZP7CJf4iPXqkGCGF5GABOtk2UMmSDCviKdsq8AL43dpOnCClYrgFm6o&u_asig=05Z6NcKy0zBHRKYB83RslIJm1nlhGK4j-gflMTQ8HZIMlbxYH5yTu4RnNrv9_i-Gch-i0W83x43hWN5wFreH6WSJTKKpWO-sMl_9588aJCx5a0Oukt1iZCU_QlPaU-X5BO_jBBVMQkqCZ82jRr5JhjIQt0CrsXv63__t-pG3aJ_rfi2Y51cDfbeSn4rjDejKhpksmHjM0JOodanL5-M1Qs1bv3ZMWg_2KiPcDT_JHndVIB_ZGXGeKb0GjLqiKsCRK65vTVZYRLTS5WjWpzDfH2edoO9jcgxctPEqxW5sQf4ZLY94r_LXIIil3Y3aVPRGAe&u_aref=ynhksRMa0ohp0ohtX9hQ3xhxWdA%3D