Install-Sublime-Text3-in-CentOS

## Download
在官网下载，tarball    下载链接http://www.sublimetext.com/3   

>提示信息：  
Ubuntu 64 bit - also available as a tarball for other Linux distributions. 
 一定要下载tarball
```
cd /home/siqin/Downloads/
ls -al
#sublime_text_3_build_3083_x32.tar.bz2
tar jxvf sublime_text_3_build_3083_x32.tar.bz2  -C /opt
```
## 可以切换的该目录下，直接  
```
cd /opt/
ls -al
cd sublime_text_3
./sublime_text
```

## 建立软链接，以方便终端打开
```
ln -s /opt/sublime_text_3/sublime_text /usr/bin/sublime
```
打开命令就是sublime

## 建立桌面快捷
```
cp /opt/sublime_text_3/sublime_text.desktop /usr/share/applications
cd /opt/sublime_text_3/Icon/48x48/
cd /usr/share/applications
vim sublime_text.desktop
修改"Icon=/opt/sublime_text_3/Icon/48x48/sublime-text.png"
# 应用程序 >编程 > Sublime Text"右键"将此启动器添加到桌面"
```

去掉更新提示：找到Preferences -> Settings-User（设置用户），
找到倒数第三行的//"update_check": false, 把注释去掉
