# Git<详解>

---

> 结合狂神视频+gitee教程
>
> - 参考视频：https://www.bilibili.com/video/BV1FE411P7B3?from=search&seid=16286896221963730977
> - 参考教程：https://gitee.com/all-about-git
> - 游戏教程：https://oschina.gitee.io/learn-git-branching/

## 版本控制

> 什么是版本控制？

- 版本控制 Revision control：是一种在开发的过程中用于管理我们对文件，目录或工程等内容的修改历史，方便查看更改历史记录，备份以便恢复以前版本的软件工程技术
  1. 实现跨区域多人协同开发
  2. 追踪和记载一个活多个文件的历史记录
  3. 组织和保护你的源代码和文档
  4. 统计工作量
  5. 并行开发、提高开发效率
  6. 跟踪记录整个软件的开发过程
  7. 减轻开发人员的负担，节省时间，同时降低认为错误
- 简单说就是用于管理多人协同开发项目的技术
- 没有进行版本控制或者版本控制本身缺乏正确的流程管理，在软件开发过程中将会引入很多问题，如软件代码的一致性、软件内容的冗余、软件过程的事务性、软件开发过程中的并发性、软件源代码的安全性、以及软件的整合等问题

- 没有版本管理的现实～～

<img src="https://gitee.com/kidjaya/imageStorage/raw/master/images/image-20201018183637293.png" alt="image-20201018183637293" style="zoom:50%;" />

> 常见的版本控制工具

- Git
- SVN：SubVersion
- CVS：Concurrent Version System
- VSS：Micorosoft Visual SourceSafe
- TFS：Team Foundation Server
- Visual Studio Online

版本控制的工具特别多，使用最广泛的是Git和SVN

> 版本控制分类

1. 本地版本控制

- 记录文件每次的更新，可以对每个版本做一个快照，或是记录补丁文件，适合个人使用（eg：RCS工具）

<img src="https://gitee.com/kidjaya/imageStorage/raw/master/images/image-20201018184138609.png" alt="image-20201018184138609" style="zoom:50%;" />

2. 集中版本控制

- 所有的版本数据都保存在服务器上，协同开发者从服务器上同步更新或上传自己的修改

<img src="https://gitee.com/kidjaya/imageStorage/raw/master/images/image-20201018184251544.png" alt="image-20201018184251544" style="zoom:50%;" />

- 所有的版本数据都存在服务器上，用户的本地只有自己以前所同步的版本，如果不连网的话，用户就看不到历史版本，也无法切换版本验证问题，或在不同工作分支工作。
  而且，所有数据都保存在单一的服务器上，有很大的风险这个服务器会损坏，这样就丢失所有的数据，当然可以定期备份。（eg：SVN、CVS、VSS工具）

3. 分布式版本控制

- 所有版本信息仓库全部同步到本地的每个用户，这样就可以在本地查看所有版本历史，可以离线在本地提交，只需要在联网时push到相应的服务器或者其他用户那里。由于每个用户那里保存的都是所有的版本数据，只要有一个用户的设备没有问题就可以恢复所有的数据，但这增加了本地存储空间的占用。

<img src="https://gitee.com/kidjaya/imageStorage/raw/master/images/image-20201018184721226.png" alt="image-20201018184721226" style="zoom:50%;" />

> Git与SVN的主要区别

- SVN是集中式版本控制系统，版本库是集中放在中央服务器的，而工作的时候，用的都是自己的电脑，所以首先要从中央服务器得到最新的版本，然后工作，完成工作后，需要把自己修改的代码推送到中央服务器。集中式版本控制系统是必须联网才能工作，对网络宽带要求较高
- GIt是分布式版本控制系统，没有中央服务器，每个人的电脑就是一个完整的版本库，工作的时候不需要联网了，因为版本都在自己电脑上。协同的方法是这样的：比如说自己在电脑上改了文件A，其他人也在电脑上改了文件A，这时，你们两之间只需要把各自修改的代码推送给对方，就可以互相看到对方的修改了。
- Git是目前世界上最先进的分布式版本控制系统

---

## Git简史

同生活中的许多伟大事物一样，Git 诞生于一个极富纷争大举创新的年代。

Linux 内核开源项目有着为数众多的参与者。 绝大多数的 Linux 内核维护工作都花在了提交补丁和保存归档的繁琐事务上（1991－2002年间）。 到 2002 年，整个项目组开始启用一个专有的分布式版本控制系统 BitKeeper 来管理和维护代码。

