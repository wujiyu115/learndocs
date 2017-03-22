python django
==============================


----

## django 安装
```pip install django```

## django 结构

+ 建立一个网站模板
```django-admin startproject mysite```
结构如下

```
mysite/
    manage.py
    mysite/
        __init__.py
        settings.py 网站设置
        urls.py
        wsgi.py
```

## django-admin 和 manage.py
### manage.py
+ migrate

```
根据 mysite/settings.py的配置加载依赖库和文件
```
+ runserver 8000 --noreload
```

启动服务器 
端口:0.0.0.0:8000
更改python代码不自动load:--noreload
单线程:--nothreading(默认多线程)
```

## django 配置
+ DATABASES 
  默认用sqllite
  
```
默认用sqllite
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': 'mydatabase',
    }
}
PostgreSQL配置:
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql_psycopg2',
        'NAME': 'mydatabase',
        'USER': 'mydatabaseuser',
        'PASSWORD': 'mypassword',
        'HOST': '127.0.0.1',
        'PORT': '5432',
    }
}
其他ENGINE:
'django.db.backends.postgresql_psycopg2'
'django.db.backends.mysql'
'django.db.backends.sqlite3'
'django.db.backends.oracle'
```
