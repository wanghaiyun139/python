# POST属性

- QueryDict类型的对象
- 包含post请求方式的所有参数
- 与form表单中的控件对应
- 问：表单中哪些控件会被提交？
- 答：控件要有name属性，则name属性的值为键，value属性的值为键，构成键值对提交
  - 对于checkbox控件，name属性一样为一组，当控件被选中后会被提交，存在一键多值的情况
- 键是开发人员定下来的，值是可变的
- 示例如下
- 定义视图postTest1


```
def postTest1(request):
    return render(request,'booktest/postTest1.html')

```

- 配置url
`url(r'^postTest1$',views.postTest1)`
- 创建模板postTest1.html


```html
<html>
<head>
    <title>Title</title>
</head>
<body>
<form method="post" action="/postTest2/">
    姓名：<input type="text" name="uname"/><br>
    密码：<input type="password" name="upwd"/><br>
    性别：<input type="radio" name="ugender" value="1"/>男
    <input type="radio" name="ugender" value="0"/>女<br>
    爱好：<input type="checkbox" name="uhobby" value="胸口碎大石"/>胸口碎大石
    <input type="checkbox" name="uhobby" value="跳楼"/>跳楼
    <input type="checkbox" name="uhobby" value="喝酒"/>喝酒
    <input type="checkbox" name="uhobby" value="爬山"/>爬山<br>
    <input type="submit" value="提交"/>
</form>
</body>
</html>

```

- 创建视图postTest2接收请求的数据

```
def postTest2(request):
    uname=request.POST['uname']
    upwd=request.POST['upwd']
    ugender=request.POST['ugender']
    uhobby=request.POST.getlist('uhobby')
    context={'uname':uname,'upwd':upwd,'ugender':ugender,'uhobby':uhobby}
    return render(request,'booktest/postTest2.html',context)

```

- 配置url

`url(r'^postTest2$',views.postTest2)`
- 创建模板postTest2.html

```
<html>
<head>
    <title>Title</title>
</head>
<body>
{{ uname }}<br>
{{ upwd }}<br>
{{ ugender }}<br>
{{ uhobby }}
</body>
</html>

```
- 注意：使用表单提交，注释掉settings.py中的中间件crsf