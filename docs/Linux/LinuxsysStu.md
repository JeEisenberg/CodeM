<div align=center><img src="..\pic\div\dog.gif"/></div>

# LinuxSYS

随着学习的深入,要捣鼓一下Linux系统,
![在这里插入图片描述](https://img-blog.csdnimg.cn/2889a00bc26d4913a77d3b3aed56adb7.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
还不错,虽然不如deepin系统那么好看,

但是对于企业来讲常常用CentOS
![在这里插入图片描述](https://img-blog.csdnimg.cn/1f30aeeb867e43d0a5c770b0dda4b5e4.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)这不我又开始搞事情了!


大家习惯用Vmware或者VirtureBox但是Win10自带一个虚拟机软件,可以不用再下载Vmvare了!
体验一下------>>>

这需要开启win10的HyperV 功能
在程序和功能中选择开启HyperV然后就可以搞事情了;


快速创建
![在这里插入图片描述](https://img-blog.csdnimg.cn/f99619adabc140bbb9b9c310f127280d.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)选择本地安装源,安装CentOs
![在这里插入图片描述](https://img-blog.csdnimg.cn/dc1c9313f30f4d01badb331e6cb636e7.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)选择本地下载好的镜像
可以去官网下载最新版,也可以使用其他版本
一般建议centos8
![在这里插入图片描述](https://img-blog.csdnimg.cn/197011655acc430eb51181a3e2fa4a10.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
[CentOS8百度网盘下载链接](https://pan.baidu.com/s/1N5qYkG0SJWxlWBrZjFu4MA)

提取码：qxpg

[CentOS9官网下载链接](https://mirrors.centos.org/mirrorlist?path=/9-stream/BaseOS/x86_64/iso/CentOS-Stream-9-latest-x86_64-dvd1.iso&redirect=1&protocol=https)

![在这里插入图片描述](https://img-blog.csdnimg.cn/a0a4fabcafaf496c9ce2e8340ad97db9.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

耐心等待就好了


安装过程就不再一一展示了:

由于centos还没有安装完毕,所以先在ubuntu上先学习一下喽!
这不是重点,重点是linux中的系统命令;


> 后来我还是选择安装了vmware虚拟机

在来了解系统命令是,先要了解liunx中的文件目录结构------>>>


/   表示linux的根目录,linux跟win不同点在于linux部分盘符而是一个硬盘一个根

打开linux系统控制台

cd / 进入根目录
ls    列出根下面的文件夹
![在这里插入图片描述](https://img-blog.csdnimg.cn/a9ff7b7dd7d346758dbdc72d7a76d99b.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)可以看到根目录下有  
/bin    系统的常用命令目录,包括控制台命令.系统可执行文件,系统的核心二进制文件;

/etc 发布目录保存系统所有核心内容,要求控制权限很高,一般不去随便读写;

/usr  用户目录 ,用于存放软件资源的,相当于win中的 programfiles目录;

/root 或者 ~ 表示 相当于win中的管理员 所在目录

/home 保存其他用户主目录的目录 

/var 系统运行过程中的数据目录




## linux的路径  ---->>>

### 全路径

在win中由于区分盘符,所以再写全路径的时候要加上盘符
比如----->>>>>
C:\Program Files\Microsoft\a.txt

在linux中同等的目录结构------>>>

/Program Files/Microsoft/a.txt

### 相对路径

![在这里插入图片描述](https://img-blog.csdnimg.cn/d9aeb378cba94d5580fa168df50e45b5.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

### 特殊路径

在linux中 特殊路径指的是 
~ 表示root目录
/ 表示根目录

![在这里插入图片描述](https://img-blog.csdnimg.cn/23e4f39c9cc94f468428e31ea5da3c94.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

## Linux中的常用命令
Linux 中的命令严格区分大小写

前面已经用了几个命令,
cd  -----全英文是change directory

 pwd----全英文是 print working directory

意思是打印当前工作目录
![在这里插入图片描述](https://img-blog.csdnimg.cn/13be803fd4e1495ab1228d93eed8f9d6.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)![在这里插入图片描述](https://img-blog.csdnimg.cn/3362e5dfa1214b6d8e0cf2b2de6cd974.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

ls---  全英文是list
列出当前文件夹下所有内容

![在这里插入图片描述](https://img-blog.csdnimg.cn/27cd66420415425bbb9cbecb0a10e697.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)touch 命令创建文件

默认创建的是文本文件

![在这里插入图片描述](https://img-blog.csdnimg.cn/9d4bcb802d034bcf9f66d3ca77beba5c.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
**ls -a 与ls -l 的区别**

![在这里插入图片描述](https://img-blog.csdnimg.cn/b479b88289af48fcbabdcc9b87a56769.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)ls -a 列出 所有文件 
ls -l列出所有文件并且带详细信息 

cat 来查看文件

![在这里插入图片描述](https://img-blog.csdnimg.cn/40dc93e11255409d96775169ad34e4b6.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16) 如果文件内容太多,则可以选择分屏显示
more +文件名
![在这里插入图片描述](https://img-blog.csdnimg.cn/c148df5bfc91433caa51ada1f78d61e7.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
查看文件内容开头几行  head +文件名
![在这里插入图片描述](https://img-blog.csdnimg.cn/8d2b8010ffb04db9bf0603c40267bfa6.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
head  -number +文件名

![在这里插入图片描述](https://img-blog.csdnimg.cn/718541141fe34651b097e37417b9da1d.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)同理也会有tail

![在这里插入图片描述](https://img-blog.csdnimg.cn/8188d984a4274574afe23f6105a17e95.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
创建单层目录 mkdir

![在这里插入图片描述](https://img-blog.csdnimg.cn/713a70058e844f8c82d61061a7e1429d.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)如果想要创建多层目录
mkdir +文件夹名+ -p
![在这里插入图片描述](https://img-blog.csdnimg.cn/600e034ffe12455bba03ca9d8d77a769.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
拷贝文件  cp + 源文件 +目标文件
![ ](https://img-blog.csdnimg.cn/4136ff0c053b4f3f9d500f9fcfcf93e8.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
拷贝整个文件夹---
cp +文件夹名 +目标文件夹名  -r

![在这里插入图片描述](https://img-blog.csdnimg.cn/7beaabb3575b421fb938229c5d913c5f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
删除文件--- rm +文件名

![在这里插入图片描述](https://img-blog.csdnimg.cn/383008932ecf47999fae761b5f9ad841.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
删除目录-- rm +目录名 + r

![在这里插入图片描述](https://img-blog.csdnimg.cn/f67a85b98a57472681fb80ee896d2772.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
剪切与重命名  mv

![在这里插入图片描述](https://img-blog.csdnimg.cn/727a627f8e0d4658b495ee590b6cc6c2.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)移动文件  mv
![在这里插入图片描述](https://img-blog.csdnimg.cn/a5eeb7cac014417ea5754dadd2abbd03.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)文本编辑器
vi 或者vim     并不是所有的linux系统走支持vim命令,


用vi命令编辑文件

![在这里插入图片描述](https://img-blog.csdnimg.cn/ab888f53cae644d5a14236b5afbe4f4a.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
vim中常用的编辑命令

![在这里插入图片描述](https://img-blog.csdnimg.cn/b7b6f66f71e84a299a3e028eecc54718.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)![在这里插入图片描述](https://img-blog.csdnimg.cn/ba9b70c51c764192a645573c01e1c554.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
## linux设置网络----
这是在虚拟机下设置的,
注意,在centos中默认时没有开启网络的,而在Ubuntu中默认时开启网络设置的;
在centos中开启网络---->>>
首先在控制台切换到root 目录下,输入
 nmcli c up ens33
 这样开启的网络设置只对当前用户有效;

 修改配置文件可以做到全局运行

在控制台输入--- 
vim /etc/sysconfig/network-scripts/ifcfg-ens33 
注意不一定是ens33,不同的机器可能不一样

centos安装完毕,查看卡名为----文件为 ens160![在这里插入图片描述](https://img-blog.csdnimg.cn/2e6971b579f04ee6921be9c5ce4968af.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/77e5011bee3247c49dcfbc701ce7d318.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)什么?提示为只读文件无法修改?

前面不是说了么etc文件下的内容修改需要很高的权限才行
cd进入到该文件所在目录,然后执行

![在这里插入图片描述](https://img-blog.csdnimg.cn/548c81d487444f58bb513064ca5d34af.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)





## linux中修改网络类型

### NAT模式

![在这里插入图片描述](https://img-blog.csdnimg.cn/3137ff8bcc174268aed14f423dd8004f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

![在这里插入图片描述](https://img-blog.csdnimg.cn/b9392a19dfe24b0e9fa037b1c0c00e53.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

### 桥连接模式
虚拟机跟win共享同一个IP网段
![在这里插入图片描述](https://img-blog.csdnimg.cn/e45a12a385874627a69f3c726492dcb3.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)将虚拟机关机
![在这里插入图片描述](https://img-blog.csdnimg.cn/55ddd905937848a9920e8a9d8066a191.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
设置为桥连接模式后在才开启虚拟机![在这里插入图片描述](https://img-blog.csdnimg.cn/dcbc6b78becd4611867dcc6a17f16826.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

## linux系统中的还原点---快照
与windows中不同的是linux中设置还原点是在关机之后
将虚拟机关机后,在虚拟机选项卡下选择快照来创建还原点
![在这里插入图片描述](https://img-blog.csdnimg.cn/c437a8ea605f45699b84fa6b5cd80cda.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)![在这里插入图片描述](https://img-blog.csdnimg.cn/fa1a99a5e06d4a56a091ee1f2e37cd5a.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

## 克隆虚拟机

克隆是创建一个跟当前状态一样的虚拟机
同样也是在关机状态下

![在这里插入图片描述](https://img-blog.csdnimg.cn/8ef10912d3d449c4898b8b92c556bb21.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/41569086f2ec41a3be7a76572c9f8db7.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
克隆完毕后可以正常的登录系统,说明Success![在这里插入图片描述](https://img-blog.csdnimg.cn/a77067f0abe94dbb91f2ed34e4b05156.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)




## 与linux系统的远程会话
这里借助xshell工具来完成与linux系统的会话

本次使用的是一个免费版的xshell6,土豪请选择最新版本xshell7
不过在使用xshell6的时候会弹出要你强制升级的弹窗!


下面是远程连接linux系统了,由于是linux系统是在虚拟机中因此虚拟机连接网络模式要更改为 桥接模式


然后进入系统
[xshell支装免升级版下载---](https://download.csdn.net/download/weixin_54061333/45585602)

控制台-----ifconfig查看虚拟机IP

如果此时你还没有设置联网,请设置一下,前面有怎么操作的步骤

打开xshell新建会话
输入linux 的IP

按照步骤操做后

连接成功
![在这里插入图片描述](https://img-blog.csdnimg.cn/42879cca4ce84d3095694278d1316536.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

linux与win互传文件----
[xftp6直装免升级版下载](https://download.csdn.net/download/weixin_54061333/45591069)


打开xftp6,配置跟xshell类似-都是输入linux的IP地址(桥接模式)


![在这里插入图片描述](https://img-blog.csdnimg.cn/167b1b9a454d460ab66b0cd82ceec75c.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
反之也一样;


## linux中的压缩与解压


我们在下载应用程序文件的时候,经常会让我们选择 平台,
我们在下载压缩文件的时候也是,我们会看到win下的rar,也会看到linux下的tar.gz文件;
 tar.gz是linux系统下的压缩文件按格式

### 在linux中如何压缩和解压文件?


#### linux中的tar.gz格式


![在这里插入图片描述](https://img-blog.csdnimg.cn/9ea2dce281de4886afa831401469d24e.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)显示压缩的所有过程--- ![在这里插入图片描述](https://img-blog.csdnimg.cn/15d9beef8bfb45c2a3cd645082d99acd.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

![在这里插入图片描述](https://img-blog.csdnimg.cn/c13563d7daf945cb8cae064344e17c9d.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/036caa772cf34e469d9bc67c3abb6faa.png)

解压缩时除了在本文件夹下操做,还可以指定解压到的文件夹

![在这里插入图片描述](https://img-blog.csdnimg.cn/b062cf0698d54ccc990d888877b34e60.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

#### Linux中另一种压缩文件格式.zip

![在这里插入图片描述](https://img-blog.csdnimg.cn/047ec4a4987a49bcac2a147879ad2b91.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

unzip  +要解压的文件名
![在这里插入图片描述](https://img-blog.csdnimg.cn/e6cea7a4995d4f379d4b008bfde4201b.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

![在这里插入图片描述](https://img-blog.csdnimg.cn/83564ff810814bba8d85f43b8f8d7000.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


## linux中安装JDK
哈哈,这才是主菜

怎么安装呢?
首先得有相应的安装包
去官网下载对应的安装包,linux版
[可以oracle移步官网](https://www.oracle.com/java/technologies/downloads/)

![在这里插入图片描述](https://img-blog.csdnimg.cn/2972751754974d7496a5d6e8a7d8bc7b.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
[此时还不够,下载一个jre吧](https://www.java.com/en/download/manual.jsp)

![在这里插入图片描述](https://img-blog.csdnimg.cn/d4710a4aa46043448248ef25fda932c1.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

准备工作结束后,解压......安装......
![在这里插入图片描述](https://img-blog.csdnimg.cn/173152430e8f4fb0acb9145ca342a5f2.png)


额----哈哈,看来直接配置环境变量就好了

首先是 java_home

这个需要在 根目录下的etc文件夹中的profile文件中配置
![在这里插入图片描述](https://img-blog.csdnimg.cn/11103963d2064674896bd2c0a143fa7b.png)
打开文件之后,在文件末尾添加配置信息
根据自己的JDK版本及JDK存放位置进行配置
classpath---java 5 以及以后的java版本都不需要再设置了。
![在这里插入图片描述](https://img-blog.csdnimg.cn/052ba99c02144f1e85b77b8af3d8c05b.png)

检测配置是否成功
![在这里插入图片描述](https://img-blog.csdnimg.cn/0871b87c70a34ee1b01a2bc621b21747.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

编辑一个java文件,然后运行
![在这里插入图片描述](https://img-blog.csdnimg.cn/fbf89859f2fe452d808f1782f78d7ba6.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
## linux中配置Tomcat
基本思路是一样
[先下载Tomcat  linux版本](https://tomcat.apache.org/download-90.cgi)

然后解压,配置环境变量就好了
![在这里插入图片描述](https://img-blog.csdnimg.cn/dc4f2af780d84498962eb53da62cee9e.png)
配置结束后要使得环境变量生效--
source /etc/profile


验证配置----

![在这里插入图片描述](https://img-blog.csdnimg.cn/1373553796164ece9dc5a52ec75b4555.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
但是并没有像win那样显示服务日志
有办法,可以这么做
先关掉 tomcat
然后执行

./startup.sh & tail -f ../logs/catalina.out
这是因为linux中将日志信息放在 logs/catalina.out下,我们在开启tomcat服务时就要执行查看 日志的命令
![在这里插入图片描述](https://img-blog.csdnimg.cn/d887ac3791ba46ffa88c5da8293f7137.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
此时我们访问一下tomcat

![在这里插入图片描述](https://img-blog.csdnimg.cn/cef9f42cff2440c7b64a34be9c154d4a.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)



发现无法访问,这是因为防火墙的存在阻止了此次请求,
可以暂时禁用linux防火墙
![在这里插入图片描述](https://img-blog.csdnimg.cn/3f192f3f86424d32b33a99156c932bf7.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/5303a025392d43c0b1a505ead7f68279.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)





> service firewalld stop
> 重启失效(Linux系统一重启Linux中的防火墙又会被开起)


再次访问Tomcat

![在这里插入图片描述](https://img-blog.csdnimg.cn/cbff5aa0fcc444f1811b048eb7fbd126.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

现阶段由于是学习阶段,可以完全禁用防火墙

> systemctl disable firewalld ----禁用防火墙



需要的时候再次开启就好了---->>


> systemctl enable firewalld---->>>开启防火墙



## linux中配置Mysql


也可以像上面一样的配置方法,在linux中可以在线安装



下载MySQL
wget https://repo.mysql.com//mysql80-community-release-el8-1.noarch.rpm

![在这里插入图片描述](https://img-blog.csdnimg.cn/7b3ceb60a81541868217fbf08b86c2f7.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


使用rpm安装MySQL 
wget https://repo.mysql.com//mysql80-community-release-el8-1.noarch.rpm
![在这里插入图片描述](https://img-blog.csdnimg.cn/f39f4b693613458783c00d995cbea850.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)



使用yum安装mysql服务
wget https://repo.mysql.com//mysql80-community-release-el8-1.noarch.rpm

![在这里插入图片描述](https://img-blog.csdnimg.cn/c8781cd55156428588853ff29d5059e6.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


![在这里插入图片描述](https://img-blog.csdnimg.cn/5799b47c5f214d1eb5bef06e41816a6e.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)



检查是否已经设置为开机启动MySQL
 systemctl list-unit-files|grep mysqld
 默认是开机不自启
![在这里插入图片描述](https://img-blog.csdnimg.cn/3c2a0564e40a422d8ab4917c77668ad7.png)



设置开机启动
systemctl enable mysqld.service

![在这里插入图片描述](https://img-blog.csdnimg.cn/1972964e67134303bebd1842dcb7c853.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


启动MySQL服务
 systemctl start mysqld.service
测试是否安装成功
mysql


![在这里插入图片描述](https://img-blog.csdnimg.cn/dcd992d57d4441669a3ce8054fda80c4.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


![在这里插入图片描述](https://img-blog.csdnimg.cn/43fcfa6bee094b708292cfda1e14432a.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)





# Unbutu更换国内源

学习路上总会遇到问题的对吧!别着急一个一个解决不就好了!!!

```c
Ign:60 https://mirrors.tuna.tsinghua.edu.cn/ubuntu bionic-proposed InRelease
Get:61 http://mirrors.aliyun.com/ubuntu bionic-backports/main amd64 Packages [10.3 kB]
Err:62 https://mirrors.tuna.tsinghua.edu.cn/ubuntu bionic Release
  Certificate verification failed: The certificate is NOT trusted. The certificate chain uses expired certificate.  Could not handshake: Error in the certificate verification. [IP: 101.6.15.130 443]
```


更新时遇到这个问题的别着急,
检查一下自己的源是否开头  https  如果是https开头的,那么只需要改为http就可以了;

一些常用的国内源

1,清华源

```
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal main restricted
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates main restricted
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal universe
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates universe
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal multiverse
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates multiverse
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-security main restricted
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-security universe
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-security multiverse

```

中科大源

```
deb http://mirrors.aliyun.com/ubuntu/ focal main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ focal-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-security main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ focal-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-updates main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ focal-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-proposed main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ focal-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-backports main restricted universe multiverse

```

改为国内源之后---->>>下载速度起飞了![下载速度起飞](https://img-blog.csdnimg.cn/65a2148e3fe24d9d84dab3f9eb7acf16.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)



# Linux知识复习

@[TOC](深入学习linux系统)

入门第一天---先从基本命令开始学习

# linux系统命令
目录切换与文件的创建
> ![这里是引用](https://img-blog.csdnimg.cn/3fd28a5c93004e2d82dbc22c2b7950c2.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/b2ab01ea1ab44ababea82f8e650d921f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

## 文件夹属性解析

![在这里插入图片描述](https://img-blog.csdnimg.cn/33c5aa432ae8490aa507ce54a08307a6.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

## 用户切换

![在这里插入图片描述](https://img-blog.csdnimg.cn/6b369cc486854b1a891533b973d451c7.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

## 目录的创建

![在这里插入图片描述](https://img-blog.csdnimg.cn/5f7e77d1d6424d1c9e3723899af80fde.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

## 文件拷贝与移动

![在这里插入图片描述](https://img-blog.csdnimg.cn/7ead8ad068e8430c9059c8a33259c08c.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

## 软连接与硬链接解析

![在这里插入图片描述](https://img-blog.csdnimg.cn/e9c36eac69274ff191e408799ac9e4e5.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
> 分析原因---新创建一个文件aaa
>
> 首先在我们查看一下aaa信息 gavin@Gavin:/tmp$ stat aaa   File: aaa   Size: 0       
> Blocks: 0          IO Block: 4096   regular empty file Device:
> 810h/2064d      Inode: 36970       Links: 1 Access: (0644/-rw-r--r--) 
> Uid: ( 1000/   gavin)   Gid: ( 1000/   gavin) Access: 2021-12-19
> 16:14:30.028057168 +0800 Modify: 2021-12-19 16:14:30.028057168 +0800
> Change: 2021-12-19 16:15:39.438057141 +0800  Birth: - 发现有一个指针指向aaa 
> inode 为 36970
>
> aaa中添加数据---->> gavin@Gavin:/tmp$ nano aaa gavin@Gavin:/tmp$ cat aaa
> helloworld gavin@Gavin:/tmp$
>
> 查看aaa信息---> gavin@Gavin:/tmp$ stat aaa   File: aaa   Size: 11         
> Blocks: 8          IO Block: 4096   regular file Device: 810h/2064d   
> Inode: 36970       Links: 1 Access: (0644/-rw-r--r--)  Uid: ( 1000/  
> gavin)   Gid: ( 1000/   gavin) Access: 2021-12-19 16:58:47.578056137
> +0800 Modify: 2021-12-19 16:58:14.748056150 +0800 Change: 2021-12-19 16:58:14.748056150 +0800  Birth: - gavin@Gavin:/tmp$
>
> 创建软连接---->> gavin@Gavin:/tmp$ ls aaa  alink  bbb  blink 
> hsperfdata_root  slink  slinked
>
> 查看alink信息---> gavin@Gavin:/tmp$ stat alink   File: alink -> aaa  
> Size: 3               Blocks: 0          IO Block: 4096   symbolic
> link Device: 810h/2064d      Inode: 40089       Links: 1 Access:
> (0777/lrwxrwxrwx)  Uid: ( 1000/   gavin)   Gid: ( 1000/   gavin)
> Access: 2021-12-19 16:59:06.488056130 +0800 Modify: 2021-12-19
> 16:59:05.138056130 +0800 Change: 2021-12-19 16:59:05.138056130 +0800 
> Birth: - gavin@Gavin:/tmp$
>
> 创建硬链接------>>
>
> gavin@Gavin:/tmp$ ln aaa alinked gavin@Gavin:/tmp$ ls aaa  alink 
> alinked  bbb  blink  hsperfdata_root  slink  slinked gavin@Gavin:/tmp$
>
> gavin@Gavin:/tmp$ stat alinked   File: alinked   Size: 11             
> Blocks: 8          IO Block: 4096   regular file Device: 810h/2064d   
> Inode: 36970       Links: 2 Access: (0644/-rw-r--r--)  Uid: ( 1000/  
> gavin)   Gid: ( 1000/   gavin) Access: 2021-12-19 16:58:47.578056137
> +0800 Modify: 2021-12-19 16:58:14.748056150 +0800 Change: 2021-12-19 17:01:48.508056067 +0800  Birth: - gavin@Gavin:/tmp$ 再次查看aaa信息---->>>
> gavin@Gavin:/tmp$ stat aaa   File: aaa   Size: 11              Blocks:
> 8          IO Block: 4096   regular file Device: 810h/2064d     
> Inode: 36970       Links: 2 Access: (0644/-rw-r--r--)  Uid: ( 1000/  
> gavin)   Gid: ( 1000/   gavin) Access: 2021-12-19 16:58:47.578056137
> +0800 Modify: 2021-12-19 16:58:14.748056150 +0800 Change: 2021-12-19 17:01:48.508056067 +0800  Birth: - gavin@Gavin:/tmp$

![在这里插入图片描述](https://img-blog.csdnimg.cn/16f3888ed0c844179028a51753c55b0f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

## 两级目录的解析
linux种是不分盘符的,在安装好linux之后有两层目录,
![在这里插入图片描述](https://img-blog.csdnimg.cn/2d99dfa6e8bc48cfa6a695477be068d8.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/b0306805ac6e497cac0d647827ee3b0d.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

> PING的时候  ctrl  c    结束ping


```bash
查看防火墙状态
[root@192 gavin]# systemctl status firewalld
● firewalld.service - firewalld - dynamic firewall daemon
   Loaded: loaded (/usr/lib/systemd/system/firewalld.service; enabled; vendor preset: enabled)
   Active: active (running) since 六 2021-12-25 04:50:55 PST; 35min ago
     Docs: man:firewalld(1)
 Main PID: 701 (firewalld)
    Tasks: 2
   CGroup: /system.slice/firewalld.service
           └─701 /usr/bin/python2 -Es /usr/sbin/firewalld --nofork --nopid

12月 25 04:50:54 localhost.localdomain systemd[1]: Starting firewalld - dynamic firewall daemon...
12月 25 04:50:55 localhost.localdomain systemd[1]: Started firewalld - dynamic firewall daemon.
12月 25 04:50:55 localhost.localdomain firewalld[701]: WARNING: AllowZoneDrifting is enabled. This is considered an insecure confi...t now.
Hint: Some lines were ellipsized, use -l to show in full.

关闭防火墙

关闭防火墙需要两步,第一步
移除防火墙服务,然后在关闭

[root@192 gavin]# systemctl disable firewalld  使得防火墙服务失效
Removed symlink /etc/systemd/system/multi-user.target.wants/firewalld.service.
Removed symlink /etc/systemd/system/dbus-org.fedoraproject.FirewallD1.service.
[root@192 gavin]# systemctl stasus firewalld
Unknown operation 'stasus'.
[root@192 gavin]# systemctl status firewalld
● firewalld.service - firewalld - dynamic firewall daemon
   Loaded: loaded (/usr/lib/systemd/system/firewalld.service; disabled; vendor preset: enabled)
   Active: active (running) since 六 2021-12-25 04:50:55 PST; 38min ago
     Docs: man:firewalld(1)
 Main PID: 701 (firewalld)
   CGroup: /system.slice/firewalld.service
           └─701 /usr/bin/python2 -Es /usr/sbin/firewalld --nofork --nopid

12月 25 04:50:54 localhost.localdomain systemd[1]: Starting firewalld - dynamic firewall daemon...
12月 25 04:50:55 localhost.localdomain systemd[1]: Started firewalld - dynamic firewall daemon.
12月 25 04:50:55 localhost.localdomain firewalld[701]: WARNING: AllowZoneDrifting is enabled. This is considered an insecure confi...t now.
Hint: Some lines were ellipsized, use -l to show in full.
[root@192 gavin]# systemctl stop  firewalld 关闭防火墙


[root@192 gavin]# systemctl status firewalld  防火墙状态
● firewalld.service - firewalld - dynamic firewall daemon
   Loaded: loaded (/usr/lib/systemd/system/firewalld.service; disabled; vendor preset: enabled)
   Active: inactive (dead)
     Docs: man:firewalld(1)

12月 25 04:50:54 localhost.localdomain systemd[1]: Starting firewalld - dynamic firewall daemon...
12月 25 04:50:55 localhost.localdomain systemd[1]: Started firewalld - dynamic firewall daemon.
12月 25 04:50:55 localhost.localdomain firewalld[701]: WARNING: AllowZoneDrifting is enabled. This is considered an insecure confi...t now.
12月 25 05:29:33 localhost.localdomain systemd[1]: Stopping firewalld - dynamic firewall daemon...
12月 25 05:29:34 localhost.localdomain systemd[1]: Stopped firewalld - dynamic firewall daemon.
Hint: Some lines were ellipsized, use -l to show in full.
[root@192 gavin]# 
```

配置网络的目的就是便于外部访问---注意虚拟机的网络链接模式要为NAT模式
配置结束后可以通过软件来操作linux了
![在这里插入图片描述](https://img-blog.csdnimg.cn/dd2c4546c07a48da9dc497e210caa542.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

## 文件的传输
借助一些ftp软件可以实现如xftp,
如果不借助外部软件,那么我们需要安装一个软件
lrzsz

```bash
zzy@localhost etc]$ sudo yum install lrzsz -y
Loaded plugins: fastestmirror, langpacks
Loading mirror speeds from cached hostfile
 * base: mirrors.bupt.edu.cn
 * extras: mirrors.bupt.edu.cn
 * updates: mirrors.bupt.edu.cn
Package lrzsz-0.12.20-36.el7.x86_64 already installed and latest version
Nothing to do
[zzy@localhost etc]$ 
```


之后传输文件----rz  然后选择发送到linux的文件

![在这里插入图片描述](https://img-blog.csdnimg.cn/cc5b89c6c26f4a62a622164d659fc635.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

![在这里插入图片描述](https://img-blog.csdnimg.cn/451e3e6ab2bd40b0b1ae06409b6ebdb8.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

# linux系统控制台提示未找到命令bash: sudo: 未找到命令...

今天在配置环境是出现了一个问题,控制台界面指令都失效了,可以说是全军覆灭

前一时间还好好的,这一回功夫就....
很显然,是配置环境是不小心配错了,

现在指令又不好使,怎么办呢?
如果是有虚拟机界面的可以在虚拟机里进行修改,但是发现当前登录用户非管理员...于是放弃这个思路了;
![在这里插入图片描述](https://img-blog.csdnimg.cn/db672d4c4eb547c7b74e7200636a292f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


命令行中输入以下命令可以暂时使得命令生效

```
export PATH=/usr/bin:/usr/sbin:/bin:/sbin
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/a637716c19ba4d6d84649a1a19409ab2.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)sudo

接着修改以下之前不当的配置

不在赘述,修改完之后重新导入以下配置文件

```
source /etc/profile
```
之后退出控制台,然后重新打开控制台
终端命令就好用了;
![在这里插入图片描述](https://img-blog.csdnimg.cn/8854e470ed334310864a09a2388a398a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
吸取教训之后,在windows中配置环境变量有两个,一个是全局变量一个是用户变量,在linux中配置全局变量要小心,如果配置错了,那么很可能就会造成命令行命令失效,进而连系统界面都进不去;
oh,这么严重吗!!!!!!

所以我们可以采取用户环境变量的配置来操作,这样即使配置有瑕疵也不会使得终端命令失效;

这个原理就是每次系统启动时都会扫描全局变量,但是也会扫描用户变量,用户变量存储在


etc/profile.d文件夹下

所以,索性将全局中的配置删除,在用户变量这里配置也能使得当前环境变量生效;

在用户变量这里新建一个文件-----java.sh

```
touch java.sh
```
添加环境变量
![在这里插入图片描述](https://img-blog.csdnimg.cn/8f22d1ad78c04ef6b9e55f889f2a1e19.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

```
[root@localhost profile.d]# cat java.sh
JAVA_HOME=/usr/local/jdk17/jdk-17.0.1
PATH=$JAVA_HOME/bin:$PATH
CONTALINA_HOME=/usr/apache-tomcat-9.0.56
PATH=$CONTALINA_HOME/bin:$PATH
MYSQL_HOME=/usr/local/mysql8
PATH=$MYSQL_HOME/bin:$PATH
export PATH JAVA_HOME CONTALINA_HOME MYSQL_HOME
```


export PATH JAVA_HOME CONTALINA_HOME MYSQL_HOME
这个命令是使得变量生效

保存后退出,
接着使得该变量生效--

```
source /etc/profile.d/java.sh
```

测试一下环境配置---->>

```
[root@localhost profile.d]# java -version
java version "17.0.1" 2021-10-19 LTS
Java(TM) SE Runtime Environment (build 17.0.1+12-LTS-39)
Java HotSpot(TM) 64-Bit Server VM (build 17.0.1+12-LTS-39, mixed mode, sharing)
[root@localhost profile.d]# javac -version
javac 17.0.1
[root@localhost profile.d]# mysql --version
mysql  Ver 8.0.20 for Linux on x86_64 (MySQL Community Server - GPL)

```

测试一下tomcat
![在这里插入图片描述](https://img-blog.csdnimg.cn/e2b1ac25183a401b94f449229332f767.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

![在这里插入图片描述](https://img-blog.csdnimg.cn/31a0aa41a6244eeead83c39a64f3146c.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)接着测试mysql
首先初始化mysql

```
[root@localhost bin]# **mysqld --initialize;chown mysql:mysql /var/lib/mysql -R;systemctl start mysqld.service;systemctl enable mysqld;**
2022-01-02T18:08:59.386227Z 0 [System] [MY-013169] [Server] /usr/local/mysql8/bin/mysqld (mysqld 8.0.20) initializing of server in progress as process 8585
2022-01-02T18:08:59.541233Z 1 [System] [MY-013576] [InnoDB] InnoDB initialization has started.
2022-01-02T18:09:05.894044Z 1 [System] [MY-013577] [InnoDB] InnoDB initialization has ended.
2022-01-02T18:09:18.400278Z 6 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: piqKsdubo7-#

```

登录后修改以下密码方便记忆(因为这是一个而临时密码,修改后可以避免以后的麻烦;)
![在这里插入图片描述](https://img-blog.csdnimg.cn/41c882e8648042f4aea241aa50892ee4.png)

@[TOC](manjorod的安装与配置)

# 欢迎各位采坑manjoro

你好！ 这是我第二次安装manjaro了如果不算虚拟机安装的话，

基本的安装步骤
1,拷贝到镜像到	U盘上
2,重启系统，然后U盘启动
3,自己选择一些配置，
可以将manjaro安装到一个盘符里，也可以和win共存

剩下的基本就是一路默认---
基本也不用选择，等待系统安装完毕


安装完毕后发现系统米有中文输入法，别着急，可以自己安装，毕竟manjaro是一个高度定制化的系统;

## 安装输入法前的准备-----》》

 **首先切换一下镜像**


```
sudo pacman-mirrors -i -c China -m rank       
```

然后选择一个比较快的镜像源，选一个就好，不然多个会影响下载速度，如果镜像源慢了，可以在切换另一个；

国内的镜像源，一般选择清华的或者国科大的，这个看自己的网络情况
```
                                                                                            
  0.680 China          : http://mirrors.huaweicloud.com/manjaro/
  0.383 China          : https://mirrors.ustc.edu.cn/manjaro/
  0.707 China          : https://mirrors.tuna.tsinghua.edu.cn/manjaro/
  0.252 China          : https://mirrors.tuna.tsinghua.edu.cn/manjaro/
  0.745 China          : https://mirrors.sjtug.sjtu.edu.cn/manjaro/
```
**刷新一下镜像源**
```
 sudo pacman -Syy      
```

安装 yay ----目的是以后就可以直接用 yay来安装软件了，不用每次都输入 pacman -S install来安装了                                                                                             

```
 sudo pacman -S yay            
```


**安装中文输入法框架**

```
 yay -S fcitx5-chinese-addons fcitx5-git fcitx5-gtk fcitx5-qt fcitx5-pinyin-zhwiki kcm-fcitx5


                                                                     
[sudo] gavin 的密码：
:: 在组 fcitx5-im 中有 4 成员：
:: 软件仓库 community
   1) fcitx5  2) fcitx5-configtool  3) fcitx5-gtk  4) fcitx5-qt
输入某个选择 ( 默认=全部选定 ): 
```
选择第一个就好；

安装完后，还需安装输入法软件，网上有很多说安装 搜狗输入法或者谷歌输入法的，但是要配置为 fictx,这个为旧版本的框架，已经不维护了；fictx与fictx5不能共存；

> 按照安装新版本不安装旧版本的原则，我选择安装了  fitcx5.



**配置全局变量**

使得中文输入法能够启动

```
nano ~/.pam_environment
```
或者使用vim编辑器也可以

```
vi ~/.pam_environment
```
注意，manjoro下只支持vi，不支持vim命令；

![](C:\Users\Gavin\Pictures\Camera Roll\71.png)



**接着就是安装输入法软件了-----》》**

```
yay -S fcitx5-rime
```


**配置输入方案**
```
   yay -S rime-cloverpinyin         
```

**写入rime-cloverpinyin输入方案**

```
nano ~/.local/share/fcitx5/rime/default.custom.yaml      
#写入内容
patch:
  "menu/page_size": 5
  schema_list:
    - schema: clover


```
**安装词库**
      
```
yay -S fcitx5-pinyin-zhwiki-rime
```
**配置输入法主题**

```
yay -S fcitx5-material-color
```

[详细配置参考链接](https://link.zhihu.com/?target=https://github.com/hosxy/Fcitx5-Material-Color)




配置好之后先别急再次配置一下声卡驱动，第一次安装时可能声卡会遇到一些问题

**安装声音管理工具**

```
  yay alsa-utils          
                                                                                    
```
一般选择最新版本就可以了

```
4 aur/alsa-utils-nosystemd-minimal-git 1.2.4-1 (+1 0.00) 
    Advanced Linux Sound Architecture - Utilities
3 aur/alsa-utils-git 1.2.4-1 (+1 0.00) 
    Advanced Linux Sound Architecture - Utilities
2 aur/alsa-utils-transparent 1.1.9-1 (+22 0.00) (孤立) (过时的: 2021-11-10) 
    A patched version of the alsa-utils package to support transparent terminals
1 extra/alsa-utils 1.2.5.1-1 (1.1 MiB 2.2 MiB) (已安装)
    Advanced Linux Sound Architecture - Utilities
==> 要安装的包 (示例: 1 2 3, 1-3 或 ^4)
==> 
```
安装结束后重启


重新设置一下输入法就可以了

![](C:\Users\Gavin\Pictures\Camera Roll\72.png)



设置一下声音

![](C:\Users\Gavin\Pictures\Camera Roll\73.png)
以上基本就配置完毕；

以上内容参考自网络，试了各种方法找到了一个适合的方式，也许会对你有所帮助；

![](C:\Users\Gavin\Pictures\Camera Roll\74.png)

# linux 环境配置教程详解


> 相信每个最先接触的是windows操作系统吧,而且用了那么多年,已经成为了习惯,但是由于工作需要,要接触linux系统,而且刚开始接触一般是选择虚拟机来安装linux;
> linux系统有很多派系而且每个派系还有一众小弟,但是企业用的linux服务器的数量就很少了; 像什么cent os,ubuntu os
> ,opensuse 服务器版,这些系统共同的特点就是比较稳定,但也有一个缺点就是更新比较慢; 毕竟稳字当先;


##  web配置
这里以centos8为例---虽然现在已经停止维护了,但基本配置思路是一样的;

```bash
[zzy@192 ]$ sudo vi /etc/sysconfig/network-scripts/ifcfg-ens160
```
初始配置如下

```bash
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=dhcp
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
NAME=ens160
UUID=a5a5e1cb-e1c0-4a57-a657-5c8c89541524
DEVICE=ens160
ONBOOT=yes

```
修改的内容有
1,BOOTPROTO=dhcp  改为static ---静态;避免每次的IP地址都不一样;
2,去掉UUID行
3,添加
IPADDR=192.168.135.145
NETMASK=255.255.255.0
GATEWAY=192.168.135.2
DNS1=114.114.114.114
最后修改后的结果如下
```bash
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=static
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=ens160
DEVICE=ens160
ONBOOT=yes
IPADDR=192.168.135.145
NETMASK=255.255.255.0
GATEWAY=192.168.135.2
DNS1=114.114.114.114

```
然后使得该配置生效---

```bash
source /etc/sysconfig/network-scripts/ifcfg-ens160 
```
查看ip 地址

```bash
[zzy@localhost ~]$ ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: ens160: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 00:0c:29:88:15:68 brd ff:ff:ff:ff:ff:ff
    inet 192.168.135.145/24 brd 192.168.135.255 scope global noprefixroute ens160
       valid_lft forever preferred_lft forever
    inet6 fe80::994e:beda:757e:3b6a/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever

```

如果跟自己配置的不一样,别着急,可能当时配置的并没有生效,在centos8 里可以重启一下网络服务

```bash
 systemctl restart NetworkManager
```

这里提一下配置网络的基本原理---->>

### 网络连接模式

**host-onboy(主机模式)**
在某些特殊的网络调试环境中，要求将**真实环境**和**虚拟环境隔离**开，这时你就可采用host-onboy模式。
在host-onboy模式中，所有的虚拟系统是可以相互通信的，但虚拟系统和真实的网络是被隔离开的。
在host-onboy模式下，虚拟系统的TCP/IP配置信息都是由VMnet1(host-onboy)虚拟网络的DHCP服务器来动态分配的

**bridged(桥接模式)**
VMWare虚拟出来的操作系统就像是局域网中的一台独立的主机，它可以访问网内任何一台机器。
使用桥接模式的虚拟系统和宿主机器的关系，就像连接在同一个Hub上的两台电脑。
当前主机IP 为 192.168.8.100 虚拟机 192.168.8.xxx
那如果用外部软件来连接虚拟机,那么这种模式是访问不到的,因为在同一个网段,路由的时候路由不到该虚拟机;

**NAT(网络地址转换模式)**
使用NAT模式，就是让虚拟系统借助NAT(网络地址转换)功能，通过宿主机器所在的网络来访问公网。
NAT模式下的虚拟系统的TCP/IP配置信息是由VMnet8(NAT)虚拟网络的DHCP服务器提供的虚 拟系统也就无法和本局域网中的其他真实主机进行通讯



## JDK的安装
查找一下java软件
```bash
[root@localhost ~]# yum search java
```
你会看到java所有的软件
![在这里插入图片描述](https://img-blog.csdnimg.cn/446ca6094b6c4e7e99a22f3bf9fead33.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
选一个进行安装(软件右面有提示说明这个软件是做什么的)
如果网络慢的话,那就可以修改一下数据源;
(这个跟你在哪里下载的系统有关,我是在阿里云下载的,所以系统默认软件源为ali云,如果是中科大那系统默认源就是中科大,官网则是官方源)
验证一下

```bash
[root@localhost ~]# java version
Error: Could not find or load main class version
Caused by: java.lang.ClassNotFoundException: version
[root@localhost ~]# java -version
openjdk version "11.0.13" 2021-10-19 LTS
OpenJDK Runtime Environment 18.9 (build 11.0.13+8-LTS)
OpenJDK 64-Bit Server VM 18.9 (build 11.0.13+8-LTS, mixed mode, sharing)
[root@localhost ~]# javac  -version
javac 11.0.13

```

接着是jdk环境的配置

找到jdk安装目录;

```bash
[root@localhost profile.d]# whereis java


java: /usr/bin/java /usr/lib/java /etc/java /usr/share/java /usr/share/man/man1/java.1.gz
[root@localhost profile.d]# 
```
这就很懵逼了 哪一个才是呢?带有bin的?都带有bin文件夹呀,真假美猴王在上演?

别着急,使出照妖镜----
元神在这里,高兴的去找它,你会发现这货并不是
```bash
[root@localhost bin]# which java
/bin/java
```

在照一下

```bash
[root@localhost /]# ls -lr /usr/bin/java
lrwxrwxrwx. 1 root root 22 Jan  2 23:56 /usr/bin/java -> /etc/alternatives/java
```
这个也不是,那就再照一下

```bash
[root@localhost /]# ls -lrt /etc/alternatives/java
lrwxrwxrwx. 1 root root 64 Jan  2 23:56 /etc/alternatives/java -> /usr/lib/jvm/java-11-openjdk-11.0.13.0.8-4.el8_5.x86_64/bin/java
[root@localhost /]# 
```
这回没跑了吧--->配置一下javahome

但是这样后期不太方便维护,还是下载到本地的压缩包配置比较容易维护
这里提供一下本地安装的配置文件
![在这里插入图片描述](https://img-blog.csdnimg.cn/aceafe65c1d74ef9a6618bbccc8230bb.png)

## mysql的安装
跟jdk开发环境软件安装思路一致,
去官网下载对应的mysql版本
然后解压到 /usr/local
然后配置一下变量环境---
在/etc/profile.d 下新建一个文件 mysql.sh
添加变量
Linux （centos8）安装 MySQL 8 数据库
tomcat,mavenp配置思路也是一样的,先去官网下载对应版本的安装包,然后解压到
/usr/local 文件夹下
之后配置环境变量
![在这里插入图片描述](https://img-blog.csdnimg.cn/f7c0b8d609304a6baeb34d56395ae4a4.png)

这是一些环境变量可以在里面增加一些自己的环境变量
![在这里插入图片描述](https://img-blog.csdnimg.cn/2183082f09734434a3175c1941945808.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
之后进入 /usr/local 目录下，创建用户和用户组并授权---->>
这一步是必要的,不然mysql只会在root权限下运行,不便于使用

注意 ----要进入 /usr/local 目录下在操作
```bash
[root@localhost local]# groupadd mysql  ----增加用组mysql
[root@localhost local]# useradd -r -g mysql mysql    ---在mysql用户组中添加用户
```
授权---->

```bash
[root@localhost local]# cd mysql/  ---相当于选中mysql文件夹下所有文件
[root@localhost mysql]# chown -R mysql:mysql ./   授权所有文件

```
查看是否成功---->>

```bash
[root@localhost mysql]# ll
total 284
drwxr-xr-x.  2 mysql mysql   4096 Sep 28 07:42 bin
drwxr-xr-x.  2 mysql mysql     55 Sep 28 07:41 docs
drwxr-xr-x.  3 mysql mysql    282 Sep 28 07:41 include
drwxr-xr-x.  6 mysql mysql    201 Sep 28 07:42 lib
-rw-r--r--.  1 mysql mysql 276550 Sep 28 04:46 LICENSE
drwxr-xr-x.  4 mysql mysql     30 Sep 28 07:41 man
-rw-r--r--.  1 mysql mysql    666 Sep 28 04:46 README
drwxr-xr-x. 28 mysql mysql   4096 Sep 28 07:42 share
drwxr-xr-x.  2 mysql mysql     77 Sep 28 07:42 support-files
[root@localhost mysql]# ll

```
配置一下mysql数据存放位置
在 /usr/local/mysql 文件夹下建立一个数据存放区  data  

```bash
[root@localhost mysql]# mkdir data
[root@localhost mysql]# ls

```
初始化数据库

```bash
[root@localhost mysql]# bin/mysqld --initialize --user=mysql --basedir=/usr/local/mysql --datadir=/usr/local/mysql/data
```


会出现下面的内容,说明操作正确;
![在这里插入图片描述](https://img-blog.csdnimg.cn/a94f40ede3294c1fa0c39b60c6d062dd.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
由于之前操作了mysql文件夹下所有文件的归属者为 mysql组下的mysql,这样做是为了能够使得mysql初始化时有权限能够生成相应的必要文件,配置结束后就要收回权限,
但是data文件夹的权限保留,不然怎么写入数据库
所以要修改一下data这个文件夹的权限
![在这里插入图片描述](https://img-blog.csdnimg.cn/bea21df722614f7f945690ab1470e35c.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

还是要进入 /usr/local/mysql目录下
```bash
[root@localhost mysql]# chown -R root:root ./
[root@localhost mysql]# chown -R mysql:mysql data
[root@localhost mysql]# ll

​```![在这里插入图片描述](https://img-blog.csdnimg.cn/5e959bca69e443bf837ff2a7c64a3cd0.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
 在 /usr/local/mysql/support-files下创建my.cnf配置文件

​```bash
root@localhost support-files]# touch my-default.cnf      ----创建一个默认配置文件为空

[root@localhost support-files]# chmod 777 ./my-default.cnf   ---授予权限

回到上级目录
[root@localhost support-files]# cd ..

[root@localhost mysql]# cp support-files/my-default.cnf /etc/my.cnf 
---复制到etc目录下,并重命名
[root@localhost mysql]# cd /etc
---查看是否操作成功
[root@localhost etc]# ls | grep my.cnf
my.cnf
[root@localhost etc]# 
```


 接着配置my.cnf


```bash
vi my.cnf

[client]
port=3306
socket=/var/lib/mysql/mysql.sock

[mysql]
socket=/var/lib/mysql/mysql.sock

[mysqld]
port=3306
user=mysql
socket=/var/lib/mysql/mysql.sock
basedir=/usr/local/mysql
datadir=/usr/local/mysql/data
log-error=/usr/local/mysql/data/error.log
pid-file=/usr/local/mysql/data/mysql.pid
transaction_isolation=READ-COMMITTED
character-set-server=utf8mb4
collation-server=utf8mb4_general_ci
lower_case_table_names=1
sql_mode=NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES

```


设置一下开机自启

```bash
[root@localhost etc]# cd /usr/local/mysql/support-files
[root@localhost support-files]# cp mysql.server /etc/init.d/mysql
[root@localhost support-files]# chmod +x /etc/init.d/mysql
[root@localhost support-files]# 
# chkconfig --add mysql

```


验证一下

```bash
[root@localhost support-files]# chkconfig --list mysql

Note: This output shows SysV services only and does not include native
      systemd services. SysV configuration data might be overridden by native
      systemd configuration.

      If you want to list systemd services use 'systemctl list-unit-files'.
      To see services enabled on particular target use
      'systemctl list-dependencies [target]'.

mysql          	0:off	1:off	2:on	3:on	4:on	5:on	6:off


```

配置自启动路径

```bash
[root@localhost support-files]# vim /etc/ld.so.conf
```
添加如下内容
![在这里插入图片描述](https://img-blog.csdnimg.cn/87c2ac900d044f48af8f579807c213c8.png)在启动过程中可能会遇到一些小小的配置错误
比如我遇到了

```bash
[root@localhost etc]# service mysql start
Starting MySQL. ERROR! The server quit without updating PID file (/usr/local/mysql/data/mysql.pid).
```

问题出现的原因就是没有在/usr/loacl/mysql/support-files 目录下进行授权,所以my-default.cnf之前的权限是这样的
![在这里插入图片描述](https://img-blog.csdnimg.cn/79afcda521c64ee8b10b478fb07d658b.png)

```bash
[root@localhost support-files]# chmod 777 ./my-default.cnf`   
```

---授予权限之后
![在这里插入图片描述](https://img-blog.csdnimg.cn/f37f475f2d924180972e3dbee849a3de.png)
这个操作之后在重复执行后续步骤
额.....还是报错,复制过去之后的文件权限 还是  -rw-r--r-- 
可能是复制过去的文件夹不和源文件夹权限相同吧,(暂且不管它)
没办法,只能在etc 文件夹下再次授权my.cnf

之后,可以正常启动mysql了

```bash
[root@localhost bin]# systemctl start mysqld.service
[root@localhost bin]# systemctl status  mysqld.service
● mysqld.service - MySQL 8.0 database server
   Loaded: loaded (/usr/lib/systemd/system/mysqld.service; disabled; vendor preset: disabled)
   Active: active (running) since Mon 2022-01-03 23:30:43 PST; 12s ago
  Process: 46474 ExecStopPost=/usr/libexec/mysql-wait-stop (code=exited, status=0/SUCCESS)
  Process: 47929 ExecStartPost=/usr/libexec/mysql-check-upgrade (code=exited, status=0/SUCCESS)
  Process: 47802 ExecStartPre=/usr/libexec/mysql-prepare-db-dir mysqld.service (code=exited, status=0/SUCCESS)
  Process: 47777 ExecStartPre=/usr/libexec/mysql-check-socket (code=exited, status=0/SUCCESS)
 Main PID: 47884 (mysqld)
   Status: "Server is operational"
    Tasks: 38 (limit: 23516)
   Memory: 452.9M
   CGroup: /system.slice/mysqld.service
           └─47884 /usr/libexec/mysqld --basedir=/usr

```

登录用户root密码是之前随机生成的,相信你已经记录下来了,如果没有,那么可以在/usr/local/mysql/data/mysql.log日志文件中找到

登陆时出现一个警告---->>

```bash
[root@localhost etc]# mysql -uroot -hlocalhost -p
mysql: [Warning] World-writable config file '/etc/my.cnf' is ignored.
Enter password: 

```
这表示my.cnf配置文件任何人都可以读写,那不行,既然mysql可以正常启动了,那么就要 杯酒释兵权呀,将权限修改回来

```bash
[root@localhost etc]# chmod 644  my.cnf

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/2e822c27ae204052b4fe2bcabad896db.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
改回来之后重启又报错了---->>

```bash
[root@localhost etc]# service mysql start
Starting MySQL. ERROR! The server quit without updating PID file (/usr/local/mysql/data/mysql.pid).
[root@localhost etc]# 

```

说名 权限改的不太对,改回644又无法登录,索性先将用户配置去掉,

找到默认的my.cnf配置文件

```bash
[root@localhost etc]# which mysql
/usr/local/mysql/bin/mysql
[root@localhost etc]# /usr/local/mysql/bin/mysql --verbose --help | grep -A 1 'Default options'
Default options are read from the following files in the given order:
/etc/my.cnf /etc/mysql/my.cnf /usr/local/mysql/etc/my.cnf /usr/local/mysql/my.cnf ~/.my.cnf 
[root@localhost etc]# 

```
可以看到配置文件默认的加载书顺序,
首先读取/etc/my.cnf,没有的话再去读取/etc/mysql/my.cnf 以此类推

于是乎我就挨个目录去找,发现所有的文件夹下都没有my.cnf 配置文件,
这个权限真的不知道怎么处理了,
于是我就想那它默认加载的配置在那个文件夹中?

```bash
[root@localhost mysql]# locate my.cnf
/etc/my.cnf
[root@localhost mysql]# cd /etc
[root@localhost etc]# ll my.cnf
ls: cannot access 'my.cnf': No such file or directory

```
好家伙,就在/etc/my.cnf,但是这个etc下又没有这个配置文件(刚刚被我重命名了,以防生效)

目前只有从权限方面入手了;
试了775,不好使, 好像只有777 好使也不能说好使,是mysql服务器直接忽略了这个配置,

如果不是777,那么就会报 ......pid not update之类的错误;

暂时没有找到更好的解决方式,还请遇到这种情况的大佬给个解决方案


## tomcat 与maven配置

这个就简单了,下载软件解压到指定文件夹
![在这里插入图片描述](https://img-blog.csdnimg.cn/09687ef6378a465eb0c18b4a6edcb870.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
配置一下变量环境
![在这里插入图片描述](https://img-blog.csdnimg.cn/e2919568de8d4f03a536190c816849c9.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)



![在这里插入图片描述](https://img-blog.csdnimg.cn/10d47dc2b96c46b2b050533e98901d9d.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/df7c2d72745e41628bc72ad4bff95831.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/91355ae1bf864cef9faf3a54a0a1a67c.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/adf57d91d17648a1aec75b66214711e9.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/7dfb023e0538480080a6ac5a59259298.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

# 分布式与集群

## 分布式

百度百科给出的定义是----->>>

![在这里插入图片描述](https://img-blog.csdnimg.cn/374c85e38750453bbbb648aec7e41265.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

简单来收就是现在的企业将不同的业务分不到不同的地方,以减少单一数据中心的压力;
在实际操作中分布式与集群常常会结合使用;

比如下面这个分布式图解----->>
![在这里插入图片描述](https://img-blog.csdnimg.cn/c9965f2db2e943799c2250f9cf3f5896.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

## 集群
百度百科给的定义----->
![在这里插入图片描述](https://img-blog.csdnimg.cn/9ed3d76b99f1445aaee75347c944b866.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
简单来说就是由原来单一业务服务器发展为多台相同的业务服务器,他们为企业提供了一组业务服务器,每一台服务器都是一个节点,当主节点没有发生故障时可以帮助主节点分散一些负载,当主节点发生故障时另一个节点自动接管服务, 提高了系统的可靠性;

考虑到在分布式架构上,万一哪一天某一个服务器宕机了,这个业务就不可访问了,为了避免这种情况,所以就要有替补的服务器跟上,在原来分布式架构的基础上有了下面的演变 **分布式加集群**
![在这里插入图片描述](https://img-blog.csdnimg.cn/1cc63ed49d324ce6b74d15c4d57f61a2.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
这种分布式加集群的好处就是:
1,减少每一个服务器的负载(最大负载量比单纯的分布式要大);
2,提高了系统的容错性与可靠性;
3,提高了并发吞吐量;

## 分布式项目的开发
这里只是做一个比较简单的分布式项目开发,
当然这是在之前写的代码的基础上去完成分布式的实现------>>>
[\[解密贴\]那些年我们上传的文件都去了哪里?](https://blog.csdn.net/weixin_54061333/article/details/122260175)

分布式的实现
首先需要有一个框架
多个服务器,这里选用多个tomcat做为服务器
整个框架是这样的---->

![在这里插入图片描述](https://img-blog.csdnimg.cn/6b88ebf0de304dd488de2a0324fa16b3.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
灰常简单,是不是!!

### 实践出真知----->

#### 环境搭建

##### 准备服务器

第一步,复制本地的tomcat,到本地磁盘的其他目录(当然如果有虚拟机的可以直接在虚拟机上开启),mysql服务器

![在这里插入图片描述](https://img-blog.csdnimg.cn/dd965a8803c24826bf87c11d86a392e5.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
第二步－－－－－＞＞＞配置服务器
主要是解决一下两个tomcat启动的端口号冲突问题

哪些端口号会冲突了呢?
3306这是mysql的默认端口,暂时没有冲突
第一个肯定是8080 端口,还有别的吗?
暂且不知道,那就启动两个tomcat看看会报什么异常吧;

在idea中启动项目--tomcat
![在这里插入图片描述](https://img-blog.csdnimg.cn/d631df1aa4d94949bae09ce371660774.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
接着启动另一个tomcat,启动之前先把log里的日志文件清掉,以便于更好的查看端口冲突;
**8080端口冲突**
![在这里插入图片描述](https://img-blog.csdnimg.cn/aa8cbdd66e364d01b26db08a5bdf6ac2.png)
**8005端口冲突**
![在这里插入图片描述](https://img-blog.csdnimg.cn/041c3eb18c4c4f1b894cbab7c95a9d69.png)

找到冲突的端口号之后,修改有冲突的端口号---->
![在这里插入图片描述](https://img-blog.csdnimg.cn/e3d2050649a74e76884fbe8666e77b2f.png)

注意,这里没有涉及到8443冲突,如果改了,那么tomcat虽然可以开启,但是关闭的时候会关不上;
![在这里插入图片描述](https://img-blog.csdnimg.cn/175f693046004ab7a543e2b9f149acf4.png)
还有一个很重要的配置文件---->>
修改web.xml中的 名为default的servlet 初始化参数  readonly为 false

> 刚开始配置的时候会没有这个属性,需要手动添加(因为有许多默认的配置信息并没有体现出来----只读默认为true,即可以读取但不允许写入,要想传输数据,那么要改为false)

![在这里插入图片描述](https://img-blog.csdnimg.cn/090f54140a3c4dd8893e82fdb0a9abe3.png)
这时候启动一下看看能否正常启动
![在这里插入图片描述](https://img-blog.csdnimg.cn/e493622a26d8444e8267d0244346491e.png)

正常关闭;
但是我发现了一个问题,即使复制了一份tomcat,并且设置了监听端口为8090,但是也会默认去找8080端口
想了想原因,原来是设置了默认的系统环境变量 CATALINA_HOME 以及path,删掉这两个环境变量就好了;

![在这里插入图片描述](https://img-blog.csdnimg.cn/d5fc8ab35686464fad040392dcbfe83f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/ac65f8cc16184282bc82cc2c17aafe2c.png)
开始上传文件了;
首先准备一个小案例----->>
前端---->>

```bash
<%--
  Created by IntelliJ IDEA.
  User: Gavin
  Date: 2021/12/20
  Time: 19:46
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" %>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="text/javascript"
            src="${pageContext.request.contextPath}/static/js/jquery-3.5.1.min.js">
    </script>
    <!--    给进度条君添加一些样式-->
    <style>
        .progress {
            width: 400px;
            height: 10px;
            border-radius: 10px;
            margin: 10px 0px;
            overflow: hidden;
        }

        .progress > div {
            width: 0px;
            height: 100px;
            background-color: yellowgreen;
            transition: all .3s ease;
        }
    </style>
</head>
<body>
<form method="post" action="addPlayer.do">

    账号:<input type="text" name="name" id="name" placeholder="请输入账号"/>
    <dev>${msg}</dev><!--检查信息-->
    <br>
    密码:<input type="password" name="pwd" id="pwd" placeholder="请输入密码"/>
    <dev>${Info}</dev>
    <br>
    昵称:<input type="text" name="nickname" placeholder="请输入昵称"><br>
    头像:<input type="file" id="upFile" value="选择文件"/>
    <a href="javascript:void(0)" id="uploadFile" onclick="loadPic()">立即上传</a>
    <br>
    <div class="progress">
        <div>
        </div>
    </div>
    <div id="divNum"></div>


    <div id="divimg" style="width: 150px;height: 200px">
        <img id="img" style="width: 140px;height: 180px;" alt="您还未上传图片"/>
    </div>
    <br>
    <input type="submit" value="注册"/>
</form>

<script type="text/javascript">
    function loadPic() {

        var photoFile = $("#upFile")[0].files[0];

        // 将文件传到这个对象中
        var formData = new FormData();

        formData.append("headerPicture", photoFile);

        $.ajax({
                url: "${pageContext.request.contextPath}/fileUpload2.do",
                data: formData,//传送的数据
                type: "post",
                processData: false,//告诉浏览器发送的是一个对象   请求数据
                contentType: false,//告诉浏览器 请求数据的类型 二进制类型
                // dataType: "json",
                success: function (data) {//接收返回来的数据并修改img标签里的内容
                    console.log(data);
                    alert(data.msg);
                    $("#img").attr("src", data.newFileName);
                },
                xhr: function () {
                    var xhr = new XMLHttpRequest();
                    xhr.upload.addEventListener('progress', function (e) {
                        console.log(e);
                        var progressRate = (e.loaded / e.total) * 100 + '%';
                        if (photoFile != null) {
                            $("#divNum").text(progressRate);
                        }
                        if (photoFile != null) {
                            $('.progress > div').css('width', progressRate);
                        }
                    })
                    return xhr;
                }
            }
        );
    }
</script>
</body>
</html>

```
后端---->>

```bash
package com.gavin.controller;




import com.sun.jersey.api.client.Client;
import com.sun.jersey.api.client.WebResource;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;
import org.springframework.web.multipart.MultipartFile;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.File;
import java.io.IOException;
import java.util.HashMap;
import java.util.Map;
import java.util.UUID;

@Controller
public class FileController2 {

    //上传到的文件位置---非本地
    private final static String  FILEDIR="http://127.0.0.1:8090/loadPic/";
    @RequestMapping("/fileUpload2.do")
    @ResponseBody
    public Map<String, String> upDataPicture(MultipartFile headerPicture, HttpServletRequest request) throws IOException {//用MultipartFile接收数据

        Map<String, String> map = new HashMap<>();
//        如果文件不存在
        if (headerPicture==null){
            map.put("msg","请上传图片!");
            return map;
        }
//有文件,则获取文件名
        String originalFilename = headerPicture.getOriginalFilename();
//        控制文件的大小
       if (headerPicture.getSize()>1024*1024*3){
            map.put("msg","文件大小不超过3M");
            return map;
        }
//        避免名字冲突,用UUID替换文件名,但是扩展名不变
        String uuid = UUID.randomUUID().toString();
//        获取拓展名
        String extendsName = originalFilename.substring(originalFilename.lastIndexOf("."));
        if (!(".jpg".equals(extendsName) || ".png".equals(extendsName))) {
            map.put("msg", "文件类型不符合要求");
            return map;
        }
//拼接成新名字
        String newFileName = uuid.concat(extendsName);
        System.out.println(newFileName);

//创建 jersey 包中的client对象,用于跨服务器请求
        Client client= Client.create();
        System.out.println(client);
     WebResource resource = client.resource(FILEDIR + newFileName);
        String result= resource.put(String.class, headerPicture.getBytes());
        System.out.println(result);
        map.put("msg","文件上传成功");
//        返回新生成的文件名
        map.put("newFileName",FILEDIR+newFileName);
        map.put("fileType",headerPicture.getContentType());
        return map;
    }
}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/a18611e844544a2a96cf9df9761131d0.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/7ffc4726637a47c1a7458ea714fb72c4.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)


我们也可以开一个虚拟机,在虚拟机开启一个图片服务器来接收;

为了便于服务器的迁移,我们上传的文件其实没必要写死地址的,
这个可以搞一个配置文件
通过配置文件来读取web地址;


在文件上传中的遇到的几个问题

第一个---->>中文文件名编码问题: 
通过过滤器来解决,如果要记录上传时间还需要通过注解或者配置转换器来解决;


第二个---->>文件存储的位置问题
1,放在当前项目下,作为静态资源,
2,放在别处即单独开一个服务器专门用于处理相应的业务;


第三个----->>分服务器上传---->>
分服务器,实际企业中会用到

    1,数据库服务器：运行我们的数据库
    
    2,缓存和消息服务器：负责处理大并发访问的缓存和消息
    
    3,文件服务器：负责存储用户上传文件的服务器。
    
    4,应用服务器：负责部署我们的应用
    
    总结：分服务器处理的目的是让服务器各司其职，
    从而提高我们项目的运行效率核容错率
![](C:\Users\Gavin\Pictures\Camera Roll\70.png)



> 出于浏览器的同源策略限制。同源策略（Sameoriginpolicy）是一种约定，它是浏览器最核心也最基本的安全功能，如果缺少了同源策略，则浏览器的正常功能可能都会受到影响。可以说Web是构建在同源策略基础之上的，浏览器只是针对同源策略的一种实现。同源策略会阻止一个域的javascript脚本和另外一个域的内容进行交互。所谓同源（即指在同一个域）就是两个页面具有相同的协议（protocol），主机（host）和端口号（port)
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/3be2536a219e47a0b646598d7ca9d5e2.png)

## AJAX跨域请求

下面简单模拟一个场景----->>
前端有一个页面

![在这里插入图片描述](https://img-blog.csdnimg.cn/c2070981ae4a4112815951ad52ecea14.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

鼠标离开用户名输入框时,检查是否符合要求,如果为空,则给提示,如果不为空,则异步查询数据库,后返回结果;

本次请求的页面是8082端口的,而响应的ajax路径却是8080端口的

前端代码--->

```html
<!DOCTYPE html>
<html>
	
<head>
    <title>$Title%sSourceCod</title>
    <meta charset="UTF-8"/>
    <script src="js/jquery-3.5.1.min.js"></script>
    <script>
        function checkUname(){
            // 获取输入框中的内容
            if(null == $("#unameI").val() || '' == $("#unameI").val()){
                $("#unameInfo").text("用户名不能为空");
                return;
            }
            $("#unameInfo").text("");
            // 通过jQuery.ajax() 发送异步请求
            $.ajax(
                    {
                        type:"GET",// 请求的方式 GET  POST
                        url:"http://localhost:8080/loadPicture_war_exploded/checkName.do?", // 请求的后台服务的路径
                        data:"uname="+$("#unameI").val(),// 提交的参数
                        success:function(info){ // 响应成功执行的函数
                            $("#unameInfo").text(info)
                        }
                    }
            )
        }
    </script>
</head>
<body>
<form action="myServlet1.do" >
    用户名:<input id="unameI" type="text" name="uname" onblur="checkUname()">
    <span id="unameInfo" style="color: red"></span><br/>
    密码:<input type="password" name="pwd"><br/>
    <input type="submit" value="提交按钮">
</form>
</body>
</html>

```
后端代码--->

```java
package com.gavin.controller;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebServlet("/checkName.do")
public class testAjax extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String uname = req.getParameter("uname");
        String callBack = req.getParameter("aaa");
        System.out.println(uname);
        String info = "";
        if ("gavin".equals(uname)) {
            info = "用户名已经占用";
        } else {
            info = "用户名可用";
        }
        // 向浏览器响应数据
        resp.setCharacterEncoding("UTF-8");
        resp.setContentType("text/javaScript;charset=UTF-8");
        resp.getWriter().print(callBack + "('" + info + "')");
    }
}
```
访问的时候浏览器提示--->
![在这里插入图片描述](https://img-blog.csdnimg.cn/915c22dfa1514533a5f22dec152ee113.png)
原因--->

```
对 CORS 请求的响应缺少必需的Access-Control-Allow-Origin头，其用于确定在当前源内操作的资源是否可以访问。

如果服务器在您的控制之下，请将请求站点的源添加到允许访问的域集，方法是将其添加到Access-Control-Allow-Origin头的值。
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/d78824c05b8d4172aea8b8cc7ca02def.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
为什么会有跨域呢?
因为实际应用中分布式与集群会涉及到跨域,前端服务器与后端服务器分离,前端服务异步请求后端服务器就涉及到了跨域;


由于浏览器的同源策略,所以跨服务器访问会有一些小麻烦,先一步一步探索去解决;
![在这里插入图片描述](https://img-blog.csdnimg.cn/a2fc4b75c3214e2ca0d3308ffd01893f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
这个时候js文件能不能加载生效呢?  答案是生效了;
![在这里插入图片描述](https://img-blog.csdnimg.cn/6ad7f302bd874281a4fc4992cd231a4b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
Web页面上调用js文件时可以跨域,也就是后拥有”src”这个属性的标签都却拥有跨域的能力

那么我们转变思路,如果将异步请求转到js文件身上
比如我们可以这么做
![在这里插入图片描述](https://img-blog.csdnimg.cn/5061a5ba6d064d6c841dfb379736865a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
后端可以接收到前端数据;
但是这样写看起来怪怪的,而且实际上这样异步请求中的url依然会被浏览器拦截
![在这里插入图片描述](https://img-blog.csdnimg.cn/83856299d0a84e07923148c09d3a1c1e.png)
如果去掉这个url,会发生不可描述的事情,像这样----整个span被页面代码填满,
![在这里插入图片描述](https://img-blog.csdnimg.cn/c4f4cc1124a54611a2e4d25d044049c4.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
那怎么处理呢?


此时异步请求添加一个属性---dataType:"jsonp"
这样就可以正常一点实现跨域的异步请求了--->

```html
        function checkUname(){
            // 获取输入框中的内容
            if(null == $("#unameI").val() || '' == $("#unameI").val()){
                $("#unameInfo").text("用户名不能为空");
                return;
            }
            $("#unameInfo").text("");
            // 通过jQuery.ajax() 发送异步请求
            $.ajax(
                    {
                        type:"GET",// 请求的方式 GET  POST
                        url:"http://localhost:8080/loadPicture_war_exploded/checkName.do?", // 请求的后台服务的路径
                        data:"uname="+$("#unameI").val(),// 提交的参数
                        dataType:"jsonp",
                        success:function(info){ // 响应成功执行的函数
                            $("#unameInfo").text(info)
                        }
                    }
            )
        }
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/9b9d513701f544e0ab7319f3918dbc4b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

![在这里插入图片描述](https://img-blog.csdnimg.cn/ba9ff608467f4b239dc1bc7f65f50245.png)

原因---->>
![在这里插入图片描述](https://img-blog.csdnimg.cn/9c027c1687e24f5e9f916470da46c159.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

虽然跨域请求实现了,但是前端接收不到后端返回的数据,即异步 请求中的success方法失效了,
为什么失效?因为如果是通过script来完成异步请求,那么返回的内容应该是一个js代码,
![在这里插入图片描述](https://img-blog.csdnimg.cn/18bbba266a1148b6849a6fcef3d6c88e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/4539249a5f5a461b85dbc8a9f817b047.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

既然是这样,我们要想在span中添加返回类的信息,那么我们不妨在前端写一个方法,用于专门像span中添加信息---然后后端返回的信息来直接调这个方法就好了;

```html
			function showInfo(info){
				$("#unameInfo").val(info);
			}
```
后端返回的数据
![在这里插入图片描述](https://img-blog.csdnimg.cn/0a128b41f7314a3ca8ec644075387e0a.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/78ad8cf645334eca80042fd67b0d307d.png)
运行原理------>>>>
![在这里插入图片描述](https://img-blog.csdnimg.cn/d29764e9fd9241a1b47d2f98e8200efc.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
解决一个小痛点,前端方法名随时可能变化,为了降低耦合度,一般会这么做,前端发送的数据中携带该方法名;

![在这里插入图片描述](https://img-blog.csdnimg.cn/4d9a227913ac451b9b953efd6dafb889.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/e3f37a5b90ec4fdfb0fb88c1a2ddb2b8.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
前面不是说success废了吗?我就想用这个方法,不想在额外定义一个别的showInfo方法,那么这个该怎么做呢?
在异步请求上添加一个参数:
jsonp:"任意的名称A"


![在这里插入图片描述](https://img-blog.csdnimg.cn/3d67789030b3470f9edcd147f0fc2126.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/854069c0153f46df8c16af8a8c20b2de.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

## GetJson实现跨域请求

```html
	        function checkUname(){
            // 获取输入框中的内容
            if(null == $("#unameI").val() || '' == $("#unameI").val()){
                $("#unameInfo").text("用户名不能为空");
                return;
            }
            $("#unameInfo").text("");
         
           $.getJSON(
           	"http://localhost:8080/loadPicture_war_exploded/checkName.do?jsoncallback=?",
           	{uname:$("#unameI").val()},
           	function(info){
           		$("#unameInfo").text(info)
           	}
           	
           )
        }
        
        
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/68f8429cbba4493ca35939f97e2a40d4.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)

> 通过后台代码也可以实现跨域,一般在过滤器中添加如下代码,那么前端在请求时就不用考虑跨域问题了

   /*请求地址白名单 *代表所有    */
  resp.setHeader("Access-Control-Allow-Origin", "*");
  /*请求方式白名单      */
  resp.setHeader("Access-Control-Allow-Methods", "POST, GET, OPTIONS, DELETE");
  resp.setHeader("Access-Control-Max-Age", "3600");
  resp.setHeader("Access-Control-Allow-Headers", "x-requested-with");



在结合springmvc之后,可以通过一个注解来完成跨域

## CrossOrigin注解实现跨域
![在这里插入图片描述](https://img-blog.csdnimg.cn/446d2c7ab2b3424a81b7b9f66f59c55b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)![在这里插入图片描述](https://img-blog.csdnimg.cn/72c9be7991b9478fba0d4849a4927b4f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)



```java
package com.gavin.controller;

import com.gavin.pojo.AlertMsg;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.*;

@Controller
public class AjaxTEST  {


    @CrossOrigin(value ="http://127.0.0.1:8020" )
  @RequestMapping("/checkName3.do")
    @ResponseBody
    public AlertMsg checkUserName(@RequestBody AlertMsg alertMsg)  {
        System.out.println(alertMsg);
        if ("gavin".equals(alertMsg.getUname())) {
            alertMsg.setMsg("用户名已经占用");
        } else {
            alertMsg.setMsg( "用户名可用");
        }
        // 向浏览器响应数据----返回一个json格式的数据
        System.out.println(alertMsg);
        return alertMsg;

    }
}
```

```html
			function checkUname3() {
				// 获取输入框中的内容
				if(null == $("#unameI3").val() || '' == $("#unameI3").val()) {
					$("#unameInfo3").text("用户名不能为空");
					return;
				}
				$("#unameInfo3").text("");
				var str = $("#unameI3").val();

				$.ajax({
					type: "post",
					url: "http://localhost:8080/loadPicture_war_exploded/checkName3.do?",
					//					data:{uname:$("#unameI3").val(),msg:""},
					//data:{"uname":$("#unameI3").val(),"msg":""},
					data: JSON.stringify({
						uname: $("#unameI3").val()

					}),
					dataType: "json",
					contentType: "application/json;charset=UTF-8",
					success: function(data) {

						$("#unameInfo3").text(data.msg);
					}
				});
			}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/dd8c8d140cf34670961f57bdcaf14b35.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/45f72a355c7040d8b6b3a8cd0ff53b98.png)

关于json对象的一些感悟与理解；
前端传过来的数据----可能是字符串，也可能是json对象，但是在处理的时候还是以字符串进行处理的，
JSON.stringify()方法是将一个JavaScript对象转换成符合JSON格式的字符串，然后后端通过解析字符串在转化为一个json对象；
所以![在这里插入图片描述](https://img-blog.csdnimg.cn/680dffa8dc704d3792f682b4fdff5253.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR2F2aW5fTGlt,size_20,color_FFFFFF,t_70,g_se,x_16)
ajax跨域的解决方案有种了，
第一种是 jsonp的形式，
另一种是getjson（）
最后一种是注解CrossOrigin

<center><b>更多Linux咨询请扫描下方二维码,回复"linux"获取</b><center>

![](..\pic\div\weichatcode.png)

