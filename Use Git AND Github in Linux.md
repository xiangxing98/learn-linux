# Use Git AND Github in Linux.md

下面给各位使用linux系统的朋友介绍在Linux下Git和GitHub使用教程，如果各位对于Git和GitHub使用不太了解可以参考此教程。

## 一、linux上安装git软件

可以直接从发行版本的源里进行安装
```bash
sudo apt-get install git   //ubuntu发行版下
yum -y install git     //redhat、centos发行版下
```

## 二、使用https用户名密码认证连接github
1. 在github上创建项目
首先需要从github上申请一个帐号，申请完成后在点击右上角的“+”号创建一个新的repository项目，如下：

2. 主机上初始化项目并同步到github服务器上
在linux主机上初始化该项目并同步到github服务器上。
```bash
[root@361way abc]# echo '# 361way' >> README.md
[root@361way abc]# git init
[root@361way abc]# git add README.md
[root@361way abc]# git commit -m "first commit"
[root@361way abc]# git remote add origin https://github.com/91it/361way.git
[root@361way abc]# git push -u origin master
Username for 'https://github.com': 91it
Password for 'https://91it@github.com':
Counting objects: 3, done.
Writing objects: 100% (3/3), 201 bytes | 0 bytes/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To https://github.com/91it/361way.git
 * [new branch]      master -> master
```
分支 master 设置为跟踪来自 origin 的远程分支 master。
[root@361way abc]#

3. 免用户名密码登录
如果想避免交互式输入用户密码，可以将git remote add包更换为：
`git remote add origin  https://用户名:密码@github.com/用户名/repository项目名.git`
配置完后再去git push
已经配置过的可以通过修改本地项目目录下的.git/config文件里的 [remote "origin"] 项下的 url 值，也可以通过git remote set-url origin 指令进行修改：

`git remote set-url origin  https://用户名:密码@github.com/用户名/repository项目名.git`
本地已经存在的项目，可以省去git init的过程，直接执行最后两步push到服务器上

`git remote add origin  https://用户名:密码@github.com/用户名/repository项目名.git`
`git push -u origin master`

## 三、使用ssh key认证连接github项目
某些云主机或vps主机，使用https认证进行连接时会出现403错误，如下：
```bash
[root@91it test]# git push -u origin master
error: The requested URL returned error: 403 Forbidden while accessing https://github.com/91it/test.git/info/refs
fatal: HTTP request failed
```
出现此类情况可以尝试使用ssh key管理。

1、ssh key认证配置
linux主机使用ssh-keygen指令生成key
```bash
[root@361way mnt]# ssh-keygen -t rsa -C "itybku@139.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /root/.ssh/id_rsa.
Your public key has been saved in /root/.ssh/id_rsa.pub.
The key fingerprint is:
8d:da:34:b3:11:f9:58:e7:5e:28:9c:e4:31:3d:df:38 itybku@139.com
The key's randomart image is:
+--[ RSA 2048]----+
|                 |
|         .   .   |
|        o . = oE |
|         B * = o+|
|        S o B . o|
|       + = . o   |
|      . o   .    |
|                 |
|                 |
+-----------------+
```
这里使用邮箱地址是我申请github账号的邮箱地址。将用户家目录下的.ssh/id_rsa.pub的内容复制。

2、github设置及linux主机验证

登录github.com,进入Account Settings，左边选择SSH Keys，Add SSH Key,title随便填，粘贴id_rsa.pub里的内容。配置完成后可以使用以下指令在linux主机上进行验证。
```bash
[root@361way test]# ssh -T git@github.com
Warning: Permanently added the RSA host key for IP address '192.30.252.130' to the list of known hosts.
Hi 91it! You've successfully authenticated, but GitHub does not provide shell access.
```
如果出现上面的提示表示增加key成功，如是出现Agent admitted failure to sign using the key 提示，则可以通过执行下面的指令增加key
```
ssh-add ~/.ssh/id_rsa
```
3、使用ssh协议进行同步
```bash
echo ' # 361way ' >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin git@github.com:91it/361way.git
git push -u origin master
```
ssh协议和https协议url 可以通过修改本地项目目录下的.git/config文件或 `git remote set-url origin` 指令进行修改。

## 四、查看git 项目源
可以通过查看项目下的.git/config文件查看，也可以通过git remote -v指令进行查看，示例如下：
```bash
#https认证
[root@361way test]# git remote -v
origin  https://github.com/91it/test.git (fetch)
origin  https://github.com/91it/test.git (push)
#ssh认证
[root@361way 361way]# git remote -v
origin  git@github.com:91it/361way.git (fetch)
origin  git@github.com:91it//361way.git (push)
```

## 五、其他错误
1、在执行`$ git remote addorigin git@github.com:91it/test.git` 错误提示：fatal: remote origin already exists.
解决方法：
`$ git remote rm origin`
然后在执行：`$ git remote add origin git@github.com:defnngj/hello-world.git`就不会报错误了
2、在执行`$ git push origin master`错误提示：error:failed to push som refs to.......
解决方法：
`$ git pull origin master // 先把远程服务器github上面的文件拉下来，再push 上去。`
