![](..\pic\div\chuan.gif)

# Git

Git是当下很流行的一个**分布式版本管理系统**,
Git是一个免费的、开源的分布式版本控制系统，可以快速高效地处理从小型到大型的项目。
为什么要版本控制,因为每次版本的升级或多或少会出现一点带你bug,有了版本控制就可以很方便的实施版本回退;

> 什么是“版本控制”，你为什么要关心？ 版本控制是一种系统，它记录一段时间内对一个文件或一组文件的更改，以便您以后可以调用特定版本。
> 对于本书中的示例，您将使用软件源代码作为受版本控制的文件，但实际上您几乎可以对计算机上的任何类型的文件执行此操作。 ----摘自《Pro Git book》

在Git之前也有很多版本管理工具  

## 集成式版本管理系统

CVS SVN Preforce 等
他们的特点---
![在这里插入图片描述](https://img-blog.csdnimg.cn/26f322bdf2f3479c88f1112f9a4af424.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)每个人可以上传下载别人最新的代码,可以很好的方便程序员之间的协同工作,但是有一个缺点就是 如果中央服务器挂了,那么协同工作就无法进行了,如果中央服务器磁盘坏掉了,这很糟糕,将丢失所有数据——包括项目的整个变更历史，只剩下人们在各自机器上保留的一份数据。

## 分布式版本管理系统
由于集中化版本控制系统的一些缺点，于是分布式版本控制系统应运而生像Git, BitKeeper 等,每一个客户端并不只提取最新版本的文件快照，而是把代码仓库完整地镜像下来,这样每当发生故障都可以从本地/或者其他服务器上将项目完整的恢复;

他们的特点---

![在这里插入图片描述](https://img-blog.csdnimg.cn/084e0e6f981e4b9aafc0dd4fb78f1c5c.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)![引自《Pro Git book》](https://img-blog.csdnimg.cn/c1e0f01539b84a31a1895335d9d8513d.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
这就很好的解决了单一中央服务器出现单点故障的问题;


> **为什么git比bitKeeper会更流行呢?因为bit是开源的,开源的他不香吗!**


## 集中式版本管理与分布式版本管理有什么特点?

集中式版本管理记录的式各个版本之间的差异,
而分布式版本管理是将版本完整的镜像下来,
在《Pro Git book》一书中给出了很好的图解----

集中式版本管理---基于时间基于差异
![在这里插入图片描述](https://img-blog.csdnimg.cn/f99c5b0f9e7a4a83ab33cf1cf834bf6f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
分布式版本管理---基于时间,将有改动的地方的文件镜像下来,对于没有改动的会直接记录地址指向源文件而不是一同镜像下来
这样会节省空间;
![在这里插入图片描述](https://img-blog.csdnimg.cn/083c7c7ce4824eeab7694de85b24e7e2.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)另一个不同点是 git的镜像操做基本是在本地完成的,很少需要从网络上来完成版本的回退;


git保证数据完整性的原理------>>>

Git 中所有的数据在存储前都**计算校验和**，然后以**校验和来引用。** 即通过hash值来校验数据是否发生了改变

git保证历史镜像准确性的原理----->>
执行的git操做只会添加往git数据库中添加数据,很难从数据库中删除数据;



# git的工作原理
![在这里插入图片描述](https://img-blog.csdnimg.cn/b6a0bdb08c6549378acadfd4757567c4.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

## git的环境配置详解----->>
在安装git时我们可以通过修改配置文件来定制我们的git环境,这只需要在本机计算机中配置一次,以后的升级会保留之前的配置信息;

如何配置呢?
首先要找到系统配置文件所在位置----->>>
![在这里插入图片描述](https://img-blog.csdnimg.cn/3214b3a00e084e47afb2d0541cec9960.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
可以通过以下命令来查看git中的所有配置----

git config --list --show-origin
![在这里插入图片描述](https://img-blog.csdnimg.cn/7eca629de2ca402986b6a1b9983b510f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
由于刚开始,先做一些基本的认知然后再去配置git的环境



## git中设置用户名和邮箱
安装完git之后要做的第一件事就是设置用户名和邮件地址,因为每一个git都会提交这些信息,并且写入到每一次提交中,以便方便后期查看代码是由谁上传的;
```bash
git config --global user.name  "张三"
git config --global user.email "**@**.com"
```
如果用global区设置用户信息,那么之后无论在该系统上做任何事情,Git都会使用这些信息,如果相对特定项目使用不同的用户名和邮件地址,则可以在该项目目录下运行没有   --global的命令来配置;


## Git中文本编辑器的选择

在安装的时候Git会提示我们选择一种文本编辑器,推荐选择VIM,如果是下载的免安装版,则默认为系统自带的文本编辑器,如果想要更改文本编辑器,则需要配置一下,这里以notepad++为例------->>>>>

```bash
 git config --global core.editor "'C:/Program Files/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin"
```


# 检查配置信息

```bash
git config --list
```

![](https://img-blog.csdnimg.cn/089130b4dd434f2387e3239365b76a58.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

## 单独查看某一项配置信息

因为git中的配置信息以键值对得形式存储,所以可以通过
git config   key的形式来查看某一项配置




## 获取帮助文档
通过git help config来获取比较详细的帮助文档,该帮助文档是本地的,非常方便,不用再去git目录下寻找,通过命令可以直接打开;
![在这里插入图片描述](https://img-blog.csdnimg.cn/172ea1e2aea342f5b4a791ffee5d2378.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)



如果想要获得精华的帮助信息--
git  add -h

![在这里插入图片描述](https://img-blog.csdnimg.cn/fb4be9a9dc774da992e9c92fa3f20b90.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
在熟悉这些基本命令和对git有了一定的认知后,该区配置给git本地仓库了



# 初始化git仓库

首先在本地创建有一个git仓库,例如在D盘下新建一个文件夹  GitRepository 
![在这里插入图片描述](https://img-blog.csdnimg.cn/38e96e28d6384d998de33d9444cb6378.png)
然后通过git命令进入该文件夹下---然后通过 git init 类似初始化git仓库
![在这里插入图片描述](https://img-blog.csdnimg.cn/0f318033f9b140209c3bc0e7b2877e54.png)
(如果你有一个尚未进行版本控制的项目目录，想要用 Git 来控制它，那么首先需要进入该项目目录中。)
初始化成功后会在仓库中创建一个文件夹  .git  

什么?没看到?
可以在git张输入 ll -la 查看一下文件创建的信息

```bash
Gavin@LAPTOP-GAVIN MINGW64 /d/GitRepository (master)
$ ll -la
total 12
drwxr-xr-x 1 Gavin 197609 0 11月 14 16:30 ./
drwxr-xr-x 1 Gavin 197609 0 11月 14 16:35 ../
drwxr-xr-x 1 Gavin 197609 0 11月 14 16:36 .git/

```
然后接着看一下.git文件夹中都有哪些文件
进入.git文件夹

```bash

Gavin@LAPTOP-GAVIN MINGW64 /d/GitRepository (master)
$ cd .git

Gavin@LAPTOP-GAVIN MINGW64 /d/GitRepository/.git (GIT_DIR!)
$ ll -la
total 11
drwxr-xr-x 1 Gavin 197609   0 11月 14 16:36 ./
drwxr-xr-x 1 Gavin 197609   0 11月 14 16:30 ../
-rw-r--r-- 1 Gavin 197609 130 11月 14 16:36 config
-rw-r--r-- 1 Gavin 197609  73 11月 14 16:30 description
-rw-r--r-- 1 Gavin 197609  23 11月 14 16:30 HEAD
drwxr-xr-x 1 Gavin 197609   0 11月 14 16:30 hooks/
drwxr-xr-x 1 Gavin 197609   0 11月 14 16:30 info/
drwxr-xr-x 1 Gavin 197609   0 11月 14 16:30 objects/
drwxr-xr-x 1 Gavin 197609   0 11月 14 16:30 refs/

```

在本地查看隐藏项目也可以看到.git文件夹
![在这里插入图片描述](https://img-blog.csdnimg.cn/7488e3dc28b04a809f45658b04e0c548.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_19,color_FFFFFF,t_70,g_se,x_16)

> 这个子目录中含有**初始化的 Git 仓库中所有的必须文件**，这些文件是 Git 仓库的骨干(不要做修改,除非必要)。 但是，在这个时候，我们仅仅是做了一个初始化的操作，你的项目里的文件还没有被跟踪.

## 如何跟踪项目中的文件?

### git中常用的命令

还记得上面提到的  git工作原理吗?
工作区的文件先要add到暂存区然后再commit到仓库,这才完成了由git仓库去管理我们的文件;

跟踪项目文件演示---
首先创建一个文件以作为模拟项目
![在这里插入图片描述](https://img-blog.csdnimg.cn/c52cac76c9484003afe06a609c100d1f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
将文件提交到暂存区

```bash
Gavin@LAPTOP-GAVIN MINGW64 /d/GitRepository (master)
$ git add HelloWorld.java

```

接下来要将暂存库里的内容进行提交到git本地仓库

```bash

Gavin@LAPTOP-GAVIN MINGW64 /d/GitRepository (master)
$ git commit -m "这是测试文件HelloWorld.java" HelloWorld.java
[master (root-commit) 4bddb8f] 这是测试文件HelloWorld.java
 1 file changed, 5 insertions(+)
 create mode 100644 HelloWorld.java

```
 命令解析---
  -m 表示 message的意思,后面的内容表示 上传时的注释,以便后续方便查看 ,最后是我要上传的文件名;

> 注意:
> 1,git管理的文件必须通过git命令来添加,即使你将文件放在git本地仓库,git也不会主动去管理;
> 2,git要管理的文件首先要添加到暂存区,然后 commit到git仓库才会生效
> 3,在git仓库之外add或者commit会提示fatal: not a git repository (or any of the parent directories):



但是但是,如果过几天忘了某些文件是否提交到了git仓库,那怎么办?

开发者已经为我们想到了


执行  
git status 查看仓库中没有被追踪的文件

```bash
Gavin@LAPTOP-GAVIN MINGW64 /d/GitRepository (master)
$ git status
On branch master
nothing to commit, working tree clean
```




此时在git仓库文件夹下增加 一个文件----hello.java

> **工作目录下的每一个文件都不外乎这两种状态：已跟踪 或 未跟踪。 已跟踪的文件是指那些被纳入了版本控制的文件，在上一次快照中有它们的记录，在工作一段时间后， 它们的状态可能是未修改，已修改或已放入暂存区。简而言之，已跟踪的文件就是Git 已经知道的文件。 ----摘自《Pro Git book》**

可以看到很简洁的显示形式

??表示未被跟踪的文件
M表示修改过的文件
A表示新添加到暂存区的文件
MM表示暂存后又做了修改,因此既有暂存的部分也有未暂存的部分


然后执行git status 查看状态
![在这里插入图片描述](https://img-blog.csdnimg.cn/7457372dbd314dc58c1aa7b7418100f0.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
提交到暂存区后再查看状态----->>>可以被提交到git仓库

![在这里插入图片描述](https://img-blog.csdnimg.cn/307cff81963f4714a0a107ec863e0d6f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
提交到git仓库再查看状态

![在这里插入图片描述](https://img-blog.csdnimg.cn/b514d87552224fec9350a28ee2f74cf9.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

提交后   修改hello.java中的内容-----再次查看状态

![在这里插入图片描述](https://img-blog.csdnimg.cn/cae9c0de864e407daf3dfe98af2fb6e1.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

![在这里插入图片描述](https://img-blog.csdnimg.cn/d6b01f17466c464e918249ec94eb85bc.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

先提交到暂存区
![在这里插入图片描述](https://img-blog.csdnimg.cn/8234795994974126960b73f9f1f86040.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
提示增加了五行数据
```bash
Gavin@LAPTOP-GAVIN MINGW64 /d/GitRepository (master)
$ git commit -m "这是修改后的hello.java文件" hello.java
[master 0b10912] 这是修改后的hello.java文件
 1 file changed, 5 insertions(+)
```

> git status 命令的输出十分详细，但其用语有些繁琐。 Git 有一个选项可以帮你缩短状态命令的输出，这样可以以简洁的方式查看更改。
> 如果你使用 git status -s 命令或 git status --short 命令，你将得到一种格式更为紧凑的输出-----<>

![在这里插入图片描述](https://img-blog.csdnimg.cn/5be20309925c4e8d882986aa7e6af50f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)如果不想看到某些文件一直被提示未跟踪
,可以建一个名为.gitignore 的文件，列出要忽略的文件的模式-----
看一个 .gitignore 文件的例子：
```c
# 忽略所有的 .a 文件
*.a

# 但跟踪所有的 lib.a，即便你在前面忽略了 .a 文件
!lib.a

# 只忽略当前目录下的 TODO 文件，而不忽略 subdir/TODO
/TODO

# 忽略任何目录下名为 build 的文件夹
build/

# 忽略 doc/notes.txt，但不忽略 doc/server/arch.txt
doc/*.txt

# 忽略 doc/ 目录及其所有子目录下的 .pdf 文件
doc/**/*.pdf


```
新建不带扩展名的文件----.gitignore     在该文件中配置忽略的信息;
![在这里插入图片描述](https://img-blog.csdnimg.cn/06adfc7c475f42b49da2f9f11fdba41e.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/7cb6ff21b7e649b6a434f0b480a11544.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)在https://github.com/github/gitignore下给出了更为详细的案例可以参考;





犯罪总会留下证据的,这不每次我们提交到git仓库都会留下痕迹----

git log 命令查看提交的历史记录
时间为从近到远进行排序的;

![在这里插入图片描述](https://img-blog.csdnimg.cn/b5a56a0b4ff64cf4927bc9ded96c2cac.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

小技能点-----当历史版本很多的时候,一页装不下,
该怎么查看呢?
首先模拟一下  分页的环境---

创建很多个文件并提交到git仓库然后用 git log查看提交的历史记录----->>
![在这里插入图片描述](https://img-blog.csdnimg.cn/bbe0bf9fe7e44c2991023262eaa2b819.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

查看下一页----->>>  空格
查看上一页----->>>b
到尾页了显示end

如何简洁的显示历史记录呢?

git log --pretty=oneline  
此时会将每一次提交精简为一行便于查看;   哈希码+提示信息
![在这里插入图片描述](https://img-blog.csdnimg.cn/e32e54b6169b4608b80881c7ef0f695a.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
还有更简洁的方式------>>>哈希码前7位+提示信息
git log --oneline
![在这里插入图片描述](https://img-blog.csdnimg.cn/8134cf93e7fc4d4f9b6fe497529ff8cf.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
git reflog  查看历史记录
![在这里插入图片描述](https://img-blog.csdnimg.cn/e26db3cdecdf48df88470f1eddd66e1c.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
展示出的HEAD@{number}表示回退到当前所在版本需要回退几步;

### 版本升级或降级
首先新建一个文件git.txt添加内容,提交到仓库然后在修改,在提交,重复几次后;

执行git reflog
![在这里插入图片描述](https://img-blog.csdnimg.cn/a6e576fbf94b4d43bba0a8e66a58aeff.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

可以看到当前指针在1148702  insert ccc这一行,
移动当前指针到想要回退到的版本所在的行

执行 git reset --hard  7a4b2dc 

![在这里插入图片描述](https://img-blog.csdnimg.cn/2e98214312ea41cb8ddcc6f4fde38f8e.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

可以看到当前指针指在7a4d2dc所在的行,查看 demo.java文件,发现回退到insert bbb时候了
![在这里插入图片描述](https://img-blog.csdnimg.cn/806c92fa5e274da7aee611512c1501c4.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_15,color_FFFFFF,t_70,g_se,x_16)

同理可以进行版本的升级

![在这里插入图片描述](https://img-blog.csdnimg.cn/7a4fa63617b84a1e9e15b7e96fe13bb0.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
升级到insert ccc的版本了;
![在这里插入图片描述](https://img-blog.csdnimg.cn/7a9e2a8fe04b47589fb385aa06075bd0.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_13,color_FFFFFF,t_70,g_se,x_16)

这就是git仓库保证数据完整性的原理----根据提交的数据计算得到一个hash值,作为数据额索引,回退或者圣洁时候通过移动指针来回退或者升级到相应的版本;

### git 中文件重命名操做

在工作区我们要对一个文件重命名后,添加到暂存区然后提交到本地库两部操做.执行下面的命令可以简化 以下


git mv 要更改的文件 名    修改完后的文件名
![在这里插入图片描述](https://img-blog.csdnimg.cn/0903a242cea246cb82da49a10d823c58.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
## git reset --hard 与git reset --mixed 和git reset --soft 的区别
git reset --hard

当git仓库中指针移动时重置暂存区,工作区,即回退/升级的时候暂存区的版本内容根工作区的版本内容与git仓库中的保持一致;



git  reset --mixed

当git仓库中指针移动时重置暂存区,工作区的内容不会受到影响

git  reset --soft

当git仓库中指针移动时暂存区,跟工作区的内容不会受到影响


## 如何找回工作区中被删除的文件?(以提交到git库)

在工作区中删除文件 同步到暂存区和git仓库中

首先
工作区新建一个文件 hello.txt
添加到暂存区
提交到仓库

![在这里插入图片描述](https://img-blog.csdnimg.cn/7a89e2ee0c98408e9d06893d0ce3cfac.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
移除工作区的文件,并同步到暂存区跟仓库
![在这里插入图片描述](https://img-blog.csdnimg.cn/ac6b2c504c684874bbfeadf63104afb1.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
删除了后悔了怎么办?
可以恢复吗?

可以的

![在这里插入图片描述](https://img-blog.csdnimg.cn/d48acb821cf64cf39a93a7ae3082c52c.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

## 如何找回暂存区删除的文件?

还用刚才的hello.txt做例子,
![在这里插入图片描述](https://img-blog.csdnimg.cn/57cdd54c2fa34aa9bae293532f3d2f31.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
在暂存区同步过后后悔了,那怎么办?
跟在库中删除恢复的原理一样,重置为当前状态就可以了
![在这里插入图片描述](https://img-blog.csdnimg.cn/08254025c2d44e37846dc46aa8d0db23.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)



## 查看已暂存和未暂存的修改----->>
比对工作区域暂存区文件内容差别

在hello.txt中添加  一行数据---aaaaa

通过git dff  "hello.txt"-------查看工作区与暂存区内的差异"


```bash
Gavin@LAPTOP-GAVIN MINGW64 /d/GitRepository (master)
$ git  diff "hello.txt"
diff --git a/hello.txt b/hello.txt
index e69de29..e4a7dd9 100644
--- a/hello.txt
+++ b/hello.txt
@@ -0,0 +1 @@
+aaaaa//这是差异所在,添加了
\ No newline at end of file



//分析一下    下面命令提示    我做了什么操做

Gavin@LAPTOP-GAVIN MINGW64 /d/GitRepository (master)
$ git diff hello.txt
diff --git a/hello.txt b/hello.txt
index e4a7dd9..11cfc4a 100644
--- a/hello.txt
+++ b/hello.txt
@@ -1 +1 @@
-aaaaa
\ No newline at end of file
+abbbb
\ No newline at end of file


```
工作区域暂存区比较
git diff  -----查询所有文件差异
git diff 文件名----查询当前文件的差异

暂存区与工作区差别
git diff  HEAD 文件名---查询当前文件暂存区与仓库中文件的差异  ---这个是通过指针
或者使用git diff +索引 +文件名


```bash
Gavin@LAPTOP-GAVIN MINGW64 /d/GitRepository (master)
$ git diff HEAD hello.txt
diff --git a/hello.txt b/hello.txt
index e4a7dd9..11cfc4a 100644
--- a/hello.txt
+++ b/hello.txt
@@ -1 +1 @@
-aaaaa
\ No newline at end of file
+abbbb
\ No newline at end of file
```

小结----

此命令比较的是工作目录中当前文件和暂存区域快照之间的差异。 也就是修改之后还没有暂存起来的变化内容。

 git diff --staged 命令  查看已暂存的将要添加到下次提交里的内容。 这条命令将比对**已暂存文件与最后一次提交的文件差异**：
git diff 本身只显示尚未暂存的改动，而不是自上次提交以来所做的所有改动。 所以有时候你一下子暂存了所有更新过的文件，

# 分支的创建

## 什么是分支
在版本控制的过程中,可以开辟多条线以使得不同的程序员同时开发项目,如果一个分支开发失败,也不影响其他分支,各个分支开发完毕后可以进行合并;

查看分支-----
git branch -v

```bash
Gavin@LAPTOP-GAVIN MINGW64 /d/GitRepository (master)
$ git branch -v
* master 8b4ef3b 第一版
```

## 创建分支

git  branch 分支名

```bash
Gavin@LAPTOP-GAVIN MINGW64 /d/GitRepository (master)
$ git branch  demo01

Gavin@LAPTOP-GAVIN MINGW64 /d/GitRepository (master)
$ git branch -v
  demo01 8b4ef3b 第一版
* master 8b4ef3b 第一版   ---当前所在分支

```

## 切换分支

git checkout 分支名
![在这里插入图片描述](https://img-blog.csdnimg.cn/457c86a6e8924c0db6dbb2f2d397e21f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

在主分支下 demo.txt添加内容   
![在这里插入图片描述](https://img-blog.csdnimg.cn/af6c1d3eb81c45699217b696ea2a50fc.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
然后加到暂存区,在提交


```bash

Gavin@LAPTOP-GAVIN MINGW64 /d/GitRepository (master)
$ git add demo.txt

Gavin@LAPTOP-GAVIN MINGW64 /d/GitRepository (master)
$ git commit -m "主版本添加了信息" demo.txt
[master d2d81bd] 主版本添加了信息
 1 file changed, 3 insertions(+), 1 deletion(-)

```
切换到分支------demo01
然后查看demo.txt中的内容

![在这里插入图片描述](https://img-blog.csdnimg.cn/1b71da0d143b4af5be9a471efaa0982d.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
可以看到在主分支修改的内容不会影响到其他分支------即各个分支之间只负责开发自己的内容;
最后如果开发完毕,将各个分支进行合并

## 分支的合并

分支合并到主分支,所以要先切换到主分支
![在这里插入图片描述](https://img-blog.csdnimg.cn/6639b961ee3640fcb29d92d9bf3d5585.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
主分支个其他分支的文件中有冲突
查看文件---->>
![在这里插入图片描述](https://img-blog.csdnimg.cn/ee89c5c5aef44d0c96aa2eefb46fc083.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

或者在git中查看----
cat demo.txt

![在这里插入图片描述](https://img-blog.csdnimg.cn/3fba189150064a6784278b69c91d46c8.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

为什么会出现冲突?
因为不同分支中修改的内容为同一位置
主分支修改的为第二行,demo01分支第二内容为空,因此冲突了,
解决方案是------

修改文件内容------>>
删除或者保留需要的内容
![在这里插入图片描述](https://img-blog.csdnimg.cn/5d1ded6a391644788970c12373398ce4.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/d34dc4d3c47c4a47b669e0e04fd52fbe.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

![在这里插入图片描述](https://img-blog.csdnimg.cn/2150635367cb468280911b34e3d7c6e2.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


## 创建git远程仓库
注册一个git账户,

![在这里插入图片描述](https://img-blog.csdnimg.cn/e4e2f90156064b41907107e312e945c7.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/b2df2586091b41988ad39bf26b51ce87.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/d64c880151d8427c9b162d2c406a3300.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/8083ecfed69443b681e97859496c4a19.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

远程库的地址太长,可以为它起一个别名
首先产看一下是否有GIT库中是否有已经存在别名----


### 查看远程库别名

git remote -v

![在这里插入图片描述](https://img-blog.csdnimg.cn/d29a029f606c41f982a7450bafd38bf5.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

### 给远程库设置别名

git remote  别名   远程库的地址
![在这里插入图片描述](https://img-blog.csdnimg.cn/11bbdc077289480f93ff5b8b4f5ce2ef.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
fetch---取回
push---推送
可以看到我们的远程库支持上传和下载;

### 往远程库中推送内容---->>>

git push 远程库链接  文件名

由于github 服务器连接超时,这里用了一下gitee来测试

![在这里插入图片描述](https://img-blog.csdnimg.cn/399adccc148f43458593bbc9ae664a75.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
原理是一样的;


## 项目的克隆
git clone + url 来执行项目的克隆

> 如果你想在克隆远程仓库的时候，自定义本地仓库的名字，你可以通过额外的参数指定新的目录名：
>
> $ git clone https://github.com/libgit2/libgit2 mylibgit
>
> 这会执行与上一条命令相同的操作，但目标目录名变为了 mylibgit。
>
> Git 支持多种数据传输协议。 上面的例子使用的是 https:// 协议，不过你也可以使用 git:// 协议或者使用 SSH
> 传输协议，比如 user@server:path/to/repo.git

![在这里插入图片描述](https://img-blog.csdnimg.cn/ad64eae296c9408abaeaf6f4a0d2d183.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
可以查看项目的克隆
![在这里插入图片描述](https://img-blog.csdnimg.cn/f831aff454134dd7b3dfbef45cf5c479.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_19,color_FFFFFF,t_70,g_se,x_16)克隆结束后立即查看  文件状态----


```
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
nothing to commit, working directory clean
```

这说名所有文件都处于未修改状态

远程库的克隆操作会自动帮我们将远程库进行命名,默认为origin;![在这里插入图片描述](https://img-blog.csdnimg.cn/36f2606a77624304a61e92677369c678.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
这里测试以下远程库克隆后的上传----已修改部分文件
![在这里插入图片描述](https://img-blog.csdnimg.cn/fa808ccfe1c546e4bf1e432776eb6378.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

可以发现,当克隆之后没经过任何验证就推送成功了,![在这里插入图片描述](https://img-blog.csdnimg.cn/99518e40171c46408b30f9a15a5197f6.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)原因是git在操作的时候会用本地缓存,上传的时候直接从缓存走的,而非是从工作区,所以删除git缓存后再尝试一下

![在这里插入图片描述](https://img-blog.csdnimg.cn/b836a106c57347d8b401b546713a3fe9.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/b03ac83881ec4cf98144a9655f507742.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

> **看了网上的教程说修改hosts文件就可以了,但还是不稳定,断断续续的,通过其他方式的操做,github可以正常的访问了,具体怎么捣鼓的由于不提倡,所以在这里就不详述了,实在好奇的话可以私信我!**

注意,上面输入的用户名和密码由于是项目经理的,所以可以提交成功,如果是其他人的,则会提示提交失败,这是因为项目经理没有邀请他人加入团队;

![在这里插入图片描述](https://img-blog.csdnimg.cn/cde0f53dd2794e4f9577296b5e7c97e4.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/55bf57bd0878451e9a2dbed9356ae0d2.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)填写团队中的他人账号然后
复制邀请链接


![在这里插入图片描述](https://img-blog.csdnimg.cn/2aa445b85be441c28998d977feec5f34.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/d126dd9aa2c749cda7fba880b6e4e64b.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
大体流程是这个样子的------->>>
![在这里插入图片描述](https://img-blog.csdnimg.cn/9177a7b253b94c1d9af97f4600567c09.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

## 远程库修改的拉取


1,项目经理 准备一个库;
2,项目经理提交一些文件;
3,邀请团队中其他人加入;
4,团队成员克隆库中内容
5,团队成员修改内容后提交,上传到远程库

```java
Gavin@LAPTOP-GAVIN MINGW64 /d/GitRepository1/JeEisenberg (master)
$ git add -all
error: did you mean `--all` (with two dashes)?

Gavin@LAPTOP-GAVIN MINGW64 /d/GitRepository1/JeEisenberg (master)
$ git add --all

Gavin@LAPTOP-GAVIN MINGW64 /d/GitRepository1/JeEisenberg (master)
$ git status
On branch master
Your branch is up to date with 'origin/master'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   fetchindex.txt

Gavin@LAPTOP-GAVIN MINGW64 /d/GitRepository1/JeEisenberg (master)
$ git commit -m"抓取文件" fetchindex.txt
[master 6109944] 抓取文件
 1 file changed, 1 insertion(+), 1 deletion(-)

Gavin@LAPTOP-GAVIN MINGW64 /d/GitRepository1/JeEisenberg (master)
$ git push origin master
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 8 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 311 bytes | 311.00 KiB/s, done.
Total 3 (delta 1), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To https://github.com/JeEisenberg/JeEisenberg.git
   b9484ab..6109944  master -> master
```

之后查看项目经理工作区文件,并没有看到修改后的文件![](https://img-blog.csdnimg.cn/6e103d8b7db643c19200b12003e613ed.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
实际上远程库修改的拉取,是将远程库文件下载到本地(缓存),但是在工作区并不开到新增内容, 此时直接可以执行合并操做,如果想要查看团队修改的内容


可以切换到远程分支中的相应分区

![](https://img-blog.csdnimg.cn/f3c8c538c4624d9aaa0658ff1bc98a1a.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
查看文件无误后可以进行合并操作了

![在这里插入图片描述](https://img-blog.csdnimg.cn/51190869e2ac4517b1eebcb68dc62e37.png)
切换回项目经理的matser分支,执行合并操做

![在这里插入图片描述](https://img-blog.csdnimg.cn/37ae1c80c53246bf84ca88f6d83dc380.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
这时候项目经理工作区有了更改之后的文件了;![在这里插入图片描述](https://img-blog.csdnimg.cn/0aae120b1cc7465896ece5c9ad9074ec.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
总结------

> 远程库的抓取分为两步----
> 一个是抓取--fetch抓取到本地,并非工作区
> 抓取之后还需要合并才能在项目经理工作区看到,如果团队提交的修改内容不合格,项目经理可以直接pass掉

有时候代码比较简单,项目经理想要直接拉去,省去合并的手动操做,那么怎么办呢?


执行 git pull origin master  可以一步到位;

总结-----
什么情况下用 fetch+merge?什么时候用pull?

> 为了保险起见,当项目代码比较复杂的时候可以用fetch+merge   如果代码比较简单,则适合用pull



## 协同合作时发生了冲突

![在这里插入图片描述](https://img-blog.csdnimg.cn/98a3649491a64750beccd29f3147e2de.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
团队推送到远程库的版本----
```java
Gavin@LAPTOP-GAVIN MINGW64 /d/GitRepository1/JeEisenberg (master)
$ git add --all

Gavin@LAPTOP-GAVIN MINGW64 /d/GitRepository1/JeEisenberg (master)
$ git commit -m"团队修改的版本" --all
[master 147d898] 团队修改的版本
 1 file changed, 2 insertions(+), 1 deletion(-)

Gavin@LAPTOP-GAVIN MINGW64 /d/GitRepository1/JeEisenberg (master)
$ git push origin master
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 8 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 351 bytes | 351.00 KiB/s, done.
Total 3 (delta 1), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To https://github.com/JeEisenberg/JeEisenberg.git
   6109944..147d898  master -> master

```
项目经理推送的版本-![-](https://img-blog.csdnimg.cn/86d54c03c36c407cbd75e99df67bc60a.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
发生了冲突,这时候需要项目经理从远程库拉取过后修改,然后项目经理才能提交大远程库![在这里插入图片描述](https://img-blog.csdnimg.cn/fd028a947e4f40709cc7a7b57ab09810.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
修改后的文件------>>>
![在这里插入图片描述](https://img-blog.csdnimg.cn/2a381bdc40e741268cfb36c4416f5826.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/44772d78819b458fbe81fac230a1d42d.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)在合并时出现以下内容-------

```
Please enter a commit message to explain why this merge is necessary, especially if it merges an updated upstream into a topic branch
```

该怎么处理?
首先按照提示要求写以下合并的理由

怎么写入呢?

按 i   (或者insert)
然后写入合并的理由
再按esc退出书写模式
最后输入   :wq来结束操做![在这里插入图片描述](https://img-blog.csdnimg.cn/f55c94d18dda410a957a19b0821f9ebe.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


熟悉了一般操做后,还是习惯怎么简单怎么来,毕竟我们都是用ide进行项目的开发;

## IDEA中集成git


![在这里插入图片描述](https://img-blog.csdnimg.cn/fb729a28a64d4efa9aed64b36caf6ef9.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
完成git的配置之后,但是我们当前这个项目并没有与git仓库关联,还需要一步使其跟git仓库建立关联

![在这里插入图片描述](https://img-blog.csdnimg.cn/4a0f3f21352a479f8dfc03ec6cd6e44e.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

选择一个本地库,之后

添加项目文件后自动给弹出提示---受否加入跟踪
![在这里插入图片描述](https://img-blog.csdnimg.cn/6c6e1cda563e4c7188a68143073d74e2.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/63d95597cb4c4e5eaaf94d6ad63d63e2.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)有时候更新会被拒绝,这是因为要把两个不同的项目合并，git需要添加一句代码， --allow-unrelated-histories  告诉 git 允许不相关历史合并。
假如我们的源是origin，分支是master，那么我们需要这样写 git pull origin master --allow-unrelated-histories  ![在这里插入图片描述](https://img-blog.csdnimg.cn/248495121934408895675a248efdd669.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

在团队协同开发时如何解决冲突以及如何避免冲突
首先制造一个冲突
将远程库中的内容clone到本地
![在这里插入图片描述](https://img-blog.csdnimg.cn/6b75febdbe914b3ea2fa062b948e4726.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

![](C:\Users\Gavin\Pictures\Camera Roll\65.png)

## 如何避免冲突?

1,在团队开发的时候,分开开发,尽量不要修改用一个文件,你写你的,我写我的;
2,在推送之前先拉取远程库,看一下哪里修改了,然后再做push操做;





# 从远程库clone过来的项目遇到的问题

![在这里插入图片描述](https://img-blog.csdnimg.cn/3c31974b8e994461a6f79ac79970414e.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
原因是项目目录结构不对,
这是什么原因造成的呢?
![在这里插入图片描述](https://img-blog.csdnimg.cn/45280513889044019cbc4ed28bfca241.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)解决方案------>>

![在这里插入图片描述](https://img-blog.csdnimg.cn/e37bfb8de0854cfbada398827c7a8376.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

<center>

[<div   style="float:left;width:180px;heigh:20px"/>![](../pic/prepage.png)</div>](/#为什么会有这个网站)

[<div   style="float:right;width:180px;heigh:10px"/>![](../pic/nextpage3.png)</div>](https://git-scm.com/docs)

</center>