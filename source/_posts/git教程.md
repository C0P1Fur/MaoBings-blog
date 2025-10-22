---
title: git分支部分教程
date: 2025-10-16 22:46:06
tags:
 - tutorial
catagories:
 - git
feature: true
---
> 得先说明，这些东西都是写给我自己看的
> 用于梳理自己的思路，以及之后复习用
> 所以某些地方写的不是很清楚

## 0.git分支特性简介
git的分支模型具有轻量的特点，支持在工作流程中频繁地使用分支和合并，这使得它在一众版本控制系统中脱颖而出
git保存数据的方式：保存某一时刻的快照

## 1.git提交对象与分支

#### 1.1 提交对象概念与新建提交对象
`git add .`
`git commit -m "some changes"`

第一条指令将根目录的文件全部存储在暂存区（可以理解为缓冲区），方便之后的commit提交
第二条将指令将暂存区里的文件提交到分支上，这一步，git做了以下的操作：
- blob对象，指向每个提交的文件，保存者该文件的当前快照和哈希值
    - 快照，相当于给文件存了个档，记录了当前文件的状态
    - 哈希值，方便git检测文件是否改变
- 树对象，指向blob对象的索引，记录着文件结构
- 提交对象，指向树对象，同时包含着提交的信息

上面对象的包含关系呈现如下：
- 提交对象
    - 树对象
        - blob对象
        - blob对象
        - blob对象

这样，一个提交对象就形成了，不同提交对象之间的关系呈现如下
提交对象1 <- 提交对象2 <- 提交对象3
#### 1.2 分支概念与新建分支
git的分支，本质上是指向不同提交对象的指针，git的默认分支为master
使用`git branch demo`会在当前提交对象上新建demo分支
这样，我们就有两个分支，master与demo，它们都指向同一个提交对象

那么，两个分支指向同一个提交对象，git是如何确定当前是在那个分支上的呢
那就是Head头指针，它没有特殊的含义，只是标识当前位于哪个分支上（可以理解当前的工作路径）

可以使用`git log --oneline --decorate`查看分支指向的提交对象
#### 1.3 分支切换
`git checkout demo`这条命令将head指针切换到demo分支上
此时再新建提交对象，提交对象就会在当前head指针所在的分支上创建
同时将head指针移动到新建的对象上
#### 1.4 查看提交对象结构
`git log --oneline --decorate --graph --all`显示提交对象的结构，以及各个分支指向的地方

## 2.git分支的合并
git中的分支合并有两种类型
#### 2.1 快进合并
如果main分支所在的提交对象是demo分支的父对象
`git checkout main`
`git merge demo`
git所做的就是直接将main分支移动到demo分支所在的位置
#### 2.2 融合合并
main分支不是demo分支的父提交对象
此时融合就是将两个提交对象融合到一起，形成一个新的提交对象
并且将分支移动到新的提交对象上

此时，如果两个提交对象的内容有冲突，git就会要求你处理冲突
冲突处理完毕后才能真正新建提交对象

## 3.git分支的管理
#### 3.1 检查分支状态
`git branch`查看当前有哪些分支，可以添加参数-v查看详细内容
`git branch --merged`查看与当前分支合并了的分支
`git branch --no-merged`查看未与当前分支合并的分支
#### 3.2 常见的分支开发工作流
- 长期分支
    - 提交对象实际上应该是线性的
    - 分为main、develop、top三个分支
    - 每个分支由一个分支指针控制
- 主题分支
    - 不同的分支由主干分支上生发出来，提交对象是树状的
    - 可以有多个不同的分支

长期分支和主题分支可以同时进行，可以将长期分支的main、develop、top分支当成主题分支的主干
#### 3.3 远程分支
常见交互远程分支的命令
- `git remote add 远程服务器名 url`添加git源，并为其命名
- `git pull origin main`将远程分支拉取到当前分支
    - 这条命令相当于`git fetch origin main`+`git merge origin main`
    - 先拉取，在融合（解决冲突）
- `git push origin main`将当前分支推送到远程分支
    - 如果本地与远程分支冲突，会推送失败
    - 这种情况要先拉取解决冲突，再推送
- `git branch -u origin main`将当前分支与远程分支绑定
    - 绑定并不会实时跟踪远程分支的状态
    - 只是起到简化代码的作用，如`git push origin main`简化为`git push`
- `git checkout -b demo origin/demo`在远程分支的地方创建一个本地分支，并将两者绑定
    - 捷径：`git checkout --track origin/main`
    - 或：`git checkout main`，直接追踪远程得到main分支
- `git push origin --delete main`删除一个远程分支
- `git remote -v`查看远程git源列表
