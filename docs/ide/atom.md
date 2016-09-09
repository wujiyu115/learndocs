title: atom
date: 2016-9-2 19:10:39
tags: atom
---
## atom 介绍
>Atom是Github专为hacker推出的开源的文本编辑器，支持linux、window等多平台



## 基本操作

+ **多光标编辑**:`cmd/ctrl + 鼠标左键`点击多个地方
+ **拼写检查**:`spell check`
+ **显示tabs或空格**:`Settings -> Settings -> Show Invisibles`



## 设置
+ Settings：全局设置
+ Keybindings：快捷键配置
+ Packages： 插件管理中心, 例如设置/禁用/删除插件
+ Themes：主题管理中心
+ Updates：查询社区包/插件的状态
+ Install：搜索插件和主题
+ Open Config Folder：配置文件集合目录，非常具有程序员风格直接修改config

## 快捷键



| 快捷键                     | 英文              | 功能              |
|----------------------------|-------------------|-------------------|
| Ctrl + ，                   | Setting         | 打开设置          |
| Ctrl + N                   | New File          | 新建文件          |
| Ctrl + W                   | Close Tab         | 关闭Tab           |
| Ctrl + Tab                 | Next Item         | 切换tab           |
| Ctrl + Shift + T           | Reopen last Item  | 重新打开刚关闭tab |
| Ctrl + Shift + P           | Command  | 命令模式 |
| Ctrl +  P           | Nav File  | 文件导航 |
| Ctrl +  R           | Nav Symbols  | 方法导航 |
| Ctrl + \`|Toggle Tree View | 切换目录树|
| Ctrl + Z                   | Undo              | 撤销              |
| Ctrl + Y                   | Redo              | 取消撤销          |
| Ctrl + G   行:列                | Go to Line        | 跳转某行          |
| Ctrl + L                   | select line       | 选定某行          |
| Ctrl + /                   | Toggle Comments   | 切换注释          |
| Ctrl + Shift +U            | Encoding selector | 切换编码格式      |
| F11                        | Full Screen       | 全屏              |
| Ctrl + F                   | Find in Buffer    | 从缓存查找        |
| Ctrl + Shift + F           | Find in project   | 从工程查找        |
| F3                         | Find next         | 查看下一个        |
| Ctrl + Shift + K           | delete line       | 删除一行          |
| Ctrl + Enter               | Newline below     | 下方新增行        |
| Ctrl + Shift + Enter       | Newline above     | 上方新增行        |

| 折叠操作                     | 英文              | 功能              |
|----------------------------|-------------------|-------------------|
| Ctrl + Alt + \[            | Fold              | 折叠              |
| Ctrl + Alt + \]            | Unfold            | 展开              |
| Ctrl + Alt + Shift + \[    | Fold all          | 折叠全部          |
| Ctrl + Alt + Shift + \]    | Unfold all        | 展开全部          |



| 分屏操作                     | 英文              | 功能              |
|----------------------------|-------------------|-------------------|
| cmd/ctrl+w+c关闭 | Close Split Screen         | 关闭分屏          |
| cmd/ctrl + w,方向键 | Split Screen         | 创建分屏          |
| cmd/ctrl + k,方向键 | Move Split Screen         | 移动分屏          |

| 选择操作       | 功能              |
|-------------- |-------------------|
| Shift+Up or Ctrl+Shift+P | Select up|
| Shift+Down or Ctrl+Shift+N | Select down|
| Shift+Left or Ctrl+Shift+B | Select previous character|
| Shift+Right or Ctrl+Shift+F | Select next character|
| Alt+Shift+Left or Alt+Shift+B | Select to beginning of word|
| Alt+Shift+Right or Alt+Shift+F | Select to end of word|
| Cmd+Shift+Right or Ctrl+Shift+E | Select to end of line|
| Cmd+Shift+Left or Ctrl+Shift+A | Select to first character of line|
| Cmd+Shift+Up | Select to top of file|
| Cmd+Shift+Down | Select to bottom of file|
| Cmd+A | Select the entire contents of the file|
| Cmd+L | Select the entire line|
| Ctrl+Shift+W | Select the current word|

| 光标操作       | 功能              |
|-------------- |-------------------|
| Alt+Left or Alt+B     | 上一个单词首      |
| Alt+Right or Alt+F     | 下一个单词尾       |
| Cmd+Left or Ctrl+A     | 行首        |
| Cmd+Right or Ctrl+E     | 行尾        |
| Cmd+Up    | 文件头       |
| Cmd+Down      | 文件尾     |
| Ctrl+P      | 上一个词    |
| Ctrl+N      | 下一个词    |
| Ctrl+B      | 左一个词    |
| Ctrl+F      | 右一个词    |


## 安装插件

### markdown
+ markdown-preview-plus@2.2.2
+ markdown-assistant@0.1.0(Upload images from the clipboard automatically, win10 下失败，osx 完美成功，粘贴图片直接转成 md 文本，不要太棒，优雅的没朋友)
+ qiniu-uploader@0.0.3
+ markdown-writer@2.3.2(在设置里面设置一下，就拥有了正常 md 编辑器的编辑 md 的各种快捷键 例如 cmd + b, cmd + shift + k 插入链接，而且更棒非常值得一试)
+ markdown-toc@0.4.1
+ markdown-scroll-sync：markdown同步滚动预览功能

### autocomplete
+ atom-ternjs@0.13.2(js 最佳补全插件)
+ css-snippets@0.9.0
+ autocomplete-html-entities@0.1.0
+ tag@0.3.0
+ autocomplete-modules@1.4.1

### indent
+ guess-indent@0.1.0
+ resize-indent@0.2.1

### code hint and linter
+ linter@1.11.3
+ jshint@1.8.3
+ jsonlint@1.1.2
+ csslint@1.1.4
+ htmlhint@1.1.3


### ui
+ activate-power-mode@0.4.1(虽然很炫酷，但是我不用)
+ file-icons@1.6.18(让你拥有高颜值的文件图标)
+ pigments 代码颜色可视化
+ foldername-tabs@0.1.11
+ highlight-column@0.5.1
+ highlight-selected@0.11.2
+ indent-guide-improved@1.4.5
+ minimap@4.21.0
+ fold-comments@0.6.0
+ fold-functions@0.4.3

### git project
+ merge-conflicts@1.3.7(amazing ，再也不怕 git 的合并冲突了，分分钟解决 conflicts)
+ git-projects@1.17.0

### other tools
+ atom-beautify@0.28.26（格式化代码）
+ line-count@0.5.0
+ change-case@0.6.0（将代码文本更改风格，比如 testCode => TEST_CODE）
+ todo-show@1.4.0
+ open-html-in-browser@0.1.0
+ pretty-json@0.4.1

### Tree-view
+ tree-view-copy-relative-path@1.0.0
+ copy-filename@1.0.1
+ tree-view-git-status@0.2.3
+ chary-tree-view@0.2.3(Tree-view responds to only double-click to avoid opening a large file accidentally.)

## 主题

## snippets

## git


## 命令行
+ **atom打开**:atom filename
+ **同时打开多个**:atom dir1 dir2

https://segmentfault.com/a/1190000003043309
http://simplyy.space/article/56ecd7303aae9e5a65c46d64
https://www.zhihu.com/question/39938370/answer/91424130?from=profile_answer_card
http://flight-manual.atom.io/using-atom/sections/editing-and-deleting-text/
http://wiki.jikexueyuan.com/project/atom/gulp.html
