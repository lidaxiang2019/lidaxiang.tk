---
layout: post
title:  "Django experience"
rootCate: "work"
categories:
- Python
tags:
- work
- Python3
- Django
---

总结 Django Framework 使用过程中的不常遇到的技巧经验，以备后用。内容包括但不限于 ORM 模型、Static、Templates、Admin 模块等原生 Django 的功能。

<!---more--->
> StartTime: 2017-03-22,ModifyTime:2018-12-12


## 一、Django ORM
1. ORM：Object Relational Mapping(关系对象映射)
2. ORM 原理：
+ Django自带的orm是data_first类型的ORM，使用前必须先创建数据库。
+ 在django中，根据代码中的类自动生成数据库的表也叫--code first。obj.id  obj.name.....类实例对象的属性
+ 类名对应------》数据库中的表名
+ 类属性对应---------》数据库里的字段
+ 类实例对应---------》数据库表里的一行数据

3. ORM 操作流程：
+ 创建数据库，设计表结构和字段
+ 使用 MySQLdb 来连接数据库，并编写数据访问层代码
+ 业务逻辑层去调用数据访问层执行数据库操作

4. Django 的优势：
Django 的优势还有 Web 界面调试， 丰富的第三方库和文档，强大的ORM模块支持。Django的ORM操作本质上会根据对接的数据库引擎，翻译成对应的sql语句；所有使用Django开发的项目无需关心程序底层使用的是 MySQL、Oracle、sqlite....，如果数据库迁移，只需要更换Django的数据库引擎即可。 

5. `Q()` 可以构造比 Django 自身函数更复杂的SQL查询条件和逻辑关系，然后Q对象可以直接传给filter。例如: `Q(status='COMPLETED', result='agree') | Q(status='RUNNING') `

6. filter 和 get 的区别以及如何判断为空：
`TABLE.objects.filter(id=2)`  ，得到一个集合对象，集合里只有一个，跟上 `first()` 或者`[0]`取到一个具体对象。判断是否为空使用 `if not XXX`。  
 `TABLE.objects.get(id=2)[0]`，得到一个单独的对象，确定能找到，可以用，如果找到多个或者没有的，都报异常错误。等同于 `TABLE.objects.filter(id=2)[0]` 的用法。判断是否为空使用 `is None`。

7. update 和 save 区别:update 可以结合 filter 一次更新多条数据，save一次只能更新一条数据。
8. 排序 order_by(`field_name`) : 默认是是从小到大顺序，逆序则需要在 `field_name` 前加上减号`-`。
9. django获取某一个字段的列表:
+ values方法可以获取number字段的字典列表。
+ values_list可以获取number的元组列表。
+ values_list方法加个参数flat=True可以获取number的值列表。
10. get_or_create(),之前文章中也提到过,有就获取过来，没有就创建，用它可以避免重复，但是速度可以会慢些，因为要先尝试获取。 返回数据是一个tuple两个值：(size, created)
11. 排序: `filter(XXX).order_by('-pub_date','headline')`
12. 总共5位，其中有两位小数，DecimalField: `DecimalField(max_digits = 5,decimal_places = 2)`


## 二、Django ORM 聚合和分页

聚合: SQL基本函数，聚合函数对一组值执行计算，并返回单个值。除了 COUNT 以外，聚合函数都会忽略空值。 常见的聚合函数有AVG / COUNT / MAX / MIN /SUM 等

#### Aggregate
```
from django.db.models import Avg, Sum, Max, Min, Count # 求Book价格平均值
models.Book.objects.all().aggregate(Sum("price"))
```

通过 `aggregate` 可以实现 `filter` 过滤的条件后对某个字段加总、求平均等操作。

#### annotate
`annotate` 可以实现更强大的功能： 分组并统计。
```
 msgS = MessageTab.objects.values_list('msg_status').annotate(Count('id'))
```
#### 分页
数据库分页只需要简单用[start:end]截取 QuerySet 的结果集就可以。
但是这样分页拿不到所有符合条件的数据条数。

