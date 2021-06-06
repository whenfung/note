---
title: ubuntu 18.04 安装 wordpress
---

wordpress 是老牌的动态博客系统了，适合非计算机人员写博客，其功能简单，插件丰富，社区活跃，而且 wordpress 现在也是一个很成功的 CMS （内容管理系统）。如今全世界三成的网站都是用 wordpress 搭建的。那么如何在自家主机上搭建 wordpress 呢？下面做详细介绍：

## 1. 运行环境配置

1. lamp 环境

   wordpress 是一个 web 服务，当然要有标准的 lamp 环境，lamp 环境的配置参考我的上一篇文章。

2. 下载 wordpress

   进入临时文件夹

   ```bash
   cd /tmp
   ```

   下载最新软件包并解压

   ```bash
   wget https://wordpress.org/latest.zip
   unzip latest.zip
   ```

   目录下会出现 wordpress 文件夹，拷贝到 apache 服务器的目录下

   ```bash
   sudo cp -r wordpress /var/www/html
   ```

3. 配置 wordpress

   进入配置文件

   ```bash
cd /var/www/html/wordpress
   sudo cp wp-config-sample.php wp-config.php
sudo vim wp-config.php
   ```

   接下来配置数据库名字、用户、密码
   
   浏览器输入 IP/wordpress，进入著名的五分钟安装过程。

## 2. apache 配置

1. 如果你想直接通过 IP 访问你的博客，可以做如下配置

   ```bash
   cd /etc/apache2/sites-available
   sudo vim 000-default.conf
   ```

2. 修改路径指向 wordpress

3. 更新并重启 apache

   ```bash
   a2ensite 000-default.conf
   sudo service apache2 restart
   ```

## 3. 自动安装

我使用的服务器是腾讯云提供的，腾讯云上面有制作好的 wordpress 镜像，你可以先备份你的云主机（腾讯云提供快照和镜像功能），然后重装 wordpress 集成环境即可。

## 4. wordpress 常见问题

你有可能会更新失败，因为你的 wordpress 目录权限还是 root 的，你需要更改权限。

```bash
sudo chown -R www-data:www-data wordpress
```

## 5. 总结

wordpress 其实也是一个软件，只不过配置起来比 windows 下的软件麻烦了一点，但是这也是服务器的一种特点，当命令行约定俗成的时候，学习成本也就低了，现在计算机足够复杂，每个人熟悉一套 API 就很费时间了，但是 API 会变的，只有知道世界运作的原理，你才能以不变应万变，多学学底层。

