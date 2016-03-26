# how to update R language and RStudio in Ubuntu 14.04.md

## how to update R language in Ubuntu
============================================================
when I use the package "rstatscn" in ubuntu
the console shows "package ‘rstatscn’ is not available (for R version 3.0.2)"

so I want to update the R to the lastest relase ,google~~
http://stackoverflow.com/questions/10476713/how-to-upgrade-r-in-ubuntu/
============================================================

Follow the instructions from here

## 1. gedit open sources.list
```
sudo gedit /etc/apt/sources.list
```
This will open up your sources.list file in gedit,

## 2. where you can add the following line.
```
    deb http://cran.cnr.berkeley.edu/bin/linux/ubuntu version/
```    
Replace version/ with whatever version of Ubuntu you are using (eg, precise/, oneric/, and so on). If you're getting a "Malformed line error", check to see if you have a space between /ubuntu/ and version/.
ubuntu 14.04 is trusty.

## 3. Fetch the secure APT key
```
gpg --keyserver keyserver.ubuntu.com --recv-key E084DAB9 
# or
gpg --hkp://keyserver keyserver.ubuntu.com:80 --recv-key E084DAB9
```

## 4. Feed it to apt-key
```
gpg -a --export E084DAB9 | sudo apt-key add -
```

## 5. Update your sources and upgrade your installation with 
```
sudo apt-get update;
sudo apt-get upgrade;
```


Since R is already installed, you should be able to upgrade it with this method.

============================================================
```
arthur@Acer:~$ sudo subl /etc/

Display all 230 possibilities? (y or n)

arthur@Acer:~$ sudo subl /etc/apt/sources.list

[sudo] password for arthur:

arthur@Acer:~$ gpg --hkp://keyserver keyserver.ubuntu.com:80 --recv-key E084DAB9gpg: Invalid option "--hkp://keyserver"
arthur@Acer:~$ gpg --keyserver keyserver.ubuntu.com --recv-key E084DAB9

gpg: directory `/home/arthur/.gnupg' created
gpg: new configuration file `/home/arthur/.gnupg/gpg.conf' created
gpg: WARNING: options in `/home/arthur/.gnupg/gpg.conf' are not yet active during this run
gpg: keyring `/home/arthur/.gnupg/secring.gpg' created
gpg: keyring `/home/arthur/.gnupg/pubring.gpg' created
gpg: requesting key E084DAB9 from hkp server keyserver.ubuntu.com
gpg: /home/arthur/.gnupg/trustdb.gpg: trustdb created
gpg: key E084DAB9: public key "Michael Rutter <marutter@gmail.com>" imported
gpg: Total number processed: 1
gpg: imported: 1 (RSA: 1)
arthur@Acer:~$ gpg -a --export E084DAB9 | sudo apt-key add -
OK
arthur@Acer:~$

```
BUT the best answer is here

https://cran.r-project.org/bin/linux/ubuntu/README.html

http://blog.sina.com.cn/s/blog_4aaba1b50102vofs.html

# another one to install R in Ubuntu
============================================================
1.首先更新sources.list
cd /etc/apt/
 sudo gedit sources.list
 2.更新软件源

deb http://mirrors.ustc.edu.cn/CRAN/bin/linux/ubuntu/ precise/
sudo apt-get update

3.安装R语言

sudo apt-get install r-base

4.安装r-base-dev包
sudo apt-get install r-base-dev

5.使用R软件
命令行下输入R

6.命令行下输入
#!/usr/bin/Rscript
print("zjhello123.blog.163.com")

输出显示 [1] "zjhello123.blog.163.com"

至此，我们便完成了在ubuntu环境下安装R语言。


============================================================
## 1、 安装JDK8（R语言安装时会用到）
```

sudo add-apt-repository ppa:webupd8team/java

sudo apt-get update

sudo apt-get install oracle-java8-installer
```
 
运行java –version有显示则表示已经安装成功

自动更新jdk配置
```
sudo apt-get install oracle-java8-set-default
```
 
============================================================
## 2、 安装R语言
```
sudo gedit/etc/apt/sources.list

#增加：
deb-src http://mirrors.ustc.edu.cn/CRAN/bin/linux/ubuntu trusty/
#增加key
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E084DAB9
sudo apt-get update
sudo apt-get install r-base r-base-dev
#（后者是开发模式，可以制作包）
```

如果安装的R不是最新的版本，需要运行以下语句
```
sudo add-apt-repository ppa:marutter/rdev
sudo apt-get update
sudo apt-get upgrade
```
 
============================================================
## 安装rJava（需要安装xlsx包的话操作此步）
```
sudo apt-get install openjdk-7-*
sudo R CMD javareconf
#（用于R语言RCurl包的安装）
sudo apt-get install libcurl4-openssl-dev
#（用于R语言XML包的安装）
sudo apt-get install libxml2-dev
#（用于R语言RQuantLib包的安装）
sudo apt-get install libquantlib0-dev
```
 
============================================================
## 3、 安装RStudio
```
sudo apt-get install gdebi
```
官网下载适用Ubuntu的rstudio的deb文件

```
#（需要在相关文件夹操作）
sudo gdebi rstudio-XXXX.deb
```
 
============================================================
## 4、 安装LAMP环境

MySQL
```
sudo apt-get install mysql-server mysql-client
```
数据库权限设置：

Shell登录
```
mysql–uroot –p 然后输入安装时设置的root权限密码

usemysql  //切换数据库

SELECT user,password,host FROM user;   //查看已有权限

CREATE USER username IDENTIFIED BY 'xxxxx';  //创建新用户，xxx为密码

