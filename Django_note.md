***

## 基础操作

### 创建Django项目

1. 通过终端创建

   首先在终端内进入某个目录，此目录就是接下来你项目创建所存放的位置；

   然后，由于Django-admin的目录Scripts在该电脑已经添加到了环境变量中，因此可以直接在终端输入 

   #### django-admin startproject 项目名称

2. 通过pycharm创建

   通过命令行创建的项目是标准的，通过pycharm创建的项目在标准之上添加了一些东西：

   - templates目录

   - settings.py中的TEMPLATES中有

     ```python
     TEMPLATES = [
         {
             'BACKEND': 'django.template.backends.django.DjangoTemplates',
             'DIRS': [BASE_DIR/'templates']
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
     ```

     项目不要和python放同一目录下

   

   3.项目中各文件的作用

   ![image-20220411150055514](C:\Users\yueya\Nutstore\1\我的坚果云\graph\image-20220411150055514.png)

​		  在一个Django项目里，不同的app用来完成不同的功能：

​          比如用来管理用户的app，订单管理的app，后台管理的app，网站的app…

​          其中每一个app中有自己独立的表结构，函数，HTML模板，CSS模板

​          这些app共同完成一个项目

***

### 启动Django项目

1. 通过命令行启动

   #### python manage.py runserver

2. 通过pycharm启动


***

### App的创建和注册

1. 在当前项目的目录下打开terminal，通过输入

   ```python
   python manage.py startapp app01 
   ```

   的命令来创建一个名为app01的文档

2. 在app01文档中，有着不同的文件，其中目前有views.py和modules.py比较重要

   - views.py 中记录着URL与函数的对应关系，而urls.py中要执行的函数都定义在views.py中，即执行urls.py时就要调用views.py中的函数
   - modules.py 是用来对数据库进行操作，不再需要pymysql库

3. 将app01添加到settings.py文件中的INSTALLED_APPS函数中就成功注册了

   ```python
   INSTALLED_APPS = [
       'django.contrib.admin',
       'django.contrib.auth',
       'django.contrib.contenttypes',
       'django.contrib.sessions',
       'django.contrib.messages',
       'django.contrib.staticfiles',
       'app01'
   ]
   ```



![](C:\Users\yueya\Nutstore\1\我的坚果云\graph\image-20220411153145032.png)

***

### 操作流程

1. urls.py中调用views.py中的函数，views.py中的函数返回值可以是字符串也可以是一个html网站。因此我们在app01内创建一个新目录templates用来放置html文件

