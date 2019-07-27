---
layout: article
title:  "Linux helpful commands"
rootCate: "work"
show_edit_on_github: false
pageview: true
comment: true
key: work_linux_helpful_command_20190414
tags:
- work
- Linux
- command
---

To introduce some helpful commands in Linux(ubuntu 18).

<!---more--->

> StartTime: 2018-03-14,ModifyTime:2018-07-10

1. 批量删除一个目录以及子目录下的 html 文件
 ```
 find . -name "*.html" -exec rm -r "{}" \;
 ```

２. 批量修改一个文件夹下面所有文件的后缀名为 .jpg
```
for i in `ls`; do mv -f $i `echo $i | sed 's/.....$/.jpg/'`; done
```

3. 列出磁盘的容量和占用情况
```
df -h
 ```
4. use htop or top tool watch which program use unnormal resource of computer, and kill it by this command:
```
htop
top
kill -9 PID
```
5. 列出当前 folder 的 folders 大小：
```
sudo du -h --max-depth=1
```

6. grep遍历文件夹查找文本内容:
`grep -r "要查找的内容" ./`

7. [Mac中如何查看正在使用的端口及其进程ID](https://www.crifan.com/mac_check_view_using_port_and_process_id/) `netstat -anp tcp | grep LISTE # MAC`

8. [使用netstat、lsof查看端口占用情况](http://lazybios.com/2015/03/netstat-notes/) `netstat -atunlp     OR   lsof:8000`

9. 