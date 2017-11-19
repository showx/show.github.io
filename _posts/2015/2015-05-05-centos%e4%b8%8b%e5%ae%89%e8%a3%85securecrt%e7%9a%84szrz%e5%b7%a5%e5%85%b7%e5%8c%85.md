---
layout: post
title: CentOS下安装SecureCRT的sz/rz工具包
date: 2015-05-05 15:40
author: admin
comments: true
categories: []
---
yum自动安装：

yum install lrzsz

手动安装方法如下：

定制安装的linux可能没有把rzsz包安装到系统，这对用securecrt这样的windows工具传输文件特别不方便。为了使用这个方便的法门，可以手动安装之。

1、 下载软件 rzsz-3.48.tar.gz。登录linux，用命令

wget http://freeware.sgi.com/source/rzsz/rzsz-3.48.tar.gz下载。

2、解压 tar zxvf rzsz-3.48.tar.gz

3、安装 cd rzsz-3.48 ; make posix 。注意：这个软件安装与常规的GNU软件不同—没有configure(配置)及make install (安装过程)。先执行命令make,看下Makefile里面的参数。

<a title="CentOS" href="http://www.linuxidc.com/topicnews.aspx?tid=14" target="_blank">CentOS</a>下选择选posix或linux就 可以了。

我make了一下显示：-bash: make: command not found。

貌似CentOS下默认系统没有安装编译器，SSH下安装一下：yum -y install gcc automake autoconf libtool make

查看源代码打印帮助01 [root@miao src]#make posix

02 cc   -O -DPOSIX -DMD=2 rz.c -o rz

03 size rz

04  text    data     bss     dec     hex filename

05  25444     648   10464   36556    8ecc rz

06 rm -f rb rx rc

07 ln rz rb

08 ln rz rx

09 ln rz rc

10 cc   -O -DPOSIX sz.c -o sz

11 size sz

12  text    data     bss     dec     hex filename

13  30565     708   43072   74345   12269 sz

14 rm -f sb sx zcommand zcommandi

15 ln sz sb

16 ln sz sx

17 ln sz zcommand

18 ln sz zcommandi

19 [root@miao src]#

为了方便使用这个工具，把相关文件复制到目录/usr/bin下面。这里只需要拷贝2个文件rz及sz,命令为：cp rz sz /usr/bin
