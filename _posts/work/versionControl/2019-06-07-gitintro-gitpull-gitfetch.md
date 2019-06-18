---
layout: article
title:  "Introduction of Git pull and git fetch command"
rootCate: "work"
show_edit_on_github: false
pageview: true
comment: true
key: work_version_control_gitintro_git_pull_20190607
categories:
- VersionControl
tags:
- work
- tools
- Git
---

介绍`git pull` `git fetch`的使用方法和技巧，末尾附上更全面的来源资料。

<!---more--->

## Git pull
1. 后面两个参数可以不加，就会默认拉取当前分支对应的远程分支
```
git pull [远程主机名, 比如 origin]  [分支名 比如 master或develop]  
```
2. 这个参数可以更新本地分支的HEAD标签，使得与某个远程分支(一般是master或者develop) 最新HEAD一致，这样你再提交commit，就会让远程分支合并后只前进一个commit节点，不会出现从历史某个commit突然跳跃到前方。尤其图形化该分支所有合并历史的时候，会明显发现在`push`每个分支前使用了 `git pull origin master--rebase`这一步骤的项目git历史，会变得更加整洁。  更大的好处是，不仅可以避免冲突，而且以后发现某个分支出现bug，可以直接剔除某个commit合并，而不用担心出现其他问题。
```
git pull origin master/develop --rebase   
```
3. 如果出现冲突，需要结合 `git add .` 、 `git rebase --continue` 命令一轮轮解决冲突。

## Git fetch
一旦远程主机的版本库有了更新（Git术语叫做commit），需要将这些更新取回本地，这时就要用到git fetch命令。
1. 拉取远端版本库的git内容更新而不是代码
```
git fetch <远程主机名> <分支名>
```

2. 精简/删除 本地存在但远程已被删除的分支。
```
git fetch -p  #-p , prune remote-tracking branches no longer on remote
```