2. 创建后在urls.py中引入app01

   ```python
   from app01 import views
   
   urlpatterns = [
       path('index/',views.department_list)
   ]
   ```

   第一个参数用来写URL，相当于用户访问我们网址时是[www.xxx.com/index/](http://www.xxx.com/index/)

   第二个参数是函数，相当于用户访问网址时就会自动执行views.py中的index函数

3. 编写视图函数

   ```python
   from django.shortcuts import render,HttpResponse,redirect
   
   def index(request):
       return HttpResponse('welcome user') 
   # 形式参数request是默认的，它是一个对象，封装了用户发送过来的所有请求相关的数据
   # 当用户访问我们的网站时，会看到welcome user的字符串
   
   def user_list(request):
       return render(request,'user_list.html') 
   
   
   ```

4. Django寻找文件的两种方式

   - 如果是通过pycharm创建的Django项目，那么在setting.py中的TEMPLATES中有

     ```python
     ‘DIRS’ : [BASE_DIR / 'templates']
     ```

     这意味着如果根目录与app的templates文件夹中有同名文件，则优先从项目djangoProject1中根目录的templates文件夹中寻找同名文件而不是从app中的templates文件夹中优先寻找

     如果根目录中没有该文件，再按照settings.py中app的注册顺序（而不是直接在当前app文件夹中的templates文件中寻找）依次在各个app中的templates目录中寻找

   - 如果是通过cmd命令行创建的Django项目，那么‘DIRS’ : [ ], \1.   意味着按照settings.py中app的注册顺序（而不是直接在当前的app文档中的templates文件中找）依次在各个app中的templates目录中寻找

***

### 浏览器请求和响应的原理

访问网站的顺序是：

1. 访问网址
2. 调用函数
3. 返回页面

一. 请求

1. 查看用户请求的方式（get/post）

   ```python
   print(request.method)
   
   GET
   ```

2. 在URL上传递值 (http://127.0.0.1:8000/something/?n1=123&n2=999)

   ```python
   print(request.GET) 
   #通过request.GET获取URL上传递的参数
   
   <QueryDict: {'n1': ['123'], 'n2': ['999']}>
   ```

3. 在请求体中提交数据

   ```python
   print(request.POST)
   
   <QueryDict: {}>
   ```

二. 响应

1. HttpResponse

2. Render

3. Redirect

   ```python
   return redirect("https://www.baidu.com")
   # Django是给浏览器返回一个新地址让浏览器自己访问，而不是Django直接给浏览器返回新页面
   ```

   ![image-20220411181253075](C:\Users\yueya\Nutstore\1\我的坚果云\graph\image-20220411181253075.png)

   

***

### 静态文件

开发过程中一般将图片，CSS，JS当作静态文件处理

因此如果想在html中插入图片等需要在当前app文件夹下创建一个static文件夹，再将图片放入该文件夹中。一般而言，会在每个app中的static文件夹再创建css，js，img，plugins这四个文件夹：

- CSS
- js：放jQuery文件
- img：放图片
- plugins：放bootstrap文件

静态文件安放的位置有两种

一. app里的static文件夹内

即在每个app内创建一个static文件夹，再将静态文件放入其中。加载静态文件时，django会自动在每个app里搜索static文件夹

其中引入static中的文件有两种方法

1. 直接将该文件的相对或绝对路径写入标签内

   ```html
   <img src="/static/img/bj.jpg">
   ```

2. 在模板中添加一行代码

   ```html
   {% load static %}
   <!--这告诉django模板引擎我们要在模板中使用静态文件，这样便可以使用static模板标签引入静态目录中的文件-->
   
   <img src="{% static 'img/bj.jpg' %}">
   <!-- 
   static 'img/bj.jpg' 告诉django，我们要显示目录名中img/bj.jpg 的图片。static标签会在img/bj.jpg前加上settings.py中STATIC_URL的指定URL，得到static/img/bj.jpg
   -->
   ```

3. 继承的文件中如果需要引入文件并且使用static方式导入也需要在在要继承的文件{% load static %}，即不会继承static

   也可以在settings.py中的TEMPLATES中添加一行代码，这样被继承文件中的static也会自动被要继承的文件所继承

   ```python
   TEMPLATES = [
       {
           'BACKEND': 'django.template.backends.django.DjangoTemplates',
           'DIRS': []
           ,
           'APP_DIRS': True,
           'OPTIONS': {
               'context_processors': [
                   'django.template.context_processors.debug',
                   'django.template.context_processors.request',
                   'django.contrib.auth.context_processors.auth',
                   'django.contrib.messages.context_processors.messages',
               ],
               #'builtins':['django.templatetags.static'] 
           },
       },
   ]
   ```


二. Django中的STATICFILES_DIRS

就是在所有的app文件外面，建立一个公共的文件夹, 也就是STATICFILES_DIRS。因为有些静态文件不是某个app独有的,那么就可以把它放到一个公共文件夹里面，方便管理(注意，建立一个公共的静态文件的文件夹只是一种易于管理的做法，但是不是必须的，app是可以跨app应用静态文件的，因为最后所有的静态文件都会在STATIC_ROOT里面存在) 

那现在的问题是如何让Django知道你把一些静态文件放到app以外的公共文件夹中呢，那就需要配置STATICFILES_DIRS了

***

### 模板语法

本质是HTML中写一些占位符，由数据对这些占位符进行替换和处理

![image-20220411194002463](C:\Users\yueya\Nutstore\1\我的坚果云\graph\image-20220411194002463.png)

模板语法是Django开发的，Django会将模板语法进行渲染和替换，因此用户浏览器是不可能读到含模板语法的html文件，网页的源代码已经被渲染完之后的字符串所替换了

***

### ORM框架

一. 作用

1. 传统操作：MySQL数据库+pymysql

2. Django开发通过ORM框架操作数据库

   ORM起着翻译python代码为数据库语言的作用，而不需要自己写SQL语句

![image-20220412004858967](C:\Users\yueya\Nutstore\1\我的坚果云\graph\image-20220412004858967.png)

ORM本身不能连接数据库还是需要pymysql或者MySQLdB或者MySQLclient，因此首先需要下载pymysql第三方库

二. 用途

ORM可以做两件事：

1. 创建，修改，删除数据库中的表（不用写sql语句）但无法创建数据库
2. 操作表中的数据（不用写sql语句）

三. Django与数据库的操作

settings.py中databases默认的数据库用的是SQLite

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}
```

1. Django与数据库相连

   将上面的代码修改成一下格式

   ```python
   DATABASES = {
   
       'default': {
           'ENGINE': 'django.db.backends.mysql',
           'NAME': 'django_test',
           'USER': 'root',
           'PASSWORD': '980311',
           'HOST': 'localhost',
           'PORT': 3306
       }
   }
   
   ```

2. 在该数据库中创建表

   在models.py中

   每定义的一个模型（类）就相当于在sql中定义一张表，类名就相当于表名

   每个模型（类）都继承django.db.models.model

   模型类中的每个属性相当于数据库的字段

   ```python
   class UserInfo(models.Model):
       name = models.CharField(verbose_name='标题' ,max_length=32) #verbose_name是相当于一个注释，没有实际意义
       password = models.CharField(max_length=64)
       age = models.IntegerField()
       
       account = models.DecimalField(verbose_name='account balance',max_digits=10,decimal_places=2,default=0)
       # max_digits=10代表其位数最多为10位，decimal_places=2代表小数点后有两位
       
       '''
       除了我们自己手动添加的属性之外，django还会自动的生成一个id字段，其类型是bigint，是表中的primary key
       当然也可以自己定义id：
       id = models.BigAutoiField(primary_key=True)
       '''
     
   ```

   ```python
   常用的字段类型：
   AutoField：自动增长的IntegerField，通常不用指定，不指定时Django会自动创建属性名为id的自动增长属性。
   BooleanField：布尔字段，值为True或False。
   NullBooleanField：支持Null、True、False三种值。
   CharField(max_length=字符长度)：字符串。其中max_length必须指定
   TextField：大文本字段，一般超过4000个字符时使用。
   参数max_length表示最大字符个数。
   IntegerField：整数。
   DecimalField(max_digits=None, decimal_places=None)：十进制浮点数。FloatField：浮点数。
   参数max_digits表示总位数。
   参数decimal_places表示小数位数。
   DateField[auto_now=False, auto_now_add=False]：日期。TimeField：时间，参数同DateField。
   参数auto_now表示每次保存对象时，自动设置该字段为当前时间，用于"最后一次修改"的时间戳，它总是使用当前日期，默认为false。
   参数auto_now_add表示当对象第一次被创建时自动设置当前时间，用于创建的时间戳，它总是使用当前日期，默认为false。
   参数auto_now_add和auto_now是相互排斥的，组合将会发生错误。
   DateTimeField：日期时间，参数同DateField。
   FileField：上传文件字段。
   ImageField：继承于FileField，对上传的内容进行校验，确保是有效的图片。
   ```

   

3. terminal中输入

   python manage.py makemigrations

   python manage.py migrate

   这样才能正式地在django_test数据库中添加名为app01_userinfo的表

   除了这张表以外，还有很多其他的表也随之创建，这是因为在settings.py中的INSTALLED_APPS中除了app01外还有很多其他内置元素：

   ![image-20220412012626185](C:\Users\yueya\Nutstore\1\我的坚果云\graph\image-20220412012626185.png)

4. 表中字段的添加和删除

   想要删除表中某个字段可以直接在该模型类中注释或删除该字段对应的属性；

   但是增加一个字段，若直接在该模型类中添加一个属性会出现问题，因为已经存在的字段中有可能存放了数据，如果突然增加一个字段就意味着每条数据都要给新字段分配数值

   因此直接在模型类中添加属性，Django会在终端出现选项：

   - 让我们输入一个值，该值会被赋予给每条数据的新字段
   - 退出放弃操作

   此外，我们可以直接在新添加的属性中设置默认值，此效果和第一个选项一样

   ```python
   data = models.IntegerField(default=2) #默认值为2
   data = models.IntegerField(null=True,blank=True) //默认值为空
   ```

5. 表中数据的增删改查

   filter函数

   ```python
   以下的id可以是表中的任何字段只要是数字类型
   filter(id__gt=12)	#获取id大于12的记录
   filter(id__gte=12)	#获取id大于等于12的记录
   filter(id__lt=12)	#获取id小于12的记录
   filter(id__lte=12)	#获取id小于等于12的记录
   
   以下的mobile可以是表中的任何字段只要是字符串类型
   filter(mobile__startwith='1') 	#获取mobile以1开头的记录
   filter(mobile__endwith='1')	#获取mobile以1结尾的记录
   filter(mobile__contains='1')	#获取mobile包含1的记录
   ```

   order_by函数

   

   - 添加数据

     一般对表中的数据进行增删改查都写在views.py的一个特定函数中

     由于models.py在app01这个文件夹内，因此要记得import

     ```python
     from app01.models import department, UserInfo
     
     def orm_test(request):
         department.objects.create(title='sales')
         department.objects.create(title='IT')
         department.objects.create(title='administration')
         
         UserInfo.objects.create(name='sara',password='896') 
         #由于UserInfo中age属性的默认值为2，所以可以不用添加
     ```

   - 删除数据

     添加过的数据直接注释掉或者移除代码是无法删除的

     ```python
     department.objects.filter(id=3).delete() #删除id=3的这条数据
     department.objects.filter(id=3,title="IT") #删除id=3，title为IT的数据
     #filter()也支持传入字典，上面的写法等价于：
     a = {"title":"IT","id":123}
     department.objects.filter(**a) #传入字典时需要在前面带两个星号，当字典为空时效果相当于all()
     
     department.objects.all().delete() #删除department表中的全部数据
     ```

   - 获取数据

     ```python
     data_list = UserInfo.objects.all()
     print(data_list)
     for obj in data_list:
         print(obj.id,obj.name,obj.password,obj.name)
         
         
     <QuerySet [<UserInfo: UserInfo object (11)>, <UserInfo: UserInfo object (12)>]>
     11 sala 123 2
     12 jess 456 23
     ```

     QuerySet是一个列表，UserInfo里的每条数据就是data_list里面的元素，每个元素都是一个对象

   - 更新数据

     ```python
     UserInfo.objects.filter(id=11).update(id=1)
     UserInfo.objects.filter(id=12).update(id=2)
     UserInfo.objects.filter(id=13).update(id=3）
     ```

6. 表与表的连接

![image-20220412025122600](C:\Users\yueya\Nutstore\1\我的坚果云\graph\image-20220412025122600.png)

```python
from django.db import models

class Department(models.Model):
    """
    the table of department
    """
    
    title = models.CharField(max_length=32)

class Employee(models.Model):
    """
    the table of employees
    """
    
    name = models.CharField(max_length=16)
    password = models.CharField(max_length=64)
    age = models.IntegerField()
    account = models.DecimalField(verbose_name='account balance',max_digits=10,decimal_places=2,default=0)
	create_time = models.DateTimeField(verbose_name = 'entry time')
    
    depart = models.ForeignKey(to = 'Department',to_field = 'id', on_delete = models.CASCADE)
    #理论上，to_filed可以是id也可以是title，开发时一般用id，因为数字比字符的使用的空间少
    #但有些大公司会用title，因为查询的次数非常多，连表操作比较耗时
    
    gender = models.smallInteger(choices=((1,'male'),(2,'female')))
    #我们创建表是为了以后如果有该表中有新的字段可以直接加上去，如果有的表比如性别，它本身的字段只有男和女两种，所以就没有必要  		单独为性别创建一张表；
    #但是，为了节省空间我们又不想直接存储字符串，此时我们可以在内部添加一个choices参数，它是一个元组，该元组的每个元素也是元组      每个元素的元组的第一个值存储数值，第二个值存储字符串。以后向该字段添加数据只能用规定数值而不能用字符串。
    #这样的好处是即不用为该字段重新创建一张表，又可以直接存储对应字符串的数字，而且该数字不能任意必须是已存在于choices中的数字
```

```python
'''
1. to = 'Department': 通过 ForeignKey 将 Department 和 Employee 两表相连
2. to_field = 'id': 此时是将Employee中的depart列与Departmnet中的id列相连，所以以后在Employee中添加depart时只能添加   	    Employee中以有的id，虽然我们定义的是depart但Django在数据库中生成的列名为depart_id，
3. on_delete = models.CASCADE: 一旦我们删除Department中的某个部门比如sales，那么Employee中该部门的员工老奇幻和邱恩婷的整  	个记录也全部删除
4. depart = models.ForeignKey(to='Department', to_field='id', null=True, blank= True, on_delete=models.SET_NULL)
   一旦我们删除Department中的某个部门比如Employee中该部门的员工老奇幻和邱恩婷的depart_id变成空值
'''
```

6. Django中的排序

   ```python
   PhoneNumber.objects.all().order_by("id") #以id的升序排列
   PhoneNumber.objects.all().order_by("-id") #以id的降序排列
   ```

   

***

### 模板的继承

```django
Format.html:

{% load static %}
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <link rel="stylesheet" href="{% static 'plugins/bootstrap-3.4.1/css/bootstrap.css' %}">
</head>
<body>

{% block content %}  

{% endblock %}

<script src="{% static 'js/jquery-3.6.0.min.js' %}"></script>
<script src="{% static 'plugins/bootstrap-3.4.1/js/bootstrap.min.js' %}"></script>

</body>
</html>
```

```django
{% extends 'Format.html' %}

{% block content %}  
	<div>
    <div class="container" style="margin-bottom: 10px">
        <a class="btn btn-success" href="/department/add/" target="_blank"><span class="glyphicon glyphicon-zoom-in" aria-hidden="true"></span> New Department</a>
    </div>

    <div class="container">

        <div class="panel panel-default">
            <div class="panel-heading"><span class="glyphicon glyphicon-list" aria-hidden="true"></span> Department List</div>

            <table class="table table-bordered">
                <thead>
                <tr>
                    <th>ID</th>
                    <th>Departments</th>
                    <th>Manipulations</th>
                </tr>
                </thead>

                <tbody>
                {% for obj in queryset %}
                    <tr>
                        <td>{{ obj.id }}</td>
                        <td>{{ obj.title }}</td>
                        <td>
                            <a href="/department/{{ obj.id }}/delete/" class="btn btn-primary btn-xs">Delete</a>
                            <a href="/department/{{ obj.id }}/edit/" class="btn  btn-danger btn-xs">Edit</a>
                        </td>
                    </tr>
                {% endfor %}
                </tbody>
            </table>
        </div>

    </div>

</div>
{% endblock %}

```

***

### cookie和session

浏览器在访问网址时，发送的都是http或https请求，它是一个无状态的短链接

1. 短链接：浏览器向网址发出一个http或https请求，网址会向浏览器返回一个相应。在每次请求和相应之后，会断开浏览器和网址之间的连接称为短链接。之后浏览器可以再向浏览器发请求，但是网站每次都将其视为新的用户。
2. 无状态：每次都以一个全新的用户身份访问网址的状态称为无状态

因此为了使网站能够辨认出该用户以前曾经登录过此网站，我们引入了cookie和session机制

1. cookie：网站给浏览器返回的字符串，在浏览器中以键值对的形式储存（保存在客户端）

2. session：由于给浏览器发的键值对可以被伪造，因此网站本身在给浏览器返回响应时也会在网站内部开辟一块区域（实际上是存储

   ​                在数据库中）用来存储当前户的信息（保存在服务端）

当浏览器向网站发送请求后，网站要做两件事：

1. 返回响应体：即网站本身的内容
2. 返回响应头（字典，cookie是其中之一）：给当前浏览器返回一个键值对，当浏览器以后访问网站时该键值对也会被自动携带，所以可以通过浏览器携带的键值对判断以前是否访问过该网站；此外，在网站也会为当前用户创建一块区域（session）用来存储用户的信息和记录键值对。当用户下次访问时，不仅需要键值对而且需要将该键值对与之前访问网站时存储的用户信息进行比对，这样才会为当前通过验证的用户显示属于他的数据信息。

由于我们会在定义大量的视图函数，在每个视图函数中都进行cookie和session的用户登录验证会过于复杂，所以我们需要中间件来帮助我们处理

***

### 中间件

当我们访问网站，网站发送请求后，Django并不是直接就调用到该网站所对应的视图函数，在期间还要传过所有的中间件；如果视图函数有return值的话同样也需要穿过所有的中间件才能返回到浏览器

Django中，每个中间件都是一个类，在这些类中都会定义process_request方法和process_response方法，即用户发送的请求会依次执行每个中间件的process_request方法；在return返回时也会倒序地执行每个中间件的process_response方法。

如果在通过某个中间件时它的process_request方法有返回值，则会立刻返回并调用其process_response方法，因而视图函数无法被调用。因此，我们可以在某个中间件的process_request方法内判断当前用户是否登录，如果登录可以继续向后走，如果没有则返回到登录页面

![WeChat Image_20220424000958](C:\Users\yueya\Nutstore\1\我的坚果云\graph\WeChat Image_20220424000958.png)

1. 定义中间件：

   ```python
   from django.utils.deprecation import MiddlewareMixin
   
   
   class M1(MiddlewareMixin):
       def process_request(self, request):
   
       def process_response(self, request, response):
           return response
   
   class M2(MiddlewareMixin):
       def process_request(self,request):
   
       def process_response(self,request,response):
           return response
   ```

2. 在settings.py中的MIDDLEWARE列表中，添加中间件M1，M2的路径

   ```python
   MIDDLEWARE = [
       'django.middleware.security.SecurityMiddleware',
       'django.contrib.sessions.middleware.SessionMiddleware',
       'django.middleware.common.CommonMiddleware',
       'django.middleware.csrf.CsrfViewMiddleware',
       'django.contrib.auth.middleware.AuthenticationMiddleware',
       'django.contrib.messages.middleware.MessageMiddleware',
       'django.middleware.clickjacking.XFrameOptionsMiddleware',
       'app01.middleware.auth.M1',
       'app01.middleware.auth.M2',
   ]
   ```

3. 如果process_request方法中没有返回值或者返回None，则继续向后走

   如果process_request方法中有返回值 HTTPResponse，render，redirect，则不再继续向后执行

***

### Ajax 请求

目前浏览器向网站发送请求是GET和POST请求，它们都是都是通过URL和表单form提交的，特点是每次提交请求后页面都会刷新

通过Ajax发送请求的特点就是页面不会刷新即偷偷地发送请求

一. DOM方式绑定事件

1. GET

   ```javascript
   <input type="button" class="btn btn-primary" value="click" onclick='clickme()'>
   
   {% block js %}
       <script type="text/javascript">
           function clickme(){
               $.ajax({
                   url: '/ajax/test2/',
                   type: 'get',
                   data: {
                       n1:123,
                       n2:456,
                   },
                   success: function (res){
                       console.log(res);
                   }
   
               })
           }
       </script>
   {% endblock %}
   ```

   ```python
   def test_ajax(request):
       return render(request,'_12test_ajax.html')
   
   def test_ajax2(request):
       print(request.POST)
       return HttpResponse('request by Ajax')
   ```

2. POST

   ```javascript
   <input type="button" class="btn btn-primary" value="click" onclick='clickme()'>
   
   {% block js %}
       <script type="text/javascript">
           function clickme(){
               $.ajax({
                   url: '/ajax/test2/',
                   type: 'post',
                   data: {
                       n1:123,
                       n2:456,
                   },
                   success: function (res){
                       console.log(res);
                   }
   
               })
           }
       </script>
   {% endblock %}
   ```
   
   ```python
   def test_ajax(request):
       return render(request,'_12test_ajax.html')
   
   #当前我们是以POST方式发送的请求，所以需要csrf_token，由于我们此时是在用Ajax而不是表单，因此可以在函数前添加@csrf_exmept来免除认证
   from django.views.decorators.csrf import csrf_exempt
   @csrf_exempt
   def test_ajax2(request):
       print(request.POST)
       return HttpResponse('request by Ajax')
   
   ```

二. JQuery方式绑定事件

1. GET

   ```javascript
   <input id="btn1" type="button" class="btn btn-primary" value="click">
   
   {% block js %}
       <script type="text/javascript">
           $(function () {
   
               $('#btn1').click(function () {
                   $.ajax({
                       url: '/ajax/test2/',
                       type: 'POST',
                       data: {
                           n1: 123,
                           n2: 456,
                       },
                       success: function (res) {
                           console.log(res);
                       }
                   })
               })
   
           })
   
       </script>
   {% endblock %}
   ```

   

***

## 实战

### 易漏点

- 忘记安装 mysqlclient第三方库

- models.py中定义类是继承的是models.Model

- urls.py中URLpatterns，在path中调用views.py中的函数首先要记得import，

  path()函数的第一个参数访问网址开头不用加/，第二个参数调用html文件时不需要加引号

- redirect中的网址以/开始以/结尾，在其他任何地方设置超链接时的网址都/开始以/结尾

- 当给models.py的class中的字段修改之后需要更新数据，即重新输入那两行命令:

  python manage.py makemigrations

  python manage.py migrate
  
- 连接数据库时，user：root和passoword都需要小写

***

### 给服务器传参数有两种方法

1. ```python
   在html中：
   <a href="/department/delete/?nid={{ obj.id }}" class="btn btn-primary btn-xs">Delete</a>
   #直接在网址后添加以问号开始的键值对，然后该参数就会传递到该网址中
   
   在urls.py中:
   path('department/delete/',views.department_delete)
   #通过该网址访问对应的视图函数
   
   在views.py中：
   def department_delete(request):
       nid = request.GET.get('nid')
       Department.objects.filter(id=nid).delete()
       return redirect('/department/list/')
   #通过request.Get.get('nid')获取该键对应的值
   #再通过python对该值所在的表进行数据操作
   #最后重新定向到初始网址中
   ```

2. ```python
   在html中：
   <a href="/department/{{ obj.id }}/edit/" class="btn  btn-danger btn-xs">Edit</a>
   #通过正则表达式传递参数
   
   在urls.py中:
   path('department/<int:nid>/edit/',views.department_edit)
   #此时的网址格式也要相对应，再访问对应的视图函数
   
   在views.py中：
   def department_edit(request,nid):
       first_obj = Department.objects.filter(id=nid).first()
       return render(request,'department_edit.html',{'first_obj':first_obj})
   #此时的参数要通过函数接收
   ```

***

### 特殊字段的定义和获取方式

```python
class Employee(models.Model):
    name = models.CharField(max_length=16)
    password = models.CharField(max_length=64)
    age = models.IntegerField()
    account = models.DecimalField(verbose_name='account balance',max_digits=10,decimal_places=2,default=0)
    entry_time = models.DateTimeField(verbose_name='entry time')
```

1. choices

   - 在python中获取display_name

     ```python
     gender_choices = ((1,'male'),(2,'female')) #gender_choices没有明确数据类型，所以它只是一个变量而不是字段
     gender = models.SmallIntegerField(choices=gender_choices)
     #在字段中可以设置choices参数，choices参数必须是一个元组（保证值不可变），该元组的每个值也是一个元组（value,display_name）
         
     #我们实例一个Employee对象obj，则obj.gender就可取到value，obj.get_gender_display() 就可取到 display_name，通过get_字段名_display()即可取display_name
     ```

   - 在html中获取display_name

     由于模板语言中不可以出现括号，需要将方法的括号去掉，Django读取时会自动加上

     ```django
     {{ obj.get_gender_display }}
     ```

2. ForeignKey

   ```py
   department = models.ForeignKey(to='Department',to_field='id', on_delete=models.CASCADE)
   #对于表与表间相关联的字段department，数据库中生成的是department_id,因此我们在html中使用模板语言{{obj.department_id}}获取的是另一张表的与该obj的department所对应的部门id
   #使用模板语言{{obj.department.title}}:
    首先{{obj.department}}获取了当前obj的department_id在另一张表中相对应的那一行数据，{{obj.department.title}}便得到了当前哪一行数据的title
   ```

   ![WeChat Image_20220415145542](C:\Users\yueya\Nutstore\1\我的坚果云\graph\WeChat Image_20220415145542.png)

3. Datetime

   ```python
   entry_time = models.DateField(verbose_name='入职时间')
   ```

   ```html
   {{ obj.entry_time }}
   
   March 11, 2023
   
   {{ obj.entry_time|date:"m-d-y" }}
   
   03-11-23	
   ```

   

***

### 在用户页面添加数据（user_add)

1. 原始方法

   缺点：

   - 如果提交的数据为空没有错误提示
   - 在函数中，如果是POST方法则需要接收用户输入的所有值，因此需要给每个字段都创建一遍
   - 关联后的数据得在html中循环

   ```python
   def user_add(request):
       if request.method == 'GET':
           content={
               'gender_choices':Employee.gender_choices,
               'department_list': Department.objects.all()
           }
           return render(request,'_5user_add.html',content)
   
       username = request.POST.get('username')
       password = request.POST.get('pwd')
       age = request.POST.get('age')
       account = request.POST.get('ab')
       entry_time = request.POST.get('et')
       department_id = request.POST.get('dp')
       gender_id = request.POST.get('gd')
   
       Employee.objects.create(name=username,
                               password=password,
                               age=age,account=account,
                               entry_time=entry_time,
                               department=Department.objects.get(id=department_id), #注意传入的是对象而不是参数
                               gender=gender_id)
   
   
       return redirect('/user/list/')
   ```

   ```django
   <div class="form-group">
       <label class="col-sm-2 control-label">Department</label>
       <div class="col-sm-10">
           <select class="form-control">
               {% for item in department_list %}
                   <option value="{{ item.id }}">{{ item.title }}</option> 
               {% endfor %}
           </select>
       </div>
   </div>
   
   <div class="form-group">
       <label class="col-sm-2 control-label">Gender</label>
       <div class="col-sm-10">
           <select class="form-control">
               {% for item in gender_choices %}
                   <option value="{{ item.0 }}">{{ item.1 }}</option>
               {% endfor %}
           </select>
       </div>
   </div>
   ```

2. Form组件

   ```python
   class MyForm(Form):
       user = forms.CharField(widget=forms.Input)  #这会自动在html中生成input的标签
       pwd = forms.CharField(widget=forms.Input)
       email = forms.CharField(widget=forms.Input)
       
   def user_add_form(request):
       if request.method == 'GET':
           form = MyForm()
           return render(request,'user_add_form.html',{"form":form})
   ```

   ```html
   <form method = 'POST'>
       {% csrf_token %}
       
       {% for field in form %}
       	{{ field }}
       {% endfor %}
   </form>
   ```

3. ModelForm

   ```python
   class MyForm(ModelForm):
       class Meta:
           model = Employee
           fields = "__all__"  #将Employee表中的所有的字段都存入到fields中,而且每个字段会自动在html中生成自己对应的标签
          	'''
          	  其效果类似于：
          	  fields = ['name','password','age','account','entyry_time','department','gender']
          	  注意部门字段要写department而不能写department_id,
          	'''
           
           widgets = { 
               "name" : forms.TextInput(attrs={"class":"form-control"}）
       	   "password" : forms.TextInput(attrs={"class":"form-control"})
               ...
                                        
               "department": forms.Select(attrs={"class": "form-control"}),
               "gender": forms.Select(attrs={"class": "form-control"})
               #注意，由于department和gender的标签是select所以不能用TextInput         
         	}
              # 通过widgets给字段的input标签分配class属性     
                                        
           
   def user_add_form(request):
       if request.method == 'GET':
           form = MyForm() #第一次通过GET方式提交的的form没有任何数据
           return render(request,'_5user_add_form.html',{"form":form})
       form = MyForm(data=request.POST) #第二次通过POST方式提交的form包含了提交的数据及错误信息
     	if form.is_valid():
     		form.save()
            return redirect(request,'user/list/')
       else:
            return render(request,'_5user_add_form.html',{"form":form})
                                      
   ```
   
   ```python
   class Department(models.Model):
       title = models.CharField(max_length=32)
       
       def __str__(self):
           return self.title
       #由于fields中部门字段中我们写的是department而不能写department_id，因此在html的{{field}}中department显示的选项是对象而不是IT和SALES
       #我们设置__str__(self)魔法函数可以达到打印某一个实例得到的是其函数中定义的返回值的效果
   ```
   
   ```html
   <form class="form-horizontal" method="POST" novalidate> <!--浏览器会自动帮我们验证输入框内有没有错误信息,通过															novalidate 关闭-->
       {% csrf_token %}
       
       {% for field in form %} <!--此时form中的每一个对象都是Employee表中的一个字段-->
       	<div class="form-group">
       	{{ field.label }}:{{field}} 
            <!--{{ field.label }}显示Employee每个字段中的verbose_name,如果没定义verbose_name则显示的是字段名-->
            <!--{{field}}的用处是生成相对应的标签,比如字段name对应的是TextInput,那么此时会生成一个文字输入框；gender对		       应的是select，那么此时会生成一个选择框 -->
            <span style="color:red">{{field.error.0}}</span>
            <!--field.error是一个列表，其中每个元素代表着该字段的错误信息，只需要显示第一个错误就可以了 -->
   		</div>		
       {% endfor %}
   </form>
   
   
   最终效果为：
   <form class = "form-horizontal" method = 'POST'>
        <div class="form-group">
           <label>姓名</label> <input type="text" name="password" maxlength="64" required="" id="id_password">
       </div>
       
      ...
       
       
       
   </form>
   
   ```
   
   第一次：
   
   当第一访问user/add/modelform/这个网址时，由于是通过GET方式访问，因此返回的是_5user_add_form.html这个html页面，由于此时form中没有任何数据，所以此时传入的form在该html页面中的模板语法不起作用
   
   第二次：
   
   当我们添加数据并提交时，是通过POST方式访问
   
   如果添加的数据有效，则定向到user/list/这个页面
   
   如果添加的数据无效，此时再返回到_5user_add_form.html这个html页面，由于此时的form有输入的数据及错误数据，模板语法会显示数据错误原因

***

### 给当前用户编辑数据（user_edit)

```python
from django import forms

       #通过modelForm添加数据
class MyForm(forms.ModelForm):
    class Meta:
        model = Employee
        # fields = "__all__"
        fields = ["name","password","age","account","entry_time","department","gender"]

        widgets = {
            "name" : forms.TextInput(attrs={"class":"form-control"}),
            "password": forms.TextInput(attrs={"class": "form-control"}),
            "age": forms.TextInput(attrs={"class": "form-control"}),
            "account": forms.TextInput(attrs={"class": "form-control"}),
            "entry_time": forms.TextInput(attrs={"class": "form-control"}),
            "department": forms.Select(attrs={"class": "form-control"}),
            "gender": forms.Select(attrs={"class": "form-control"})
        }

def user_add_modelform(request):
    if request.method == 'GET':
        form = MyForm()
        return render(request,'_5user_add_modelform.html',{"form":form})

    form = MyForm(data=request.POST)
    if form.is_valid():
        form.save()
        return redirect(request,'/user/list/')
    else:
        return render(request,'_5user_add_modelform.html',{"form":form})


```

```python
def user_edit(request,nid):
    current_obj = Employee.objects.filter(id=nid).first()

    if request.method == 'GET':
        form = MyForm(instance=current_obj) #instance = current_obj相当于在标签内写value的效果，它会自动给每个输入框填入										 当前对象的属性
        return render(request,'_6user_edit.html',{'form':form})

    form = MyForm(data=request.POST, instance=current_obj) #在MyForm内添加参数instance = current_obj使得新填入的数据是														  来的基础上更新而不是添加
    if form.is_valid():
        form.save() #form.save()是用来添加数据
        return redirect('/user/list/')
    return render(request,'_6user_edit.html',{'form':form})
```

***

### 展示数据库内的手机号记录（phone_number）

1. 搜索

   不同于之前的展示部门和用户的记录，我们添加了一个搜索功能，用于展示符合条件的手机号。

   当在输入框内输入数据时，得到的是符合条件的手机号记录；如果没有输入数据或者输入的数据为空，得到的是全部的手机号记录

   ```python
   <div class="col-lg-6" style="float: right; width: 300px">
       <form method="GET">
           <div class="input-group">
               <input type="text" name="input" class="form-control" placeholder="Search">
               <span class="input-group-btn">
                   <input class="btn btn-default" type="submit"></input>
               </span>
           </div>
       </form>
   </div>
   ```

   ```python
   # 过滤1：搜索
   def phone_number(request):
       filter = request.GET.get('input')
       filter_list = {}
       if filter:
           filter_list["mobile__contains"] = filter
       phone_list = PhoneNumber.objects.filter(**filter_list) 
       #如果filter_list为空，则filter()里面的参数也为空，此时相当all()的效果
       
       return render(request,'_7phone_number.html',{'phone_list':phone_list})
   ```

2. 分页

   ```python
   #过滤2：分页
   from django.utils.safestring import mark_safe
   def phone_number(request):
       #获取当前用户点击的页面，如果没有点击即原页面则默认获取第一页
       page = int(request.GET.get('page', 1))  
   	
       #规定每页显示多少条数据
       num_per_page = 10
       phone_list = PhoneNumber.objects.all()[(page - 1) * num_per_page:page * num_per_page]
   
       #获取数据库中数据的总条数
       total = PhoneNumber.objects.all().count()
       
       #动态创建在网页生成li标签的代码，并将一组li标签代码作为一个元素添加到一个空列表中
       page_str_list = []
       
       #添加上一页
       if page > 1:
           pre = f'<li><a href="?page={page-1}">Previous</a></li>'
       else:
           pre = f'<li><a href="?page={page}">Previous</a></li>'
       page_str_list.append(pre)
       
       #添加页面
      	half_scope = 3  #设定以当前page为中心，half_scope为半径所展示的页面数
       page_number = total//num_per_page + 1 #总页数
       if page_number <= 2*half_scope: #如果总页数小于等于要展示的页面数，则全部显示
           start_page = 1
           end_page = page_number
       else: #如果总页数多于要展示的页面数，则分批展示
           if page <= half_scope: #如果当前页在从前往后数的half_page页内，则开始页为1
               start_page = 1
               end_page = 2*half_scope+1  #
           else: 
               if page+half_scope>=page_number: #如果当前页在从后往前数的half_page页内，结束页为总页数
                   start_page = page_number-2*half_scope
                   end_page = page_number
               else:
                   start_page = page-half_scope
                   end_page = page+half_scope
      for i in range(start_page, end_page + 1):
          if i == page:
   	       ele = f'<li class="active"><a href="?page={i}">{i}</a></li>'
   	   else:
   		   ele = f'<li><a href="?page={i}">{i}</a></li>'
          page_str_list.append(ele)
       
       #添加下一页
       if page == (total // num_per_page + 1):
           Next =  f'<li><a href="?page={page}">Next</a></li>'
       else:
           Next =  f'<li><a href="?page={page+1}">Next</a></li>'
       page_str_list.append(Next)
   
   	#列表中的元素拼接成一个字符串，并将这个字符串返回到html文件中，在django模板语法的渲染下生成网页
       page_string = mark_safe("".join(page_str_list))
       return render(request, '_7phone_number.html', {"page_string": page_string, 'phone_list': phone_list})
   ```

3. 搜索+分页：

   ```python
   from app01.utils.Pagination import Pagniation
   
   def phone_number(request):
       # 首先搜索即过滤出符合条件的数据
       filter_dict = {}
       filter = request.GET.get('input')
       if filter:
           filter_dict["mobile__contains"] = filter
       phone_list = PhoneNumber.objects.filter(**filter_dict).order_by("-level")
    	
       #过滤后对Pagniation类进行实例化,通过分页对象可以得到过滤后的数据集page_phone_list和生成一系列<li>标签的字符串		page_obj_string
       page_obj = Pagniation(request,phone_list)
       page_phone_list = page_obj.page_phone_list
       page_obj_string = page_obj.html()
    
   	#将实例的属性page_phone_list和page_string通过render传入html中
       return render(request,'_7phone_number.html', {
           "filter":filter, 
           "page_string": page_obj.page_string, 
           'page_phone_list': page_obj.page_phone_list})
   ```

   ```python
   import copy
   from django.http.request import QueryDict
   
   class Pagniation(object):
       def __init__(self,request,phone_list,page_param='page',num_per_page=10, half_scope=3):
           
           #我们希望的是在这个input键值对后面跟上一个page键值对以达到过滤数据后分页的效果，例如：
           # http://127.0.0.1:8000/phone/number/?input=27&page=8，这个网址就代表着我们对数据过滤的条件是Input=27，然后  			再此基础上，再跳转到第8页
           # 由于在 django.http.request.py这个文件中有一个名为QueryDict(MultiValueDict)的class，该类的__init__()方法中		   的参数mutable默认设置了False。我们将其改为True后，才可以使用setlist()函数将网址中对参数深拷贝后的值与要添加的           值拼接起来
           # 因此首先我们要深拷贝一个当前网址上的当前input的键值对，这个input键是search所在input标签中的属性name，
           query_dict = copy.deepcopy(request.GET)
           query_dict._mutable = True
           self.query_dict = query_dict
           self.page_param = page_param
   
           # 然后我们要得到当前用户想跳转的page，如果没有则默认为第一页。另外我们需要对用户输入的值
           page = request.GET.get(page_param,"1")
           if page.isdecimal():
               page = int(page)
           else:
               page = 1
   
           self.page = page
           self.num_per_page = num_per_page
   
           self.start = (page-1)*num_per_page
           self.end = (page)*num_per_page
   
           self.page_phone_list = phone_list[self.start:self.end] #传进来一个过滤搜索后的phone_list，然后返回去一个分															 好页的phone_list
           total = phone_list.count()
           self.total = total
   
           page_number = total // num_per_page + 1
           self.page_number = page_number
           self.half_scope = half_scope
   
       def html(self):
           from django.utils.safestring import mark_safe
   
           # self.query_dict.setlist(self.page_param,[1])
   
   
           page_str_list = []
           # 添加上一页
           if self.page > 1:
               self.query_dict.setlist(self.page_param,[self.page-1])
               pre = f'<li><a href="?{self.query_dict.urlencode()}">Previous</a></li>'
           else:
               self.query_dict.setlist(self.page_param,[1])
               pre = f'<li><a href="?{self.query_dict.urlencode()}">Previous</a></li>'
           page_str_list.append(pre)
   
   
           if self.page_number <= 2 * self.half_scope:
               start_page = 1
               end_page = self.page_number
           else:
               if self.page <= self.half_scope:
                   start_page = 1
                   end_page = 2 * self.half_scope + 1  #
               else:
                   if self.page + self.half_scope >= self.page_number:
                       start_page = self.page_number - 2 * self.half_scope
                       end_page = self.page_number
                   else:
                       start_page = self.page - self.half_scope
                       end_page = self.page + self.half_scope
   
           for i in range(start_page, end_page + 1):
               self.query_dict.setlist(self.page_param, [i])
               if i == self.page:
                   ele = f'<li class="active"><a href="?{self.query_dict.urlencode()}">{i}</a></li>'
               else:
                   self.query_dict.setlist(self.page_param,[i])
                   ele = f'<li><a href="?{self.query_dict.urlencode()}">{i}</a></li>'
               page_str_list.append(ele)
   
           # 添加下一页
           if self.page == (self.total // self.num_per_page + 1):
               self.query_dict.setlist(self.page_param,[self.page_number])
               Next = f'<li><a href="?{self.query_dict.urlencode()}">Next</a></li>'
           else:
               self.query_dict.setlist(self.page_param,[self.page+1])
               Next = f'<li><a href="?{self.query_dict.urlencode()}">Next</a></li>'
           page_str_list.append(Next)
   
           self.page_string = mark_safe("".join(page_str_list))
           return self.page_string
   ```

   

在 django.http.request.py这个文件中有一个名为QueryDict(MultiValueDict)的class，该类的__init__()方法中的参数mutable默认设置了False。
我们将其改为True后，就可以使用setlist()函数将网址中对参数深拷贝后的值与要添加的值拼接起来

```python
query_dict = copy.
```



***

### 给手机号添加记录(phone_add)

不同于之前的给部门和用户添加记录，由于给手机号添加记录需要不仅需要验证用户输入不为空（通过is_valid())，而且需要验证手机号的格式，位数是否满足要求，以及添加的手机号在数据库中是否已经存在。
因此以下是有条件限制地添加记录

1. 限制输入手机号的格式

   - 方法一：正则表达式

     ```python
     from django.core.validators import RegexValidator
     
     class MyForm(forms.ModelForm):
     
         mobile = forms.CharField(
             label='Mobile',
             validators=[RegexValidator(r'^1\d{10}$','wrong number')], #以1开头，之后的数字必须是10个，否则会报错
             widget=forms.TextInput(attrs={"class":"form-control"})
             
             """
             还可以设置其他的功能比如说disabled=True使得mobile不可修改
             """
         )
     
     # 也就是说，每个字段都可以在ModelForm中进行设置来规定用户输入的要求
     # 由于在这里单独给mobile进行了设置，里面有了一个widget参数，因此在之后的widget中就不必设置了
     
         class Meta:
             model = PhoneNumber
             fields = '__all__'
             widgets={
                 #由于单独在上面的mobile中设置了widget，所以mobile的输入框遵从自己内部参数的设置，哪怕我们在这里给mobile              设置了不同的标签
                 "price": forms.TextInput(attrs={"class":"form-control"}),
                 "level": forms.Select(attrs={"class":"form-control"}),
                 "status": forms.Select(attrs={"class":"form-control"})
             }
     
     def phone_add(request):
         if request.method == 'GET':
             form = MyForm()
             return render(request,"_8phone_add.html",{'form':form})
         form = MyForm(data=request.POST)
         if form.is_valid():
             form.save()
             return redirect('/phone/number/')
         return render(request,'_8phone_add.html',{'form':form})
     ```

   - 方式二

     ```python
     from django.core.exceptions import ValidationError
     
     class MyForm(forms.ModelForm):
             class Meta:
             model = PhoneNumber
             fields = '__all__'
             widgets={
                 "mobile": forms.TextInput(attrs={"class": "form-control"}),
                 "price": forms.TextInput(attrs={"class":"form-control"}),
                 "level": forms.Select(attrs={"class":"form-control"}),
                 "status": forms.Select(attrs={"class":"form-control"})
             }
     
             # 当我们定义了字段后会自动生成一个clean_字段名的方法
             def clean_mobile(self):
                 txt_mobiile = self.cleaned_data["mobile"] #self.cleaned_data获取了用户输入的所有值并存到字典中，设				              						    置键用来获取其对应的值
                 if len(txt_mobiile) != 11:
                     raise ValidationError('wrong format')
                 return txt_mobiile
     ```

2. 限制输入重复的手机号

   由于新输入的手机号要与数据库中的手机号做比对，因此只能用方法二而无法用正则表达式

   ```python
   class MyForm2(forms.ModelForm):
   
       class Meta:
           model = PhoneNumber
           fields = '__all__'
           widgets={
               "mobile": forms.TextInput(attrs={"class": "form-control"}),
               "price": forms.TextInput(attrs={"class":"form-control"}),
               "level": forms.Select(attrs={"class":"form-control"}),
               "status": forms.Select(attrs={"class":"form-control"})
           }
   
   
       def clean_mobile(self):
           new_mobile = self.cleaned_data["mobile"] 
           if len(new_mobile) != 11 or PhoneNumber.objects.filter(mobile=new_mobile).exists(): #用exists()方法来判断
               raise ValidationError('wrong format')
           return new_mobile
   
   ```

3. 分页效果

   ```python
   from django.utils.safestring import mark_safe
   def phone_number(request):
   
       page = int(request.GET.get('page'))
       total = PhoneNumber.objects.all().count()
       print(total)
       num_per_page = 10
       phone_list = PhoneNumber.objects.all()[(page-1)*num_per_page:page*num_per_page]
   
       page_str_list=[]
       for i in range(1,total//num_per_page+2):
           ele = f'<li><a href="?page={i}">{i}</a></li>'
           page_str_list.append(ele)
       page_string = mark_safe("".join(page_str_list)) #mark_safe()让一系列由li标签构成的字符串在html中变成标签
   
       return render(request, '_7phone_number.html',{"page_string":page_string, 'phone_list': phone_list})
   ```

   ```html
   {% extends 'Format.html' %}
   
   {% block content %}
   
       <div class="container clearfix">
   
           <div class="container" style="margin-bottom: 10px; padding-left: 0">
               <a class="btn btn-success" href="/phone/add/" target="_blank"><span class="glyphicon glyphicon-zoom-in" aria-hidden="true"></span> New Phone</a>
   
               <div class="col-lg-6" style="float: right; width: 300px">
                   <form method="get">
                       <div class="input-group">
                           <input type="text" name="input" class="form-control" placeholder="Search for...">
                           <span class="input-group-btn">
                               <input class="btn btn-default" type="submit"></input>
                           </span>
                       </div>
                   </form>
               </div>
   
           </div>
   
           <div class="panel panel-default">
   
               <div class="panel-heading"><span class="glyphicon glyphicon-list" aria-hidden="true"></span> Department List</div>
   
               <table class="table table-bordered">
   
                   <thead>
                   <tr>
                       <th>ID</th>
                       <th>Mobile</th>
                       <th>Price</th>
                       <th>Level</th>
                       <th>Status</th>
                       <th>Manipulations</th>
                   </tr>
                   </thead>
   
                   <tbody>
                   {% for obj in phone_list %}
                       <tr>
                           <td>{{ obj.id }}</td>
                           <td>{{ obj.mobile }}</td>
                           <td>{{ obj.price }}</td>
                           <td>{{ obj.get_level_display }}</td>
                           <td>{{ obj.get_status_display }}</td>
                           <td>
                               <a href="/phone/{{ obj.id }}/delete/" class="btn btn-primary btn-xs">Delete</a>
                               <a href="/phone/{{ obj.id }}/edit/" class="btn  btn-danger btn-xs">Edit</a>
                           </td>
                       </tr>
                   {% endfor %}
                   </tbody>
   
               </table>
   
           </div>
   
           <nav aria-label="...">
               <ul class="pagination">
   
                   {{ page_string }}
               </ul>
   
           </nav>
   
       </div>
   
   {% endblock %}
   ```







***

### 给当前手机号编辑数据(phone_edit)

不同于之前的给部门和用户编辑记录，由于给当前手机号编辑信息需要不仅需要验证用户输入不为空（通过is_valid())，而且需要验证手机号的格式，位数是否满足要求，以及编辑后的手机号在数据库中是否已经存在。
因此以下是有条件限制地编辑数据







总结：

定义MyForm类并继承forms.ModelForm的意义在于，在MyForm类中通过model关联数据表，fields关联数据表中要管理的字段，再通过实例化 form = MyForm()让该数据表中每个字段都成为form对象中的元素

添加用户和编辑当前用户信息都可以用到ModelForm,因此需要在视图函数前定义类，并且它们可以共用一个MyForm
一旦编辑与添加共用一个MyForm，那在MyForm中的约束也相同，例如：
如果在MyForm中对添加约束了输入的手机号不能与数据库内已有的号码重复，那么编辑更新数据号时也会受影响因为当前编辑的手机号已经存在于数据库中，如果我们只是想修改该手机号的price, level等那么提交数据也会失败。

区别在于

1. 编辑当前用户信息需要传递当前用户的一个属性（比如id）作为参数到网页地址中，然后通过ORM框架获取到当前用户对象的所有字属性；但是添加用户不需要
2. 通过GET请求时，编辑当前用户信息如果想在编辑页面自动在各个输入框显示当前用户的属性，需要在实例化中添加参数：instance=当前用户对象
3. 通过POST请求时，编辑当前用户信息和添加用户都需要在实例化中添加参数：data=request.POST 用来获取用户输入的值；但是编辑当前用户信息仍需要添加参数：instance=当前用户对象 用来更新数据而不是添加数据因为save()方法只有添加数据的功能

***

### 用户登陆（login）

1. 在调用单个视图函数前做用户登录验证

   ```python
   def admin_list(request):
   
       #首先检查用户是否已经登录，如果未登录则跳转到登录页面
       #如果用户发来登录请求，则网站获取当前用户cookie的随机字符串并在session中查看是否有匹配的字符串，如果有则说明该用户以前登陆过
       info = request.session.get("info")
       if not info:
           return redirect('/login/')
       
       #以下是之前写的搜索和分页功能，不算在登录功能之中
       # filter = request.GET.get('input', '')
       # filter_dict = {}
       # if filter:
       #     filter_dict['admin_name__contains'] = filter
       # 
       # manager = Administration.objects.filter(**filter_dict)
       # 
       # page_obj = Pagniation(request, manager)
       # manager_list = page_obj.page_phone_list
       # page_string = page_obj.html()
       # return render(request, '_10admin_list.html', {"manager_list": manager_list, "page_string": page_string})
   
   class MyForm5(forms.Form):
       admin_name = forms.CharField(
           widget=forms.TextInput(attrs={"class": "form-control"}),
           required=True
       )
       password = forms.CharField(
           widget=forms.PasswordInput(attrs={"class": "form-control"}),
           required=True
       )
   
       def clean_password(self):
           password_input = self.cleaned_data.get('password')
           return md5(password_input)
       
   def login(request):
       if request == 'GET':
           form = MyForm5()
           return render(request,'_11login.html',{"form":form})
       form = MyForm5(data=request.POST)
       if form.is_valid():
           form.cleaned_data
           
           admin_obj = Administration.objects.filter(**cleaned_data).first()
           
           if not admin_obj:
               form.add_error("password","username or password is incorrect")
               return render(request,'_11login.html',{"form":form})
           request.session["info"] = {'id':admin_obj.id, 'name':admin_obj.admin_name}
           
           return redirect('/admin/list/')
       
       return render(request,'_11login.html',{"form":form})
   ```

2. 通过中间件在所有的视图函数调用前都进行用户验证

   ```python
   from django.utils.deprecation import MiddlewareMixin
   from django.shortcuts import render, HttpResponse, redirect
   
   class M1(MiddlewareMixin):
       def process_request(self, request):
           #首先我们需要排除哪些不需要验证登录就能访问的页面
           request.path_info #获取当前用户请求的URL
           if request.path_info == '/login/':
               return
           #再读取当前访问用户的session信息，如果能读取说明已经登录
           info = request.session.get("info")
           if info:
               return
           return redirect('/login/')
   
       def process_response(self, request, response):
           return response
   ```

   ```python
   #中间件仅是判断能否获取当前用户的session即是否登录，而没有判断数据库中有无匹配的数据，所以login函数仍然需要。但不需要admin_list函数内再进行验证，因为只有验证成功的用户Django才能调用admin_list函数并返回admin_list页面
   def login(request):
       if request == 'GET':
           form = MyForm5()
           return render(request,'_11login.html',{"form":form})
       form = MyForm5(data=request.POST)
       if form.is_valid():
           form.cleaned_data
           
           admin_obj = Administration.objects.filter(**cleaned_data).first()
           
           if not admin_obj:
               form.add_error("password","username or password is incorrect")
               return render(request,'_11login.html',{"form":form})
           request.session["info"] = {'id':admin_obj.id, 'name':admin_obj.admin_name}
           
           return redirect('/admin/list/')
       
       return render(request,'_11login.html',{"form":form})
   ```

   - 当前用户未登录

     如果当前用户在访问/admin/list/网站，经过中间件时由于当前网站不是/login/所以立即返回到/login/网站，由于/login/网站不需要验证信息所以Django直接调用login视图函数。由于当前是以GET请求发送请求返回的页面

     当用户输入用户名和密码后提交，如果输入的格式正确则

***

### 导入时间模块

1. 普通方式创建的表单：

   需要在输入时间的那一个Input标签中定义一个id，然后再通过js中的定义达到页面上呈现时间标签的效果

   ```python
   <div class="form-group">
   	<label class="col-sm-2 control-label">Entry Time</label>
       	<div class="col-sm-10">
           	<input id='dt' type="text" class="form-control" placeholder="Entry Time" name="et">
           </div>
   </div>
   ```

   ```javascript
   <script>
       $(function (){
           $('#dt').datetimepicker({  
               format: 'yyyy-mm-dd',
               startDate: '0',
               language: "English",
               autoclose: true
           })
       }
       )
   </script>
   ```

   

2. 通过ModelForm创建的表单：

   通过ModelForm来创建的表单，会自动给每个字段生成一个id: id_字段名

   ```javascript
   <script>
       $(function (){
           $('#id_entry_time').datetimepicker({  
               format: 'yyyy-mm-dd',
               startDate: '0',
               language: "English",
               autoclose: true
           })
       }
       )
   </script>
   ```

***

### 问题

在编辑中：对于数据库中该id目前的密码，如果对其修改并仍使用原来密码，不会报错

在reset中：如果新输入的密码和再次确认输入的密码不一样不会报错并且password和confirm Your password消失

