---
layout: article
title:  "Django REST framework"
rootCate: "work"
show_edit_on_github: false
pageview: true
comment: true
key: work_python_django_modules_django_rest_framwork_20190319
categories:
- Python
tags:
- work
- Python3
- Django
- RESTful
- restframework
---

Django REST框架是一个功能强大且灵活的工具包，用于构建Web API。
它的功能包括以下内容：Web浏览器API为您的开发人员带来了巨大的可用性; 身份验证策略包括OAuth1a和OAuth2的程序包; 支持ORM和非ORM数据源的序列化;可自定义功能。

<!---more--->


## 1. Django rest_framework 简介
rest_framework 可以轻易的甚至自动化的搞定很多事情，比如：

+ 自动生成符合 RESTful 规范的 API
+ 支持 OPTION、HEAD、POST、GET、PATCH、PUT、DELETE
+ 根据 Content-Type 来动态的返回数据类型（如 text、json）
+ 生成 browserable 的交互页面（自动为 API 生成非常友好的浏览器页面）
+ 非常细粒度的权限管理（可以细粒度到 field 级别）

继承这个框架后开发的view等，**可以不用写所有的post/get/put/delete方法**，只需要写自己新增的其他方法就好了。REST基恩的4~6个方法，可以直接访问使用。

### 1.1 使用教程
1. Models
Model有太多的field类型，记得恰当使用，比如 `OneToOne、ManyToMany、UnixTimeField(结合第三方插件)、DateTimeField、BoolleanField、Forienkey`，另外还可以设置最长最短字符长度、设置外键表如何调用本表的 `relate_names` 、设置默认值、 **通过设置objects变量自定义软删功能等**。
还有 class meta 里记得设置自己想要的表名，以及可以设置 `ordering` 的排序主键。
2. Serializers
定义 Models，我们可以开始写 Serializers，这个相当于 Django 的 Form，用于校验前端访问提交的参数的格式、值内容是否符合要求，也可以做一些。  
接下来就是写 View 文件，这里有两种选择，如果是简单的需求可以用 APIView 一个方法搞定，复杂的需求就可以用 ViewSet 类。本质上，ViewSet 是对APIView结合了 Mixin 类的封装，默认就拥有各种 POST/PUT 等功能。

3. Mixin
Mixin 类翻译是组合类，作用是配合其他类一起被某个子类继承，这个子类就同时拥有了父类和这个 Mixin 类的权限和功能。在 DRF 框架中，Minin 类
ListModelMixin : **列表视图扩展类**，提供 `list(request, *args, **kwargs)`方法快速实现列表视图，返回200状态码。该Mixin的 `list` 方法会对数据进行 `过滤和分页`。

CreateModelMixin : **创建视图扩展类**，提供 `create(request, *args, **kwargs)`方法快速实现创建资源的视图，成功返回201状态码。如果序列化器对前端发送的数据验证失败，返回400错误。

RetrieveModelMixin : **详情视图扩展类**，提供 `retrieve (request, *args, **kwargs)` 方法，可以快速实现返回一个存在的数据对象。如果存在，返回200， 否则返回404。

UpdateModelMixin : **更新视图扩展类**，提供 `update(request, *args, **kwargs)` 方法，可以快速实现更新一个存在的数据对象。同时也提供partial_update(request, *args, **kwargs)方法，可以实现局部更新。成功返回200，序列化器校验数据失败时，返回400错误。

DestroyModelMixin :  **删除视图扩展类**，提供 `destroy(request, *args, **kwargs)` 方法，可以快速实现删除一个存在的数据对象。成功返回204，不存在返回404。

4. APIView 
   APIVIEW 对 django 本身的View进行封装，与自己写的view方法的差别看起来不大，但实际中 APIView 做了很多东西，它定义了很多属性与方法。
> 4.1. 加入 queryset 属性，可以直接设置这个属性，不必再将实例化的 courses，再次传给 seriliazer，系统会自动检测到。除此之外，可以重载 get_queryset()，这样就不必设置 `queryset=\*`，这样就变得更加灵活，可以进行完全的自定义。
> 4.2. 加入 `serializer_class` 属性与实现 `get_serializer_class()` 方法。两者的存在一个即可，通过这个，在返回时，不必去指定某个serilizer
+ 设置过滤器模板：`filter_backends`
+ 设置分页模板：pagination_class
在generics除了GenericAPIView还包括了其他几个View: CreateAPIView、ListAPIView、RetrieveAPIView、ListCreateAPIView···等等，其实他们都只是继承了相应一个或多个mixins和GenericAPIView

5. Viewset
定义好了 Serializers，就可以开始写 viewset 了。其实 viewset 反而是最简单的部分，`django_rest_framework` 原生提供了四种 ViewSet，四个 ViewSet 区别主要体现在能否对 `Put/Post/Patch/Get` 等不同方法访问的权限上面，这四个 ViewSet 实际上是对 APIView 类进一步封装，组合了各种权限 Mixin 类，所以才默认就拥有了各种权限。

