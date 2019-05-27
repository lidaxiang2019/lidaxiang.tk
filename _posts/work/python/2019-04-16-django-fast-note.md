---
layout: article
title:  "Fast note about Django and Python"
rootCate: "work"
categories:
- Python
tags:
- work
- Python3
- Django
---

做一些快速笔记，主要是关于 Django Framework 可以利用的第三方库， 以便翻阅。

<!---more--->

## git
1. git log --name-only
2. git rm --cache  XX文件名  //从staging area 暂存区移除并解除track 跟踪状态
3. [git的使用和别名配置](https://www.jianshu.com/p/5c4511c7dd88)
4. 冲突里 HEAD 标签 是远程的这次commit的，另外一个是本地的(待确认)。
5. [ git rebase   [startpoint]   [endpoint]  --onto  [branchName]](https://www.jianshu.com/p/4a8f4af4e803)
6. 

## Django
1. [Django中的中间件执行顺序](https://www.cnblogs.com/wf-skylark/p/9310448.html)  Django中的中间件是一个轻量级、底层的插件系统，可以介入Django的请求和响应处理过程，修改Django的输入或输出。
2.  [Serializer or ModelViewset's create](https://stackoverflow.com/questions/41094013/when-to-use-serializers-create-and-modelviewsets-create-perform-create )关于 Serializer's create(self, validated_data) ModelViewset's create(self, request, *args, \*\*kwargs) , perform_create(self, serializer) ，原则是 **Thin views, Thick serializers** 。
在针对某一个特定的对象创建时，如果需要修改逻辑可以重写serializer的create方法而不是viewset，因为viewset可以选择调用不同的serializer来完成不同情况下的创建。
3. [python操作redis 和 redis-py 连接池用法详解](https://www.jianshu.com/p/2639549bedc8) redis-py使用connection pool来管理对一个redis server的所有连接，避免每次建立、释放连接的开销。默认，每个Redis实例都会维护一个自己的连接池
4. Django 的 ORM 还可以选择 SQLAlchemy，但是定义繁琐，要求开发者完全明白 engine、metadata、table、column、mapper 等概念. [ORM：django的ORM和SQLalchemy
](https://blog.csdn.net/GeekLeee/article/details/52806385)
5. [知乎技术方案初探](https://my.oschina.net/wanghhao/blog/502274) 知乎和头条都已经转向了 Go 。
6. [Django配置celery执行异步任务和定时任务](https://juejin.im/post/5b588b8c6fb9a04f834655a6)
7. [理解Django的makemigrations和migrate](https://blog.csdn.net/liuweiyuxiang/article/details/71150965) `python manger.py sqlmigrate theapp 0001`
8. [善用migrations 初始化数据，解决多人migration冲突](https://juejin.im/post/5c186ad45188254a4313c17c) 直接新建migrations文件，写代码来使用migrations中的RunPython来初始化数据。
9.[合并 migrations文件](https://stackoverflow.com/questions/40028586/how-to-squash-recent-django-migrations)
python manage.py squashmigrations <appname> <squashfrom> <squashto>
10.  [django-storages 管理静态资源](https://juejin.im/post/5ce13506f265da1ba2522ee1) ImageField或者FileField，有两个参数是你可以利用的：STATICFILES_STORAGE和DEFAULT_FILE_STORAGE。另外还可以使用 [django-webpack-loader 和webpack 技术](https://segmentfault.com/a/1190000011805705)。
[Django使用国内第三方存储服务器](http://yshblog.com/blog/172)
11.  [用django-notifications实现消息通知](https://juejin.im/post/5cdfecd0f265da1b667baa36)   [django-notifications, GitHub的通知同样适用于 Django](https://www.helplib.com/GitHub/article_90646)
12. [Django_外键查询和反向查询](https://www.jianshu.com/p/20e078a718ed) 主表查询子表，即反向查询：
Tom = Person.objects.get(id=1)
 方式一:  查询方式: 主表.子表_set(), 返回值为一个queryset对象. Tom.Car_set().all()
 方式二： 通过在外键中设置related_name属性值既可 Tom.cars.all()
 方式三： 通过@property装饰器在model中预定义方法实现, Tom.all_cars

13. [有效使用Django的QuerySets](https://www.oschina.net/translate/django-querysets?print)  `if` 语句会触发queryset的执行。`if restaurant_set:` 遍历时用的是cache中的数据。如果不需要所有数据，queryset的cache可能会是个问题。处理成千上万的记录时，将它们一次装入内存是很浪费的。更糟糕的是，巨大的queryset可能会锁住系统进程，让你的程序濒临崩溃。防止不当的优化: queryset的cache是用于减少程序对数据库的查询，在通常的使用下会保证只有在需要的时候才会查询数据库。
14.  Good query : exists = Blog.objects.filter(category='django').exists()
15.  Good query : count = Blog.objects.filter(category='django').count()
16.  Good query : 使用 values 和 values_list
17.  [Django 数据查询性能优化最佳实践](https://blog.csdn.net/Ahri_J/article/details/73610365)
   通过 queryset.only 与 difer 方法来优化查询性能。通过 only 方法可以只查询指定的列。而 difer 查询除了指定列之外的其他列。
   通过建立合适的索引来提高性能。记住，索引会降低写的性能。
  django-debug-tool 是一个功能非常强大的 debug 插件，它可以展现项目中每次查询所执行的 SQL 语句的执行时间。
18. [Write and Run test](https://docs.djangoproject.com/en/2.2/topics/testing/overview/)  [更简单版本解释test](https://code.ziqiangxuetang.com/django/django-test.html)


## Python
1. defaultdict
2. [字符串匹配和搜索](https://python3-cookbook.readthedocs.io/zh_CN/latest/c02/p04_match_and_search_text.html)string: startswith, endswith,find 。复杂的匹配需要使用正则表达式和 re 模块: `re.match(r'\d+/\d+/\d+', text1):`
3. re.split() 是非常实用的，因为它允许你为分隔符指定多个正则模式。
4. Find keys in common : a.keys() & b.keys() # { 'x', 'y' }
5. Find keys in a that are not in b: a.keys() - b.keys() # { 'z' }
6. Find (key,value) pairs in common: a.items() & b.items() 
7. 以现有字典构造一个排除几个指定键的新字典: c = {key:a[key] for key in a.keys() - {'z', 'w'}}
8. 命名切片: SHARES = slice(20, 23); PRICE = slice(31, 37); cost = int(record[SHARES]) * float(record[PRICE])
9. [每周一个 Python 模块 | heapq](https://juejin.im/post/5c0f9c1fe51d451ac27c470a)
10. 统计list里出现频率最高的 ： from collections import Counter;word_counts = Counter(words);top_three = word_counts.most_common(3)

## 其他开发相关
1. [Elastic APM - kibana](https://segmentfault.com/a/1190000014864630)  Opbeat被Elastic收购了，做成了Elastic APM。功能包括监控性能表现(每个路由、每次请求)，以及类似sentry的错误收集功能.，架构上Elastic APM和ES、Kibana还是相对独立的，集成也不难。

补充：[ELK日志系统：Elasticsearch + Logstash + Kibana 搭建教程](https://www.cnblogs.com/yjmyzz/p/ELK-install-tutorial.html)

2. [Zendesk 工单系统](https://developer.zendesk.com/rest_api/docs/support/introduction)

3. [api接口、RPC、WebService分别解决什么问题？](https://segmentfault.com/q/1010000000484829)
RPC：所谓的远程过程调用 (面向方法)
SOA：所谓的面向服务的架构(面向消息)
REST：所谓的 Representational state transfer (面向资源)


## 设计思路
1. [Django多级评论（上）](https://zhuanlan.zhihu.com/p/35354495)
2. [Richardson成熟度模型(Richardson Maturity Model) - 通往真正REST的步骤](https://blog.csdn.net/dm_vincent/article/details/51341037) [英文地址](https://martinfowler.com/articles/richardsonMaturityModel.html)简而言之就是切分资源访问，引入http动词，以及每次请求带上下次访问的地址。


## 其他思想
1. [正态分布](http://www.ruanyifeng.com/blog/2017/08/normal-distribution.html) 
   [正态分布与幂律曲线](http://blog.sciencenet.cn/home.php?mod=space&uid=242272&do=blog&id=1053659)
 帕累托（Pareto）的意大利经济学家发现，财富分配是不均的。帕累托被称做“资本主义的马克思”，他指出：英格兰财富的80%，掌握在了20%的人口手里。许多其他国家和地方也是如此。帕累托由此得出一个结论：财富分配和人口结构之间，存在着一种可以估算的比率。帕累托称之为“关键少数定律”，也就是现在所谓的“二八法则”。

类似帕累托的这种财富曲线，叫作幂律（Power Law）曲线。

以图书销售为例，把图书的图书的销售排名作为横轴，销售量作为纵轴，我们就会得到类似下面这个销售曲线，排名前二十的图书，占据了全部销售额的八成。

[使用幂律、正态和爱尔朗分布来做出正确的预测](https://kknews.cc/zh-sg/news/2lergog.html)

银行业年报中展示了当下国内财富差距的一个客观事实。（这么多年，我怎么就忽略了这么重要的一份资料呢）

根据招商银行2018年年报显示，在其12541.44万户零售客户中，50万元及以上零售客户数量为236.26万户，占总零售客户数的1.88%。同时，50万元及以上客户的总资产占零售客户总资产的80.98%。（1.2亿人口样本体量，相对于13.95亿人口而言，具有一定代表性）这意味着，近2%的群体掌握了80%的财富。回顾招商银行过去几年的年报数据，2%与80%这两个数字几乎没有大变动。即以招商银行的数据口径得出的结论是，国内财富分配极其不均衡。

根据中信银行2018年年报显示，在整个8831.76万户个人客户中，50万元及以上资产的中高端客户数量为73.5万户，占总零售客户数的0.8%。

2. [2018—2020年全球经济或有一次崩塌](https://pit.ifeng.com/a/20170316/50783771_0.shtml)
经济崩塌后接着就是物价飞涨，恶性通胀。

3. [PMI、CPI、PPI 之间的相互关系是什么？](https://www.zhihu.com/question/19765287)PPI主要反映生产端的价格变化，而CPI主要反映消费环节商品和服务价格的变化水平，两者之间不仅存在着天然的传导机制，并且还有着走势趋同以及CPI增速高于PPI增速等特性。

4.  [PMI与股市同期走势图](https://www.legulegu.com/stockdata/pmi) 另外一篇预测了2015年的股市大跌，[PMI指数预测股市即将大跌](https://zhuanlan.zhihu.com/p/20054921)  

5. [PMI、CPI、PPI 之间的相互关系是什么？](https://www.zhihu.com/question/19765287/answer/135239655)
PMI 与股市有着高度相关性，而且PMI一般具有先导性。

PMI及PPI是先行指标，CPI是滞后指标，前两者对后者会有一定的传导性    PMI在50%以上的扩张区间，并处于上升通道时，经济有逐渐过热的趋势，传导至CPI，表现为通胀压力。PPI向CPI的传导则在于，中间品价格的上涨会导致终端商品价格的上涨。不过需要注意的是，在中国，CPI的构成中，最大头的是农产品价格，而非工业品价格，而PPI所影响的更多是工业产成品的价格，在这种结构性差异下，PPI对CPI的传导性并不强，甚至于，中国PPI的影响可能通过出口传导至美国。
 
 PMI对CPI的影响则主要由于PMI本身是对某个领域的经济或者整个经济荣衰的衡量指数，一般以50为界，低则衰，高则荣。一般而言，当经济趋向繁荣即超越50时，表明经济走势良好，如果大幅度超过50表明经济过热，供不应求，CPI则很可能随之升高；相反，如果大幅低于50则表明有通货紧缩的危险，CPI自然降低甚至为负数。

