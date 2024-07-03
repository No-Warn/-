# 初识Django模块

## 模型层简介
1：模型层是什么

**从数据库中取出数据，再将数据在浏览器中渲染出来，模型层就是位于视图和数据库中间的一层**

2:为什么需要模型层

可以屏蔽数据库之间的差异

可以使开发者更加专注于逻辑业务的开发

可以提供很多便携有助于开发

3：模型层相关的配置

**在项目文件的settings.py文件中**

其中包含了项目配置的很多参数，包括数据库等参数的配置


# 初始django shell

1：django shell是什么

python shell就是用于交互式的python编程，通过在控制台上输入python脚本可以直接执行

django shell与python shell很类似，继承django的环境，可以使用项目中的相关变量等


2：为什么需要django shell

临时操作使用django shell更加方便

小范围内debug更加简单，不需要运行整个项目来进行测试

3：django shell的使用

新建一篇文章

~~~ shell
# 运行以下脚本执行进入django shell环境
python manage.py shell
~~~


# 快速上手

**使用vscode创建django项目**

~~~python
django-admin startproject 项目名称
~~~

**创建app**

~~~ python
python manage.py startapp 应用名称
~~~

使用pycharm创建django项目需要注意，pycharm需要是专业版，并且在创建项目之后需要将根目录生成的templates目录删除，在settings文件中需要删除templates设置中**BASE_DIR / 'templates'**



1:在views.py中编写所需要的函数，以及函数的返回值 (views.py文件就是保存需要进行的操作)

2：在urls.py中将url与函数操作进行绑定(用户输入url转到views.py文件中执行相关操作函数返回预期结果)

3：在项目的urls.py中将app与url进行绑定（面对多个app的编写有必要进行相关操作）

4：在settings.py文件中注册app


# templates模板

1：在views.py文件中如果只想返回字符串则可以使用HttpResponse进行返回

~~~ python
# 在这里编辑函数以及相对应的操作
def helloworld(request):
    return HttpResponse("hello world")
~~~

2：如果向返回HTML文件的话则需要return一个render


~~~ python
# 默认会在app目录下寻找templates目录，然后再在其中寻找所指向的html文件，这里所指向的文件为index.html
def index(request):
    return render(request,"index.html")
~~~

# 静态文件

在app目录下创建static文件夹,将所有静态文件放入static文件夹中

在开发过程中一般将图片和css和js等分文件编辑，都当作静态文件处理

**静态文件不能乱放，必须放在static目录下**

首先得确认在settings.py文件中确定规定了静态文件的目录
~~~ python
# 配置静态文件的目录
STATIC_URL = '/static/'
~~~

在html中引用静态文件时，该文件必须在static目录下,一般在static文件夹下会有css js img plugins等目录,用来存放不同类型的文件

**一般在真正使用static中的静态文件时,一般不会在link中的url中每次引用,一般在django中一次load就可以**

~~~ python
# 使用load将static直接引入到文件中
{% load static %}

# 在使用静态文件时通过以下代码引入,以下以引入js代码为例
<script src="{% ststic % 'js/script.js'}"></script>
~~~

# django中的模板语法

本质上在html中写一些占位符,再用一些数据对占位符进行处理

~~~ python
# 通过在views.py文件中编写函数,对变量进行赋值操作,并且在返回数据时将变量的数值以字典形式进行返回

# views.py

def temp(request):
    name = "zangsan"
    return render(request,"temp.html",{"str":name})

# html
<body>
    <h1>模板语法的学习</h1>
    <h2>{{str}}</h2>
</body>
</html>

~~~

**其语法类似vue**
要想获取到列表中某个索引的值,可以通过"变量名.索引"的方式来获取到列表变量值

~~~ python
# 如果在其中传的是列表数据的话,使用{{}}会直接显示列表
def temp(request):
    name = "zangsan"
    roles = ["root","user"]
    return render(request,"temp.html",{"str":name,"rol":roles})
# 要想在其中获取列表的每一个数据,可以通过下标获取
    <h3>{{rol.0}}</h3>
    <h3>{{rol.1}}</h3>

# 也可通过循环将其展示
    {% for item in rol %}
    <span>{{item}}</span>
    {%endfor%}
~~~


如果现在有个字典,可以在数据传出之后通过对应变量的索引键值

~~~ python
    <span>
        {{n3}}
        {{n3.name}}
        {{n3.salary}}
        {{n3.role}}
    </span>
<ul>
# 当后面为values时则显示键值,当后面为keys时,则显示键
    {%for item in n3.keys%}
        <li>{{item}}</li>
    {%endfor%}
</ul>

<ul>
# 这样可以将键和值分别显示在浏览器中
    {%for k,v in n3.items%}
        <li>{{k}} {{v}}</li>
    {%endfor%}
</ul>
~~~

# 条件语句

