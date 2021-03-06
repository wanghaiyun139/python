# ORM简介

- MVC框架中包括一个重要的部分，就是ORM，它实现了数据模型与数据库的解耦，即数据模型的设计不需要依赖于特定的数据库，通过简单的配置就可以轻松更换数据库
- ORM是“对象-关系-映射”的简称，主要任务是：
  - 根据对象的类型生成表结构
  - 将对象、列表的操作，转换为sql语句
  - 将sql查询到的结果转换为对象、列表
- 这极大的减轻了开发人员的工作量，不需要面对因数据库变更而导致的无效劳动
- Django中的模型包含存储数据的字段和约束，对应着数据库中唯一的表
  ![](/assets/orm.png)

# 使用MySql数据库

- 在虚拟环境中安装mysql包

```
pip install mysql-python

```

- 在mysql中创建数据库

```
create databases test2 charset=utf8
```


- 打开settings.py文件，修改DATABASES项


```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'test2',
        'USER': '用户名',
        'PASSWORD': '密码',
        'HOST': '数据库服务器ip，本地可以使用localhost',
        'PORT': '端口，默认为3306',
    }
}


```


# 开发流程

1. 在models.py中定义模型类，要求继承自models.Model
2. 把应用加入settings.py文件的installed_app项
3. 生成迁移文件
4. 执行迁移生成表
5. 使用模型类进行crud操作

# 使用数据库生成模型类


```

python manage.py inspectdb > booktest/models.py
```


