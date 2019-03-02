---
layout: post
title:  "Python and Python3 experience"
rootCate: "work"
categories:
- Python
tags:
- work
- Python3
---

总结 Python 使用过程中的小技巧，以备后用。内容包括但不限于对list/set/map等数据结构、算法的记录。

<!---more--->

## General experience for all
1. 使用正则表达式的速度比 list in 方法检索要快
2. numpy 处理 data 的速度比一般的 list 要快很多，而且可以直接一行 code 读取 txt文档里的数据，不用再转换。
```
dataArr = numpy.loadtxt('./data/data.txt')
```
3. 移除 list 中的空格项：
```
while ' ' in list1:
    list1.remove(' ')
```
4. list 转换成 string，join会更快：
```
','.join(list1)
```

5.  string 替换 replace，可以去掉多余的字符或者重复的字符：
```
str.replace(old, new[, max])
old -- 将被替换的子字符串。
new -- 新字符串，用于替换old子字符串。
max -- 可选字符串, 替换不超过 max 次
```

6. Python dict 数据类型
```
a={'a':1,'b':[2]}
a['c']=3
// 此时a = {'a':1,'b':[2],'c':3}

a['b'].append(4)
// 此时a = {'a':1,'b':[2,4],'c':3}
 ```
7. Python 3 中字典(Dictionary) `has_key(KEY)` 函数变为`__contains__(KEY)`

8. request请求返回的 string 结果重构:
```
r=requests.get('XXXX',params=params)
r_text=r.text[1:-1].split('{')
if bool(r_text):
    result.append(eval('{'+r_text[1]))
```
9. 用列表方式生成 list 会比for循环更快:
```
list=[value for value in XX_list]
```
10. 对 queryset 操作，用 `.count()` 获取长度比 `len()`更快
11. 比较python里两个list **内容一致**,使用set: `set(list1)==set[list2]`
12. Python3 实现日期加减一天:
```
import datetime
now = datetime.datetime.now()
date = now + datetime.timedelta(days = 1)
```

## list
1. access: 通过index访问
2. 删除：
```
lst = [1, 0, 2, 0, 0, 8, 3, 0]
lst = filter(lambda x: x != 0, lst)
print lst
```
3. 列表解析:
```
lst = [x for x in lst if x != 0]
```

4. 循环删除 :
```
for item in lst[:]:
    if item == 0:
        lst.remove(item)
//加上[:]是表示倒序删除，如果不加而正序删除会有问题，如果有重复的连续元素只会删除第一个
```

5. 将一个list中的dict 按某个key分组，生成dict:
```
方法一：
import collections
s = [('yellow', 1), ('blue', 2), ('yellow', 3), ('blue', 4), ('red', 1)]

# defaultdict
d = collections.defaultdict(list)
for k, v in s:
    d[k].append(v)

方法二：
output_dict = {}
for item in input_list:
    output_dict.setdefault(item['a'], []).append(item)

//结果
list(d.items())
[('blue', [2, 4]), ('red', [1]), ('yellow', [1, 3])]
```
而且好处是当查找d[key]里的key不存在时，并不会报错而是可能返回默认值，比如里面的key是list类型则会返回[]。

## 参考
[Python 3 collections.defaultdict() 与 dict的使用和区别](https://blog.csdn.net/kyi_zhu123/article/details/80203118)

[python中defaultdict的用法详解](https://blog.csdn.net/dpengwang/article/details/79308064)

[python通过某个字段将记录分组](https://blog.csdn.net/zhousishuo/article/details/78391238)

[Python语法糖——遍历列表时删除元素](https://segmentfault.com/a/1190000007214571)
