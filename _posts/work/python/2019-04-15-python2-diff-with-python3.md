---
layout: article
title:  "Python2 & Python3 Differences"
rootCate: "work"
show_edit_on_github: false
pageview: true
comment: true
key: work_python_django_fast_note_20190416
categories:
- Python
tags:
- work
- Python3
---

这是遇过的一道面试题，应该是常被忽略的问题，来反思和回忆下。

<!---more--->

## 类型提醒: Type hinting
部分IDE会帮助你根据这些 type hinting 预先检查代码是否正确。
```
def func(height : int) -> bool:
  return height > 180
```

## 运行时类型检查：
正常情况下，函数的注释处理理解代码用，其他没什么用。你可以是用enforce来强制运行时检查类型。

## 使用@特殊字符表示矩阵乘法 （没用过）
使用@符号，整个代码变得更可读和方便移植到其他如numpy、tensorflow等库。

## **特殊字符来递归文件路径（没用过）
在Python2中，递归查找文件不是件容易的事情，即使使用glob库，但是python3中，可以通过通配符简单的实现。

```
import glob

# Python 2
found_images = \
    glob.glob('/path/*.jpg') \
  + glob.glob('/path/*/*.jpg') \
  + glob.glob('/path/*/*/*.jpg') \
  + glob.glob('/path/*/*/*/*.jpg') \
  + glob.glob('/path/*/*/*/*/*.jpg') 

# Python 3
found_images = glob.glob('/path/**/*.jpg', recursive=True)
```

## pathlib
系统文件路径处理库：pathlib
  使用Python2的同学，应该都用过os.path这个库，来处理各种各样的路径问题，比如拼接文件路径的函数：os.path.join()，用Python3，你可以使用pathlib很方便的完成这个功能。相比与os.path.join()函数，pathlib更加安全、方便、可读。pathlib还有很多其他的功能。

## print
打印到文件中:
```
print >>sys.stderr, "critical error"      # Python 2
print("critical error", file=sys.stderr)  # Python 3

#不使用join函数拼接字符串
# Python 3
print(*array, sep='\t')
print(batch, epoch, loss, accuracy, time, sep='\t')
```

## 字符串格式化
```
# python2
print('{batch:3} {epoch:3} / {total_epochs:3}'.format(batch=batch, epoch=epoch))
#python3
print(f'{batch:3} {epoch:3} / {total_epochs:3})
```

## 严格排序
不同类型的数据无法排序

## NLP Unicode问题
Python3的字符串都是Unicode编码

## 整数类型变化
python2提供了两个整数类型：int和long，python3只提供有个整数类型：int，如下的代码：

## 更高性能的默认pickle engine
有人测试可以缩短到python2 的1/3

## 更安全的列表推导
```
labels = <initial_value>
predictions = [model.predict(data) for data, labels in dataset]

# labels 在 Python 2 会被覆盖重写
# labels 在 Python 3 中不会被影响
```

## 更简易的super()
```
# Python 2
super(MySubClass, self).__init__(name='subclass', **options)
        
# Python 3
super().__init__(name='subclass', **options)
```

## Multiple unpacking
合并两个Dict: # Python 3.5+ `z = {**x, **y}`

# Python 3.6 以上特性
asyncio 模块添加了新的功能

time 模块现在提供 纳秒级精度函数 的支持

async 和 await 现在是保留的关键字。

新secrets模块被用于简化那些适用于管理密文的密码学安全伪随机数生成器(cryptographically strong pseudo-random numbers)的生成过程，如1.生成随机数  2.密码和一次性密码(OTP即One Time Password)  3.随机token  4.密码恢复安全URL和会话密钥。

数字字面量中的下划线,更好的阅读数字. 

## 参考文献
[Python3的这些新特性很方便](https://segmentfault.com/a/1190000013066350)