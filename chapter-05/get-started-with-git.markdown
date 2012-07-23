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

## 开始
