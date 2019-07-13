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
#### 检查SSH目录生成SSH密钥
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
$ git remote add <shortname> <url>
$ git remote add origin git@github.com:bergkamp89/gitnote.git
```
#### 查看已经配置的远程仓库服务器
```bash
$ git remote  效果同  $ git remote show
```
#### 查看远程仓库
```bash
$ git remote -v
```
>显示需要读写远程仓库使用的 Git 保存的简写与其对应的 URL

#### 显示获得远程引用的完整列表
```bash
$ git ls-remote <remote-name>
```
#### 得到远程分支更为详细的信息
```bash
$ git remote show <remote-name>
```
> 参数 remote-name 通常都是缩写名 origin，可以得到远程分支更为详细的信息以及 pull 和 push 相关提示信息。

#### 重命名一个远程仓库的简写名
```bash
$ git remote rename old_name new_name
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

#### 推送到远程仓库
```bash
$ git push -u origin master
```
`-u` 表示第一次推送master分支的所有内容，此后，每次本地提交后，只要有必要，就可以使用命令`git push origin master`推送最新修改。

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
#### 查看远程分支列表
```bash
$ git branch -r
```
#### 显示所有分支，包括本地和远程
```bash
$ git branch -a
```
#### 查看每一次分支的最后一次提交
```bash
$ git branch -v
```
#### 查看本地分支与远程分支的关联关系(查看上游分支)
```bash
$ git branch -vv
```
#### 查看所有已经被 merge 的 branch
```bash
$ git branch --merged
```
#### 查看所有还没被 merge 的 branch
```bash
$ git branch --no-merged
```
#### 删除所有已经被 merge 的 branch
```bash
$ git branch --merged | xargs git branch -d
```
#### 切换分支
```bash
$ git checkout branchname
```
#### 将远程分支迁到本地
```bash
$ git checkout brancdname origin/branchname
```
#### 把远程分支迁到本地顺便切换到该分支
```bash
$ git checkout -b branchname origin/branchname
```
#### 创建+切换分支
```bash
$ git checkout -b branchname
```
#### 在本地创建和远程分支对应的分支
```bash
$ git checkout -b branchname origin/branchname
```
#### 应用某个分支的某一次commitid
```bash
$ git cherry-pick commitid
```
>假如我们在某个 branch 做了一大堆 commit，而当前 branch 想应用其中的一个，可以使用该命令。

#### 删除分支
```bash
$ git branch -d branchname
```
>-d是--delete的缩写,在使用--delete删除分支时,该分支必须完全和它的上游分支merge完成,如果没有上游分支,必须要和HEAD完全merge

```bash
$ git branch -D branchname
```
>-D是--delete --force的缩写,这样写可以在不检查merge状态的情况下删除分支

#### 在本地和远程同步删除分支
```bash
$ git push origin -d branchname
```
>该指令也会删除追踪分支。

#### 删除一个远程分支
```bash
$ git push origin --delete 远程分支
或者
$ git push origin:远程分支
```
>基本上这个命令做的只是从服务器上移除这个指针。 Git 服务器通常会保留数据一段时间直到垃圾回收运行，所以如果不小心删除掉了，通常是很容易恢复的。

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
#### 只把某分支修改的内容合并到当前分支，不合并commitid
```bash
$ git merge --squash branchname 
```
#### 普通模式合并分支
```bash
$ git merge --no-ff -m "description" <branchname>
```
>因为本次合并要创建一个新的commit，所以加上`-m`参数，把commit描述写进去。合并分支时，加上`--no-ff`参数(表示禁用 Fast forward 快进模式)就可以用普通模式合并，能看出来曾经做过合并，包含作者和时间戳等信息，而fast forward合并就看不出来曾经做过合并。

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
或者
```bash
$ git branch --set-upstream branchname origin/branchname
```
#### 完全获取最新的追踪分支信息
```bash
$ git fetch --all
```
#### 撤销本地当前分支与远程分支的关联
```bash
$ git branch --unset-upstream
```
#### 从本地推送分支
```bash
$ git push origin branch-name
```
>如果推送失败，先用git pull抓取远程的新提交；

#### 推送一部分提交
```bash
$ git push <remote-name> <commit SHA>:<remote-branch_name>
```
>push 一部分 commit。例如：git push origin 9790eff:master 即为 push 9790eff 之前的所有 commit 到 master。

#### 抓取远程库最新提交，拉取并合并
```bash
$ git pull
```
>如果有冲突，要先处理冲突。

#### 抓取远程库最新提交，拉取但不合并
```bash
$ git fetch
```
>没有 merge 的 pull

#### 上游分支的简写
当已经设置了追踪分支,可以通过@{upstream}或 @{u}来引用其上游分支,举例,如果在master分支上,可以通过git merge @{u}等指令来代替git merge origin/master

