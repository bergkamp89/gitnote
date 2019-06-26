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
git clone默认会把远程仓库整个给clone下来，但只会在本地默认创建一个master分支
### 分支
#### 创建分支
```bash
$ git branch <branchname>
```
#### 查看分支
```bash
$ git branch
```
`git branch`命令会列出所有分支，当前分支前面会有个*号
#### 切换分支
```bash
$ git checkout <branchname>
```
#### 创建+切换分支
```bash
$ git checkout -b <branchname>
```
#### 删除分支
```bash
$ git branch -d <branchname>
```
#### 合并某分支到当前分支
```bash
$ git merge <branchname>
```