ViewSet视图集类的动作：
+ list() 提供一组数据
+ retrieve() 提供单个数据
+ create() 创建数据
+ update() 保存数据
+ destory() 删除数据

## 2. DRF View 
(1) CreateAPIView
提供 post 方法，继承自： GenericAPIView、CreateModelMixin

(2）ListAPIView
提供 get 方法，继承自：GenericAPIView、ListModelMixin

(3）RetireveAPIView
提供 get 方法，继承自: GenericAPIView、RetrieveModelMixin

(4）DestoryAPIView
提供 delete 方法，继承自：GenericAPIView、DestoryModelMixin

(5）UpdateAPIView
提供 put 和 patch 方法，继承自：GenericAPIView、UpdateModelMixin

(6）RetrieveUpdateAPIView
提供 get、put、patch方法，继承自： GenericAPIView、RetrieveModelMixin、UpdateModelMixin

(7）RetrieveUpdateDestoryAPIView
提供 get、put、patch、delete方法，继承自：GenericAPIView、RetrieveModelMixin、UpdateModelMixin、DestoryModelMixin

## 3. DRF ViewSet 属性

+ ViewSet : 继承自APIView，作用也与APIView基本类似，提供了身份认证、权限校验、流量管理等。在ViewSet中，没有提供任何动作action方法，需要我们自己实现action方法。
+ GenericViewSet : 继承自GenericAPIView，作用也与GenericAPIVIew类似，提供了get_object、get_queryset等方法便于列表视图与详情信息视图的开发。
+ ModelViewSet : 继承自GenericAPIVIew，同时包括了ListModelMixin、RetrieveModelMixin、CreateModelMixin、UpdateModelMixin、DestoryModelMixin。
+ ReadOnlyModelViewSet : 继承自GenericAPIVIew，同时包括了ListModelMixin、RetrieveModelMixin。

### 3.1 DRF 的request参数
##### GET
`request.query_params` 中可以获取`?param1=32&param2=23`形式的参数.

`request.query_params` 返回的数据类型为 QueryDict。
QueryDict转为普通python字典使用 `query_params.dict()`即可.

##### POST

|提交类型 |	参数位置|	参数类型|
|---|---|---|---|
|form-data提交,	|request.data	|QueryDict|
|application/json提交|request.data |	dict|
|(swagger)使用接口文档提交|用curl提交，参数被拼接到了url之后,	所以参数在`query_params`中| QueryDict|
|x-www-form-urlencoded|	request.data| QueryDict|

### 3.2 mixins
但 GenericViewSet 本身依然不存在list, create方法，需要我们与mixins一起混合使用，那么新问题来了？我们依然需要自己写get、post方法，然后再return list或者create等方法吗？

当然不！重写as_view的方法为我们提供了绑定的功能，我们在设置url的时候:
```
# 进行绑定
courses = CourseViewSet.as_view({
    'get': 'list',
    'post': 'create'
})
urlpatterns = [
    ...
    # 常规加入url匹配项
    url(r'courses/', CourseViewSet.as_view(), name='courses')]
```
当然，很多时候可以改用其他的 ViewSet 来运行，这样可以省略 mixins 搭配，例如 ModelViewSet 已经集成了相关的 mixins。

minxis的类有5种，他们分别对应了对数据库的增查改删操作,使用它们的好处是不用重复写着相同的业务代码逻辑,因为每个mixins内部都写好了对应的逻辑,只需要设置一下queryset和serializer_class就可以了 :
CreateModelMixin,ListModelMixin,RetrieveModelMixin,UpdateModelMixin,DestroyModelMixin

ViewSet也有5种,分别是:
ViewSetMixin,ViewSet,GenericViewSet,ReadOnlyModelViewSet,ModelViewSet

### 3.3 router
asview方法可以绑定url，并添加一些action。很明显，当然有，这个时候，route方法注册与绑定可以更加简化。
```
from rest_framework.routers import DefaultRouter
router = DefaultRouter() # 只需要实现一次
router.register(r'courses', CourseViewSet, base_name='courses')
urlpatterns = [
    ...
    # 只需要加入一次
    url(r'^', include(router.urls)),]
```

在设置路由时，我们可以如下操作，使用as_view()方法的时候，才会将视图集(ViewSet)的 action动作与具体请求方式对应上
```
urlpatterns = [
    url(r'^books/$', BookInfoViewSet.as_view({'get':'list'}),
    url(r'^books/(?P<pk>\d+)/$', BookInfoViewSet.as_view({'get': 'retrieve'})
]
```

### 3.4 Filters
`pip install django-filter`, 然后把django-filter加到INSTALLED_APPS列表中
前面根据 serializers 和 viewset 我们已经可以很好的提供数据接口和展示了。但是有时候我们需要通过 url参数 来对数据进行一些 **排序或过滤** 的操作，为此，rest-framwork 提供了 filters 来满足这一需求（filters）。

### 3.5 自定义分页
自定义分页（DRF框架也有默认的）：
```
from rest_framework.pagination import PageNumberPagination
class UsersPagination(PageNumberPagination):
    # 指定每一页的个数
    page_size = 10
    # 可以让前端来设置page_szie参数来指定每页个数
    page_size_query_param = 'page_size'
    # 设置页码的参数
    page_query_param = 'page'
```
viewset:
```
class UserViewSet(mixins.ListModelMixin, mixins.CreateModelMixin, viewsets.GenericViewSet):
    # 设置分页的class
    pagination_class = UsersPagination
```

### 3.6 Permissions
顾名思义就是权限管理，用来给 ViewSet 设置权限，使用 premissions 可以方便的设置不同级别的权限：
+ 全局权限控制
+ ViewSet 的权限控制
+ Method 的权限
+ Object 的权限
  
自定义权限：
```
from rest_framework import permissions
from rest_framework.permissions import IsAuthenticated
class IsOwnerOrReadOnly(permissions.BasePermission):
    def has_object_permission(self, request, view, object):
        if request.method in permissions.SAFE_METHODS:
            return True
        return object.user == request.user
```

viewset:
```
from rest_framework.permissions import IsAuthenticated
class UserViewSet(ModelViewSet):
    permission_classes = (IsAuthenticated, IsOwnerOrReadOnly)
```

## 4. ViewSet 和 APIView 的区别和联系
ViewSet类型的类：可以直接用router引入后，不用具体指定每个view方法的url。url最后以方法名结尾。ViewSet类会在背后结合serializer做参数校验、做很多权限认证之类的事情，一般用于实现一套的RESTful风格的代码。

不同的ViewSet类有不同权限(Post/Put/List等)，一般使用权限最多的ModelViewSet。

APIView类型的类：可以用as_view方法引入，不用具体指定每个view方法的url。url最后以url文件里配置的名字结尾，django会转到view里面的类和类的方法。重点是APIView一般是比较裸的，也就是说当前端发起请求到最后找到方法执行的过程里会省略一些权限认证之类的东西，一般也只有一个方法实现。

ViewSet视图集类不再实现get()、post()等方法，而是实现动作 action 如 list() 、create() 等。

视图集只在使用as_view()方法的时候，才会将action动作与具体请求方式对应上。

##5 permissions - JWT

###5.1 使用 JWT
第一步: `pip install djangorestframework-jwt`

第二步: 在url.py中配置
```
from rest_framework_jwt.views import obtain_jwt_token
urlpatterns = [
  ...
  url(r'^api-token-auth/', obtain_jwt_token),
  ...
]
```

第三步: 在需要jwt认证的ViewSet的类里面设置
```
from rest_framework_jwt.authentication import JSONWebTokenAuthentication
from rest_framework.authentication import SessionAuthentication

class XXXViewSet(mixins.CreateModelMixin, viewsets.GenericViewSet):
  authentication_classes = (JSONWebTokenAuthentication, SessionAuthentication)
  //加上 SessionAuthentication 是为了在网站上调试方便
```

###5.2 使用 JWT 登录
现在注册登录有两种方式:
+ 用户注册之后跳转到登录页面让其登录
+ 用户注册之后自动帮他登录了

第一种情况的话我们无需再做其他操作,

第二种情况我们应该在用户注册之后返回`jwt token`的字段给前台,所以要做两步:
+ 因为返回字段是`mixins`帮我们做好了,所以我们要重写对应的方法来修改返回字段。
+ 需要查看`djangorestframework-jwt`的源码找到生成`jwt token`的方法

```
from rest_framework_jwt.serializers import jwt_encode_handler, jwt_payload_handler
class UserViewSet(CreateModelMixin, RetrieveModelMixin,UpdateModelMixin,viewsets.GenericViewSet):
    # 重写create方法
    def create(self, request, *args, **kwargs):
        serializer = self.get_serializer(data=request.data)
        serializer.is_valid(raise_exception=True)
        user = self.perform_create(serializer)
        # 在新建用户保存到数据库之后
        tmp_dict = serializer.data
        # 生成JWT Token
        payload = jwt_payload_handler(user)
        tmp_dict['token'] = jwt_encode_handler(payload)
        headers = self.get_success_headers(serializer.data)
        return Response(tmp_dict, status=status.HTTP_201_CREATED, headers=head
```



## 参考列表
[django-rest-framework(概念篇)——apiview&viewset](https://www.jianshu.com/p/2700ff413250)

[利用 Django REST framework 编写 RESTful API（drf的filter）](https://www.cnblogs.com/xiaojikuaipao/p/6009882.html)

[django-rest-framework解析请求参数](https://www.jianshu.com/p/f2f73c426623)

[Django REST framework的关键技巧](https://www.jianshu.com/p/f3606a5def69)

[django-filter](https://django-filter.readthedocs.io/en/master/)

[](https://www.cnblogs.com/skaarl/p/9487882.html)