~~~ python
# 注意,在条件判断的==前后一定要加空格,要不然系统会出现报错
{% if str == "zhangsan" %}
    <h1>dadada</h1>
    {% else  %}
    <h1>ddddd</h1>
{% endif %}
~~~


# 请求和相应

在django中如何获取请求和相应相关数据

在浏览器中通过地址以及回车获取数据的方式为GET方式获取数据

~~~ python
def something(request):
    # request是一个对象，封装了用户通过浏览器发送过来的所有请求相关数据
    print(request.method)
    # 在url上传递一些值,通过下面的方法获取到url中的数值
    print(request.GET)
    # 响应
    return HttpResponse("return_view")
~~~

**关于重定向**

django项目中return的rediect中的结果，将网址直接交给浏览器，浏览器再将网址进行解析


**在编写用户登录页面时，在HTML代码的表单<form>中必须编写{% csfr_token %}，否则会出现页面报错的情况**


# 连接数据库，进行数据库操作

MySQL数据库+pymysql

Django操作数据库的方法，Django操作数据库的方法更加简单，内部提供了ORM框架

代码操作ORM，通过ORM来操作数据库

**操作步骤**

I even tried a fresh install
1：安装第三方模块 pip install mysqlclient(等价与pymysql)但是需要安装前置依赖

2：配置mysql数据库

**ORM可以帮助我们作两件事**

1：操作数据库的表[无法创建数据库，数据库需要自己创建]

2：创建和修改和删除数据库中的表，操作表中的数据[不用亲自写sql语句，django会通过ORM翻译为sql语句]

~~~ django

~~~

创建数据库

1:启动mysql服务

2：使用自带工具创建数据库

3:使用django连接数据库

在settings文件里面连接数据库

~~~ python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'Scarecrow_test',   # 数据库名字
        'USER': 'root',         #mysql账户名
        'PASSWPRD': 'wry040306',    # 用户密码
        'HOST': '127.0.0.1',        #数据库所在主机，哪台机器安装了mysql
        'PORT': '3306'          # 端口
    }
}
~~~


4:基于django操作表

在models.py文件中编写代码
创建表
~~~ python
class UserInfo(models.Model):
    name = models.CharField(max_length=32)
    password = models.CharField(max_length=64)
    age = models.IntegerChoices()
~~~
添加的表项添加到数据库

执行命令
**python manage.py makemigrations**    生成数据库同步脚本
**python manage.py migrate**            迁移处理表
将数据写入mysql

登陆mysql
mysql -u root -p
输入密码登陆

显示数据库
show databases结果如下：
~~~
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| Scarecrow_test     |
| information_schema |
| mysql              |
| performance_schema |
| srping             |
| sys                |
+--------------------+
6 rows in set (0.01 sec)
~~~

进入数据库
use Scarecrow_test
进入数据库

显示列表项
show tables
~~~
mysql> show tables;
+----------------------------+
| Tables_in_Scarecrow_test   |
+----------------------------+
| auth_group                 |
| auth_group_permissions     |
| auth_permission            |
| auth_user                  |
| auth_user_groups           |
| auth_user_user_permissions |
| django_admin_log           |
| django_content_type        |
| django_migrations          |
| django_session             |
| scarecrow_main_userinfo    |
+----------------------------+
11 rows in set (0.00 sec)
~~~

~~~
mysql> desc scarecrow_main_userinfo;结果如下：

+----------+-------------+------+-----+---------+----------------+
| Field    | Type        | Null | Key | Default | Extra          |
+----------+-------------+------+-----+---------+----------------+
| id       | bigint      | NO   | PRI | NULL    | auto_increment |
| name     | varchar(32) | NO   |     | NULL    |                |
| password | varchar(64) | NO   |     | NULL    |                |
| age      | int         | NO   |     | NULL    |                |
+----------+-------------+------+-----+---------+----------------+
~~~


删除表可以在models.py文件中注释到class类，然后再执行两段语句将数据变化同步到数据库中

修改表，在表中添加表项时：

**写在mudels.py文件中的每一个类都是mysql中的一张表**在表中新建一个字段会给出提示，一般要求添加默认值或者允许该项为空（null=True,blank=True）


因为在表中一开始的各项都存在数据，所以在添加表项时会要求给一个初始项的值，选择1的话就会让你添加一个初始值，选择2就会退出，可以在函数中括号添加default="预期的默认值"

**以后在开发中如果想对表结构进行调整的话，只需要在mudels.py文件中操作类即可，操作完成之后在执行同步数据库的指令即可**

# 操作表中的数据

**在表中添加数据**
~~~ python
class UserInfo(models.Model):
    name = models.CharField(max_length=32)
    password = models.CharField(max_length=64)
    age = models.IntegerField()
# 新建数据,创建表项
UserInfo.objects.create(name='zhangsan',password='zhangsan111',age=28)
~~~

