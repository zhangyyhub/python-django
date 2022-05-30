# 1、ORM 介绍

对象关系映射（Object Relational Mapping，简称 ORM）是一种为了解决面向对象与关系数据库存在的互不匹配的现象的技术。简单的说，ORM 是通过使用描述对象和数据库之间映射的元数据，将程序中的对象自动持久化到关系数据库中。&#x20;

ORM 的方法论基于三个核心原则：

*   简单性：以最基本的形式建模数据

*   传达性：数据库结构被任何人都能理解的语言文档化

*   精确性：基于数据模型创建正确标准化了的结构

__ORM 的优点：__ 

1.  易用性：使用 ORM 做数据库的开发可以有效的减少重复 SQL 语句的概率，写出来的模型也更加直观、清晰。

2.  性能损耗小：ORM 转换成底层数据库操作指令确实会有一些开销。但从实际的情况来看，这种性能损耗很少（不足 5%），只要不是对性能有严苛的要求，综合考虑开发效率、代码的阅读性，带来的好处是远远大于性能损耗，而且项目越大作用越明显。

3.  设计灵活：可以轻松的写出复杂的查询。

4.  可移植性：Django 封装了底层的数据库实现，支持多个关系数据库引擎，包括流行的 MySQL、PostgreSQL 和 SQLite。

__ORM 映射关系：__ 

| ORM       | Database |
| --------- | -------- |
| Models 类 | 数据库   |
| 对象实例  | 一条记录 |
| 属性      | 字段     |

__ORM 解析流程：__ 

1.  将 python 代码转为 SQL 语句；

2.  SQL 语句通过 pymysql 传送到数据库服务端；

3.  在数据库中执行 SQL 语句并将结果返回。



# 2、Model 类

模型是一个用于表示数据的 python 类，包含基本的数据字段和行为，通常一个模型就代表一张数据库表。

模型继承自 Django.db.models.Model，模型的每一个属性代表一个数据的字段。

__字段：__ 

字段是模型最重要的组成成分，它是一系列的数据字段的定义。模型字段是模型的类属性，它的命名不能与模型接口相同，如不能定义名为 clean、save、delete 等，同时字段名字中不能连续出现两个下划线，因为连续的两个下划线是 Django 数据库 API 的特殊语法。

每一个模型字段都是 django.db.models.Field 的实例，对应一种数据库存储格式以及 HTML 元素样式。

__常用字段：__ 

1.  AutoField

    IntegerField 的改进形式，字段值根据已有的 id 自动增长，常用作主键。一般情况下 Django 是为我们自动创建好了主键的。

2.  CharField

    用于保存不太长的字符串。使用该字段必须给出字段参数 max\_length，该属性指定了 CharField 所能接收的最大字符数，也有用于字段有效性验证。

3.  BooleanField

    字段值只包含 True 和 False。类似于 SQLServer 中的 bit 类型。

4.  DateField

    日期类型，对应 python 中的 datetime.date 类型。

5.  DateTimeField

    日期时间类型，对应 python 中的 datetime.datetime 类型。

6.  EmailField

    本质就是 CharField，不过会验证输入的字符串是不是一个有效的邮件地址。

7.  FileField

    文件上传空间。该字段不允许使用 primary\_Key 属性。包含两个可选参数：FileField.upload\_to 和 FileField.storage

8.  InteferField

    整数字段，取值范围 -2147483648\~2147483648，该类型对于所有 Django 支持的数据库来说都是安全的。

__字段参数：__ 

1.  null

    如果设置为 True，当该字段为空时，Django 会将数据库中该字段设置为 NULL。默认为 False。对于文本型字段，尽可能不使用 null 属性，因为当使用默认值 null 时，数据库中就可能出现两种空数据：NULL 和空字符串，而 Django 默认使用空字符串。

2.  Blank

    默认为 False，当设置 Field.blank=True 时，字段值允许为空。与 null 不同的是，null 只是表示数据库值是空的，而 blank 用于表单验证。当字段属性 blank=True 时，表单验证将允许字段值为空；当 blank=False 时，表单字段将变成必填字段。

3.  Default

    该字段的默认值，可以是一个值或者一个可调用对象，如果是一个可调用对象，每次实例化模型都会调用该对象。

