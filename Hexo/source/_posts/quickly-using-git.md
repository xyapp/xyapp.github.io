title: Quickly Using Git and Hexo
date: 2014-05-11 20:48:35
author: XY
categories: Tools
tags: [git, hexo]
---

###An Introduction of Using Git and Hexo to Build My Blog

这篇文章是我自己的操作过程的详细介绍说明，在这里列出详细步骤，以作备份供随时查看。

本篇文章只是一个简单的入门操作，更深层次的高级用法请查看其他相关的教程，在[这里][links]有给出相关链接。

[links]: http://xyapp.github.io/notice/

<!--more-->

###一、首先下载和安装git。具体操作可以参看[Zippera's Blog][Zippera]，其中有详细的介绍。

[Zippera]: http://zipperary.com/2013/05/28/hexo-guide-2/

###二、在[github主页][github]注册一个账户，并建立自己相应的版本库。

[github]: https://github.com/

###三、把远程版本库同步到本地并发表博客的基本流程

## 1、先把远程版本库克隆到本地

- 先在本地一个工作文件夹（空文件夹，如E/MyBlog）下右键选择git bush。
```
git clone git@github.com:xyapp/xyapp.github.io
# 注释：上述命令是将PhysBar账户下的xyapp.github.io版本库克隆到本地E/MyBlog/xyapp.github.io文件夹中。
cd xyapp.github.io
# 注释：原来是在E/xyapp目录中，上述命令使得进入到E/MyBlog/xyapp.github.io这个目录，亦即进入到对应的xyapp.github.io版本库，默认是在master主分支上。所有的git操作都在版本库中进行。
cd ..
# 注释：上述命令返回上一级目录，亦即返回到E/MyBlog目录。
ls
# 注释：上述命令返回当前目录内的文件夹和文件列表
```

[PhysBar]: https://github.com/xyapp

## 2、创建和切换分支

- 刚从远程克隆到本地的版本库中只含有master主分支，不含有其他分支。例如先进入到xyapp.git.io版本库
```
git branch
# 注释：查看当前版本库的分支。可以看到目前只有master分支。
git checkout -b hexo origin/hexo
# 注释：在本地创建hexo分支并切换到hexo分支，并让本地的hexo分支与远程的hexo分支关联起来。
```
- 此时使用**git branch**命令即可查看到有两个分支，并且当前所在分支为hexo分支。
```
git checkout master
# 注释：切换到master分支，可以使用该命令随意切换分支。
```
- 同样的步骤可以在其他三个版本库中创建和切换不同的分支。


## 3、发布博客文件并把文件加入到版本库并推送到远程

- 先使用命令**cd xyapp.git.io**进入到xyapp.git.io版本库，再使用**git checkout hexo**进入到hexo分支,再使用**ls**查看当前版本库当前分支中的文件结构，再使用**cd Hexo**进入到Hexo文件夹。
```
hexo new "newblog"
# 注释：创建文件名为newblog的新博客文章。
```
- 关掉命令窗口，在目录E/MyBlog/xyapp.git.io/Hexo/source/_posts中找到新建的newblog.md文件。
- 使用markdown语言在sublime编辑器中编辑修改该文件。修改完后保存。
- 在目录E/MyBlog/xyapp.git.io/Hexo中右键选择git bush
```
hexo g
# 注释：将上述编辑好的md文件生成博客文件
hexo s
# 注释：在本地查看文章效果。在浏览器中输入上面显示的地址即可进行查看。查看过后，在命令行窗口中Ctrl+C键结束。
hexo d
# 注释：将该文章发布到博客上，提交过程会提示让输入用户名和密码。
```
- 提交成功后就会在[XuYuan主页][XuYuan主页]上查看到最新发布的文章。
- 接下来需要将新添加的文件加入到git版本控制系统中。先使用**cd ..**命令返回到xyapp.git.io版本库的根目录。
```
git status
# 注释：上述命令显示当前工作区中文件的状态，并会有提示是否有文件需要加入到版本库或者提交到版本库。
git add -A （或者git add newblog.md）
# 注释：上述命令将所有文件修改（或者是newblog.md这个文件）准备加入到版本库，这时并没有真正将该文件放入到版本库。
git commit -m "add a newblog file"
# 注释：上述命令是将工作区的所有修改提交到本地版本库中。add a newblog file是对此次提交的说明文字，可以任意修改，方便后期查看历史提交记录。
git push origin hexo
# 注释：把本地的版本库修改提交到远程对应的版本库分支中。
```

[XuYuan主页]: http://xyapp.github.io

## 4、从远程库更新最新的版本库到本地

- 如果别人对远程版本库进行了修改，在工作之前要先把本地的版本库和远程的版本库同步。
```
git pull
# 注释：上述命令把远程库更新到本地，使本地的版本库中的所有内容与远程库中同步。
```


=================================================================================

一般的工作周期
------

1 先进入到本地工作区的xyapp.github.io版本库，切换到hexo分支，拉取远程最新版本库到本地


- **cd xyapp.github.io**
- **git checkout hexo**
- **cd Hexo**
- **git pull**


2 在本地工作区修改文件,生成博客文件

- **hexo new "new blog"**
- **hexo g**
- **hexo s**
- **hexo d**

3 将本地文件修改加入到本地的版本库中

- **git add -A**
- **git commit -m "describing the commit"**

4 将本地版本库推送到远程版本库

- **git push origin hexo**

**说明**：在进行上述步骤的过程中，可以随时使用**git branch**查看当前所在分支信息；使用**git status**查看当前工作状态，根据提示进行下一步操作；使用**git log**显示提交历史记录。





