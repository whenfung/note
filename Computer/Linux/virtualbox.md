---
title: VirtualBox 上安装 Linux 系统
---

利用 VirtualBox 安装 Linux 系统，方便管理，但是安装过程有些复杂，下面介绍如何用 VirtualBox 安装 Centos 和 Ubuntu 系统。

- [Centos Linux DVD ISO](https://www.centos.org/download/) 下载
- [VirtualBox](https://www.virtualbox.org/wiki/Downloads) 下载
- [Ubuntu 18.04 LTS](https://ubuntu.com/download/desktop) 下载

## 1. 安装 VirtualBox

默认默认，下一步。。。完成安装。

## 2. 安装 Centos

1. 点击新建按钮。
2. 选择虚拟磁盘大小时，最好选 32 G 或以上，以后不用扩张。
3. 点击设置按钮。
4. 选择系统→启动顺序→光驱调至第一位。
5. 点击存储→没有盘片→右侧光盘图标→选择本地 ISO 镜像文件。
6. 点击启动，进去 Centos 安装界面。
7. 安装 Centos 时，软件选择选带 GUI 的，安装开发工具。

## 3. 安装 Ubuntu

和安装 Centos 的前 5 步一样，然后进入 Ubuntu 安装界面。

### 3.1 更换默认源

Ubuntu 18.04 的默认国外源下载速度过慢，修改为国内源会快很多。首先备份源文件

```bash
sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak
sudo vi /etc/apt/sources.list
```

清空 `sources.list`，选择下列其中一个国内源拷贝到 `sources.list` 。

1. 阿里源

   ```bash
   deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
   deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
   deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
   deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
   deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
   deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
   deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
   deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
   deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
   deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
   ```

2. 清华源

   ```bash
   deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse
   deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse
   deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
   deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
   deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
   deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
   deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
   deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
   deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
   deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
   ```

保存 `sources.list` 后执行

```bash
sudo apt-get update
sudo apt-get upgrade
```

## 4. 常见问题

1. 安装增强功能的时候，需要将桌面的磁盘弹出。

2. 共享文件无权限访问。

   `sudo usermod -aG vboxsf $(whoami)`

   其中，`usermod -aG <group> <user>` 表示将用户`<user>`加入到（追加到）组`<group>`中，选项`[-aG]`是追加到组的意思。

3. 为了预防出事故，最好周期性备份。Virtualbox 提供了快照功能。点击控制按钮，生成备份即可。若要回到备份，将虚拟机关机，可以在虚拟机条款的 “三杆” 图标点击找到，点击恢复备份即可。

4. 如果常用虚拟机，不需要每次都关机，只需快速休眠即可，这也是 Virtualbox 的默认关闭方式。