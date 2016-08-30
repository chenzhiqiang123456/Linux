#git分布式版本控制系统

##1、git与SVN的区别
**1. git是分布式版本控制系统，SVN是集中式版本控制系统**

>集中式版本是集中存在中央服务器的，而干活的时候，用的都是自己的电脑
>
>  集中式版本控制系统最大的毛病就是必须联网才能工作，如果在局域网内还好，带宽够大，速度够快，可是如果在互联网上，遇到网速慢的话，可能提交一个10M的文件就需要6分钟，

**2. 在SVN中，分支 是一个完整的目录，且这个目录拥有完整的实际文件，而Git 的分支相比 SVN 要轻量很多，这是因为 Git 分支并不是复制一个新仓库，而是为一个分支存储一个指针，这个指针将指向某个提交对象**

**3. 异地协同工作**

**4. 重写提交说明** 

**5. 更好的差异比较**

**6. 工作进度保存**

**7. git快 提交快， 克隆分支快，克隆时Git只是获取文件的每个版本的 元素**
###Git 的工作原理
- 工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库
- Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD。

- 第一步是用git add把文件添加进去，实际上就是把文件修改添加到暂存区；
- 第二步是用git commit提交更改，实际上就是把暂存区的所有内容提交到当前分支。
- 因为我们创建Git版本库时，Git自动为我们创建了唯一一个master分支，所以，现在，git commit就是往master分支上提交更改。
- 你可以简单理解为，需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。
- 如果新添加一个文件 然后用git status 查看状态是Untreacked表示从来没有添加过
- Git非常清楚地告诉我们，readme.txt被修改了，而LICENSE还从来没有被添加过，所以它的状态是Untracked添加后index是这样的

###为GitHub账号添加SSH Keys
- 以公钥认证方式访问SSH协议的Git服务器时无需输入口令，而且更安全。（访问HTTP协议的Git服务器时，比如提交修改，每次都需要输入口令。）
 1. 创建SSH key 
     ssh-keygen -t rsa -C "chenzhiqiangcto@163.com"
 2. 复制公钥
     vim ~/.ssh/id_rsa.pub
 3. 添加GitHub 
     github-> Accounting settings图标-> SSH key-> Add SSH key-> 填写SSH key的名称（可以起一个自己容易区分的），然后将拷贝的~/.ssh/id_rsa.pub文件内容粘帖-> add key”按钮添加。
 4. 测试
     ssh -T git@github.com
 
##2、 Git及强大的分支管理
> 每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”
> Git只能跟踪文本文件的改变，而对于图片、视频这些二进制文件，虽然也能有版本控制管理，但无法跟踪文件的变化
###编译安装
<pre>
yum install curl-devel expat-devel gettext-devel openssl-devel zlib-devel gcc perl-ExtUtils-MakeMaker -y
wget http://codemonkey.org.uk/projects/git-snapshots/git/git-2016-06-28.tar.xz
tar xf git-2016-06-28.tar.xz 
make prefix=/usr/local all
make prefix=/usr/local/git install

</pre>

###2.1 创建版本库
[root@web ~]# mkdir gitdopot     创建目录
[root@web ~]# cd gitdopot/
[root@web gitdopot]# git init         创建版本库
Initialized empty Git repository in /root/gitdopot/.git/
###2.2 添加文件到Git仓库分为两步
<pre>
第一步 git add file  可以反复多次使用添加多个 git add file file2
第二步 git commit -m "说明"  使用-m可以说明提交内容的大意
</pre>

###2.3 查看仓库当前的状态
<pre>
root@web gitdopot]# git status  
#       modified:   readme.txt               表示修改了readme.txt文件
#
no changes added to commit (use "git add" and/or "git commit -a") 表示还没有进行添加和commit
#################
[root@web gitdopot]# git status    git告诉我们当前没有需要提交的修改，而且目录是干净的
# On branch master
nothing to commit (working directory clean)
</pre>

###2.4 修改前后对比
[root@web ~]# git diff  file 显示的格式正式Unix通用的diff格式

##3、版本回退
- HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令git reset --hard commit_id。
- 穿梭前，用git log可以查看提交历史，以便确定要回退到哪个版本。
- 要重返未来，用git reflog查看命令历史，以便确定要回到未来的哪个版本。
- Git必须知道当前版本是哪个版本，在Git中，用HEAD表示当前版本 上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100

###3.1 查看每次提交的历史记录
<pre>
root@web gitdopot]# git log
[root@web gitdopot]# git log --pretty=oneline (减少输出信息)
269d81ae6793cf1ebd4aa9788bf7801040782e27 add chen readme.file    
10af44ce083f15efa91f745338d58f1ea30f4d64 wrote a readme.file
前面的数字是commit的ID号；和SVN不一样，Git的commit ID不是1，2，3，....递增的数字，而是一个SHA1计算出来的非常大的数字，用十六进制表示
</pre>
###3.2 版本回退
<pre>
[root@web gitdopot]# git reset --hard HEAD^ 表示回退到上个版本的add chen readme.file
[root@web gitdopot]# git reset --hard 1a99 根据commit的ID号来回退到以前的版本
</pre>

###3.3 git的历史记录 git reflog
<pre>
[root@web gitdopot]# git reflog  git reflog用来记录你每一次的命令
</pre>

