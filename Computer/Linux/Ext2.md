---
title: ext2 文件系统
---

EXT2 第二代扩展文件系统（second extended filesystem），是 Linux 内核所用的文件系统。这里介绍 EXT2 磁盘文件系统的管理数据格式和文件数据格式的布局安排。

1. 一个物理磁盘可以划分为多个磁盘分区，每个磁盘分区可以从逻辑上看成是从 0 开始编号的大量扇区，各自可以格式化成不同类型的文件系统（EXT2，NTFS 等）。
2. 如果磁盘分区格式化为 EXT2 文件系统，则内部按照 EXT2 的规范，将磁盘盘块组织成超级块、组描述符和位图、索引节点、目录等管理数据，放在分区前端称为元数据区，剩余空间用于保存文件数据。

## 1. 整体布局

1. 格式化为 EXT2 文件系统的分区上，这些盘块分为两类：存放文件内容数据的数据盘块和保存元数据（管理数据）的元数据盘块。但是物理布局不是一刀切地简单分为两段，而是首先分成多个同尺寸块组（block group）的形式，在块组内再分成两部分：前面是元数据盘块后面是数据盘块。
2. 一个盘块的可引导分区的第一个盘块是为引导系统而保留的，通常称为引导块（引导扇区），启动扇区并不是所有分区都需要的，非启动分区不需要用到。

布局排列如下表所示

| 引导块         | 块组0           | 块组1           | 块组n           |
| -------------- | --------------- | --------------- | --------------- |
| 启动分区才有用 | 元数据+文件数据 | 元数据+文件数据 | 元数据+文件数据 |

其中，每个块组包含的信息和布局如下。每个块组由多个连续盘块组成，块组内部还有自己的组织机构。

| super_block                                              | group_descriptors                                    | Reserve_GDT                              | data_block_bitmap                                       | inode_bitmap                                              | inode_table                                                  | data_blocks                                                  |
| -------------------------------------------------------- | ---------------------------------------------------- | ---------------------------------------- | ------------------------------------------------------- | --------------------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1 个超级块                                               | n 块                                                 | k 块                                     | 1 块                                                    | 1 块                                                      | m 块                                                         | x 块                                                         |
| 所有块组相同互为备份，记录文件系统全局性的管理信息和属性 | 所有块组相同互为备份，记录块组内的管理信息和相关属性 | 所有块组相同互为备份，保留的块组描述符表 | 块组私有，记录着本块组数据盘块（data blocks）的使用情况 | 块组私有，记录着本块组索引节点表（inode table）的使用情况 | 块组私有，每个表项记录一个文件（普通文件或目录文件）的属性及数据块所在位置（含 12 个直接索引项、1个间接索引项、1 个二级索引项和 1 个三级索引项） | 块组私有，记录着各种文件数据，使用情况由前面的数据盘块位图记录，因此一个块组的数据盘块数量不能超过数据盘块位图的记录能力 |

超级盘块和块组描述符是很重要的，因此超级块和块组描述符有很多备份（出现错误时可以恢复）。早期 EXT2 在每个块组中都做了备份，后续版本的 EXT2 则采用稀疏超级块（sparse super block）技术，超级块只在 0 、1块组，以及编号为 3、5、7次幂的块组中，以节约超级块的备份空间。

## 2. 超级块

超级块是文件系统的起点信息，记录了文件系统全局性的一些信息。磁盘上的 EXT2 超级块是一个 `struct ext2_super_block` 结构体，共 1K 字节。

