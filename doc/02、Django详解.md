# 1、项目文件详解

## 1.1 项目结构划分

```python
$ tree
.
├── db.sqlite3          # 默认的数据库
├── demoDjango
│   ├── asgi.py         # 异步通信服务启动文件
│   ├── __init__.py     # 初始化文件
│   ├── __pycache__     # 字节码缓存文件
│   │   ├── __init__.cpython-38.pyc
│   │   ├── settings.cpython-38.pyc
│   │   ├── urls.cpython-38.pyc
│   │   └── wsgi.cpython-38.pyc
│   ├── settings.py     # Django配置文件
│   ├── urls.py         # 路由文件
│   └── wsgi.py         # 服务启动文件
└── manage.py           # 管理文件

```

## 1.2 配置文件详解

```python
# demoDjango/demoDjango/settings.py
import os
from pathlib import Path

# BASE_DIR
BASE_DIR = Path(__file__).resolve().parent.parent

# 安全警告：对生产中使用的密钥保密！
SECRET_KEY = 'django-insecure-yj3kn1-(4*j@e=-9ww=^l0y=yw-ltwsz-@-f0#0r7y(3suy^f5'

# 安全警告：不要在生产中打开调试运行！
DEBUG = True

# 主机白名单：设置为'*'允许所有
ALLOWED_HOSTS = []

# 应用定义

# 应用注册
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]

# 中间件
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]

# 主路由配置文件
ROOT_URLCONF = 'demoDjango.urls'

# 模版
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [os.path.join(BASE_DIR, 'templates')]
        ,
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]

WSGI_APPLICATION = 'demoDjango.wsgi.application'

# Database
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}

# 密码验证
AUTH_PASSWORD_VALIDATORS = [
    {
        'NAME': 'django.contrib.auth.password_validation.UserAttributeSimilarityValidator',
    },
    {
        'NAME': 'django.contrib.auth.password_validation.MinimumLengthValidator',
    },
    {
        'NAME': 'django.contrib.auth.password_validation.CommonPasswordValidator',
    },
    {
        'NAME': 'django.contrib.auth.password_validation.NumericPasswordValidator',
    },
]

# 语言：默认英文、中文（zh-hans）
LANGUAGE_CODE = 'en-us'

# 时区：默认标准时区、上海（Asia/Shanghai）
TIME_ZONE = 'UTC'

USE_I18N = True

USE_L10N = True

# 是否使用系统默认时区
USE_TZ = True

# 静态网址
STATIC_URL = '/static/'

# 默认主键字段类型
DEFAULT_AUTO_FIELD = 'django.db.models.BigAutoField'
```

## 1.3 静态文件详解

__什么是静态文件？__ 

静态文件通常是指不是由服务器生成的文件，例如脚本，CSS 文件，图像等，但是必须在请求时发送给浏览器。

__静态文件的特点：__ 

一般像文本或图片，且它们不会通过后台的控制来更改的元素。

__配置：__ 

1.  在根文件下，新建 static 文件夹（一般在子应用下）

    和 manage.py 文件、templates 文件同级

2.  指定静态资源路径

    ```python
    # demoDjango/demoDjango/settings.py
    STATIC_URL = '/static/'
    # 指定静态资源路径
    STATICFILES_DIRS = [
        os.path.join(BASE_DIR, 'static')
    ]
    ```

3.  上传文件、访问测试

    http://127.0.0.1:8000/static/yannic.png



# 2、项目子应用

Django 中，一个子应用对应前端的一个功能板块，以此类推；不同的子应用实现不同的功能，而不是将所有功能放在一起实现，这样也有利于我们对项目代码进行管理，同时如果其它模块有类似功能的话，也增强了代码的复用性。

__创建：__ 

```bash
python manage.py startapp home  # 会创建一个home的文件夹，和manage.py文件同级
```

__注册：__ 

```python
# demoDjango/demoDjango/settings.py
......
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    # 注册home子应用
    # 'home',
    'home.apps.HomeConfig',
]
......
```

访问测试：http://127.0.0.1:8000/static/yannic.png

## 2.1 子应用的静态文件

一般我们最常使用子应用来包含静态文件。

步骤:【新建子应用】—【新建 static 文件夹】—【新建子应用同名文件夹】—【静态文件】

```bash
# /Users/zhang/PycharmProjects/demoDjango/home
[root@zhangyy home]# tree
.
├── static
│   └── home
│       └── yannic.jpeg

```

访问测试：<http://127.0.0.1:8000/static/home/yannic.png>



# 3、URL详解

URL 即统一资源定位符（UniformResource Locator），在互联网上用来表示某个资源的网络地址，即我们平常所理解的网址。

URL 的一般语法格式：`protocol://domain[:port]/path[?query][#fragment]`

*   protocol（协议）

    `http://` or `https://` or `file:///`

*   domain（域名）

    <https://www.djangoproject.com/>

*   prot（端口号）

    <http://127.0.0.1:8000/>

*   path（路由地址）

    <https://docs.djangoproject.com/zh-hans/3.2/>

*   query（查询）

    <https://docs.djangoproject.com/zh-hans/3.2/search/?q=view>

*   fragment（信息片段）

    <https://docs.djangoproject.com/zh-hans/3.2/#first-steps>



# 4、路由&视图

## 4.1 路由

### 4.1.1 路由解析流程

1.  客户端发起请求

    `http://www.***.com/1/100/?a=10`

2.  服务端接收请求，解析出路由地址（`1/100/`）

    先匹配项目目录下的 url，在匹配具体应用中的 url。

    *   匹配成功

        执行相应的视图

    *   匹配失败

        返回 404，未找到页面

3.  客户端接收响应并渲染执行

```python
# demoDjango/demoDjango/settings.py
ROOT_URLCONF = 'demoDjango.urls'
```

### 4.1.2 路由的使用

创建项目，默认会为我们创建一个 /project\_name/urls.py 文件，并且自动包含下面内容，这就为项目的主 URL：

```python
"""demoDjango URL Configuration

The `urlpatterns` list routes URLs to views. For more information please see:
    https://docs.djangoproject.com/en/3.2/topics/http/urls/
Examples:
Function views
    1. Add an import:  from my_app import views
    2. Add a URL to urlpatterns:  path('', views.home, name='home')
Class-based views
    1. Add an import:  from other_app.views import Home
    2. Add a URL to urlpatterns:  path('', Home.as_view(), name='home')
Including another URLconf
    1. Import the include() function: from django.urls import include, path
    2. Add a URL to urlpatterns:  path('blog/', include('blog.urls'))
"""
from django.contrib import admin
from django.urls import path

# 列表数据(path or re_path的实例)
urlpatterns = [
    # 绑定路由到视图函数
    path('admin/', admin.site.urls),
]
```

__admin 管理员后台：__ 

1.  `python manage.py migrate`，执行迁移

2.  `python manage.py createsuperuser`，创建管理账号

