---
title:  "[WesternCTF2018]shrine"
date: 2020-05-12 21:12:56
tags: Web	
---

```python
import flask
import os
app = flask.Flask(__name__)
app.config['FLAG'] = os.environ.pop('FLAG')
@app.route('/')
def index():
    return open(__file__).read()
@app.route('/shrine/<path:shrine>')
def shrine(shrine):
def safe_jinja(s):
        s = s.replace('(', '').replace(')', '')
        blacklist = ['config', 'self']
        return ''.join(['{{% set {}=None%}}'.format(c) for c in blacklist]) + s
return flask.render_template_string(safe_jinja(shrine))
if __name__ == '__main__':
    app.run(debug=True)
```

题目出现这个

**flask引擎-jinja2模板注入**

利用tplmap这个工具进行检测是否有模板注入漏洞

```
python tplmap.py -u 
```

![](1.png)

> Automatic Server-Side Template Injection Detection and Expleion tool [checks] Tested parameters appear to be not injectable
> 翻译：
> 服务器端模板自动注入检测和删除离子工具！][检查经过测试的参数似乎无法注入  

**模板渲染接受的参数需要用两个大括号括起来{ { } }**

模板注入也在大括号里构造

![](2.png)

在shrine路径下 ssti注入能运行

回头看下源码

```
app.config['FLAG'] = os.environ.pop('FLAG')
```

注册了一个名为FLAG的config，猜测这就是flag，

**如果没有过滤可以直接{{config}}即可查看所有app.config内容**

推测{{config}}可查看所有app.config内容，但是这题设了黑名单[‘config’,‘self’]并且过滤了括号

不过python还有一些内置函数，比如url_for和get_flashed_messages

> /shrine/{{url_for.__globals__}}

![](3.png)

current_app-当前app，查看当下app中的config

![](4.png)

得到flag

知识点：

**SSTI模板注入：**

![](5.png)

模板注入涉及的是服务端Web应用使用模板引擎渲染用户请求的过程

服务端把用户输入的内容渲染成模板就可能造成SSTI(Server-Side Template Injection)

引擎有不同的测试以及注入方式！

> **flask/jinja2模板注入**
>
> **PHP/模版引擎Twig注入**