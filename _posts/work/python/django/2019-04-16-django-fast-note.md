---
layout: article
title:  "Fast note about Django and Python"
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
19. [django-celery-beat 动态建立celery job](http://www.lidaxiang.tk/python/2019/05/16/django-celery-beat.html) 
20. datetime.now() 获取是utc时间，存进去数据库也是utc，当serializer之后才会变成本地时间。timezone.now()会直接按照设定的timezone来获取对应的时间。
21. 执行 `./manage.py check --deploy` 命令来查看可能存在的安全问题：
22. DRF通常使用BrowsableAPIRenderer, 部署到生产环境后，最好将这个Renderer移除
23. django 有 sigal 机制，可以在 create、save 和 delete ORM 操作时，触发事件函数。另外也可以自己新建signal。update 操作没有内置signal。
24. [DRF 搜索、筛选、排序、分页权限功能](https://q1mi.github.io/Django-REST-framework-documentation/api-guide/filtering_zh/#djangoobjectpermissionsfiltedjango)DRF 框架可以设置 `filter_backends` `serializer_class` `SearchFilter` `OrderingFilter` `Pagination`
25. [DRF 限流量Throttling、主机名信息携带、Authentication、版本号支持](https://blog.csdn.net/m0_43394876/article/details/88857732) 需要获取请求的版本号时，可以通过request.version来获取。
26. 当想要找的内容清晰具体，用户一般通过键入 **关键词（数据输入）的方式搜索**。而当用户游离在模糊的区间时，通常使用到**筛选功能来聚焦**。
27. [Django safedelete 防止被外键引用的数据被删](https://django-safedelete.readthedocs.io/en/latest/)
28. [ManyToManyField 多对多关系](https://blog.csdn.net/bbwangj/article/details/79935375) `members = models.ManyToManyField(Person, through='Membership', through_fields=('group', 'person'))`
29. [Django-haystack来实现搜索功能](https://blog.csdn.net/AC_hell/article/details/52875927) haystack是django的开源搜索框架，该框架支持Solr,Elasticsearch,Whoosh, *Xapian*搜索引擎，不用更改代码，直接切换引擎，减少代码量。
30. **非常重要:** [django 使用 select_related 改lazy查询为一次性查出结果，由此提高查询效率避免多次query的思路](https://www.jianshu.com/p/be0392af0b64)，[Django 索引加快数据查询](https://www.jianshu.com/p/e6c7e244c980) 对于一对一字段（OneToOneField）和外键字段（ForeignKey），可以使用select_related 来对QuerySet进行优化。select_related 执行了一个 sql join 查询。
31. **非常重要**对于多对多字段（ManyToManyField）和一对多字段(例如外键) ，prefetch_related 执行一个单独的查找，它允许预先读取多对多和多对一的对象数据，这是 select_related 做不到的。另外 perfetch_related 也可以与通用外键和关系一起使用。设置索引方式:1. `db_index=True` 2. `unique_together` 3. `index_together`
32. [mysql 索引使用原则](https://cloud.tencent.com/developer/article/1004912) mysql中普遍使用B+Tree做索引。原则： **1. 最左前缀匹配原则,，where子句中使用最频繁的一列放在最左边。 2.尽量选择区分度高的列作为索引 3. =和in可以乱序 4.索引列不能参与计算，保持列“干净” 5.尽量的扩展索引，不要新建索引** 
33. [索引的坏处和额外时间、空间开销、数据变更还需要重新计算和维护](https://cloud.tencent.com/developer/article/1004912) 索引可以提高查询效率，但索引也有自己的不足之处。索引的额外开销：(1) 空间：索引需要占用空间;(2) 时间：查询索引需要时间；(3) 维护：索引须要维护（数据变更时）；
34. [不建议使用索引的情况：(1) 数据量很小的表 (2) 空间紧张](https://cloud.tencent.com/developer/article/1004912)
35. `[dict(q) for q in qs]` 把 **queryset** 转换成list
36. [sort list of dictionaries by values in Python](https://www.geeksforgeeks.org/ways-sort-list-dictionaries-values-python-using-lambda-function/) `print sorted(lis, key = lambda i: i['age'],reverse=True) ` 
37. **非常实用**aggregate 聚合 & annotate 汇总 函数在统计上非常实用。EX1: `pubs = Publisher.objects.aggregate(num_books=Count('book'))` [简单聚合 aggregate不单单可以求和，还可以求平均Avg，最大最小等等。](https://blog.csdn.net/AyoCross/article/details/68951413)   EX2: ` msgS = MessageTab.objects.values_list('msg_status').annotate(Count('id'))`  [annotate 查询各个 消息状态(未发送、已发送等等) 的消息数量](https://blog.csdn.net/AyoCross/article/details/68951413) 
39. [select for update 不小心锁了全表带来的性能问题](http://ju.outofmemory.cn/entry/178798)
40. `model._meta.get_fields()` 获取一个model所有字段名字。
41. `exclude`
42. `icotains`

## Django & DRF library
1. [drf-problems Handles exception to return response with Problem Details model](https://pypi.org/project/drf-problems/)  这个库可以使得drf返回的错误信息非常详细，包括url、type、参数、错误原因。
2. [drf-dynamic-fields](https://pypi.org/project/drf-dynamic-fields/) Dynamically return subset of Django REST Framework serializer fields
3. `with cache.lock(f"redis_lock:XXX`):`
4. django render 和 serializer 哪个先执行? 这个问题还是有点没理清。但目前的看法是在 `modelviewset` 的`viewmin` `apiview`的`as_view`  `view`方法中，先调用了`get_queryset` `get_serializer`获取了 `serializer` 里的额外定义字段，之后才去调用了render_class去渲染，最后在 `finalize_response` 的方法里完成对`response`的最后添加数据和装饰，最后返回给前端渲染后的文本。 参考资料: [DRF render](https://juejin.im/post/5a9e5ff46fb9a028b6170a7f) [DRF viewset 源码分析](https://blog.csdn.net/u013210620/article/details/79879611)
  

## Python
1. defaultdict
2. [字符串匹配和搜索](https://python3-cookbook.readthedocs.io/zh_CN/latest/c02/p04_match_and_search_text.html)string: startswith, endswith,find 。复杂的匹配需要使用正则表达式和 re 模块: `re.match(r'\d+/\d+/\d+', text1):`
3. re.split() 是非常实用的，因为它允许你为分隔符指定多个正则模式。
4. Find keys in common : `a.keys() & b.keys() # { 'x', 'y' }`
5. Find keys in a that are not in b: `a.keys() - b.keys() # { 'z' }`
6. Find (key,value) pairs in common: `a.items() & b.items() `
7. 以现有字典构造一个排除几个指定键的新字典: c = {key:a[key] for key in a.keys() - {'z', 'w'}}
8. 命名切片: SHARES = slice(20, 23); PRICE = slice(31, 37); cost = int(record[SHARES]) * float(record[PRICE])
9. 统计list里出现频率最高的 ： from collections import Counter;word_counts = Counter(words);top_three = word_counts.most_common(3) 
10. [Python dict pop method](https://lightless.me/archives/python-dict-pop-method.html) `res = dict.pop('logged_in', None)`  指定了第二个参数 None 之后，即便尝试 pop 一个不存在的键，也不会有异常
11. `PRICE = slice(31, 37)` 命名list的切片，避免无直观意义的硬编码
12. `from collections import Counter ; word_counts = Counter(words);top_three = word_counts.most_common(3)` #出现频率最高的3个单词
13. 确保线程安全的办法: **锁和信号**
14. 线程间传递数据常用 Queue 。
15. Python3 尽量使用 pickle 序列化，而不是用json。
16. [repr 可以重新拿到对象， str 区别](https://www.cnblogs.com/shaosks/p/5737089.html)内建函数str()和repr() 或反引号操作符（``）可以方便地以字符串的方式获取对象的内容、类型、数值属性等信息。repr()函数得到的字符串通常可以用来重新获得该对象。
17. [Python format 格式化函数](https://www.runoob.com/python/att-string-format.html) 字段后的 : 后面加一个整数会限定该字段的最小宽度，这在美化表格时很有用: `print('{0:10} ==> {1:10d}'.format(name, phone))`
18. [使用 heapq 实现一个优先级队列](https://blog.csdn.net/andybegin/article/details/81027511)
19. [[Python标准库]heapq——堆排序算法](https://blog.csdn.net/dapeng0802/article/details/50389857)
20. [基于线程通信的程序，想让它们实现发布/订阅模式的消息通信。](https://python3-cookbook.readthedocs.io/zh_CN/latest/c12/p11_implement_publish_subscribe_messaging.html) 一个交换机就是一个普通对象，负责维护一个活跃的订阅者集合，并为绑定、解绑和发送消息提供相应的方法。
21. [self._” to variable declarations within class methods](https://stackoverflow.com/questions/30294383/python3-when-exactly-do-you-need-to-prepend-self-to-variable-declarations)class某个方法里如果有 `self._length_of_thing`，则表示这是个class 变量，可以被class其他方法访问。


## 其他开发相关
1. [Elastic APM - kibana](https://segmentfault.com/a/1190000014864630)  Opbeat被Elastic收购了，做成了Elastic APM。功能包括监控性能表现(每个路由、每次请求)，以及类似sentry的错误收集功能.，架构上Elastic APM和ES、Kibana还是相对独立的，集成也不难。

补充：[ELK日志系统：Elasticsearch + Logstash + Kibana 搭建教程](https://www.cnblogs.com/yjmyzz/p/ELK-install-tutorial.html)
2. [Zendesk 工单系统](https://developer.zendesk.com/rest_api/docs/support/introduction)
3. [api接口、RPC、WebService分别解决什么问题？](https://segmentfault.com/q/1010000000484829) RPC：所谓的远程过程调用 (面向方法) SOA：所谓的面向服务的架构(面向消息) REST：所谓的 Representational state transfer (面向资源)
4. http是无状态协议，ftp是有状态。所以http需要cookie、session来维持状态和认证。
5. 哈希洪水攻击（Hash-Flooding Attack）是一种拒绝服务攻击（Denial of Service），一旦后端接口存在合适的攻击面（找到你的哈希生成算法），攻击者就能轻松让整台服务器陷入瘫痪。比如哈希表（Hash Table）。假设我们想要连续插入 n 个元素到哈希表中：如果这些元素的键（Key）极少出现相同哈希值，这项任务就只需 O(n) 的时间。如果这些键频繁出现相同的哈希值（频繁发生碰撞），这项任务就需要 O(n^2) 的时间。防御措施：如果黑客不能掌握算法的所有细节，是不是就不能算出一组频繁碰撞的键，也就没法发动哈希洪水攻击。每建一张哈希表，我们就随机生成一个新的秘密参数。SipHash官网，里面列举了众多采用SipHash-2-4算法的知名项目。其中Rust、Python、Ruby等语言更是把SipHash-2-4作为默认的哈希表实现方法。
6. 


## 设计思路
1. [Django多级评论（上）](https://zhuanlan.zhihu.com/p/35354495)
2. [Richardson成熟度模型(Richardson Maturity Model) - 通往真正REST的步骤](https://blog.csdn.net/dm_vincent/article/details/51341037) [英文地址](https://martinfowler.com/articles/richardsonMaturityModel.html)简而言之就是切分资源访问，引入http动词，以及每次请求带上下次访问的地址。
3. [Python RPC 远程方法调用](https://python3-cookbook.readthedocs.io/zh_CN/latest/c11/p08_implementing_remote_procedure_calls.html) RPCHandler 和 RPCProxy 的基本思路是很比较简单的。 如果一个客户端想要调用一个远程函数，比如 foo(1, 2, z=3) ,代理类创建一个包含了函数名和参数的元组 ('foo', (1, 2), {'z': 3}) 。 这个元组被pickle序列化后通过网络连接发生出去。 这一步在 RPCProxy 的 __getattr__() 方法返回的 do_rpc() 闭包中完成。 服务器接收后通过pickle反序列化消息，查找函数名看看是否已经注册过，然后执行相应的函数。 执行结果(或异常)被pickle序列化后返回发送给客户端。我们的实例需要依赖 multiprocessing 进行通信。
4. RPC 调用安全问题，因为黑客可以创建特定的消息，能够让任意函数通过pickle反序列化后被执行，所以**永远不要允许来自不信任或未认证的客户端的RPC**。特别是你绝对不要允许来自Internet的任意机器的访问， 这种只能在内部被使用，位于防火墙后面并且不要对外暴露。
5. [Seif 有人设计的一套基于公钥加密的传递信息方法，取代http传递](https://www.crockford.com/seif.html) 难点可能在于如何分发证书。
6. 

## 其他思想
1. [正态分布](http://www.ruanyifeng.com/blog/2017/08/normal-distribution.html) 
   [正态分布与幂律曲线](http://blog.sciencenet.cn/home.php?mod=space&uid=242272&do=blog&id=1053659) 帕累托（Pareto）的意大利经济学家发现，财富分配是不均的。帕累托被称做“资本主义的马克思”，他指出：英格兰财富的80%，掌握在了20%的人口手里。许多其他国家和地方也是如此。帕累托由此得出一个结论：财富分配和人口结构之间，存在着一种可以估算的比率。帕累托称之为“关键少数定律”，也就是现在所谓的“二八法则”。
   类似帕累托的这种财富曲线，叫作幂律（Power Law）曲线。  
   以图书销售为例，把图书的图书的销售排名作为横轴，销售量作为纵轴，我们就会得到类似下面这个销售曲线，排名前二十的图书，占据了全部销售额的八成。

2. [使用幂律、正态和爱尔朗分布来做出正确的预测](https://kknews.cc/zh-sg/news/2lergog.html) 
   银行业年报中展示了当下国内财富差距的一个客观事实: 
   根据招商银行2018年年报显示，在其12541.44万户零售客户中，50万元及以上零售客户数量为236.26万户，占总零售客户数的1.88%。同时，50万元及以上客户的总资产占零售客户总资产的80.98%。（1.2亿人口样本体量，相对于13.95亿人口而言，具有一定代表性）这意味着，近2%的群体掌握了80%的财富。回顾招商银行过去几年的年报数据，2%与80%这两个数字几乎没有大变动。即以招商银行的数据口径得出的结论是，国内财富分配极其不均衡。  
   根据中信银行2018年年报显示，在整个8831.76万户个人客户中，50万元及以上资产的中高端客户数量为73.5万户，占总零售客户数的0.8%。   
3. . [2018—2020年全球经济或有一次崩塌](https://pit.ifeng.com/a/20170316/50783771_0.shtml)
经济崩塌后接着就是物价飞涨，恶性通胀。

4. [PMI、CPI、PPI 之间的相互关系是什么？](https://www.zhihu.com/question/19765287)PPI主要反映生产端的价格变化，而CPI主要反映消费环节商品和服务价格的变化水平，两者之间不仅存在着天然的传导机制，并且还有着走势趋同以及CPI增速高于PPI增速等特性。
5. [PMI与股市同期走势图](https://www.legulegu.com/stockdata/pmi) 另外一篇预测了2015年的股市大跌，[PMI指数预测股市即将大跌](https://zhuanlan.zhihu.com/p/20054921)  

6. [PMI、CPI、PPI 之间的相互关系是什么？](https://www.zhihu.com/question/19765287/answer/135239655)  PMI 与股市有着高度相关性，而且PMI一般具有先导性。
7. **PMI及PPI是先行指标，CPI是滞后指标，前两者对后者会有一定的传导性**   PMI在50%以上的扩张区间，并处于上升通道时，经济有逐渐过热的趋势，传导至CPI，表现为通胀压力。PPI向CPI的传导则在于，中间品价格的上涨会导致终端商品价格的上涨。不过需要注意的是，在中国，CPI的构成中，最大头的是农产品价格，而非工业品价格，而PPI所影响的更多是工业产成品的价格，在这种结构性差异下，PPI对CPI的传导性并不强，甚至于，中国PPI的影响可能通过出口传导至美国。PMI对CPI的影响则主要由于PMI本身是对某个领域的经济或者整个经济荣衰的衡量指数，一般以50为界，低则衰，高则荣。一般而言，当经济趋向繁荣即超越50时，表明经济走势良好，如果大幅度超过50表明经济过热，供不应求，CPI则很可能随之升高；相反，如果大幅低于50则表明有通货紧缩的危险，CPI自然降低甚至为负数。

8. [解构《伟大的领袖如何激励行动》](https://www.jianshu.com/p/8a76fe41049f) 黄金思维圈对应的三个层面——信念 why 、行动 how、现象what。人们买的不是你的产品，而是你的信念。例子: 苹果公司在进行营销时，沟通方式如下" 我们做的每一件事情，都是为了突破和创新，在这个过程里做出了Mac。天使用户”，他们对产品的接受度和传播都起着重要的影响。在决定是否购买的时候，市场中的顾客需要相互参考。