3.  http://127.0.0.1:8000/admin/，访问测试

### 4.1.3 路由匹配规则

1.  urlpatterns 列表中从上到下进行匹配

2.  如果匹配不成功，那么会往下匹配

3.  如果当前路由匹配成功，会自动调用 path 函数第二个参数的函数（即视图）

4.  一旦路由匹配成功，那么不会继续往下匹配

5.  如果所有的路由都匹配不成功，会抛出 404 异常（返回 404 页面）

## 4.2 视图

函数视图，简称视图，就是一个简单的 Python 函数，它接受 Web 请求并且返回 Web 响应。响应的内容可以是一个 HTML 页面、一个 404 错误页面、重定向页面、XML 文档、或者是一张图片。

注意：无论视图本身是什么逻辑，一定要返回响应。每个视图函数都要返回一个 HttpResponse 对象，对象中就包含了生成的响应。

视图中最重要的两个对象：请求对象（HttpRequest）和响应对象（HttpResponse）。

__注意：__ 默认没有视图文件，需要手动去创建 views.py 文件。

## 4.3 实例

### 4.3.1 简单实例

1.  路由

    ```python
    # demoDjango/demoDjango/urls.py

    from django.contrib import admin
    from django.urls import path, re_path, include
    from demoDjango import views

    # 列表数据(path or re_path的实例)
    urlpatterns = [
        path('admin/', admin.site.urls),
        path('index/', views.index_views),    # 绑定路由到视图函数
    ]
    ```

2.  视图

    ```python
    # demoDjango/demoDjango/views.py
    from django.http import HttpResponse

    def index_views(request):
        html = "<h1>Hello World</h1>"

        return HttpResponse(html)
    ```

3.  浏览器访问测试：http://127.0.0.1:8000/index/


### 4.3.2 传递变量

1.  路由

    ```python
    # demoDjango/demoDjango/urls.py
    from django.contrib import admin
    from django.urls import path, re_path, include
    from demoDjango import views

    # 列表数据(path or re_path的实例)
    urlpatterns = [
        # 绑定路由到视图函数
        # path('admin/', admin.site.urls),
        # path('index/', views.index_views),
        path('args/<int:num>/', views.args_views),
    ]
    ```

2.  视图

    ```python
    # demoDjango/demoDjango/views.py
    from django.http import HttpResponse
    
    # def index_views(request):
    #     html = "<h1>Hello World</h1>"
    #     return HttpResponse(html)
    
    def args_views(request, num):
        html = f"<h1>商品：{num}</h1>"
        return HttpResponse(html)
    ```
    
3.  访问测试：http://127.0.0.1:8000/args/32/


### 4.3.3 正则匹配路由

可以通过 re\_path 函数实现，正则匹配路由到视图。

__实例：__ 

```bash
# demoDjango/demoDjango/urls.py
from django.contrib import admin
from django.urls import path, re_path
from demoDjango import views

urlpatterns = [
    path('admin/', admin.site.urls),

    re_path(r'^repath/[0-9]{3}/$', views.re_views),
]
```

```bash
# demoDjango/demoDjango/views.py
from django.http import HttpResponse

def re_views(request):
    html = '<h1>这是路由匹配的视图</h1>'
    return HttpResponse(html)
```

访问测试：http://127.0.0.1:8000/repath/111/

## 4.4 子路由

__为什么要使用子路由？__ 

如果所有的路由都放到主路由中，那主路由就会十分臃肿，不便于维护和管理。所以我们在子应用中创建子路由，在主路由中匹配 url 的第一部分，在子路由中匹配后面的部分。

__创建：__ 在子应用中创建一个 urls.py 的文件，这就是子路由。

## 4.5 路由转发

通过将主应用的路由转发至子应用中的路由。

__语法：__ `path('', include('home.urls')),`

__实例：__ 

```python
# demoDjango/demoDjango/urls.py
from django.contrib import admin
from django.urls import path, include, re_path
from demoDjango import views

urlpatterns = [
    path('admin/', admin.site.urls),

    # 主页视图: include路由转发
    path('', include('home.urls')),
]
```

```python
# demoDjango/home/urls.py
from django.urls import path
from home import views

urlpatterns = [
    # 绑定路由到视图
    path('', views.homepage)
]
```

```python
# demoDjango/home/views.py
from django.http import HttpResponse
from django.shortcuts import render

# Create your views here.

def homepage(request):
    html = "<h1>home page</h1>"
    return HttpResponse(html)
```

访问测试：http://127.0.0.1:8000/

## 4.6 路由命名

__目的：__ 在定义路由的时候，可以为路由命名，方便查找特定视图的具体路径信息。

*   普通命名

    用 name 参数指明路由名字。

    语法：`path('', views.master_home_views, name='home'),`

*   命名空间

    使用 include 函数定义路由时，可以使用 namespace 参数定义路由的命名空间。

    语法：`path('', include(('home.urls', 'home'), namespace='home')),`

__作用：__ 路由命名，避免了不同的子应用中路由使用相同的名字发生冲突，可以使用命名空间区别开。

__实例：__ 

```python
# demoDjango/demoDjango/urls.py
from xml.etree.ElementInclude import include
from django.contrib import admin
from django.urls import path, re_path, include
from demoDjango import views

urlpatterns = [
    path('admin/', admin.site.urls),

    # home子路由
    path('', include(('home.urls', 'home'))),  # namespace: home
]
```

```python
# demoDjango/home/urls.py
from django.urls import path
from home import views

urlpatterns = [
    path('', views.homepage, name='homepage')  # name: homepage
]
```

## 4.7 逆向解析

通过路由名称返回具体的访问路径的过程，称为路由逆向解析（reverse）。

__使用：__ 

*   from django.urls import reverse

*   对于没有指明命名空间的，格式为：`reverse(路由名)`

*   对于指明了命名空间的，格式为：`reverse(命名空间:路由名)`

__实例：__ 

```python
# demoDjango/demoDjango/urls.py
from xml.etree.ElementInclude import include
from django.contrib import admin
from django.urls import path, re_path, include
from demoDjango import views

urlpatterns = [
    path('admin/', admin.site.urls),

    # home子路由
    path('', include(('home.urls', 'home'))),  # namespace: home
]
```

```python
# demoDjango/home/urls.py
from django.urls import path
from home import views

urlpatterns = [
    path('', views.homepage, name='homepage'),
    path('reverse/', views.reverse_homepage)
]

```

```python
# demoDjango/home/views.py
from django.http import HttpResponse
from django.shortcuts import render
from django.urls import reverse

# Create your views here.

def homepage(request):
    html = "<h1>home page</h1>"
    return HttpResponse(html)

def reverse_homepage(request):
    path = reverse('home:homepage')
    return HttpResponse(path)

```

访问测试：http://127.0.0.1:8000/reverse/

## 4.8 重定向

重定向就是由一个 URL 跳转到另一个 URL。

__语法：__ `return redirect(path)`