#### git rebase
```bash
$ git rebase 目标分支（通常是 master）
```
>在本地 master 上进行变基操作。注意：merge 与 rebase 都是整合来自不同分支的修改。
>+ merge 会把两个分支的最新快照以及二者最近的共同祖先进行三方合并，合并的结果是生成一个新的快照（并提交）。
>+ rebase 会把提交到某一分支（当前分支）上的所有修改都转移至另一分支（目标分支）上，就好像“重新播放”一样。
>+ 变基是将一系列提交按照原有次序依次应用到另一分支上，而合并是把最终结果合在一起。简言之：这两种整合方法的最终结果没有任何区别，但是变基使得提交历史更加整洁。
>+ 采用变基操作后，项目的最终维护者就不再需要进行整合工作，只需要快进合并便可。
>+ git rebase –ongo 目标分支 第一分支 第二分支：选中在第二分支里但不在第一分支里的修改，将它们在目标分支（通常是 master）上重演。
>+ 变基有风险，需要遵守的准则是：不要对在你的仓库外有副本的分支执行变基。否则，会导致混乱。总的原则是，只对尚未推送或分享给别人的本地修改执行变基操作清理历史，从不对已推送至别处的提交执行变基操作，这样才能享受到两种方式带来的便利。
>+ 还可以有这样的命令：git rebase -i master，git rebase -i 22e21f2，git rebase -i HEAD~3。

### 基本用法
#### 添加文件
```bash
$ git add <file>
```
>把文件添加到 暂存区（stage），可被 track 追踪纪录下来。可多次使用来添加多个文件。
```bash
$ git add *
```
>添加所有修改到暂存区
```bash
$ git add -A
```
>暂存所有的文件，包括新增加的、修改的和删除的文件。
```bash
$ git add .
```
>暂存新增加的和修改的文件，不包括已删除的文件。即当前目录下所有文件。
```bash
$ git add -u
```
>暂存修改的和删除的文件，不包括新增加的文件。
```bash
$ git add -f 文件
```
>git add -f 文件

#### 提交文件
```bash
$ git commit -m "本次提交说明"
```
>一次性把暂存区所有文件修改提交到仓库的当前分支。
```bash
$ git commit -am "本次提交说明"
```
>使用该命令，Git 就会自动把所有已经跟踪过的文件暂存起来一并提交，从而跳过 git add 步骤，参数 -am 也可写成 -a -m。

#### 重新提交
```bash
$ git commit --amend
```
>重新提交，最终只会有一个提交，第二次提交将代替第一次提交的结果。尤其适用于提交完了才发现漏掉了几个文件没有添加，或者提交信息写错了的情况。

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
#### 查看仓库当前的状态的简览
```bash
$ git status -s 或者 $ git status --short
```
#### 查看修改内容
```bash
$ git diff
```
```bash
$ git diff --cached  或者 $ git diff --staged
```
```bash
$ git diff HEAD -- <file>
```
- `git diff` 对比工作区(未 git add)和暂存区(git add 之后)
- `git diff --cached` 对比暂存区(git add 之后)和版本库(git commit 之后)
- `git diff HEAD -- <file>` 对比工作区(未 git add)和版本库(git commit 之后)

#### 比较两次commit之间的差异
```bash
$ git diff commitid1 commitid2
```
#### 比较两个分支之间的差异
```bash
$ git diff branchname1 branchname2
```
#### 查看指定文件具体修改了哪些内容
```bash
$ git diff filename
```
#### 删除文件
```bash
$ git rm <file>
```
`git rm <file>`相当于执行
```bash
$ rm <file>
$ git add <file>
```
> 进一步解释：
> Q：比如执行了`rm text.txt` 误删了怎么恢复？
> A：执行`git checkout -- text.txt` 把版本库的东西重新写回工作区就行了
> Q：如果执行了`git rm text.txt`我们会发现工作区的text.txt也删除了，怎么恢复？
> A：先撤销暂存区修改，重新放回工作区，然后再从版本库写回到工作区
```bash
$ git reset head text.txt
$ git checkout -- text.txt
```
> Q：如果真的想从版本库里面删除文件怎么做？
> A：执行`git commit -m "delete text.txt"`，提交后最新的版本库将不包含这个文件

#### 强制删除版本库中有修改的文件
```bash
$ git rm -f filename
```
#### 把文件从版本库中删除，但让文件保留在工作区且不被 Git 继续追踪（track）
```bash
$ git rm --cached filename
```
>通常适用于在 rm 之后把文件添加到 .gitignore 中的情况。

#### 删除 log/ 目录下扩展名为 .log 的所有文件
```bash
$ git rm log/\*.log
```
#### 删除以 ~ 结尾的所有文件
```bash
$ git rm \*~
```
#### 保存工作现场
```bash
$ git stash
```
#### 查看工作现场
```bash
$ git stash list
```
#### 恢复工作现场，删除stash内容
```bash
$ git stash apply + git stash drop
```
>用 git stash apply 命令恢复最近 stash 过的工作现场，但是恢复后，stash 内容并不删除，用 git stash drop 命令来删除。apply 和 drop 后面都可以加上某一指定的 stash_id。

