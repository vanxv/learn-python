#编写第一个app first app

目标：
    允许用户查看投票并在其中投票的公共网站
    一个网站，你可以增加修改删除投票。
先运行下面的代码

```
python -m django --version
```

####创建第一个app

```
$ django-admin startproject mysite
```

你会看见下面的自动生产的文件夹：
```
mysite/
    manage.py
    mysite/
        __init__.py
        settings.py
        urls.py
        wsgi.py
```
mysite/：

前面你生成的文件夹名称

manage.py:
这个指令行程序，这个让我们和django项目互动作用。
各种指令方法。你能读所有这个细节manage.py在django-admin和manage.py
内部mysite/：

目录这是python包含你的项目，

mysite/__init__.py:
这是一个空文件，这个告知python这个目录应该是一个python包

mysite/settings.py:
设置配置这个django项目。将要[django setting](https://docs.djangoproject.com/en/1.10/topics/settings/)将要告知你全部关于怎样设置工作

mysite/urls.py:
这个URL是申明这个Django项目；
一个"目录"你的Django路由器，你能阅读更多关于[URLs dispatcher](https://docs.djangoproject.com/en/1.10/topics/http/urls/)

mysite/wsgi.py:
一个空的指向对于WSGI-兼容网页服务至服务于你的项目。可以看WSGI文章

###运行系统
```
>python manage.py runserver
```

你可以看见下面的输出指令
```python
Performing system checks...
#正在检验系统
System check identified no issues (0 silenced).
#系统检验确定没有问题
You have unapplied migrations; your app may not work properly until they are applied.
Run 'python manage.py migrate' to apply them.
#你拥有没有应用的迁移；你的App
January 30, 2017 - 15:50:53
2017年1月30-
Django version 1.10, using settings 'mysite.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
```
你可以打开页面：http://127.0.0.1:8000/
也可以修改端口
```
python manage.py runserver 8080
```
如果需要修改网站的ip
```
python manage.py runserver 0.0.0.0:8000
```

##创建一个投票的app
```
python manage.py startapp polls
```

会生产目录：
```python
polls/
    __init__.py
    admin.py
    apps.py
    migrations/
        __init__.py
    models.py
    tests.py
    views.py
```

###写你的第一个页面
插入HttpResponse
返回请求值
意思是给index返回数值
当访问你的程序时，你的程序直接返回数值("hello world")
**polls/views.py**
```python
from django.http import HttpResponse

def index(request):
    return HttpResponse("Hello, world. You're at the polls index.")
```

####创建一个URL路由在这个polls投票目录。
创建一个文件名为 **urls.py** 你的目录变成这个样子

```
polls/
    __init__.py
    admin.py
    apps.py
    migrations/
        __init__.py
    models.py
    tests.py
    urls.py
    views.py
```

####下面是在url中设置路由功能

在 ** polls/urls.py ** 文件，跟着我打代码
```python
from django.conf.urls import url
#插入url模块，让系统可以访问网站
from . import views
#插入view视觉模块。
urlpatterns = [
    url(r'^$', views.index, name='index'),
    #默认访问views.index函数。
]
```
因为polls是mysite的子系统，所以polls的路由要到mysite urls.py报道才能用。
下一步是指向polls.urls模型。在mysite/urls.py中
增加插入 **django.conf.urls.include** 和插入 **include()** 在urlpatterns列表中。
因此你在mysite/urls.py

```python
from django.conf.urls import include, url
#插入url方法的include包含模块和url模块
from django.contrib import admin
#插入admin模块
urlpatterns = [
    url(r'^polls/', include('polls.urls')),
    #这个
    url(r'^admin/', admin.site.urls),
]
```
这个 **include()** 函数允许访问其他的URLconfs.
注意这个规则表达式对于include() 例：**include('polls.urls')**

```pyhton
python manage.py runserver
```
打开localhost:8000/polls/ 你的浏览器和你的应该可以看见你的文字
