---
layout: post
title: Laravel开发一（Centos6下LNMP环境搭建）
date: 2017-08-01 23:00:00.000000000 +08:00
categories: [Laravel, Linux]
---

> 工欲善其事，必先利其器，想要在Laravel下开发必先有一套可以运行Laravel的环境.

在本地的开发环境我这边用的是`Homestead`，homestead为我的开发环境部署提供了很大的便利，基本不用修改什么。互联网上现在有许多关于安装LNMP环境的教程，为什么我要重复造轮子来写这篇文章呢?

* 公司的正式环境系统用的是 `centos 6.5`。

* `Laravel 5.5` 即将更新 默认PHP环境最低需要 `PHP 7.0` ，升级到PHP 7.0 方便以后更新laravel版本。

* `Mysql 5.7` 支持json的存储，这边想体验一下。

* 这边CMDB子业务需要和主系统要做分离，刚好提供了契机。

* 需要针对专门的业务属性，做专门的配置优化。

* 留存文档，供后面的同学或有相同需求的同学参考使用。

* 更新编译所依赖的各种库至最新版本。

#### 安装软件版本一览

>
> * nginx 1.13.3
> * php 7.1.7
> * mysql 5.7.19
> * composer 1.4.2
> * redis 4.0.1
>

#### 安全配置

因为公司的机器系统文件加了`i`权限所以操作前先需要去掉这几个文件的该权限。`后面全部流程安装完后i权限还是需要添加的`

```bash
chattr -i /etc/hosts.allow
chattr -i /etc/hosts.deny
chattr -i /etc/passwd
chattr -i /etc/group
chattr -i /etc/shadow
chattr -i /etc/gshadow
```

防火墙策略这边按照业务去设定就好，这边文档不做配置说明。

YUM源因为公司有内部源，所以也不做配置，如果你的yum源是国外的，建议更换成国内源。

详细请参照配置：