GRANT ALL PRIVILEGES ON *.* TO username@’%’ IDENTIFIED BY "xxxxx" WITH GRANT OPTION ;  //为用户创建权限，with grant option为创建用户管理权限，否则该用户不可创建新用户
```
 

修改/etc/mysql/my.cnf文件
```
sudo gedit /etc/mysql/my.cnf
```
将bind-address 127.0.0.1注释掉，以便外网可以访问

重启mysql
```
sudo /etc/init.d/mysql restart
```
 
============================================================
## Apache2
```
sudo apt-get install apache2-mpm-prefork apache2-prefork-dev
```
安装apache2之后可以在本机浏览器运行http://localhost/，如果跳出apache的欢迎界面则说明安装成功。

需要在/etc/apache2/apache2.conf文件中增加ServerName变量，如：
```
​ServerName localhost
```
 
============================================================
## PHP5
```
sudo apt-get install php5 libapache2-mod-php5
```
 

PHP5相关插件
```
sudo apt-get install php5-mysql php5-curl php5-gd php5-intl php-pear php5-imagickphp5-imap php5-mcrypt php5-memcache php5-ming php5-ps php5-pspell php5-recodephp5-snmp php5-sqlite php5-tidy php5-xmlrpc php5-xsl
```
 
============================================================
5、 安装rApache
```
sudo apt-getinstall liblzma-dev
#（不安装此步将无法安装rapache）

wget http://www.rapache.net/rapache-VERSION.tar.gz

tar -zxvfrapache-VERSION.tar.gz

cdrapache-VERSION

./configure

make

makeinstall
```
 
============================================================
## 6、 安装komodo-edit编辑器（用于编写常规代码的文本编辑器）
```
sudo add-apt-repository ppa:mystic-mirage/komodo-edit

sudo apt-get update

sudo apt-get install komodo-edit
```
 
============================================================
## 7、 安装谷歌拼音输入法（自带的拼音输入法太难用了）
```
sudo apt-get install ibus-googlepinyin
```
重启——设置——文本输入——加号——搜索google——找到谷歌拼音添加

 
============================================================
## 8、 安装gftp软件
```
sudo apt-get install gftp
```
（详细设置可参考百度经验里面的一篇，经测可用）​


============================================================
# Install R & RStudio in Ubuntu 14.04 trusty
Install R in Ubuntu is extremely easy if you don’t meet any exception, but if you meet, then you’d better be a very advanced linux user :-)

============================================================
## Install R

Because the Ubuntu official source R version is usually half of years older than R-project official source, so it is recommanded to using r-project.org official source to install the latest R system.

### add R source
```
sudo vi /etc/apt/sources.list
# append below line to end of sources.list
# you can view mirror at http://cran.r-project.org/mirrors.html
deb http://ftp.ctex.org/mirrors/CRAN/bin/linux/ubuntu precise/
```

### import the GPG key and install r-base
```
cd ~
gpg --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys E084DAB9
gpg -a --export E084DAB9 | sudo apt-key add -
apt-get upgrade
apt-get install r-base
```

============================================================
## Install Oracle DB access package
ROracle is much faster compare with RODBC or RJDBC in performance, You can found new version of ROracle or DBI package in CRAN, it is also required you properly install the Oracle Instant Client.
### manual install the ROracle
```
wget http://cran.r-project.org/src/contrib/DBI_0.2-7.tar.gz
R CMD INSTALL DBI_0.2-7.tar.gz
wget http://cran.r-project.org/src/contrib/ROracle_1.1-11.tar.gz
R CMD INSTALL --configure-args='--with-oci-inc=/opt/oracle/instantclient_11_2/sdk/include --with-oci-lib=/opt/oracle/instantclient_11_2' ROracle_1.1-11.tar.gz
```

============================================================
## Install RStudio Server

###Install RStudio Server
```
apt-get install gdebi-core
apt-get install libapparmor1  # Required only for Ubuntu, not Debian
wget http://download2.rstudio.org/rstudio-server-0.97.551-i386.deb
gdebi rstudio-server-0.97.551-i386.deb
rstudio-server verify-installation
```

### Do some RStudio Server setting
below setting depend on your system

```
echo 'rsession-memory-limit-mb=1000' > /etc/rstudio/rserver.conf
echo 'rsession-stack-limit-mb=4' >> /etc/rstudio/rserver.conf
echo 'rsession-process-limit=20' >> /etc/rstudio/rserver.conf
# Only pass below if you will using proxy mode
echo 'www-address=127.0.0.1' >> /etc/rstudio/rserver.conf
```

### groupadd rstudio
Setting the proxy server for RStudio server
This section is optional, assured already install nginx in server.
```
# do not forgot link to /opt/nginx/conf/vhosts
server {
  listen       80;
  server_name  cvprstudio;
  location / {
    proxy_pass http://localhost:8787;
    proxy_redirect http://localhost:8787/ $scheme://$host/;
  }
}
```

### Setting auto restart and PATH
```
ln -s /usr/lib/rstudio-server/extras/init.d/debian/rstudio-server /etc/init.d/rstudio-server
vi /etc/init.d/rstudio-server

vi

# append below line to /usr/lib/rstudio-server/extras/init.d/debian/rstudio-server SCRIPTNAME
ORACLE_BASE=/opt/oracle
ORACLE_HOME=/opt/oracle/instantclient_11_2
TNS_ADMIN=/opt/oracle/network/admin
NLS_LANG=AMERICAN_AMERICA.AL32UTF8
```

Now you can restart rstudio-server via standard init.d service way

```
/etc/init.d/rstudio-server restart
```

Add a user in RStudio
```
adduser --ingroup rstudio cindy
passwd cindy # setting password
```

Update package

Usually it is more good to upgrade the r-base in system wide packages instead of per user.
after run R in root console
```
update.packages() # select mirror to check
```

