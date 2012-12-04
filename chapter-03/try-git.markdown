# Git初体验

## 初始化Git仓库

为了更好的学习Git，有必要亲自试一试Git的操作。现在让我们新建一个空的目录来使用Git控制一些东西。  
首先新建一个空白的目录，名字可以取自己喜欢的，我在这里取名叫 *learn-git* 。创建成功之后进入它。
```bash
$ mkdir ~/learn-git && cd ~/learn-git
```

进入目录之后，我们首先需要告诉Git，这里归你管了，俗称初始化Git仓库。命令很简单：
```bash
$ git init
```

在这之后，现在所在的目录就是一个Git的工作树了。

	Git在`git init`之后做了什么？它在当前的目录下生成了一个.git的目录，Git所需要的东西都放在里面。

## 提交之前的一些准备
如果你是第一次使用Git，那么会被要求填入你名称以及电子邮箱地址。Git会禁止掉所有的匿名提交。所以首先向Git自我介绍一番：
```bash
$ git config --global user.name 'your username'
$ git config --global user.email 'name@example.com'
```

## 首次提交
在初始化完成之后，我们就可以开始编写我们的项目了。这里我们模拟创建两个文件，一个是 *test.rb* ，一个是 *try.txt* 。
```bash
touch test.rb try.txt
```

接着我们来看看Git现在是如何看待这两个新文件的。
```bash
$ git status
# On branch master
#
# Initial commit
#
# Untracked files:
#   (use "git add <file>..." to include in what will be committed)
#
#	test.rb
#	try.txt
nothing added to commit but untracked files present (use "git add" to track)
```

我们看到，他将这两个文件归类在 *Untracked files* 中。意思就是Git没有跟踪这两个文件的任何更改行为，包括删除。如果我们想让Git跟踪这个文件，我们就需要运行`add`命令，将文件添加到Git的跟踪清单中。现在我们先将 *test.rb* 添加到Git的控制之中。
```bash
$ git add test.rb
```
接下来我们在此运行`git status`来查看当前状态：
```bash
$ git status
# On branch master
#
# Initial commit
#
# Changes to be committed:
#   (use "git rm --cached <file>..." to unstage)
#
#	new file:   test.rb
#
# Untracked files:
#   (use "git add <file>..." to include in what will be committed)
#
#	try.txt
```

现在可以看出，我们刚刚添加的 *test.rb* 已经在Git暂存区了，但是 *try.txt* 任然在未跟踪列表中。我们当然可以接着运行`git add try.txt`来将它添加到暂存区，但是如果文件过多的话，我们一定不希望把文件挨个添加到git控制列表中吧。所以当文件过多，可以直接使用`git add .`来添加所有文件到Git的跟踪清单里。如果有不想添加的文件，可以写一个`.gitignore`文件到根目录，具体的语法格式会放在后面的章节讲解。
```bash
$ git add .
$ git status
# On branch master
#
# Initial commit
#
# Changes to be committed:
#   (use "git rm --cached <file>..." to unstage)
#
#	new file:   test.rb
#	new file:   try.txt
#
```

现在，我们想让Git控制的文件都放到暂存区了。之后我们输入`git commit`命令就可以将暂存区的文件正式提交到Git仓库中，并且生成一个快照。
```bash
$ git commit
[master (root-commit) 73c519b] My first commit
 0 files changed
 create mode 100644 test.rb
 create mode 100644 try.txt
```

这时，git会要求你输入一些必要的提交信息。Git和SVN不一样的是Git禁止不输入任何信息的提交。在这里你可以输入任何信息比如`My first git commit`。但是如果是真正的提交，还是建议大家输入一些有用的信息以便之后的Log查看甚至回滚。

## 查看提交Log
当你在提交之后，刚才在暂存区的文件已经不复存在了，确切的说他们已经不在暂存区了，而是被Git记录了下来。这时运行`git status`应该看到这样的显示：
```bash
$ git status
# On branch master
nothing to commit (working directory clean)
```

Git会告诉你，当前的工作目录的完全干净的。这意味着：

1. 没有任何多余（未被跟踪）的文件
2. 没有被修改（已经被跟踪但是还未放在暂存区）的文件
3. 没有准备提交（修改并且已经放入暂存区）的文件
4. 只有被Git完全控制在仓库内的文件

	请记住这四种状态，这四种状态是Git中仅有的四种文件的状态。在以后的章节中会说明他们之间的区别。

现在让我们查看Git的提交历史
```bash
$ git log
commit 73c519b0f86155904957482db266fb4da25835e5
Author: sirius zhuang <siriusibunny@gmail.com>
Date:   Thu Jul 19 22:10:03 2012 +0800

    My first commit
```

你会看到你刚才提交的一个快照信息，例如时间、你的一些个人信息以及提交的说明。  
到这里为止，你可能对Git有了一个基本的认识了。接下来，我们会再深入的说明Git中的一些概念。

## 复习
回顾刚才的一些操作，因为这是最基本的Git操作。接下来，我们会接触到一些Git的基本概念。

1. 使用`git init`来初始化Git仓库。
2. 使用`git add`将需要Git管理的文件添加到管理清单并且添加到暂存区。
3. 使用`git commit`将放在暂存区的文件正式的提交到Git仓库中，并且生成一个快照。
4. 使用`git log`来查看历史提交。
