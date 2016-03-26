# User and Previlege Control in Ubuntu.md
============================================================
## ubuntu下查看权限的命令为：

```
ls -l filename
```

ubuntu下权限一共有10位数
其中： 最前面那个 - 代表的是类型
中间那三个 rw- 代表的是所有者（user）
然后那三个 rw- 代表的是组群（group）
最后那三个 r-- 代表的是其他人（other）

然后我再解释一下后面那9位数：
r 表示文件可以被读（read）
w 表示文件可以被写（write）
x 表示文件可以被执行（如果它是程序的话）
- 表示相应的权限还没有被授予

## 现在该说说修改文件权限了

在终端输入：
```
chmod o+w xxx.xxx
```
表示给其他人授予写xxx.xxx这个文件的权限
```
chmod go-rw xxx.xxx
```
表示删除xxx.xxx中组群和其他人的读和写的权限

其中：
u 代表所有者（user）
g 代表所有者所在的组群（group）
o 代表其他人，但不是u和g （other）
a 代表全部的人，也就是包括u，g和o
r 表示文件可以被读（read）,可以用数字来代替, r ------------4
w 表示文件可以被写（write）,可以用数字来代替, w -----------2
x 表示文件可以被执行（如果它是程序的话）,可以用数字来代替, x ------------1
- ------------0 表示相关权限还没有配置

行动：
+ 表示添加权限
- 表示删除权限
= 表示使之成为唯一的权限

当大家都明白了上面的东西之后，那么我们常见的以下的一些权限就很容易都明白了：
-rw------- (600) 只有所有者才有读和写的权限
-rw-r--r-- (644) 只有所有者才有读和写的权限，组群和其他人只有读的权限
-rwx------ (700) 只有所有者才有读，写，执行的权限
-rwxr-xr-x (755) 只有所有者才有读，写，执行的权限，组群和其他人只有读和执行的权限
-rwx--x--x (711) 只有所有者才有读，写，执行的权限，组群和其他人只有执行的权限
-rw-rw-rw- (666) 每个人都有读写的权限
-rwxrwxrwx (777) 每个人都有读写和执行的权限

若分配给某个文件所有权限，则利用下面的命令：
sudo chmod -R 777 文件或文件夹的名字（其中sudo是管理员权限）

============================================================
# Root 权限

## 1、首先重置root密码：
利用现有管理员帐户登陆Ubutu，在终端执行命令：sudo passwd root，回车后提示要输入当前用户密码,验证通过后会提示设置root密码，重复密码。再重新启动就可以用root登陆。

## 2、允许以root用户登录：
默认情况是不允许用root帐号直接登陆图形界面的。这可以通过修改/etc/gdm/gdm.conf文件来允许root直接登陆，在该文件中找到 AllowRoot＝false 将其改为 AllowRoot=true 切换用户就可以了。

注：gdm.conf默认是只读属性，修改前请先使用sudo chmod 777 /etc/gdm/gdm.conf 将文件权限设置为为777。

