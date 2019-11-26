---
layout: post
title: Git学习笔记
date: 2017-07-02
Author: yifei
categories: 
tags: [Git, markdown]
comments: true
---

## Git学习笔记

### 安装

```shell
add-apt-repository ppa:git-core/ppa #添加源
apt-get install git
```

### Git基本操作
把某个项目加入git管理
```
git init  #初始化
```
修改代码后提交git,若修改代码处于`modified`可省去`add`
```
git add README  #add new file
git commit -m '描述信息' #modified代码
```
复制git仓库代码(克隆)
```
git clone git://github.com/schacon/grit.git
```
检查当前文件状态
```
git status
```
一共分为四个状态
- untracked
- unmodified
- modified
- staged

移除文件
```
rm filename
git rm filename
```

查看历史
```
git log -p -2 #查看最近的2条修改历史，并显示修改详情
  			 --pretty=oneline #一行显示
git last #最近的修改信息
```
撤销修改
1、忘记add文件
```
git commit -m 'initial commit'
git add forgotten_file
git commit --amend
```
2、取消对文件的修改,包括`add`
```
git checkout
```
**远程仓库**
```
git remote add origin http://xxx #添加远程仓库 
git fetch [remote-name] #拉取远程仓库代码和分支
git push origin master #推送代码到远程仓库和指定分支
```
### git分支
`HEAD`不直接指向提交，而是指向当前分支，由当前分支指向提交，创建新的分支`dev`时，`HEAD`指向`dev`，当`dev`分支完成提交需要合并时，`master`指向`dev`的当前提交，所以，创建和合并分支都是轻量级的，因为只需要修改指针的位置
```
git branch  #查看分支

git branch <name>  #创建分支

git checkout <name>或者git switch <name>  #切换分支

git checkout -b <name>或者git switch -c <name>  #创建+切换分支

git merge <name> #合并某分支到当前分支

git branch -d <name>  #删除分支
```

**解决冲突**
当两个分支提交同一个文件，合并时，会产生冲突，需要手动解决，打开有冲突的文件，手动删除不需要的内容，保存后重新提交`commit`，随后自动完成合并
```
git log --graph --pretty=oneline --abbrev-commit
```

首先，`master`分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；
那在哪干活呢？干活都在`dev`分支上，也就是说，`dev分支`是不稳定的，到某个时候，比如1.0版本发布时，再把`dev`分支合并到`master`上，在`master`分支发布1.0版本；你和你的小伙伴们每个人都在`dev`分支上干活，每个人都有自己的分支，时不时地往`dev`分支上合并就可以了。

**bug分支**
当前分支的任务未完成时需要临时修复其他bug，要先保存当前的工作环境，再切换到其他分支修改，完成bug修改后，再把代码"复制"到未完成的工作分支，即可开始继续写新的bug
```
git stash #存储当前工作环境
git stash pop #恢复工作环境，并删除stash内容
git cherry-pick 4c805e2 #复制修复好的bug代码到工作分支，或者直接在工作分支修复bug，可省去该步骤
```

**feature分支**
如果分支上的功能因某些原因没办法合并，需要删除，使用`-D`强制删除
```
git branch -D <name>
```
