---
layout: article
title:  "Introduction of git tag command"
rootCate: "work"
show_edit_on_github: false
pageview: true
comment: true
key: work_version_control_gitintro_git_tag_20190618
categories:
- VersionControl
tags:
- work
- tools
- Git
---

介绍`git tag`的使用方法和技巧，末尾附上更全面的来源资料。

<!---more--->
> StartTime: 2019-06-18,ModifyTime:2019-06-18

## Git tag
1. `git tag -l` 列出所有tag
2. `git show TAG_XXX` 展示某个tag_name的变更和描述
3. `git tag -a XXX -m "XXXX"` 添加一个tag，`-a`是tag名字比如 `v0.1`，`-m`是这个tag的描述。
4. `git push origin tag TAG_NAME` 把本地新增的tag 推到远端
5. `git tag -d XXX` 删除某个tag， 之后把远端的也删除 `git push origin :Release_1_0` 


## Source
[Git语言 的 Git标签操作](http://www.hechaku.com/git/Gitbiaoqiancaozuo.html)