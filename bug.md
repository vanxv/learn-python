##msysqlbug
```shell
export PATH=${PATH}:/usr/local/mysql/bin
```

##不能连接
```shell
(28000): Access denied for user 'VANXV'@'localhost' (using password:
sudo mysqld_safe --skip-grant-tables &
```

####Pycharm 断点失败的解决办法

1.参考下面的设置

2.重启一下。（对Pycharm很坑）

设置: ![gras](img/pyCharmbug.png)


####python request.POST.has_key 没有值bug
不知什么原因request.POST.has_key找不到值了。
现在找的办法
        if 'post1' in request.POST:
        在POST中找是否包含post1，如果包含则执行。
*views.py:**
```python 

def index(request):
    if request.method == 'POST':# 当提交表单时
        if 'post1' in request.POST:
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

**index.html**
```html
<form method='post'>
{% csrf_token %}
{{ form }}
<input type="submit" name="post1"value="+">
<input type="submit" name="post2"value="-">
</form>

```

**urls.py**
```python
def index(request):
    if request.method == 'POST':# 当提交表单时
        if 'post1' in request.POST:
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


BUG:
OperationalError at /weather/
(1366, "Incorrect string value: '\\xE7\\x94\\xB5\\xE5\\xBD\\xB1' for column 'name' at row 1")

数据库没有设置成utf-8不支持中文。
用mysql软件设置字段成为utf-8就可以
