# Centos下删除系统多余旧内核的命令与方法

### Source From: 
[http://www.myzhenai.com/thread-15671-1-1.html(]http://www.myzhenai.com/thread-15671-1-1.html)
[http://www.myzhenai.com.cn/post/1170.html](http://www.myzhenai.com.cn/post/1170.html)

Tags: centos, kernel, linux, remove, ubuntu
关键字:centos linux kernel 内核 命令 方法

Centos升级的时候会多余出一些旧的内核,这些是可以删除节省系统空间的.但是linux下不同版本的系统删除系统多余旧内核的名令还不一样.这点是需要了解的,Ubuntu和Centos查询内核与删除内核的方法也是不一样的.

```
uname -r
//*先查询当前linux内核版本.这个命令是通用的.
2.6.32-504.8.1.el6.i686
rpm -q kernel
//*列出当前所有linux内核版本信息.
kernel-2.6.32-431.29.2.el6.i686
kernel-2.6.32-504.3.3.el6.i686
kernel-2.6.32-504.8.1.el6.i686
kernel-2.6.32-504.16.2.el6.i686
//*Centos下删除linux内核的命令.

yum remove kernel
//*删除当前linux内核版本外的其他所有linux内核版本.
yum remove kernel-2.6.32-431.29.2.el6.i686
yum remove kernel-2.6.32-504.16.2.el6.i686

#apt-get remove linux内核版本文件
//*这是Ubuntu下删除linux内核版本文件的命令.带有版本号的linux内核文件才是可以删除的,其他不带版本号的linux内核文件是不能删除的,会出问题.切记
//当然,您也可以一个一个的删除,这是为保险起见. 例如我的多余的旧linux系统内核.
#yum remove kernel-2.6.32-358.0.1.el6.i686
#yum remove kernel-2.6.32-358.2.1.el6.i686
#yum remove kernel-2.6.32-358.6.1.el6.i686
#yum remove kernel-2.6.32-358.6.2.el6.i686
```

[RucLinux@localhost ~]$ `su root`
密码：
[root@localhost RucLinux]# `rpm -q kernel`
kernel-2.6.32-358.0.1.el6.i686
kernel-2.6.32-358.2.1.el6.i686
kernel-2.6.32-358.6.1.el6.i686
kernel-2.6.32-358.6.2.el6.i686
kernel-2.6.32-358.11.1.el6.i686
[root@localhost RucLinux]# `yum remove kernel`
Loaded plugins: fastestmirror, refresh-packagekit, security
Setting up Remove Process
Skipping the running kernel: kernel-2.6.32-358.11.1.el6.i686
Resolving Dependencies
--> Running transaction check
---> Package kernel.i686 0:2.6.32-358.0.1.el6 will be erased
---> Package kernel.i686 0:2.6.32-358.2.1.el6 will be erased
---> Package kernel.i686 0:2.6.32-358.6.1.el6 will be erased
---> Package kernel.i686 0:2.6.32-358.6.2.el6 will be erased
--> Finished Dependency Resolution

Dependencies Resolved

================================================================================
 Package        Arch         Version                     Repository        Size
================================================================================
Removing:
 kernel         i686         2.6.32-358.0.1.el6          @updates          85 M
 kernel         i686         2.6.32-358.2.1.el6          @updates          85 M
 kernel         i686         2.6.32-358.6.1.el6          @updates          85 M
 kernel         i686         2.6.32-358.6.2.el6          @updates          85 M

Transaction Summary
================================================================================
Remove        4 Package(s)

Installed size: 341 M
Is this ok [y/N]: y
Downloading Packages:
Running rpm_check_debug
Running Transaction Test
Transaction Test Succeeded
Running Transaction
  Erasing    : kernel.i686                                                  1/4 
  Erasing    : kernel.i686                                                  2/4 
  Erasing    : kernel.i686                                                  3/4 
  Erasing    : kernel.i686                                                  4/4 
  Verifying  : kernel-2.6.32-358.0.1.el6.i686                               1/4 
  Verifying  : kernel-2.6.32-358.6.2.el6.i686                               2/4 
  Verifying  : kernel-2.6.32-358.2.1.el6.i686                               3/4 
  Verifying  : kernel-2.6.32-358.6.1.el6.i686                               4/4 

Removed:
  kernel.i686 0:2.6.32-358.0.1.el6       kernel.i686 0:2.6.32-358.2.1.el6      
  kernel.i686 0:2.6.32-358.6.1.el6       kernel.i686 0:2.6.32-358.6.2.el6      

Complete!
[root@localhost RucLinux]#


## 第一种方法 Procedure 
1. 查看系统当前内核版本:
```
uname -a
Linux localhost 2.6.18-274.18.1.el5 #1 SMP Thu Feb 9 12:45:52 EST 2012 i686 i686 i386 GNU/Linux
```
2. 查看系统中全部的内核RPM包:
```
rpm -qa | grep kernel
kernel-devel-2.6.18-194.el5
kernel-devel-2.6.18-274.18.1.el5
kernel-headers-2.6.18-274.18.1.el5
kernel-2.6.18-194.el5
kernel-2.6.18-274.18.1.el5
```

3. 删除旧内核的RPM包
```
yum remove kernel-2.6.18-194.el5
yum remove kernel-devel-2.6.18-194.el5
```

4. 重启系统
```
reboot
#注意:不需要手动修改/boot/grub/menu.lst
```
## 第二种方法
 手动修改/boot/grub/menu.lst   把多余的项删除。
 
