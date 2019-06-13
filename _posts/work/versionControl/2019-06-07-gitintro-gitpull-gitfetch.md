---
layout: article
title:  "Introduction of Git pull and git fetch command"
rootCate: "work"
categories:
- VersionControl
tags:
- work
- tools
- Git
---

介绍`git log`的使用方法和技巧，末尾附上更全面的来源资料。

<!---more--->

## Git pull
一般情况下(单人单仓库使用默认master开发)，使用下面三条命令就足够用：  
```
git fetch -h
```




## Git fetch
一旦远程主机的版本库有了更新（Git术语叫做commit），需要将这些更新取回本地，这时就要用到git fetch命令。

```
git fetch <远程主机名> <分支名>
```
精简/删除 本地存在但远程已被删除的分支。
```
git fetch -p  #-p , prune remote-tracking branches no longer on remote
```