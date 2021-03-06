# URLconf

- 在settings.py文件中通过ROOT_URLCONF指定根级url的配置
- urlpatterns是一个url()实例的列表
- 一个url()对象包括：
  - 正则表达式
  - 视图函数
  - 名称name
- 编写URLconf的注意：
  - 若要从url中捕获一个值，需要在它周围设置一对圆括号
  - 不需要添加一个前导的反斜杠，如应该写作'test/'，而不应该写作'/test/'
  - 每个正则表达式前面的r表示字符串不转义
- 请求的url被看做是一个普通的python字符串，进行匹配时不包括get或post请求的参数及域名

`http://www.itcast.cn/python/1/?i=1&p=new，只匹配“/python/1/”部分`
- 正则表达式非命名组，通过位置参数传递给视图

`url(r'^([0-9]+)/$', views.detail, name='detail'),`
- 正则表达式命名组，通过关键字参数传递给视图，本例中关键字参数为id

`url(r'^(?P<id>[0-9]+)/$', views.detail, name='detail'),`
- 参数匹配规则：优先使用命名参数，如果没有命名参数则使用位置参数
- 每个捕获的参数都作为一个普通的python字符串传递给视图
- 性能：urlpatterns中的每个正则表达式在第一次访问它们时被编译，这使得系统相当快

## 包含其它的URLconfs

- 在应用中创建urls.py文件，定义本应用中的urlconf，再在项目的settings中使用include()


```
from django.conf.urls import include, url
urlpatterns = [
    url(r'^', include('booktest.urls', namespace='booktest')),
]

```

- 匹配过程：先与主URLconf匹配，成功后再用剩余的部分与应用中的URLconf匹配

```
请求http://www.itcast.cn/booktest/1/
在sesstings.py中的配置：
url(r'^booktest/', include('booktest.urls', namespace='booktest')),
在booktest应用urls.py中的配置
url(r'^([0-9]+)/$', views.detail, name='detail'),
匹配部分是：/booktest/1/
匹配过程：在settings.py中与“booktest/”成功，再用“1/”与booktest应用的urls匹配

```

- 使用include可以去除urlconf的冗余
- 参数：视图会收到来自父URLconf、当前URLconf捕获的所有参数
- 在include中通过namespace定义命名空间，用于反解析

## URL的反向解析

- 如果在视图、模板中使用硬编码的链接，在urlconf发生改变时，维护是一件非常麻烦的事情
- 解决：在做链接时，通过指向urlconf的名称，动态生成链接地址
- 视图：使用django.core.urlresolvers.reverse()函数
- 模板：使用url模板标签