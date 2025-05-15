---
title: 'Debian服务器上了一个SSL证书'
author: 波比
date: '2017-12-26'
slug: Debian服务器上了一个SSL证书
categories:
  - 网络技术
tags:
  - apache2
  - cert
  - debian
  - openssl
  - sslforfree
  - SSL证书
  - tech
---

1. 申请SSL证书 https://www.sslforfree.com/

2. 依照https://www.sslforfree.com/的操作指南，一步一步的在VPS上建立目录验证，然后下载打包的证书文件。

3. Openssl的安装

    ```bash
    # OpenSSL已安装和更新
    sudo apt-get update
    sudo apt-get upgrade openssl
    ```

    **此处默认已安装好Apache，如果没有就执行下面的命令**

    ```bash
    apt-get install apache2
    
    # 启用Apache SSL模块
    
    a2enmod ssl
    
    # 默认的Apache网站提供了一个启用SSL的有用模板，因此我们现在将激活默认网站。
    a2ensite default-ssl
    
    # 重启使修改生效
    
    service apache2 reload
    
    # 在Apache目录下创建ssl目录存放证书文件
    
    mkdir /etc/apache2/ssl
    
    # 设置文件权限以保护您的私钥和证书
    
    chmod 600 /etc/apache2/ssl/*
    ```

    **用FTP将打包下载的证书文件解压后上传至/etc/apache2/ssl/目录下**

    ```bash
    # 修改默认ssl-conf文件
    
    nano /etc/apache2/sites-enabled/default-ssl.conf
    
    # 添加一行与下面您的服务器名称directy ServerAdmin电子邮件线。 这可以是您的域名或IP地址：
    
    ServerAdmin webmaster@localhost
    ServerName example.com:443
    
    SSLCertificateFile "/etc/apache2/ssl/certificate.crt"
    SSLCertificateKeyFile "/etc/apache2/ssl/private.key"
    SSLCACertificateFile "/etc/apache2/ssl/ca_bundle.crt"
    
    # 保存后退出
    
    # 重启apache
    
    service apache2 reload
    
    ```
