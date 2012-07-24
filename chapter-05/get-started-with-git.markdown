# 开始使用Git

## 得到一个Git仓库

### 新建
就像之前说的一样，你可以通过`git init`自己初始化一个空白的Git仓库。

### 克隆
如果你想从现有的Git仓库克隆一份，你可以使用`git clone`命令：
```bash
git clone user@server-name:path/to/.git
git clone http://server-name/path/to/git
git clone git://server-name/path/to/git
```

上面三条命令的差别仅仅在于使用的传输协议的不同。它们分别是ssh\http\git三种。现在你还不需要在意它们之间的区别，我们以后会慢慢说明。

## 开始让Git管理你的文件
通过第三章的尝试，我们对Git有了初步的认识，但这还远远不够。接下来我们会尝试Git的常用命令。通过这一章的了解，我们就可以真正地在Git上开展我们的工作了。

### 文件状态的变化
如果你还记得上一张提到的Git中文件的四种状态的话，就太好了。但如果没记住也没关系，我将它拷贝了一份。在阅读下面几小节的时候，你可以回顾一下这幅图，希望这可以帮助你更好的理解Git！

	                      +----------- checkout -------->+
	                      |                              |
	                      +<---- reset ---+              |
	                      |               |              |
	+-----------+    +----------+    +--------+    +-----------+
	| untracked |    | modified |    | staged |    | committed |
	+-----------+    +----------+    +--------+    +-----------+
	      |               |               |              |
	      |               +----- add ---->+              |
	      |                               |              |
	      +------------- add ------------>+--- commit -->+

### 添加文件让Git得以跟踪它
Git从来不会做你不让他做的事情。所以，如果你想让Git开始跟踪一个文件的时候，你需要显式的告诉它：
```bash
$ git add path/to/filename # 添加单个文件
$ git add dirname/.        # 添加一个目录下面的所有文件
$ git add .                # 添加改工作树的所有文件
```

这似乎很好理解，值得注意的是，在`git add`之后，那些被你添加的文件会直接到达Git的暂存区（staged）内。这样当你在`git commit`的时候会通通提交到Git仓库中去。所以，在使用`git add .`的时候，应该特别注意。因为很有可能这条命令会把各种意想不到的文件加入到Git仓库中，就像是vim的交换文件、编译好的pyc、甚至是rails需要用到的gem包。所以我建议每次添加新文件的时候最好的办法是一个目录一个目录地添加。如果确实需要添加全部文件，确保你的*.gitignore*文件（在这一章的后面会讲到）的全面以及健壮。

### 暂存已经修改的文件
如果你的已文件已经提交，但在提交之后你又做了一些修改，这时运行`git commit`同样不会将他们提交到仓库中去。让我们进入上次我们初始化的Git仓库*learn-git*目录中亲自试一试。

进入目录之后，我们尝试修改了*test.rb*和*try.txt*文件并保存。之后运行`git status`看看现在工作树的状态。
```bash
$ git status
# On branch master
# Changes not staged for commit:
#   (use "git add <file>..." to update what will be committed)
#   (use "git checkout -- <file>..." to discard changes in working directory)
#
#	modified:   test.rb
#	modified:   try.txt
#
no changes added to commit (use "git add" and/or "git commit -a")
```

我们可以清楚地看到，这两个文件所在的状态为*Changes not staged for commit*，也就是修改过但是没有在暂存区待提交。这时我们运行`git commit`是不会把这两个文件提交到Git仓库中去的（不信可以试一试）。如果需要让这两个文件在`git commit`的时候顺利被提交，我们可以用`git add`命令将他们添加到暂存区（对，没错，是`git add`）。
```bash
$ git add test.rb try.txt
$ git status
# On branch master
# Changes to be committed:
#   (use "git reset HEAD <file>..." to unstage)
#
#	modified:   test.rb
#	modified:   try.txt
#
```

现在，我们已经成功地将这两个文件提交到暂存区了。接下来我们将会把他们提交到Git的仓库中。但是先等等，在提交之前，我还想和大家分享一个忽略文件的方法。

### 使用.gitignore忽略文件
稍微有经验的开发者一定会注意到，如果在真正开发一个项目的时候，情况一定不像上面的例子那么简单。在我们的项目目录下可能到处都是一些对项目源码本身没有意义、自动生成、自动编译等等的文件。这些我们可不想让Git在意他们，但是项目目录下充斥的大量目录、文件让我们没办法一个一个的添加他们，让Git能够顺利的跟踪这些文件。这个时候，我们就会用到Git忽略文件*.gitignore*。

使用这个文件很简单，只需要在工作树的根目录下新建一个*.gitignore*文件，在其中添加规则即可。一下是一些规则的例子，我们可以举一反三。
```bash
$ cat .gitignore
# 此为注释 – 将被 Git 忽略
*.[oa]      # 忽略以 .o 或者 .a 结尾的文件
*.swp       # 忽略所有以 .swp 结尾的文件（vim的交换文件）
.*          # 忽略所有以 . 开头的文件（隐藏文件）
!.gitignore # 但此文件本身 .gitignore 除外
/TODO       # 仅仅忽略项目根目录下的 TODO 文件，不包括 doc/TODO
log/        # 忽略所有 log/ 目录下的所有文件
```

### 提交文件到Git仓库
到这里，我们已经有两个文件在暂存区了，如果你能在进一步的话，可能已经有了一个合适自己的*.gitignore*文件。现在我们需要将他们真正的提交到Git仓库中。
```bash
$ git commit # 之后Git会使用一个编辑器让你输入一些对这次提交的一些说明。你也可以使用 git commit -m 'Fix bugs'将说明直接添加在这条命令的后面。
[master 5a083d3] Fix bugs
 2 files changed, 2 insertions(+)
```

现在，我们已经将文件安全的保存在了Git仓库中。让我们来看看之前做过的一切。
```bash
$ git status
# On branch master
nothing to commit (working directory clean)
$ git log
commit 5a083d36f8c64f671079b1613e9e00f13ab0f7e5
Author: sirius zhuang <siriusibunny@gmail.com>
Date:   Tue Jul 24 14:15:14 2012 +0800

    Fix bugs

commit 73c519b0f86155904957482db266fb4da25835e5
Author: sirius zhuang <siriusibunny@gmail.com>
Date:   Thu Jul 19 22:10:03 2012 +0800

    My first commit
```

### 跳过暂存文件直接提交
当我们修改了比较多的文件，而又添加了一些不想提交的新文件的时候。提交却成为了麻烦的事情，我们不能使用`git add .`将所有文件添加到暂存区，也不想把几十个文件一个一个的`git add`。  
Git为这种情况提供了一个有好的方法——`git commit -a`。
```bash
$ echo a >> try.txt
$ echo b >> test.rb
$ git status
# On branch master
# Changes not staged for commit:
#   (use "git add <file>..." to update what will be committed)
#   (use "git checkout -- <file>..." to discard changes in working directory)
#
#	modified:   test.rb
#	modified:   try.txt
#
no changes added to commit (use "git add" and/or "git commit -a")
$ git commit -a -m 'Fast commit'
[master 612b4d0] Fash commit
 2 files changed, 2 insertions(+)
```

事实上，我的每次提交都会用到`-a`，`git add`只是在我确认需要添加新的文件是才会用。这样可以最大限度的防止我们在毫无准备的情况下添加了一些莫名其妙的文件。

### 从Git仓库中删除文件

### 移动文件
