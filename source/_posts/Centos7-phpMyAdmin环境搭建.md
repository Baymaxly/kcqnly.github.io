---
title: Nginx+php+Mysql环境搭建
thumbnail: /gallery/CentOS.jpg
date: 2019-10-16 18:40:06
tags: CentOS
categories: 
- CentOS 
---
服务器系统：Centos7

# 安装Nginx
```bash
yum install nginx
```
出现提示：是否要下载，输入y回车

# 安装Mysql
CentOS7版本将MySQL数据库软件从默认的程序列表中移除，用MariaDB代替了,直接使用yum安装Mysql会出问题
1. 卸载MariaDB
```bash
yum list installed | grep mariadb #检查mariadb是否已安装
yum -y remove mariadb*  #将mariadb完全卸载
```
2. 安装mysql的yum源
```bash
wget http://repo.mysql.com/mysql-community-release-el7-5.noarch.rpm
#下载yum源
rpm -ivh mysql-community-release-el7-5.noarch.rpm
#安装yum源
```

3. 安装mysql
```bash
yum install mysql-server
# 按y回车
```

4. 启动mysql
```bash
systemctl start mysqld
```

5. 设置密码
```bash
mysql_secure_installation
```
![mysql1](https://ly-object-1259106193.cos.ap-chengdu.myqcloud.com/Centos7/mysql%E5%AF%86%E7%A0%81.png)

因为之前没有设置密码，所以这里直接按回车
![mysql2](https://ly-object-1259106193.cos.ap-chengdu.myqcloud.com/Centos7/mysql%E5%AF%86%E7%A0%812.png)
输入密码
![masql3](https://ly-object-1259106193.cos.ap-chengdu.myqcloud.com/Centos7/mysql%E5%AF%86%E7%A0%813.png)
依次输入n n n y

# 安装php