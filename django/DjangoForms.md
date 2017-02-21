教材Django forms.py入门
    1.理解forms表单的作用
    2.做一个简单的forms案例

主要的事说三遍：
debug!debug!!debug!!!(看清楚每一步)
写教材交给6个月以前sb的自己，因为6个月以后自己忘记了还有地方抄。
看跟着下面的代码打一次，简单的跑一次流程：

环境 Django python3.6
生成APP，weather
```python
django-admin startproject weatherAPI
cd weatherAPI
python3 manage.py startapp weather
```

在weather中增加forms.py forms这个产生表单
在weather 目录下面生成 forms.py
```python
from django import forms

class AddForm(forms.Form):
    a = forms.IntegerField()
```

设置urls.py路由器
```python
from django.conf.urls import url
from . import views

urlpatterns = [
    url(r'^test/', views.test),
    url(r'^$', views.index, name='index'),
]
```
下面是解释：
from django.conf.urls import url **页面路由的功能**
from . import views **载入views.py这个页面**

url(r'^test/', views.test),**访问test页面，跳转到views.test页面**
url(r'^$', views.index, name='index'),
    url(r'^$'**打开默认页面**
    views.index **访问views下面的index函数**
    name='index'不知道什么意思


生成 **/templates/index.html** 页面 并贴入下面的代码
```html
<form method='post'>
{% csrf_token %}
{{ form }}
<input type="submit" name="post1" value="+">
<input type="submit" name="post2" value="-">
</form>
```

**<form method='post'>**
    form这个表单是post的功能。
        POST：向服务器发送数据指令，让他修改数据，或者处理数据
**{% csrf_token %}** 据说是为了防止跨站脚本攻击用的，
**{{ form }}** ：
        **views.py** 通过
            form = AddForm() ## 等于载入2张forms的表单a和b
            return render(request, 'index.html', {'form': form})
                #赋值给index.html，html读取form表单。

<input type="submit" value="提交">
当点提交的时候，出发form 向def views传forms的，值
然后在def index(request):中处理数据
整个过程用测试跑一次看看数据存储过程。


设置 **views.py** index默认访问页面的结果
```python
from .forms import AddForm  #载入forms.s 下面的AddForm模块
def index(request):
    if request.method == 'POST':# 当提交表单时
        if 'post1' in request.POST: #
#        if request.POST.has_key('post1'):
            form = AddForm(request.POST) # form 包含提交的数据

            if form.is_valid():# 如果提交的数据合法
                a = form.cleaned_data['a']
                b = form.cleaned_data['b']
                return HttpResponse(str(int(a) + int(b)))
        else:
            form = AddForm(request.POST) # form 包含提交的数据

            if form.is_valid():# 如果提交的数据合法
                a = form.cleaned_data['a']
                b = form.cleaned_data['b']
                return HttpResponse(str(int(a) - int(b)))
    else:# 当正常访问时
        form = AddForm()
    return render(request, 'index.html', {'form': form})
```


——————伟大的分割线——————

知乎上的get/post例子

在template中生成 **template/weather/test1.html** 
```html
<h1>确定打开吗？</h1> 
<form action="/weather/test1/" method="get">  #打开页面/weather/test1/以 get的模式
  <button name="subject" type="submit" value="Submit">确定</button> #打开页面按钮
</form>
```
如果出错了，看看代码。有时候html放的地方不一样

在template中生成  **templates/weather/firstpage.html**
```html
{{head}}
```

编写 **weather/urls.py**
```python
urlpatterns =[
    url(r'^$',views.test1,name='test1'), # app001首页显示“确定”按钮
    url(r'^/test1/',views.index1,name='index1'), # 匹配点击后传递回来的url
]
```

**views.py**
```pyhton
def test1(request):
  return render(request,'app001/test1.html')

def index1(request):
    ans={}
    ans['head']='hello world'
    return render(request,'app001/firstpage.html',ans)
```

运行
```python
python manage.py runserver
```
打开127.0.0.1:8000/weather 看看是否是按钮跳转页面

下面GET传值运用：
修改 **templates/weather/test1.html**
```html
<form action="/weather/test1/" method="get">
  <p>姓名：<input type="text" name="name1"></p>
  <input type="submit" value="确定">
</form>
```

input type="text"文字 name="name1"
type="submit" value="确定" 按钮。
然后form

修改 **views.py**
```python
def index1(request):
    name=request.GET.get('name1') # 获取参数值
    ans={}
    ans['head']='hello,'+name # 运算处理
    return render(request,'weather/firstpage.html',ans)
```

然后会出现把test1.html中的数据传送给 views.py中

———————————伟大的POST会传数据值———————————————————

1.修改 **‘templates/weather/test1.html’**
```python
<form action="/weather/" method="post">
 {% csrf_token %}
  <p>姓名：<input type="text" name="name1"></p>
  <input type="submit" value="确定">
</form>
<p>{{head}}<p>
```

 {% csrf_token %} 是跨站伪装保护请求。

2.修改 **urls.py**
```python
urlpatterns =[
    url(r'^$',views.index1, name='index1'),
]
```
修改登录的urls指向views.py的index1
制定框架的数据表类型。

3新建**forms.py**
```python
from django import forms

class nameForm(forms.Form):
    name1=forms.CharField() #检查提交的name1值是否为字符串
```
设定forms的框架
4.修改**views.py**
```python
from django.shortcuts import render
from .forms import nameForm
from django.views.decorators.csrf import csrf_protect

    
@csrf_protect   
def index1(request):
    if request.method == 'POST': #是否为post请求
        form=nameForm(request.POST)
        if form.is_valid(): #检查输入是否规范
            ans={}
            name=form.cleaned_data['name1']
            ans['head']=name  
            return render(request,'weather/test1.html',ans) 
    else:
         return render(request,'weather/test1.html')
```

        form=nameForm(request.POST) 把传值回来发给forms测一下
        然后清洗post数据，返回给test1.html
返回数据址到test1.html中
————————————————————————————————————————————————
多表单：
可以修改 <method="post"> 的名字建多表单的任务
```html
<form action="/weather/" method="POST">
 {% csrf_token %}
  <p>姓名：<input type="text" name="name1"></p>
  <input type="submit" value="确定">
</form>
<p>{{head}}<p>
<form action="/weather/" method="POST1">
 {% csrf_token %}
  <p>姓名：<input type="text" name="name1"></p>
  <input type="submit" value="确定">
</form>
<p>{{head}}<p>
```
然后在网站里面做判断，再把值传给服务器。