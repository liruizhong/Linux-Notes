   * [Git 快速入门](#git-快速入门)
      * [1. Git特性](#1-git特性)
         * [1.1  Git文件存储](#11--git文件存储)
         * [1.2  Git操作基本都是本地的](#12--git操作基本都是本地的)
         * [1.3  Git文件的三种状态](#13--git文件的三种状态)
      * [2. Git基本命令](#2-git基本命令)
         * [2.1 创建仓库](#21-创建仓库)
         * [2.2 检出仓库](#22-检出仓库)
         * [2.3 工作流](#23-工作流)
         * [2.4 添加和提交](#24-添加和提交)
         * [2.5 推送改动](#25-推送改动)
         * [2.6 分支介绍](#26-分支介绍)
         * [2.7 更新与合并](#27-更新与合并)
         * [2.8 标签](#28-标签)
         * [2.9 撤销本地改动](#29-撤销本地改动)
         * [2.10 删除文件](#210-删除文件)
         * [2.11 添加远程库](#211-添加远程库)
         * [2.12 配置](#212-配置)
      * [3. 分支管理](#3-分支管理)
         * [3.1 创建与合并分支](#31-创建与合并分支)
         * [3.2 解决冲突](#32-解决冲突)
         * [3.3 Bug 分支](#33-bug-分支)
         * [3.4 Feature分支](#34-feature分支)
         * [3.5 多人协作](#35-多人协作)
      * [4. 标签管理](#4-标签管理)
         * [4.1 创建标签](#41-创建标签)
         * [4.2 操作标签](#42-操作标签)
      * [5. Github配置](#5-github配置)
         * [5.1 为Github配置ssh key](#51-为github配置ssh-key)
         * [5.2 在Github上建立仓库](#52-在github上建立仓库)

# Git 快速入门
---

[TOC]

>git是目前最先进的分布式版本控制系统

## 1. Git特性

`git`相比`svn`可以很方便的在本地进行版本管理，如同在本地有一个版本管理服务器一样。你可以在合适的时候将本地版本提交到版本管理服务器。

### 1.1  Git文件存储

Git 和其它版本控制系统（包括 Subversion 和近似工具）的主要差别在于 Git 对待数据的方法。 概念上来区分，其它大部分系统以文件变更列表的方式存储信息。 这类系统（CVS、Subversion、Perforce、Bazaar 等等）将它们保存的信息看作是一组基本文件和每个文件随时间逐步累积的差异。 
![svn文件存储](https://git-scm.com/book/en/v2/images/deltas.png)

Git 不按照以上方式对待或保存数据。 反之，Git 更像是把数据看作是对小型文件系统的一组快照。 每次你提交更新，或在 Git 中保存项目状态时，它主要对当时的全部文件制作一个快照并保存这个快照的索引。 为了高效，如果文件没有修改，Git 不再重新存储该文件，而是只保留一个链接指向之前存储的文件。 Git 对待数据更像是一个 **快照流**。
![git文件存储](https://git-scm.com/book/en/v2/images/snapshots.png)

### 1.2  Git操作基本都是本地的

在 Git 中的绝大多数操作都只需要访问本地文件和资源，一般不需要来自网络上其它计算机的信息。 如果你习惯于所有操作都有网络延时开销的集中式版本控制系统，Git 在这方面会让你感到速度之神赐给了 Git 超凡的能量。 因为你在本地磁盘上就有项目的完整历史，所以大部分操作看起来瞬间完成。

举个例子，要浏览项目的历史，Git 不需外连到服务器去获取历史，然后再显示出来——它只需直接从本地数据库中读取。 你能立即看到项目历史。 如果你想查看当前版本与一个月前的版本之间引入的修改，Git 会查找到一个月前的文件做一次本地的差异计算，而不是由远程服务器处理或从远程服务器拉回旧版本文件再来本地处理。

### 1.3  Git文件的三种状态

 Git 有三种状态，你的文件可能处于其中之一：
 已修改（`modified`）和已暂存（`staged`）、已提交（`committed`）。  已修改表示修改了文件，但还没保存到数据库中。 已暂存表示对一个已修改文件的当前版本做了标记，使之包含在下次提交的快照中。已提交表示数据已经安全的保存在本地数据库中。

由此引入 Git 项目的三个工作区域的概念：Git 仓库、工作目录以及暂存区域。

![git三个工作区域](https://git-scm.com/book/en/v2/images/areas.png)

Git 仓库目录是 Git 用来保存项目的元数据和对象数据库的地方。 这是 Git 中最重要的部分，从其它计算机克隆仓库时，拷贝的就是这里的数据。

工作目录是对项目的某个版本独立提取出来的内容。 这些从 Git 仓库的压缩数据库中提取出来的文件，放在磁盘上供你使用或修改。

暂存区域是一个文件，保存了下次将提交的文件列表信息，一般在 Git 仓库目录中。 有时候也被称作“索引”，不过一般说法还是叫暂存区域。

 **基本的 Git 工作流程如下**：

1. 在工作目录中修改文件。
2. 暂存文件，将文件的快照放入暂存区域。
3. 提交更新，找到暂存区域的文件，将快照永久性存储到 Git 仓库目录。

如果 Git 目录中保存着的特定版本文件，就属于已提交状态。 如果作了修改并已放入暂存区域，就属于已暂存状态。 如果自上次取出后，作了修改但还没有放到暂存区域，就是已修改状态。

## 2. Git基本命令

### 2.1 创建仓库
什么是版本库呢？版本库又名仓库，英文名repository，你可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。

所以，创建一个版本库非常简单，首先，选择一个合适的地方，创建一个空目录：
```
$ mkdir learngit
$ cd learngit
$ pwd
/Users/michael/learngit
```
第二步，通过`git init`命令把这个目录变成Git可以管理的仓库：
```
$ git init
Initialized empty Git repository in /Users/michael/learngit/.git/
```
瞬间Git就把仓库建好了，而且告诉你是一个空的仓库（empty Git repository），细心的读者可以发现当前目录下多了一个`.git`的目录，这个目录是Git来跟踪管理版本库的，没事千万不要手动修改这个目录里面的文件，不然改乱了，就把Git仓库给破坏了。

如果你没有看到`.git`目录，那是因为这个目录默认是隐藏的，用`ls -ah`命令就可以看见。
### 2.2 检出仓库

执行如下命令以创建一个本地仓库的克隆版本：

```
git clone /path/to/repository
```

如果是远端服务器上的仓库，你的命令会是这个样子：

```
git clone username@host:/path/to/repository
```

### 2.3 工作流

你的本地仓库由 git 维护的三棵“树”组成。第一个是你的 `工作目录`，它持有实际文件；第二个是 `暂存区（Index）`，它像个缓存区域，临时保存你的改动；最后是 `HEAD`，它指向你最后一次提交的结果。
![git工作流](http://img.meelive.cn/NjMxNDUxNDYzNDk4MDM2.jpg) 

### 2.4 添加和提交

你可以提出更改（把它们添加到暂存区），使用如下命令：

```
git add <filename>
git add *
```

使用如下命令以实际提交改动

```
git commit -m "代码提交信息"
```

你的改动已经提交到了 **HEAD**，但是还没到你的远端仓库。

### 2.5 推送改动

执行如下命令以将这些改动提交到远端仓库:

```
git push origin master
```

master 为远程分支名。

可以使用如下命令添加远程服务器:

```
git remote add origin <server>
```

### 2.6 分支介绍

分支是用来将特性开发绝缘开来的。在你创建仓库的时候，*master* 是“默认的”分支。在其他分支上进行开发，完成后再将它们合并到主分支上。

![分支模型](http://img.meelive.cn/MjQ3NzgxNDYzNDk4MzUy.jpg)

创建一个叫做“feature_x”的分支，并切换过去:

```
git checkout -b feature_x
```

切回主分支:

```
git checkout master
```

再把新建的分支删掉：

```
git branch -d feature_x
git brach -a
```

除非你将分支推送到远端仓库，不然该分支就是 *不为他人所见的*

```
git push origin <branch>
```

### 2.7 更新与合并

首先获取远程的最新改动, 执行:

```
git pull
git fech = git fetch && git merge
```

以在你的工作目录中 *获取（fetch）* 并 *合并（merge）* 远端的改动。
要合并其他分支到你的当前分支（例如 master），执行

```
git merge <branch>
```

git 会尝试去自动合并改动,  但是并不保证每次都成功, 并可能出现冲突。 这时候就需要你修改这些文件来手动合并这些*冲突*, 改完之后，你需要执行如下命令以将它们标记为合并成功：

```
git add <filename>
```

在合并改动之前，可以使用如下命令预览差异

```
git diff <source_branch> <target_branch>
```

### 2.8 标签

为软件发布创建标签, 在SVN中也有类似的功能. 执行如下命令创建一个叫做 *1.0.0* 的标签：

```
git tag 1.0.0 1b2e1d63ff
```

*1b2e1d63ff* 是想要标记的提交 ID的前16位字符,  可以只是用提交ID的前几位, 只要能保证提交ID的唯一性。 可以使用下列命令获取提交 ID：

查看提交历史记录：
```
git log
```
要随时掌握工作区的状态，使用`git status`命令

如果`git status`告诉你有文件被修改过，用`git diff`可以查看修改内容


### 2.9 撤销本地改动

在SVN中也有类似的功能(svn revert), 使用如下命令：

```
git checkout -- <filename>
```

此命令会使用 HEAD 中的最新内容替换掉你的工作目录中的文件。已添加到暂存区的改动以及新文件都不会受到影响。

如果要丢弃在本地的所有改动和提交，可以先获取服务器上的最新仓库状态，然后将本地主分支只想它：

```
git fetch origin
git reset --hard/soft/mixed origin/master
```

`HEAD`指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令
```
git reset --hard commit_id
```
穿梭前，用`git log`可以查看提交历史，以便确定要回退到哪个版本。
要重返未来，用`git reflog`查看命令历史，以便确定要回到未来的哪个版本。

场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令
```
git checkout -- file
```
场景2：你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令
```
git reset HEAD <file>
```
就回到了场景1，第二步按场景1操作。

### 2.10 删除文件

如果想删除Git仓库中的文件，可以使用如下命令

```
git rm <filename>
```

### 2.11 添加远程库
本地仓库要关联一个远程库，使用命令
```
git remote add origin git@server-name:path/repo-name.git
```

关联后，使用命令`git push -u origin master`第一次推送`master`分支的所有内容

此后，每次本地提交后，只要有必要，就可以使用命令`git push origin master`推送最新修改

分布式版本系统的最大好处之一是在本地工作完全不需要考虑远程库的存在，也就是有没有联网都可以正常工作，而SVN在没有联网的时候是拒绝干活的！当有网络的时候，再把本地提交推送一下就完成了同步，真是太方便了！

### 2.12 配置

使用如下命令，每次提交记录都会包含用户名和邮箱

```
git config --global user.name "your name"
git config --global user.email "your_email@youremail.com"
```

## 3. 分支管理
### 3.1 创建与合并分支
* 创建 `dev` 分支，然后切换到 `dev` 分支
```
$ git checkout -b dev
Switched to a new branch 'dev'
```
* `git checkout`命令加上`-b`参数表示创建并切换，相当于以下两条命令：
```
$ git branch dev
$ git checkout dev
Switched to branch 'dev'
```
* 然后，用`git branch`命令查看当前分支：
```
$ git branch
* dev
  master
```
* 合并分支到到当前分支
```
$ git merge dev
Updating d46f35e..b17d20e
Fast-forward
 readme.txt | 1 +
 1 file changed, 1 insertion(+)
```
### 3.2 解决冲突
当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。

解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交。

用`git log --graph`命令可以看到分支合并图。

[廖雪峰老师解决冲突介绍](https://www.liaoxuefeng.com/wiki/896043488029600/900004111093344)

### 3.3 Bug 分支
当线上遇到紧急 `bug` 需要修复，而自己正在开发新功能而且没有开发完当前无法提交，`git` 提供了一个 `stash` 功能，可以把当前的工作现场“存储”起来，等以后恢复现场后再继续工作
```
$ git stash
Saved working directory and index state WIP on dev: f52c633 add merge
```
当修复完 bug ,如果回到原先的工作现场呢？用`git stash list`命令看看：
```
$ git stash list
stash@{0}: WIP on dev: f52c633 add merge
```

工作现场还在，Git把stash内容存在某个地方了，但是需要恢复一下，有两个办法：

一是用`git stash apply`恢复，但是恢复后，stash内容并不删除，你需要用`git stash drop`来删除；

另一种方式是用`git stash pop`，恢复的同时把stash内容也删了

在master分支上修复的bug，想要合并到当前dev分支，可以用`git cherry-pick <commit>`命令，把bug提交的修改“复制”到当前分支，避免重复劳动。

[廖雪峰老师Bug分支介绍](https://www.liaoxuefeng.com/wiki/896043488029600/900388704535136)

### 3.4 Feature分支
开发一个新feature，最好新建一个分支；

如果要丢弃一个没有被合并过的分支，可以通过`git branch -D <name>`强行删除。

### 3.5 多人协作
查看远程库信息，使用`git remote -v`；

本地新建的分支如果不推送到远程，对其他人就是不可见的；

从本地推送分支，使用`git push origin branch-name`，如果推送失败，先用`git pull`抓取远程的新提交；

在本地创建和远程分支对应的分支，使用`git checkout -b branch-name origin/branch-name`，本地和远程分支的名称最好一致；

建立本地分支和远程分支的关联，使用`git branch --set-upstream branch-name origin/branch-name`；

从远程抓取分支，使用`git pull`，如果有冲突，要先处理冲突。

## 4. 标签管理
### 4.1 创建标签
命令`git tag <tagname>`用于新建一个标签，默认为`HEAD`，也可以指定一个commit id；

命令`git tag -a <tagname> -m "blablabla..."`可以指定标签信息；

命令`git tag`可以查看所有标签。

### 4.2 操作标签
命令`git push origin <tagname>`可以推送一个本地标签；

命令`git push origin --tags`可以推送全部未推送过的本地标签；

命令`git tag -d <tagname>`可以删除一个本地标签；

命令`git push origin :refs/tags/<tagname>`可以删除一个远程标签。


## 5. Github配置

- github是一个基于git的代码托管平台, 提供git仓库的web管理
- SourceTree 是 Windows 和Mac OS X 下免费的 Git  客户端，同时也是Subversion版本控制系统工具。支持创建、克隆、提交、push、pull 和合并等操作。


### 5.1 为Github配置ssh key

在本地创建ssh密钥， 使用如下命令：

```
ssh-keygen -t rsa -C "your_email@youremail.com"
```

后面的`your_email@youremail.com`改为你在github注册的邮箱，之后会要求确认路径和输入密码，这里使用默认的一路回车就行。成功的话会在`~/`下生成`.ssh`文件夹，进去，打开`id_rsa.pub`，复制里面的`key`。

登陆github，在右上角点击个人设置页面（Account Settings），左边选择SSH Keys，Add SSH Key,title随便填，粘贴在刚才生成的`key`，保存。

![添加公钥2](https://www.liaoxuefeng.com/files/attachments/919021379029408/0)

使用如下命令， 验证是否成功

```
ssh -T gi@106.38.48.147
```

如果是第一次的会提示是否continue，输入yes就会看到：`You've successfully authenticated, but GitHub does not provide shell access `， 这就表示已成功连上github。

### 5.2 在Github上建立仓库

点击`+`在弹出的选择框中点击`New repository`，在Repository name中填入仓库名，点击`Create repository`确认：

![建立仓库](https://www.liaoxuefeng.com/files/attachments/919021631860000/0)



参考网站：
[廖雪峰 git 教程 ](https://www.liaoxuefeng.com/wiki/896043488029600)
[Pro Git Book](https://git-scm.com/book/zh/v2)





