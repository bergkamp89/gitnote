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

### 基本用法
#### 添加文件到Git仓库
```bash
$ git add <file>
$ git commit -m "description"
```
>`git add`可以反复多次使用，添加多个文件到仓库，`git commit`可以一次提交很多文件到仓库，`-m`后面输入的是本次提交的说明，可以输入任意内容。

#### 查看仓库当前的状态
```bash
$ git status
```
#### 查看修改内容
```bash
$ git diff
```
```bash
$ git diff --cached
```
```bash
$ git diff HEAD -- <file>
```
- `git diff` 对比工作区(未 git add)和暂存区(git add 之后)
- `git diff --cached` 对比暂存区(git add 之后)和版本库(git commit 之后)
- `git diff HEAD -- <file>` 对比工作区(未 git add)和版本库(git commit 之后)

### 查看日志
#### 查看提交日志
```bash
$ git log
```
>显示从最近到最远的提交日志，包括每个提交的 SHA-1 校验和、作者的名字和电子邮件地址、提交时间以及提交说明等基本信息。

#### 按数量查看
```bash
$ git log -n
```
#### 按日期查看
```bash
$ git log --after="2019-06-29"
$ git log --after="yesterday"
$ git log --after="2019-06-20" --before="2019-06-29"
```
>注意--since --until和 --after --before是一个意思.

#### 按作者
```bash
$ git log --author="Bergkamp"
$ git log --author="Arsenal\|Bergkamp"
```
#### 按commit描述
```bash
$ git log --grep="#bug-154"
```
>可以传入-i忽略大小写

#### 按文件
```bash
$ git log -- <file>
```
>这里的--是告诉Git后面的参数是文件路径而不是branch的名字. 如果后面的文件路径不会和某个branch产生混淆, 你可以省略--.

#### 按内容
```bash
$ git log -S "Hello,World!"
```
>有时想搜索和新增或删除某行代码相关的commit. 可以使用`－S"<string>"`.如果你想使用正则表达式去匹配而不是字符串, 那么你可以使用-G代替-S.
>这是一个非常有用的debug工具, 使用他你可以定位所有跟某行代码相关的commit. 甚至可以查看某行是什么时候被copy的, 什么时候移到另外一个文件中去的.

#### 按范围
```bash
$ git log master..feature
```
>master..feature这个range包含了在feature有而在master没有的所有commit.

#### 查看分支合并图
```bash
$ git log --graph
```
#### 简化日志信息(将每个提交放在一行显示)
```bash
$ git log --pretty=oneline
```
#### 查看分支的合并情况，包括分支合并图、一行显示、提交校验码缩略显示
```bash
$ git log --graph --pretty=oneline --abbrev-commit
```
#### 让git log显示每个commit的引用(如:分支、tag等)
```bash
$ git log --oneline --decorate
```
#### 过滤merge commit
```bash
$ git log --no-merges
```
#### 只显示merge commit
```bash
$ git log --merges
```
### 回退版本
首先，Git必须知道当前版本是哪个版本，在Git中，用HEAD表示当前版本，上一个版本就是HEAD^，上上一个版本就是HEAD^^，往上100个版本就是HEAD~100。
#### 回退到上一个版本
```bash
$ git log --hard HEAD^
```
