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


## Code
```
$ mkdir git-repos
$ cd git-repos
$ git clone https://github.com/AlracWebmaven/playground.git
Cloning into 'playground'...
remote: Counting objects: 4, done.
remote: Compressing objects: 100% (4/4), done.
remote: Total 4 (delta 0), reused 0 (delta 0)
Unpacking objects: 100% (4/4), done.
Checking connectivity... done.
$ ls playground/
LICENSE  README.md
```

### Branching

Git branches are gloriously excellent for safely making and testing changes. You can create and destroy them all you want. Let's make one for editing README.md:

`$ cd playground`
`$ git checkout -b test`
Switched to a new branch 'test'

Run git status to see where you are:

`$ git status`
On branch test
nothing to commit, working directory clean

What branches have you created?

`$ git branch`
* test
  master

The asterisk indicates which branch you are on. master is your main branch, the one you never want to make any changes to until they have been tested in a branch. Now make some changes to README.md, and then check your status again:

`$ git status`
On branch test
Changes not staged for commit:
  (use "git add ..." to update what will be committed)
  (use "git checkout -- ..." to discard changes in working directory)
        modified:   README.md
no changes added to commit (use "git add" and/or "git commit -a")

Isn't that nice, Git tells you what is going on, and gives hints. To discard your changes, run

`$ git checkout README.md`

Or you can delete the whole branch:

`$ git checkout master`
`$ git branch -D test`

Or you can have Git track the file:

`$ git add README.md`
`$ git status`
On branch test
Changes to be committed:
  (use "git reset HEAD ..." to unstage)
        modified:   README.md

At this stage Git is tracking README.md, and it is available to all of your branches. Git gives you a helpful hint-- if you change your mind and don't want Git to track this file, run git reset HEAD README.md. This, and all Git activity, is tracked in the .git directory in your repository. Everything is in plain text files: files, checksums, which user did what, remote and local repos-- everything.

What if you have multiple files to add? You can list each one, for example git add file1 file2 file2, or add all files with git add *.

When there are deleted files, you can use git rm filename, which only un-stages them from Git and does not delete them from your system. If you have a lot of deleted files, use git add -u.

Committing Files

Now let's commit our changed file. This adds it to our branch and it is no longer available to other branches:

`$ git commit README.md`
[test 5badf67] changes to readme
 1 file changed, 1 insertion(+)

You'll be asked to supply a commit message. It is a good practice to make your commit messages detailed and specific, but for now we're not going to be too fussy. Now your edited file has been committed to the branch test. It has not been merged with master or pushed upstream; it's just sitting there. This is a good stopping point if you need to go do something else.

What if you have multiple files to commit? You can commit specific files, or all available files:

`$ git commit file1 file2`
`$ git commit -a`

How do you know which commits have not yet been pushed upstream, but are still sitting in branches? git status won't tell you, so use this command:

`$ git log --branches --not --remotes`
commit 5badf677c55d0c53ca13d9753344a2a71de03199
Author: Carla Schroder
Date:   Thu Nov 20 10:19:38 2014 -0800
    changes to readme

This lists un-merged commits, and when it returns nothing then all commits have been pushed upstream. Now let's push this commit upstream:

`$ git push origin test`
Counting objects: 7, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 324 bytes | 0 bytes/s, done.
Total 3 (delta 1), reused 0 (delta 0)
To https://github.com/AlracWebmaven/playground.git
 * [new branch]      test -> test

You may be asked for your Github login credentials. Git caches them for 15 minutes, and you can change this. This example sets the cache at two hours:

`$ git config --global credential.helper 'cache --timeout=7200'`

Now go to Github and look at your new branch. Github lists all of your branches, and you can preview your files in the different branches (figure 2).

fig-2 github

Figure 2: Your new commit and branch in Github.

Now you can create a pull request by clicking the Compare & Pull Request button. This gives you another chance to review your changes before merging with master. You can also generate pull requests from the command line on your computer, but it's rather a cumbersome process, to the point that you can find all kinds of tools for easing the process all over the Web. So, for now, we'll use the nice clicky Github buttons.

Github lets you view your files in plain text, and it also supports many markup languages so you can see a generated preview. At this point you can push more changes in the same branch. You can also make edits directly on Github, but when you do this you'll get conflicts between the online version and your local version. When you are satisfied with your changes, click the Merge pull request button. You'll have to click twice. Github automatically examines your pull request to see if it can be merged cleanly, and if there are conflicts you'll have to fix them.

Another nice Github feature is when you have multiple branches, you can choose which one to merge into by clicking the Edit button at the right of the branches list (figure 3).

fig-3 github

Figure 3: Choosing which branches to merge.

After you have merged, click the Delete Branch button to keep everything tidy. Then on your local computer, delete the branch by first pulling the changes to master, and then you can delete your branch without Git complaining:

`$ git checkout master`
`$ git pull origin master`
`$ git branch -d test`

You can force-delete a branch with an uppercase -D:

`$ git branch -D test`

Reverting Changes

Again, the Github pointy-clicky way is easiest. It shows you a list of all changes, and you can revert any of them by clicking the appropriate button. You can even restore deleted branches.

You can also do all of these tasks exclusively from your command line, which is a great topic for another day because it's complex. For an exhaustive Git tutorial try the free Git book.