4.  verbose\_name

    字段说明，如果没有给出 verbose\_name 属性，Django 会使用字段名作为 verbose\_name 值。如果字段名中包含下划线，下划线会被转换为空格。ForeignKey、ManyToManyField、OneToOneField 三种字段类型要求第一个参数必须是模型类，因此必须使用 verbose\_name。

5.  primary\_Key

    如果设置为 True，将该字段设置为模型的主键。如果模型中任何字段都不包含 primary\_Key=True 属性，Django 将会自动为模型添加一个 IntegerField 字段作为主键。主键永远是只读的，如果修改一个模型实例的主键并保存，等同于创建了一个新的模型实例了。

__实例：__ 

1.  定义 Model 类

    ```python
    # demoDjango/home/models.py
    
    from django.db import models
    
    # Create your models here.
    
    # 书籍属性
    class book(models.Model):
        # 书名、类型、概要、作者：字符类型
        book_name     = models.CharField(verbose_name='书名', max_length=30)
        book_type     = models.CharField(verbose_name='类型', max_length=20)
        book_outline  = models.CharField(verbose_name='概要', max_length=50)
        book_author   = models.CharField(verbose_name='作者', max_length=20)
        # 书籍价格：整数类型
        book_price    = models.IntegerField(verbose_name='价格')
        # 上架时间：时间类型
        book_datetime = models.DateField(auto_now=True)
        # 是否库存：布尔类型
        book_residual = models.BooleanField(default=True)
    
        # 元数据
        class Meta:
            db_table = 'book'
            verbose_name = '书籍'
            verbose_name_plural = verbose_name  # 不显示复数形式
    
        def __str__(self):
            return f'{self.book_name},{self.book_type},{self.book_outline},{self.book_author},{self.book_price},{self.book_datetime},{self.book_residual}'
    
          
    class bookInfo(models.Model):
        # 书籍位置
        book_site = models.CharField(verbose_name='位置', max_length=30)
        # 书籍编号：ISBN-10
        book_serial =models.CharField(verbose_name='编号', max_length=30)
        # 外键关联：一起删除(CASCADE)、保护(PROTECT)
        # book = models.ForeignKey(book, on_delete=models.CASCADE)  # 默认字段名为book_id，db_column='book'
    
        class Meta:
            db_table = 'bookinfo'
            verbose_name = '书籍信息'
            verbose_name_plural = verbose_name  # 不显示复数形式

2.  数据库迁移

    *   配置、创建数据库

    *   安装 pymysql 库，配置驱动

    ```python
    # 生成迁移文件
    python manage.py makemigrations
    
    # 迁移
    python manage.py migrate
    ```



# 3、增删改查

## 3.1 增加

*   方法一

    实例化Model类，save保存

    ```python
    # demoDjango/home/views.py

    from django.shortcuts import render
    from django.views import View
    from home.models import book

    class home_views(View):
        def get(self, request):
            # 方法一
            my_book = book(
                book_name='追风筝的人',
                book_type='长篇小说',
                book_outline='关于人性的背叛与救赎',
                book_author='Khaled Hosseini',
                book_price=45,
                # 上架时间：时间类型
                book_datetime='2021-11-02',
                # 是否库存：布尔类型
                book_residual=True,
            )
            my_book.save()

            return render(request, 'index.html')
    ```

*   方法二

    Model.objects.create方法添加数据

    ```python
    # demoDjango/home/views.py
    
    from django.shortcuts import render
    from django.views import View
    from home.models import book
    
    class home_views(View):
        def get(self, request):
            # 方法二
            book.objects.create(
                book_name='活着',
                book_type='长篇小说',
                book_outline='生的苦难与伟大',
                book_author='余华',
                book_price=35,
                # 上架时间：时间类型
                book_datetime='2021-11-04',
                # 是否库存：布尔类型
                book_residual=True,
            )
    
            return render(request, 'index.html')
    ```

## 3.2删除

*   方法一

    使用Model.objects.get()，先查询、后删除（delete）

    ```python
    # demoDjango/home/views.py

    from django.shortcuts import render
    from django.views import View
    from home.models import book

    class home_views(View):
        def get(self, request):
            # 删除方法一
            data = book.objects.get(book_name='活着')
            data.delete()

            return render(request, 'index.html')
    ```

*   方法二

    使用Model.objects.filter(book\_name='活着').delete()，过滤删除

    ```python
    # demoDjango/home/views.py
    
    from django.shortcuts import render
    from django.views import View
    from home.models import book
    
    class home_views(View):
        def get(self, request):
            # 删除方法二
            book.objects.filter(book_name='活着').delete()
    
            return render(request, 'index.html')
    
    ```

## 3.3修改

*   方法一

    使用Model.objects.get() 先查询，在修改，最后save保存

    ```python
    # demoDjango/home/views.py

    from django.shortcuts import render
    from django.views import View
    from home.models import book

    class home_views(View):
        def get(self, request):
            # 修改方法一
            data = book.objects.get(book_name='活着')
            data.book_price = 100
            data.save()

            return render(request, 'index.html')

    ```

*   方法二

    使用Model.objects.filter().update() 先过滤再修改

    ```python
    # demoDjango/home/views.py
    
    from django.shortcuts import render
    from django.views import View
    from home.models import book
    
    class home_views(View):
        def get(self, request):
            # 修改方法二
            book.objects.filter(book_name='活着').update(book_price=80)
    
            return render(request, 'index.html')
    
    ```

## 3.4查询

### 3.4.1基本查询

*   精准查询：Model.objects.get()

*   查询所有：Model.objects.all()

*   查询数量：Model.objects.count()

```python
# demoDjango/home/views.py

