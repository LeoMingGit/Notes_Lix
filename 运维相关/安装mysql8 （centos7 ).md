#### **卸载系统自带的MariaDB（MySQL）**



```
rpm -qa | grep mariadb

sudo yum remove mariadb-libs-5.5.68-1.el7.x86_64

```





#### 使用npm包安装





```
sudo wget https://repo.mysql.com/mysql80-community-release-el7-1.noarch.rpm


sudo yum install mysql80-community-release-el7-1.noarch.rpm

sudo yum -y install mysql-community-server




```

#### 安装mysql报错及解决

##### 报错1：

![](E:\代码练习\Notes_Lix\运维相关\images\Snipaste_2023-09-28_14-41-02.png)

 Failing package is: mysql-community-server-8.0.34-1.el7.x86_64
 GPG Keys are configured as: file:///etc/pki/rpm-gpg/RPM-GPG-KEY-mysql

```
rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2022
```

