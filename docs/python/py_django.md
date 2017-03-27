python django
==============================


----

## django 安装
```
pip install django
```

## django 结构

+ 建立一个网站模板
```
django-admin startproject mysite
```

结构如下

```
mysite
    manage.py
    mysite
        __init__.py
        settings.py
        urls.py
        wsgi.py
```

## django-admin 和 manage.py

### manage.py
```shell
#根据 mysite/settings.py的配置加载依赖库和文件
python manage.py migrate --run-syncdb
#启动服务器
python manage.py runserver 8000 --noreload
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
