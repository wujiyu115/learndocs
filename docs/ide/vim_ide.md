vim插件
===============

## vim成为ide基础功能
### 移动到下一行首`+`

### 整段缩进 `

>- 整篇代码进行排版: 光标移动到文件开头，然后按 `=G`即可。
>- 对指定行反缩进: `:1,10<`
>- 可视模式下操作: `v`模式选中，然后按`[Shift+,`或`.]`，就是输入`<`反缩进 `>`缩进

### 块注释
使用查找替换的方法 在linux中，文本每一行的起始标志是^，结束标志为$，因此使用vim搜索^并替换为^#即可。
```shell
:10,20s/^/#/g
#取消注释为
:10,20s/^#//g
```

或者用`nerdcommenter`插件

### 任意文件导航
+ ctrlp: https://github.com/ctrlpvim/ctrlp.vim


### 精确查找替换

### 多文件查找替换
`vimgrep`

### 选择
`v`模式

### 复制
`yy`

### 粘贴
`p`

### 查找
+ ack(搜索): https://github.com/mileszs/ack.vim
+ fuzzysearch(模糊查找) https://github.com/ggVGc/vim-fuzzysearch

### 替换

### 光标定位


## 插件
+ vimfiler(文件树): https://github.com/Shougo/vimfiler.vim
+ nerdtree(文件树): https://github.com/scrooloose/nerdtree
+ wildfire(快速选择代码) https://github.com/gcmt/wildfire.vim
+ vim-surround(快速加双引号) https://github.com/tpope/vim-surround



