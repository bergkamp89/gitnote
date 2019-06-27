# Git笔记
## Git简介
+ GIT可以说是目前世界上用的最多的、最先进的、最火的分布式版本控制系统了，而且还是开源的。
+ GIT是由Linus花了两周时间自己用C写了一个分布式版本控制系统，当初只是用来管理Linux系统的源码的。后来随着开发的深入，Git 的正常使用都由一些友好的脚本命令来执行，使 Git 变得非常好用，再加上GitHub网站上线了，为开源项目免费提供Git存储，使得Git迅速成为最流行的分布式版本控制系统。
+ 分布式VS集中式
集中式，顾名思义就是有一个服务器集中的存放版本库。在开发的时候呢用的是各自的电脑，首先要（从服务器，也就是版本库）获取最新的版本；然后在进行开发；开发完成之后，再提交代码。集中式版本控制系统必须联网才能工作，这也是它最大的缺点，局域网内的网速快的话还可以接受，可如果网速慢的话，这个体验就会很难受了。集中式版本控制系统还有一大缺点就是：如果中央服务器坏了，开发者将无法再提交修改，开发者之间也无法再协作。
## Git命令
### Git配置
```bash
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```
### 远程仓库
#### 创建SSH Key
```bash
$ ssh-keygen -t rsa -C "youremail@example.com"
```
##### 检查SSH目录生成SSH密钥
1. 进入ssh目录
输入命令 `cd ~/.ssh`
2. 获取ssh密钥信息
ssh目录下的id_rsa.pub为公钥，id_rsa为私钥。github中SSH keys配置的是公钥。
#### 初始化一个Git仓库
```bash
$ git init
```
#### 关联远程仓库
```bash
$ git remote add origin git@github.com:bergkamp89/gitnote.git
```
#### 查看远程仓库
```bash
$ git remote -v
```
#### 从远程克隆
```bash
$ git clone git@github.com:bergkamp89/gitnote.git
```
>执行git clone指令后:
>1. 自动将远程服务器命名为origin
>2. 下载该服务器下的所有数据
>3. 创建一个指向master分支的指针,并将该分支命名为origin/master
>4. 创建名为master的本地分支,并且和远程分支在同一个提交节点
>5. 本地追踪分支master会自动追踪origin/master分支
>git clone默认会把远程仓库整个给clone下来，但只会在本地默认创建一个master分支

### 分支
#### 概念介绍
+ origin 远程服务器
+ origin/master 远程分支
+ master 本地分支
>当执行git init时,master会作为初始分支的默认名字
>当执行git clone时，origin是默认服务器名称
>可以通过指令git clone -o cat,使得默认服务器名称为cat,而默认远程分支为cat/master
>master是默认的本地分支,是远程分支origin/master在本地的拷贝

+ 追踪分支
>从远程分支`checkout`一个本地分支,该本地分支被称为追踪分支(tracking branch),被追踪的分支被称为上游分支(upstream branch),追踪分支可以理解为是和远程分支有直接关联的本地分支.如果我们在追踪分支时执行git pull,git会自动知道需要获取和merge的分支的服务器.

#### 创建分支
```bash
$ git branch branchname
```
#### 查看分支
```bash
$ git branch
```
`git branch`命令会列出所有分支，当前分支前面会有个*号
#### 切换分支
```bash
$ git checkout branchname
```
#### 创建+切换分支
```bash
$ git checkout -b branchname
```
#### 删除分支
```bash
$ git branch -d branchname
```
>-d是--delete的缩写,在使用--delete删除分支时,该分支必须完全和它的上游分支merge完成,如果没有上游分支,必须要和HEAD完全merge

```bash
$ git branch -D branchname
```
>-D是--delete --force的缩写,这样写可以在不检查merge状态的情况下删除分支

#### 删除远程分支
```bash
$ git push origin -d branchname
```
>该指令也会删除追踪分支

#### 删除追踪分支
```bash
$ git branch --delete --remotes origin/remote_branch
```
>该指令可以删除追踪分支,该操作并没有真正删除远程分支,而是删除的本地分支和远程分支的关联关系,即追踪分支

```bash
$ git fetch origin --prune
```
>git在版本1.6.6之后,可以通过git fetch origin --prune或它的简写git fetch origin -p来单独删除追踪分支

#### 合并某分支到当前分支
```bash
$ git merge branchname
```
#### 创建新的追踪分支
```bash
$ git checkout -b branchname origin/remote_branch
```
或者
```bash
$ git checkout --track origin/dev
```
#### 建立本地分支和远程分支的关联(已有的本地分支追踪远程分支)
```bash
$ git branch --set-upstream-to=origin/remote_branch your_branch
```
或者
```bash
$ git branch -u origin/remote_branch
```
#### 查看本地分支与远程分支的关联关系(查看上游分支)
```bash
$ git branch -vv
```
#### 完全获取最新的追踪分支信息
```bash
$ git fetch --all
```
#### 撤销本地当前分支与远程分支的关联
```bash
$ git branch --unset-upstream
```
#### 上游分支的简写
当已经设置了追踪分支,可以通过@{upstream}或 @{u}来引用其上游分支,举例,如果在master分支上,可以通过git merge @{u}等指令来代替git merge origin/master
