# Git的安装
## Ubuntu
Ubuntu的apt-get早已经包含git的deb文件，你可以运行`sudo apt-get install git`直接安装默认配置，或者运行`apt-cache search git`来查看关于Git的所有程序选择安装。

## Mac
Mac下安装Git就没有那么方便了，但是你可以任意从一下两种安装方式中选择自己喜欢的安装方法。
* 使用图形化的安装方法：在Google code上，有Mac下Git的dmg安装包，[地址在这里](http://code.google.com/p/git-osx-installer/)。
* 使用Port安装git：首先你需要安装[Macports](http://www.macports.org/)，这是一个Mac下类似apt-get或yum的软件中心，之后，直接运行`sudo port install git-core`安装Git的核心文件。
* 使用brew安装git：或者使用[Homebrew](https://github.com/mxcl/homebrew)，安装Git。`brew install git`。这也是我在Mac下安装Git的方法。

## Windows
Windows下安装比较麻烦，基本上我没有尝试过，但是可以下载Github的Windows客户端来实现图形界面以及类Shell界面对Git的一些控制。具体可以[参见此页](https://help.github.com/articles/set-up-git#platform-windows)。 由于本人没有在Windows的经验，所以一下的教学都是在`*unix`上进行的。有Windows下了解Git的人可以push我，谢谢:)
