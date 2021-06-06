---
title: YouCompleteMe
---

YCM 是 vim 的代码补全引擎，由于本人常在 vim 上写代码，故装 YCM 插件提高效率。下面的操作是在 Ubuntu 18.04 下进行的。

## 1. Vundle 配置

[Vundle](https://github.com/VundleVim/Vundle.vim) 是一款 vim 插件管理器，我使用 Vundle 来安装 YCM。Vundle 的安装配置步骤如下：

1. 下载 Vundle

   ```bash
   git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
   ```

2. 配置插件

   ```bash
   vim ~/.vimrc  # 进入配置文件
   :set paste    # 进入复制模式
   ```
   
   将[官方提供的模板](https://github.com/VundleVim/Vundle.vim#quick-start)复制到其中，并删除无关插件。
   
3. 安装配置好的插件

   ```bash
   vim              # 打开 vim
   :PluginInstall   # 命令行安装
   ```

## 2. YCM 安装

[YCM](https://github.com/Valloric/YouCompleteMe) 是 vim 的代码补全引擎，一定程度上可以提高你的编码速度和精确度，Ubuntu 18.04 的配置方式如下：

1. 安装依赖工具

   ```bash
   sudo apt install build-essential cmake python-dev python3-dev
   ```

2. 利用 Vundle 安装 YCM

   ```bash
   vim ~/.vimrc                    # 进入 vim 配置文件
   Plugin 'Valloric/YouCompleteMe' # 添加 YCM 插件
   :PluginInstall                  # 安装 YCM 并等待
   ```

3. 通过 `libclang` 编译带有 C 家族语言的静态分析的 YCM 

   ```bash
   cd ~/.vim/bundle/YouCompleteMe   
   ./install.py --clang-completer
   ```

   编译时间可能会有点长，耐心等待即可。

## 3. YCM 配置文件

YCM 的[个性设置](https://github.com/ycm-core/YouCompleteMe#options)都是在 `.vimrc` 中配置的，下面是我常用的一些选项。

1. YCM 配置文件的路径在 `.vimrc` 文件中定义，比如我的配置的文件是自带的 `.ycm_extra_conf.py`，在 `.vimrc` 中添加如下信息：

   ```bash
   let g:ycm_global_ycm_extra_conf = '~/.vim/bundle/YouCompleteMe/third_party/ycmd/.ycm_extra_conf.py'
   ```

2. 字符串匹配

   ```bash
   let g:ycm_seed_identifiers_with_syntax=1
   ```

3. 选中后关闭预览

   ```bash
   let g:ycm_autoclose_preview_window_after_completion = 1
   ```

4. 注释补全功能开启

   ```bash
   let g:ycm_complete_in_comments = 1
   ```

5. 收录文本中的注释和代码

   ```bash
   let g:ycm_collect_identifiers_from_comments_and_strings = 1
   ```

6. 关闭错误提示

   有时候错误提示很碍眼，所以可以选择关掉它
   
   ```bash
   let g:ycm_enable_diagnostic_signs = 0         #关闭错误提示行
   let g:ycm_enable_diagnostic_highlighting = 0  # 关闭高亮段
   ```
   
7. 补全触发器

   ```bash
   let g:ycm_semantic_triggers =  {
     \   'c' : ['->', '.','re![_a-zA-z0-9]'],
     \   'objc' : ['->', '.', 're!\[[_a-zA-Z]+\w*\s', 're!^\s*[^\W\d]\w*\s',
     \             're!\[.*\]\s'],
     \   'ocaml' : ['.', '#'],
     \   'cpp,objcpp' : ['->', '.', '::','re![_a-zA-Z0-9]'],
     \   'perl' : ['->'],
     \   'php' : ['->', '::'],
     \   'cs,java,javascript,typescript,d,python,perl6,scala,vb,elixir,go' : ['.'],
     \   'ruby' : ['.', '::'],
     \   'lua' : ['.', ':'],
     \   'erlang' : [':'],
     \ }
   ```

## 4. 让 YCM 支持 C/C++ 补全

如果你添加的头文件 YCM 没有识别，说明 YCM 没有找到对应的头文件，这里需要自己配置，例如 Objective-C 的库 Foundation，位于 `/usr/include/GNUstep/Foundation`。

在 YCM 配置文件 `.ycm_extra_conf.py` 中的 `flags` 加上 C++ 头文件库，就可以完成对 C/C++ 的补全。

```bash
'-isystem',
'/usr/include/c++/7.3.0/',
'-isystem',
'/usr/include',
```

## 5. 参考文献

- [Vundle](https://github.com/VundleVim/Vundle.vim)
- [YCM 的 flags 配置](https://github.com/ycm-core/YouCompleteMe#option-2-provide-the-flags-manually)

