# 创建虚拟环境

- 创建：mkvirtualenv [虚拟环境名称]
- 删除：rmvirtualenv [虚拟环境名称]
- 进入：workon [虚拟环境名称]
- 退出：deactivate
- 所有的虚拟环境，都位于/home/.virtualenvs目录下
- 进入虚拟环境前的提示：
![](/assets/workon1.png)

- 进入虚拟环境后的提示：
![](/assets/workon2.png)

- 查看当前的所有虚拟环境：workon [两次tab键]
- 查看虚拟环境中已经安装的包

```
pip list
pip freeze

```

# 安装django

- 建议安装1.8.2版本，这是一个稳定性高、使用广、文档多的版本
`pip install django==1.8.2`
- 查看版本：进入python shell，运行如下代码
import django
django.get_version()
- 说明：使用pip install django命令进行安装时，会自动删除旧版本，再安装新版本

# 创建项目

- 命令django-admin startproject test1
- 进入test1目录，目录结构如下图：

![](/assets/test1.png)

# 目录说明

- manage.py：一个命令行工具，可以使你用多种方式对Django项目进行交互
- 内层的目录：项目的真正的Python包
- _init _.py：一个空文件，它告诉Python这个目录应该被看做一个Python包
- settings.py：项目的配置
- urls.py：项目的URL声明
- wsgi.py：项目与WSGI兼容的Web服务器入口