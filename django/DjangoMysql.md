#Django mysql add delete change search
重要的事情说三次，一切代码都是输入与输出
print（debug）！print（debug）！！print（debug）！！！

mysql
下载安装MySQLdb类库
http://www.djangoproject.com/r/python-mysql/


```python
django-admin startproject sitename
python manage.py startapp web
```

在 **setting.py** 中 修改
```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'msadcs',
        'USER': 'root',
        'PASSWORD': '',
        'HOST': 'localhost',   # Or an IP Address that your DB is hosted on
        'PORT': '3306',
    }
}
```

启动app叫web **setting.py**

在 **setting.py** 捆绑web
```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'web',
]
```
编写 **models.py**
```python
from django.db import models

class UserInfo(models.Model):
    user = models.CharField(max_length=16)
    pwd = models.CharField(max_length=32)
    ip = models.GenericIPAddressField(protocol="ipv4",null=True,blank=True)
    img = models.ImageField(null=True,blank=True,upload_to="upload")
# Create your models here.
```


```python
python manage.py makemigrations

python manage.py migrate
```
建数据库表
数据库增加到admin系统
web\admin.py
```python
from django.contrib import admin
from web import models
admin.site.register(models.UserInfo)
# Register your models here.
```

```python
python manage.py createsuperuser
python manage.py runserver
```
http://127.0.0.1:8000/admin

编写 **views.py** 建系统操作表
```python
from django.shortcuts import render
from django.template import loader
def show(request):
    from web import models
    if request.method == 'POST':
        a_user = request.POST['a_user']
        a_pwd = request.POST['a_pwd']
        models.UserInfo.objects.create(user=a_user,pwd=a_pwd)
    user_list = models.UserInfo.objects.all() #get all
    return render(request, 'show.html', {'user_list' :user_list})
# Create your views here.
```

给 **web/urls.py** 修改路径
```python
from django.conf.urls import url
from . import views
urlpatterns = [
    url(r'^show/', views.show)
]
```

设置主 **urls.py** 载入子web/urls.py
```python
from django.conf.urls import url, include
from django.contrib import admin
from web import views
urlpatterns = [
    url(r'^admin/', admin.site.urls),
#    url(r'^web/', include('web.urls')),

    url(r'^show/', views.show)
]
```

在web下设置 **web/Templates/show.html**
```html
<body>
  <form action="/show/" method="POST">
    <input type="text" name="a_user" />
    <input type="password" name="a_pwd" />
    <input type="submit" name="增加" />
    {% csrf_token %}
  </form>
{% for line in user_list %}
  <tr>
    <td>{{ line.user }}</td>
    <td>{{ line.pwd }}</td>
  </tr>
{% endfor %}
</body>
```

编辑好了运行一下咯，
```python
python manage.py runserver 
```

访问
http://127.0.0.1:8000/show/
可以随便输入添加宝贝咯。
如果有bug请恢复我。
现在解释里面的意思
服务器通过urls，访问views中对应的方法 def show
然后执行判断语句，返回html。

数据库实现增删改
```python
from django.shortcuts import render
from django.template import loader
def show(request):
    from web import models
    if request.method == 'POST':
        a_user = request.POST['a_user'] #读取a_user
        a_pwd = request.POST['a_pwd'] #读取a_user
        #models.UserInfo.objects.create(user=a_user,pwd=a_pwd) #增加
        #models.UserInfo.objects.filter(user=a_user).delete() #删除
        models.UserInfo.objects.filter(user=a_user).update(pwd='520')
    user_list = models.UserInfo.objects.all() #get all
    return render(request, 'show.html', {'user_list' :user_list}) #读取数据
# Create your views here.
```

自行把每一句话测试一下。
models.UserInfo.objects.filter查到的意思。

        models.UserInfo.objects.create(user=a_user,pwd=a_pwd) #增加
        models.UserInfo.objects.filter(user=a_user).delete() #删除
        models.UserInfo.objects.filter(user=a_user).update(pwd='520')


下面是django ORM的操作数据，用debug，用debug，用debug
```python
# 增
    #
    # models.Tb1.objects.create(c1='xx', c2='oo')  增加一条数据，可以接受字典类型数据 **kwargs
    # obj = models.Tb1(c1='xx', c2='oo')
    # obj.save()
    # 查
    #
    # models.Tb1.objects.get(id=123)         # 获取单条数据，不存在则报错（不建议）
    # models.Tb1.objects.all()               # 获取全部
    # models.Tb1.objects.filter(name='seven') # 获取指定条件的数据
    # 删
    #
    # models.Tb1.objects.filter(name='seven').delete() # 删除指定条件的数据
    # 改
    # models.Tb1.objects.filter(name='seven').update(gender='0')  # 将指定条件的数据更新，均支持 **kwargs
     
    # obj = models.Tb1.objects.get(id=1)    # 修改单条数据
    # obj.c1 = '111'
    # obj.save()                                                 
   
   
    # 获取个数
    #
    # models.Tb1.objects.filter(name='seven').count()
    # 大于，小于
    #
    # models.Tb1.objects.filter(id__gt=1)              # 获取id大于1的值
    # models.Tb1.objects.filter(id__lt=10)             # 获取id小于10的值
    # models.Tb1.objects.filter(id__lt=10, id__gt=1)   # 获取id大于1 且 小于10的值
    # in
    #
    # models.Tb1.objects.filter(id__in=[11, 22, 33])   # 获取id等于11、22、33的数据
    # models.Tb1.objects.exclude(id__in=[11, 22, 33])  # not in
    # contains
    #
    # models.Tb1.objects.filter(name__contains="ven")
    # models.Tb1.objects.filter(name__icontains="ven") # icontains大小写不敏感
    # models.Tb1.objects.exclude(name__icontains="ven")
    # range
    #
    # models.Tb1.objects.filter(id__range=[1, 2])   # 范围bettwen and
    # 其他类似
    #
    # startswith，istartswith, endswith, iendswith,
    # order by
    #
    # models.Tb1.objects.filter(name='seven').order_by('id')    # asc
    # models.Tb1.objects.filter(name='seven').order_by('-id')   # desc
    # limit 、offset
    #
    # models.Tb1.objects.all()[10:20]
    # group by
    from django.db.models import Count, Min, Max, Sum
    # models.Tb1.objects.filter(c1=1).values('id').annotate(c=Count('num'))
    # SELECT "app01_tb1"."id", COUNT("app01_tb1"."num") AS "c" FROM "app01_tb1" WHERE "app01_tb1"."c1" = 1 GROUP BY "app01_tb1"."id"
```
