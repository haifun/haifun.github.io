---
layout: post
title:  "git 常用命令"
date:   2014-07-02 10:36:52
categories: git
---

##一，git的个人本地使用及操作

 
1，  创建Git库

    cd 源码目录
    git init    #初始化  在源码目录内生成一个.git的目录
 
2，  注册用户信息

    git config --global user.name XXX      用户名
    git config --global user.email XXX     用户邮箱
    git config –list                       #查看用户信息
 
3，  向git库中添加或删除文件

    git add XX                 #加单个文件
    git add .                  #加所有
    git add [path］会把对应目录或文件，添加到stage状态
    git add . 会把当前所有的untrack files和changed but not updated添加到stage状态
 
4，  向版本库提交变化

    git commit –m “XXXX”        #直接添加简单提交信息,添加注释
    git status                  #查看当前代码库的状态
    git log                     #查看版本信息
    git log –p                  #查看版本信息并显示每次修改的diff
    git show sdjf974654dd….     #查看指定版本信息
                                #(show后面为每次提交系统自动生成的一串哈希值)
    git show sdji97             #一般只使用版本号的前几个字符即可
 
5，  撤销与恢复

    git reset
    git reset --hard                #回到原来编辑的地方,改动会丢失。
                                    #（同样适用于团队对于其他人的修改恢复）
    git reset --hard sdv143kvf…...  #可回到指定的版本
                                    #(hard后面为每次提交系统自动生成的一串哈希值)
     
    git reset [path] 会改变path指定的文件或目录的stage状态，到非stage状
    git reset 会将所有stage的文件状态，都改变成非stage状
 
回退1个change的写法就是git reset HEAD^，2个为HEAD^^，3个为HEAD~3，以此类推。
 
6，  向服务器提交变化

    git push                   #向服务器提交
 
7, 暂存改动

    git stash可以把当前的改动（stage和unstage，但不包括untrack的文件）暂存。然后通过git stash list查看。并通过git stash apply重新取出来。但apply之前要保证worktree是干净的。
 
##二，git的团队开发及操作

1，  获取项目

    cd 本地工作目录（自定）
    git clone 服务器帐户@IP：项目.根路经
这里具体操作为：

    git clone git@192.168.20.22：android2.2.git
 
说明：这里假定服务器的用户名为git，服务器IP为192.168.20.22，项目名为android2.2，根路经为git的home（即根路径）

2，  团队开发的基本流程

    git add 改动的文件
    git  commit  #（提交至本地）
    git  pull     #（将服务器项目与本地项目合并）
    git  push    #（将本地项目上传至服务器）（在提交前要git  pull  --rebase一下，确保当前的本地的代码为最新。）
 
##三，git的分支管理

git分支操作在本地建立分支，然后与本地主枝合并，最终提交到服务器。有效的避免了因个人操作不当向服务器提交过多脏数据，避免频繁git clone服务器来更新本地库。

分支操作指令：

1，  建立分支

    git branch AAA     #建立分支AAA
2，分支切换

    git checkout AAA    #从当前分支切换到AAA分支
3，  将分支与主枝master合并

    git checkout master     #（首先切换回主枝）
    git merge AAA         #（将分支AAA与主枝合并）
4，  当前分支查看

    git  branch            #默认有master（也称为主枝）
    git  branch –a 查看当前所有分支
5，  删除分支

    git branch –d  AAA     #删除分支AAA