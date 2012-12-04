# 我能后悔吗？
到现在为止，我们学会了让Git完完全全的管理我们的代码以及其他文件了。但问题是，怎样才能够让代码还原到上个星期的版本呢？很有可能上个星期的项目还能够编译成功，或者还能够正常运行……  
没人愿意遇到这样的情况，但如果你足够倒霉的话，你就只能靠上帝，噢不，是靠Git来帮你解决问题了！

## 提交之前的撤销操作
在我们将代码提交到Git仓库之前，你可以使用`git checkout`将文件恢复到最进一次的版本状态。还是让我们用例子说话吧。  
接着上一个例子的Git仓库，`git status`先:
```bash
$ git status
# On branch master
nothing to commit (working directory clean)

$ git ls-files
test.new.rb
try.txt
```

现在版本是clean状态，有两个文件在Git的控制的文件，`test.new.rb`和`try.txt`。  
现在假定我们已经把一推代码混杂在`try.txt`和`test.new.rb`之中了:
```bash
$ echo new-lint >> try.txt
$ echo "100.times.do{ |i| p i }" >> test.new.rb
$ git status
# On branch master
# Changes not staged for commit:
#   (use "git add <file>..." to update what will be committed)
#   (use "git checkout -- <file>..." to discard changes in working directory)
#
#       modified:   test.new.rb
#       modified:   try.txt
#
no changes added to commit (use "git add" and/or "git commit -a")
```

但是PM突然过来，说，“小X”，刚才的需求不用做了。 *WTF* !!  
还好，我们使用的Git，在你执行`git commit -a`或者`git add try.txt`之前，我们可以瞬间恢复到修改过的版本:
```bash
$ git checkout -- try.txt # 恢复指定文件到修改之前
$ git checkout -- .       # 恢复所有文件到修改之前
```

`git checkout --`表示从当前的索引中检索出文件，如果文件没有加入缓存区，就可以用这种方法快速的检出最近提交的版本。  
如果不幸的是现在已经将文件加入到暂存区了，这时使用`git checkout --`是没有用的，因为暂存区里记录了你添加时候的文件快照，这时你需要用到`git reset` 命令:
```bash
$ echo "new-line" >> try.txt
$ git add try.txt
$ git status
# On branch master
# Changes to be committed:
#   (use "git reset HEAD <file>..." to unstage)
#
#       modified:   try.txt
#
$ git checkout -- try.txt # 不会改变Git暂存区里的快照
$ git status
# On branch master
# Changes to be committed:
#   (use "git reset HEAD <file>..." to unstage)
#
#       modified:   try.txt
#
$ git reset -- try.txt # 将位于暂存区的try.txt移除
$ git status
# On branch master
# Changes not staged for commit:
#   (use "git add <file>..." to update what will be committed)
#   (use "git checkout -- <file>..." to discard changes in working directory)
#
#       modified:   try.txt
#
no changes added to commit (use "git add" and/or "git commit -a")
$ git checkout -- try.txt # 将try.txt从最近的索引复原
# On branch master
nothing to commit (working directory clean)
```

看到了么？用Git，后悔成本如此之低！相信现在大家已经可以轻松地把提交之前对代码的改动复原到原来的状态了。

## 提交之后的撤销操作
当我们已经把代码提交到Git仓库之后，想恢复到之前的版本，这改如何是好？下面我们就一起详细的看看`git reset`命令吧。它将让你在Git代码仓库中为所欲为！

相信在之前一节中，大家已经接触到一个`git reset --`的命令了——从暂存区将暂存的文件重置为没有暂存的状态。其实我们可以这样理解：`git checkout`是将索引中的一个指定节点检出到工作目录（工作树）中，索引历史没有任何变化。而`git reset`是会直接改变索引（也就是提交的历史）的。

让我们接着上一节的工作目录继续研究`git reset`命令。

