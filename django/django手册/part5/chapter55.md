# 分页

- Django提供了一些类实现管理数据分页，这些类位于django/core/paginator.py中

# Paginator对象

- Paginator(列表,int)：返回分页对象，参数为列表数据，每面数据的条数

## 属性

- count：对象总数
- num_pages：页面总数
- page_range：页码列表，从1开始，例如[1, 2, 3, 4]

## 方法

- page(num)：下标以1开始，如果提供的页码不存在，抛出InvalidPage异常

## 异常exception

- InvalidPage：当向page()传入一个无效的页码时抛出
- PageNotAnInteger：当向page()传入一个不是整数的值时抛出
- EmptyPage：当向page()提供一个有效值，但是那个页面上没有任何对象时抛出

# Page对象

## 创建对象

- Paginator对象的page()方法返回Page对象，不需要手动构造

## 属性

- object_list：当前页上所有对象的列表
- number：当前页的序号，从1开始
- paginator：当前page对象相关的Paginator对象

## 方法

- has_next()：如果有下一页返回True
- has_previous()：如果有上一页返回True
- has_other_pages()：如果有上一页或下一页返回True
- next_page_number()：返回下一页的页码，如果下一页不存在，抛出InvalidPage异常
- previous_page_number()：返回上一页的页码，如果上一页不存在，抛出InvalidPage异常
- len()：返回当前页面对象的个数
- 迭代页面对象：访问当前页面中的每个对象

# 示例

- 创建视图pagTest



```
from django.core.paginator import Paginator

def pagTest(request, pIndex):
    list1 = AreaInfo.objects.filter(aParent__isnull=True)
    p = Paginator(list1, 10)
    if pIndex == '':
        pIndex = '1'
    pIndex = int(pIndex)
    list2 = p.page(pIndex)
    plist = p.page_range
    return render(request, 'booktest/pagTest.html', {'list': list2, 'plist': plist, 'pIndex': pIndex})

```

## 配置url

`url(r'^pag(?P<pIndex>[0-9]*)/$', views.pagTest, name='pagTest'),`

## 定义模板pagTest.html



```
<!DOCTYPE html>
<html>
<head>
    <title></title>
</head>
<body>
<ul>
{%for area in list%}
<li>{{area.id}}--{{area.atitle}}</li>
{%endfor%}
</ul>

{%for pindex in plist%}
{%if pIndex == pindex%}
{{pindex}}&nbsp;&nbsp;
{%else%}
<a href="/pag{{pindex}}/">{{pindex}}</a>&nbsp;&nbsp;
{%endif%}
{%endfor%}
</body>
</html>
```

