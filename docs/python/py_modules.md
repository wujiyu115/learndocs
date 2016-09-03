python模块使用
==========================

### requests
```python
#requests 自带 session。
#http://docs.python-requests.org/en/latest/user/advanced/#session-objects

#如果想更改某个 cookie。
def update_cookie(cookiejar, cookie):
    _cookies = requests.cookies
    _cookies.remove_cookie_by_name(cookiejar, 'cookie_name')
    cookiejar.set_cookie(_cookies.create_cookie('cookie_name', cookie, **{'domain': '.example.com'}))
```

## MySQLdb
MySQLdb 是python是的mysql driver.
[MySQLdb Sourceforge](http://mysql-python.sourceforge.net/MySQLdb.html)
win下载对应的exe版本:
[https://pypi.python.org/pypi/MySQL-python/1.2.5](https://pypi.python.org/pypi?:action=show_md5&digest=6f43f42516ea26e79cfb100af69a925e)

### 安装
```shell
pip install mysql-python
```

###  帮助
+  版本信息
```py
#查看ping对象帮助
import MySQLdb;   help(MySQLdb._mysql.connection.ping)
#查看版本
print(MySQLdb._mysql.__version__)
```

+ 基础函数
```py
import MySQLdb

if __name__ == '__main__':
    print(dir(MySQLdb._mysql))
    print(dir(MySQLdb._mysql.connection))

## 输出
#mysql对象
['DataError', 'DatabaseError', 'Error', 'IntegrityError', 'InterfaceError', 'InternalError', 'MySQLError', 'NULL', 'NotSupportedError', 'OperationalError', 'ProgrammingError', 'Warning', 'connect', 'connection', 'debug', 'escape', 'escape_dict', 'escape_sequence', 'escape_string', 'get_client_info', 'result', 'server_end', 'server_init', 'string_literal', 'thread_safe', 'version_info']

#connection对象
['affected_rows', 'autocommit', 'change_user', 'character_set_name', 'client_flag', 'close', 'commit', 'converter', 'dump_debug_info', 'errno', 'error', 'escape', 'escape_string', 'field_count', 'get_autocommit', 'get_character_set_info', 'get_host_info', 'get_proto_info', 'get_server_info', 'info', 'insert_id', 'kill', 'next_result', 'open', 'ping', 'port', 'query', 'rollback', 'select_db', 'server_capabilities', 'set_character_set', 'set_server_option', 'shutdown', 'sqlstate', 'stat', 'store_result', 'string_literal', 'thread_id', 'use_result', 'warning_count']

```

### 基本用例
+   select 查询
```py
cur = conn.cursor(cursorclass=MySQLdb.cursors.DictCursor)
cur.execute(sql)
result = cur.fetchall()
cur.close()
```

+   执行其他sql
```py
conn.autocommit(True)
cur = conn.cursor()
result = cur.execute(sql)
cur.close()
```

### 解决mysql has gone away问题
```py
def build_conn(self):
        ret =None
        try:
            ret = MySQLdb.connect(host=self.host, user=self.user, passwd=self.passwd, db=self.db, port=self.port,charset="utf8",connect_timeout=20)
        except Exception, e:
            writeLog(traceback.format_exc())
        return ret

    def get_conn(self):
        if self.conn == None:
            self.conn = self.build_conn()
        else:
            try:
                # fix reconnect
                self.conn.ping()
            except Exception, e:
                self.close_conn()
                # print("error",e)
                writeLog(traceback.format_exc())
                self.conn = self.build_conn()
        return self.conn

    def close_conn(self):
        if self.conn:
            self.conn.close()
            self.conn = None
```

###  完整util
```py
import MySQLdb
import traceback
from mylog import writeLog

class mymysqldb(object):

    def __init__(self, host, user, passwd, db, port):
        self.host = str(host)
        self.user = str(user)
        self.passwd = str(passwd)
        self.db = str(db)
        self.port = int(port)
        self.conn = None

    def build_conn(self):
        ret =None
        try:
            ret = MySQLdb.connect(host=self.host, user=self.user, passwd=self.passwd, db=self.db, port=self.port,charset="utf8",connect_timeout=20)
        except Exception, e:
            writeLog(traceback.format_exc())
        return ret

    def get_conn(self):
        if self.conn == None:
            self.conn = self.build_conn()
        else:
            try:
                # fix reconnect
                self.conn.ping()
            except Exception, e:
                self.close_conn()
                # print("error",e)
                writeLog(traceback.format_exc())
                self.conn = self.build_conn()
        return self.conn

    def close_conn(self):
        if self.conn:
            self.conn.close()
            self.conn = None

    def excute_sql(self, sql):
        conn = self.get_conn()
        if conn == None:
            return 0

        try:
            conn.autocommit(True)
            cur = conn.cursor()
            result = cur.execute(sql)
            cur.close()
            return result
        except:
            writeLog(traceback.format_exc())
            self.close_conn()
            return 0

        return 0

    def select_sql(self, sql):

        conn = self.get_conn()
        if conn == None:
            return None

        try:
            cur = conn.cursor(cursorclass=MySQLdb.cursors.DictCursor)
            cur.execute(sql)
            result = cur.fetchall()
            cur.close()
            return result
        except:
            writeLog(traceback.format_exc())
            self.close_conn()
            return None

        return None

    def escape_string(self, string):
        result = ''

        for c in string:
            escape = None
            char_num = ord(c)

            if char_num == 0:
                escape = '0'
            elif char_num == ord('\''):
                escape = '\''
            elif char_num == ord('"'):
                escape = '"'
            elif char_num == ord('\b'):
                escape = 'b'
            elif char_num == ord('\n'):
                escape = 'n'
            elif char_num == ord('\r'):
                escape = 'r'
            elif char_num == ord('\t'):
                escape = 't'
            elif char_num == 26:
                escape = 'Z'
            elif char_num == ord('\\'):
                escape = '\\'
            elif char_num == ord('%'):
                escape = '%'

            if escape:
                result = result + '\\' + escape
            else:
                result = result + c

        return result

#----------------------------------------------------------------------
# testing reconnect ,  in mysql setting, set global wait_timeout=5; trigger the error of "MySQL has gone away"
#----------------------------------------------------------------------
import time
if __name__ == '__main__':
    db = mymysqldb('127.0.0.1','root','12345678','t_log',3306)
    print("result",db.select_sql('SELECT * FROM t_visit'))
    time.sleep(6)
    print("result111",db.select_sql('SELECT * FROM t_visit'))
    # time.sleep(6)
    print("result2222",db.select_sql('SELECT * FROM t_visit'))

```