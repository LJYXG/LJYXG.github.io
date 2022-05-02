---
title: Debian安装Apache简单记录
date: 2022-02-15 20:10:16
tags: 
 - Apache
 - Debian
 - Linux
categories: HTTP-Server
---

前记：前一阵不是CentOS不是停止维护了嘛，然后论坛里([V2EX](https://www.v2ex.com/))了解到，相对来说 国内用CentOS的比较多可能是因为收到台湾的影响，国外来说用Debian和Ubuntu Server作为服务器系统的还是比较多的，所以当时是萌生了尝试下Debian做服务器系统的想法，之前入门就是CentOS,对这个还是比较期待的不过一直没机会。

<!-- more -->

这次恰好得知一个平台有个免费小机器，拿这次机会正好试下在Debian Linux的环境下搭建下Apache服务(MySQL,PHP先咕咕咕咕)。

小机器情况 
1H 512M 5G Ipv6-Only

鸽子也比较懒，没有选择下载源码编译安装大神的操作，而是采用了很懒的操作，利用Debian的包管理器apt。
首先更新包管理器

```bash
apt update
```

我们知道哈 在CentOS的yum包管理器下 安装apache我们用的是 yum install httpd 包名为"httpd"
不过这一点在Debian就有所不同了,在Debian指令为 apt-get install apache2 包名为"apache2"
这个包名首先就是不同的 了解到这一点后 就可以正式安装了(本文中指令默认用户为root)

```shell
apt-get install apache2
```

等在数秒后 即可安装好 而后访问对应的 ip/domain 即可（鸽子懒 已经事先把ipv6绑定域名了
打开http://ip/domain后即可看到安装好的欢迎也面，可以明显看到其与CentOS的明显不同
而后就是其余CentOS的配置文件的区别了
Centos下 配置格式如下(其中的部分解释只是我作为小白的解释，大佬勿喷求饶！！)

```text
/etc/httpd/
|---conf/
|	|---httpd.conf			………………………………………………			主配置文件
|---conf.d
|	|---welcom.conf			………………………………………………			其他配置文件
|	|---······
|---logs/					………………………………………………			日志
|---········
```

Debain则为下列大致格式(其中的部分解释只是我作为小白的解释，大佬勿喷求饶！！)

```
/etc/apache2/
|---apache2.conf			……………………………………………… 			主配置文件
|---conf-available/			………………………………………………			可用的配置文件
|	|···
|---conf-enable/			………………………………………………			启用的配置文件
|	|···
|---mods-available/			………………………………………………			可用的模组
|	|···
|---mods-enable/			………………………………………………			启用的模组
|	|···
|---sites-available/		………………………………………………			可用的站点
|	|···
|---sites-enable/			………………………………………………			启用的站点
|	|···
|---·······
```

在表中能看到有好多以*-available 、 *-enable 的目录这是分开放的 其中前者是可选用的 而后者储存的是启用的 二者联系的方式则为 将前者的配置子项 软连接的后者的文件夹内 其中的某些默认不是全部开启的。sites-available/enable 这个是apache的虚拟主机（一个服务器上根据不同的个domain可以有多个网页）（我记得之前在CentOS建站时有篇大佬的教程就是教的这种分布[Centos 8 安装apache、mysql、php7.3、PHPmyadmin及多域名多ssl配置方法](https://www.niuqi360.com/lamp-config/centos-8-install-apache-mysql-php73-phpmyadmin-ssl/)）

那么正式开始页面布置 我简单粗暴的 把之前的CentOS下的vhsot文件修改下 放进了 /etc/apache2/sites-available/并将它软连接到 /etc/apache2/sites-enable/以启用（假设我的域名为 a.b.c）
a.b.c.conf

```conf
<VirtualHost *:80>
    ServerName a.b.c
    ServerAlias b.c
    DocumentRoot /var/www/a.b.c/public
    ErrorLog /var/www/a.b.c/logs/error.log
    CustomLog /var/www/a.b.c/logs/requests.log combined
    #跳转443端口
    RewriteEngine on
    RewriteCond %{SERVER_PORT} !^443$
    RewriteRule ^/?(.*)$ https://%{SERVER_NAME}/$1 [L,R]
</VirtualHost>

<VirtualHost *:443>
    DocumentRoot /var/www/a.b.c/public

    ServerName a.b.c
    ServerAlias a.b
 
    <Directory /var/www/a.b.c/public>
           #Allowoverride all    ###Uncomment if required
    </Directory>
 
    SSLEngine on
    SSLCertificateFile /etc/apache2/cert/cert.cer  
    SSLCertificateKeyFile  /etc/apache2/cert/cert.key
    SSLCertificateChainFile /etc/apache2/cert/fullchain.cer

 
    ErrorLog logs/a.b.c_ssl-error.log
    CustomLog logs/a.b.c_ssl-access.log combined
</VirtualHost>

```

对照这个配置文件  开始创建网页的目录

```
mkdir -p /var/www/a.b.c/public
mkdir -p /var/www/a.b.c/los
```

而后记得把实例网页 上传至 public文件夹 上传完成后 记得对目录及其子目录授予权限 否则会报错

```
chmod -R 755 /var/www/a.b.c/
```

之后再次看配置文件 可以看到 在跳转443的配置中 有这样的语句" RewriteEngine on"不过在Debain下这个功能默认是关闭的 需要手动开启否则是会报错的 
这个在CentOS是默认开启的 不过Debain这里则是作为一个模组 需要手动开启 
开启方式也很简单 把mods-available中的rewrite.load软连接到mods-enable里即可

解决这个之后便是SSL证书了首先创建cert文件夹即网站证书文件夹位置自定 我就放在配置问价同目录了

```
mkdir /etc/apache2/cert/
```

此后要安装SSL模块

```
a2enmod ssl 
```

之后可以看到SSL也是有日志的 而这个日志的目录也是默认不存在的 需要我们手动设置 
创建目录 并给予读写权限

```
mkdir /etc/apache2/log
chmod 766 /etc/apache2/log
```

而后一定记得在主配置文件/etc/apache2/apache.conf的最后一行加上一句

```
ServerName localhost
```

否则会报错

```
 apr_sockaddr_info_get() failed for 
```