__实例：__ 

```python
# demoDjango/demoDjango/urls.py
from xml.etree.ElementInclude import include
from django.contrib import admin
from django.urls import path, re_path, include
from demoDjango import views

urlpatterns = [
    path('admin/', admin.site.urls),

    # home子路由
    path('', include(('home.urls', 'home'))),  # namespace: home
]
```

```python
# demoDjango/home/urls.py
from django.urls import path
from home import views

urlpatterns = [
    # 主页
    path('', views.homepage, name='homepage'),
    # 返向解析
    path('reverse/', views.reverse_homepage),
    # 重定向
    path('new_homepage/', views.new_homepage, name='newhomepage'),
    path('redirect/', views.homepage_redirect)
]

```

```python
# demoDjango/home/views.py
from django import http
from django.http import HttpResponse
from django.shortcuts import redirect, render
from django.urls import reverse

# Create your views here.

# 主页
def homepage(request):
    html = "<h1>home page</h1>"
    return HttpResponse(html)

# 返向解析
def reverse_homepage(request):
    path = reverse('home:homepage')
    return HttpResponse(path)

# 重定向
def new_homepage(request):
    html = "<h1>new home page</h1>"
    return HttpResponse(html)

def homepage_redirect(request):
    path = reverse('home:newhomepage')
    return redirect(path)
```

访问测试：http://127.0.0.1:8000/redirect/



# 5、请求与响应

## 5.1 HttpRequest 对象

服务器接收 http 请求后，会根据报文创建 HttpRequest 对象作为视图的第一个参数，第一个参数必须是 HttpRequest 对象。

__django 请求处理流程：__ 

1.  确定 URLconf 的根配置位置，通常是在 ROOT\_URLCONF 中设置；

2.  加载配置信息，在配置信息中查找 urlpatterns；

3.  按顺序查找 urlpatterns 中的所有 URL 模式字符串，并匹配；

4.  当匹配到 URL 模式字符串后，调用对应的视图，并传递以下参数：

    *   一个 HttpRequest 对象实例；

    *   如果匹配的 URL 模式字符串包含路径参数，那么则会作为位置参数传递给视图；

    *   路径表达式中的命名部分组成了视图的关键字参数，以上关键字参数会被 django.url.path() 或 django.url.re\_path() kwargs 覆盖。

5.  如果在 URLconf 中没有找到任何匹配的模式字符串，或者出现其他任何错误，Django 将会调用一个用于处理错误信息的视图。

__常用属性：__ 

*   path：请求的完整路径（不包括域名和端口）

*   method：表示请求的方式，常用的有 GET, POST

*   encoding：表示浏览器提交的数据的编码方式，一般为 utf-8

*   GET：类似于字典的对象，包含了 get 请求的所有参数（查询字符串）

*   POST：类似于字典的对象，包含了 post 请求的所有参数

*   FILES：类似字典的对象，包含了所有上传的文件

*   COOKIES：字典，包含所有的 cookie

*   session：类似字典的对象，表示当前会话方法

*   is\_ajax()：如果是通过 XMLHttpRequest 发起的，返回 True

*   body：字符串，请求体的内容（POST 或 PUT）

__实例：__ 

```python
# demoDjango/demoDjango/urls.py
from xml.etree.ElementInclude import include
from django.contrib import admin
from django.urls import path, re_path, include
from demoDjango import views

urlpatterns = [
    path('admin/', admin.site.urls),

    # home子路由
    path('', include(('home.urls', 'home'))),  # namespace: home
]
```

```python
# demoDjango/home/urls.py
from django.urls import path
from home import views

urlpatterns = [
    # 绑定路由到视图
    path('index/', views.index_views, name='index'),
]

```

```python
# demoDjango/home/views.py

from django.http import HttpResponse
from django.shortcuts import render, redirect

# Create your views here.

# index
def index_views(request):
    print(f"request path: {request.path}")
    print(f"request method: {request.method}")
    print(f"request encoding: {request.encoding}")
    return HttpResponse('请求成功')

```

访问测试：http://127.0.0.1:8000/index/

## 5.2 查询字符串

__什么是查询字符串？__ 

又称为 URL 参数，指在 URL 末尾加上用于向服务器发送信息的字符串（变量）。用 "?" 连接，后面紧接 "key=value"，如果想添加多个参数的话，使用 "&" 连接多个参数。以这样的形式，将想要发送给服务器的数据添加到 URL 中。

