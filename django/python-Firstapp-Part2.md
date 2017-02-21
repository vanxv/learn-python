#编写第一个app第二步
这个教材是延续part1的，我们安装数据库，创建第一个模型
和一个快速建立Django自动建立admin页面

##数据库安装
现在打开 **mysite/settings.py**
他是一个通用的python模型一起模型等级变量Django设置

默认下，这个配置使用SQLite.如果你要用新的数据库，
或者你要试试Django，比较简单，SQLite是自带在python的
因此你不需要安装任何的，除非你需要支持你的数据库
如果你需要别的，需要修改DATABASES默认设置。
可以使用多数据库

MYSQL django支持5.5以上
在**mysite/settings.py** 中设置

下面是安装好的模块：
```
django.contrib.admin 这个admin页面
django.contrib.auth 这是一个验证系统
django.contrib.contenttypes -框架内容类型
django.contrib.sessions 对话框架
django.contrib.messages 信息框架
django.contrib.staticfiles 管理静态文件
```

这些应用是包默认的。
一些属于这些应用制作使用属于这个最小的一个数据库表格
因此我们需要创建一个表格在这个数据库中。
之前我们能使用他们，运行下面的指令

```python
python3 manage.py migrate
```
migrate指令给默认app设置和必要的数据库表
根据 mysite/settings.py这个文件中的数据库设置
迁移应用程序的数据库
你会看见一个信息来自于迁移的app
如果你感兴趣，可以运行指令行在你的数据库

##建python模型
编辑**polls/models.py**
文件如下

```python
from django.db import models

class Question(models.Model):
    question_text = models.CharField(max_length=200)
    pub_date = models.DateTimeField('date published')
class Choice(models.Model):
    question = models.ForeignKey(Question, on_delete=models.CASCADE)
    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)
```

django自带了自动生成数据库的模型
polls/models.py这个数据库就是为了自建数据库用的。

class Question()
每一个模型是由一个子类的类django.db.models.Model.
class Question(models.Model):
建一个Question的数据表

    question_text = models.CharField(max_length=200)

    字段的名称   =  模型.char场地要求：最大值不能大于200

    pub_date = models.DateTimeField('date published')

    字段的名称   =  模型.DateTime场地 要求：发型时间（自动生成）

    CharField字符字段和DateTimeField日期时间。

class Choice(models.Model):

    question = models.ForeignKey(Question, on_delete=models.CASCADE)

    字段的名称   =  模型.主键 要求：(Question数据表, **on_delete=models.CASCADE不知什么用，也可以删了** )

    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)
Django支持所有常见的数据库关系:多对一,多对多,一对一。

##启动模型
这个小的模型 代码给django 很多信息 一起来，Django是可以做：
·创建一个数据库刚要（创建表格声明）来自于这个app
·创一个python数据库访问API在访问Question和Choice项目
但是第一步我们需要告诉我们的django这个是投票app的安装

Django APP哲学
```
你能创建一个项目，也可以成倍的创建apps
因为他们不用需要捆绑一个制定的Django安装系统
```
告诉djangoPollsConfig设置完成
**mysite/settings.py**
```python
INSTALLED_APPS = [
    'polls.apps.PollsConfig',
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
```

运行下面的指令：

```python
python manage.py makemigrations polls
```

你会看见下面的指令：
```python
Migrations for 'polls':
  polls/migrations/0001_initial.py:
    - Create model Choice
    - Create model Question
    - Add field question to choice
```

运行makemigrations，告诉Django这是你要做一个改变你的模型
（在这个时间，你要创建一个新的事件）
migrations是怎样你的模型，在你的django商店（和你的数据库）
他们是刚好文件在硬盘.你能读取这个migration在你的新模型如果你喜欢。
它是这个**  polls/migrations/0001_initial.py:**
不用担心，你是不能将每次Django使人读他们的文章,
但他们设计为人类可读的如果你想手动调整Django如何改变的事情
他们是一个指令，这个将要运行这个自动迁移在你和管理你的数据库schema。
这是呼叫migrate，和我们将要等待片刻，
第一，
一起去看什么是SQL迁移运行。这个数据库迁移的指令是

```python
python manage.py sqlmigrate polls 0001
```
你可以看到下面的代码
```python
BEGIN;
--
-- Create model Choice
--
CREATE TABLE "polls_choice" ("id" integer NOT NULL PRIMARY KEY AUTOINCREMENT, "choice_text" varchar(200) NOT NULL, "votes" integer NOT NULL);
--
-- Create model Question
--
CREATE TABLE "polls_question" ("id" integer NOT NULL PRIMARY KEY AUTOINCREMENT, "question_text" varchar(200) NOT NULL, "pub_date" datetime NOT NULL);
--
-- Add field question to choice
--
ALTER TABLE "polls_choice" RENAME TO "polls_choice__old";
CREATE TABLE "polls_choice" ("id" integer NOT NULL PRIMARY KEY AUTOINCREMENT, "choice_text" varchar(200) NOT NULL, "votes" integer NOT NULL, "question_id" integer NOT NULL REFERENCES "polls_question" ("id"));
INSERT INTO "polls_choice" ("choice_text", "votes", "id", "question_id") SELECT "choice_text", "votes", "id", NULL FROM "polls_choice__old";
DROP TABLE "polls_choice__old";
CREATE INDEX "polls_choice_7aa0f6ee" ON "polls_choice" ("question_id");
COMMIT;
```


