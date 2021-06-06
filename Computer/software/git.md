---
title: Git
---

在 git 下载完后，需要配置用户名和用户邮箱（不要直接复制下面语句，那是我的名字和邮箱）。

```bash
git config --global user.name "professordeng"
git config --global user.email "2248262060@qq.com"
```

下面记录了最常用的一些命令。

```bash
git help                    # git 的说明书
git add --all               # 将所有修改过的文件放到暂存区
git add filename            # 将某个文件提交到暂存区
git commit -m "description" # 将暂存区的文件提交到本地仓库
git log                     # 历史记录
git checkout filename       # 撤回当前工作区的某文件的修改
git status                  # 查看工作区和暂存区的修改情况
git diff                    # 已经修改的内容对比
```

## 1. 绑定 github 账号

由于我经常重装机子，所以仓库通常备份到 github 上，因此需要绑定 github 账号。

```bash
git config --list    # 查看自己的名字和邮箱，没有的话自行配置
ssh-keygen -t rsa -C "2248262060@qq.com" # 生成密钥和公钥
```

然后在用户目录下有个 `.ssh` 的文件，利用 `cat` 指令得到公钥复制到 `github` 设置中的 ssh 中完成认证。接下来客户端和 `github` 就可以互相传送文件了。

```bash
git pull origin master  # 拉取远程仓库的 master 更新
git push origin master  # 推送 master 更新到远程仓库
```

如果原来使用 https 和远程仓库同步，需要每次都输入账号密码，可使用如下指令将 https 连接改为 ssh 连接。

```bash
git remote set-url origin xxx.git
```

## 2. 版本回退

1. 工作区

   工作区的文件回退，修改过的文件直接被删除或扔到回收站。

   ```bash
   git checkout filename   # 对某一文件进行回退。
   git checkout .          # 对所有跟踪文件进行回退。
   ```

2. 暂存区

   暂存区的文件回退，修改过的文件会被扔到工作区。

   ```bash
   git reset HEAD file     # 将暂存区文件放回工作区。
   git reset HEAD .        # 将所有暂存区文件放回工作区。
   ```

3. 已经提交的修改

   ```bash
   git reset --hard HEAD^      # 彻底回到上一个版本。
   git reset --hard commit_id  # 回到某一版本。
   ```

   `commit_id` 可以使用 `git log` 查看，`commit_id` 很长，但是一般只要敲前面几位就行，不会重复就行。如果有一个 `commit_id` 是多个分支的交点，就会出现指代不明确的问题。

## 3. 分支管理

如果你的项目需要多个方向开发或者多人同时开发不同的部件，那么就需要进行分支管理。git 默认主分支是 master。

1. 创建分支 `dev`

   ```bash
   git branch dev    # 创建 dev 分支
   git checkout dev  # 将 dev 分支切换为当前分支
   git branch        # 列出所有分支
   ```

2. 将分支 `dev` 合并到 master 上

   ```bash
   git checkout master  # 切换 master 为当前分支
   git merge dev        # 将 dev 合并到当前分支
   ```

3. 删除 `dev` 分支（如果 `dev` 是当前分支，需要先切换）

   ```bash
   git branch -d dev    # 删除 dev 分支
   git branch           # 列出所有分支
   ```

4. 将分支 `dev` 推送到远程仓库

   ```bash
   git push origin dev
   ```

5. 从远程仓库拉取 `dev` 分支

   ```bash
   git clone -b dev address
   ```

   这里的 address 是指项目地址。

## 4. GitHub 无 commit 记录

提交记录是很有用的，无论是炫耀，还是查找记录。如果你发现你的 commit 没有记录，那么大概率是提交时的邮箱不对。

1. 查看提交时的邮箱是否和 GitHub settings 里的主邮箱一致。

   ```bash
   git show
   Author:xxx <youremail@email.com>
   ```

2. 修改邮箱

   ```bash
   git config --global user.email "youremail@email.com"
   ```

3. 查看修改后的邮箱是否和 GitHub settings 的主邮箱一致

   ```bash
   git config --global user.email
   ```

## 5. GitHub 代理

```bash
git config --global http.proxy 'http://127.0.0.1:1080'
```

其中端口是你代理软件的代理端口。如果软件关了，把上面的配置删掉，如下：

```bash
git config --global --unset http.proxy
```

## 6. Cherry-pick

假如 master 是主分支，dev 是开发分支，此时 dev 分支有多个 commit 要合并到 master 分支，我们可以使用 cherry-pick 逐个将 commit 移植到 master 分支上。

1. 拉取远程服务器上的多个分支：

   ```bash
   cd Desktop          # 桌面上操作
   git clone A.git     # 拉取远程 A 分支
   cd A                # 进入仓库
   git branch -a       # 查看所有分支
   * A
     remotes/origin/HEAD -> origin/master
     remotes/origin/A
     remotes/origin/B
   git checkout -b B origin/B  # 拉取远程 B 分支并切换到该分支
   ```

   此时我们将服务器上的 A 和 B 分支都拉取到了本地。

2. 使用 cherry-pick 将 B 分支的一个 commit （假设记录就是 `sha_x`）移动到 B 分支上：

   ```bash
   git switch A
   git git cherry-pick sha_x
   ```

   此时，我们就将 B 分支上记录为 `sha_x` 的 `commit` 移植到了 A 分支。

## 7. 远程仓库 commit 和工作区文件冲突

我工作的时候仓库只有两种状态：

1. 干净的工作区，commit 顺手就同步到远程服务器
2. 本地的修改存放在工作区

在合作的时候就会出现，远程仓库的 commit 比你的新，导致文件冲突，此时使用 `stash` 指令可以解决该冲突。步骤如下：

```bash
git stash
git pull
git stash pop
```

这样就可以同步远程服务器的 commit 了，如果还存在冲突，那么手动解决即可。

## 进阶

- [Git LFS](https://git-lfs.github.com/) ：编码过程中大文件提交请使用 Git LFS。使用 Git LFS 提交的大文件不占用 Git 仓库存储空间，理论上可以提交的单个文件大小无上限。
- [Git LFS 教程](https://help.coding.net/docs/host/git/lfs.html)
- [.gitignore 生成网站](https://gitignore.io)
- [git cherry pick](http://www.ruanyifeng.com/blog/2020/04/git-cherry-pick.html)
- [learn git branching](https://oschina.gitee.io/learn-git-branching/)