**删除操作**
~~~ python
# 删除数据，可以在前面添加筛选条件
UserInfo.objects.filter(name='zhangsan').delete()   #删除name=zhangsan的数据
UserInfo.objects.all().delete()   #删除这张表中的所有数据

~~~

**获取数据**

~~~ python
# 获取到所有数据,最后得到的结果都是Queryset类的数据，其中每个对象就是一行数据
data_list = UserInfo.objects.all()
# 可以通循环来输出数据
for obj in data_list:
    print(obj.name,obj.password,obj.age)
~~~

不管筛选到的对象是几个最终返回的都是Queryset对象，都不能直接输出


**更新数据**

~~~ python
# 通过筛选找到相对应的数据，通过调用update函数来对数据进行更新
UserInfo.objects.filter(name='zhangsan').update(password='999')
~~~

~~~ python
# 获取到所有数据,最后得到的结果都是Queryset类的数据，其中每个对象就是一行数据
data_list = UserInfo.objects.all()
# 可以通循环来输出数据
for obj in data_list:
    print(obj.name,obj.password,obj.age)
~~~

不管筛选到的对象是几个最终返回的都是Queryset对象，都不能直接输出


# 表之间的关联
~~~ python
class UserInfo(models.Model):
    name = models.CharField(verbose_name='姓名',max_length=32)
    password = models.CharField(verbose_name='密码',max_length=64)
    age = models.IntegerField(verbose_name='年龄')
    account = models.DecimalField(verbose_name="账户余额",max_length=10,decimal_places=2)      #该表项表示用户余额，最大长度为10,小数点后两位数字
    # 这种写法是没有约束的，用户可以填写任何id
    department_id = models.IntegerField(verbose_name='部门') #在部门表已经存在的情况之下，用户在存储部门信息的时候一般会存储部门信息表中的ID编号
    # 有约束的写法,只允许部门id在已经存在的表项
    depart_id = models.ForeignKey(to = 'Department',to_field="id")
    # to表示要与哪张表关联，to_filed表示需要与u表中的哪一列关联

class Department(models.Model):
    title = models.CharField(verbose_name='部门',max_length=64)
~~~

**一些特别大的公司会在departnemt的表项中存储直接的部门名称，因为在获取到id之后需要在depertment表中区查找对应id的名称，这涉及到表的关联操作，连表操作比较耗时【为了加速查找，允许数据冗余】，一般情况下写成id**

在存储时需要注意，部门id需要有约束，只能是部门表中已经存在的id,通过以下写法进行约束，填写的部门id只能是表中已经存在的id，否则报错
~~~ python
    # 有约束的写法,只允许部门id在已经存在的表项
    depart_id = models.ForeignKey(to = 'Department',to_field="id")
    # to表示要与哪张表关联，to_filed表示需要与u表中的哪一列关联
~~~

当部门被删除时，其部门下的员工也需要一并删除，或者置空
~~~ python
    # 级联删除——部门被删除时其下员工也将一并被删除
    department_id = models.ForeignKey(to = 'Department',to_field="id",on_delete=models.CASCADE)

    # 置空，表示该表项允许为空，并且在部门被删除时，部门所有的用户的部门置为空
    department_id = models.ForeignKey(to = 'Department',to_field="id",null = True, blank = True,on_delete=models.SET_NULL)

~~~

**django中的约束——与数据库的约束不同，数据库的约束是根据表项来进行约束的，而django中的约束是通过自身定义来约束的**

~~~ python

class Department(models.Model):
    title = models.CharField(verbose_name='部门',max_length=64)



class UserInfo(models.Model):
    name = models.CharField(verbose_name='姓名',max_length=32)
    password = models.CharField(verbose_name='密码',max_length=64)
    age = models.IntegerField(verbose_name='年龄')
    account = models.DecimalField(verbose_name="账户余额",max_length=10,decimal_places=2)      #该表项表示用户余额，最大长度为10,小数点后两位数字
    # 这种写法是没有约束的，用户可以填写任何id
    department_id = models.IntegerField(verbose_name='部门') #在部门表已经存在的情况之下，用户在存储部门信息的时候一般会存储部门信息表中的ID编号

    # 有约束的写法,只允许部门id在已经存在的表项
    department_id = models.ForeignKey(to = 'Department',to_field="id",on_delete=models.CASCADE)
    # 人为规定限制设置性别的选择
    gander_choices = (
        (1,"男"),
        (2,"女")
    )  
    # 在性别的选区中只能在gender_choices中进行选取
    gender = models.SmallIntegerField(verbose = "性别",choices=gander_choices)

~~~

# 编写连接实现前端连接发送请求后端处理请求

***在编写前端页面时，各种请求可以通过POST以及GET来发送，可以在组建中添加href（指向作要访问的地址，然后后端根据地址将其和函数相对应绑定）***

