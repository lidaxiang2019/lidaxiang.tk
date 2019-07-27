---
layout: article
title:  "Some userful library of Django"
rootCate: "work"
show_edit_on_github: false
pageview: true
comment: true
key: work_python_django_library_list_20181015
categories:
- Python
tags:
- work
- Python3
- Django
---

总结 Django Framework 可以利用的第三方库，以备后用。

<!---more--->

## pip library
1. channels
2. django-postgrespool==0.3.1
3. django-redis==4.8.0
4. django-socketio==0.3.9
5. djangorestframework==3.6.3
6. APScheduler
7. beautifulsoup4
8. daphne
9. numpy
10. oauth
11. pandas
12. django-socketio(maybe need you download from website)
13. Celery : 是基于Python开发的一个分布式任务队列框架，支持使用任务队列的方式在分布的机器/进程/线程上执行任务调度。
14. 处理时区、模糊时间范围、本月最后一个星期五、节假日计算可以用 [dateutil.relativedelta](https://python3-cookbook.readthedocs.io/zh_CN/latest/c03/p12_convert_days_to_seconds_and_others.html) 例子 : `print(datetime.date.today() - relativedelta(months=+1))`
15. 

## software to help django
1. redis
2. uwsgi
3. supervisor
4. MySQL/PostgreSQL
5. nginx
