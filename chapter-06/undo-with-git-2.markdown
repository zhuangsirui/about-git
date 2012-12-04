# 提交之后的撤销操作
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