* `网易源` [http://mirrors.163.com](http://mirrors.163.com)

* `阿里源` [http://mirrors.aliyun.com](http://mirrors.aliyun.com)

#### 系统约定

软件源代码包存放位置：`/usr/local/src`

源码包编译安装位置：`/usr/local/软件名字`

#### 下载软件包

1. 下载 `nginx`

    [http://nginx.org/download/nginx-1.13.3.tar.gz](http://nginx.org/download/nginx-1.10.2.tar.gz)

2. 下载 `MySQL`

    [https://cdn.mysql.com/Downloads/MySQL-5.7/mysql-boost-5.7.19.tar.gz](https://cdn.mysql.com/Downloads/MySQL-5.7/mysql-boost-5.7.19.tar.gz)

3. 下载 `php`

    [http://cn2.php.net/distributions/php-7.1.7.tar.gz](http://cn2.php.net/distributions/php-5.5.38.tar.gz)

4. 下载 `pcre` `支持nginx伪静态`

    [http://ftp.exim.llorien.org/pcre/pcre-8.41.tar.gz](http://ftp.exim.llorien.org/pcre/pcre-8.39.tar.gz)

5. 下载 `openssl` `nginx扩展`

    [https://www.openssl.org/source/openssl-1.1.0f.tar.gz](https://www.openssl.org/source/openssl-1.1.0f.tar.gz)

6. 下载 `zlib` `nginx扩展`

    [http://zlib.net/zlib-1.2.11.tar.gz](http://zlib.net/zlib-1.2.8.tar.gz)

7. 下载 `cmake` `MySQL编译工具`

    [http://www.cmake.org/files/v3.9/cmake-3.9.0.tar.gz](http://www.cmake.org/files/v3.6/cmake-3.6.2.tar.gz)

8. 下载 `libmcrypt` `php扩展`

    [http://nchc.dl.sourceforge.net/project/mcrypt/Libmcrypt/2.5.8/libmcrypt-2.5.8.tar.gz](http://nchc.dl.sourceforge.net/project/mcrypt/Libmcrypt/2.5.8/libmcrypt-2.5.8.tar.gz)

9. 下载 `yasm` `php扩展`

    [http://www.tortall.net/projects/yasm/releases/yasm-1.3.0.tar.gz](http://www.tortall.net/projects/yasm/releases/yasm-1.3.0.tar.gz)

10. 下载 `t1lib` `php扩展`

    [http://download.openpkg.org/components/cache/t1lib/t1lib-5.1.2.tar.gz](http://download.openpkg.org/components/cache/t1lib/t1lib-5.1.2.tar.gz)

11. 下载 `gd` 库安装包

    [https://github.com/libgd/libgd/releases/download/gd-2.2.4/libgd-2.2.4.tar.gz](https://github.com/libgd/libgd/releases/download/gd-2.2.4/libgd-2.2.4.tar.gz)

12. 下载 `libvpx` `gd库需要`

    [https://github.com/webmproject/libvpx/archive/v1.6.1.tar.gz](https://github.com/webmproject/libvpx/archive/v1.6.1.tar.gz)

13. 下载 `tiff` `gd库需要`

    [http://download.osgeo.org/libtiff/tiff-4.0.8.tar.gz](http://download.osgeo.org/libtiff/tiff-4.0.8.tar.gz)

14. 下载 `libpng` `gd库需要`

    [https://sourceforge.net/projects/libpng/files/libpng16/1.6.31/libpng-1.6.31.tar.gz](https://sourceforge.net/projects/libpng/files/libpng16/1.6.31/libpng-1.6.31.tar.gz)

15. 下载 `freetype` `gd库需要`

    [http://ftp.twaren.net/Unix/NonGNU/freetype/freetype-2.8.tar.gz](http://ftp.twaren.net/Unix/NonGNU/freetype/freetype-2.8.tar.gz)

16. 下载 `jpegsrc` `gd库需要`

    [http://www.ijg.org/files/jpegsrc.v9b.tar.gz](http://www.ijg.org/files/jpegsrc.v9b.tar.gz)

17. `Redis` `php-redis` 扩展，`php-swoole` 扩展详见下文。

	以上软件包上传到 `/usr/local/src` 目录

	因为 `GFW` 的缘故，上面的软件包不一定不能通过原地址全部下载，可以根据自己的方法下载后上传到指定目录

#### 安装编译工具及库文件（使用yum命令安装）

```bash
yum install apr* autoconf automake bison bzip2 bzip2* cloog-ppl compat* cpp curl curl-devel\
    fontconfig fontconfig-devel freetype freetype* freetype-devel gcc gcc-c++ gtk+-devel\
    gd gettext gettext-devel glibc kernel kernel-headers keyutils keyutils-libs-devel\
    krb5-devel libcom_err-devel libpng libpng* libpng-devel libjpeg* libsepol-devel\
    libselinux-devel libstdc++-devel libtool* libgomp libxml2 libxml2-devel libXpm*\
    libX* libtiff libtiff* make mpfr ncurses* ntp openssl nasm nasm* openssl-devel\
    patch pcre-devel perl php-common php-gd policycoreutils ppl telnet t1lib t1lib*\
    wget htop vim zlib-devel
```

--------

#### 安装cmake

```
cd /usr/local/src
tar zxf cmake-3.9.0.tar.gz
cd cmake-3.9.0
./configure
make
make install
```

#### 安装MySQL

```bash
cd /usr/local/src                  #进入src目录
groupadd mysql                     #添加mysql组
useradd -r -s /sbin/nologin -g mysql mysql
                                   #创建用户mysql并加入到mysql组，不允许mysql用户直接登录系统
mkdir -p /data/mysql               #创建MySQL数据库存放目录
chown -R mysql:mysql /data/mysql   #设置MySQL数据库存放目录权限
mkdir -p /usr/local/mysql          #创建MySQL安装目录
cd /usr/local/src                  #进入软件包存放目录
tar zxf mysql-boost-5.7.19.tar.gz  #解压
cd mysql-5.7.19                    #进入mysql源码目录
```

```bash
cmake . -DCMAKE_INSTALL_PREFIX=/usr/local/mysql \
-DSYSCONFDIR=/usr/local/mysql/etc \
-DMYSQL_DATADIR=/data/mysql \
-DMYSQL_UNIX_ADDR=/tmp/mysql.sock \
-DMYSQL_USER=mysql \
-DEXTRA_CHARSETS=all \
-DDEFAULT_CHARSET=utf8 \
-DDEFAULT_COLLATION=utf8_general_ci \
-DENABLED_LOCAL_INFILE=ON \
-DWITH_INNOBASE_STORAGE_ENGINE=1 \
-DWITH_MYISAM_STORAGE_ENGINE=1 \
-DWITH_FEDERATED_STORAGE_ENGINE=1 \
-DWITH_BLACKHOLE_STORAGE_ENGINE=1 \
-DWITHOUT_EXAMPLE_STORAGE_ENGINE=1 \
-DWITH_EMBEDDED_SERVER=OFF \
-DDOWNLOAD_BOOST=1 \
-DWITH_BOOST=boost/boost_1_59_0/
```

-DENABLED_LOCAL_INFILE=ON

部分编译参数说明

```txt
-DCMAKE_INSTALL_PREFIX=/usr/local/mysql \ #安装路径
-DMYSQL_DATADIR=/usr/local/mysql/data \   #数据文件存放位置
-DSYSCONFDIR=/etc \                       #my.cnf路径
-DWITH_MYISAM_STORAGE_ENGINE=1 \          #支持MyIASM引擎
-DWITH_INNOBASE_STORAGE_ENGINE=1 \        #支持InnoDB引擎
-DMYSQL_UNIX_ADDR=/tmp/mysqld.sock \      #连接数据库socket路径
-DMYSQL_TCP_PORT=3306 \                   #端口
-DENABLED_LOCAL_INFILE=1 \                #允许从本地导入数据
-DWITH_PARTITION_STORAGE_ENGINE=1 \       #安装支持数据库分区
-DEXTRA_CHARSETS=all \                    #安装所有的字符集
-DDEFAULT_CHARSET=utf8 \                  #默认字符
-DDEFAULT_COLLATION=utf8_general_ci\      #默认校对规则
                                          #utf8_general_ci 不区分大小写
                                          #utf8_general_cs 区分大小写
-DWITH_BOOST=boost/boost_1_59_0/          #这一行是指定boost库的位置，
如果下载的是mysql-boost-5.7.19.tar.gz 那么这个包已经包含boost库的包，
那么需要在配置项中指明下载 boost 库。
```

```bash
make         #编译(大约10-20分钟)
make install #安装
```

如果编译出错, 重新编译前要删除编译失败的文件，重新编译时，需要清除旧的对象文件和缓存信息。

```bash
make clean
rm -f CMakeCache.txt
```

进入MySQL安装目录

```bash
cd /usr/local/mysql
```

```bash
rm -rf /etc/my.cnf
```

```bash
vim /usr/local/mysql/etc/my.cof
```

添加如下内容：

```txt
[mysqld]
datadir=/data/mysql
socket=/tmp/mysql.sock
user=mysql

symbolic-links=1
character-set-server=utf8

# by zuojie,do not know if it wooks
max_connect_errors = 1000
skip-name-resolve

key_buffer_size = 67108864
max_connections = 6000
#table_cache = 4096
binlog_cache_size = 4M
max_heap_table_size = 256M
sort_buffer_size = 32M
thread_cache_size = 32
query_cache_size = 256M
tmp_table_size = 256M

innodb_buffer_pool_size = 6G

[mysqld_safe]
#log-error=/var/log/mysqld.log
#pid-file=/var/run/mysqld/mysqld.pid
#err-log=/var/log/mysqld.log
#pid-file=/var/lib/mysql/mysqld.pid

[client]
default-character-set=utf8

[mysql]
default-character-set=utf8
```

添加到/etc目录的软连接

```bash
ln -s /usr/local/mysql/my.cnf /etc/my.cnf
```

生成mysql系统数据库

```bash
./bin/mysqld --user=mysql --initialize --basedir=/usr/local/mysql --datadir=/data/mysql
```

看到下面这一行，里面有生成的 `mysql密码` 请先记录下来

```bash
A temporary password is generated for root@localhost: VHsFrB3jYg.o
--initialize表示默认生成密码, --initialize-insecure 表示不生成密码, 密码为空。
```

把mysql加入系统启动

```bash
cp /usr/local/mysql/support-files/mysql.server /etc/rc.d/init.d/mysqld
chmod 755 /etc/init.d/mysqld      #增加执行权限
chkconfig mysqld on               #加入开机启动
```

编辑mysqld

```bash
vim /etc/rc.d/init.d/mysqld
```

添加如下内容：

```txt
basedir=/usr/local/mysql #MySQL程序安装路径
datadir=/data/mysql #MySQl数据库存放目录
```

添加环境变量

```bash
vim /etc/profile
```

把mysql服务加入系统环境变量：在最后添加下面这一行

```txt
export PATH=$PATH:/usr/local/mysql/bin
```

使配置立刻生效

```bash
source /etc/profile
```

下面这两行把myslq的库文件链接到系统默认的位置，这样你在编译类似PHP等软件时可以不用指定mysql的库文件地址。

```bash
ln -s /usr/local/mysql/lib/mysql /usr/lib/mysql
ln -s /usr/local/mysql/include/mysql /usr/include/mysql
```

创建目录

```bash
mkdir /var/lib/mysql
ln -s /tmp/mysql.sock /var/lib/mysql/mysql.sock #添加软链接
```

启动mysql

```bash
service mysqld start
```

修改mysql root密码,执行下面命令:

```txt
mysql_secure_installation #修改Mysql密码，输入之前生成的密VHsFrB3jYg.o回车，根据提示操作。

Press y|Y for Yes, any other key for No: y #是否安装密码安全插件？选择N

There are three levels of password validation policy:

#有以下几种密码强度选择
LOW Length >= 8
MEDIUM Length >= 8, numeric, mixed case, and special characters
STRONG Length >= 8, numeric, mixed case, special characters and dictionary file

Please enter 0 = LOW, 1 = MEDIUM and 2 = STRONG: 0
#选择0,只要8位数字即可，选1要有大写，小写，特殊字符等
```

如果安装了密码插件，想卸载用以下命令：

```bash
UNINSTALL PLUGIN validate_password ;
```

--------

#### 安装pcre

```bash
cd /usr/local/src
mkdir /usr/local/pcre
tar zxvf pcre-8.41.tar.gz
cd pcre-8.41
./configure --prefix=/usr/local/pcre
make
make install
```

#### 安装openssl

```bash
cd /usr/local/src
mkdir /usr/local/openssl
tar zxvf openssl-1.1.0f.tar.gz
cd openssl-1.1.0f
./config --prefix=/usr/local/openssl
make
make install
```

添加环境变量

```bash
vi /etc/profile
```

添加如下内容

```txt
export PATH=$PATH:/usr/local/openssl/bin
```

刷新环境变量

```txt
source /etc/profile
```

#### 安装zlib

```bash
cd /usr/local/src
mkdir /usr/local/zlib
tar zxvf zlib-1.2.11.tar.gz
cd zlib-1.2.11
./configure --prefix=/usr/local/zlib
make
make install
```

#### 安装Nginx

```bash
groupadd nginx
useradd -r -s /sbin/nologin -g nginx nginx
cd /usr/local/src
tar zxvf nginx-1.13.3.tar.gz
cd nginx-1.13.3

./configure --prefix=/usr/local/nginx \
--without-http_memcached_module \
--user=nginx --group=nginx \
--with-http_stub_status_module \
--with-http_ssl_module \
--with-http_gzip_static_module \
--with-openssl=/usr/local/src/openssl-1.1.0f \
--with-zlib=/usr/local/src/zlib-1.2.11 \
--with-pcre=/usr/local/src/pcre-8.41
```

注意：

* --with-openssl=/usr/local/src/openssl-1.1.0f

* --with-zlib=/usr/local/src/zlib-1.2.11

* --with-pcre=/usr/local/src/pcre-8.40

指向的是源码包解压的路径，而不是安装的路径，否则会报错

```bash
make
make install
```

修改nginx配置文件

```bash
vim /usr/local/nginx/conf/nginx.conf
```

修改内容如下：

```txt
# For more information on configuration, see:
#   * Official English Documentation: http://nginx.org/en/docs/
#   * Official Russian Documentation: http://nginx.org/ru/docs/

user              nginx;
worker_processes  16;

error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

pid        logs/nginx.pid;

events {
    worker_connections  5120;
}

http {
    include       mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  logs/access.log  main;
    proxy_ignore_client_abort on;
    proxy_read_timeout 300;
    proxy_send_timeout 300;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  300;
    client_max_body_size 10000m;

    #gzip  on;

    # Load config files from the /etc/nginx/conf.d directory
    # The default server is in conf.d/default.conf
    include /usr/local/nginx/conf/conf.d/*.conf;

}
```

单个域名的的nginx配置可以放到这个目录下：`/usr/local/nginx/conf/conf.d/`

配置自己的域名ngixn目录

```bash
vim /usr/local/nginx/conf/conf.d/cmdb.xunleioa.com.conf
```

配置如下：

```bash
server {
    listen 80;

    root /data/www/cmdb/public;
    index index.php index.html index.htm;

    server_name cmdb.xunleioa.com;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    error_page 500 502 503 504 /50x.html;
    location = /50x.html {
        root /usr/local/nginx/html;
    }

    location ~ .php$ {
        fastcgi_read_timeout 600;
        fastcgi_pass 127.0.0.1:9000;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }

    location ~ /.ht {
        deny all;
    }

}
```

启动Nginx

```bash
/usr/local/nginx/sbin/nginx
```

设置nginx开机启动

```bash
vim /etc/rc.d/init.d/nginx #编辑启动文件添加下面内容
```

```txt
################################################
#!/bin/sh
#
# nginx - this script starts and stops the nginx daemon
#
# chkconfig:   - 85 15
# description:  Nginx is an HTTP(S) server, HTTP(S) reverse \
#               proxy and IMAP/POP3 proxy server
# processname: nginx
# config:      /etc/nginx/nginx.conf
# config:      /etc/sysconfig/nginx
# pidfile:     /var/run/nginx.pid

# Source function library.
. /etc/rc.d/init.d/functions

# Source networking configuration.
. /etc/sysconfig/network

# Check that networking is up.
[ "$NETWORKING" = "no" ] && exit 0

nginx="/usr/local/nginx/sbin/nginx"
prog=$(basename $nginx)

sysconfig="/etc/sysconfig/$prog"
lockfile="/var/lock/subsys/nginx"
pidfile="/usr/local/nginx/logs/${prog}.pid"

NGINX_CONF_FILE="/usr/local/nginx/conf/nginx.conf"

[ -f $sysconfig ] && . $sysconfig


start() {
    [ -x $nginx ] || exit 5
    [ -f $NGINX_CONF_FILE ] || exit 6
    echo -n $"Starting $prog: "
    daemon $nginx -c $NGINX_CONF_FILE
    retval=$?
    echo
    [ $retval -eq 0 ] && touch $lockfile
    return $retval
}

stop() {
    echo -n $"Stopping $prog: "
    killproc -p $pidfile $prog
    retval=$?
    echo
    [ $retval -eq 0 ] && rm -f $lockfile
    return $retval
}

restart() {
    configtest_q || return 6
    stop
    start
}

reload() {
    configtest_q || return 6
    echo -n $"Reloading $prog: "
    killproc -p $pidfile $prog -HUP
    echo
}

configtest() {
    $nginx -t -c $NGINX_CONF_FILE
}

configtest_q() {
    $nginx -t -q -c $NGINX_CONF_FILE
}

rh_status() {
    status $prog
}

rh_status_q() {
    rh_status >/dev/null 2>&1
}

# Upgrade the binary with no downtime.
upgrade() {
    local oldbin_pidfile="${pidfile}.oldbin"

    configtest_q || return 6
    echo -n $"Upgrading $prog: "
    killproc -p $pidfile $prog -USR2
    retval=$?
    sleep 1
    if [[ -f ${oldbin_pidfile} && -f ${pidfile} ]];  then
        killproc -p $oldbin_pidfile $prog -QUIT
        success $"$prog online upgrade"
        echo
        return 0
    else
        failure $"$prog online upgrade"
        echo
        return 1
    fi
}

# Tell nginx to reopen logs
reopen_logs() {
    configtest_q || return 6
    echo -n $"Reopening $prog logs: "
    killproc -p $pidfile $prog -USR1
    retval=$?
    echo
    return $retval
}

case "$1" in
    start)
        rh_status_q && exit 0
        $1
        ;;
    stop)
        rh_status_q || exit 0
        $1
        ;;
    restart|configtest|reopen_logs)
        $1
        ;;
    force-reload|upgrade)
        rh_status_q || exit 7
        upgrade
        ;;
    reload)
        rh_status_q || exit 7
        $1
        ;;
    status|status_q)
        rh_$1
        ;;
    condrestart|try-restart)
        rh_status_q || exit 7
        restart
	    ;;
    *)
        echo $"Usage: $0 {start|stop|reload|configtest|status|force-reload|upgrade|restart|reopen_logs}"
        exit 2
esac
################################################
```

```bash
chmod 775 /etc/rc.d/init.d/nginx #赋予文件执行权限

chkconfig nginx on #设置开机启动

/etc/rc.d/init.d/nginx restart #重启
```

至此，nginx已经安装成功了，可以绑定hosts 写一个测试用的`index.html` 放到刚刚配置的目录测试了。

**如果到这里nginx还有问题，请查看相关目录文件权限，请保证nginx用户可以访问该目录或文件**

--------

#### 安装yasm

```bash
cd /usr/local/src
tar zxvf yasm-1.3.0.tar.gz
cd yasm-1.3.0
./configure
make
make install
```

#### 安装libmcrypt

```bash
cd /usr/local/src
tar zxvf libmcrypt-2.5.8.tar.gz
cd libmcrypt-2.5.8
./configure
make
make install
```

#### 安装libvpx

```bash
cd /usr/local/src
tar xvf v1.6.1.tar.gz
cd libvpx-1.6.1
./configure --prefix=/usr/local/libvpx --enable-shared --enable-vp9
make
make install
```

#### 安装tiff

```bash
cd /usr/local/src
tar zxvf tiff-4.0.8.tar.gz
cd tiff-4.0.8
./configure --prefix=/usr/local/tiff --enable-shared
make
make install
```

#### 安装libpng

```bash
cd /usr/local/src
tar zxvf libpng-1.6.31.tar.gz
cd libpng-1.6.31
./configure --prefix=/usr/local/libpng --enable-shared
make
make install
```

#### 安装freetype

```bash
cd /usr/local/src
tar zxvf freetype-2.8.tar.gz
cd freetype-2.8
./configure --prefix=/usr/local/freetype --enable-shared
make
make install
```

#### 安装jpeg

```bash
cd /usr/local/src
tar zxvf jpegsrc.v9b.tar.gz
cd jpeg-9b
./configure --prefix=/usr/local/jpeg --enable-shared
make
make install
```

#### 安装libgd

```bash
cd /usr/local/src
tar zxvf libgd-2.2.4.tar.gz #解压
cd libgd-2.1.1 #进入目录
```

因为这个版本有个bug所以编译前需要修改下代码

```bash
vim /usr/local/src/libgd-2.2.4/src/gd_gd2.c
```

include头添加如下内容：

```txt
#include <limits.h>
```

```bash
./configure --prefix=/usr/local/libgd \
--enable-shared --with-jpeg=/usr/local/jpeg \
--with-png=/usr/local/libpng \
--with-freetype=/usr/local/freetype \
--with-fontconfig=/usr/local/freetype \
--with-xpm=/usr/lib64 \
--with-tiff=/usr/local/tiff \
--with-vpx=/usr/local/libvpx

make #编译
make install #安装
```

#### 安装t1lib

```bash
cd /usr/local/src
tar zxvf t1lib-5.1.2.tar.gz
cd t1lib-5.1.2
./configure --prefix=/usr/local/t1lib --enable-shared
make without_doc
make install
```

#### 安装php

注意：如果系统是64位，请执行以下两条命令，否则安装php会出错。

```bash
cp -frp /usr/lib64/libltdl.so* /usr/lib/
cp -frp /usr/lib64/libXpm.so* /usr/lib/
```

```bash
cd /usr/local/src
tar -zxf php-7.1.7.tar.gz
cd php-7.1.7

export LD_LIBRARY_PATH=/usr/local/libgd/lib

./configure --prefix=/usr/local/php --with-config-file-path=/usr/local/php/etc \
--with-mysqli=/usr/local/mysql/bin/mysql_config --with-mysql-sock=/tmp/mysql.sock \
--with-pdo-mysql=/usr/local/mysql --with-gd=/usr/local/libgd --with-png-dir=/usr/local/libpng \
--with-jpeg-dir=/usr/local/jpeg --with-freetype-dir=/usr/local/freetype --with-xpm-dir=/usr/lib64 \
--with-zlib-dir=/usr/local/zlib --with-iconv --enable-libxml --enable-xml --enable-bcmath \
--enable-shmop --enable-sysvsem --enable-inline-optimization --enable-opcache --enable-mbregex \
--enable-fpm --enable-mbstring --enable-ftp --enable-gd-native-ttf --with-openssl --enable-pcntl \
--enable-sockets --with-xmlrpc --enable-zip --enable-soap --without-pear --with-gettext \
--enable-session --with-mcrypt --with-curl --enable-ctype --enable-mysqlnd

make #编译

make install #安装
```

设置配置文件

```bash
cp php.ini-production /usr/local/php/etc/php.ini #复制php配置文件到安装目录

rm -rf /etc/php.ini #删除系统自带配置文件

ln -s /usr/local/php/etc/php.ini /etc/php.ini #添加软链接到 /etc目录

cp /usr/local/php/etc/php-fpm.conf.default /usr/local/php/etc/php-fpm.conf #拷贝模板文件为php-fpm配置文件

ln -s /usr/local/php/etc/php-fpm.conf /etc/php-fpm.conf #添加软连接到 /etc目录

cp /usr/local/php/etc/php-fpm.d/www.conf.default /usr/local/php/etc/php-fpm.d/www.conf
```

修改php-fpm.conf

```bash
vim /usr/local/php/etc/php-fpm.conf #编辑
```

```txt
pid = run/php-fpm.pid #取消前面的分号
```


编辑www.conf

```bash
vim /usr/local/php/etc/php-fpm.d/www.conf
```

修改以下内容

```txt
user = nginx  #设置php-fpm运行账号为nginx
group = nginx #设置php-fpm运行组为nginx
```

设置 php-fpm开机启动

```bash
cp /usr/local/src/php-7.1.7/sapi/fpm/init.d.php-fpm /etc/rc.d/init.d/php-fpm #拷贝php-fpm到启动目录

chmod +x /etc/rc.d/init.d/php-fpm #添加执行权限

chkconfig php-fpm on #设置开机启动
```

编译基本php配置

```bash
vim /usr/local/php/etc/php.ini #编辑配置文件
```

修改相关参数如下：

```txt
#限制函数
disable_functions = passthru,exec,system,chroot,scandir,chgrp,chown,shell_exec,proc_open,proc_get_status,ini_alter,ini_alter,ini_restore,dl,openlog,syslog,readlink,symlink,popepassthru,stream_socket_server,escapeshellcmd,dll,popen,disk_free_space,checkdnsrr,checkdnsrr,getservbyname,getservbyport,disk_total_space,posix_ctermid,posix_get_last_error,posix_getcwd, posix_getegid,posix_geteuid,posix_getgid, posix_getgrgid,posix_getgrnam,posix_getgroups,posix_getlogin,posix_getpgid,posix_getpgrp,posix_getpid, posix_getppid,posix_getpwnam,posix_getpwuid, posix_getrlimit, posix_getsid,posix_getuid,posix_isatty, posix_kill,posix_mkfifo,posix_setegid,posix_seteuid,posix_setgid, posix_setpgid,posix_setsid,posix_setuid,posix_strerror,posix_times,posix_ttyname,posix_uname
date.timezone = PRC         #设置时区
expose_php = Off            #禁止显示php版本的信息
short_open_tag = ON         #支持php短标签
opcache.enable=1            #php支持opcode缓存
opcache.enable_cli=0
zend_extension=opcache.so   #开启opcode缓存功能
```


```bash
service php-fpm start #启动php-fpm
```

**此处nginx不用做关于php的配置，php的配置详见nginx配置： `cmdb.xunleioa.com.conf` 的配置**

#### 测试

```bash
cd /data/www/cmdb/public
```

```bash
vim index.php #新建index.php文件
```

```
<?php
    phpinfo();
?>
```

```bash
chown nginx:nginx /data/www/cmdb/public -R #设置目录所有者
chmod 700 /data/www/cmdb/public -R #设置目录权限
```

在浏览器中输入绑定的域名，会看到phpinfo页面

--------

#### 安装composer

```bash
cd /usr/local/src
mkdir composer
cd composer

php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('SHA384', 'composer-setup.php') === '669656bab3166a7aff8a7506b8cb2d1c292f042046c5a994c43155c0be6190fa0355160742ab2e1c88d40d5be660b410') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
php composer-setup.php
php -r "unlink('composer-setup.php');"
mv composer.phar /usr/local/bin/composer
```

设置composer为国内源

```bash
composer config -g repo.packagist composer https://packagist.phpcomposer.com 
```

--------

#### 安装redis

```bash
wget http://download.redis.io/releases/redis-4.0.1.tar.gz
tar -zxvf redis-4.0.1.tar.gz
cd redis-4.0.1
make
make PREFIX=/usr/local/redis install

mkdir /usr/local/redis/etc/
cp redis.conf /usr/local/redis/etc/

mkdir -p /usr/local/redis/logs
touch /usr/local/redis/logs/redis.log

mkdir -p /var/run/redis
```

修改配置redis配置文件

```bash
vim /usr/local/redis/etc/redis.conf
```

修改如下

```txt
# 修改一下配置
# redis以守护进程的方式运行
# no表示不以守护进程的方式运行(会占用一个终端)
daemonize yes

pidfile="/var/run/redis/redis.pid"

# 客户端闲置多长时间后断开连接，默认为0关闭此功能
timeout 300

# 设置redis日志级别，默认级别：notice
loglevel notice

# 设置日志文件的输出方式,如果以守护进程的方式运行redis 默认:""
# 并且日志输出设置为stdout,那么日志信息就输出到/dev/null里面去了
logfile "/usr/local/redis/logs/redis.log"

```

系统自动启动配置

```bash
vim /etc/rc.d/init.d/redis
```

添加如下内容：

```bash
#!/bin/sh## redis        init file for starting up the redis daemon## chkconfig:   - 20 80# description: Starts and stops the redis daemon.# Source function library.. /etc/rc.d/init.d/functionsname="redis-server"exec="/usr/local/redis/bin/$name"pidfile="/var/run/redis/redis.pid"REDIS_CONFIG="/usr/local/redis/etc/redis.conf"REDIS_USER="root"[ -e /etc/sysconfig/redis ] && . /etc/sysconfig/redislockfile=/var/lock/subsys/redisstart() {    [ -f $REDIS_CONFIG ] || exit 6    [ -x $exec ] || exit 5    echo -n $"Starting $name: "    daemon --user ${REDIS_USER-redis} "$exec $REDIS_CONFIG"    retval=$?    echo    [ $retval -eq 0 ] && touch $lockfile    return $retval}stop() {    echo -n $"Stopping $name: "    killproc -p $pidfile $name    retval=$?    echo    [ $retval -eq 0 ] && rm -f $lockfile    return $retval}restart() {    stop    start}reload() {    false}rh_status() {    status -p $pidfile $name}rh_status_q() {    rh_status >/dev/null 2>&1}case "$1" in    start)        rh_status_q && exit 0        $1        ;;    stop)        rh_status_q || exit 0        $1        ;;    restart)        $1        ;;    reload)        rh_status_q || exit 7        $1        ;;    force-reload)        force_reload        ;;    status)        rh_status        ;;    condrestart|try-restart)        rh_status_q || exit 0        restart        ;;    *)        echo $"Usage: $0 {start|stop|status|restart|condrestart|try-restart}"        exit 2esacexit $?
```

给脚本增加运行权限

```bash
chmod 755 /etc/rc.d/init.d/redis
```

查看服务列表

```bash
chkconfig --list
chkconfig --add redis
chkconfig --level 2345 redis on
```

Redis 启动、停止测试

```bash
service redis start   #或者 /etc/init.d/redis start
service redis stop   #或者 /etc/init.d/redis stop
```

# 查看redis进程

```bash
ps -el|grep redis
```

调整下内存分配使用方式并使其生效

* #此参数可用的值为0,1,2
* #0表示当用户空间请求更多的内存时，内核尝试估算出可用的内存
* #1表示内核允许超量使用内存直到内存用完为止
* #2表示整个内存地址空间不能超过swap+(vm.overcommit_ratio)%的RAM值

```bash
echo "vm.overcommit_memory=1">>/etc/sysctl.conf
sysctl -p
```

--------

#### php-redis 扩展安装

```bash
cd /usr/local/src
wget http://pecl.php.net/get/redis-3.1.3.tgz
tar zxf redis-3.1.3.tgz
cd redis-3.1.3
phpize
./configure --with-php-config=/usr/local/php/bin/php-config
make
make install
```

#### php-swoole 扩展安装

```bash
cd /usr/local/src
wget https://github.com/swoole/swoole-src/archive/v1.9.17.tar.gz
tar xvf v1.9.17.tar.gz
swoole-src-1.9.17
phpize
./configure
make
make install
```

`php.ini`注册安装的扩展

```bash
vim /usr/local/php/etc/php.ini
```

在文件末尾添加如下内容：

```txt
extension = redis.so
extension = swoole.so
```

本文在整个过程中可能有地方没有设置各个软件的环境变量，特在这里补充一下,具体配置如下：

```bash
vim /etc/profile
```

最后一行添加如下内容

```bash
export PATH=$PATH:/usr/local/openssl/bin:/usr/local/mysql/bin:/usr/local/nginx/sbin:/usr/local/php/bin:/usr/local/redis/bin:/usr/local/bin
```

刷新全局生效：

```bash
source /etc/profile
```

基本的配置就完成了，这边在强调下，这个过程中如果有问题，请查看打出生成的日志，以及请查看相关目录文件权限。

