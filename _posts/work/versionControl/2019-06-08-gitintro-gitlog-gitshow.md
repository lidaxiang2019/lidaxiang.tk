---
layout: article
title:  "Introduction of git log and git show command"
rootCate: "work"
show_edit_on_github: false
pageview: true
comment: true
key: work_version_control_gitintro_git_log_20190622
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
3. `git log --stat` 选项显示每次提交的 **文件增删数量**
4. `git shortlog` 按人分类，列出这个人的提交内容，只有一行。可以用于发布更新摘要声明。
5. `--pretty=format:"<string>"` 用于格式化输出 log 内容，例如`%cn`、`%h` 和 `%cd` 这三种占位符会被分别替换为作者名字、缩略标识和提交日期。

6. `git log -NUMBER`  筛选几条log
7. `git log --after="2014-7-1" --before="2014-7-4"` 筛选时间范围
8. `git log --author="John"`   `git log --author="John\|Mary"` 筛选作者，筛选**或**的关系。
9. `--committer`  筛选提交者，提交者不等于作者。
10.  `git log -- foo.py bar.py`  -- 告诉 git log 接下来的参数是文件路径而不是分支名。
11.  `git log master..feature`  列出了在 feature 分支而不在 master 分支中所有的提交,也就是feature从master fork之后的变更。
12. `git log --no-merges` 未合并的记录 & `git log --merges` 已合并的记录
13. `--name-only` 仅在提交信息后显示已修改的文件清单。
14. `--name-status` 显示新增、修改、删除的文件清单。

#### git log --pretty
简单用法:
```
git log --pretty=format:"%h - %an, %ar : %s"
#print
#ca82a6d - Scott Chacon, 6 years ago : changed the version number
```

**参数说明:**
```
%H 提交对象（commit）的完整哈希字串
%h 提交对象的简短哈希字串
%T 树对象（tree）的完整哈希字串
%t 树对象的简短哈希字串
%P 父对象（parent）的完整哈希字串
%p 父对象的简短哈希字串
%an 作者（author）的名字
%ae 作者的电子邮件地址
%ad 作者修订日期（可以用 --date= 选项定制格式）
%ar 作者修订日期，按多久以前的方式显示
%cn 提交者（committer）的名字
%ce 提交者的电子邮件地址
%cd 提交日期
%cr 提交日期，按多久以前的方式显示
%s 提交说明
```

#### 统计某个人的代码量
```
git log --author="USER_NAME" --pretty=tformat: --numstat | awk '{ add += $1; subs += $2; loc += $1 - $2 } END { printf "added lines: %s, removed lines: %s, total lines: %s\n", add, subs, loc }' -
```

## git show 
`git show XXXX`  展示某次提交的内容变更


## Source
[Git log 高级用法](https://github.com/geeeeeeeeek/git-recipes/wiki/5.3-Git-log-%E9%AB%98%E7%BA%A7%E7%94%A8%E6%B3%95)
[git代码统计](https://segmentfault.com/a/1190000008542123)