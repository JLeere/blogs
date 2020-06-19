---
title: Github学习笔记
date: 2020-06-15 16:15:29
categories:
- 工具技能学习
- Github
tags:
- Github
---

## github操作指令
[教程](https://www.runoob.com/git/git-tutorial.html)

## 工作流程
- 克隆 Git 资源作为工作目录。
- 在克隆的资源上添加或修改文件。
- 如果其他人修改了，你可以更新资源。
- 在提交前查看修改。
- 提交修改。
- 在修改完成后，如果发现错误，可以撤回提交并再次修改并提交。

## 基本概念
1. 工作区:当前电脑下的工作目录空间
2. 暂存区index:.git文件夹中,保存目录文件的索引
3. 版本库: 远端存储的工作空间
![1](基本概念.png)

## 操作
### 创建仓库
如果要自己定义要新建的项目目录名称，可以在上面的命令末尾指定新的名字：
```$ git clone git://github.com/schacon/grit.git mygrit```

### 基本指令
git add. //add all files of workspace工作区 to stage暂存区
git commit -m "add files" // 将暂存区的文档commit到本地的master，并记录此次commit消息的名称为“add files”
git push //将本地master的文档push到Github的master上保存实现同步
git status
git log --oneline //查看历史记录简洁信息
git commit -am "修改..." //跳过git add步骤

## 分支
git branch newbranchname
git branch //查看分支
git checkout branchname //切换到分支
git checkout -b newbranchname
git merge branchname //合并分支到master
git branch -d (name)

## Github 
有两种连接远程仓库的模式,一个是Http,每次修改都要输入帐号密码http://github.com；一个是SSH,将本地计算机的锁和钥匙添加到账户里面就不用每次输入了.
但是在clone仓库的时候就要选择相应的方式git@github.com:xiaozhenc/rep
https://www.jianshu.com/p/c9aa544a11d3
为了避免每次push都输密码,可以生成SSH钥匙和锁
ssh-keygen -t rsa -C "email@example.com"