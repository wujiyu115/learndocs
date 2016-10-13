vim
===============


## 入门

1. 启 动Vim后，vim在 Normal 模式下。
2. 让我们进入 Insert 模式，请按下键 i 。(陈皓注：你会看到vim左下角有一个–insert–字样，表示，你可以以插入的方式输入了）
3. 此时，你可以输入文本了，就像你用“记事本”一样。
4. 如果你想返回 Normal 模式，请按 ESC 键。
现在，你知道如何在 Insert 和 Normal 模式下切换了。下面是一些命令：

    > + i → Insert 模式，按 ESC 回到 Normal 模式.
    > + x → 删当前光标所在的一个字符。
    > + :wq → 存盘 + 退出 (:w 存盘, :q 退出)   （陈皓注：:w 后可以跟文件名）
    > + dd → 删除当前行，并把删除的行存到剪贴板里
    > + p → 粘贴剪贴板
    > + hjkl (强例推荐使用其移动光标，但不必需) →你也可以使用光标键 (←↓↑→). 注: j 就像下箭头。
    > + :help <command> → 显示相关命令的帮助。你也可以就输入 :help 而不跟命令。（陈皓注：退出帮助需要输入:q）

## 编辑命令

1. 各种插入模式
> + a → 在光标后插入
> + o → 在当前行后插入一个新行
> + O → 在当前行前插入一个新行
> + cw → 替换从光标所在位置后到一个单词结尾的字符

2. 简单的移动光标
> + 0 → 数字零，到行头
> + ^ → 到本行第一个不是blank字符的位置（所谓blank字符就是空格，tab，换行，回车等）
> + $ → 到本行行尾
> + g_ → 到本行最后一个不是blank字符的位置。
> + /pattern → 搜索 pattern 的字符串（陈皓注：如果搜索出多个匹配，可按n键到下一个）

3. 拷贝/粘贴
> + P → 粘贴
> + yy → 拷贝当前行当行于 ddP

4. Undo/Redo
> + u → undo
> + <C-r> → redo

5. 打开/保存/退出/改变文件
> + :e <path/to/file> → 打开一个文件
> + :w → 存盘
> + :saveas <path/to/file> → 另存为 <path/to/file>
> + :x， ZZ 或 :wq → 保存并退出 (:x 表示仅在需要时保存，ZZ不需要输入冒号并回车)
> + :q! → 退出不保存 :qa! 强行退出所有的正在编辑的文件，就算别的文件有更改。
> + :bn 和 :bp → 你可以同时打开很多文件，使用这两个命令来切换下一个或上一个文件。（陈皓注：我喜欢使用:n到下一个文件）

6. 查找
> /开始查找,n查找下一个,N向上查找

##　进阶

1. . → (小数点) 可以重复上一次的命令
2. N<command> → 重复某个命令N次
> + 2dd → 删除2行
> + 3p → 粘贴文本3次
> + 100idesu [ESC] → 会写下 “desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu desu “
> + . → 重复上一个命令—— 100 “desu “.

### 光标移动更有效率
1. NG → 到第 N 行 （陈皓注：注意命令中的G是大写的，另我一般使用 : N 到第N行，如 :137 到第137行）
2. gg → 到第一行。（陈皓注：相当于1G，或 :1）
3. G → 到最后一行。
4.  按单词移动：
> + w → 到下一个单词的开头。
> + e → 到下一个单词的结尾。
> + % : 匹配括号移动，包括 (, {, [. （陈皓注：你需要把光标先移到括号上）
> + \* 和 #: 匹配光标当前所在的单词，移动光标到下一个（或上一个）匹配单词（*是下一个，#是上一个）

### 更快的拷贝
1. `0y$`命令解释:
> + 0 → 先到行头
> + y → 从这里开始拷贝
> + $ → 拷贝到本行最后一个字符
2. `ye`:从当前位置拷贝到本单词的最后一个字符
3. `y2/foo`:拷贝2个 “foo” 之间的字符串

##  高级

1. 在当前行上移动光标: `0 ^ $ f F t T , ;`
>+ 0 → 到行头
>+ ^ → 到本行的第一个非blank字符
>+ $ → 到行尾
>+ g_ → 到本行最后一个不是blank字符的位置。
>+ fa → 到下一个为a的字符处，你也可以fs到下一个为s的字符。
>+ t, → 到逗号前的第一个字符。逗号可以变成其它字符。
>+ 3fa → 在当前行查找第三个出现的a。
>+ F 和 T → 和 f 和 t 一样，只不过是相反方向  
![vim_move.jpg](/_static/vim_move.jpg)

2. `dt`:→ 删除所有的内容，直到遇到双引号

放两张速记图:

![vim_go](/_static/vim_go.png)
![vim_cheat_sheet_for_programmers_print](/_static/vim_cheat_sheet_for_programmers_print.png)