到了 2005 年，开发 BitKeeper 的商业公司同 Linux 内核开源社区的合作关系结束，他们收回了 Linux 内核社区免费使用 BitKeeper 的权力。 这就迫使 Linux 开源社区（特别是 Linux 的缔造者 Linus Torvalds）基于使用 BitKeeper 时的经验教训，开发出自己的版本系统。 他们对新的系统制订了若干目标：

- 速度
- 简单的设计
- 对非线性开发模式的强力支持（允许成千上万个并行开发的分支）
- 完全分布式
- 有能力高效管理类似 Linux 内核一样的超大规模项目（速度和数据量）

自诞生于 2005 年以来，Git 日臻成熟完善，在高度易用的同时，仍然保留着初期设定的目标。 它的速度飞快，极其适合管理大项目，有着令人难以置信的非线性分支管理系统（参见 [Git 分支](https://git-scm.com/book/zh/v2/ch00/ch03-git-branching)）。

---

## Git环境配置

> 软件下载

- 打开[git](https://git-scm.com/),下载对应操作系统的版本
- Windows无脑安装即可，Mac自带git

> 启动Git

- 安装成功后在开始菜单中会有Git项，菜单有三个程序
  - Git Bash：Unix和Linux风格的命令行，使用最多，最推荐使用
  - Git CMD：Windows风格的命令行
  - Git GUI：图形界面的Git，不建议初学则使用，尽量先熟悉常用命令

![image-20201018193717629](https://gitee.com/kidjaya/imageStorage/raw/master/images/image-20201018193717629.png)

> 基本的Linux命令学习

```bash
#1.改变目录
cd
#2.回退到上一个目录，直接cd进入默认目录
cd ..
#3.显示当前所在目录路径
pwd
#4.列出当前目录中的所有文件，只不过ll列出的内容更为详细
ls/ll
#5.新建一个文件（在当前目录下）
touch
#6.删除一个文件
rm
#7.新建一个目录，就是新建一个文件夹
mkdir
#8.删除一个文件夹
rm -r #eg：rm -r src
rm -rf #强制删除一个文件夹
#9.移动文件
mv    #eg: mv index.html src
#10.重新初始化终端/清屏
reset
#11.清屏
clear
#12.查看命令历史
history
#13.帮助
help
#14.退出
exit
#15.表示注视
#
```

> Git配置

- 查看配置 `git config -l `

<img src="https://gitee.com/kidjaya/imageStorage/raw/master/images/image-20201018195722130.png" alt="image-20201018195722130" style="zoom:50%;" />

- 查看不同等级的配置文件：

```bash
#查看系统config
git config --system --list
#查看当前用户配置
git config --global --list
```

- Git相关配置其实就是一个文件（Windos目录）
  1. git安装目录下的 etc/gitconfig （system级别）
  2. C:/User/Administrator/.gitconfig 只适用于当前登陆用户的配置（全局配置）

- 这里可以直接编辑配置文件，通过命令设置后会响应到这里

> 设置用户名与邮箱（用户标识，必要）

- 当你安装git后首先要做的事情就是设置你的用户名和email，这是非常必要的，因为每次git提交都会使用该信息。他被永远嵌入到你的提交当中

```bash
git config --global user.name "kidjaya" 
git config --global user.email 1725236585@qq.com
```

- 只需要做一次这个设置，如果你场地了--global选项，因为Git总将会使用这个信息来处理你在系统中所操作的一切。如果你希望在一个特定的项目中使用不同的名称或者email地址，你可以在该项目中运行该命令而不要`--global`选项。总之，他是全局配置，不加的话为 某个项目的特定配置

---

## Git基本理论

> 工作区域

- Git本地有三个工作区域：工作目录 Working Directory、暂存区 Stage/Index、资源库 Repository/Git Directory。如果再加上远程的git仓库`git Remote Directory`可以算做四个工作区域

<img src="https://gitee.com/kidjaya/imageStorage/raw/master/images/image-20201018200933882.png" alt="image-20201018200933882" style="zoom:50%;" />

- Workspace：工作区，就是你平时存放代码的地方
- Index/Stage：暂存区，用于临时存放你的改动，事实上它知识一个文件，保存即将提交到文件的列表信息
- Repository：仓库区/本地仓库，就是安全存放数据的位置，这里面有你提交到所有版本的数据。其中HEAD指向最新放入仓库的版本（HEAD与最新分支一致情况）
- Remote：远程仓库，托管代码的服务器，可以简单的认为是你项目组中的一台电脑用于远程数据交换

> 本地的三个区域确切的说应该是git仓库中HEAD指向的版本

<img src="https://gitee.com/kidjaya/imageStorage/raw/master/images/image-20201018235053779.png" alt="image-20201018235053779" style="zoom:50%;" />

- Directory：使用Git管理的一个目录，也就是一个仓库，包含我们的工作空间和Git的管理空间
- WorkSpace：需要通过Git进行版本控制的目录和文件，这些目录和文件组成了工作空间
- .git：存放Git管理信息的目录，初始化仓库的时候自动创建。
- Index/Stage：暂存区/提交更新区，在提交进入repo之前，我们可以吧所有的更新放在暂存区
- Local Repo：本地仓库，一个存放在本地的版本仓库，HEAD指向的是当前开发的分支
- Stash：隐藏，是一个工作状态保存栈，用于保存/恢复WorkSpace中的临时状态

> 工作流程

- git的工作流程
  1. 在工作目录中添加、修改文件
  2. 将需要进行版本管理的文件放入暂存区域
  3. 将暂存区域的文件提交到git仓库
- 所以，git管理的文件有三种状态：已修改modified，已暂存staged，已提交committed

<img src="https://gitee.com/kidjaya/imageStorage/raw/master/images/image-20201019000053435.png" alt="image-20201019000053435" style="zoom:50%;" />

## Git项目搭建

> 创建工作目录与常用命令

- 工作目录一般就是你希望Git帮助你管理的文件夹，可以是你项目的目录，也可以是一个空目录，建议不要有中文
- 日常使用命令指示图

<img src="https://gitee.com/kidjaya/imageStorage/raw/master/images/image-20201019000353450.png" alt="image-20201019000353450" style="zoom:50%;" />

> 本地仓库搭建
> `创建本地仓库的方法有两种：一种是是创建全新的仓库，另一种是克隆远程仓库`

1. 创建权限的仓库，需要使用GIT管理的项目的根目录执行：

  ```bash
# 在当前目录新建一个Git代码库
  git init
  ```

  2. 执行后可以看到，仅仅在项目目录多出了一个.git目录，关于版本等的所有信息都在这个目录里面

> 克隆远程仓库

1. 另一种方法是克隆远程目录，就是将远程服务器上的仓库完全镜像一份至本地

```bash
# 克隆一个项目和他的整个代码历史（版本信息）
git clone [url]
```

2. gitee或者github上克隆开源或者自己的项目～

---

## Git文件操作

> 文件的四种状态

- 版本控制就是对文件的版本控制，要对文件进行修改，提交等操作，首先要知道文件当前在什么状态，不然可能会提交了现在还不想提交的文件，或者要提交的文件没有提交上

  - **Untracked**：未跟踪，此文件在文件夹中，但是没有加入到git库，不参与版本控制，通过`git add`状态改变为`Staged`

  - **Unmodify**：文件已经入库，未修改，即版本库中的文件快照内容与文件夹完全一致，这种类型的文件有两种去处。如果他被修改，而变为`Modified`。如果使用`git rm`移出版本库，则成为`Untracked`文件

  - **Modified**：文件已修改，仅仅是修改，并没有进行其他的操作。这个文件也有两个去处，通过

    `git add`可进入暂存`staged`状态，使用`git checkout`则丢弃修改，返回到`unmodifuy`状态，

    这个`git checkout`即从库中取出文件，覆盖当前修改

  - **Staged**：暂存状态，执行`git commit`则将修改同步到库中，这时库中的文件和本地文件又变为一致，文件为`unmodify` 状态。执行`git reset HEAD filename`取消暂存，文件状态为`Modified`

  　

> 查看文件状态

- 上面说文件有4中状态，通知如下命令个可以查看到文件状态

```bash
# 查看置顶文件状态
git status [filename]

# 查看所有文件状态
git status
```



>  忽略文件

- 有些时候我们不想把某些文件纳入版本控制中，比如数据库文件，临时文件，设计文件等
  在主目录下建立`.gitgnore`文件，此文件有如下规则
  1. 忽略文件中的空行或以`#`开始的行将会被忽略
  2. 可以使用Linux通配符。例如（*）代表任意多个字符，问号（?）代表一个字符，方括号([abc])代表可选字符范围，打括号({string1,string2,...})代表可选的字符串
  3. 如果名称的最前面有一个感叹号(!)，代表例外规则，将不被忽略
  4. 如果名称的最前面是一个路径分隔符(/)，表示要忽略的文件在此目录下，而子目录的文件不忽略
  5. 如果名称的最后面是一个路径分隔符(/)，表示要忽略的是此目录下该名称的子目录，而非文件（默认文件或目录都忽略）

```bash
#为注释
*.txt.  		#忽略所有 .txt结尾的文件
!lib.txt 		#但lib.txt除外
/temp				#仅忽略项目根目录下的TODO文件，不包括其他目录temp
build/			#忽略build/目录下的所有文件
doc/*.txt	  #会忽略 doc/note.txt 但不包括doc/下面二级目录的txt文件
```

---

## Git常用命令

> [Git大全](https://gitee.com/all-about-git)
>
> 参考：以下常用命令来自阮一峰老师的博客文章《常用 Git 命令清单》，感谢阮老师！

### 仓库

```bash
# 在当前目录新建一个Git代码库
$ git init

# 新建一个目录，将其初始化为Git代码库
$ git init [project-name]

# 下载一个项目和它的整个代码历史
$ git clone [url]
```

### 配置

```bash
# 显示当前的Git配置
$ git config --list

# 编辑Git配置文件
$ git config -e [--global]

# 设置提交代码时的用户信息
$ git config [--global] user.name "[name]"
$ git config [--global] user.email "[email address]"
```

### 增加删除文件

```bash
# 添加指定文件到暂存区
$ git add [file1] [file2] ...

# 添加指定目录到暂存区，包括子目录
$ git add [dir]

# 添加当前目录的所有文件到暂存区
$ git add .

# 添加每个变化前，都会要求确认
# 对于同一个文件的多处变化，可以实现分次提交
$ git add -p

# 删除工作区文件，并且将这次删除放入暂存区
$ git rm [file1] [file2] ...

# 停止追踪指定文件，但该文件会保留在工作区
$ git rm --cached [file]

# 改名文件，并且将这个改名放入暂存区
$ git mv [file-original] [file-renamed]
```

### 代码提交

```bash
# 提交暂存区到仓库区
$ git commit -m [message]

# 提交暂存区的指定文件到仓库区
$ git commit [file1] [file2] ... -m [message]

# 提交工作区自上次commit之后的变化，直接到仓库区
$ git commit -a

# 提交时显示所有diff信息
$ git commit -v

# 使用一次新的commit，替代上一次提交
# 如果代码没有任何新变化，则用来改写上一次commit的提交信息
$ git commit --amend -m [message]

# 重做上一次commit，并包括指定文件的新变化
$ git commit --amend [file1] [file2] ...
```

### 分支

```bash
# 列出所有本地分支
$ git branch

# 列出所有远程分支
$ git branch -r

# 列出所有本地分支和远程分支
$ git branch -a

# 新建一个分支，但依然停留在当前分支
$ git branch [branch-name]

# 新建一个分支，并切换到该分支
$ git checkout -b [branch]

# 新建一个分支，指向指定commit
$ git branch [branch] [commit]

# 新建一个分支，与指定的远程分支建立追踪关系
$ git branch --track [branch] [remote-branch]

# 切换到指定分支，并更新工作区
$ git checkout [branch-name]

# 切换到上一个分支
$ git checkout -

# 建立追踪关系，在现有分支与指定的远程分支之间
$ git branch --set-upstream [branch] [remote-branch]

# 合并指定分支到当前分支
$ git merge [branch]

# 选择一个commit，合并进当前分支
$ git cherry-pick [commit]

# 删除分支
$ git branch -d [branch-name]

# 删除远程分支
$ git push origin --delete [branch-name]
$ git branch -dr [remote/branch]
```

### 标签

```bash
# 列出所有tag
$ git tag

# 新建一个tag在当前commit
$ git tag [tag]

# 新建一个tag在指定commit
$ git tag [tag] [commit]

# 删除本地tag
$ git tag -d [tag]

# 删除远程tag
$ git push origin :refs/tags/[tagName]

# 查看tag信息
$ git show [tag]

# 提交指定tag
$ git push [remote] [tag]

# 提交所有tag
$ git push [remote] --tags

# 新建一个分支，指向某个tag
$ git checkout -b [branch] [tag]
```

### 查看信息

```bash
# 显示有变更的文件
$ git status

# 显示当前分支的版本历史
$ git log

# 显示commit历史，以及每次commit发生变更的文件
$ git log --stat

# 搜索提交历史，根据关键词
$ git log -S [keyword]

# 显示某个commit之后的所有变动，每个commit占据一行
$ git log [tag] HEAD --pretty=format:%s

# 显示某个commit之后的所有变动，其"提交说明"必须符合搜索条件
$ git log [tag] HEAD --grep feature

# 显示某个文件的版本历史，包括文件改名
$ git log --follow [file]
$ git whatchanged [file]

# 显示指定文件相关的每一次diff
$ git log -p [file]

# 显示过去5次提交
$ git log -5 --pretty --oneline

# 显示所有提交过的用户，按提交次数排序
$ git shortlog -sn

# 显示指定文件是什么人在什么时间修改过
$ git blame [file]

# 显示暂存区和工作区的差异
$ git diff

# 显示暂存区和上一个commit的差异
$ git diff --cached [file]

# 显示工作区与当前分支最新commit之间的差异
$ git diff HEAD

# 显示两次提交之间的差异
$ git diff [first-branch]...[second-branch]

# 显示今天你写了多少行代码
$ git diff --shortstat "@{0 day ago}"

# 显示某次提交的元数据和内容变化
$ git show [commit]

# 显示某次提交发生变化的文件
$ git show --name-only [commit]

# 显示某次提交时，某个文件的内容
$ git show [commit]:[filename]

# 显示当前分支的最近几次提交
$ git reflog
```

### 远程同步

```bash
# 下载远程仓库的所有变动
$ git fetch [remote]

# 显示所有远程仓库
$ git remote -v

# 显示某个远程仓库的信息
$ git remote show [remote]

# 增加一个新的远程仓库，并命名
$ git remote add [shortname] [url]

# 取回远程仓库的变化，并与本地分支合并
$ git pull [remote] [branch]

# 上传本地指定分支到远程仓库
$ git push [remote] [branch]

# 强行推送当前分支到远程仓库，即使有冲突
$ git push [remote] --force

# 推送所有分支到远程仓库
$ git push [remote] --all
```

### 撤销

```bash
# 恢复暂存区的指定文件到工作区
$ git checkout [file]

# 恢复某个commit的指定文件到暂存区和工作区
$ git checkout [commit] [file]

# 恢复暂存区的所有文件到工作区
$ git checkout .

# 重置暂存区的指定文件，与上一次commit保持一致，但工作区不变
$ git reset [file]

# 重置暂存区与工作区，与上一次commit保持一致
$ git reset --hard

# 重置当前分支的指针为指定commit，同时重置暂存区，但工作区不变
$ git reset [commit]

# 重置当前分支的HEAD为指定commit，同时重置暂存区和工作区，与指定commit一致
$ git reset --hard [commit]

# 重置当前HEAD为指定commit，但保持暂存区和工作区不变
$ git reset --keep [commit]

# 新建一个commit，用来撤销指定commit
# 后者的所有变化都将被前者抵消，并且应用到当前分支
$ git revert [commit]

暂时将未提交的变化移除，稍后再移入
$ git stash
$ git stash pop
```

### 其他

```bash
# 生成一个可供发布的压缩包
$ git archive
```

---

## Git命令练习

> 练习网址：https://oschina.gitee.io/learn-git-branching/

### 基础篇

#### 1. git commit

- 提交`暂存区/工作区`的改变到`仓库区`，使得当前`工作区`和`仓库区`保持一致，然后将`HEAD`指向提交之后的记录～

#### 2. git branch

- 分支非常的轻量
- 建议：早建分支，多用分支

```bash
git checkout -b <your-branch-name> # 来实现创建一个新的分支同时切换到新创建的分支
```

```bash
git branch <your-branch-name> # 创建一个新的分支，指向当前的HEAD
```

```bash
git checkout <your-branch-name> # 切换分支
```

#### 3. git merge

- 合并分支，产生一个特殊的提交记录，他有两个父节点，包含了父节点的所有提交记录
- 合并分支的分支指向不变，当前合并的支线，指向产生的新的特殊分支节点～

```bash
git merge <your-branch-name> # 合并分支
```

#### 4. git rebase

- 实际上就是取出一系列的提交记录，复制他们，然后在另外一个地方逐个的放下去
- 优势：可以创造更线性的提交历史

```bash
git rebase <your-branch-name> # 将当前分支的提交记录copy到你设置的分支下
```

- 再将你设置的分支<your-branch-name>使用`git rebase + 之前设置rebase的分支 `将两个分支合并后的指向，指向为同一个地方～



### 高级篇

#### 1.分离HEAD

- HEAD一般都是指向你正在其基础上进行工作的提交记录
- HEAD总是指向当前分支最近的一次提交记录，大多数修改提交树的git命令都是从改变HEAD开始的
- HEAD通常下是指向各分支名的（比如master），在提交时，改变了bugFix的状态，这一变化通过HEAD变得**可见**
- HEAD可以通过`cat .git/HEAD` 和`git symbolic-ref HEAD`查看他的指向 

```bash
# 分离的HEAD
# 原本：HEAD -> master -> C1
git checkout c1
# 现在：master -> C1
#      HEAD -> C1
```



#### 2.相对引用^

- 通过指定提交记录的hash值在git中移动不方便，可以通过命令`git log`查看提交记录的hash值
  hash值一般很长（给予SHA-1 共40位）
  git对哈希的处理很智能，只需要提供能为宜表示提交记录的前几个字符即可，而不是一长串的hash值
- 对于上面的不便，git引入了相对引用，通过相对引用就可以从一个易于记忆的地方开始计算位置～
  - 使用`^`向上移动**1**个提交记录
  - 使用~<num>向上移动多个提交记录
- 操作符`^`把这个符号加载引用名称的后面，表示让git寻找置顶提交记录的父提交。所以`master^`相当于`master`的父节点，而`master^^`就是`master^`的父节点



#### 3.相对引用～

- 如果需要在提交树上移动很多步的话，使用`^`就很麻烦，所以引入了`~`，该操作符后面可以跟一个数字（可选，不跟数字时与`^`相同，向上移动一次），指定向上移动多少次
- 使用相对引用最多的就是移动分支，可以使用`-f`选项让分支指向另一个提交
  例如：`git branch -f master HEAD~3`：将master分支**强制指向**HEAD的第3级父提交



#### 4.撤销变更

- 和提交一样，撤销变更由底层部分（暂存区的独立文件或片段）和上层部分（变更到底是通过哪种方式被撤销的）组成。
- 主要有两种方式撤销变更
  - `git reset HEAD`
  - `git revert HEAD` 
- `git reset`通过把分支记录回退几个提交记录来实现撤销改动，`git reset` 向上移动分支，原来指向的提交记录就跟从来没有提交过一样～
- 虽然`git reset`使用很方便，但是这种“改写历史”的方法对大家一起使用的远程分支时无效的
  为了撤销更改并**分享**给其他人，我们需要使用`git revert`
- `git revert`通过一个新的提交记录，来撤销之前的操作，并且这个提交记录和之前未改变的提交记录的代码是一致的，通过这个就可以把你的更改推送到远程仓库与别人分享～



### 移动提交记录

#### 1.git cherry-pick

- `git cherry-pick <提交号>...`
- 作用：将一些提交复制到当前所在的位置（HEAD）下面的话，cherry-pick时最直接的方式
- 可以对比`git rebase`使用

#### 2.交互式rebase

- 当你知道所需要的提交号（并且知道这些提交记录的hash值时）用`cherry-pick`再好不过了
  但是如果不清楚提交号，就需要利用交互式的rebase
- 交互式rebase指的是使用带参数`--interactive`的rebase命令，简写为`-i`。
  如果你在命令后增加了这一个选项，Git会在文本编辑器（vim）打开一个文件
  - 列出将要被复制到目标分支的备选提交记录
  - 显示每个记录提交的哈希值和提交说明

- 通过`git rebase -i HEAD~[num]`（包含HEAD计算在内）可以在不知道提交号或者分支名或者哈希值的时候，将一些提交复制到HEAD的下面
- `git rebase -i HEAD~`与`cherry-pick`的区别：
  - `cherry-pick`复制时节点在上面(为需要提交节点的父节点)
  - `git rebase -i HEAD~`复制时，复制的节点为当前分支节点和其父节点，然后往上+1的一个父级节点为分叉点，去复制新的节点～

### 杂项

#### 1.只有一个提交记录



#### 2.提交的技巧1



#### 3.提交的技巧2




#### 4.git tag



#### 5.Git Describe



### 高级话题

#### 1.多次rebase



#### 2.两个父节点



#### 3.纠缠不清的分支



### Push&Pull —— Git远程仓库

#### 1.git clone



#### 2.远程分支



#### 3.git fetch



#### 4.git pull



#### 5.模拟团队合作



#### 6.git push



#### 7.偏离的提交历史



### 关于origin和他的周边 —— Git远程仓库高级操作

#### 1.推送主分支



#### 2.合并远程仓库



#### 3.远程追踪



#### 4.git push的参数



#### 5.git push参数2



#### 6.git fetch的参数



#### 7.没有source的source



#### 8.git pull的参数