例如：[https://www.baidu.com/?name=yannic\&sex=man\&age=22](https://www.baidu.com/?name=yannic\&sex=man\&age=22 "https://www.baidu.com/?name=yannic\&sex=man\&age=22")

注意："?" 只能使用一次。

__参数：__ 

*   被动参数

    被动参数对显示的内容没有影响。无论是否添加参数，显示的页面都是相同的。

    为了进行 Web 网站的访问分析。了解用户是从什么地方到达了自己的网站，设定固有的参数去统计。

*   活动参数

    不同于被动参数，活动参数会影响显示的内容，也就是在添加参数之后，网站上显示的内容也会发生改变。

__实例：__ 

```python
# demoDjango/demoDjango/urls.py
from xml.etree.ElementInclude import include
from django.contrib import admin
from django.urls import path, re_path, include
from demoDjango import views

urlpatterns = [
    path('admin/', admin.site.urls),

    # home子路由
    path('', include(('home.urls', 'home'))),  # namespace: home
]
```

```python
# demoDjango/home/urls.py

from django.urls import path
from home import views

urlpatterns = [
    # 绑定路由到视图
    path('query/', views.query_views, name='query'),
]

```

```python
# demoDjango/home/views.py

from django.http import HttpResponse
from django.shortcuts import render, redirect

# Create your views here.

# 查询字符串
def query_views(request):
    if request.method == "GET":
        print(f"all args: {request.GET}")              # all args: <QueryDict: {'name': ['yannic', 'zhang'], 'age': ['18']}>
        print(f"name: {request.GET['name']}")          # name: zhang
        print(f"name: {request.GET.get('name')}")      # name: zhang
        print(f"name: {request.GET.getlist('name')}")  # name: ['yannic', 'zhang']
    return HttpResponse("查询字符串成功")

```

访问测试：http://127.0.0.1:8000/query/?name=yannic&age=18&name=zhang

## 5.3 表单数据接收

前端发送的 Form Data 表单类型的请求体数据，可以通过 request.POST 属性获取，返回 QueryDict 对象。

__实例：__ 

```python
# demoDjango/demoDjango/urls.py
from xml.etree.ElementInclude import include
from django.contrib import admin
from django.urls import path, re_path, include
from demoDjango import views

urlpatterns = [
    path('admin/', admin.site.urls),

    # home子路由
    path('', include(('home.urls', 'home'))),  # namespace: home
]
```

```python
# demoDjango/home/urls.py

from django.urls import path
from home import views

urlpatterns = [
    # 绑定路由到视图
    path('query/', views.query_views, name='query'),
]
```

```python
# demoDjango/home/views.py

from django.http import HttpResponse
from django.shortcuts import render, redirect

# Create your views here.

# 查询字符串
def query_views(request):
    if request.method == "GET":
        print(f"all args: {request.GET}")              # all args: <QueryDict: {'name': ['yannic', 'zhang'], 'age': ['18']}>
        print(f"name: {request.GET['name']}")          # name: zhang
        print(f"name: {request.GET.get('name')}")      # name: zhang
        print(f"name: {request.GET.getlist('name')}")  # name: ['yannic', 'zhang']
    elif request.method == "POST":
        print(f"all args: {request.POST}")             # all args: <QueryDict: {'name': ['yannic', 'zhang'], 'age': ['18']}>
        print(f"name: {request.POST['name']}")         # name: zhang
        print(f"name: {request.POST.get('name')}")     # name: zhang
        print(f"name: {request.POST.getlist('name')}") # name: ['yannic', 'zhang']
    return HttpResponse("查询字符串成功")
```

访问测试：Postman 发起 POST 请求，Body（from-data）携带参数（name=yannic, age=18, name=zhang）

## 5.4 非表单数据

非表单类型的请求体数据，Django 无法自动解析，可以通过 request.body 属性获取最原始的请求体数据，自己按照请求体格式（JSON、XML 等）进行解析。request.body 返回 bytes 类型。

*   `request.body.decode()`：解码为字符串类型

*   `json.loads(字符串类型)`：将字符串转换为字典类型

*   `json.dumps(字典类型)`：将字典数据 dict 序列化为字符串形式

__实例：__ 

```python
# demoDjango/demoDjango/urls.py
from xml.etree.ElementInclude import include
from django.contrib import admin
from django.urls import path, re_path, include
from demoDjango import views

urlpatterns = [
    path('admin/', admin.site.urls),

    # home子路由
    path('', include(('home.urls', 'home'))),  # namespace: home
]
```

```python
# demoDjango/home/urls.py

from django.urls import path
from home import views

urlpatterns = [
    # 绑定路由到视图
    path('json/', views.json_views, name='json'),
]

```

```python
# demoDjango/home/views.py

from django.http import HttpResponse
from django.shortcuts import render, redirect
import json

# Create your views here.

# 非表单数据(JSON)
def json_views(request):
    bytes_data = request.body
    print(type(bytes_data), bytes_data)

    str_data = bytes_data.decode()
    print(type(str_data), str_data)

    dict_data = json.loads(str_data)
    print(type(dict_data), dict_data)
    
    return HttpResponse("接收json数据完成")

```

访问测试：Postman 发起 POST 请求，Body（row-json）携带 json 数据。

## 5.5 文件类型数据

__实例：__ 

```python
# demoDjango/demoDjango/urls.py
from xml.etree.ElementInclude import include
from django.contrib import admin
from django.urls import path, re_path, include
from demoDjango import views

urlpatterns = [
    path('admin/', admin.site.urls),

    # home子路由
    path('', include(('home.urls', 'home'))),  # namespace: home
]
```

```python
# demoDjango/home/urls.py

from django.urls import path
from home import views

urlpatterns = [
    # 绑定路由到视图
    path('file/', views.file_views, name='file'),
]

```

```python
# demoDjango/home/views.py

from django.http import HttpResponse
from django.shortcuts import render, redirect
import json

# Create your views here.
def file_views(request):
    image = request.FILES.get('image').read()
    print(image)
    with open('yannic.jpeg', 'wb') as f:
        f.write(image)

    return HttpResponse("文件上传成功")

```

访问测试：Postman 发起 POST 请求，Body（form-data）携带 文件。

## 5.6 HttpResponse 对象

视图在接收请求并处理后，必须返回 HttpResponse 对象或子对象。HttpRequest 对象由 Django 创建，HttpResponse 对象由开发人员创建。

当客户端访问一个网页时，浏览器会向网页所在服务器发出请求。当浏览器接收并显示网页前，此网页所在的服务器会返回一个包含 HTTP 状态码的信息头（serverheader）用以响应浏览器的请求。

HTTP 状态码的英文为 HTTP Status Code。常见的HTTP状态码如下：

| 状态码   | 描述                     |
| ----- | ---------------------- |
| 1\*\* | 信息，服务器收到请求，需要请求者继续执行操作 |
| 2\*\* | 成功，操作被成功接收并处理          |
| 3\*\* | 重定向，需要进一步的操作已完成请求      |
| 4\*\* | 客户端错误，请求包括语法错误或无法完成请求  |
| 5\*\* | 服务器错误，服务器在处理请求的过程中发生错误 |

使用 django.http.HttpResponse 来构造响应对象。

__语法：__ `HttpResponse(content=响应体, content_type=响应体数据类型, status=状态码)`

*   content 参数：传递字符串，不要传递对象，字典等数据

*   content\_type 参数：是一个 MIME 类型，语法格式是：大类/小类，例如：

    text/html, text/css, text/javascript, application/json, image/png, image/gif, image/jpeg 等等

*   status 参数：只能使用系统规定的状态码

也可通过 HttpResponse 对象属性来设置响应体、状态码：

*   content：表示返回的内容

*   status\_code：返回的HTTP响应状态码

Django 为我们提供了一系列的 HttpResponse 的子类，可以快速设置响应状态码。

*   HttpResponseRedirect：302

*   HttpResponsePermanentRedirect：301

*   HttpResponseNotModified：304

*   HttpResponseBadRequest：400

*   HttpResponseNotFound：404

*   HttpResponseForbidden：403

*   HttpResponseNotAllowed：405

*   HttpResponseGone：410

*   HttpResponseServerError：500

若要返回 json 数据，可以使用 JsonResponse 来构造响应对象，作用：

*   帮助我们将数据转换为 json 字符串

*   设置响应头 content\_type 为 application/json

__实例：__ 

```python
# demoDjango/demoDjango/urls.py
from xml.etree.ElementInclude import include
from django.contrib import admin
from django.urls import path, re_path, include
from demoDjango import views

urlpatterns = [
    path('admin/', admin.site.urls),

    # home子路由
    path('', include(('home.urls', 'home'))),  # namespace: home
]
```

```python
# demoDjango/home/urls.py

from django.urls import path
from home import views

urlpatterns = [
    # 绑定路由到视图
    path('response/', views.response_views, name='response'),
]

```

```python
# demoDjango/home/views.py

from django.http import HttpResponse, JsonResponse
from django.shortcuts import render, redirect
import json

# Create your views here.
# response
def response_views(request):
    # response = HttpResponse('request success.', content_type='text/html', status=500)
    # print(f"状态码: {response.status_code}")   # 状态码: 500

    response = HttpResponse('request success.', content_type='application/json')
    data = {"name": "yannic", "age": 23}
    return JsonResponse(data)
```

访问测试：http://127.0.0.1:8000/response/



# 6、Cookie and Session

## 6.1 Cookie

HTTP Cookie（也叫 Web Cookie 或浏览器 Cookie）是服务器发送到用户浏览器并保存在本地的一小块数据，它会在浏览器下次向同一服务器再发起请求时被携带并发送到服务器上。通常，它用于告知服务端两个请求是否来自同一浏览器，如保持用户的登录状态。Cookie 使基于无状态的 HTTP 协议记录稳定的状态信息成为了可能。

__Cookie 的特点：__ 

*   Cookie 以键值对的格式进行信息的存储。

*   Cookie 基于域名安全，不同域名的 Cookie 是不能互相访问的，如访问 jd.com 时向浏览器中写了 Cookie 信息，同一浏览器访问 baidu.com 时，无法访问到 jd.com 写的 Cookie 信息。

*   当浏览器请求某网站时，会将浏览器存储的跟网站相关的所有 Cookie 信息提交给网站服务器。

__Cookie 的类型：__ 

*   会话 Cookie

    会话 Cookie 是一种临时 Cookie，用户退出浏览器，会话 Cookie 就会被删除了。

*   持久 Cookie

    持久 Cookie 则会储存在硬盘里，保留时间更长，关闭浏览器，重启电脑，它依然存在，通常是持久性的 cookie 会维护某一个用户周期性访问服务器的配置文件或者登录信息。

__Cookie 主要用于以下三个方面：__ 

*   会话状态管理（如用户登录状态、购物车、游戏分数或其它需要记录的信息）

*   个性化设置（如用户自定义设置、主题等）

*   浏览器行为跟踪（如跟踪分析用户行为等）

### 6.1.1 Set Cookie

__语法：__ `response.set_cookie(self, key, value='', max_age=相对时间, expires=绝对时间)`

__实例：__ 

```python
# demoDjango/demoDjango/urls.py
from xml.etree.ElementInclude import include
from django.contrib import admin
from django.urls import path, re_path, include
from demoDjango import views

urlpatterns = [
    path('admin/', admin.site.urls),

    # home子路由
    path('', include(('home.urls', 'home'))),  # namespace: home
]
```

```python
# demoDjango/home/urls.py

from django.urls import path
from home import views

urlpatterns = [
    # 绑定路由到视图
    path('set_cookie/', views.set_cookie_views, name='set_cookie'),
]

```

```python
# demoDjango/home/views.py

from django.http import HttpResponse

# Create your views here.
def set_cookie_views(request):
    response = HttpResponse('set cookie success.')
    response.set_cookie('name', 'yannic', max_age=600)
    response.set_cookie('age', '23', expires=datetime.datetime.now()+datetime.timedelta(days=14))

    return response
```

访问测试：http://127.0.0.1:8000/set_cookie/

### 6.1.2 Get Cookie

__语法：__ `request.COOKIES(key)`

__实例：__ 

```python
# demoDjango/demoDjango/urls.py
from xml.etree.ElementInclude import include
from django.contrib import admin
from django.urls import path, re_path, include
from demoDjango import views

urlpatterns = [
    path('admin/', admin.site.urls),

    # home子路由
    path('', include(('home.urls', 'home'))),  # namespace: home
]
```

```python
# demoDjango/home/urls.py

from django.urls import path
from home import views

urlpatterns = [
    # 绑定路由到视图
    path('set_cookie/', views.set_cookie_views, name='set_cookie'),
    path('get_cookie/', views.get_cookie_views, name='get_cookie'),
]

```

```python
# demoDjango/home/views.py

from django.http import HttpResponse

def set_cookie_views(request):
    response = HttpResponse('set cookie success.')
    response.set_cookie('name', 'yannic', max_age=600)
    response.set_cookie('age', '23', expires=datetime.datetime.now()+datetime.timedelta(days=14))

    return response


def get_cookie_views(request):
    name = request.COOKIES.get('name')

    return HttpResponse(name)
```

访问测试：

1. http://127.0.0.1:8000/set_cookie/
2. http://127.0.0.1:8000/get_cookie/

### 6.1.3 Del Cookie

```python
# demoDjango/demoDjango/urls.py
from xml.etree.ElementInclude import include
from django.contrib import admin
from django.urls import path, re_path, include
from demoDjango import views

urlpatterns = [
    path('admin/', admin.site.urls),

    # home子路由
    path('', include(('home.urls', 'home'))),  # namespace: home
]
```

```python
# demoDjango/home/urls.py

from django.urls import path
from home import views

urlpatterns = [
    # 绑定路由到视图
    path('set_cookie/', views.set_cookie_views, name='set_cookie'),
    path('get_cookie/', views.get_cookie_views, name='get_cookie'),
    path('del_cookie/', views.del_cookie_views, name='del_cookie'),
]

```

```python
# demoDjango/home/views.py

from django.http import HttpResponse

def set_cookie_views(request):
    response = HttpResponse('set cookie success.')
    response.set_cookie('name', 'yannic', max_age=600)
    response.set_cookie('age', '23', expires=datetime.datetime.now()+datetime.timedelta(days=14))

    return response


def get_cookie_views(request):
    name = request.COOKIES.get('name')

    return name


def del_cookie_views(request):
    response = HttpResponse('delete cookie success.')
    response.delete_cookie('name')

    return response
```

访问测试：

1. http://127.0.0.1:8000/set_cookie/
2. http://127.0.0.1:8000/get_cookie/
3. http://127.0.0.1:8000/del_cookie/

## 6.2 Session

Session 代表着服务器和客户端一次会话的过程。Session 对象存储特定用户会话所需的属性及配置信息。这样，当用户在应用程序的 Web 页之间跳转时，存储在 Session 对象中的变量将不会丢失，而是在整个用户会话中一直存在下去。当客户端关闭会话，或者 Session 超时失效时会话结束。

默认情况下，Django 将会话存储在您的数据库中（使用模型 `django.contrib.sessions.models.Session`）。虽然这很方便，但在某些设置中，将会话数据存储在其他地方会更快，因此可以将 Django 配置为将会话数据存储在您的文件系统或缓存中。

数据库支持的会话需要注册：`'django.contrib.sessions'` 默认已经配置在 `INSTALLED_APPS` 设置中。

配置安装后，运行以安装存储会话数据的单个数据库表 `python manage.py migrate`。

### 6.2.1 配置数据库存储

1.  检查 session 是否注册，默认已经注册

    `'django.contrib.sessions'`

2.  安装 pymysql 库

3.  创建数据库

    `create database demoDjangoData charset='utf8';`

4.  配置数据库参数

    ```python
    # demoDjango/demoDjango/settings.py

    DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.mysql',
            'HOST': '127.0.0.1',
            'POST': '3306',
            'USER': 'root',
            'PASSWORD': 'Zhang890',
            'NAME': 'demoDjangoData',
        }
    }
    ```

5.  在init里驱动mysql

    ```python
    # demoDjango/demoDjango/__init__.py

    import pymysql

    pymysql.install_as_MySQLdb()
    ```

6.  创建表：`python manage.py migrate`

### 6.2.2 配置缓存/混合存储

1.  存储与数据库

    ```python
    DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.mysql',
            'NAME': 'django_forth',
            'HOST': '127.0.0.1',
            'PORT': 3306,
            'USER': 'root',
            'PASSWORD': 'mysql'
        }
    }
    
    # 存储于数据库
    SESSION_ENGINE= 'django.contrib.sessions.backends.db'
    ```
    
2.  存储于缓存中

    ```python
    CACHES = {
        "default": {
            "BACKEND" : "django_redis.cache.RedisCache",
            "LOCATION": "redis://127.0.0.1:6379/0",
            "OPTIONS" : {
                "CLIENT_CLASS": "django_redis.client.DefaultClient",
            }
        }
    }
    
    # 存储于非关系型数据库
    SESSION_ENGINE= 'django.contrib.sessions.backends.cache'
    ```
    
3.  混合存储

    ```python
    # 存储于混合型的数据库之中，即二者皆用
    SESSION_ENGINE= 'django.contrib.sessions.backends.cached_db'
    ```
    
4.  指定缓存的默认库

    ```python
    # 指定缓存的默认库
    SESSION_CACHE_ALIAS= "default"
    ```

### 6.2.3 Set/Get/Del Session

通过 HttpRequest 对象的 session 属性进行会话的读写操作，session 的默认有效期为两周。

1.  set session

    `request.session[key] = value`

2.  get session

    `request.session.get(key, default_value)`

3.  del session

    *   清除 all session 的 value 值，key 保留

        `request.session.clear()`

    *   彻底清除 all session 的 key, value

        `request.session.flush()`

    *   删除 session 中指定的键的值，key 保留

        `del request.session[key]`

4.  set session expiry

    `request.session.set_expiry(value)`，value 值为 0，session 保存周期为会话周期

__实例：__ 

```python
# demoDjango/demoDjango/urls.py
from xml.etree.ElementInclude import include
from django.contrib import admin
from django.urls import path, re_path, include
from demoDjango import views

urlpatterns = [
    path('admin/', admin.site.urls),

    # home子路由
    path('', include(('home.urls', 'home'))),  # namespace: home
]
```

```python
# demoDjango/home/urls.py

from django.urls import path
from home import views

urlpatterns = [
    # 绑定路由到视图
    # session
    path('set_session/', views.set_session_views, name='set_session'),
    path('get_session/', views.get_session_views, name='get_session'),
    path('del_session/', views.del_session_views, name='del_session'),
]
```

```python
# demoDjango/home/views.py

from django.http import HttpResponse

def set_session_views(request):
    request.session['name'] = 'zhangsan'
    request.session.set_expiry(600 )
    return HttpResponse('set session success.')


def get_session_views(request):
    name = request.session.get('name')

    return HttpResponse(name)


def del_session_views(request):
    # 清除session，只是清除不是删除
    request.session.clear()

    # 彻底删除
    request.session.flush()

    # 只删除一项
    del request.session['name']

    return HttpResponse('delete session success.')
```

访问测试：

1. http://127.0.0.1:8000/set_session/
2. http://127.0.0.1:8000/get_session/
3. http://127.0.0.1:8000/del_session/



# 7、类视图

函数视图区分：以函数的方式定义的视图称为函数视图，函数视图便于理解。

函数视图缺点：遇到一个视图对应的路径提供了多种 HTTP 请求方式的支持时，便需要在一个函数中编写不同的业务逻辑，代码可读性与复用性都不佳。

使用类来定义的视图，称为类视图。在其核心，基于类的视图允许您使用不同的类实例方法来响应不同的 HTTP 请求方法，而不是在单个视图函数中使用有条件的分支代码。

__使用类视图的好处：__ 

1.  代码可读性好；

2.  类视图相对于函数视图有更高的复用性，如果其他地方需要用到某个类视图的特定逻辑，直接 copy 即可。

__实例：__ 

```python
# demoDjango/demoDjango/urls.py
from xml.etree.ElementInclude import include
from django.contrib import admin
from django.urls import path, re_path, include
from demoDjango import views

urlpatterns = [
    path('admin/', admin.site.urls),

    # home子路由
    path('', include(('home.urls', 'home'))),  # namespace: home
]
```

```python
# demoDjango/home/urls.py


from django.urls import path
from home import views

urlpatterns = [
    # 绑定路由到视图
    # class view
    path('class/', views.class_views.as_view(), name='class_view'),
]
```

```python
# demoDjango/home/views.py

from django.http import HttpResponse
from django.views import View

# class view
class class_views(View):    # 继承View类
    def get(self, request):
        html = "<h1>我是类视图的get请求</h1>"
        return HttpResponse(html)

    def post(self, request):
        html = "<h1>我是类视图的post请求</h1>"
        return HttpResponse(html)
```

访问测试：Postman 向 http://127.0.0.1:8000/class/ 发起 get, post 请求测试。

## 7.1 类视图装饰器

__调用方式：__ 

1.  直接调用

    `path('path/', views.wrapper(views.class_views.as_view()), name='decorator_class_view'),`

2.  装饰器语法糖

    *   给所有方法都添加

        `@method_decorator(wrapper, name='dispatch')`，在类视图前加。

    *   给特定的方法添加

        `@method_decorator(wrapper, name='get')`，在类视图前加。

        `@method_decorator(wrapper)`，在方法前加。

__实例1：__ 给所有方法都添加，在视图文件中前加。

```python
# demoDjango/home/urls.py

from django.urls import path
from home import views

urlpatterns = [
    # 绑定路由到视图
    # class view
    path('class/', views.class_views.as_view(), name='class_view'),
    # decorator class view
    path('decorator_class/', views.logger(views.class_views.as_view()), name='decorator_class_view'),
]

```

```python
# demoDjango/home/views.py
from django.http import HttpResponse
from django.views import View

# logger装饰器
def logger(fn):
    print("\033[0;32mlogger decorator.\033[0m")

    def wrapper(request, *args, **kwargs):
        print("\033[0;32mbrgin...\033[0m")
        ret = fn(request, *args, **kwargs)
        print("\033[0;32mrequest path: {}\033[0m".format(request.path))
        print("\033[0;32mend...\033[0m")
        return ret

    return wrapper


# class view
class class_views(View):    # 继承View类
    def get(self, request):
        html = "<h1>我是类视图的get请求</h1>"
        return HttpResponse(html)

    def post(self, request):
        html = "<h1>我是类视图的post请求</h1>"
        return HttpResponse(html)
```

__实例二：__ 给所有的方法加，在类视图前加。

```python
# demoDjango/home/urls.py

from django.urls import path
from home import views

urlpatterns = [
    # 绑定路由到视图
    # class view
    path('class/', views.class_views.as_view(), name='class_view'),
    # decorator class view
    path('decorator_class/', views.class_views.as_view(), name='decorator_class_view'),
]

```

```python
# demoDjango/home/views.py
from django.http import HttpResponse
from django.views import View
from django.utils.decorators import method_decorator

# logger装饰器
def logger(fn):
    print("\033[0;32mlogger decorator.\033[0m")

    def wrapper(request, *args, **kwargs):
        print("\033[0;32mbrgin...\033[0m")
        ret = fn(request, *args, **kwargs)
        print("\033[0;32mrequest path: {}\033[0m".format(request.path))
        print("\033[0;32mend...\033[0m")
        return ret

    return wrapper


# class view
@method_decorator(logger, name='dispatch')    # 给所有类视图中的方法添加装饰器
class class_views(View):    # 继承View类
    def get(self, request):
        html = "<h1>我是类视图的get请求</h1>"
        return HttpResponse(html)

    def post(self, request):
        html = "<h1>我是类视图的post请求</h1>"
        return HttpResponse(html)

```

__实例三：__ 给特定的方法加，在类视图前加。

```python
# demoDjango/home/urls.py

from django.urls import path
from home import views

urlpatterns = [
    # 绑定路由到视图
    # class view
    path('class/', views.class_views.as_view(), name='class_view'),
    # decorator class view
    path('decorator_class/', views.class_views.as_view(), name='decorator_class_view'),
]

```

```python
# demoDjango/home/views.py
from django.http import HttpResponse
from django.views import View
from django.utils.decorators import method_decorator

# logger装饰器
def logger(fn):
    print("\033[0;32mlogger decorator.\033[0m")

    def wrapper(request, *args, **kwargs):
        print("\033[0;32mbrgin...\033[0m")
        ret = fn(request, *args, **kwargs)
        print("\033[0;32mrequest path: {}\033[0m".format(request.path))
        print("\033[0;32mend...\033[0m")
        return ret

    return wrapper

# class view
@method_decorator(logger, name='get')
class class_views(View):    # 继承View类
    def get(self, request):
        html = "<h1>我是类视图的get请求</h1>"
        return HttpResponse(html)

    def post(self, request):
        html = "<h1>我是类视图的post请求</h1>"
        return HttpResponse(html)

```

**实例四：** 给特定的方法加，在方法前加。

```python
# demoDjango/home/urls.py

from django.urls import path
from home import views

urlpatterns = [
    # 绑定路由到视图
    # class view
    path('class/', views.class_views.as_view(), name='class_view'),
    # decorator class view
    path('decorator_class/', views.class_views.as_view(), name='decorator_class_view'),
]
```

```python
# demoDjango/home/views.py
from django.http import HttpResponse
from django.views import View
from django.utils.decorators import method_decorator

# logger装饰器
def logger(fn):
    print("\033[0;32mlogger decorator.\033[0m")

    def wrapper(request, *args, **kwargs):
        print("\033[0;32mbrgin...\033[0m")
        ret = fn(request, *args, **kwargs)
        print("\033[0;32mrequest path: {}\033[0m".format(request.path))
        print("\033[0;32mend...\033[0m")
        return ret

    return wrapper

# class view
class class_views(View):    # 继承View类
    @method_decorator(logger)
    def get(self, request):
        html = "<h1>我是类视图的get请求</h1>"
        return HttpResponse(html)

    def post(self, request):
        html = "<h1>我是类视图的post请求</h1>"
        return HttpResponse(html)

```



# 8、中间件

Django 中的中间件是一个轻量级、底层的插件系统，可以介入 Django 的请求和响应处理过程，修改 Django 的输入或输出。中间件的设计为开发者提供了一种无侵入式的开发方式，增强了 Django 框架的健壮性。我们可以使用中间件，在 Django 处理视图的不同阶段对输入或输出进行干预。

```python
MIDDLEWARE = [
    # 安全中间件
    'django.middleware.security.SecurityMiddleware',
    # 会话中间件
    'django.contrib.sessions.middleware.SessionMiddleware',
    # 通用中间件
    'django.middleware.common.CommonMiddleware',
    # 保护中间件：CSRF
    # 'django.middleware.csrf.CsrfViewMiddleware',
    # 认证中间件
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    # 消息中间件
    'django.contrib.messages.middleware.MessageMiddleware',
    # X-Frame-Options中间件：劫持
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]

```

__中间件定义：__ 

通过中间件工厂函数，返回一个可以被调用的中间件。中间件工厂函数需要接收一个可以调用的 get\_response 对象。

返回的中间件也是一个可以被调用的对象，并且像视图一样需要接受一个 request 对象参数，返回一个 response 对象。

__中间件实例：__ 

1.  home 子应用新建 middle.py 文件

2.  编写中间件

    ```python
    # demoDjango/home/middle.py

    def my_middleware(get_response):
        print("\033[0;32m加载自定义中间件...\033[0m")
        
        def _middleware(request):
            print("\033[0;32m拦截返回之前...\033[0m")
            response = get_response(request)
            print("\033[0;32m拦截返回之中...\033[0m")
            return response
        
        return _middleware
    ```

3.  注册中间件

    ```python
    # demoDjango/demoDjango/settings.py
    
    MIDDLEWARE = [
        # 安全中间件
        'django.middleware.security.SecurityMiddleware',
        # 会话中间件
        'django.contrib.sessions.middleware.SessionMiddleware',
        # 通用中间件
        'django.middleware.common.CommonMiddleware',
        # 保护中间件：CSRF
        # 'django.middleware.csrf.CsrfViewMiddleware',
        # 认证中间件
        'django.contrib.auth.middleware.AuthenticationMiddleware',
        # 消息中间件
        'django.contrib.messages.middleware.MessageMiddleware',
        # X-Frame-Options中间件：劫持
        'django.middleware.clickjacking.XFrameOptionsMiddleware',
        # 自定义中间件
        'home.middle.my_middleware',
    ]
    ```

注意：中间件会拦截所有的请求过程，做进一步处理，进而返回数据。



# 9、模版

Django 的模板是一个简单的文本文件，可以是任何文本格式，如 HTML、XML、TXT 等，推荐使用 HTML 格式。在 Django 中可以看做创建 HTML 页面的样本。包含静态的 HTML 和用于描述如何动态生成 HTML 的特殊语法两个部分。&#x20;

Django 模板的结构与 Html 文件相似，甚至完全可以使用一个 HTML 文件作为模板。

Django 使用模板引擎 Django Template Language（往后统称 DTL）与 Jinja2 对模板文件进行解释，一个 Django 工程可以配置一个或者多个模板引擎。如果项目中没有使用模板，那么也可以不配置模板引擎。

Django 自带的模板系统叫做 DTL，通过该引擎可以方便地加载模板文件并在内存中进行编译，然后插入动态数据，最后返回一个字符串。Django 中很多内部组件都使用了 DTL，例如 admin，因此不建议放弃 DTL。

__模版配置：__ 

1.  创建 templates 模版文件夹

    新建文件夹—右击更改文件夹属性为模版文件夹

2.  配置 settings.py 文件—模版参数

    ```python
    # demoDjango/demoDjango/settings.py
    TEMPLATES = [
        {
            # 自带的模版引擎
            'BACKEND': 'django.template.backends.django.DjangoTemplates',
            # 定位模版文件夹
            'DIRS': [os.path.join(BASE_DIR, 'templates')]
            ,
            # 是否在子应用中寻找templates
            'APP_DIRS': True,
            # 默认操作
            'OPTIONS': {
                'context_processors': [
                    'django.template.context_processors.debug',
                    'django.template.context_processors.request',
                    'django.contrib.auth.context_processors.auth',
                    'django.contrib.messages.context_processors.messages',
                ],
            },
        },
    ]
    ```

__注意：__ 模板引擎的 key 用于渲染模板时查找模板引擎，key 必须唯一，即当存在多个模板引擎时只能有一个叫做 BACKEND。DIRS 的值，列表中的位置顺序就是模板引擎查找模板的顺序。

__加载模版：__ 

`django.template.loader` 模块中提供了两个用于加载模板的方法：

*   `get\_template('template\_name', using=None)`

    该方法使用给定的名称查找和加载模板，并返回一个 Template 对象。如果想使用指定的模板引擎进行查找，请将模板引擎的 NAME 赋给 get\_template 的 using 参数。

*   `select\_template('template\_name\_list', using=None)`

    该方法接收一个模板名称的列表，返回第一个存在的 Template 对象。同样 using 参数用于改变模板引擎。

当找不到对应的模板时，这两个方法都会返回 TemplateDoesNotExist 异常。如果模板找到了，但是模板中存在语法错误，返回 TemplateSyntaxError 异常。

__模版渲染：__ 

__语法：__ `template.render(context=None, request=None)`

__参数：__ 

*   context：需要展示到 HTML 文件上的数据集合，是一个字典对象。如果 render 没有接受任何 context，模板引擎就会直接渲染模板而不插入任何数据。

*   request：一个 HttpRequest 对象。不同的模板引擎对 request 对象的处理方式不同。

Django 提供了一个函数 render 可以简写上述方法：`render(request, 'index.html', data)`

__实例：__ 

1.  新建 demoDjango/templates/index.html 文件

    ```python
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>主页</title>
    </head>
    <body>
    <h1>我是模版文件</h1>
    {{ '名字为: ' }}
    {{ name }}
    </body>
    </html>
    ```
    
2.  加载html模版文件

    ```python
    # demoDjango/home/views.py

    from django.http import HttpResponse
    from django.shortcuts import render
    from django.views import View
    from django.template import loader

    class index_views(View):
        def get(self, request):
            # 方法一：
            # 加载模版文件
            template = loader.get_template('index.html')
            # 渲染模版文件
            html = template.render()
            return HttpResponse(html)

        def post(self, request):
            data = {
                'name': 'yannic',
                'age' : 18,
                'books': {
                    'book1': 'python',
                    'book2': 'mysql',
                    'book3': 'kubernetes',
                }
            }
            # 方法二：
            return render(request, 'index.html', data)
    ```

3. 访问测试：http://127.0.0.1:8000/index/



## 9.1 模版语言

### 9.1.1 注释

__语法：__ 

1.  单行注释

    ```html
    {# 注释一：单行注释 #}

    <!-- 注释二：前端语法支持的注释，可以在F12中查看 -->
    ```

2.  多行注释

    ```html
    {% comment %}
    注释三：多行注释，本质来说是标签
    {% endcomment %}
    ```

### 9.1.2 变量

__语法：__ `{{ valuename }}`

*   如果变量是一个字典数据，那么可以使用 `.` 来进行 key 查找。

*   如果变量是一个方法数据，那么可以使用 `.` 来直接调用方法。

*   如果变量是一个列表数据，那么可以使用下标的方式来取值。

__实例：__ 

```html
{# demoDjango/templates/index.html #}

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>主页</title>
</head>
<body>
<h3>1.name</h3>
{{ name }}
<h3>2.age</h3>
{{ age }}
<h3>3.book</h3>
{{ books.book1 }}<br>{{ books.book2 }}<br>{{ books.book3 }}
</body>
</html>
```

```python
# demoDjango/home/views.py

class index_views(View):
    def get(self, request):
        # 加载模版文件
        template = loader.get_template('index.html')

        # 渲染模版文件
        html = template.render()
        return HttpResponse(html)

    def post(self, request):
        data = {
            'name': 'yannic',
            'age': 23,
            'books': {
                'book1': 'python',
                'book2': 'mysql',
                'book3': 'kubernetes',
            }
        }
        return render(request, 'index.html', data)
```

访问测试：http://127.0.0.1:8000/index/

### 9.1.3 标签

类似于 `{% tag %}`，相对于变量来说标签更加复杂，标签可以用于输出文本、控制代码逻辑等。

有的标签还需要有开始标记和结束标记，这类标签的格式类似于 `{% tag %}···{% endtag %}`。

__常用标签：__ 

1.  comment，注释标签

    ```html
    {% comment %}
    {% endcomment %}

    ```

2.  for，循环标签

    ```html
    <ul>
        {% for k, v in books.items %}
            {{ k }}: {{ v }}<br>
        {% endfor %}
    </ul>
    ```

3.  if，判断标签

    ```html
    {% if age >= 18 %}
        {{ "已成年" }}
    {% else %}
        {{ "未成年" }}
    {% endif %}

    ```

4.  with，别名标签

    ```html
    {% with books.book1 as b1 %}
        b1: {{ b1 }}
    {% endwith %}
    ```

5.  block，定义可被重写

6.  extends，用于标记当前模版继承自哪个模版

### 9.1.4 过滤器

作用：过滤器可以用来修改变量的显示样式。

语法：`{{ value|过滤器1|过滤器2:arg }}`

*   safe，关闭 html 自动转义

*   length，获取长度

*   default，默认值，如果变量不存在时则返回默认值

*   date，日期，用关于对日期类型的值进行字符串格式化

    `{{ time|date:"%Y-%m-%d %H:%M:%S" }}`

*   add，将参数添加到值中

    `{{ age|add:1 }}`

注意：变量、管道符('|')和过滤器方法之间不能有空格

## 9.2 模版继承

目的：模板继承和类的继承含义是一样的，主要是为了提高代码重用，减轻开发人员的工作量。

标签 block：用于在父模板中预留区域，留给子模板填充差异性的内容，名字不能相同。为了更好的可读性，建议给 endblock 标签协商名字，这个名字与对应的 block 名字相同。父模板中也可以使用上下文中传递过来的数据。

__语法：__ 

*   父模板中&#x20;

    ```html
    {% block father %}

    {% endblock %}
    ```

*   子模版中

    ```html
    {% extends "index.html" %}
    ```
