
第五部分 参数
===========

## 16 位置参数

语法  |                               含义
-----|--------------------------------------------------
${n} | 命令行某个单独的参数；当1<=n<=9时，也可以简记作$n。
$0   | 当前程序名。
$#   | 命令行参数的数目，用十进制数表示。
$*   |  当$*放在双引号中时，命令行上的所有参数将被视为单个字符。 即”$*”等同于”$1 $2 $3 ….”。 $IFS的第一个字符用来作为分隔符，以分隔不同的值来建立字符串。 如果没有设置$IFS，就用空格来分隔；如果$IFS为空，则参数不使用中间分隔符来链接。
$@   | 与$*相同，但有区别：将所有命令行参数各自视为单独的个体，也就是单独字符串。 即”$@”等同于”$1”“$2”“$3” ……。 这是将参数传递给其他程序的最佳方式，因为它会保留所有内嵌在每个参数里的任何空白。 如果”$@”出现在一个单词中， ”$@”扩展后的第一部分”$1”将与原单词的前部分连接， ”$@”扩展后的最后一部分”$n”与原单词的后部分连接。
$$   | 当前shell的进程号；在子shell中，它扩展成当前shell的进程ID，而不是子shell的进程 （参见shell内置变量BASH PID）。
$?   | 上一条命令的退出状态。
$!   | 最近发出的后台命令的进程号。
$-   | 当前执行的选项（参阅set内置命令）。默认时，hB代表脚本，himBH代表交互式shell。
$_   | 初值被设置为调用这个shell的文件名，然后把每个命令设置为前一条命令的最后一个单词。

**说明：**

	位置参数不能用赋值表达式赋值，但可以被set内建命令重新赋值。如果没有位置参数，则”$*”和”$@”不进行扩展。
 
 
## 17 数组

Bash提供了一维的索引式数组变量和关联式数组变量。数组的大小没有最大值限制。索引式数组通过使用非负整数（包括算术表达式）来引用，关联数组通过任意的字符串来引用。数组能隐式自动创建，也能使用内建命令declare来创建。

如果使用形如：`name[subscript]=value`的形式来给变量赋值，则一个索引数组将自动被创建；下标subscript可以是算术表达式，但必须计算成一个大于或等于0的整数。此外，还可以通过使用`declare -a name`来显示地声明一个索引式数组。`declare -a name[subscript]`也是被允许的，但下标subscript将被忽略。关联式数组是通过使用`declare -A name`来创建。

数组可以使用复合赋值的形式来赋值，如：`name=(value_1 …… value_2)`，其中的每个值等同于`[subscript]=string`的形式。索引式数组赋值不需要中括号和下标。当赋值给一个索引式数组时，如果提供可选的中括号和下标，则那个下标索引指定的数组变量元素将被赋值；否则，被赋值的元素的索引为在已被赋值的最后一个元素的索引上加1，**索引起始于0**。

当给关联式数组赋值时，下标是必须的。单独数组元素赋值可以使用`name[subscript]=value`。

数组的每个元素都可以使用`${name[subscript]}`来引用。大括号是必须的，以避免与路径我扩展的冲突。如果subscript为`*`或`@`，`${name[@]}`或`${name[*]}`扩展成name的所有成员（参见`$*`或`$@`）。

引用一个没有下标的数组变量，等同于引用一个下标为0的数组。 

`unset`内建命令可用销毁数组变量。`unset name[subscript]`销毁下标subscript处的数组元素。`unset name`或`unset name[@]` 或 `unset name[*]`除整个数组。