一般不建议直接用root用户登陆系统,因为这样系统安全得不到保障. 
通常做法是在需要root权限的时候在shell命令前加 sudo , 例如: sudo apt-get update.
或者输入 su 回车,然后输入root用户密码即可(你会发现终端命令的前导字符有$变为了#).
变为#后输入的命令都会以root权限执行了

============================================================
Ubuntu 更改文件夹权限
Ubuntu的许多操作是在终端中进行的，通过sudo命令管理的文件是由root持有权限的，一般用户是无法改变的。在图形界面上，我们可以通过属性中的权限选项夹进行操作。但是一旦文件的属性显示当前用户没有读写权力时，无法在图形界面上修改权限。

常用方法如下：
```
sudo chmod 600 ××× （只有所有者有读和写的权限）
sudo chmod 644 ××× （所有者有读和写的权限，组用户只有读的权限）
sudo chmod 700 ××× （只有所有者有读和写以及执行的权限）
sudo chmod 666 ××× （每个人都有读和写的权限）
sudo chmod 777 ××× （每个人都有读和写以及执行的权限）
```

其中×××指文件名（也可以是文件夹名，不过要在chmod后加-ld）。


解释一下，其实整个命令的形式是

sudo chmod -（代表类型）×××（所有者）×××（组用户）×××（其他用户）

三位数的每一位都表示一个用户类型的权限设置。取值是0～7，即二进制的[000]~[111]。

这个三位的二进制数的每一位分别表示读、写、执行权限。

如000表示三项权限均无，而100表示只读。这样，我们就有了下面的对应：

0 [000] 无任何权限

4 [100] 只读权限

6 [110] 读写权限

7 [111] 读写执行权限


现在看上面的几个常用用法就非常清楚了。试着自己来修改权限吧


最后同时附上查询文件（或文件夹）权限的命令
```
ls -l 文件名称 （文件夹将-l改为-ld）。 
```

============================================================
chmod命令详细用法
```
指令名称 : chmod
使用权限 : 所有使用者
使用方式 : chmod [-cfvR] [--help] [--version] mode file...
说明 : Linux/Unix 的档案调用权限分为三级 : 档案拥有者、群组、其他。利用 chmod 可以藉以控制档案如何被他人所调用。
参数 :
mode : 权限设定字串，格式如下 : [ugoa...][[+-=][rwxX]...][,...]，其中
u 表示该档案的拥有者，g 表示与该档案的拥有者属于同一个群体(group)者，o 表示其他以外的人，a 表示这三者皆是。
+ 表示增加权限、- 表示取消权限、= 表示唯一设定权限。
r 表示可读取，w 表示可写入，x 表示可执行，X 表示只有当该档案是个子目录或者该档案已经被设定过为可执行。
-c : 若该档案权限确实已经更改，才显示其更改动作
-f : 若该档案权限无法被更改也不要显示错误讯息
-v : 显示权限变更的详细资料
-R : 对目前目录下的所有档案与子目录进行相同的权限变更(即以递回的方式逐个变更)
--help : 显示辅助说明
--version : 显示版本
```
范例 :
```
将档案 file1.txt 设为所有人皆可读取 :
chmod ugo+r file1.txt 
将档案 file1.txt 设为所有人皆可读取 :
chmod a+r file1.txt 
将档案 file1.txt 与 file2.txt 设为该档案拥有者，与其所属同一个群体者可写入，但其他以外的人则不可写入 :
chmod ug+w,o-w file1.txt file2.txt 
将 ex1.py 设定为只有该档案拥有者可以执行 :
chmod u+x ex1.py 
将目前目录下的所有档案与子目录皆设为任何人可读取 :
chmod -R a+r * 
此外chmod也可以用数字来表示权限如 chmod 777 file
语法为：chmod abc file
```

其中a,b,c各为一个数字，分别表示User、Group、及Other的权限。
r=4，w=2，x=1
若要rwx属性则4+2+1=7；
若要rw-属性则4+2=6；
若要r-x属性则4+1=7。
范例：
chmod a=rwx file 
和
chmod 777 file 
效果相同
chmod ug=rwx,o=x file 
和
chmod 771 file 
效果相同
若用chmod 4755 filename可使此程序具有root的权限
 
============================================================
# ubuntu12.04启用root用户登录

Ubuntu 12.04默认是不允许root登录的，在登录窗口只能看到普通用户和访客登录。

以普通身份登陆Ubuntu后,在终端窗口里面输入: sudo  -s.
然后输入普通用户登陆的密码，回车即可进入 root用户权限模式。
然后执行: vi /etc/lightdm/lightdm.conf.
增加 greeter-show-manual-login=true  allow-guest=false.
修改完后的整个配置文件是
[SeatDefaults]
greeter-session=unity-greeter
user-session=Ubuntu
greeter-show-manual-login=true #手工输入登陆系统的用户名和密码
allow-guest=false   #不允许guest登录
然后启用root帐号：
sudo passwd root
根据提示输入root帐号密码，重启Ubuntu，登录窗口会有“登录”选项，就可以通过root登录了。

============================================================
# Ubuntu给用户添加sudo权限

## 方法一：要添加新用户到sudo，最简单的方式就是使用 usermod 命令。运行 
$sudo usermod -G admin username 
这就你要作的，然而，如果用户已经是其他组的成员，你需要添加 -a 这个选项，象这样 
$sudo usermod -a -G admin username 

## 方法二：使用visudo命令编辑器修改 /etc/sudoers 文件：

添加：
your_user_name ALL=(ALL) 

然后ctrl+w保存文件，ctrl+x退出 
这样就加入了sudo组，可以使用sudo命令了。 
如果想用sudo的时候不输入密码输入换成如下内容即可： 
your_user_name ALL=(ALL)NOPASSWD: ALL 

============================================================
# Ubuntu优化清理无用的包

Ubuntu Linux与Windows系统不同，Ubuntu Linux不会产生无用垃圾文件，但是在升级缓存中，

Ubuntu Linux不会自动删除这些文件，今天就来说说这些垃圾文件清理方法。
## 1，非常有用的清理命令：
```
sudo apt-get autoclean
sudo apt-get clean
sudo apt-get autoremove
```
这三个命令主要清理升级缓存以及无用包的。

## 2，清理opera firefox的缓存文件：
```
ls ~/.opera/cache4
ls ~/.mozilla/firefox/*.default/Cache
```

## 3，清理Linux下孤立的包：
图形界面下我们可以用：gtkorphan
```
sudo apt-get install gtkorphan -y
```
终端命令下我们可以用：deborphan
```
sudo apt-get install deborphan -y
```

## 4，卸载：tracker
这个东西一般我只要安装Ubuntu就会第一删掉tracker 他不仅会产生大量的cache文件而且还会影响开机速度。所以在新得利里面删掉就行。

## 5，删除多余的内核：一定不要删错哦，切记！
打开终端敲命令：dpkg --get-selections|grep linux
有image的就是内核文件
删除老的内核文件：
```
sudo apt-get remove 内核文件名 （例如：linux-image-2.6.27-2-generic）
```
内核删除，释放空间了，应该能释放130－140M空间。
最后不要忘了看看当前内核：uname -a

============================================================
## 附录：
包管理的临时文件目录:
包在
/var/cache/apt/archives
没有下载完的在
/var/cache/apt/archives/partial

apt-get与dpkg等各种安装命令:http://mintelong.iteye.com/blog/480132


## 创建一个自启动的service(适用于Ubuntu与centOS)

以创建nexus为例

1.使用以下内容创建文件 /etc/init.d/nexus
```
#!/bin/bash
/usr/bin/nexus $*
```
并保存文件；


2.注册到启动项
```
Register nexus at boot time (Ubuntu, 32 bit):
sudo ln -s $NEXUS_HOME/bin/nexus /usr/bin/nexus
sudo chmod 755 /etc/init.d/nexus
sudo update-rc.d nexus defaults


Register nexus at boot time (RedHat, CentOS, 64 bit):
sudo ln -s $NEXUS_HOME/bin/nexus /usr/bin/nexus
sudo chmod 755 /etc/init.d/nexus
sudo chkconfig --add nexus
```

============================================================
# 环境变量的设置

## 一、临时设置
export ORACLE_DIR=/opt/oracle/product

## 二、当前用户的全局设置
打开~/.bashrc，添加行：
export ORACLE_DIR=/opt/oracle/product
注销
这样每次以此用户登录Ubuntu，该环境变量都会生效；
若要产立即生效，终端执行命令：
test@ubuntu:~$ source .bashrc

## 三、所有用户的全局设置
$ vi /etc/profile 
在里面加入：
export ORACLE_DIR=/opt/oracle/product
若要产立即生效，终端执行命令：
test@ubuntu:~$ source /etc/profile

Bash 在用户登录时从四个文件中读取环境,执行顺序: 
/etc/profile -> ~/.bash_profile -> ~/.bashrc -> /etc/bashrc

## 添加用户与组并设置权限
sudo groupadd nexus #添加nexus用户组
sudo useradd -g nexus nexus #添加nexus用户并添加到nexus用户组
sudo usermod -G root nexus #将nexus用户添加到root组
sudo gpasswd -d nexus root #从root组中删除nexus
sudo usermod -G vvvv nexus #将nexus用户添加到vvvv组
#sudo passwd nexus #给nexus用户创建密码
sudo usermod -G nexus vvvv #将vvvv用户添加到nexus组
id #显示当前用户id，群组id
id vvvv #查看用户vvvv的用户id，以及所属群组id
groups vvvv #查看用户vvvv的所有群组id

============================================================
#Ubuntu系统目录结构

以下为Ubuntu目录的主要目录结构，您稍微了解它们都包含了哪些文件就可以了，不需要记忆。
```
/   根目录 

    │ 

    ├boot/      启动文件。所有与系统启动有关的文件都保存在这里 

    │    └grub/   Grub引导器相关的文件 

    │ 

    ├dev/       设备文件 

    ├proc/      内核与进程镜像 

    │ 

    ├mnt/      临时挂载 

    ├media/   挂载媒体设备 

    │ 

    ├root/      root用户的$HOME目录 

    ├home/          

    │    ├user/   普通用户的$HOME目录 

    │    └.../ 

    │ 

    ├bin/      系统程序 

    ├sbin/      管理员系统程序 

    ├lib/      系统程序库文件 

    ├etc/      系统程序和大部分应用程序的全局配置文件 

    │   ├init.d/   SystemV风格的启动脚本 

    │   ├rcX.d/   启动脚本的链接，定义运行级别 

    │   ├network/   网络配置文件 

    │   ├X11/      图形界面配置文件 

    │ 

    ├usr/       

    │   ├bin/      应用程序 

    │   ├sbin/   管理员应用程序 

    │   ├lib/      应用程序库文件 

    │   ├share/   应用程序资源文件 

    │   ├src/      应用程序源代码 

    │   ├local/       

    │   │     ├soft/      用户程序       

    │   │     └.../      通常使用单独文件夹 

    │   ├X11R6/   图形界面系统 

    │ 

    ├var/         动态数据 

    │ 

    ├temp/         临时文件 

    ├lost+found/   磁盘修复文件
```

