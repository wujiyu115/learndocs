MAC PHP环境安装
=====================
## 集成环境
+ XAMPP
<http://www.apachefriends.org/zh_cn/xampp.html>  
<http://www.xampps.com/>
+ phpstudy
<http://www.phpstudy.net/a.php/208.html>
+ Wamp
<http://www.wampserver.com/en/>
+ easyphp
<http://www.easyphp.org/>
+ ampps
<http://www.ampps.com/>
+ mamp
<https://www.mamp.info/en/>

## 安装环境

### php
+ mac自带的php路径:
```
/private/etc/php.ini.default
```
+ for nginx:
```
brew install php55 --with-fpm #Nginx
```
+ for apache
```
brew install php55 #Apache
```
+ php-mysql
```
brew install php55-mysql
```


### apache
+ mac自带的apache路径:
```
/etc/apache2/
```
+ 开启apache
```
sudo apachectl start
```

