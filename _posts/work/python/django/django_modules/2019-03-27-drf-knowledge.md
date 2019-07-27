---
layout: article
title:  Knowledge of Django REST framework
rootCate: "work"
show_edit_on_github: false
pageview: true
comment: true
key: work_python_django_modules_drf_20190327
tags:
- work
- Python3
- Django
- RESTful
- restframework
---

Django REST框架是一个功能强大且灵活的工具包，用于构建Web API。
它的功能包括以下内容：轻松构建符合 RESTful 规范的 api; 验证前端的参数以及处理; 身份验证策略包括OAuth1a和OAuth2的程序包; 支持ORM和非ORM数据源的序列化;可自定义功能。

本文列出相关少见的知识点和应用。

<!---more--->


## 1. parser
DRF 框架的 `filter` 有设置参数，对应的 `parser` 会解析前端传递的参数，默认是解析 `application/json` 格式。如果使用 `postman` 发起测试请求，注意选择参数格式，否则后端接收不到。

## 2. DRF Viewset Create
#### create(self, validated_data)
serializer 里`create(self, validated_data)`用于在保存操作前对参数做一些额外处理，例如 format 参数格式或者 call an external API or modify another table。

`viewset` 里的 `create(self, request, *args, **kwargs)` 只关心calling validation(验证) on your serializer and producing(处理) the correct response。这样，viewset 只关心逻辑，不用担心验证和输出 response 的格式。验证逻辑已经用 serializer 的 `valid` 做完，输出response 的格式已经在 serializer 的 `to_represention()` 做完。

![图片引用自掘金](https://user-gold-cdn.xitu.io/2018/1/23/1612249e5ce017f1?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)
> 图片引用自掘金 - __奇犽犽


#### perform_create
mixin类提供了以下方法，并提供了对象保存或删除行为的轻松覆盖：
```
perform_create(self, serializer) - Called by CreateModelMixin when saving a new object instance.
perform_update(self, serializer) - Called by UpdateModelMixin when saving an existing object instance.
perform_destroy(self, instance) - Called by DestroyModelMixin when deleting an object instance.
```

所以其实这个方法和django 的钩子(signal)信号的作用是类似的。

 `perform_create((self, serializer)` 一般默认第一句写 `instance.save()` ，之后可以做一些其他操作，比如发送邮件请求或者存到其他表记录等特别有用。下面是一些例子：
 例子1：
```
def perform_update(self, serializer):
    instance = serializer.save()
    send_email_confirmation(user=self.request.user, modified=instance)
```
例子2：使用这些钩子(perform_create)来提供额外的验证，通过提高ValidationError()。如果您需要在数据库保存点应用某些验证逻辑，这可能很有用。例如：
```
def perform_create(self, serializer):
    queryset = SignupRequest.objects.filter(user=self.request.user)
    if queryset.exists():
        raise ValidationError('You have already signed up')
    serializer.save(user=self.request.user)
```
serilizer 里目前没看到有 `perform_create`这个方法。

## 3. DRF 查询
查询大体有两种：
`DjangoFilterBackend` 对应filter_fields属性，做单字段相等查询，`SearchFilter` 对应search_fields，用来做多字段模糊查询。但 `search fileds`使用 `like` 做的，所以存在效率问题，如果有并发什么的需求，请接入其他搜索

两者使用时需要配置 `filter_fields`或`search_fields`。两个`fields` 都可以采用 **外键__属性**的方式来做查询，例如 `search_fields = ('school__name')`。


#### filter 机制运作原理
机制是这样的，首先view调用 `get_queryset` 拿到 `queryset`，然后在`filter_backends` 用取到所有的 `backend` 进行 `filter`，因此你可以写出很多通用的 `filter_backend` 然后组合调用，例如 `filter_backends = (DjangoFilterBackend,SchoolPermissionFilterBackend)`。

因为django的 `queryset` 是只有真正做`list(list(qs))`或者`get(qs[2])`或者`for in`的时候才会**真正`hit`数据库**，所以这种拼接是没有任何问题的。

使用 `django-filter` 配合开发时，可以自定义其他的 `filter`，这样就省去在 `viewset` 里写大量简单的过滤字段查询接口，比如提供一个针对时间的查询接口。
`Django` 默认的自带了 `DjangoFilterBackend`过滤器。

另外，当使用`filter_class`时就 **不要写 `filter_fields`** 了，因为对应的Class中有一个Meta Class的fields属性代替了filter_fields。

## 4. django datetimefield 警告
1. 存储 `DateTimeField` 时，如果遇到警告 `RuntimeWarning: DateTimeField ModelName.field_name received a naive` 需要使用 `datetime.now().replace(tzinfo=pytz.utc)` 来 save 数据库记录。

## 5. put patch post 区别
从 HTTP 规范上来说，用PUT还是POST，不是看这是创建还是更新资源的动作，这不是风格的问题，而是语义的问题。
在HTTP中，PUT被定义为 idempotent (幂等)的方法，POST则不是，这是一个很重要的区别。**如果一个方法重复执行多次，产生的效果是一样的，那就是 idempotent (幂等)的**。

put、patch都是更新操作，但是前者是全量传递和更新，后者是局限于某一条件或者范围而言，简单的说就是 **两者的粒度是不同的**。

HTTP 协议规定如下:

```
1，GET 安全、幂等；用于获取资源；

2，HEAD 安全、幂等；与get方法类似，但不返回message body内容，仅仅是获得获取资源的部分信息（content-type、content-length）；restful框架中较少使用。

3，POST 非安全、非幂等；用于创建子资源

4，PUT 非安全、幂等；用于创建、更新资源；

5，DELETE 非安全、幂等；删除资源；

6，OPTIONS 安全、幂等；用于url验证，验证接口服务是否正常；

7，TEACE 安全、幂等；回显服务器收到的请求，这样客户端可以看到（如果有）哪一些改变或者添加已经被中间服务器实现。restful框架中较少使用

8，PATCH 非安全、幂等；用于创建、更新资源，于PUT类似，区别在于PATCH代表部分更新；后来提出的接口方法，使用时可能去要验证客户端和服务端是否支持；
```

## Refrences

[Django REST framework的各种技巧——5.搜索](https://segmentfault.com/a/1190000004400863)

[DRF Viewset里的ModelMin perform_create ](https://www.django-rest-framework.org/api-guide/generic-views/)

[django RuntimeWarning](https://docs.djangoproject.com/en/1.10/topics/i18n/timezones/#time-zones)
[when-to-use-serializers-create-and-modelviewsets-create-perform-create](https://stackoverflow.com/questions/41094013/when-to-use-serializers-create-and-modelviewsets-create-perform-create)

[HTTP协议中PUT和POST使用区别](https://blog.csdn.net/mad1989/article/details/7918267)

[http请求方法协议规定](https://blog.csdn.net/sunhuansheng/article/details/83183325)