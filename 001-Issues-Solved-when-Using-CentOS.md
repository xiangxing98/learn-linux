001-Issues-Solved-when-Using-CentOS.md

Issue001-初始化失败、初始化软件包后端失败、yum-complete-transaction解决方法
直接在终端中输入：“yum-complete-transaction”命令会提示：
bash: yum-complete-transaction: command not found。
在网上搜寻很久后，尝试了多种解决方法，最终的解决方法如下
解决方法：
1、打开终端，su切换到root权限，并输入命令如下：
yum install yum-utils
2、根据1中提示，一路‘y’下去，完成安装。
3、输入问题中提示的“yum-complete-transaction”命令如下：
