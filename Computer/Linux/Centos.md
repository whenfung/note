---
title: centos 服务器配置
---

centos 默认用户是 root 用户，先在默认用户下下载一些必要软件

```bash
yum install epel-release            # 安装 fedora 拓展源
yum update                          # 软件更新
yum install git zsh  
yum install ruby ruby-devel         # 安装 ruby
yum install lua lua-devel           # 安装 lua
yum install luajit luajit-devel     # C 语言写的 Lua 代码的解释器
yum install python python-devel     # 安装 python2.7
yum install python36 python36-devel # 安装 python3.6
yum install ctags                   # vim下方便代码阅读的工具
yum install tcl tcl-devel
yum install perl perl-devel perl-ExtUtils-ParseXS 
yum install perl-ExtUtils-XSpp perl-ExtUtils-CBuilder
yum install perl-ExtUtils-Embed
```

修复 `XSpp` 链接

```bash
# symlink xsubpp (perl) from /usr/bin to the perl dir
sudo ln -s /usr/bin/xsubpp /usr/share/perl5/ExtUtils/xsubpp 
```

手动编译最新版本 vim，支持的语言基本上都启用了。

```bash
cd ~
git clone https://github.com/vim/vim.git
cd vim
./configure --with-features=huge \
            --enable-multibyte \
	        --enable-rubyinterp=yes \
	        --enable-pythoninterp=yes \
	        --with-python-config-dir=/lib64/python2.7/config  \
	        --enable-python3interp=yes \
	        --with-python3-config-dir=/lib64/python3.6/config \
	        --enable-perlinterp=yes \
	        --enable-luainterp=yes \
            --enable-gui=gtk2 \
            --enable-cscope \
	        --prefix=/usr/local
make VIMRUNTIMEDIR=/usr/local/share/vim/vim81
```

如果要安装，你可以使用 `make` 指令

```bash
cd ~/vim
make install
```

卸载用到 `checkinstall`

```bash
yum install checkinstall
cd ~/vim
checkinstall
```

将 vim 设置为默认编辑器使用  `update-alternatives` 指令

```bash
sudo update-alternatives --install /usr/bin/editor editor /usr/local/bin/vim 1
sudo update-alternatives --set editor /usr/local/bin/vim
sudo update-alternatives --install /usr/bin/vi vi /usr/local/bin/vim 1
sudo update-alternatives --set vi /usr/local/bin/vim
```

利用 `vim --version` 查看是否安装成功

设置 vim 脚本 ，`vim ~/.vimrc`

```bash
syntax on  # 语法高亮
```



添加新用户，默认用 `zsh` 终端

```bash
useradd -s /bin/zsh deng  # 添加用户
passwd deng               # 启用用户
visudo                    # 添加 sudo 权限
```

## 其余事件

安装 vundle，参照其他笔记

安装 ycm，参照其他笔记

安装 qemu，yum install qemu-kvm [网址](<https://www.qemu.org/download/>)

## 参考资料

- [centos7.2 下安装 vim 和 YCM](<https://www.cnblogs.com/nidey/p/8657016.html>)



