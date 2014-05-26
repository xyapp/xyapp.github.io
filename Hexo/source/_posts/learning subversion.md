title: Learning Subversion
date: 2014-05-24 12:50:35
author: XY
categories: Tools
tags: [Subversion]
---

###An Introduction of Leaning Subversion

实验室科研过程中需要用到Subversion，就学了一点点相关的基本概念和操作知识。这里给出一些基本的操作流程以供参考。

<!--more-->

Subversion 是一个集中式的版本控制系统，可以在[维基百科][维基百科Subversion]上查看相关介绍，这里不再说明，只介绍操作使用流程。

[维基百科Subversion]: http://zh.wikipedia.org/wiki/Subversion

##操作流程

###一、搭建Subversion服务器

1. 下载Subversion服务端安装程序。下载地址在[这里][Subversion Download]，可以去其[官网][Subversion官网]查看相关介绍。
2. 打开安装程序，安装Subversion。安装地址为D:/Subversion
3. 下载图形化的Subversion客户端TortoiseSVN，下载地址在[这里][TortoiseSVN Download]，也可在其[官网][Tortoise 官网]看到相关介绍和说明。该客户端使Subversion的使用变得更轻松和简单，省去了命令行方式需要记忆命令的苦恼。

[Subversion Download]: http://sourceforge.net/projects/win32svn/?source=recommended
[Subversion官网]: http://subversion.apache.org/
[TortoiseSVN Download]: http://sourceforge.net/projects/tortoisesvn/?source=directory-featured
[Tortoise 官网]: http://tortoisesvn.net/

###二、创建和配置版本库

1. 在空文件夹F:/Text中鼠标右击选择TortoiseSVN，再选择在此处创建版本库（Creat Repository Here），这样就在文件夹F:/Text中创建了一个版本库，该文件夹中也会相应的生成一系列的配置文件。
2. 配置版本库。在F:/Text/conf文件夹中可以看到三个配置文件svnserve.conf，passwad，authz。其中文件svnserve.conf是总的服务配置文件，文件passwad是用户账号和密码配置文件，文件authz是组和访问权限配置文件。下面分别对这三个文件进行简单的修改。
- 修改svnserve.conf文件。用记事本打开该文件，将其中的
```
# [general]
# passward-db=passwad
```
改为
```
[general]
passward-db=passwad
```

- 修改passwad文件。用记事本打开该文件，将其中的
```
# [users]
# harry=harryssecret
# sally=sallyssecret
```
改为
```
[users]
harry=harryssecret
sally=sallyssecret
```
这样就添加了两个用户分别为harry和sally，上面的等号后面分别为二者相应的密码。
至此就完成了简单的Subversion服务器的版本库的创建配置。其他客户端就可以直接访问该版本库。

###三、运行独立服务器

**> ** 打开命令窗口，输入命令：
```
svnserve -d -r F:/Text 
```
回车之后Subversion服务器即可打开。**注意：**该命令行窗口不能关闭，一旦关闭Subversion服务也就停止了。


**> ** 也可以将subversion服务注册为windows系统服务，可以随系统启动，这样就避免了每次启动服务都需要输入命令行的麻烦。操作步骤如下：

- 用管理员身份打开命令行窗口。
- 输入如下命令：

```
sc create svnserve binpath= "D:\Subversion\bin\svnserve.exe -service -r F:\Text" displayname= "Subversion" depend= Tcpip start= auto
```

- 如果上述命令执行成功则会显示[SC] CreateService成功。
- 然后在windows系统服务中可以看到Subversion服务，手动开启该服务即可。


###四、客户端访问版本库

1. 检出版本库的工作副本。在任意一台装有TortoiseSVN客户端的电脑上都可以检出版本库的工作副本。在一个空文件夹里（E:/SVN）右键选择SVN 检出(SVN checkout)，然后再弹出的检出对话框中输入版本库的地址，地址格式为**svn://服务器IP地址/版本库在服务器中的地址**，例如这里为**svn://101.6.98.29/F:/Text**这样就可以将版本库导出到本地的E:/SVN文件夹中。
2. 导入文件。由于刚建立的是一个空版本库，所以接下来需要向版本库中导入需要控制版本的文件。导入文件有两种方式，一种是右击要导入的文件选择TortoiseSVN，再选择导入(Import)，在弹出的导入对话框中输入要导入的地址，即可将该文件导入到版本库中。第二种方式是直接将需要导入到版本库的文件复制粘贴到刚才检出的工作副本F:/Text中，再在该文件夹中鼠标右击选择TortoiseSVN，再选择增加(Add)，即可将文件导入到版本库。
3. 如果上面导入之后发现某些文件不需要导入到版本库，可以直接将其删除，也可以使用SVN 还原功能将其返回到最开始状态。在工作副本文件夹中右击选择TortoiseSVN，再选择SVN还原(SVN Revert)即可。
4. 上面两种方式的导入只是将文件导入到本地的工作副本，并没有将文件提交到远程的版本库中。因此还需要使用SVN Commit将文件提交到远程版本库。在工作副本文件夹F:/Text中右击选择SVN 提交(SVN Commit)，输入远程版本库的地址以及说明信息，并在认证对话框中输入登录信息，即可将新增加的文件导入到远程的版本库中。这样任何人在其他电脑上也可查看到该文件。
5. 如果别人已对远程版本就行了修改，则需要将本地的工作副本进行更新使其保持与远程版本库一致。在本地工作副本文件夹中右击选择SVN 更新(SVN Update)，这样即可将远程的版本库改动更新到本地。
6. 查看版本库提交历史TortoiseSVN $\Rightarrow$ Show Log，查看版本库结构TortoiseSVN $\Rightarrow$ Repo-browser。
7. 时光机器——回退到任意历史版本。TortoiseSVN $\Rightarrow$ Update to revision，在弹出对话框中输入要返回到的版本号，即可瞬间返回到任意一个历史状态。
8. 文件对比。选择一个已修改过的文件，右击TortoiseSVN $\Rightarrow$ Diff，即可显示其修改前后的对比。此外也可以对比同一个文件的不同版本的区别。


###五、客户端操作的基本流程
```
1. SVN Checkout  #检出工作副本
2. SVN Update  #更新本地工作副本
3. SVN Import  #导入文件到工作副本
4. SVN Add  #添加文件到工作副本
5. SVN Diff  #显示修改前后的对比
6. SVN Revert  #还原本地工作副本到之前未修改状态
7. SVN Commit  #提交本地更改到远程版本库

```

###六、一些有用的参考链接

- [Subversion 中文站][Subversion 中文站]
- [vcmman的专栏：subversion与TortoiseSVN的使用][subversion与TortoiseSVN的使用]
- [51CT0博客：subversion的安装、配置、使用示例][51CT0博客]
- [Selway: Windows平台下SVN安装配置及使用][Selway]
- [360doc: Windows下安装并配置SVN服务器全过程][360doc]


[Subversion 中文站]: http://www.subversion.org.cn/?action-viewnews-itemid-76
[subversion与TortoiseSVN的使用]: http://blog.csdn.net/vcmman/article/details/6007479
[51CT0博客]: http://haoyun.blog.51cto.com/2038762/1232412
[Selway]: http://www.cnblogs.com/selway/articles/Windows_SVN_SetupAndConfig.html
[360doc]: http://www.360doc.com/content/10/1224/16/5243121_80996952.shtml