#### 恢复工作现场
```bash
$ git stash pop
```
>相当于上面两条命令，恢复回到工作现场的同时把 stash 内容也删除了。

#### 清空所有暂存区的 stash 纪录
```bash
$ git stash clear
```
>清空所有暂存区的 stash 纪录。drop 是只删除一条，当然后面可以跟 stash_id 参数来删除指定的某条纪录，不跟参数就是删除最近的。

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
#### 查看最近n条更新日志，并且简单显示出所涉及的文件
```bash
$ git log -n --stat
```
#### 查看每次提交都改动了哪些文件
```bash
$ git log --stat
```
#### 查看最近两次提交的基本信息和每次提交的内容差异
```bash
$ git log -p -2
```
>除显示基本信息之外，还显示每次提交的内容差异，-2 意思是仅显示最近两次提交。特别适用于进行代码审查，或者快速浏览某个搭档提交的 commit 所带来的变化。

#### 查看最后一个 commit 的修改
```bash
$ git show
```
#### 查看倒数第四个 commit 的修改
```bash
$ git show HEAD~3
```
>HEAD~3 就是向前数三个的 commit，即倒数第四个 commit。

#### 查看某次 commit 的修改
```bash
$ git show commitid
```
#### 查看某一次提交所涉及的文件
```bash
$ git show commitid --stat
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
#### 查看命令历史
```bash
$ git reflog
```
### 回退版本
首先，Git必须知道当前版本是哪个版本，在Git中，用HEAD表示当前版本，上一个版本就是HEAD^，上上一个版本就是HEAD^^，往上100个版本就是HEAD~100。
#### 回退到上一个版本
```bash
$ git reset --hard HEAD^
```
#### 回退指定版本号
```bash
$ git reset --hard commitid
```
#### 撤销工作区的修改
```bash
$ git checkout -- <file>
```
>丢弃工作区的修改，包括修改后还没有放到暂存区和添加到暂存区后又作了修改两种情况。总之，让该文件回到最近一次git commit 或git add 之后的状态。注意：没有 – ，就变成了切换分支的命令了。

#### 撤销暂存区的修改
1. 把暂存区的修改撤销掉(unstage)，重新放回工作区：
```bash
$ git reset HEAD <file>
```
2. 撤销工作区的修改:
```bash
$ git checkout -- <file>
```
#### 重置所有文件到未修改的状态
```bash
$ git reset --hard
```
#### 重置到某个 commit
```bash
$ git reset commitid
```
#### 还原某个 commit
```bash
$ git revert commitid
```
>还原（revert）的实质是产生一个新的 commit，内容和要还原的 commit 完全相反。比如，A commit 在 main.c 中增加了三行，revert A 产生的 commit 就会删除这三行。如果我们非常确定之前的某个 commit 产生了 bug，最好的办法就是 revert 它。git revert 后 git 会提示写一些 commit message，此处最好简单描述为什么要还原；而重置（reset）会修改历史，常用于还没有 push 的本地 commits。

#### 还原到上次 commit
```bash
$ git revert HEAD
```
### 标签
>tag就是一个让人容易记住的有意义的名字，它跟某个commit绑在一起。

#### 查看所有标签
```bash
$ git tag
```
#### 新建一个标签
```bash
$ git tag <tagname>  或者  $ git tag <tagname> <commitid>
```
>命令`git tag <tagname>`用于新建一个标签，默认为HEAD，也可以指定一个commit id

#### 查看标签信息
```bash
$ git show <tagname>
```
#### 使用特定的模式查找标签
```bash
$ git tag -l 'tag-name'
```
#### 切换 tag
```bash
$ git checkout <tagname>
```
#### 指定标签信息
```bash
$ git tag -a <tagname> -m <description> <branchname> or commit_id
```
`git tag -a <tagname> -m "here is tag description"`可以指定标签信息。
#### 推送一个本地标签
```bash
$ git push origin <tagname>
```
>默认情况下，git push 命令并不会推送标签到远程，必须显示推送。

#### 推送全部未推送过的本地标签
```bash
$ git push origin --tags
```
>参数 –tags 表示一次性推送全部未推送到远程的本地标签，当其他人从仓库中克隆或拉取，他们也能得到那些标签。

#### 删除一个本地标签
```bash
$ git tag -d <tagname>
```
>因为创建的标签都只存储在本地，不会自动推送到远程。所以，打错的标签可以在本地安全删除。

#### 删除一个远程标签
```bash
$ git push origin :refs/tags/<tagname>
```
>先从本地删除，再用该命令从远程删除。