```c
struct ext2_super_block {
    __le32 s_inodes_count; // 文件系统中inode的总数
    __le32 s_blocks_count; // 文件系统中块的总数
    __le32 s_r_blocks_count; // 保留块的总数
    __le32 s_free_blocks_count; // 未使用的块的总数（包括保留块）
    __le32 s_free_inodes_count; // 未使用的inode的总数
    __le32 s_first_data_block; // 块ID，在小于1KB的文件系统中为0，大于1KB的文件系统中为1
    __le32 s_log_block_size; // 用以计算块的大小（1024算术左移该值即为块大小）
    __le32 s_log_frag_size; // 用以计算段大小（为正则1024算术左移该值，否则右移）
    __le32 s_blocks_per_group; // 每个块组中块的总数
    __le32 s_frags_per_group; // 每个块组中段的总数
    __le32 s_inodes_per_group; // 每个块组中inode的总数
    __le32 s_mtime; // POSIX中定义的文件系统装载时间
    __le32 s_wtime; // POSIX中定义的文件系统最近被写入的时间
    __le16 s_mnt_count; // 最近一次完整校验后被装载的次数
    __le16 s_max_mnt_count; // 在进行完整校验前还能被装载的次数
    __le16 s_magic; // 文件系统标志，ext2中为0xEF53
    __le16 s_state; // 文件系统的状态
    __le16 s_errors; // 文件系统发生错误时驱动程式应该执行的操作
    __le16 s_minor_rev_level; // 局部修订级别
    __le32 s_lastcheck; // POSIX中定义的文件系统最近一次检查的时间
    __le32 s_checkinterval; // POSIX中定义的文件系统最近检查的最大时间间隔
    __le32 s_creator_os; // 生成该文件系统的操作系统
    __le32 s_rev_level; // 修订级别
    __le16 s_def_resuid; // 报留块的默认用户ID
    __le16 s_def_resgid; // 保留块的默认组ID
    // 仅用于使用动态inode大小的修订版（EXT2_DYNAMIC_REV）
    __le32 s_first_ino; // 标准文件的第一个可用inode的索引（非动态为11）
    __le16 s_inode_size; // inode结构的大小（非动态为128）
    __le16 s_block_group_nr; // 保存此超级块的块组号
    __le32 s_feature_compat; // 兼容特性掩码
    __le32 s_feature_incompat; // 不兼容特性掩码
    __le32 s_feature_ro_compat; // 只读特性掩码
    __u8 s_uuid[16]; // 卷ID，应尽可能使每个文件系统的格式唯一
    char s_volume_name[16]; // 卷名（只能为ISO-Latin-1字符集，以’\0’结束）
    char s_last_mounted[64]; // 最近被安装的目录
    __le32 s_algorithm_usage_bitmap; // 文件系统采用的压缩算法
    // 仅在EXT2_COMPAT_PREALLOC标志被设置时有效
    __u8 s_prealloc_blocks; // 预分配的块数
    __u8 s_prealloc_dir_blocks; // 给目录预分配的块数
    __u16 s_padding1;
    // 仅在EXT3_FEATURE_COMPAT_HAS_JOURNAL标志被设置时有效，用以支持日志
    __u8 s_journal_uuid[16]; // 日志超级块的卷ID
    __u32 s_journal_inum; // 日志文件的inode数目
    __u32 s_journal_dev; // 日志文件的设备数
    __u32 s_last_orphan; // 要删除的inode列表的起始位置
    __u32 s_hash_seed[4]; // HTREE散列种子
    __u8 s_def_hash_version; // 默认使用的散列函数
    __u8 s_reserved_char_pad;
    __u16 s_reserved_word_pad;
    __le32 s_default_mount_opts;
    __le32 s_first_meta_bg; // 块组的第一个元块
    __u32 s_reserved[190];
};
```

保留的盘块可以让超级用户在磁盘空间紧缺时仍有可用的盘块。文件系统 `s_state` 字段为 0 表示已经安装挂载或者为安全卸载，为 1 表示正常卸载，为 2 表示包含错误。

## 3. 块描述符

一个 EXT2 格式的磁盘文件系统内含多个块组，每个块组都有自己的描述符，对应结构体是 `struct ext2_group_desc`。

```c
struct ext2_group_desc
{
    __le32 bg_block_bitmap;      // 块位图所在的第一个块的块ID
    __le32 bg_inode_bitmap;      // inode位图所在的第一个块的块ID
    __le32 bg_inode_table;       // inode表所在的第一个块的块ID
    __le16 bg_free_blocks_count; // 块组中未使用的块数
    __le16 bg_free_inodes_count; // 块组中未使用的inode数
    __le16 bg_used_dirs_count;   // 块组分配的目录的inode数
    __le16 bg_pad;
    __le32 bg_reserved[3];
};
```

当分配索引节点和数据盘块时，涉及空闲盘块计数值（`bg_free_block_count`）、空闲索引节点计数值（`bg_free_indoes_count`）和在用目录计数值 `bg_used_dirs_count` 字段。

数据盘块位图的块号在 `bg_block_bigmap` 指出，索引节点位图盘块号由 `bg_inode_bitmap` 指出，索引节点起点块号由 `bg_inode_table` 指出。

## 4. 索引节点