[详情链接](https://www.cnblogs.com/inns/p/5516539.html)

## 三、Django 关于优化执行时间

Django QuerySet是懒执行的，只有访问到对应数据的时候，才会去访问数据库。另外如果你再次读取查询到的数据，将不会触发数据库的访问。

```
blogs = Blog.objects.filter(category='django') # 这里不会访问数据     
if blogs: # 这里需要访问数据，因此会执行数据库查询
randomBlog = blogs[0]# 再次读取，并不会访问数据库
blog = Blog.objects.get(id=124)# 访问一次数据库
author = blog.author # 再次访问数据库
```
#### Django 的加快办法
总的原则是 **尽可能减少对database的访问次数**，例如把需要for循环多次请求的query一次全部提取出来再做进一步python的筛选和计算，即便会因此产生join问题、大量for循环和重复计算的问题。

> 实例操作证明：通过把query从520个减少到10个，访问时间从2700ms降低到430ms，整体接口访问时间从6s降低到1s左右。(真实要看server当前访问的对database访问的压力，既有压力越大那么可能请求多的情况下速度更慢)  

从 Django 官网和其他地方得到的已知的办法有：
1. 拿到`queryset`后先用`if`判断一下是否为`None`，这样 django 会把 queryset 真正去`access Database`然后把结果放入`cache`。这样之后再去`for`循环读取这个`queryset`变量就不会重复`access Database`。
2. 判断数据是否存在用`exists`而不是`len` 或 `count`
exists = Blog.objects.filter(category='django').exists()
3. 获取数据的数量使用`queryset.count()` 而不是用`len(queryset)`获取长度。前者是 select count()语法，后者会返回整个查询结果集 :
```
count = Blog.objects.filter(category='django').count()
```
4. 如果你只想获取外键的 id, 通过 `fk_id` 的方法获取要优于 `fk.id` 的方式。`fk.id` 的方式会为子表内容保存额外的数据库查询。
5. **筛选数据：** 如果你明确知道需要使用的数据，那就没没必要查询数据库中所有列的数据。比如使用`values()` 返回的是以字典的形式返回每一条数据，而 `values_list()` 返回的是每条数据元组的列表。
6. **筛选数据：** 使用 queryset.only 与 difer:
```
blogs = Blog.objects.filter(category='django').only('author','title').select_related('author')
blogs = Blog.objects.filter(category='django').defer('description').select_related('author')
```
7. builk_create() 可以执行批量的插入：
```
querysetlist=[]
for i in resultlist:
    querysetlist.append(Account(name=i))        
Account.objects.bulk_create(querysetlist)
```
> create()每保存一条就执行一次SQL，而bulk_create()是执行一条SQL存入多条数据，做会快很多！当然用列表解析代替 for 循环会更快。


#### Python的加快办法

1. 少用split，多使用他人开发好的Library。  
3. 多构造和使用复杂的数据结构(比如dict)，这样更容易理清代码的逻辑关系、也有助于解决牵扯多个model时的bussiness复杂逻辑处理的问题。  
4. 正向查询：以使用外键的表为主，以外键为id的表数据为条件。B中某个具体实例所关联的A。
```
models.ForeignKey(A)
A.B_set.all()    //比下面的方法多消耗0.2-0.3秒
B.objects.filter(A)  //更快
```
5. 反向查询: 想查询A 中某个具体实例所关联的 B
6. join 会比 + 更快


## 四、Django 的时区问题
很多时候，即使设置了setting里的timezone，可是使用datetime类型的field访问数据库还是会有warning。所以转换时区常常出现。

```
from pytz import timezone
cst_tz = timezone('Asia/Shanghai')  
utc_tz = timezone('UTC')  
now = datetime.now().replace(tzinfo=cst_tz)  
```


## 五、Django Signal
信号也可以称为钩子。它可以在 model 某些操作之前或之后立刻执行某些命令。

特别的是update方法没有信号量，需要自己写。

Model_signals

```
pre_init                        # Django中的model对象执行其构造方法前,自动触发
post_init                       # Django中的model对象执行其构造方法后,自动触发
pre_save                        # Django中的model对象保存前,自动触发
post_save                       # Django中的model对象保存后,自动触发
pre_delete                      # Django中的model对象删除前,自动触发
post_delete                     # Django中的model对象删除后,自动触发
m2m_changed                     # Django中的model对象使用m2m字段操作数据库的第三张表(add,remove,clear,update),自动触发
class_prepared                  # 程序启动时,检测到已注册的model类,对于每一个类,自动触发
```
特别的，post_save 接收到的参数里created可以判断是否是第一次添加记录。

Managemeng_signals

```
pre_migrate                     # 执行migrate命令前,自动触发
post_migrate                    # 执行migrate命令后,自动触发
```

Request/response_signals

```
request_started                 # 请求到来前,自动触发
request_finished                # 请求结束后,自动触发
got_request_exception           # 请求异常时,自动触发
```

Test_signals

```
setting_changed                 # 配置文件改变时,自动触发
template_rendered               # 模板执行渲染操作时,自动触发
Datebase_Wrapperd
connection_created              # 创建数据库连接时,自动触发
```

还可以自行定义其他signal。

## 六、Django Class-View method
基于函数的视图的问题在于，虽然它们很好地覆盖了简单的情形，但是不能扩展或自定义它们，即使是一些简单的配置选项，这让它们在现实应用中受到很多限制。  

基于类的通用视图然后应运而生，目的与基于函数的通用视图一样，就是为了使得视图的开发更加容易。

## 七、调试
加入`import pdb;pdb.set_trace()`这一行代码，之后访问该方法，命令行端会停在这里，等待手动输入python命令。

用 PyCharm 调试效果会更好。

## 八、pip 安装 requirements.txt
生成requirements.txt文件

```
pip freeze > requirements.txt
```

安装requirements.txt依赖

```
pip install -r requirements.txt
```

## 九、migrate 和makemigrations的差别

`python3 manger.py makemigrations` 相当于在该app下建立 migrations目录，并记录下你所有的关于modes.py的改动，比如0001_initial.py， 但是这个改动还没有作用到数据库。

在此之后执行命令`python3 manager.py migrate`
将该改动作用到数据库文件，比如产生table之类。

后来有种解释补充说，migrate是把别人的修改部署到自己这里。


## 参考
[Django ForeignKey 反向查询中 filter 和 _set的效率对比](https://blog.csdn.net/aaazz47/article/details/78914956/)
[Django中的信号及其用法](https://www.cnblogs.com/renpingsheng/p/7566647.html)
[django中聚合aggregate和annotate GROUP BY的使用方法](https://blog.csdn.net/AyoCross/article/details/68951413)
[Django 数据查询性能优化最佳实践](https://blog.csdn.net/Ahri_J/article/details/73610365)

[QuerySet的分页和排序](https://www.cnblogs.com/inns/p/5516539.html)
