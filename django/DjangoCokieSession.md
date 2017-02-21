##教材Django cookie Sesstion入门
来了对吧？
先把cookies和sesstion跑用debug跑一次，再说。。
千万记得。别看解释，就看代码。。。。。
千万记得。别看解释，就看代码。。。。。
千万记得。别看解释，就看代码。。。。。

在 **views.py** 中输入
```python
def test_count_session(request):
    if 'count' in request.session:
        request.session['count'] = 'test'
        request.session['b'] = 'googlesession'
        request.session['f'] = 'f'
        request.session['d'] = 'd'
        request.session['e'] = 'e'
        return HttpResponse('new count=%s'   % request.session['b'])
    else:
        request.session['count'] = 1
        return HttpResponse('No count in session. Setting to 1')
```

在 **urls.py** 中输入增加
```python
    url(r'^test/$', views.test_count_session),
```
然后访问用debug访问

解释给你听：
cookies是在客户端(访问的电脑上) 放的一个字段（自动生成的）
session是在服务器端对应cookies存的一个字典

比如cookies是一个身份证，每次来电脑可以领取一个session字典使用。

django.http.HttpRequest，是一个传播的中间件。传播cookie和session用的。
