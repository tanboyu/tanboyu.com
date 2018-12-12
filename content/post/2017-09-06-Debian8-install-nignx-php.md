---
title: 'Debian8服务器安装nignx、php环境'
author: 波比
date: '2017-09-06'
slug: Debian8服务器安装nignx、php环境
categories:
  - 网络技术
tags:
  - debian8
  - nignx
  - php环境
  - 服务器
  - 网络技术
---

这个技能不是人人都要会，但是用到VPS的朋友，而且对linux不是那么熟悉的朋友，这个文章可能就有意义了。如题：

## 第一步：安装所需的环境

既然是debian系统肯定，这个是必须的。同时，还得由一个root的权限账号，或者没有root权限的也可以。下面我贴出的代码都是具有root权限的例子，如果网友是非root权限的账号安装，需要在所有代码前加上sudo即可。

## 第二步：开始安装nginx

```shell
apt-get update
apt-get install nginx
```

到这里系统已经开始安装配置nginx了，如果你的系统装有**ufw**防火墙，这时你可能需要在防火墙设置上作适当修改。

```shell
ufw allow 'Nginx HTTP'
ufw status # 检查状态

Output #输出结果如下
Status: active

To                         Action      From

---

OpenSSH                    ALLOW       Anywhere                  
Nginx HTTP                 ALLOW       Anywhere                  
OpenSSH (v6)               ALLOW       Anywhere (v6)             
Nginx HTTP (v6)            ALLOW       Anywhere (v6)
```

### 第三步：安装mysql

```shell
apt-get install mysql-server mysql-client
```

### 第四步：安装php

```shell
nano /etc/apt/sources.list #修改安装包源
```

## 屏幕展示结果如下

```
/etc/apt/sources.list
...
deb http://mirrors.digitalocean.com/debian jessie main contrib non-free
deb-src http://mirrors.digitalocean.com/debian jessie main contrib non-free

deb http://security.debian.org/ jessie/updates main contrib non-free
deb-src http://security.debian.org/ jessie/updates main contrib non-free

jessie-updates, previously known as 'volatile'

deb http://mirrors.digitalocean.com/debian jessie-updates main contrib non-free
deb-src http://mirrors.digitalocean.com/debian jessie-updates main contrib non-free

使用:wq 保存退出。

apt-get update #更新

安装php5-fpm和php5-mysql模块

apt-get install php5-fpm php5-mysql

安装 php模块后修改php-fpm配置文件

nano /etc/php5/fpm/php.ini

把cgi.fix_pathinfo=1; 改为cgi.fix_pathinfo=0; 记得取消行前的注释符“#” 重新启动php-fpm

systemctl restart php5-fpm
```

### 第五步：配置Nginx 支持 PHP

修改nginx默认设置

```shell
nano /etc/nginx/sites-available/default
```

将默认文档

    server {
        listen 80 default_server;
        listen [::]:80 default_server;
        root /var/www/html;
    index index.html index.htm index.nginx-debian.html;
    
    server\_name \_;
    
    location / {
        try\_files $uri $uri/ =404;
    	}
    }


改成（注意红色部分）

    server {
        listen 80 default_server;
        listen [::]:80 default_server;
        root /var/www/html;
    index index.php index.html index.htm index.nginx-debian.html;
    
    server\_name  www.30plans.com ; #注意这里域名需要修改成自己的
    location / {
            try_files uri uri/ =404;
        }
    location ~ \\.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi\_pass unix:/var/run/php5-fpm.sock;
    }
    
    location ~ /\\.ht {
        deny all;
    	}
    }

 检测nginx配置状态

```shell
nginx -t

如果没有问题，重启nginx

systemctl reload nginx
```

### 第六步：检验php能否展示

```shell
nano /var/www/html/info.php #新建一个php页

# 将一下内容黏贴进info.php页面

<?php
  phpinfo();
?>
```

记得:wq保存退出。 浏览info.php页面 http://你邦德的域名\_或\_服务器IP/info.php 如果能看到下面这个页面，就表示大功告成了。   ![](https://ws1.sinaimg.cn/large/8f5e6680gy1fjmxsv3imwj20qa0k70xb.jpg)