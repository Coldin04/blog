---
title: 解决mysql_secure_installation ERROR 2002
description: 解决这个问题，其实不妨尝试使用其他方式初始化和配置MySQL，勤看官方文档有好处
slug: mysql_error_2002
date: 2023-11-21 21:00:00+0800
#image: 2.jpg
categories:
    - mysql
    - mysql_secure_installation
    - error
tags:
    - mysql
    - mysql_sercure_installation
    - error
    - 过时的方案
    - mariadb
---

## 问题描述
在使用mysql_secure_installation命令时，出现以下错误：
```
ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/var/lib/mysql/mysql.sock' (2 "No such file or directory")
```

## 问题分析
显而易见的是，1mysql_secure_installation1命令无法连接到本地的MySQL服务，原因是MySQL服务没有启动。

但在之前的操作步骤中，已经使用了`systemctl start mariadb`命令启动了MySQL服务，为什么还是无法连接呢？

进入目录查看可知，在此目录下无`/var/lib/mysql/mysql.sock`文件。

使用` find / -name 'mysql.sock'` 命令查找，发现文件在`/tmp/mysql.sock`（部分在`/tmp/sys…/tmp/mysql.sock` 之下）。

## 解决方法
先使用 `find / -name 'mysql.sock'` 命令查找mysql.sock文件，找到文件所在目录。

复制这个目录，为其创建软链接，如下：
```
ln -s /tmp/sys.../tmp/mysql.sock（这里写的是你复制的目录） /var/lib/mysql/mysql.sock
```
再次尝试使用`mysql_secure_installation`命令，问题解决。

## 多说两句
其实，在新版的Mysql/MariaDB中，修改密码的方式有所变更，使用mysql_secure_installation命令修改密码的方式已经不再适用。[具体可以看这里](https://blog.xzr.moe/archives/4/)

新版初始化可以考虑手动初始化:
```
mysql(如果之前有密码则加上有关内容)
ALTER USER 'root'@'localhost' IDENTIFIED BY '新密码';
flush privileges;
exit;
```