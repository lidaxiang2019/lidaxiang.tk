---
layout: article
title:  "Introduction of git log and git show command"
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
> StartTime: 2017-03-22,ModifyTime:2018-11-29

## Git log
1. 按提交信息来过滤提交，你可以使用 `--grep` 和 `--author` 标记，前者搜索的是提交信息(如果你的commit内容里包含了issue编号，这个对你很有帮助)，后者搜索的是作者。
```
git log --grep="XXX"
```
2. `--graph` 是在左侧简单构造一个链接关系， `--oneline`则是简化commit内容为一行，`--decorate=short` 会把每个commit 关联的分支或者标签很有用
```
git log --graph --oneline --decorate=short
```
3. `git log --stat` 选项显示每次提交的文件增删数量
4. `git shortlog` 按人分类，列出这个人的提交内容，只有一行。可以用于发布更新摘要声明。
5. `--pretty=format:"<string>"` 用于格式化输出 log 内容，例如`%cn`、`%h` 和 `%cd` 这三种占位符会被分别替换为作者名字、缩略标识和提交日期。

6. `git log -NUMBER`
7. `git log --after="2014-7-1" --before="2014-7-4"`
8. `git log --author="John"`   `git log --author="John\|Mary"`
9. `--committer`
10.  `git log -- foo.py bar.py`  -- 告诉 git log 接下来的参数是文件路径而不是分支名。
11.  `git log master..feature`
12. `git log --no-merges`  & `git log --merges`