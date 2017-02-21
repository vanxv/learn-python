  #写你第一个Django app part3

现在的任务是：
  博客的页面：显示最新的几项
  输入“细节”页面 -永久链接页面为单个条目
  存档页面-显示事件
  基于月份的归档页面 - 显示指定月份中所有条目的天数。
  基于天的归档页面 - 显示给定日期的所有条目。
  注释操作 - 处理将注释发布到给定条目。

  在我们的投票应用程序中，我们将有以下四个视图：
  问题“索引”页 - 显示最近的几个问题。
  问题“详细信息”页面 - 显示问题文本，没有结果，但带有要投票的表格。
  问题“结果”页 - 显示特定问题的结果。
  投票行动 - 处理特定问题中的特定选择的投票。
  在Django中，网页和其他内容是通过视图传递的。 每个视图由一个简单的Python函数（或基于类的视图的方法）表示。 Django将通过检查所请求的URL（即域名后的URL部分）来选择一个视图。

**polls/views.py**
修改成：
```python
from django.shortcuts import render
from django.http import HttpResponse
def index(request):
    return HttpResponse("Hello, world, you're at the polls index.")
def detail(request, question_id):
    return HttpResponse("You're looking at question %s." % question_id)

def results(request, question_id):
    response = "You're looking at the results of question %s."
    return HttpResponse(response % question_id)

def vote(request, question_id):
    return HttpResponse("You're voting on question %s." % question_id)
```

修改polls/urls.py
```python
from django.conf.urls import url
from . import views
urlpatterns = [
    url(r'^$', views.index, name='index'),
    url(r'^(?Ppoll<question_id>[0-9]+)/$', views.detail, name='detail'),
    # ex: /polls/5/results/
    url(r'^(?P<question_id>[0-9]+)/results/$', views.results, name='results'),
    # ex: /polls/5/vote/
    url(r'^(?P<question_id>[0-9]+)/vote/$', views.vote, name='vote'),
]
```
现在可以访问：
http://127.0.0.1:8000/polls/34/results/

http://127.0.0.1:8000/polls/34/vote/

设置views这个

**polls/views.py**
跟我一起打代码：
```python
from django.http import HttpResponse
from .models import Question
def index(request):
    latest_question_list = Question.objects.order_by('-pub_date')[:5]
    output = ', '.join([q.question_text for q in latest_question_list])
    return HttpResponse(output)
```

views是数据库和html的中间件
然后和html在这里读取值和数据库对接

下面生成html代码和数据库连接
新建html文件： **polls/templates/polls/index.html**
```html
{% if latest_question_list %}
  <ul>
    {% for question in latest_question_list %}
      <li><a href="/polls/{{ question.id }}/" >{{ question.question_text }}</a></li>
    {% endfor %}
  </ul>
{% else %}
  <p>No polls are available.</p>
{% endif %}
```
修改
**polls/views.py**
```python
from django.http import HttpResponse
from django.template import loader

from .models import Question


def index(request):
    latest_question_list = Question.objects.order_by('-pub_date')[:5]
    template = loader.get_template('polls/index.html')
    context = {
        'latest_question_list': latest_question_list,
    }
    return HttpResponse(template.render(context, request))
```

原理：
从urls.py中获取views.py的数据访问路径：def index
再views.py中获取def index
latest_question_list = Question.objects.order_by('-pub_date')[:5]
读取数据库Question按照objects排序拿到5个
template = loader.get_template('polls/index.html')
加载polls/index.html
context = { 'latest_question_list': latest_question_list, }
文本Question.objects.order_by('-pub_date')[:5]

return HttpResponse(template.render(context, request))
放回网页(template读取(context, request))
意思是加载网页

index.html
```
{% if latest_question_list %}
    <ul>
    {% for question in latest_question_list %}
        <li><a href="/polls/{{ question.id }}/">{{ question.question_text }}</a></li>
    {% endfor %}
    </ul>
{% else %}
    <p>No polls are available.</p>
{% endif %}
```
{% if latest_question_list %}
如果有数据
{% for question in latest_question_list %}
循环 latest_question_list
    <li><a href="/polls/{{ question.id }}/">{{ question.question_text }}</a></li>
变成网站/polls/{{ question.id }}
{{ question.question_text }}</a></li>标题是书库中的text
{% endfor %}
使用循环语句打出结果。
