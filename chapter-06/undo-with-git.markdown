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

看到了么？用Git，后悔成本如此之低！

## 提交之后的撤销操作
