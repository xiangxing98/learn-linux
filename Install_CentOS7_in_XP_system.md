Window XP下安装centOS7双系统总结
2015-08-08 09:56:44 下午 Collected and Modified By siqin.hou@gmail.com 
首先，按照网络教程，使用硬盘安装模式（失败）：
一、使用分盘工具（easeUS Partion Master）分出一块未使用的空间，为安装centOS和保存镜像文件作准备。

二、将分出来的一部分空格，使用分盘工具（其实分盘不过就是修改mbr ，主引导记录），创建分区，创建了格式为ext2的分区，设置其为逻辑分区，保证让Linux认识，同时由于windows系统不认识ext2格式，所以需要使用到Ext2Fsd软件为该分区分配盘符。另一部分暂时保持未分配状态，留着安装centOS7。

三、在官网下载了下载了CentOS-7.0-1406-x86_64-DVD.iso镜像文件，保存至步骤“二”中分配的分区的根目录。并按照网络教程，使用解压工具，“部分解压”出CentOS-7.0-1406-x86_64-DVD.iso中的images和isolinux文件夹。

四、修改文件查看选项，修改c盘根目录下(隐藏文件)boot.ini，添加一行：C:\grldr="Grub" 代码。

五、由于xp下easyBCD软件无法使用，下载Grub For Dos，复制menu.lst 文件至C盘根目录，并修改该文件，添加如下几行代码：
title Install-RHEL7/CentOS7
  root (hd0,5)                   //注意：(hd0,5)和下面的sda6都指向步骤二新分配出的逻辑分区。
  kernel /isolinux/vmlinuz linux repo=hd:/dev/sda6:/
  initrd /isolinux/initrd.img
  boot

六、重启电脑，结果并未出现安装centOS的引导，本方法尝试以失败告终。虽然没有成功，但是种种尝试仍然记录下来，以备后用。


方法二：使用U盘安装centOS（成功）
一、使用USBWriter.exe 程序，将镜像文件写入U盘，然后重启电脑，设置系统从U盘启动，可以成功进入CENTOS7的安装引导界面。（不知道写入U盘后，可否删除本机的CentOS-7.0-1406-x86_64-DVD.iso文件？尚未实验。）

二、按照提示一路设置并安装centOS7即可。其中，默认最小化安装，为了方便使用，我选择了 桌面安装，并勾选了所有配套的软件。其次在分区上，没有使用自动分区，而是使用手动分区，点击“创建他们”，和+ 创建/boot,/,swap分区等，其中只有boot可以设置为“标准分区”其他都设置为lvm，这里为分区而使用到的空间，就是一开始预留的未分配的空间，否则会在左下方显示的可用空间几乎为0MB，导致无法成功手动分区，或者将要删除windows下的磁盘空间，来分配给Linux。

三、重启电脑后，发现只有centOS的引导，没有windows XP的启动引导。于是查询网络方案

网络方案一（失败）：
启动时，可以使用grub命令行手动引导进入win7系统。系统启动进入下面的画面时，按键盘上c键进入grub命令行。
使用ls命令查看所有硬盘装置，显示结果如下：
(hd0)(hd0, msdos6) (hd0, msdos5)...(hd0,msdos1) (hd1) (hd1,msdos1)
然后在grub命令行连续输入执行下面的命令，就能进入到win7系统了。
set root=(hd0, msdos1)
chainloade +1//动手尝试时，系统提示错误，不认识“+1”
boot
上面三条命令中，set命令指定将要启动系统的分区，我的win7系统安装在第一块硬盘hd0的msdos1（即第一个分区）。第二条命令注意chainloader和参数之间必须有空格。第三条命令启动系统。

网络方案二（成功）
修改/boot/grub2/下的grub.cfg文件，由于该文件是只读属性，不能双击打开修改，需要使用root用户登录，再用命令打开文件，再手动修改文件，再保存即可。操作如下图：
cd /boot/grub2
ls
```
device.map grub.cfg grub.conf ...
```
su
sudo chmod +w grub.cfg
sudo gedit grub.cfg
（sudo chmod +w grub.cfg命令是为了给grub.cfg文件添加“写“的权限，后来尝试，哪怕去掉”写权限“：sudo chmod -w grub.cfg
也可以使用 sudo gedit grub.cfg打开文件，修改，再点击保存按钮。）

修改/boot/grub2/grub.cfg文件，如下：
### BEGIN /etc/grub.d/30_os-prober ###
menuentry ‘Windows XP ‘ {
insmod ntfs
set root=(hd0,1)//指向C盘中安装的XP系统
chainloader +1
}
### END /etc/grub.d/30_os-prober ###
保存文件。
重启电脑，成功出现”Windows XP“（修改grub.cfg文件中取的名字）的启动windows的引导。

方案三
vim /boot/grub/grub.conf
可以很清晰的看到CentOS启动系统时加载的grub菜单，因此，只要知道格式，自己编辑这个菜单就可以多系统引导了，很方便。

由于系统安装的时候已经发现了other系统(windows),因此这里会多一个title Other，编辑一下，改成容易看懂的标题就可以了。如下

#title Other
#	rootnoverify (hd0,0)
#	chainloader +1
title Windows XP
	root (hd0,0)
	chainload +1

title这里已经可以看到刚才改过的标题了。
grub识别的硬盘，U盘都是hd，所以用root指定启动分区，(hd0,0)代表第一块硬盘的第一个分区，也就是windows xp所在的分区（C盘）。
为什么是win xp&win 7？很明显，装完win7重启系统的时候我们已经发现了，win7把boot loader放在了sda1(c盘)，覆盖掉了XP的引导，而win7的系统盘sda2（D盘）上是没有引导文件的，无法引导系统。试试看，将grub菜单中改为root (hd0,1)会是什么情况？

此时还未结束，进一步扩展grub引导，想开机默认引导win7，怎么办？
grub中开头会有一个default=0参数，这个参数是指明grub默认引导哪一个title，0代表第一个，1代表第二个，以此类推。
因此只需要把default=0改为default=1即可。
继续vi /boot/grub/grub.conf，继续刚才未完成的事业吧。

这次仔细观察了一下grub.conf，发现前面几个参数同样比较有意思。
default=1 #默认引导第二个title，即windows XP&Windows 7
timeout=5 #超时5秒，grub引导时5秒不做任何操作采用默认的default title引导系统
splashimage=xxxxxx #grub背景图片，由于CentOS使用的是grub1版本，因此只支持640*480的图片，色深16色，而且必须压制成.gz，具体可以百度，这里不戏表。
hiddenmenu #开机只显示default title和time out，隐藏掉启动菜单，这里注释掉了，为了看得更清楚一些。

继续:wq之后reboot看效果
