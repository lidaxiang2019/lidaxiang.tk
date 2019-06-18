---
layout: article
title:  "Introduction of Git reset and git rebase command"
rootCate: "work"
show_edit_on_github: false
pageview: true
comment: true
key: work_version_control_gitintro_git_reset_20190618
categories:
- VersionControl
tags:
- work
- tools
- Git
---

介绍 `git reset` 的使用方法和技巧，末尾附上更全面的来源资料。

<!---more--->

## Git reset
1. 撤销最新一次提交的commit
```
git reset --soft HEAD~1
```
2. --mixed 参数: add 和 commit 两步操作都撤销。
```
git reset HEAD
```
3. --hard 删除本地工作空间改动代码，撤销commit，撤销git add . 
```
git reset --hard
```

## Git rebase
常用方法，push之前合并多个本地commit：
1. 先`rebase`要合并的前面的一个commit_id `git rebase -i commit_id`.之后可以修改前面的commit，可以加入文件，可以删除commit。
2. 修改类似下面内容的文件里的第二个开始的所有pick为s
```
pick 16a9a47 update3
pick a7186d8 update2
pick 7b16b28 update1
 # Rebase a9269a3..7b16b28 onto a9269a3 (3 commands)
 #.....
```
3. 一般如果你修改 `pick` 为 `s`，下一步`:x` 保存这个文件后，需要修改commit内容。如果是多个commit间隔几个commit 合并，或者其他rebase操作，还可能需要 `git rebase --continue`，意思是进入下一个commit操作(合并或者修改内容)。

其他参数解释:
```
pick : 保留该commit（缩写:p）
reword : 保留该commit，但我需要修改该commit的注释（缩写:r）
edit : 保留该commit, 但我要停下来修改该提交(不仅仅修改注释)（缩写:e）
squash : 将该commit和前一个commit合并（缩写:s）
fixup : 将该commit和前一个commit合并，但我不要保留该提交的注释信息（缩写:f）
exec : 执行shell命令（缩写:x）
drop : 我要丢弃该commit（缩写:d）

```

高级玩法: 把某个分支的某一段commit复制粘贴到另外一个分支，[startpoint]  [endpoint]指定的是一个前开后闭的区间。
```
git rebase   [startpoint]   [endpoint]  --onto  [branchName]
```

## Source
[git使用情景2：commit之后，想撤销commit](https://blog.csdn.net/w958796636/article/details/53611133)
[Git rebase 介绍](https://www.jianshu.com/p/4a8f4af4e803)