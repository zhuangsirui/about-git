# 使用远程仓库

## 远程仓库的概念
在最开始的时候就提到过，Git是一个分布式的版本控制软件。这就是为什么一直还没有向大家介绍远程仓库的概念，但是大家已经可以随心所欲的控制自己的项目代码了。但往往在开发项目的时候，我们并不孤独，所以可能会有几个人协同开发一个项目，这样的话，就需要想svn一样建立一个公共的远程仓库，大家都用它来交换代码，控制总版本。下面就为大家介绍Git远程仓库的使用:

## 查看远程仓库
想要看我们项目中添加了哪些远程仓库，可以用`git remote`命令，加上参数`-v`可以查看到各个仓库的地址以及权限。

为了先讲解查看远程仓库，我们需要先克隆一个开源的项目，大家可以随意选一个自己喜欢的开源项目，在这里，我用本教程的源码地址。还是老样子——实战开始:
```bash
$ git clone git://github.com/siriuszhuang/about-git.git
$ cd about-git
$ g remote -v
origin  git://github.com/siriuszhuang/about-git.git (fetch)
origin  git://github.com/siriuszhuang/about-git.git (push)
```

可以看到我们刚才克隆的仓库中，已经有一个`origin`仓库了。默认的，`origin`仓库就是该项目原始仓库，同时你也可以添加一些其他的仓库。

在取得仓库之后，我们本地也会有一个远端仓库的分支被加进来了，但是和其他的分支有所不同，我们可以用`git branch -a`查看:
```bash
$ git branch -a
* master                               # 当前所在分支
  remotes/origin/HEAD -> origin/master # 远端仓库分支
  remotes/origin/develop               # 远端仓库分支
  remotes/origin/master                # 远端仓库分支
```

可以看到远端仓库的分支都是以`remote/`开头的，后面分别是仓库名称以及分支名称。怎样才能理解远程分支呢？其实我们只要将它看作是一个本地分支就行了，只不过当我们执行一些命令`fetch`,`pull`,`push`等等之后，这些（远程）分支会与远端的分支同步而已，并且我们不能在这些分支的索引中做变动而已。

总而言之，对于远端分支，我们一样可以对其执行`checkout`，查看其索引中的文件，查看提交历史等等操作。

## 从仓库
