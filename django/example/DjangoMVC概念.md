####DjangoMVC概念.md

![gras](https://www.tutorialspoint.com/django/images/django_mvc_mvt_pattern.jpg)

用户访问服务器IP->进入URL.py找到views.py中的访问代码，访问代码通过models和html放回结果。

**环境**
MAC + PYthon3.5 + Django 1.10

第一步安装python

安装就不写了。自行google一下
3.5以上的吧。

第二步安装Django
```python
pip install django
```

第三步检验一下是否安装成功
```python
django-admin.py --version
```

第四步 创建一个DJango项目
```python
django-admin startproject exampleapp
```

第五步 运行测试一下app
```python
python manage.py runserver
```

应该可以打开主页了。
```python
Validating models...

0 errors found
September 03, 2015 - 11:41:50
Django version 1.6.11, using settings 'myproject.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
```

创建一个app
```python
python manage.py startapp myapp
```

绑定系统
在 setting.py中绑定
```
INSTALLED_APPS = (
   'django.contrib.admin',
   'django.contrib.auth',
   'django.contrib.contenttypes',
   'django.contrib.sessions',
   'django.contrib.messages',
   'django.contrib.staticfiles',
   'myapp', #增加的
)
```

自动生成数据库
```python
python manage.py migrate
```

创建超级用户
```python
python manage.py createsuperuser
```


启动服务器
```python
python manage.py runserver
```

打开
127.0.0.1:8000/admin

DJango的数据管理系统／admin管理系统
##做一个简单的页面helloword

在 **myapp/templates/hello.html**
启动一个hello.html
输入代码
```html
from django.http import HttpResponse

def hello(request):
   text = """<h1>welcome to my app !</h1>"""
   return HttpResponse(text)
```

在 **myapp/views.py** 中 修改下面的代码，
```python
from django.shortcuts import render
from django.template import loader

def hello(request):
   return render(request, "hello.html")
```

在 **exampleapp/urls.py** 中 修改下面的代码
```python
from django.conf.urls import url
from django.contrib import admin
from myapp import views


urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'^hello/', views.hello, name = 'hello'),
]

   )
```

解释：
**'^hello/'** 访问的地址http://127.0.0.1/hello

**'myapp.views.hello'** 访问的函数

**name = 'hello'** 传值的意思，先不管他。

现在打开网站可以访问了
```python
python manage.py runserver
```

访问的地址http://127.0.0.1/hello 

简单的网页就做好了，

##设置URLS，每一个APP有URLS

修改 **exampleapp/urls.py**

```python
from django.conf.urls import url, include
from django.contrib import admin


urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'^myapp/', include('myapp.urls'))
        ]
```


修改 **myapp/urls.py**
```python
from django.conf.urls import include, url
from . import views
urlpatterns = [
    url(r'^hello/', views.hello)
]
```
分页面分发urls路由
打开服务器
```
python manage.py runserver
```

访问 http://127.0.0.1:8000/myapp/hello/


##获取功能
- [  ] 一个简单功能请求
- [  ] 模版的路径
- [  ] 参考字典

###DJANGO的模版语言（Django Template Language (DTL)）

修改**templates/hello.html**
```html
<html>
   
   <body>
      Hello World!!!<p>Today is {{today}}</p>
   </body>
   
</html>
```

修改 **myapp/views.py**
```
from django.shortcuts import render
from django.template import loader
from datetime import datetime

def hello(request):
    today = datetime.now()
    print(today)
    helloDict = {}
    helloDict['today'] = today
    return render(request, 'hello.html', helloDict)
```

这里的helloDict是字典传到前端读取字段。
{{today}}是为了显示前面的字典。

修改 **hello.html**
```html
<html>
   <body>
   
      Hello World!!!<p>Today is {{today}}</p>
      We are
      {% if today.day == 1 %}
      
      the first day of month.
      {% elif today == 30 %}
      
      the last day of month.
      {% else %}
      
      I don't know.
      {%endif%}
      
   </body>
</html>
```
增加判断语句。这里还是用debug看一次。数据是怎么传输的。
https://www.tutorialspoint.com/django/django_models.htm补充语句需要增加。

##models.py 增加数据库
```python
from django.db import models

class Dreamreal(models.Model):

   website = models.CharField(max_length = 50)
   mail = models.CharField(max_length = 50)
   name = models.CharField(max_length = 50)
   phonenumber = models.IntegerField()

   class Meta:
      db_table = "dreamreal"
```
3个Char变量
1个int变量

运行：
python3 manage.py makemigrations
python3 manage.py migrate