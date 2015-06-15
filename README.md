# CentOS Recipes

[罗辑思维记录](http://www.ljsw.cc/)

### About me

我就是我，我懂我爱我。
思考-行动，勤奋-专业
https://github.com/xiangxing98
http://www.worldhello.net/gotgithub/
2015-06-15 kindle paperwhites
2015-06 GITHUB+R+VBA
2015-05 DSS+GIT+GITHUB+R
2015-04 JHU DS +ggplot2 + R
2015-03 Getting and Cleaning Data-Coursera
2015-03 在读古典的《拆掉思维里的墙》
2015-02 罗辑思维：过往不念，当下不杂，未来不惧。
2015 MOOC+Coursera+慕课+网易云课堂study.163.com+果壳慕课学院
http://www.douban.com/people/xiangxing98/

### Add Linux
http://www.sublimetext.com/3

http://linux.vbird.org/

http://vbird.dic.ksu.edu.tw/

### Add CentOS site
http://www.centoscn.com/

http://www.iplaysoft.com

### …or create a new repository on the command line
```
echo "# learn-centos" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/xiangxing98/learn-centos.git
git push -u origin master

#SSH
echo "# learn-centos" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin git@github.com:xiangxing98/learn-centos.git
git push -u origin master
```

### …or push an existing repository from the command line
```
git remote add origin https://github.com/xiangxing98/learn-centos.git
git push -u origin master

#SSH
git remote add origin git@github.com:xiangxing98/learn-centos.git
git push -u origin master
```

Repository for learning linux in general and CentOS specifics. This repository contains basically a whole bunch of small recipes :)

 - How do I install Poppler PDF Utilities?
 - How do I get OS version?
 - How do I know how many cores my system has?
 - How do I install MySql?
 - How do I install Node.js?
 - How do I install Git?
 - How to allow specific user run command as sudo without password?
 - How do I install nginx?

### How do I install Poppler PDF Utilities?
Just run `sudo yum install poppler-utils.x86_64`. This will allow you to use to following commands:
```
pdfdetach    pdffonts     pdfimages    pdfinfo      pdfroff
pdfseparate  pdftocairo   pdftohtml    pdftoppm     pdftops
pdftotext    pdfunite
```

### How do I get OS version?

You should run `cat /etc/centos-release`

### How do I know how many cores my system has?

Run the command `cat /proc/cpuinfo` to list details for all cores, and run `grep 'model name' /proc/cpuinfo | wc -l` to know how many cores your system has.

### How do I install MySql?

```bash
sudo yum upgrade -y
sudo yum install mysql -y
sudo yum install mysql-server -y
sudo /etc/init.d/mysqld start
mysqladmin -u root password 'rootPassword'
sudo chkconfig mysqld on

# my.cnf should be located at; /etc/my.cnf
# add the following line to allow external connections;
# bind-address = 0.0.0.0
# then restart mysql to reload configurations;
# sudo /etc/init.d/mysqld restart
```

You will be required to grant remote access to any user you would like to be able to connect from outside localhost.

```sql
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'rootPassword';
FLUSH PRIVILEGES;
```

This is a per database permission so don't forget to use wildcards or specify specific databases.

### How do I install Node.js?

```bash
curl https://raw.githubusercontent.com/creationix/nvm/v0.13.1/install.sh | bash
source ~/.bashrc
nvm install 0.10
nvm use 0.10
sudo visudo # add `dirname $(which node)` to secure_path so you're able to `sudo node` and `sudo npm`
```

### How do I install Git?

Simply run `sudo yum install -y git`

### How to allow specific user run command as sudo without password?

Log in as root then run `sudo visudo` and add the following line `joe ALL=(ALL) NOPASSWD: /full/path/to/command`, or if you want to restrict wich arguments this user may pass you can change that line to `joe ALL=(ALL) NOPASSWD: /full/path/to/command ARG1 ARG2`.

### How do I install nginx?

To install nginx you must run these commands:

```bash
sudo yum install epel-release
sudo yum install nginx
```

To start the services run `sudo /etc/init.d/nginx start`

### learn-centos
```bash
git clone git@github.com:xiangxing98/learn-centos.git learn-centos
git add .
git commit -m "comment here about what your have done"
git push -u origin master
```

[Thanks renatoargh/centos ](https://github.com/renatoargh/centos)
