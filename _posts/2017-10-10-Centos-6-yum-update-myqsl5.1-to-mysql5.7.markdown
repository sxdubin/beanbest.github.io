---
layout: post
title: Centos6 mysql5.1通过yum升级到mysql5.7
date: 2017-10-10 10:00:00.000000000 +08:00
categories: [Mysql, Linux]
---

> Centos6 mysql5.1通过yum升级到mysql5.7

### 升级前的准备

下载更新源：https://dev.mysql.com/downloads/mysql/

查询相应系统版本，相应mysql版本源并下载  

```
wget https://repo.mysql.com/mysql57-community-release-el6-11.noarch.rpm
```

安装Mysql5.7源

```
yum install mysql57-community-release-el6-11.noarch.rpm
```

### 升级Mysql

首先如果有主备同步先停掉主备同步；

```
stop slave;
```

停止mysql服务

```
service mysqld stop
```

升级mysql

```
yum update mysql-server -y
```

上面执行完 mysql的 升级基本完成了。

### 善后工作

因为5.7的版本对mysql users 表中的密码字段发生了改变 以及规范了一些对全局库表的一些规范，所以先要升级改表。 

通过免密码登录mysql  `启动前请参考备注 修改/etc/my.cnf的配置文件`

```
mysqld_safe --sikp-grant-tables &
```

用mysql_upgrade命令 对全局表结构进行升级

```
mysql_upgrade
```

通过上面的操作基本就完成了mysql的升级。

之后清理后台的mysql程序

```
ps aux | grep mysql
```

kill掉 过滤出来的进程号

```
kill -9 XXX
```

启动mysql  `启动前请参考备注 修改/etc/my.cnf的配置文件`

```
servie mysqld start
```


### 备注 my.cnf配置文件修改

在mysqld下添加如下配置:

该字段是为了兼容之前版本的一些旧的属性，详情请查看官方文档。

```
sql_mode = "STRICT_TRANS_TABLES,NO_ENGINE_SUBSTITUTION,NO_ZERO_DATE,NO_ZERO_IN_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER"
```

注释掉如下字段 因为该字段在新版本中 已经不支持。

```
#table_cache = 4096
```

### 备注 遇到的坑

**升级有风险，数据一定要做好备份。**
**升级有风险，数据一定要做好备份。**
**升级有风险，数据一定要做好备份。**

如果遇到1032错误码 请跳过。

修改/etc/my.cnf mysqld 下添加如下：

```
slave-skip-errors=1032
```