作为磁盘文件系统，EXT2 只有静态的文件概念，也就是索引节点所代表的文件，而没有 VFS 中的 `struct file` 的动态打开的文件概念。文件读写操作先经过一个地址空间 `address_space` 的中间层，此时的逻辑文件时具有简单的线性编址空间。但是将逻辑文件映射到磁盘上时，就必须由 EXT2 索引节点提供必要的信息用于从线性的逻辑地址编址空间转换到离散的盘块号的映射，最终才落实到盘块的读写。EXT2 的磁盘上索引节点是 `struct ext2_inode` ，主要记录数据盘块的位置（VFS 的 `inode` 没有数据盘块位置信息）和访问权限等信息。

```c
struct ext2_inode {
    __le16 i_mode; // 文件格式和访问权限
    __le16 i_uid; // 文件所有者ID的低16位
    __le32 i_size; // 文件字节数
    __le32 i_atime; // 文件上次被访问的时间
    __le32 i_ctime; // 文件创建时间
    __le32 i_mtime; // 文件被修改的时间
    __le32 i_dtime; // 文件被删除的时间（如果存在则为0）
    __le16 i_gid; // 文件所有组ID的低16位
    __le16 i_links_count; // 此inode被连接的次数
    __le32 i_blocks; // 文件已使用和保留的总块数（以512B为单位）
    __le32 i_flags; // 此inode访问数据时ext2的实现方式
    union {
        struct {
        	__le32 l_i_reserved1; // 保留
        } linux1;
        struct {
        	__le32 h_i_translator; // “翻译者”标签
        } hurd1;
        struct {
        	__le32 m_i_reserved1; // 保留
        } masix1;
    } osd1; // 操作系统相关数据
    __le32 i_block[EXT2_N_BLOCKS]; // 定位存储文件的块的数组，前12个为块号，第13个为一级间接块号，第14个为二级间接块号，第15个为三级间接块号
    __le32 i_generation; // 用于NFS的文件版本
    __le32 i_file_acl; // 包含扩展属性的块号，老版本中为0
    __le32 i_dir_acl; // 表示文件的“High Size”，老版本中为0
    __le32 i_faddr; // 文件最后一个段的地址
    union {
        struct {
            __u8 l_i_frag; // 段号
            __u8 l_i_fsize; // 段大小
            __u16 i_pad1;
            __le16 l_i_uid_high; // 文件所有者ID的高16位
            __le16 l_i_gid_high; // 文件所有组ID的高16位
            __u32 l_i_reserved2;
        } linux2;
        struct {
            __u8 h_i_frag; // 段号
            __u8 h_i_fsize; // 段大小
            __le16 h_i_mode_high;
            __le16 h_i_uid_high; // 文件所有者ID的高16位
            __le16 h_i_gid_high; // 文件所有组ID的高16位
            __le32 h_i_author;
        } hurd2;
        struct {
            __u8 m_i_frag; // 段号
            __u8 m_i_fsize; // 段大小
            __u16 m_pad1;
            __u32 m_i_reserved2[2];
        } masix2;
    } osd2; // 操作系统相关数据
};
```

一个块组中的大量 EXT2 的索引节点在磁盘上构成 `inode table` ，表中每个元素（EXT2 索引节点）代表一个文件，每个索引节点内含 `i_block[]` 数组用于记录文件的数据块所在位置， `i_block[EXT2_N_BLOCKS]` 虽然称为数组，但是它的元素属性并不完全一样（看注释）。

## 5. 目录结构

目录用于将整个文件系统的文件形成按路径名访问的树形组织结构排列。EXT2 文件系统并没有单独形成用于目录数据的盘块区，而是将目录与普通文件一样存在 data blocks 区域的盘块。每一个目录都作为一个文件，所以也是使用盘上索引节点管理其内部的数据盘块，只不过内容是用于存放所包含的文件或目录的目录项，通过上一级目录项将其类型标识为 “目录” 以区分普通文件。根目录有固定的起点（例如 EXT2 文件系统根目录的索引号为 2），因此根目录文件就不需要上级目录来定位了。

目录在存储管理上和普通文件相同，也是通过索引节点来管理目录项数据所在的盘块，只不过盘块数据内容含有多条目录项数据结构，每个目录项使用 `ext2_dir_entry_2` 结构体来描述。

```c
struct ext2_dir_entry_2 {
    __le32 inode; // 文件入口的inode号，0表示该项未使用
    __le16 rec_len; // 目录项长度
    __u8 name_len; // 文件名包含的字符数
    __u8 file_type; // 文件类型
    char name[255]; // 文件名
};
```

