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