首先查看Git提交历史（也就是查看索引中的节点）:
```bash
$ git log
commit 0fc6e17fb925edb73f61cf649df81d0624b4a47d
Author: Zhuang Sirui <siriusibunny@gmail.com>
Date:   Thu Nov 29 16:20:08 2012 +0800

    save

commit e7b480e4abd570b76641900de235b0fb666a9337
Author: Zhuang Sirui <siriusibunny@gmail.com>
Date:   Thu Nov 29 16:19:51 2012 +0800

    save

commit efcb9c7db32b41964592954aa5e232552fd9768a
Author: Zhuang Sirui <siriusibunny@gmail.com>
Date:   Wed Nov 28 17:40:48 2012 +0800

    qwe

commit 1c8740634b8be5c399c282f641ff2248c5cdf6a9
Author: Zhuang Sirui <siriusibunny@gmail.com>
Date:   Wed Nov 28 14:13:43 2012 +0800

    save
```

我们看到当前的仓库中，保存了4次提交记录，假设我们最近一次提交是无效的，想退回到上一个提交版本:
```bash
$ git reset --hard e7b480e4abd570b76641900de235b0fb666a9337
HEAD is now at e7b480e save
$ git log
commit e7b480e4abd570b76641900de235b0fb666a9337
Author: Zhuang Sirui <siriusibunny@gmail.com>
Date:   Thu Nov 29 16:19:51 2012 +0800

    save

commit efcb9c7db32b41964592954aa5e232552fd9768a
Author: Zhuang Sirui <siriusibunny@gmail.com>
Date:   Wed Nov 28 17:40:48 2012 +0800

    qwe

commit 1c8740634b8be5c399c282f641ff2248c5cdf6a9
Author: Zhuang Sirui <siriusibunny@gmail.com>
Date:   Wed Nov 28 14:13:43 2012 +0800

    save
```

发生了什么？之前的提交完全不见了，甚至一点记录都没有，就这样消失了。`git reset --hard`会同时将索引中和工作树的版本恢复到你想要的那一次提交的状态！

可以看出，`git reset --hard`的破坏性是相当强大的，在不谨慎的情况下，极容易出现完全丢失代码的情况。当然Linus Torvalds会让我们放心的使用`git reset`的，不是吗？于是，就有了`git reset --mixed`以及`git reset --soft`。

好的，经过刚才的`git reset --hard`，还让人心有余悸。下面让我们试试没那么“硬”的命令:
```bash
$ git reset --mixed efcb9c7db32b41964592954aa5e232552fd9768a
$ git status
# On branch master
# Untracked files:
#   (use "git add <file>..." to include in what will be committed)
#
#       qwe.qwe
nothing added to commit but untracked files present (use "git add" to track)
$ git log
commit efcb9c7db32b41964592954aa5e232552fd9768a
Author: Zhuang Sirui <siriusibunny@gmail.com>
Date:   Wed Nov 28 17:40:48 2012 +0800

    qwe

commit 1c8740634b8be5c399c282f641ff2248c5cdf6a9
Author: Zhuang Sirui <siriusibunny@gmail.com>
Date:   Wed Nov 28 14:13:43 2012 +0800

    save
```

看到了什么，在索引（index）中的提交已经恢复到我们指定的版本号了，但是，工作树中的文件却还是我们回滚代码之前的样子，而且没有被放入暂存区中，也就是说：*Git让时间退回到了上一个版本改动之后，添加到暂存区之前的那一刻*！

这样的话，我们就不会怕我们的改动突然消失了吧。在这种情况下，我们还可以选择性的用上一节提交之前的撤销操作（`git checkout -- path/to/file）撤销某一写文件的的改动，然后再重新提交到Git仓库（index）中去。这样，我们终于可以在时间的长河里为所欲为了！

等等，不是还有一个`git reset --soft`么？当你理解了`git reset --mixed`之后，在理解它就不是事儿了，因为，`--soft`和`--mixed`的区别仅仅是，`--soft`会将回滚后，默认将改变的代码直接放入暂存区而已。;)