###3.4 Git的远程库
<pre>
git remote add origin git@github.com:chenzhiqiang123456/CentOS7.git

git push -u origin master 第一次推送master分支的所有内容
此后，每次本地提交后，只要有必要，就可以使用命令git push origin master推送最新修改；

</pre>

###3.5 克隆远程仓库
<pre>
git clone git@github.com:chenzhiqiang123456/Linux2.git
Git 支持多种协议，包括https,但是通过ssh支持的原生git协议速度最快
</pre>

###3.6 恢复修改
<pre>
git checkout -- file  意思是把file文件在工作区修改全部撤销，有两种情况
一种 是file文件自修改还没有暂存区，现在撤销修改就回到版本库一模一样的状态
一种 file文件已添加到暂存区，又做了修改，撤销修改就回到添加到暂存区后的状态，
总之，就是让这个文件回到最近一次git commit 或者git add时状态

</pre>
###3.7 删除文件
<pre>
如果当你添加一个文件add，然后又删除了，有两种情况
1. 彻底删除 git rm file
2. 恢复删除  git checkout -- file
</pre>
##4、 Git分支
- 如果在分支上修改文件并且提交，那么在切换到master主支上修改的文件不会发生改变
- 与谁合并就切换到那个支(分支或者master)
###4.1 创建分支
<pre>
[root@web gitdopot]# git checkout -b dev   创建分支，并且切换到dev分支
Switched to a new branch 'dev'
相当于：
    git branch dev    创建分支
    git checkout dev   切换分支

</pre>
###4.2 查看当前分支
<pre>
[root@web gitdopot]# git branch   查看当前分支
* dev
  master
</pre>
###4.3 合并分支
<pre>
[root@web gitdopot]# git merge dev  切到master主支上然后合并分支dev
root@web gitdopot]# git branch -d dev             删除分支dev
Deleted branch dev (was 8e6c00a).
[root@web gitdopot]# git branch
* maste

git merge --no-ff -m "merge with no-ff" dev
准备合并dev分支，请注意--no-ff参数，表示禁用Fast forward：
</pre>

###4.4 Git功能储藏
<pre>
[root@web gitdopot]# git stash  
幸好，Git还提供了一个stash功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作：
[root@web gitdopot]# git stash list          查看stash里内容
stash@{0}: WIP on dev: a3f1a1f dev is
</pre>
###4.5 储藏的恢复
<pre>
(1) 一是用git stash apply恢复，但是恢复后，stash内容并不删除，你需要用git stash drop来删除；
(2) 另一种方式是用git stash pop，恢复的同时把stash内容也删了：

你可以多次stash，恢复的时候，先用git stash list查看，然后恢复指定的stash，用命令：
[root@web gitdopot]# git stash apply stash@{0}
</pre>

##5、Git标签管理
- 发布一个版本时，我们通常 先在版本库中打一个标签，这样就唯一确定了打标签时刻的版本。将来无论什么时候，取某个标签的版本，就是把那个打标签的时刻的历史版本取出来。所以，标签也是版本库的一个快照。

###5.1 创建标签
<pre>
git checkout master  切到master
git tag v1.0           打标签
git  tag               查看所有标签
</pre>
###5.2 根据commitID 来打标签
<pre>
git log --pretty=oneline --abbrev-commit  查看历史
git tag v0.9 6224937  可以指定commit ID 来打标签
</pre>

###5.3 删除标签
<pre>
git tag -d v0.1  删除标签
如果要删除远程标签
1. 首先删除本地标签
2. 然后远程删除  git push origin :refs/tags/v0.9
</pre>

###5.4 推送标签到远程
<pre>
git push origin v0.1   推送标签到远程
git push origin --tags    推送尚未推送到远程的本地标签
</pre>

##6、Git 忽略特殊文件
  > 在Git工作区的根目录下创建一个特殊的.gitignore文件，然后把要忽略的文件名填进去，Git就会自动忽略这些文件

1. 假设你在Windows下进行Python开发，Windows会自动在有图片的目录下生成隐藏的缩略图文件，如果有自定义目录，目录下就会有Desktop.ini文件，
2. 继续忽略Python编译产生的.pyc、.pyo、dist等文件或目录：

3. git check-ignore -v file   #在添加fiee报错是因为.gitignore文件规则限制,你可用此命令检查是那条命令

4. 忽略某些文件时，需要编写.gitignore；

5. .gitignore文件本身要放到版本库里，并且可以对.gitignore做版本管理！

##7、Git 特殊设置
###7.1  颜色设置
<pre>
 git config --global color.ui true  让 Git适当显示颜色
</pre>

###7.2别名设置
<pre>
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"

git lg
每个仓库的Git配置文件都放在.git/config文件中：
</pre>

<font size=4 color=red>Git 远程拉取实践</font>
<pre>
mkdir Image   创建与项目相同的名称
cd Image/
rz -y  elk流程图.jpg
git add Image/*
git commit -m "1"
git remote add origin git@github.com:chenzhiqiang123456/Image.git##<br/>如果报错则 git remote rm origin
git push -u origin master   #第二次提交则不用加-u ##
<font size=2 color=red>如果报错则 git push -f 则会提交文件</font>
添加新的远程仓库 git remote set-url --add --push origin git@github.com:chenzhiqiang123456/Linux.git
</pre>