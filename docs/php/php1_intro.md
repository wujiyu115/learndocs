PHP web基础函数解读
==========================

## 一些用法

+ `error_reporting(E_ALL ^ E_NOTICE);` :报告除E_NOTICE外的所有消息
+ `@set_time_limit(0);` :设置代码最大执行时间
+  控制脚本中数据的输出
```php
ob_start();
 ob_flush();
ob_clean();
```
+ 定义常量
`define('IA_ROOT', str_replace("\\",'/', dirname(__FILE__)));`

+ header
```php
header('content-type: text/html; charset=utf-8'); //设置编码
header('location: ./index.php'); //跳转
header('location: ?refresh');//刷新
```

+ setcookie
`setcookie('action', 'env');`

+ exit();
```php
exit(json_encode($vars)); //退出并输出一段信息
```

+ extension_loaded
检查模块是否开启


## 作用域
函数作用域,与括号无关

## 基础语法
```php
<?php
// 此处是 PHP 代码
?>
```

## 注释
```php
<?php
// 这是单行注释

# 这也是单行注释

/*
这是多行注释块
它横跨了
多行
*/
?>
```