文件类型有如下

```c
enum {               // ext2_dir_entry_2 中的 file_type 枚举类型
    EXT2_FT_UNKNOWN = 0,
    EXT2_FT_REG_FILE = 1,  // 普通文件
    EXT2_FT_DIR = 2,    // 目录文件
    EXT2_FT_CHRDEV = 3, // 字符设备文件
    EXT2_FT_BLKDEV = 4, // 块设备文件
    EXT2_FT_FIFO = 5, // 命名管道 FIFO
    EXT2_FT_SOCK = 6, // SOCK
    EXT2_FT_SYMLINK = 7, // 符号链接
    EXT2_FT_MAX
}; 
```

字符设备、块设备和 FIFO 文件等类型的文件，不需要在磁盘上占用空间，即它们不需要数据盘块来存放文件内容，只需要目录项和相应的索引节点即可。

在 `inode` 成员中存放该目录项对应的文件 `inode` 号（可以有多个目录项通过硬链接指向同一个磁盘文件的 `inode`）。由于文件名是不定长的，所以目录项长度也不定，使用 `rec_len` 来记录。文件名的长度由 `name_len` 指出。文件名存在 `name[]` 数组中，长度不超过 `EXT2_NAME_LEN` （即 255 个字符）。目录项的长度是以 4 的整数倍字节递增的。

## 6. 根据节点查看文件内容过程

每一个文件都有一个 `id` ，也就是文件索引号，用 `ls -i` 可以查看。查看过程如下

1. 每个块组可以存储固定数量的节点号（`inodes_per_group`），利用除法得到其节点所在块组。
2. 

## 7. 分配磁盘空间

利用 `dd` 命令创建一个大小为 512MB 的文件 ，后面再将它变成环回（loopback）设备并在其上创
建文件系统。

```bash
dd if=/dev/zero of=bean bs=1k count=512000 
ll bean  # 查看是否创建成功 
```

从 /dev/zero 读取任意大小数量的 null 字符到当前目录的文件 bean 下，拷贝 512000 个块（count = 512000），每一个块的大小为 1k（bs = 1k）。

## 8. 创建环回设备

通过 `losetup` 将制定文件设置为环回设备。

```bash
sudo losetup /dev/loop0 bean 
```

使用 `losetup -d /dev/loop0` 卸载设备。

检查这个环路设备是否创建成功。

```bash
cat /proc/partitions
```

## 9. 创建 EXT2 文件系统

使用 `mke2fs` 命令在该设备上建立 EXT2 文件系统。

```bash
mke2fs /dev/loop0
```

## 10. 挂载文件系统

首先创建挂载点（目录，该目录会被挂载对象所覆盖）。

```
mkdir /mnt/bean
```

然后使用 `mount` 命令挂载 `/dev/loop0` 设备（该设备上已经创建了 `EXT2` 文件系统）。

```bash
mount -t ext2 /dev/loop0 /mnt/bean 
```

使用 `mount` 查看是否挂载成功。`umount /dev/loop0` 解除挂载。

正确挂载后，可以正常使用了。我们如果在该挂载点目录下执行 `ll` 命令可以看到一个空的
目录。

```bash
cd /mnt/bean
ll
```

## 11. 布局信息

在 `mke2fs` 时，将会告知把超级块放到那些块组。

用 `dumpe2fs` 命令来查看前面创建的 `EXT2` 文件系统的超级块的信息。

```bash
dumpe2fs /dev/loop0
```

如果想进一步验证这些信息是否正确，我们可以直接读入超级块。

```bash
dd if=/dev/loop0 bs=1k count=261 |od -tx1 -Ax > /tmp/dump_sp_hex 
cat /tmp/dump_sp_hex | more
```

观察重定向文件的数值和字段数值是否相同。

## 12. 块组描述符

数据块的位图只占用 1 个块，一个块有 1024 字节，每字节 8 个 bit，共有 8192 个 bit，可记录最多 8192 个块。

## 13. 索引节点与文件内容

`touch hello` 在 `/mnt/bean` 目录下

1. 索引节点的定位需要分两个步骤：首先确定该索引节点属于那个块组，然后在该块组内的索引节点表中找到所需的索引节点内容。

## 14. 相关代码

- [C 语言实现 EXT2 元数据读取](<https://github.com/professordeng/linux/tree/master/ext2>)