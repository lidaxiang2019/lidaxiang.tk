---
layout: article
title:  你忽略的 Django 那点事
rootCate: "work"
categories:
- Python
tags:
- work
- Python3
- Django
- RESTful
- restframework
---

Django 框架是一个功能强大且灵活的工具包，用于构建 Web。
它的功能包括以下内容：轻松构建 Web 站点；默认提供强大的 admin 后台管理模块; 验证前端的参数以及处理; 自带 User 注册管理模块; 支持ORM和非ORM数据源的序列化;可自定义功能。

本文列出相关少见的知识点和应用。

<!---more--->

1. 存储 `DateTimeField` 时，如果遇到警告 `RuntimeWarning: DateTimeField ModelName.field_name received a naive` 需要使用 `datetime.now().replace(tzinfo=pytz.utc)` 来 save 数据库记录。
