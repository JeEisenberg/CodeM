![在这里插入图片描述](https://img-blog.csdnimg.cn/07f3bf6d187d4750a0c03405ca3bc11a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

# Nginx

## Niginx简介

>Nginx ("engine x") 是一个高性能的 HTTP 和反向代理服务器，也是一个 IMAP/POP3/SMTP 代理服务器。

![在这里插入图片描述](https://img-blog.csdnimg.cn/a8aa0746334645bd9a9b7cd9a5a97e20.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)由于FastDFS没有文件范文的功能,需要借助其他工具来实现图片的HTTP访问,Nginx就具备代理虚拟机主机功能。


**正向代理**
![在这里插入图片描述](https://img-blog.csdnimg.cn/f267586fd7c34ef8a0bc532af2342123.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
正向代理图解~

![](https://img-blog.csdnimg.cn/17ad93a03a6942138862bfa8a004272b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_19,color_FFFFFF,t_70,g_se,x_16)
**正向代理的应用**

 - 访问原来无法访问的资源

 - 用作缓存，加速访问速度

 - 对客户端访问授权，上网进行认证

 - 代理可以记录用户访问记录（上网行为管理），对外隐藏用户信息


>典型的正向代理是一种最终用户知道并主动使用的代理方式

**反向代理**

![在这里插入图片描述](https://img-blog.csdnimg.cn/adc555f6bc524e5089223028b079c06e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)反向代理图解~

![在这里插入图片描述](https://img-blog.csdnimg.cn/bc55d2f880f34f15ab7a5e41f906e925.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
**反向代理的应用**

 - 保护内网安全
 - 负载均衡
 - 缓存，减少服务器的压力


**正向代理和反向代理的区别**

 - **所处的位置不同**

正向代理，架设在客户机和目标主机之间；

反向代理，架设在服务器端；

 - **所代理的对象不同**

正向代理，代理客户端，服务端不知道实际发起请求的客户端；

反向代理，代理服务端，客户端不知道实际提供服务的服务端；

 - **用途不同**

正向代理，为在防火墙内的局域网客户端提供访问Internet的途径；

反向代理，将防火墙后面的服务器提供给Internet访问；

## nginx的作用

  1. 提供http协议代理
    只要支持http协议访问的内容都可以使用nginx代理;
  2. 搭建虚拟主机
    监听主机的某个端口,对外支持访问这个接口的http访问
  3. 负载均衡
    nginx可以代理多个主机,内置负载均衡策略

![在这里插入图片描述](https://img-blog.csdnimg.cn/fb976dc06c3e4417a747ad4e53117454.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)![在这里插入图片描述](https://img-blog.csdnimg.cn/16f69a284b31416da82820b5277b5be1.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
3、网络缓存
Nginx可以对**不同的文件**做**不同的缓存处理**，配置灵活，并且支持FastCGI_Cache，主要用于对FastCGI的动态程序进行缓存。配合着第三方的ngx_cache_purge，对制定的URL缓存内容可以的进行增删管理。


## Nginx的安装
安装之前可以先查看一下~[fastdfs的安装与使用](https://blog.csdn.net/weixin_54061333/article/details/123119546)

[下载fastdfs的nginx安装模块](https://sourceforge.net/projects/fastdfs/files/FastDFS%20Nginx%20Module%20Source%20Code/)

![在这里插入图片描述](https://img-blog.csdnimg.cn/d2a23583f22d4032bcce939b70744b28.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

### fastdfs-nginx-module安装

安装步骤~

  1. 进入安装目录~ `cd /usr/local`
  2. rz命令 上传下载好的fastdfs~nginx模块
>安装`yum -y install lrzsz`    **rz~上传到linux** |   **sz~从linux下载**
  4. 解压~ `tar -zxf fastdfs-nginx-module_v1.16.tar.gz` 
  5. 配置~ `vi fastdfs-nginx-module/src/config`

修改为如下配置~
![在这里插入图片描述](https://img-blog.csdnimg.cn/caab3fa15a1d4b5d9b6c9ec93496c56d.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)其中CORE_INCS 是nginx在什么位置查找fastdfs核心代码的路径,这个要跟之前的配置相符
![在这里插入图片描述](https://img-blog.csdnimg.cn/f0494e0a8a594e8b9369e907e8347343.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)其他配置项默认即可; 
>CORE_LIBS是C函数库,由于之前创建了软连接,所以不用修改这个,

以上就是fastdfs-nginx模块的配置

### Nginx的安装步骤
 **依赖的安装**

>同fastdfs一样,需要安装c函数库
>
  1. 安装nginx依赖~

`yum install -y gcc gcc-c++ make automake autoconf libtool pcre pcre-devel zlib zlib-devel openssl openssl-devel`
![在这里插入图片描述](https://img-blog.csdnimg.cn/49e64999c17f47cd96f34793df5c79e1.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)           [下载nginx稳定版](https://nginx.org/en/download.html)

  2. 上传Nginx到安装目录~  `cd /usr/local`  rz 上传文件
  3. 解压~ `tar -zxf nginx-1.20.2.tar.gz` 
  4. 新建文件夹~ `mkdir -p /var/temp/nginx`
  5. 修改配置文件参数~此时依旧在nginx安装目录文件夹下
    执行下面的命令:

```c
./configure \
--prefix=/usr/local/nginx \
--pid-path=/var/run/nginx/nginx.pid \
--lock-path=/var/lock/nginx.lock \
--error-log-path=/var/log/nginx/error.log \
--http-log-path=/var/log/nginx/access.log \
--with-http_gzip_static_module \
--http-client-body-temp-path=/var/temp/nginx/client \
--http-proxy-temp-path=/var/temp/nginx/proxy \
--http-fastcgi-temp-path=/var/temp/nginx/fastcgi \
--http-uwsgi-temp-path=/var/temp/nginx/uwsgi \
--http-scgi-temp-path=/var/temp/nginx/scgi \
--add-module=/usr/local/fastdfs-nginx-module/src
```
注意~
>--add-module=/usr/local/fastdfs-nginx-module/src的路径要匹配本地安装的fastdfs-nginx-module路径;
>--add-module 必须定义，此配置信息是用于指定安装Nginx时需要加载的模块，如果未指定，Nginx安装过程不会加载fastdfs-nginx-module模块，后续功能无法实现。


安装成功截图~
![在这里插入图片描述](https://img-blog.csdnimg.cn/84efb83bf6fa4025852f593aae738802.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
  **编译与安装装nginx**

  1. 编译nginx ~`make` 
    ![在这里插入图片描述](https://img-blog.csdnimg.cn/5c04d1bd82cb4fbe9fad454f6148e4f7.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
  2. 安装nginx~ `make install`
    ![在这里插入图片描述](https://img-blog.csdnimg.cn/9bfee5b300754ce895d33d7274d13fdb.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
  3. 配置fastfds-nginx-module
    将该模块配置文件复制到/etc/fdfs文件夹下~`cp /usr/local/fastdfs-nginx-module/src/mod_fastdfs.conf  /etc/fdfs/`
  4. 修改fastfds-nginx-module配置文件
    `cd /etc/fdfs/` 然后`vim mod_fastdfs.conf` 
    修改配置~

```c
# connect timeout in seconds
# default value is 30s
connect_timeout=10

# FastDFS tracker_server can ocur more than once, and tracker_server format is
#  "host:port", host can be hostname or ip address
# valid only when load_fdfs_parameters_from_tracker is true
tracker_server=192.168.135.145:22122

# if the url / uri including the group name
# set to false when uri like /M00/00/00/xxx
# set to true when uri like ${group_name}/M00/00/00/xxx, such as group1/M00/xxx
# default value is false
url_have_group_name = true

# store_path#, based 0, if store_path0 not exists, it's value is base_path
# the paths must be exist
# must same as storage.conf
store_path0=/usr/local/FastDFS/storage/store

```

  5. 提供FastDFS需要的http配置文件

```c
[root@localhost conf]#  cp /usr/local/FastDFS/conf/http.conf  /etc/fdfs/
[root@localhost conf]# cp /usr/local/FastDFS/conf/mime.types  /etc/fdfs/
```

  6. 创建网络服务存储软连接~`ln -s /usr/local/FastDFS/storage/store/data/ /usr/local/FastDFS/storage/store/data/M00`

将虚拟目录定位到真实数据目录上
>在FastDFS上传文件后生成对应的数据存储ID,该ID包含组名,由于配置时已经配置了解析group的配置
>![在这里插入图片描述](https://img-blog.csdnimg.cn/26479960e236460ab52578e82ffc66de.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)
>但是按照真实的目录结构并不存在M00这个文件夹,所以要将M00映射成为真实的文件夹
>/usr/local/FastDFS/storage/store/data/  (因为该文件夹下对应的是256*256跟文件夹)

![在这里插入图片描述](https://img-blog.csdnimg.cn/91efaf89e75f47f3802118c9defdda87.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

  7. 修改nginx配置文件
    进入nginx安装目录 `cd nginx-1.20.2/conf`
    修改配置文件~将nginx的权限设为root权限,避免文件无法访问的问题
    ![在这里插入图片描述](https://img-blog.csdnimg.cn/be87449b0f26473c91e7755673b2788b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)监听8888端口,这里要跟storage.conf中的配置端口一样

![在这里插入图片描述](https://img-blog.csdnimg.cn/2327e3fce5ee4753a119468614111663.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQ29kZU1hcnRhaW4=,size_20,color_FFFFFF,t_70,g_se,x_16)

storage.conf中的配置端口
![在这里插入图片描述](https://img-blog.csdnimg.cn/f8180809ccff498d947ffc24511bb118.png)

nginx配置的文件结构~

```c
#全局块：配置影响**nginx全局的指令**。一般有运行nginx服务器的用户组，nginx进程pid存放路径，日志存放路径，配置文件引入，允许生成worker process数等。
user  root;
worker_processes  1;
#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;
#pid        logs/nginx.pid;

# events块：配置影响**nginx服务器或与用户的网络连接**。有每个进程的最大连接数，选取哪种事件驱动模型处理连接请求，是否允许同时接受多个网路连接，开启多个网络连接序列化等。

events {         #events块
   ...
}

#http块：可以**嵌套多个服务器**，配置代理，缓存，日志定义等绝大多数功能和第三方模块的配置。如文件引入，mime-type定义，日志自定义，是否使用sendfile传输文件，连接超时时间，单连接请求数等。
http      #http块
{
    ...   #http全局块
#    server块：**配置虚拟主机的相关参数**，一个http中可以有多个服务器。
    server        #server块
    { 
        ...       #server全局块
#        location块：**配置请求的路由**，以及各种页面的处理情况。
        location [PATTERN]   #location块
        {
            ...
        }
        location [PATTERN] 
        {
            ...
        }
    }
    server
    {
      ...
    }
    ...     #http全局块
}
```

**nginx的基本配置~参考**

```c
########### 每个指令必须有分号结束。#################
#user administrator administrators;  #配置用户或者组，默认为nobody nobody。
#worker_processes 2;  #允许生成的进程数，默认为1
#pid /nginx/pid/nginx.pid;   #指定nginx进程运行文件存放地址
error_log log/error.log debug;  #制定日志路径，级别。这个设置可以放入全局块，http块，server块，级别以此为：debug|info|notice|warn|error|crit|alert|emerg
events {
    accept_mutex on;   #设置网路连接序列化，防止惊群现象发生，默认为on
    multi_accept on;  #设置一个进程是否同时接受多个网络连接，默认为off
    #use epoll;      #事件驱动模型，select|poll|kqueue|epoll|resig|/dev/poll|eventport
    worker_connections  1024;    #最大连接数，默认为512
}
http {
    include       mime.types;   #文件扩展名与文件类型映射表
    default_type  application/octet-stream; #默认文件类型，默认为text/plain
    #access_log off; #取消服务日志    
    log_format myFormat '$remote_addr–$remote_user [$time_local] $request $status $body_bytes_sent $http_referer $http_user_agent $http_x_forwarded_for'; #自定义格式
    access_log log/access.log myFormat;  #combined为日志格式的默认值
    sendfile on;   #允许sendfile方式传输文件，默认为off，可以在http块，server块，location块。
    sendfile_max_chunk 100k;  #每个进程每次调用传输数量不能大于设定的值，默认为0，即不设上限。
    keepalive_timeout 65;  #连接超时时间，默认为75s，可以在http，server，location块。

    upstream mysvr {   
      server 127.0.0.1:7878;
      server 192.168.10.121:3333 backup;  #热备
    }
    error_page 404 https://www.baidu.com; #错误页
    server {
        keepalive_requests 120; #单连接请求上限次数。
        listen       4545;   #监听端口
        server_name  127.0.0.1;   #监听地址       
        location  ~*^.+$ {       #请求的url过滤，正则匹配，~为区分大小写，~*为不区分大小写。
           #root path;  #根目录
           #index vv.txt;  #设置默认页
           proxy_pass  http://mysvr;  #请求转向mysvr 定义的服务器列表
           deny 127.0.0.1;  #拒绝的ip
           allow 172.18.5.54; #允许的ip           
        } 
    }
}
```
**几个常见配置项：**

> 1.$remote_addr 与 $http_x_forwarded_for 用以记录客户端的ip地址;
>
> 2.$remote_user ：用来记录客户端用户名称;
>
> 3.$time_local ： 用来记录访问时间与时区;
>
> 4.$request ： 用来记录请求的url与http协议;
>
> 5.$status ： 用来记录请求状态;成功是200;
>
> 6.$body_bytes_s ent ：记录发送给客户端文件主体内容大小;
>
> 7.$http_referer ：用来记录从那个页面链接访问过来的;
>
> 8.$http_user_agent ：记录客户端浏览器的相关信息;

## 启动nginx
进入nginx安装目录下的sbin~`cd /usr/local/nginx/sbin`
启动nginx~ `./nginx`

![在这里插入图片描述](https://img-blog.csdnimg.cn/cf48da9b1cfe4999a6d4a5e6c2c1d36e.png)

## Nginx历史版本变化

<div  align="center" ><iframe   style="width: 648px; height: 502px;" src="https://static-fc7496ff-74c7-4252-870a-6687bb16a468.bspapp.com "></iframe></div>

![](..\pic\div\plant.gif)