```python
python manage.py migrate
```

下面的代码
```python
Operations to perform:
  Apply all migrations: admin, auth, contenttypes, polls, sessions
Running migrations:
  Applying polls.0001_initial... OK
```

“迁移是非常强大的,让你改变模型随着时间的推移,当你开发项目,而不需要删除你的数据库或表,让新的——专业升级数据库,不丢失数据。
我们将讨论他们更深入地在后面的教程的一部分,
但是现在,记得三步指南制作模型的变化:改变你的模型(models.py)。
python运行管理。py makemigrations创建迁移这些变化python运行管理。
py迁移将这些更改应用到数据库中。的原因,
有单独的命令和应用迁移是由于你提交迁移到版本控制系统和船舶用应用程序;
他们不仅使您的开发变得更容易,也可用其他发展这个项目

##使用API

现在我们用shell 和paly 免费的API Django 给你
到python shell
```python
python manage.py shell
```

输入下面的指令
```python
>import django
>django.setup()
>from polls.models import Question, Choice
>Question.objects.all()
>>> from django.utils import timezone
>>> q = Question(question_text="What's new?", pub_date=timezone.now())
>>> q.save()
>>> q.id
1
>>> q.question_text
"What's new?"
>>> q.pub_date
datetime.datetime(2017, 2, 1, 10, 21, 11, 147457, tzinfo=<UTC>)
>>> q.question_text = "What's up?"
>>> q.save()
>>> Question.objects.all()
<QuerySet [<Question: Question object>]>
```

修改models.py
```python
import datetime
from django.db import models
from django.utils import timezone
class Question(models.Model):
    question_text = models.CharField(max_length=200)
    pub_date = models.DateTimeField('date published')
    def __str__(self):
        return self.question_text

    def was_published_recently(self):
        return self.pub_date >= timezone.now() - datetime.timedelta(days=1)

class Choice(models.Model):
    question = models.ForeignKey(Question, on_delete=models.CASCADE)
    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)
# Create your models here.
···

python3 指令直接用api shell对数据进行操作，和api差不多
可以进行增删改。
```python
python3 manage.py shell
```
然后输入
```python
>>> from polls.models import Question, Choice
>>> Question.objects.all()
<QuerySet [<Question: What's up?>]>
>>> Question.objects.filter(id=1)
<QuerySet [<Question: What's up?>]>
>>> Question.objects.filter(question_text__startswith='What')
<QuerySet [<Question: What's up?>]>
>>> from django.utils import timezone
>>> current_year = timezone.now().year
>>> Question.objects.get(pub_date__year=current_year)
<Question: What's up?>
>>> Question.objects.get(id=2)
Traceback (most recent call last):
  File "<console>", line 1, in <module>
  File "/Library/Frameworks/Python.framework/Versions/3.5/lib/python3.5/site-packages/django/db/models/manager.py", line 85, in manager_method
    return getattr(self.get_queryset(), name)(*args, **kwargs)
  File "/Library/Frameworks/Python.framework/Versions/3.5/lib/python3.5/site-packages/django/db/models/query.py", line 385, in get
    self.model._meta.object_name
polls.models.DoesNotExist: Question matching query does not exist.
>>> Question.objects.get(pk=1)
<Question: What's up?>
>>> q = Question.objects.get(pk=1)
>>> q.was_published_recently()
True
>>> q = Question.objects.get(pk=1)
>>> q.choice_set.all()
<QuerySet []>

>>> q.choice_set.create(choice_text='Not much', votes=0)
<Choice: Choice object>
>>> q.choice_set.create(choice_text='The sky', votes=0)
<Choice: Choice object>
>>> c = q.choice_set.create(choice_text='Just hacking again', votes=0)
>>> c.question
<Question: What's up?>
>>> q.choice_set.all()
<QuerySet [<Choice: Choice object>, <Choice: Choice object>, <Choice: Choice object>]>
>>> q.choice_set.count()
3
>>> Choice.objects.filter(question__pub_date__year=current_year)
<QuerySet [<Choice: Choice object>, <Choice: Choice object>, <Choice: Choice object>]>

>>> c = q.choice_set.filter(choice_text__startswith='Just hacking')
>>> c.delete()
(1, {'polls.Choice': 1})
```

创建一个admin账户
```python
>python3 manage.py createsuperuser
```
运行服务
```python
python3 manage.py runserver
```
登陆http://127.0.0.1:8000/admin/

打开django的时候里面没有polls的数据库
我们需要修改polls/admin.py中添加数据库字段
**polls/admin.py**
```python
from django.contrib import admin
from .models import Question
admin.site.register(Question)
# Register your models here.
```
这个数据库模型很神奇。可以自动关联数据库后台

然后刷新一下就会出现Questions这个数据库
点击What's up?这个数据
然后点 Today 和 Now 保存数据后，点击history
可以看见历史的修改记录。

完成第二步了。下面是第三步了。
