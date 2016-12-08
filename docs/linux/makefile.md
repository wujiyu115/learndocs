A Simple Makefile Tutorial
======================



## 几个概念
### target

### PHONY 伪目标


### notdir (去除路径)
### patsubst (替换通配符)

### wildcard (扩展通配符)
Makefile在变量的定义和函数引用时，通配符将失效。需要使用函数“wildcard”，它的用法是：$(wildcard PATTERN...)

```shell
#工作目录下的所有的.c文件进行编译并最后连接成为一个可执行文件
objects := $(patsubst %.c,%.o,$(wildcard *.c))
foo : $(objects)
cc -o foo $(objects)
```

### install命令
```shell
#复制并改变权限
install -p -m 0755 skynet/3rd/lua/lua $(BIN_DIR)/lua
```