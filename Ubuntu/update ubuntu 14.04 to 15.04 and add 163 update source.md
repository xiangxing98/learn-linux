# 1、阶梯升级方式。

就是我现在是ubuntu14.04可以直接升级到ubuntu14.10，然后从ubuntu14.10升级到ubuntu15.04.
这种升级安装的好处是之前的配置及软件都可以保存。

升级方式:
```
$sudo apt-get update  //更新资源链接

$sudo update-manager  -c  -d   
//升级版本，如果你的系统软件不是最新的，
//第一次升级有可能需要更新安装，重新启动后再次使用此命令才可以升级版本。
```

================================================================================
#update 163 mirror

## 切换到/etc/apt/目录下
```
cd /etc/apt/
```

## 将里面的sources.list复制一下
```
cp sources.list sources_backup.list
sudo gedit sources.list
# add 163 sources
deb http://mirrors.163.com/ubuntu/ trusty main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ trusty-security main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ trusty-updates main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ trusty-proposed main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ trusty-backports main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ trusty main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ trusty-security main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ trusty-updates main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ trusty-proposed main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ trusty-backports main restricted universe multiverse
```

## 最后执行：
```
apt-get update
```
