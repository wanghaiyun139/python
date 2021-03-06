# 中间件

- 是一个轻量级、底层的插件系统，可以介入Django的请求和响应处理过程，修改Django的输入或输出
- 激活：添加到Django配置文件中的MIDDLEWARE_CLASSES元组中
- 每个中间件组件是一个独立的Python类，可以定义下面方法中的一个或多个
  - _init _：无需任何参数，服务器响应第一个请求的时候调用一次，用于确定是否启用当前中间件
  - process_request(request)：执行视图之前被调用，在每个请求上调用，返回None或HttpResponse对象
  - process_view(request, view_func, view_args, view_kwargs)：调用视图之前被调用，在每个请求上调用，返回None或HttpResponse对象
  - process_template_response(request, response)：在视图刚好执行完毕之后被调用，在每个请求上调用，返回实现了render方法的响应对象
  - process_response(request, response)：所有响应返回浏览器之前被调用，在每个请求上调用，返回HttpResponse对象
process_exception(request,response,exception)：当视图抛出异常时调用，在每个请求上调用，返回一个HttpResponse对象
- 使用中间件，可以干扰整个处理过程，每次请求中都会执行中间件的这个方法
- 示例：自定义异常处理
- 与settings.py同级目录下创建myexception.py文件，定义类MyException，实现process_exception方法

```
from django.http import HttpResponse
class MyException():
    def process_exception(request,response, exception):
        return HttpResponse(exception.message)

```

- 将类MyException注册到settings.py中间件中

```
MIDDLEWARE_CLASSES = (
    'test1.myexception.MyException',
    ...
)

```

- 定义视图，并发生一个异常信息，则会运行自定义的异常处理