from django.shortcuts import render
from django.views import View
from home.models import book

class home_views(View):
    def get(self, request):
        # 基本查询
        query01 = book.objects.get(book_name='活着')
        print(query01)    # 活着,长篇小说,生的苦难与伟大,余华,80,2021-11-14,True
        
        # 查询所有
        query02 = book.objects.all()
        print(query02)    # <QuerySet [<book: 追风筝的人,长篇小说,关于人性的背叛与救赎,Khaled Hosseini,45,2021-11-14,True>, <book: 活着,长篇小说,生的苦难与伟大,余华,80,2021-11-14,True>]>

        # 查询数量
        print(book.objects.count())
        return render(request, 'index.html')

```

### 3.4.2过滤查询

**语法：**`属性__比较运算符 = value`

*   book.objects.filter() &#x20;

*   book.objects.get() &#x20;

*   book.objects.exclude()

```python
# demoDjango/home/views.py

from django.shortcuts import render
from django.views import View
from home.models import book

class home_views(View):
    def get(self, request):
        # 返回单个对象
        book.objects.get(book_price=80)
        book.objects.get(book_price__exact=80)
        
        # 返回一个集合
        book.objects.filter(book_price=80)
        book.objects.filter(book_price__exact=80)

        return render(request, 'index.html')

```

### 3.4.3模糊查询

**语法：**`属性__contains = value`

```python
# demoDjango/home/views.py

from django.shortcuts import render
from django.views import View
from home.models import book

class home_views(View):
    def get(self, request):
        # 模糊查询
        book.objects.filter(book_name__contains='人')
        book.objects.filter(book_name__startswith='追')
        book.objects.filter(book_name__endswith='人')
        return render(request, 'index.html')

```

### 3.4.4比较查询

**比较运算符：**`exact(等于), gt(大于), gte(大于等于), lt(小于), lte(小于等于)`

```python
# demoDjango/home/views.py

from django.shortcuts import render
from django.views import View
from home.models import book

class home_views(View):
    def get(self, request):
        # 比较查询
        book.objects.filter(book_price__gt=50)

        return render(request, 'index.html')
```

### 3.4.5不等查询

```python
# demoDjango/home/views.py

from django.shortcuts import render
from django.views import View
from home.models import book

class home_views(View):
    def get(self, request):
        # 不等查询
        book.objects.exclude(book_price=80)
        book.objects.exclude(book_price__exact=80)

        return render(request, 'index.html')

```

### 3.4.6日期查询

```python
# demoDjango/home/views.py

from django.shortcuts import render
from django.views import View
from home.models import book

