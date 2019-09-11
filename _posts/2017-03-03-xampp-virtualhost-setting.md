---
layout: post
title: XAMPP虚拟主机VirtualHost配置随笔
---

本文列举了配置XAMPP虚拟主机VirtualHost可能遇到的错误。

错误表现为：

Apache Access forbidden! Error 403

No Object Found 404

涉及到的文件：

1、mac系统相关

/private/etc/apache2/httpd.conf（默认配置）

/private/etc/apache2/extra/httpd-vhosts.conf（默认配置）

/private/etc/hosts（增加虚拟主机的配置 127.0.0.1  example.com）

2、xampp相关

/Applications/XAMPP/xamppfiles/etc/extra/httpd-vhosts.conf（重要配置步骤）

需要设置两个虚拟主机，一个是给默认的localhost，一个是给自己的example.com

给默认localhost的：

<VirtualHost *:80>
ServerName localhost
DocumentRoot “/Applications/XAMPP/htdocs”
<Directory “/Applications/XAMPP/htdocs”>
Options Indexes FollowSymLinks Includes execCGI
AllowOverride All
Order Allow,Deny
Allow From All
</Directory>
</VirtualHost>

给自己的（可以增加数量）：

<VirtualHost *:80>
ServerAdmin webmaster@dummy-host.example.com
DocumentRoot “/Applications/XAMPP/xamppfiles/htdocs/dummy”
ServerName dummy.com
ServerAlias www.dummy.com
<Directory “/Applications/XAMPP/xamppfiles/htdocs/dummy”>
Require all granted
</Directory>
ErrorLog “/private/var/log/apache2/dummy-error_log”
CustomLog “/private/var/log/apache2/dummy-access_log” common
</VirtualHost>

/Applications/XAMPP/xamppfiles/etc/extra/httpd-xampp.conf（不确定需不需要改）

<Directory “/Applications/XAMPP/htdocs”>
Options All
AllowOverride All
Require all granted
</Directory>

/Applications/XAMPP/xamppfiles/etc/httpd.conf（需要开启vhost，去除注释，以及更改权限）

以下修改

<Directory />
#AllowOverride none
#Require all denied
Options FollowSymLinks
AllowOverride None
Order deny,allow
Deny from all
</Directory>

以下几行取消注释

Include etc/extra/httpd-xampp.conf
Include /Applications/XAMPP/xamppfiles/apache2/conf/httpd.conf

# Virtual hosts
Include /Applications/XAMPP/xamppfiles/etc/extra/httpd-vhosts.conf
Include etc/extra/httpd-vhosts.conf

最后，如果cakephp工程包是从别的地方搬迁过来的，需要注意！！！！！

.htaccess等隐藏文件会被遗忘，rewrite设置会被忽略掉的，.gitignore也被忽略了，都要手动搬迁。