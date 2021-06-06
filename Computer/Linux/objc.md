---
title: objc 环境搭建
---

介绍如何在 Ubuntu 18.04 的 vim 下使用 objc 编程。

## 1. 软件安装

1. 安装编译器 `gobjc` 

   ```bash
   sudo apt install gobjc
   ```

2. 安装 `gnustep` 库的安装，IOS 编程的 Foundation 库就在这里。

   ```bash
   sudo apt install gnustep
   ```

3. 安装 `gnustep-devel`

   ```bash
   sudo apt install gnustep-devel
   ```

## 2. 配置

编译之前，需要对编译环境进行设置，如果你的 shell 是 bash，执行

```bash
sudo bash /usr/share/GNUstep/Makefiles/GNUstep.sh
```

如果你的 shell 不是 bash，例如是 `zsh`，则执行

```bash
sudo zsh /usr/share/GNUstep/Makefiles/GNUstep.sh
```

## 3. 搭建项目

接下来可以写段代码进行编译了。首先创建文件夹 `hello`，在文件夹里面建立一个 `main.m` 文件。

```objective-c
#import <Foundation/Foundation.h>

int main(void)
{
    NSAutoreleasePool *pool = [[NSAutoreleasePool alloc] init];    

    NSLog(@"Hello, world!");

    unichar c = u'加';

    NSLog(@"The character is: %C", c);

    [pool drain];
}
```

接着创建 make 文件，命名为 `GNUmakefile`

```objective-c
GNUSTEP_MAKEFILES = /usr/share/GNUstep/Makefiles

include $(GNUSTEP_MAKEFILES)/common.make

ADDITIONAL_FLAGS += -std=gnu11

TOOL_NAME = test
test_OBJC_FILES = main.m

include $(GNUSTEP_MAKEFILES)/tool.make
```

由于我们在源代码中使用了 `C11` 标准中才引入的 `Unicode` 前缀字面量表达式 `u'加'`，表示一个 `UTF-16` 字符，因此我们在 `GNUmakefile` 中也加入了 `-std=gnu11` 这个编译选项来使得编译器使用最新的 `C11` 标准与 `GNU` 规范语法扩展。

这里要注意的是，对于其它 Linux 版本的系统，GNUStep 的默认安装路径可能不是在 `/usr/share/` 之中，因此需要根据当前 `GNUStep/Makefiles` 的路径对 `GNUSTEP_MAKEFILES` 进行设置。而且这个变量必须在 `include` 之前定义好。

`TOOL_NAME` 指定了 `make` 之后最终的目标可执行文件名。这里命名为 `test`。

完成以上步骤后，如果我们之前已经执行过 `GNUstep.sh`，那么可以直接敲 make，然后回车。工程即构建完成。