class home_views(View):
    def get(self, request):
        # 日期查询
        book.objects.filter(book_datetime__gt='2021-11-01')

        return render(request, 'index.html')

```

### 3.4.7 F对象

F对象应用于两个字段之间的比较，需要在django.db.models中导入，语法：`filter(字段名__运算符=F('字段名'))`

```python
# demoDjango/home/views.py

from django.shortcuts import render
from django.views import View
from home.models import book
from django.db.models import F, Q

class home_views(View):
    def get(self, request):
        # F对象
        book.objects.filter(book_price__gt=F('book_residual')*2)

        return render(request, 'index.html')
```

### 3.4.8 Q对象

Q对象之间可以通过&、|运算符组合起来，多个Q对象通过运算符组合起来形成一个新的Q对象。使用\~来表示逻辑非。

```python
# demoDjango/home/views.py

from django.shortcuts import render
from django.views import View
from home.models import book
from django.db.models import F, Q

class home_views(View):
    def get(self, request):
        # Q对象
        book.objects.filter(book_price__gt=30, book_datetime__gt='2021-11-01')
        book.objects.filter(book_price__gt=30).filter(book_datetime__gt='2021-11-01')
        
        book.objects.filter(Q(book_price__gt=30) & Q(book_datetime__gt='2021-11-01'))
        book.objects.filter(Q(book_price__gt=30) | Q(book_datetime__gt='2021-11-01'))
        book.objects.filter(~Q(book_price__gt=30))

        return render(request, 'index.html')

```

### 3.4.9 关联查询

在django中可以通过模型之间的关系查找数据。

```python
from django.shortcuts import render
from django.views import View
from home.models import book, bookInfo

class home_views(View):
    def get(self, request):
        mybook = bookInfo.objects.get(id=1)
        print(mybook.book_set.all())

        return render(request, 'index.html')

```

## 3.5查询集

查询集，也称为查询结果集、QuerySet，表示从数据库中获取的对象集合。当调用all, filter, exclude, order\_by过滤器方法时，django会返回查询集而不是返回简单的列表。

对查询集可以再次调用过滤器进行过滤。也就意味着查询集可以含有零个、一个或多个过滤器。过滤器基于所给的参数限制查询的结果。

从SQL的角度讲，查询集与select语句等价，过滤器像where、limit、orderby子句。

判断某一个查询集中是否有数据：

*   \-exists()：判断查询集中是否有数据，如果有则返回True，没有则返回False。

```python
from django.shortcuts import render
from django.views import View
from home.models import book, bookInfo

class home_views(View):
    def get(self, request):
        # 查询排序
        print(book.objects.filter(book_price__gt=60).order_by('book_price'))
        print(book.objects.filter(book_price__gt=60).order_by('book_price').exists())

        return render(request, 'index.html')
```

**查询集特性：**

1.  惰性执行

    创建查询集不会访问数据库，直到调用数据时，才会访问数据库，调用数据的情况包括迭代、序列化等。

2.  缓存

    使用同一个查询集，第一次使用会发生数据库的查询，然后Django会将结果缓存下来，再次使用这个查询集时会使用缓存的数据，减少了数据库的查询次数。

**限制查询集：**

可以对查询集进行取下标或切片操作，等同于sql中的limit和offset子句。注意：不支持负数索引，对查询集进行切片后返回一个新的查询集，不会立即执行查询。

如果获取一个对象，直接使用\[0]，等同于\[0:1].get()，但是如果没有数据，\[0]引发IndexError异常，\[0:1].get()如果没有数据，引发DoesNotExist异常。

```python
from django.shortcuts import render
from django.views import View
from home.models import book, bookInfo

class home_views(View):
    def get(self, request):
        # 限制查询集
        # 也支持切片操作(不支持负数)
        print(book.objects.all()[0])

        return render(request, 'index.html')

```

**查询集分页：**

```python
from django.shortcuts import render
from django.views import View
from home.models import book, bookInfo

class home_views(View):
    def get(self, request):
        # 查询集分页
        mybook = book.objects.all()
        paginator = Paginator(mybook, 2)
        # 获取第二页数据
        print(paginator.page(2))
        # 获取一共多少页
        print(paginator.num_pages)

        return render(request, 'index.html')

```
