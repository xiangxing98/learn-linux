# Install_ubuntu14_in_XP.md

# 在XP下用U盘安装Ubuntukylin到硬盘的方法（双系统共存）

主要面向对象是内存2G以下的老旧电脑，才用xp系统，新电脑的话都是windows 8.1系统了。
下载ubuntukylin 14.04iso镜像。官网：http://www.ubuntukylin.com/downloads/
在XP下用U盘安装Ubuntukylin到硬盘的方法（双系统共存）

 在XP下用U盘安装Ubuntukylin到硬盘的方法（双系统共存）

一、首先是磁盘分区：
右键我的电脑--管理--磁盘管理，将xp系统下的硬盘分一个10G左右的空间出来（如果不放东西的话，10G足矣）。推荐使用分区助手5.5，可以在不格式化原有文件基础下将一个分区（比如EFG）分为两个。也可以删除一个现有的Windows分区给Ubuntu用。
分区助手下载：http://www.disktool.cn/
二、制作U盘Ubuntu的启动盘，使用UltraISO软件或UNetbootin软件，个人建议使用UNetbootin，因为有些使用Ultraiso制作的Ubuntu启动盘启动不了。
制作方法：
需要一个4G的空u盘，插到电脑上后，运行UNetbootin (Universal Netboot Installer)软件。选择下面的光盘镜像，再选择你下载的ubuntu iso镜像文件，接着就仔细查看，选中你的u盘，（请查看u盘盘符，千万不要选错了，不要选成硬盘。）点确定，就可以安装到U盘里，重启电脑，设置U盘为第一启动设备（电脑重启按F12进入BOOT启动项选择usb菜单），即可启动安装在U盘里的ubuntu系统，然后就可以将其安装在电脑上了。
UNetbootin下载地址：http://pan.baidu.com/s/1hqHsZf2
在XP下用U盘安装Ubuntukylin到硬盘的方法（双系统共存）

注意：一定要先插U盘再运行UNetbootin！
 
二、UBUNTU 14.04安装过程
     u盘启动成功后，用键盘上的上下左右选“install ubuntukylin”安装ubuntukylin，
最重要之处：xp安装ubuntu双系统分区：
Xp用户和新手主要注意之处：就在于linux系统的分区不熟悉。
适合新手的最简单的分区方式，即只分“/”和“/home”两个分区，简单方便。
 分区说明：
一、/：根目录分区，装系统和软件，视硬盘大小而定。10G空间选择7000MB即可。     （EXT4格式）
二、swap：交换分区，好像xp下的虚拟内存，如果是老旧电脑物理内存小于1G，该分区则至少要1G（这样的电脑想体验ubuntu，推荐下载Lubuntu，是Ubuntu快速轻量级版本。）。若电脑内存为2G及2G以上，就不需要这个分区，一般也用不到（swap休眠会用到）。10G空间想选择的话分700MB即可。     （EXT4格式）
网上的说法是：物理内存小于或等于 512MB，建议分配实际物理内存容量2倍大小的swap；物理内存大于512MB，建议分配与物理内存等容量的swap，不置可否。
三、/home：用户分区，越大越好，你装程序和数据存放处。剩下的分给它即可。类似xp“我的文档”     （EXT4格式）
四、/boot：引导分区，没有必要把/boot分区独立出来。因/boot目录的大小通常都非常小，大约20MB，分一个100MB的分区无疑是一种浪费，而且还把把硬盘分的支离破碎的。
XP下独立分boot后，容易造成进入windows引导不了ubuntu，还要用grub4dos设置，很不好操作。（不分区的话，卸载ubuntu时候，需要使用mbr 修复来正确引导xp系统，不过这个很easy。）
另外：Linux的所有分区都可以位于逻辑分区中。安装双系统且已安装Windows的话，/分区的类型选择主分区和逻辑分区都可以，所以不要再浪费有限的主分区了，放心的把Linux安装在逻辑分区中吧。
 
推荐选择：10G空间可以这样分区：
1、引导分区: /boot 选择C盘 sda1
2、系统分区: / 　EXT4 7000MB
3、交换分区: swap　EXT4 700MB
 4、个人文件分区：/home　EXT4剩下的。
建议你先分/和swap，然后把所有剩余空间分给/home。
操作说明：
分区前先选中10G空间点击下面的“删除”按钮，这样就有了空闲空间来分区。
然后在列表中选择“空闲空间”，然后点击下面的“+”按钮，会弹出“创建新分区”窗口，设置空间大小。文件系统的类型当然选EXT4,各方面性能比EXT3或EXT2都要好。
初学不建议将分区搞的太复杂，只需要“/”和“/home”两个分区即可。
类似的图：
在XP下用U盘安装Ubuntukylin到硬盘的方法（双系统共存）

说明：
/dev/sda1就是xp下的C盘，系统盘。/dev/sda5就是xp下的D盘了。
在Linux下的hda1就相当于windows下的C盘，hda2就相当于你的拓展分区（不可见的）（Linux下hda1 至hda4可以是主分区，hda5开始是逻辑分区）。那么hda5就相当于你的D盘，hda6相当于E盘，同理hda7地位等同与F盘。
更多：
sda表示的是你的第一块sata硬盘，sda1表示的是你的第一块sata硬盘的第一个分区。hda一般是指IDE接口的硬盘，hda一般指第一块硬盘，类似的有hdb,hdc等
sda一般是指SATA接口的硬盘，sda一般指第一块硬盘，类似的有sdb,sdc等。第一个设备为sda，第二个设备为sdb。
现在的内核都会把硬盘，移动硬盘，U盘之类的识别为sdX的形式

其他：
关于分/boot安装后重启没有ubuntu引导：
sda指整块硬盘，sda1是第一个分区，开头有一块地方存引导信息，叫mbr，windows不论安装在sda1还是sda2，引导都写入mbr，也就是linux下所谓sda。
linux的grub可以在mbr里写引导也可以在任何一个分区开头写引导，这和系统安装在哪个分区没有关系，即你在sda上写grub，如果之后装windows，就把grub覆盖了，找不到linux的引导了，如果你把linux的grub写在sda2，你装windows，不会覆盖grub，但是系统默认进入windows，即sda的引导区mbr。
所以，/boot当然留一部分在硬盘，分区的头部。
安装配图：
在XP下用U盘安装Ubuntukylin到硬盘的方法（双系统共存）

      在XP下用U盘安装Ubuntukylin到硬盘的方法（双系统共存）
然后就是一些其他选项，直接继续，注意的是在设置“用户名和密码”时，如果你习惯不输入密码登录，可以点登录“自动登录”，这样每次开机就不用输入密码。类似的图：
然后就开始安装了
等安装完重启，ubuntukylin就安装好了！
系统运行后ubuntukylin 14.04界面：
在XP下用U盘安装Ubuntukylin到硬盘的方法（双系统共存）

在XP下用U盘安装Ubuntukylin到硬盘的方法（双系统共存）

在XP下用U盘安装Ubuntukylin到硬盘的方法（双系统共存）

在XP下用U盘安装Ubuntukylin到硬盘的方法（双系统共存）

附：安装后ubuntu内存占用情况：
可以看到XP安装ubuntukylin 14.04后，只占用了400MB左右内存，设置的730MB swap空间就没有使用。
在XP下用U盘安装Ubuntukylin到硬盘的方法（双系统共存）

附：ubuntukylin 14.04 壁纸包
请看下篇：
windows XP和ubuntu双系统下如何卸载ubuntu
