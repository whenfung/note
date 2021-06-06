---
title: zsh 终端
---

ubuntu18.04 默认的 bash 终端没有补全功能，而 zsh 提供了一些补全功能。

## 1. 安装 zsh

```bash
sudo apt-get install zsh # 安装 zsh
cat /etc/shells          # 显示所有的 shell 程序
chsh -s /usr/bin/zsh     # 这里不要用 sudo，因为我们是给当下用户安装 zsh
echo $SHELL              # 查看当前用户的 shell 
```

然后重新开启终端，选择 2 自动设置（centos 选 1）。

## 2. 安装 oh-my-zsh

[oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh) 是一个管理 zsh 插件的工具。[安装说明请点击](https://github.com/ohmyzsh/ohmyzsh#getting-started)。一般选择使用 curl 或 wget 安装。

```bash
vim ~/.zshrc # 配置文件
```

一般我会解除下面 3 个注释。

```bash
export UPDATE_ZSH_DAYS=13        # 自动更新
ENABLE_CORRECTION="true"         # 自动校正
alias zshconfig="mate ~/.zshrc"  # 快捷命令
```

## 3. 插件安装

1. [zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions)

   很多命令我们都会重复敲，虽然可以使用 tab 键补全，还是麻烦，使用该插件可以记录所有使用过的命令并自动补全。

   选择 `oh-my-zsh` 方式安装，[安装说明请点击](https://github.com/zsh-users/zsh-autosuggestions/blob/master/INSTALL.md#oh-my-zsh)。

   最后执行 `source ~/.zshrc` 更新配置文件。



