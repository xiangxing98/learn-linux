## Detailed Information About Linux chmod and chown.md

chmod----改变一个或多个文件的存取模式(mode)
chmod [options] mode files
只能文件属主或特权用户才能使用该功能来改变文件存取模式。mode可以是数字形式或以who opcode permission形式表示。who是可选的，默认是a(所有用户)。只能选择一个opcode(操作码)。可指定多个mode，以逗号分开。
options：
-c，--changes
只输出被改变文件的信息
-f，--silent，--quiet
当chmod不能改变文件模式时，不通知文件的用户
--help
输出帮助信息。
-R，--recursive
可递归遍历子目录，把修改应到目录下所有文件和子目录
--reference=filename
参照filename的权限来设置权限
-v，--verbose
无论修改是否成功，输出每个文件的信息
--version
输出版本信息。
who
u
用户
g
组
o
其它
a
所有用户(默认)
opcode
+
增加权限
-
删除权限
=
重新分配权限
permission
r
读
w
写
x
执行
s
设置用户(或组)的ID号
t
设置粘着位(sticky bit)，防止文件或目录被非属主删除
u
用户的当前权限
g
组的当前权限
o
其他用户的当前权限
作为选择，我们多数用三位八进制数字的形式来表示权限，第一位指定属主的权限，第二位指定组权限，第三位指定其他用户的权限，每位通过4(读)、2(写)、1(执行)三种数值的和来确定权限。如6(4+2)代表有读写权，7(4+2+1)有读、写和执行的权限。
还可设置第四位，它位于三位权限序列的前面，第四位数字取值是4，2，1，代表意思如下：
4，执行时设置用户ID，用于授权给基于文件属主的进程，而不是给创建此进程的用户。
2，执行时设置用户组ID，用于授权给基于文件所在组的进程，而不是基于创建此进程的用户。
1，设置粘着位。
实例：
$ chmod u+x file 　　　   给file的属主增加执行权限
$ chmod 751 file 　　　   给file的属主分配读、写、执行(7)的权限，给file的所在组分配读、执行(5)的权限，给其他用户分配执行(1)的权限
$ chmod u=rwx,g=rx,o=x file 上例的另一种形式
$ chmod =r file  　　　　为所有用户分配读权限
$ chmod 444 file    　　　　 同上例
$ chmod a-wx,a+r   file   　　 　   同上例
$ chmod -R u+r directory  　   递归地给directory目录下所有文件和子目录的属主分配读的权限
$ chmod 4755 　　设置用ID，给属主分配读、写和执行权限，给组和其他用户分配读、执行的权限。


## 本文主要详细介绍centos用户&组权限&添加删除用户。

### 1.Linux用户操作系统

Linux操作系统是多用户多任务操作系统，包括用户账户和组账户两种：
细分用户账户（普通用户账户，超级用户账户）除了用户账户以为还有组账户所谓组账户就是用户账户的集合，centos组中有两种类型，私有组和标准组：
当创建一个新用户时，若没有指定他所属的组，centos就建立以个和该用户相同的私有组，此私有组中只包括用户自己。
标准组可以容纳多个用户，如果要使用标准组，那创建一个新的用户时就应该指定他所属于的组，从另外一方面讲，同一个用户可以属于多个组。当一个用户属于多个组时，其登录后所属的组是主组，其它组为附加组。

### 2.Linux环境下的账户系统文件

账户系统文件主要在/etc/passwd, /etc/shadow,/etc/group,和/etc/gshadow四个文件中。
其中root的uid是0，从1-499是系统的标准账户，普通用户从uid 500开始。

### 3.使用命令管理账户

    useradd 选项  用户名//添加新用户
    usermod 选项  用户名//修改已经存在的用户
    userdel -r    用户名//删除用户表示自家目录一起删除。
    groupadd 选项  组名// 添加新组
    groupmod 选项  组名//修改已经存在的组
    groupdel 组名  //删除已经存在的特定组。
例子
    useradd zhh888 //添加一个用户zh888
    groupadd blog  //新建一个blog组
    useradd -G blog zh //表示创建一个新用户zh，同时加入blog附加组中。
    useradd -d /var/ftp/pub -M ftpadmin //创建一个新用户ftpadmin,指定目录是/var/ftp/pub,不创建自家目录（-M)
    usermod -G blog zh888 //表示将zh888添加到附加组blog中去。
    userdel ftpadmin //表示删除ftpadmin用户
    userdel -r zhh888 //表示删除zh888和/home中的目录一起删除。
    groupdel blog //表示删除blog组。

### 4.口令管理及时效

创建用户之后就要给用户添加密码，设置的口令的命令式passwd
    passwd 选项  用户名
    passwd -l 用户名账号名//禁止用户账户口令
    passwd -S 用户名//表示查看用户账户口令状态
    passwd -u 用户名//表示恢复用户账号
    passwd -d 用户名//表示删除用户账户口令

### 5.chage 命令

chage是保护密码的时效这样可以防止其他人猜测密码的时间。
chage 选项 用户名
参数有 -m days, -M days ,-d days, -I days ,-E date, -W days,-l
例子：
#chage -m 2 -M 30 -W zhh//表示的意思是要求用户zhh两天内不能更改密码，并且口令最长存活期是30天，并且口令过期5天通知zhh

### 6.用户和组的状态查询命令

    whoami //用于显示当前的用户名称。
    groups 用户名//表示显示指定的用户所属的组，如果没指定用户则是当前用户所属的组。
    id //表示显示当前用户的uid gid和用户所属的组列表。
    su - 用户//表示转换到其他用户，如果su表示切换到自己的当前用户。
    newgrp 组名//表示转换用户的当前组到指定的附加组，用户必须属于该组才能进行。

### 7.更改属主和同组人

有时候还需要更改文件的属主和所属的组。只有文件的属主有权更改其他属主和所属的组，用户可以把属于自己的文件转让给大家。改变文件属主用chown命令
    chown [-R] &lt;用户名或组&gt;&lt;文件或目录&gt;
    chown zh888 files//把文件files属主改成zh888用户。
    chown zh888.zh888 files//将文件files的属主和组都改成zh888。
    chown -R zh888.zh888 files//将files所有目录和子目录下的所有文件或目录的主和组都改成zh888.
    sudo chown siqin ../learn-centos/*

使用chown命令可以修改文件或目录所属的用户：

命令：chown 用户 目录或文件名
例如：chown qq /home/qq  (把home目录下的qq目录的拥有者改为qq用户)

使用chgrp命令可以修改文件或目录所属的组：

命令：chgrp 组 目录或文件名
例如：chgrp qq /home/qq  (把home目录下的qq目录的所属组改为qq组)


### 8.设置文件的目录和目录生成掩码

用户可以使用umask命令设置文件默认的生成掩码。默认的生成掩码告诉系统创建一个文件或目录不应该赋予哪些权限。如果用户将umask命令放在环境文件.bash_profile中，就可以控制所有新建的文件和目录的访问权限。
    umask [a1a2a3]
    a1表示的是不允许属主的权限，a2表示的是不允许同组人的权限，a3代表不允许其他人的权限。
    umask 022//表示设置不允许同组用户和其他用户有写的权限。
    umask //显示当前的默认生成掩码。

### 9.特殊权限的设置

    SUID SGID 和sticky-bit
除了一般权限还有特殊的权限存在，一些特殊权限存在特殊的权限，如果用户不需要特殊权限一般不要打开特殊权限，避免安全方面的问题。

