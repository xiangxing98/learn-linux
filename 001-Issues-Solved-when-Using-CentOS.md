# 001-Issues-Solved-when-Using-CentOS.md

## Issue001-初始化失败、初始化软件包后端失败、yum-complete-transaction解决方法
直接在终端中输入：“yum-complete-transaction”命令会提示：
bash: yum-complete-transaction: command not found。
在网上搜寻很久后，尝试了多种解决方法，最终的解决方法如下
解决方法：
1、打开终端，su切换到root权限，并输入命令如下：
`yum install yum-utils`
2、根据1中提示，一路‘y’下去，完成安装。
3、输入问题中提示的“yum-complete-transaction”命令如下：

## database disk image is malformed
在执行yum更新，即运行 yum update 命令时，提示以下错误：
Error:database disk image is malformed
估计是由于yum的原数据损坏导致的，与rpm的数据库损坏类似，前者会导致更新不能正常执行，后者会导致安装失败并出现乱码，前者的解决参见yum更新和rpm安装包问题(rpmdb: PANIC: Invalid argument)，后者的错误可以通过一下方法解决：
终端，依次输入：
```
yum clean metadata
yum clean dbcache
yum makecache
```
即先删除原数据和数据库缓存，然后重建之，问题即可解决
OR below code is also ok
`yum clean all` 

## Add centOS repository-yum添加网易/搜狐源
先进入yum源配置目录
`cd /etc/yum.repos.d`
备份系统自带的yum源
`mv CentOS-Base.repo CentOS-Base.repo.save`
163的yum源：
`wget http://mirrors.163.com/.help/CentOS-Base-163.repo`
sohu的yum源：
`wget http://mirrors.sohu.com/help/CentOS-Base-sohu.repo`
http://mirrors.sohu.com/help/CentOS-Base-sohu.repo
下载CentOS-Base-sohu.repo, 放入/etc/yum.repos.d/
运行yum makecache生成缓存
更新玩yum源后，建议更新一下，使操作立即生效
`yum makecache`
 
## 为CentOS安装EPEL软件仓库
EPEL全称: Extra Packages for Enterprise Linux.传说中最全的yum源
通过以下命令安装:
CentOS 6.x 32-bit
`rpm -Uvh http://mirror.overthewire.com.au/pub/epel/6/i386/epel-release-6-7.noarch.rpm`
CentOS 6.x 64-bit
`rpm -Uvh http://download.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-7.noarch.rpm`
CentOS 5.x 32-bit
`rpm -Uvh http://dl.fedoraproject.org/pub/epel/5/i386/epel-release-5-4.noarch.rpm`
CentOS 5.x 64-bit
`rpm -Uvh http://dl.fedoraproject.org/pub/epel/5/x86_64/epel-release-5-4.noarch.rpm`
想暂停使用EPEL，在下面的文件中设置enabled=0即可.
`vim /etc/yum.repos.d/epel.repo`

## Add centOS repository-
参考 http://mirrors.163.com/.help/centos.html 和 http://mirrors.sohu.com/help/centos.html 中的介绍。
设置方法如下：
1，进入yum源配置目录
`cd /etc/yum.repos.d`
2，备份系统自带的yum源
`mv CentOS-Base.repo CentOS-Base.repo.bak`
下载163网易的yum源：
`wget http://mirrors.163.com/.help/CentOS6-Base-163.repo`
更改文件名
`mv CentOS6-Base-163.repo CentOS-Base.repo`

3，更新玩yum源后，执行下边命令更新yum配置，使操作立即生效
```
yum clean all
yum makecache
```

4，除了网易之外，国内还有其他不错的yum源，比如搜狐的
sohu的yum源
`wget http://mirrors.sohu.com/help/CentOS-Base-sohu.repo`

但是搜狐的好像截止到笔者发布此文章时，还没有centos6的yum源。
中科大的
`wget http://lug.ustc.edu.cn/wiki/_export/code/mirrors/help/centos?codeblock=2`

## CentOS6.3安装VLC media player
2012/12/25Linux运维centos、Linux、redhatbear	
VLC media player是Linux系统里一个很受欢迎的视频播放器，在Ubuntu软件中心里，这款播放器的下载量非常巨大，可见其受欢迎的程度。下面是在CentOS6.3系统安装VLC media player的过程。
```
su - root
cd /etc/yum.repos.d/
wget http://pkgrepo.linuxtech.net/el6/release/linuxtech.repo
yum --enablerepo=linuxtech-testing install vlc
```
