---
title: lamp 的安装和使用
---

LAMP 指的是 Web 开发的环境，L 是指 Linux 操作系统，A 是指 Apache 或者 nignx 服务器，M 是指 Mysql 数据库，P 是指 php 脚本语言 。

linux 操作系统负责管理各种硬件资源，apache 担任 Http 服务器，mysql 提供数据存储服务，php 全名是 Hypertext Preprocessor，也就是对数据做各种处理，解析成固定格式的超文本传给浏览器。 

## 1. 环境介绍

LAMP 作为 网络服务，一般是以 Server-Client 模式工作的，所以我直接买了台腾讯云服务器来捣鼓。

```bash
ubuntu18.04 + apache2.4 + mysql5.7 + php7.2
```

## 2. 手动安装

作为 web 服务典型环境，肯定是有集成环境，但是作为一名专业的计算机人员，还是要知道如何配置各组件的环境。

1. 安装 apache2 

   ```bash
   sudo apt-get install apache2
   ```

   利用 ubuntu 自带的软件源安装完毕。通过 `apache2 -v` 查看版本。浏览器输入服务器 `IP` 地址可以发现 `apache2 ubuntu Default page`。

2. 安装 mysql5.7

   ```bash
   sudo apt-get install mysql-server
   ```

   mysql5.7 安装完毕。通过 `mysql --version` 查看版本。

   ```bash
   sudo cat /etc/mysql/debian.cnf # 查看默认密码，刚安装一般没有默认密码
   sudo mysql                     # 进入数据库，创建数据库和用户
   ```

   建立一个名叫 neuron 的数据库，并创建数据库用户 neuron 用来管理 neuron 数据库。

   ```mysql
   create database neuron; 
   create user 'neuron'@'localhost' identified by '123456';
   grant all privileges on neuron.* to 'neuron'@'localhost' identified by '123456';
   flush privileges;
   ```

3. 安装 php7.2

   ```bash
   sudo apt-get install php7.2
   ```

   通过 `php7.2 -v` 查看版本。作为脚本语言，php 就是负责预处理的，将 mysql 的东西预处理为标准超文本，然后将资源扔给 apache，最终 apache 会把用户需要的资源发送过去，因此要想使三个程序正常工作，会有相应的链接程序来链接三个程序。

   php 有一些很有用的功能可供使用，例如图片处理，编码转换。安装如下

   ```bash
   sudo apt-get install php7.2-gd php7.2-mbstring
   ```

4. 验证 LAMP 环境是否正常工作

   在 apache2.4 的默认目录 `/var/www/html` 创建一个叫 `info.php` 的文件，并编辑如下内容。

   ```php
   <?php phpinfo(); ?>
   ```

   返回浏览器，输入 `IP/info.php` 查看是否可以正常解析，你可以看见完整的信息，如果没有解析成功，可能是模块之间的链接程序没有安装，操作如下：

   ```bash
   sudo apt-get install libapache2-mod-php7.2
   ```

   解析成功后如果没有发现 mysql 的程序，需要安装另一个链接模块使 mysql 和 php 可以连接，然后重启 apache2。操作如下：

   ```bash
   sudo apt-get install php7.2-mysql
   sudo service apache2 restart
   ```

这就是 LAMP 手动安装的全部过程了。

## 3. 自动安装

ubuntu 中有一个叫 tasksel 的安装程序，提供了很多常用的服务，包括 LAMP-SERVER，安装特别简单。如下

```bash
sudo apt-get install tasksel
sudo tasksel
```

执行上面的指令后你会进入一个界面，按空格键，然后选择 LAMP-SERVER 然后回车就行了。

安装完后还是需要配置 mysql 的。

## 4. 安装 phpmyadmin

数据库用来存储数据，mysql 是一个关系型数据库，为了操作方便，我们可以在浏览器上设计并创建我们的数据库，需要安装一个叫 phpmyadmin 的程序。

1. 手动安装

   ```bash
   cd /tmp
   wget https://files.phpmyadmin.net/phpMyAdmin/4.8.5/phpMyAdmin-4.8.5-all-languages.zip
   unzip phpMyAdmin-4.8.5-all-languages.zip
   sudo cp -r phpMyAdmin-4.8.5-all-languages /var/www/html/phpmyadmin
   ```

   然后浏览器访问 `IP/phpmyadmin` 就能看见界面了，使用刚刚创建的数据库用户和密码进入操作界面。

   注意：在外网连接数据库默认是没有权限创建数据库的，需要自己在服务器上设置。

   手动安装可能会出现一些配置问题，这里标记一下

   - 编码问题

     ```bash
     sudo apt-get install php-mbstring
     ```

   - 短语密码问题

     ```bash
     cd /var/www/html/phpmyadmin/libraries
     sudo vim config.default.php
     ```

     填写 `blowfish_secret` 字段密码，要 32 位。

   - 缓存缓慢，是因为缓存文件夹 /tmp 不存在。

     ```bash
   cd /var/www/html/phpmyadmin
     sudo mkdir tmp
   sudo chmod 777 tmp
     ```

2. 自动安装 （不建议，版本跟不上，可能有各种小错误）

   ```bash
   sudo apt-get install phpmyadmin
   ```

   软链接将共享文件映射到 apache 服务的根目录

   ```bash
   sudo ln -s /usr/share/phpmyadmin /var/www/html/phpmyadmin
   ```

   然后浏览器访问 IP/phpmyadmin 就能看见界面了，使用之前创建的数据库用户和密码。


## 5. 总结

软件的安装一般版本号会变，其他指令都一样，如果出了问题，很有可能是新版本有不一样的设置，这时候只需要通通手动安装最新版本就可以了，手动安装的步骤基本是

利用 wget 获取安装包

根据相关官方文档进行相应配置

https://www.apache.org

https://www.mysql.com/

https://www.php.net/

https://www.phpmyadmin.net/








