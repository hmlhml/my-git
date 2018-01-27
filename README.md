[参考资料](http://blog.jobbole.com/102957/)
## Git简介
Git是目前世界上最先进的分布式版本控制系统（没有之一）。
git命令是一些命令行工具的集合，它可以用来跟踪，记录文件的变动。比如你可以进行保存，比对，分析，合并等等。这个过程被称之为版本控制。已经有一系列的版本控制系统，比如SVN, Mercurial, Perforce, CVS, Bitkeepe等等。

Git是分布式的，这意味着它并不依赖于中心服务器，任何一台机器都可以有一个本地版本的控制系统，我们称之为仓库。如果是多人协作的话，你需要还需要一个线上仓库，用来同步信息。这就是GitHub, BitBucket的工作。

## 基本了解
### 1.安装Git
* [下载Git](https://git-scm.com/downloads)
* 安装

### 2.配置Git
安装完git,首要任务是配置我们的信息，最重要的是用户名及邮箱，打开终端，执行以下命令。

	$ git config --global user.name "My Name"
	$ git config --global user.email myEmail@example.com

配置好这两项，用户就能知道谁做了什么，并且一切都更有组织性了

### 3.创建一个新仓库 – git init
git 会把所有文件以及历史记录保存在你的项目中，创建一个新的仓库，首先要去到项目路径，执行 git init。然后git会创建一个隐藏的文件夹.git，所有的信息都储存在其中。

1.选择一个合适的地方，创建一个空目录： 

	$ mkdir gitDemo
	$ cd gitDemo

2.通过`git init`命令把这个目录变成Git可以管理的仓库：

	$ git init
当前目录下多了一个.git的目录，这个目录是Git来跟踪管理版本库的，没事千万不要手动修改这个目录里面的文件，不然改乱了，就把Git仓库给破坏了。

### 4.检查状态 – git status
命令可以让我们时刻掌握仓库当前的状态   
4.1.在空目录下 `git status`
	
	$ git status
	On branch master
	Initial commit
	nothing to commit (create/copy files and use "git add" to track)

4.2在目录下添加README.md，执行 `git status`

	$ git status
	On branch master
	Initial commit
	Untracked files:
	  (use "git add <file>..." to include in what will be committed)
	        README.md
	nothing added to commit but untracked files present (use "git add" to track)

git 告诉我们，README.md尚未跟踪，这是因为这个文件是新的，git不知道是应该跟踪它的变动呢，还是直接忽略不管呢。为了跟踪我们的新文件，我们需要暂存它。

### 5.暂存 – git add
git 有个概念叫 暂存区，你可以把它看成一块空白帆布，包裹着所有你可能会提交的变动。它一开始为空，你可以通过 git add 命令添加内容，并使用 git commit 提交。
	
	$ git add README.md

如果需要提交目录下的所有内容，可以这样：

	$ git add -A

再次使用git status查看：

	$ git status
	On branch master
	Initial commit
	Changes to be committed:
	  (use "git rm --cached <file>..." to unstage)
	        new file:   README.md

我们的文件已经提交了。状态信息还会告诉我们暂存区文件发生了什么变动，不过这里我们提交的是一个全新文件。

### 6.提交 – git commit
一次提交代表着我们的仓库到了一个交付状态，通常是完成了某一块小功能。它就像是一个快照，允许我们像使用时光机一样回到旧时光。

创建提交，需要我们提交东西到暂存区（git add），然后：
	
	$ git commit -m "Initial commit."

这就创建了一次提交，`-m “Initial commit.”`表示对这次提交的描述，建议使用有意义的描述性信息。

### 7.日志记录 – git log
在目录下增加`hello.txt`   

	 $ git status 
	 $ git add hello.txt
	 $ git commit -m " add hello txt."
查看历史记录

	$ git log
	commit c4d9814ed970251b420a30dfd16e3bbc2338d822
	Author: hml <405185142@qq.com>
	Date:   Thu Jan 18 21:26:42 2018 +0800
	
	    add hello txt
	
	commit 8b98282f5002eb80e29bf4e88b3e7591907bd41a
	Author: hml <405185142@qq.com>
	Date:   Thu Jan 18 21:00:28 2018 +0800
	
	    Initial commit.
`git log`命令显示从最近到最远的提交日志，我们可以看到2次提交  
如果嫌输出信息太多，看得眼花缭乱的，可以试试加上`--pretty=oneline`参数：

	$ git log --pretty=oneline
	c4d9814ed970251b420a30dfd16e3bbc2338d822 add hello txt
	8b98282f5002eb80e29bf4e88b3e7591907bd41a Initial commit.

看到的一大串类似3628164...882e1e0的是commit id（版本号），

查看`hello.txt`的内容

	$ cat hello.txt
	hello
	world


### 8.版本回退 - git reset
首先，Git必须知道当前版本是哪个版本，在Git中，

* 用`HEAD`表示当前版本，
* 上一个版本就是`HEAD^`，
* 上上一个版本就是`HEAD^^`，
* 当然往上100个版本写100个^比较容易数不过来，所以写成`HEAD~100`。

现在，我们要把当前版本`"add hello txt"`回退到上一个版本`"Initial commit"`，就可以使用`git reset`命令：

	$ git reset --hard HEAD^
	HEAD is now at 8b98282 Initial commit.

**找回被回退的版本**：只要上面的命令行窗口还没有被关掉，找到那个`"add hello txt"`的`commit id`是`c4d9814...`，就可以指定回到未来的某个版本

	$  git reset --hard c4d9814
	HEAD is now at c4d9814 add hello txt

版本号没必要写全，前几位就可以了，Git会自动去找。当然也不能只写前一两位

## 远端仓库
到目前为止，我们的操作都是在本地的，它存在于.git文件中。为了能够协同开发，我们需要把代码发布到远端仓库上。

### 1.链接远端仓库 – git remote add
*  创建远端仓库 
`my-git`  
* 链接远端仓库 
`git remote add origin https://github.com/hmlhml/my-git.git`

一个项目可以同时拥有好几个远端仓库为了能够区分，通常会起不同的名字。通常主远端仓库被称为origin。


# git vim 编辑器基本操作
用 git 命令行提交文件时，默认使用 vim 编辑器，基本操作：

* 按 a, i 或 o 进入编辑模式
* 按 ESC 进入操作模式
* 在操作模式下，:wq 为写入退出，:q! 不保存退出   
