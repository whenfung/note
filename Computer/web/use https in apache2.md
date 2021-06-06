---
title: 如何实现站点的 https 服务  
---

## 1. 配置 apache 的 https 服务

http 协议传输过程中，信息是不加密透明的，密码或者敏感信息很容易被窃取，http 服务在端口 80 上启动，现在的网站大都是 https 服务，在端口 443 上服务，利用 `ssl` 加密传输，如果要想在自己的主机上配置 https，可以通过 apache 和 腾讯云颁发的证书实现。

1. 证书下载

   这里涉及密码学知识，我不多说，直接在腾讯云上买了域名，申请证书然后下载，会提供不同服务器的证书，选择 apache 的，传到服务器上。

2. 配置 apache2

   首先让 apache2 支持 `ssl` ：

   ```bash
sudo apt-get install openssl
   sudo a2enmod ssl
   ```
   
   接着将 apache2 的 `ssl` 默认软件移动到有效区：
   
   ```bash
   sudo ln -s /etc/apache2/sites-available/default-ssl.conf /etc/apache2/sites-enabled/default-ssl.conf
   cd /etc/apache2/sites-available
   ```
   
   然后下载腾讯云上面申请的 `ssl` 证书，里面有 apache 命名的文件夹，利用软件 `xftp` 发送到云服务器的 `/tmp` 临时文件夹中，然后拷贝到想应目录：
   
   ```bash
   sudo cp -r /tmp/Apache /etc/apache2    # 拷贝操作
   ```
   
   编辑 `default-ssl.conf` 文件：
   
   ```bash
   cd /etc/apache2/sites-available
   sudo vim default-ssl.conf
   ```
   
   配置站点路径（例如 `wordpress` 站点）、站点域名、添加证书，内容如下
   
   ```bash
   DocumentRoot /var/www/html
   ServerName neuron.zone
      
   SSLCertificateFile /etc/apache2/Apache/2_xxx.crt
   SSLCertificateKeyFile /etc/apache2/Apache/3_xxx.key
   SSLCertificateChainFile /etc/apache2/Apache/1_root_bundle.crt
   ```
3. 退出编辑，重启 apache

   ```bash
   sudo service apache2 restart
   ```

## 2. 启动重定向

强迫症患者可以强制 http 转 https。利用 apache 的重定向功能实现。操作如下

1. 启动重定向

   ```bash
   sudo a2enmod rewrite
   ```

2. 配置文件使得访问 80 端口的时候启动重定向

   编辑相关文件：

   ```bash
   cd /etc/apache2/sites-available
   sudo vim 000-default.conf
   ```

   填入以下内容：

   ```bash
   RewriteEngine on
   RewriteCond   %{HTTPS} !=on
   RewriteRule   ^(.*)  https://%{SERVER_NAME}$1 [L,R=301]
   ```

3. 重启 apache

   ```bash
   sudo service apache2 restart
   ```

### 3. apache 利用不同域名对不同目录进行访问

多个域名可以绑定到同一个主机，利用 apache 实现不同目录的访问，实现一个服务器，多个站点的功能。

1. 每个域名都应该有对应的配置文件，如下

   ```bash
   cd /etc/apache2/sites-availabled
   cp 000-default.conf 111-default.conf
   ```

2. 对拷贝的文件做相应的修改

   - 更改 `ServerName` 为新域名地址

   - 更改 `DocumentRoot` 为新域名对应的新站点位置

   然后将有效区的文件链接到功能区：

   ```bash
   sudo ln -s ../sites-available/mysql.conf 111-default.conf
   ```

3. 重启 apache

   ```bash
   sudo service apache2 restart
   ```

### 4. 多证书认证

在 `default-ssl.conf` 中添加新域名和对应的证书：

```bash
<VirtualHost 0.0.0.0:443>
    DocumentRoot "/var/www/html"
    ServerName www.domain.com
    SSLEngine on
    SSLCertificateFile /usr/local/apache/conf/2_www.domain.com_cert.crt
    SSLCertificateKeyFile /usr/local/apache/conf/3_www.domain.com.key
    SSLCertificateChainFile /usr/local/apache/conf/1_root_bundle.crt
</VirtualHost>
```

记得是将这段话添加到 `<IfModule mod_ssl.c></IfModule>` 内 ，重启 apache。

## 5. 总结

https 的实现思路大概是这样的。

1. 需要一个信任机构提供的证书，当别人访问你的网站的时候，浏览器会去信任中心确认你的证书是否可信，证书是通过域名绑定的。
2. 接下来，客户和服务器上数据的传输，都会通过证书提供的密钥来加密传输。

当然这部分的实现还是挺复杂的，所以 apache 帮我们实现了，我们只需要稍微修改相应的设置即可。