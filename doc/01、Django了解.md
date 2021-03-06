# 1、Django 概念

Python 下有许多款不同的 Web 框架。Django 是重量级选手中最有代表性的一位。许多成功的网站和 APP 都基于 Django。

Django 是一个开放源代码的 Web 应用框架，由 Python 写成。

Django 基于 python 开发，只要很少的代码，Python 的程序开发人员就可以轻松地完成一个正式网站所需要的大部分内容，并进一步开发出全功能的 Web 服务。

Django 本身基于 MVC 模型，即 Model（模型）+ View（视图）+ Controller（控制器）设计模式，MVC 模式使后续对程序的修改和扩展简化，并且使程序某一部分的重复利用成为可能。

Django 采用了 MVT 的软件设计模式，即模型（Model），视图（View）和模板（Template）。

Django 遵守 BSD 版权，初次发布于 2005 年 7 月, 并于 2008 年 9 月发布了第一个正式版本 1.0。

Python + Django 是快速开发、设计、部署网站的最佳组合。

__特点：__ 

*   强大的数据库功能

*   自带强大的后台功能

*   优雅的网址

## 1.1 MVC 和 MVT 模型

### 1.1.1 MVC 模式

MVC 模式（Model–view–controller）是软件工程中的一种软件架构模式，把软件系统分为三个基本部分：模型（Model）、视图（View）和控制器（Controller）。

MVC 以一种插件式的、松耦合的方式连接在一起。

*   模型（M）：编写程序应有的功能，负责业务对象与数据库的映射（ORM）。

*   视图（V）：图形界面，负责与用户的交互（页面）。

*   控制器（C）：负责转发请求，对请求进行处理。

<img width="820" alt="截屏2022-05-11 17 42 22" src="https://user-images.githubusercontent.com/68216337/168033876-fdef33f4-9aa8-40c7-a034-158173bdeb5e.png">

__MVC 优势：__ 

*   低耦合

*   开发快捷

*   部署方便

*   可重用性高

*   维护成本低

*   ...

### 1.1.2 MVT 模型

Django 的 MTV 模式本质上和 MVC 是一样的，也是为了各组件间保持松耦合关系，只是定义上有些许不同，Django 的 MTV 分别是指：

*   M 表示模型（Model）：编写程序应有的功能，负责业务对象与数据库的映射（ORM）。

*   T 表示模板（Template）：负责如何把页面（html）展示给用户。

*   V 表示视图（View）：负责业务逻辑，并在适当时候调用 Model 和 Template。

除了以上三层之外，还需要一个 URL 分发器，它的作用是将一个个 URL 的页面请求分发给不同的 View 处理，View 再调用相应的 Model 和 Template，MVT 的响应模式如下所示：

<img width="820" alt="截屏2022-05-12 16 59 50" src="https://user-images.githubusercontent.com/68216337/168033991-006dbab3-d4a6-4ca4-9008-817792bcb8df.png">

__解析：__ 

用户通过浏览器向我们的服务器发起一个请求（request），这个请求会去访问视图函数：

*   如果不涉及到数据调用，那么这个时候视图函数直接返回一个模板也就是一个网页给用户。

*   如果涉及到数据调用，那么视图函数调用模型，模型去数据库查找数据，然后逐级返回。

视图函数把返回的数据填充到模板中，最后返回网页给用户。



# 2、Django 使用

## 2.1 Django 安装

```bash
pip install Django
```

## 2.2 创建项目

### 2.2.1 使用Pycharm创建

![image_GS3ybyajnW](https://user-images.githubusercontent.com/68216337/168034551-6f0c38d1-a091-453c-bb72-072bc5f1f390.png)

### 2.2.2 使用命令行创建

使用 django-admin 来创建 demoDjango 项目：

```python
django-admin startproject demoDjango
```

创建完成后我们可以查看下项目的目录结构：

```python
$ cd demoDjango 
$ tree
.
├── demoDjango         # 项目的容器
│   ├── __init__.py    # 一个空文件，告诉Python该目录是一个Python包。
│   ├── asgi.py        # 一个 ASGI兼容的Web服务器的入口，以便运行你的项目。
│   ├── settings.py    # 该Django项目的设置/配置
│   ├── urls.py        # 该Django项目的URL声明; 一份由Django驱动的网站"目录"
│   └── wsgi.py        # 一个WSGI兼容的Web服务器的入口，以便运行你的项目
└── manage.py          # 一个实用的命令行工具，可让你以各种方式与该Django项目进行交互

1 directory, 6 files

```

## 2.3 启动项目

### 2.3.1 终端启动

```python
$ python manage.py runserver 0.0.0.0:8000

```

在浏览器输入你服务器的ip（这里我们输入本机IP地址：127.0.0.1:8000）及端口号：

<img width="820" alt="截屏2022-05-11 17 42 22" src="https://user-images.githubusercontent.com/68216337/168035871-8066e6e0-230e-441d-8660-3d2d760a67e3.png">

### 2.3.2配置快速启动

![image_UIvuVw2LSs](https://user-images.githubusercontent.com/68216337/168036087-3562c6c1-e63b-43d8-b22d-d9908dc926a2.png)


