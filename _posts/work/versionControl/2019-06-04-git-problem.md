---
layout: article
title:  "Solution of using Git commands"
rootCate: "work"
show_edit_on_github: false
pageview: true
comment: true
key: work_version_control_git_problems_20190604
categories:
- VersionControl
tags:
- work
- tools
- Git
- Github
---


<!---more--->

介绍Git的几种使用场景和解决问题方案，末尾附上更全面的来源资料。

<!---StartTime: 2018-03-22,ModifyTime:2018-11-29--->
## Permission denied 
`warning : permanently added the RSA host key for IP address '192.30.252.130' to the list of known list`  ：
这个大概说的是, git自己把公钥自动加到~/.git/known_host文件里面了, 进去查一下,确实是这样的.总之能上传成功就是好的.

1. 提交之后遇到类似Permission denied (publickey).大意说你不具有读和写的权限：

```
解决办法1
git remote rm origin
git remote add origin git@github.com:LyleLee/practice.git
解决办法2
git push origin master
```

2. `remote: GitLab: You are not allowed to push code to protected branches on this project.`    
解决办法是请管理员开通对应branch的提交权限。

## 多个 SSH-Key 如何使用
git可以新建多个ssh-key，有的可以专门用于自己的github，有的可以用于公司提交代码。

第一步是使用`ssh-keygen -t rsa -c 'YOUR_WANT_NAME'`创建自己想要命名的一对key文件，
第二步是创建名为`config`的文件，里面内容类似下方代码这样:

```
Host github.com
HostName github.com
User git
IdentityFile ~/.ssh/github

Host code.aliyun.com
HostName code.aliyun.com
User git
IdentityFile ~/.ssh/COMPANY
```
第三步是把.pub结尾的公钥文件里的全部内容，粘贴到对应的远端仓库服务里，比如GitHub的setting里可以加入ssh-key，   
第四步是提交或pull测试。


## push -f 覆盖了别人代码而且pull到server
所以绝对尽量避免`push -f`的情况，有冲突就先`pull --rebase`更新下，尤其不要在 `master`和`develop` 分支`push -f`。

假设真的做了然后还pull到server了，那么可以先检查下server，
1. 先从server 端push到代码库，因为server此时是最全的。

2. 之后拉到本地来，重新 `rebase`，并调整`pick XXX`记录的位置，使得**commit**记录顺序符合时间顺序。
3. 接着强制 `push -f`到代码库，并更新到server。

4. 此时server 会多一个merge记录，这是因为server的本地和代码库被我们强推的代码，基准点不一样，所以此时需要在server 执行 `git fetch -p`命令，跟代码库保持同样的`index`记录，
5. 再使用命令 `git reset --hard origin/master` 强制跟代码库代码保持一致。

此时，你的本地、server、代码库的git记录实现完美同步。


## 单纯push -f 覆盖了别人代码
此时需要找对应的开发人员，请他们再重新`rebase`推一次。