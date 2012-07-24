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

### 提交文件到Git仓库

### 跳过暂存文件直接提交

### 从Git仓库中删除文件

### 移动文件
