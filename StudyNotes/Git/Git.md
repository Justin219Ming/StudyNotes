# Git

### Git基本设置

配置用户名：git config --global user.name "Your Name"

配置用户邮箱：git config --global user.email "your.email@example.com"



### 创建版本库

先电脑上创建一个空目录

`pwd`可以查看当前目录

使用`git init`命令可以使当前目录变成Git可以管理的仓库



### 在本地创建一个编程项目，将所有的项目源程序文件提交到本地库中：

创建一个新的文件夹作为项目目录：mkdir project_name

进入项目目录：cd project_name

创建一个新的源程序文件：touch main.cpp

将文件添加到暂存区：git add main.cpp

提交文件到本地库：git commit -m "Initial commit"

Git是先把文件放在工作区 add 命令推送到暂存区

再通过提交命令commit 推送到本地库





### 多次修改某一个源代码文件，进行多次提交：

修改源代码文件

将文件添加到暂存区：git add main.cpp

提交文件到本地库：git commit -m "Commit message"

这里的"Commit message"是你自己提交本次修改的梗概



### 查看文件的历史记录，并且查看不同版本之间的差别：

查看文件的历史记录：`git log main.cpp`

查看不同版本之间的差别：git diff commit_id1 commit_id2 main.cpp



### 将文件恢复到前面的某个版本，给出详细的恢复过程：

看文件的历史记录：git log main.cpp

找到要恢复的版本的commit id

查看我所执行的每一次命令: `git reflog`

恢复文件到指定版本：git checkout commit_id main.cpp

回退上一个版本：` git reset --hard HEAD^`

回退指定版本： `git reset --hard commit_id`



### 删除某一个提交文件，并且查看项目状态：

- 删除文件：git rm main.cpp
- 提交删除操作：git commit -m "Delete main.cpp"
- 查看项目状态：`git status`
- 如果删错了文件 可以通过

`git checkout -- test.txt`

来恢复文件 但是要在你提交之前执行这个命令



## 远程仓库

### github

第1步：创建SSH Key。

在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有`id_rsa`和`id_rsa.pub`这两个文件，如果已经有了，可直接跳到下一步。

如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：

```
$ ssh-keygen -t rsa -C "youremail@example.com"
```

你需要把邮件地址换成你自己的邮件地址，然后一路回车，使用默认值即可，由于这个Key也不是用于军事目的，所以也无需设置密码。

如果一切顺利的话，可以在用户主目录里找到`.ssh`目录，里面有`id_rsa`和`id_rsa.pub`两个文件，这两个就是SSH Key的秘钥对，`id_rsa`是私钥，不能泄露出去，`id_rsa.pub`是公钥，可以放心地告诉任何人。

第2步：登陆GitHub，打开“Account settings”，“SSH Keys”页面：

然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴`id_rsa.pub`文件的内容：

为什么GitHub需要SSH Key呢？因为GitHub需要识别出你推送的提交确实是你推送的，而不是别人冒充的，而Git支持SSH协议，所以，GitHub只要知道了你的公钥，就可以确认只有你自己才能推送。（信息安全密码学）

在本地的`learngit`仓库下运行命令，从而完成链接，把里面的`michaelliao`替换成自己的GitHub账户名

```
$ git remote add origin git@github.com:michaelliao/learngit.git
```

添加后，远程库的名字就是`origin`，这是Git默认的叫法，也可以改成别的，但是`origin`这个名字一看就知道是远程库。

### 推送到远程库

下一步，就可以把本地库的所有内容推送到远程库上：

```
$ git push -u origin master
```

把本地库的内容推送到远程，用`git push`命令，实际上是把当前分支`master`推送到远程。

由于远程库是空的，我们第一次推送`master`分支时，加上了`-u`参数，Git不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令。

从现在起，只要本地作了提交，就可以通过命令：

```
$ git push origin master
```

把本地`master`分支的最新修改推送至GitHub，现在，你就拥有了真正的分布式版本库！

### SSH警告

当你第一次使用Git的`clone`或者`push`命令连接GitHub时，会得到一个警告：

```
The authenticity of host 'github.com (xx.xx.xx.xx)' can't be established.
RSA key fingerprint is xx.xx.xx.xx.xx.
Are you sure you want to continue connecting (yes/no)?
```

这是因为Git使用SSH连接，而SSH连接在第一次验证GitHub服务器的Key时，需要你确认GitHub的Key的指纹信息是否真的来自GitHub的服务器，输入`yes`回车即可。

Git会输出一个警告，告诉你已经把GitHub的Key添加到本机的一个信任列表里了：

```
Warning: Permanently added 'github.com' (RSA) to the list of known hosts.
```

这个警告只会出现一次，后面的操作就不会有任何警告了。

如果你实在担心有人冒充GitHub服务器，输入`yes`前可以对照[GitHub的RSA Key的指纹信息](https://help.github.com/articles/what-are-github-s-ssh-key-fingerprints/)是否与SSH连接给出的一致。

### 删除远程库

如果添加的时候地址写错了，或者就是想删除远程库，可以用`git remote rm <name>`命令。使用前，建议先用`git remote -v`查看远程库信息：

```
$ git remote -v
origin  git@github.com:michaelliao/learn-git.git (fetch)
origin  git@github.com:michaelliao/learn-git.git (push)
```

然后，根据名字删除，比如删除`origin`：

```
$ git remote rm origin
```

此处的“删除”其实是解除了本地和远程的绑定关系，并不是物理上删除了远程库。远程库本身并没有任何改动。要真正删除远程库，需要登录到GitHub，在后台页面找到删除按钮再删除。

### 小结

要关联一个远程库，使用命令

`git remote add origin git@server-name:path/repo-name.git`；

关联一个远程库时必须给远程库指定一个名字，`origin`是默认习惯命名；

关联后，使用命令`git push -u origin master`第一次推送master分支的所有内容；

此后，每次本地提交后，只要有必要，就可以使用命令`git push origin master`推送最新修改；

### 从远程库克隆

在关联好远程库后

可以通过

> git clone git@github.com:your_name/your_repo_name.git

来进行克隆操作

Git支持多种协议，https，ssh等 但是ssh快

## 分支管理

