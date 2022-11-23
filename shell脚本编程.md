# 6.shell脚本编程

### 6.1 编程基础

#### 6.1.1 程序组成

* 程序：算法+数据结构 
* 数据：是程序的核心 
* 数据结构：数据在计算机中的类型和组织方式 
* 算法：处理数据的方式

#### 6.1.2 程序编程风格

* 面向过程语言 

  @ 做一件事情，排出个步骤，第一步干什么，第二步干什么，如果出现情况A，做什么处理，如 果出现了情况B，做什么处理 

  @ 问题规模小，可以步骤化，按部就班处理 

  @ 以指令为中心，数据服务于指令 

  @ C，shell

* 面向对象语言 

  @ 将编程看成是一个事物，对外界来说，事物是直接使用的,不用关心事物内部的情况。而编程 就是设置事物能够完成功能。 

  @ 一种认识世界、分析世界的方法论。将万事万物抽象为各种对象 

  @ 类是抽象的概念，是万事万物的抽象，是一类事物的共同特征的集合 

  @ 对象是类的具象，是一个实体 

  @ 问题规模大，复杂系统 

  @ 以数据为中心，指令服务于数据 

  @ java，C#，python，golang等

#### 6.1.3 编程语言

编程语言排名链接

```
https://www.tiobe.com/tiobe-index/
```

计算机：运行二进制指令 

编程语言：人与计算机之间交互的语言。分为两种：低级语言和高级语言

* 低级编程语言： 

  机器：二进制的0和1的序列，称为机器指令。与自然语言差异太大，难懂、难写 

  汇编：用一些助记符号替代机器指令，称为汇编语言 

  ​		如：ADD A,B 将寄存器A的数与寄存器B的数相加得到的数放到寄存器A中 

  ​		汇编语言写好的程序需要汇编程序转换成机器指令 

  ​		汇编语言稍微好理解，即机器指令对应的助记符，助记符更接近自然语言 

* 高级编程语言： 

  编译：高级语言-->编译器-->机器代码文件-->执行，如：C，C++ 

  解释：高级语言-->执行-->解释器-->机器代码，如：shell，python，php，JavaScript，perl

编译和解释型语言

#### 6.1.4 编程逻辑处理方式

三种处理逻辑 

* 顺序执行：程序按从上到下顺序执行 
* 选择执行：程序执行过程中，根据条件的不同，进行选择不同分支继续执行 
* 循环执行：程序执行过程中需要重复执行多次某段语句

### 6.2 shell 脚本语言的基本用法

#### 6.2.1 shell 脚本的用途

* 将简单的命令组合完成复杂的工作,自动化执行命令,提高工作效率 
* 减少手工命令的重复输入，一定程度上避免人为错误 
* 将软件或应用的安装及配置实现标准化 
* 用于实现日常性的,重复性的运维工作,如:文件打包压缩备份,监控系统运行状态并实现告警等

#### 6.2.2 shell 脚本基本结构

shell脚本编程：是基于过程式、解释执行的语言 

编程语言的基本结构： 

* 各种系统命令的组合 
* 数据存储：变量、数组 
* 表达式：a + b 
* 控制语句：if  
* shell脚本：包含一些命令或声明，并符合一定格式的文本文件 

格式要求：首行shebang机制

```bash
#!/bin/bash
#!/usr/bin/python
#!/usr/bin/perl 
```

#### 6.2.3 shell脚本创建过程

第一步：使用文本编辑器来创建文本文件 

第一行必须包括shell声明序列：#!

```bash
#!/bin/bash 
```

添加注释,注释以#开头 

第二步：加执行权限 

给予执行权限，在命令行上指定脚本的绝对或相对路径 

第三步：运行脚本 

直接运行解释器，将脚本作为解释器程序的参数运行

#### 6.2.4 shell 脚本注释规范

1、第一行一般为调用使用的语言 

2、程序名，避免更改文件名为无法找到正确的文件 

3、版本号 

4、更改后的时间 

5、作者相关信息 

6、该程序的作用，及注意事项 

7、最后是各版本的更新简要说明

#### 6.2.5 第一个 shell 脚本

```
#!SHEBANG
CONFIGURATION_VARIABLES
FUNCTION_DEFINITIONS
MAIN_CODE
```

范例：第一个 Shell 脚本 hello world

```bash
#第一步：编写相应文件
[root@sh-lzy-Centos8 23:26 data]#vim hello.sh 

#!bin/bash
#----------------------------------------------
#Filename:      hello.sh
#Version:       1.0
#Date:          2022/01/07
#Author:        Lzy
#---------------------------------------------
echo "hello,world!"

#第二步，赋权
[root@sh-lzy-Centos8 23:27 data]#chmod 755 hello.sh 
[root@sh-lzy-Centos8 23:27 data]#ll
total 44
-rw-r--r--.   1 root root    0 Jan  1 20:19 2021-12-31-20:19:53.log
drwxr-xr-x. 143 root root 8192 Jan  1 21:28 bak2022-01-01
-rw-r--r--.   1 root root    4 Jan  3 00:28 bc.log
lrwxrwxrwx.   1 root root    9 Jan  2 22:52 dirlink -> /data/dir
drwxr-xr-x. 142 root root 8192 Jan  1 18:46 etc-bak2022-01-01-21-29-39
-rw-r--r--.   1 root root    0 Jan  1 20:00 f1.txt
-rwxr-xr-x.   1 root root  192 Jan  7 23:25 hello.sh
-rw-r--r--.   1 root root    7 Jan  1 18:48 linux.txt
dr-xr-x---.   5 root root  240 Jan  1 16:01 rootdir
drwxr-xr-x.   5 root root 4096 Jan  1 21:18 sysconfig-bak
drwxr-xr-x.   2 root root   32 Jan  1 21:22 test
-rw-r--r--.   1 root root    6 Jan  1 18:51 win.txt
[root@sh-lzy-Centos8 23:27 data]#

#第三步：执行文件
[root@sh-lzy-Centos8 23:27 data]#bash hello.sh 
hello,world!
[root@sh-lzy-Centos8 23:29 data]#
```

#### 6.2.6 shell 脚本调试

只检测脚本中的语法错误，但无法检查出命令错误，但不真正执行脚本

```
bash -n /path/to/some_script
```

调试并执行

```
bash -x /path/to/some_script
```

**总结：脚本错误常见的有三种** 

* 语法错误，会导致后续的命令不继续执行，可以用bash -n 检查错误，提示的出错行数不一定是准 确的 
* 命令错误，默认后续的命令还会继续执行，用bash -n 无法检查出来 ，可以使用 bash -x 进行观察 
* 逻辑错误：只能使用 bash -x 进行观察

#### 6.2.7 变量

##### 6.2.7.1 变量

变量表示命名的内存空间，将数据放在内存空间中，通过变量名引用，获取数据

##### 6.2.7.2 变量类型

变量类型： 

* 内置变量，如：PS1，PATH，UID，HOSTNAME，$$，BASHPID，PPID，$?，HISTSIZE 
* 用户自定义变量

不同的变量存放的数据不同，决定了以下 

* 数据存储方式 
* 参与的运算 
* 表示的数据范围

变量数据类型： 

* 字符 
* 数值：整型、浮点型,bash 不支持浮点数

##### 6.2.7.3 编程语言分类

**静态和动态语言** 

* 静态编译语言：使用变量前，先声明变量类型，之后类型不能改变，在编译时检查，如：java，c 
* 动态编译语言：不用事先声明，可随时改变类型，如：bash，Python

**强类型和弱类型语言** 

* 强类型语言：不同类型数据操作，必须经过强制转换才同一类型才能运算，如java ， c# ， python 

  如：参考以下 python 代码 

  print('magedu'+ 10) 提示出错，不会自动转换类型 

  print('magedu'+str(10)) 结果为magedu10，需要显示转换类型 

* 弱类型语言：语言的运行时会隐式做数据类型转换。无须指定类型，默认均为字符型；参与运算会 自动进行隐式类型转换；变量无须事先定义可直接调用 如：bash ，php，javascript

##### 6.2.7.4 Shell中变量命名法则

###### 6.2.7.4.1 命名要求

* 区分大小写 
* 不能使程序中的保留字和内置变量：如：if, for
* 只能使用数字、字母及下划线，且不能以数字开头，注意：不支持短横线 “ - ”，和主机名相反

###### 6.2.7.4.2 命名习惯

* 见名知义，用英文单词命名，并体现出实际作用，不要用简写，如：ATM 
* 变量名大写 
* 局部变量小写 
* 函数名小写 
* 大驼峰StudentFirstName  
* 小驼峰studentFirstName  
* 下划线: student_name

##### 6.2.7.5 变量定义和引用

变量的生效范围等标准划分变量类型 

* 普通变量：生效范围为当前shell进程；对当前shell之外的其它shell进程，包括当前shell的子shell 进程均无效 
* 环境变量：生效范围为当前shell进程及其子进程 
* 本地变量：生效范围为当前shell进程中某代码片断，通常指函数

**变量赋值：**

```bash
name='value'
```

value 可以是以下多种形式

```
直接字串：name='root'
变量引用：name="$USER"
命令引用：name=`COMMAND` 或者 name=$(COMMAND)
```

**注意：**变量赋值是临时生效，当退出终端后，变量会自动删除，无法持久保存，脚本中的变量会随着脚 本结束，也会自动删除

**变量引用：**

```
$name
${name} 
```

弱引用和强引用 

* "$name " 弱引用，其中的变量引用会被替换为变量值 

* '$name ' 强引用，其中的变量引用不会被替换为变量值，而保持原字符串

显示已定义的所有变量：

```bash
set
```

**删除变量：**

```bash
unset <name>
```

**练习：** 

 1、编写脚本 systeminfo.sh，显示当前主机系统信息，包括:主机名，IPv4地址，操作系统版本，内核 版本，CPU型号，内存大小，硬盘大小

2、编写脚本 backup.sh，可实现每日将 /etc/ 目录备份到 /backup/etcYYYY-mm-dd中 

3、编写脚本 disk.sh，显示当前硬盘分区中空间利用率最大的值 

4、编写脚本 links.sh，显示正连接本主机的每个远程主机的IPv4地址和连接数，并按连接数从大到小排 序

##### 6.2.7.6 环境变量

**环境变量：** 

* 可以使子进程（包括孙子进程）继承父进程的变量，但是无法让父进程使用子进程的变量 
* 一旦子进程修改从父进程继承的变量，将会新的值传递给孙子进程 
* 一般只在系统配置文件中使用，在脚本中较少使用

变量声明和赋值：

```bash
#声明并赋值
export name=VALUE
declare -x name=VALUE

#或者分两步实现
name=VALUE
export name
```

变量引用：

```bash
$name
${name}
```

显示所有环境变量：

```bash
env
printenv
export
declare -x
```

查看指定进程的环境变量：

```bash
cat /proc/$PID/environ
```

删除变量：

```bash
unset name
```

bash内建的环境变量：

```bash
PATH		# 环境变量
SHELL
USER
UID
HOME
PWD			#当前工作路径
SHLVL 		#shell的嵌套层数，即深度
LANG
MAIL
HOSTNAME
HISTSIZE
_   		#下划线,表示前一命令的最后一个参数
```

##### 6.2.7.7 只读变量（常量）

只读变量：只能声明定义，但后续不能修改和删除，即常量

声明只读变量：

```bash
readonly name
declare -r name
```

查看只读变量：

```bash
readonly [-p]
declare -r
```

##### 6.2.7.8 位置变量

位置变量：在bash shell中内置的变量, 在脚本代码中调用通过命令行传递给脚本的参数

```
$1, $2, ... 对应第1个、第2个等参数，shift [n]换位置
$0 命令本身,包括路径
$* 传递给脚本的所有参数，全部参数合为一个字符串
$@ 传递给脚本的所有参数，每个参数为独立字符串
$# 传递给脚本的参数的个数
注意：$@ $* 只在被双引号包起来的时候才会有差异
```

清空所有位置变量

```bash
set --
```

##### 6.2.7.9 退出状态码变量

进程执行后，将使用变量 $? 保存状态码的相关数字，不同的值反应成功或失败，$?取值范例 0-255 

```bash
$?的值为0 			#代表成功
$?的值是1到255   	#代表失败
```

用户可以在脚本中使用以下命令自定义退出状态码

```bash
exit [n]
```

**注意：** 

* 脚本中一旦遇到exit命令，脚本会立即终止；终止退出状态取决于exit命令后面的数字 
* 如果exit后面无数字,终止退出状态取决于exit命令前面命令执行结果 
* 如果没有exit命令, 即未给脚本指定退出状态码，整个脚本的退出状态码取决于脚本中执行的最后 一条命令的状态码

##### 6.2.7.10 展开命令行

**展开命令执行顺序**

```
把命令行分成单个命令词
展开别名
展开大括号的声明{}
展开波浪符声明 ~
命令替换$() 和 ``
再次把命令行分成命令词
展开文件通配*、?、[abc]等等
准备I/0重导向 <、>
运行命令
```

**防止扩展**

```
反斜线（\）会使随后的字符按原意解释
```

**加引号来防止扩展**

```
单引号（’’）防止所有扩展
双引号（”“）也可防止扩展，但是以下情况例外：$（美元符号）
```

**变量扩展**

```
`` : 反引号，命令替换
\：反斜线，禁止单个字符扩展
!：叹号，历史命令替换
```

##### 6.2.7.11 脚本安全和 set

set 命令：可以用来定制 shell 环境

**$- 变量** 

* h：hashall，打开选项后，Shell 会将命令所在的路径hash下来，避免每次都要查询。通过set +h将h选 项关闭 
* i：interactive-comments，包含这个选项说明当前的 shell 是一个交互式的 shell。所谓的交互式shell, 在脚本中，i选项是关闭的 
* m：monitor，打开监控模式，就可以通过Job control来控制进程的停止、继续，后台或者前台执行等 
* B：braceexpand，大括号扩展 
* H：history，H选项打开，可以展开历史列表中的命令，可以通过!感叹号来完成，例如“!!”返回上最近的 一个历史命令，“!n”返回第 n 个历史命令

**set 命令实现脚本安全** 

* -u 在扩展一个没有设置的变量时，显示错误信息， 等同set -o nounset 

* -e 如果一个命令返回一个非0退出状态值(失败)就退出， 等同set -o errexit 

* -o option 显示，打开或者关闭选项 

  显示选项：set -o 

  打开选项：set -o 选项 

  关闭选项：set +o 选项

* -x 当执行命令时，打印命令及其参数,类似 bash -x 

#### 6.2.8 格式化输出 printf

```bash
printf "指定的格式" "文本1" ”文本2“……
```

**常用格式替换符**

| 替换符 |                             功能                             |
| :----: | :----------------------------------------------------------: |
|   %s   |                            字符串                            |
| %d,%i  |                          十进制整数                          |
|   %f   |                           浮点格式                           |
|   %c   |            ASCII字符，即显示对应参数的第一个字符             |
|   %b   | 相对应的参数中包含转义字符时，可以使用此替换符进行替换，对应的转义字符会被转 义 |
|   %o   |                           八进制值                           |
|   %u   |                     不带正负号的十进制值                     |
|   %x   |                      十六进制值（a-f）                       |
|   %X   |                      十六进制值（A-F）                       |
|   %%   |                          表示%本身                           |

**说明：**

```
%#s 中的数字代表此替换符中的输出字符宽度，不足补空格，默认是右对齐,%-10s表示10个字符宽，- 表示
左对齐
%03d 表示3位宽度,不足前面用0补全,超出位数原样输出
%.2f 中的2表示小数点后显示的小数位数
```

**常用转义字符**

| 转义符 |              功能              |
| :----: | :----------------------------: |
|   \a   | 警告字符，通常为ASCII的BEL字符 |
|   \b   |              后退              |
|   \f   |              换页              |
|   \n   |              换行              |
|   \r   |              回车              |
|   \t   |           水平制表符           |
|   \v   |           垂直制表符           |
|   \    |           表示\本身            |

#### 6.2.9 算术运算

Shell允许在某些情况下对算术表达式进行求值，比如：let和declare 内置命令，(( ))复合命令和算术扩 展。求值以固定宽度的整数进行，不检查溢出，尽管除以0 被困并标记为错误。运算符及其优先级，关 联性和值与C语言相同。以下运算符列表分组为等优先级运算符级别。级别按降序排列优先。

**注意：bash 只支持整数，不支持小数**

```
* / % multiplication, division, remainder, %表示取模，即取余数，示例：9%4=1，5%3=2
+ -   addition, subtraction
i++ i-- variable post-increment and post-decrement
++i --i variable pre-increment and pre-decrement
= *= /= %= += -= <<= >>= &= ^= |=         assignment
- +   unary minus and plus
! ~   logical and bitwise negation
**     exponentiation 乘方,即指数运算
<< >> left and right bitwise shifts
<= >= < >       comparison
== != equality and inequality
&     bitwise AND
|     bitwise OR
^     bitwise exclusive OR
&&     logical AND
||     logical OR
expr?expr:expr       conditional operator
expr1 , expr2     comma
```

乘法符号有些场景中需要转义 

实现算术运算：

```
(1) let var=算术表达式
(2) ((var=算术表达式)) 和上面等价
(3) var=$[算术表达式]
(4) var=$((算术表达式))
(5) var=$(expr arg1 arg2 arg3 ...)
(6) declare -i var = 数值
(7) echo '算术表达式' | bc 
```

内建的随机数生成器变量：

```http
$RANDOM   取值范围：0-32767
```

增强型赋值：

```
+= i+=10 相当于 i=i+10
-= i-=j   相当于 i=i-j
*=
/=
%=
++ i++,++i   相当于 i=i+1
-- i--,--i   相当于 i=i-1
```

格式：

```bash
let varOPERvalue
```

#### 6.2.10 逻辑运算

​					**与逻辑**																**或逻辑**													 **非逻辑**

|  A   |  B   | Fault |      |  A   |  B   | Fault |      |  A   | Fault |
| :--: | :--: | :---: | ---- | :--: | :--: | :---: | ---- | :--: | :---: |
|  0   |  0   |   0   |      |  0   |  0   |   0   |      |  1   |   0   |
|  0   |  1   |   0   |      |  0   |  1   |   1   |      |  0   |   1   |
|  1   |  0   |   0   |      |  1   |  0   |   1   |      |      |       |
|  1   |  1   |   1   |      |  1   |  1   |   1   |      |      |       |

**true, false**

```
1,真
0,假
#注意,以上为二进制
```

**与：&：和0相与结果为0，和1相与结果保留原值, 一假则假,全真才真**

**或：|：和1相或结果为1，和0相或结果保留原值,一真则真,全假才假**

**非：！**

**异或：^**

异或的两个值，相同为假，不同为真。两个数字X,Y异或得到结果Z，Z再和任意两者之一X异或，将得出 另一个值Y

```
0 ^ 0 = 0
0 ^ 1 = 1
1 ^ 0 = 1
1 ^ 1 = 0
```

**短路运算**

* 短路与 &&

```
CMD1 短路与 CMD2
第一个CMD1结果为真 (1)，第二个CMD2必须要参与运算，才能得到最终的结果 
第一个CMD1结果为假 (0)，总的结果必定为0，因此不需要执行CMD2
```

* 短路或 ||

```
CMD1 短路或 CMD2
第一个CMD1结果为真 (1)，总的结果必定为1，因此不需要执行CMD2
第一个CMD1结果为假 (0)，第二个CMD2 必须要参与运算,才能得到最终的结果
```

#### 6.2.11 条件测试命令

条件测试：判断某需求是否满足，需要由测试机制来实现，专用的测试表达式需要由测试命令辅助完成 测试过程，实现评估布尔声明，以便用在条件性环境下进行执行

若真，则状态码变量 $? 返回0 

若假，则状态码变量 $? 返回1

条件测试命令

* test EXPRESSION 
* [ EXPRESSION ] #和test 等价，建议使用 [ ] 
* [[ EXPRESSION ]] 相关于增强版的 [ ], 支持[]的用法,且支持扩展正则表达式和通配符

注意：EXPRESSION前后必须有空白字符

##### 6.2.11.1 变量测试

```
#判断 NAME 变量是否定义
[ -v NAME ] 
#判断 NAME 变量是否定义并且是名称引用,bash 4.4新特性
[ -R NAME ]
```

##### 6.2.11.2 数值测试

```
-eq 是否等于
-ne 是否不等于
-gt 是否大于
-ge 是否大于等于
-lt 是否小于
-le 是否小于等于
```

算术表达式比较

```
==   #相等
!=   #不相等
<= 
>= 
< 
> 
```

##### 6.2.11.3 字符串测试

**test和 [ ]用法**

```
test和 [ ]用法
-z STRING 字符串是否为空，没定义或空为真，不空为假，
-n STRING 字符串是否不空，不空为真，空为假 
   STRING   同上
STRING1 = STRING2 是否等于，注意 = 前后有空格
STRING1 != STRING2 是否不等于
> ascii码是否大于ascii码
< 是否小于
```

**[[]] 用法**

```
[[ expression ]] 用法
== 左侧字符串是否和右侧的PATTERN相同
 注意:此表达式用于[[ ]]中，PATTERN为通配符
=~ 左侧字符串是否能够被右侧的正则表达式的PATTERN所匹配
 注意: 此表达式用于[[ ]]中；扩展的正则表达式
```

**建议：当使用正则表达式或通配符使用[[ ]]，其它情况一般使用 [ ]**

##### 6.2.11.4 文件测试

存在性测试：

```bash
-a FILE：同 -e
-e FILE: 文件存在性测试，存在为真，否则为假
-b FILE：是否存在且为块设备文件
-c FILE：是否存在且为字符设备文件
-d FILE：是否存在且为目录文件
-f FILE：是否存在且为普通文件
-h FILE 或 -L FILE：存在且为符号链接文件
-p FILE：是否存在且为命名管道文件
-S FILE：是否存在且为套接字文件
```

文件权限测试：

```bash
-r FILE：是否存在且可读
-w FILE: 是否存在且可写
-x FILE: 是否存在且可执行
-u FILE：是否存在且拥有suid权限
-g FILE：是否存在且拥有sgid权限
-k FILE：是否存在且拥有sticky权限
```

**注意：最终结果由用户对文件的实际权限决定，而非文件属性决定**

文件属性测试：

```bash
-s FILE 			#是否存在且非空
-t fd 				#fd 文件描述符是否在某终端已经打开
-N FILE 			#文件自从上一次被读取之后是否被修改过
-O FILE 			#当前有效用户是否为文件属主
-G FILE 			#当前有效用户是否为文件属组
FILE1 -ef FILE2 	#FILE1是否是FILE2的硬链接
FILE1 -nt FILE2 	#FILE1是否新于FILE2（mtime）
FILE1 -ot FILE2 	#FILE1是否旧于FILE2
```

#### 6.2.12 关于 () 和 {}

（CMD1;CMD2;...）和 { CMD1;CMD2;...; } 都可以将多个命令组合在一起，批量执行

```bash
[root@sh-lzy-Centos8 13:38 data]#man bash
......
   Compound Commands
       A  compound  command  is  one of the following.  In most cases a list in a command's description may be
       separated from the rest of the command by one or more newlines, and may be followed  by  a  newline  in
       place of a semicolon.

       (list) list  is executed in a subshell environment (see COMMAND EXECUTION ENVIRONMENT below).  Variable
              assignments and builtin commands that affect the shell's environment do  not  remain  in  effect
              after the command completes.  The return status is the exit status of list.

       { list; }
              list  is  simply executed in the current shell environment.  list must be terminated with a new‐
              line or semicolon.  This is known as a group command.  The return status is the exit  status  of
              list.   Note  that  unlike the metacharacters ( and ), { and } are reserved words and must occur
              where a reserved word is permitted to be recognized.  Since they do not cause a word break, they
              must be separated from list by whitespace or another shell metacharacter.
......
```

```
( list ) 会开启子shell,并且list中变量赋值及内部命令执行后,将不再影响后续的环境
帮助参看:man bash 搜索(list)
{ list; } 不会启子shell, 在当前shell中运行,会影响当前shell环境
帮助参看:man bash 搜索{ list; }
```

#### 6.2.13 组合测试条件

##### 6.2.13.1 第一种方式

```
[ EXPRESSION1 -a EXPRESSION2 ] 并且，EXPRESSION1和EXPRESSION2都是真，结果才为真
[ EXPRESSION1 -o EXPRESSION2 ] 或者，EXPRESSION1和EXPRESSION2只要有一个真，结果就为
真
[ ! EXPRESSION ] 取反
```

**说明： -a 和 -o 需要使用测试命令进行，[[ ]] 不支持**

##### 6.2.13.2 第二种方式

```
COMMAND1 && COMMAND2 #并且，短路与，代表条件性的AND THEN
如果COMMAND1 成功,将执行COMMAND2,否则,将不执行COMMAND2

COMMAND1 || COMMAND2 #或者，短路或，代表条件性的OR ELSE
如果COMMAND1 成功,将不执行COMMAND2,否则,将执行COMMAND2

! COMMAND   #非,取反
```

**练习** 

1、编写脚本 argsnum.sh，接受一个文件路径作为参数；如果参数个数小于1，则提示用户“至少应该给 一个参数”，并立即退出；如果参数个数不小于1，则显示第一个参数所指向的文件中的空白行数 

2、编写脚本 hostping.sh，接受一个主机的IPv4地址做为参数，测试是否可连通。如果能ping通，则提示用户“该IP地址可访问”；如果不可ping通，则提示用户“该IP地址不可访问” 

3、编写脚本 checkdisk.sh，检查磁盘分区空间和inode使用率，如果超过80%，就发广播警告空间将满 

4、编写脚本 per.sh，判断当前用户对指定参数文件，是否不可读并且不可写 

5、编写脚本 excute.sh ，判断参数文件是否为sh后缀的普通文件，如果是，添加所有人可执行权限， 否则提示用户非脚本文件 

6、编写脚本 nologin.sh和 login.sh，实现禁止和允许普通用户登录系统

#### 6.2.14 使用read命令来接受输入

使用read来把输入值分配给一个或多个shell变量，read从标准输入中读取值，给每个单词分配一个变 量，所有剩余单词都被分配给最后一个变量，如果变量名没有指定，默认标准输入的值赋值给系统内置 变量REPLY

```
read [options] [name ...]
```

常见选项：

```
-p   指定要显示的提示
-s   静默输入，一般用于密码
-n N 指定输入的字符长度N
-d '字符'   输入结束符
-t N TIMEOUT为N秒
```

### 6.3 bash shell 的配置文件

bash shell的配置文件很多，可以分成下面类别

#### 6.3.1 按生效范围划分两类

全局配置：针对所有用户皆有效

```
/etc/profile
/etc/profile.d/*.sh
/etc/bashrc
```

个人配置：只针对特定用户有效

```
~/.bash_profile
~/.bashrc
```

#### 6.3.2 shell登录两种方式分类

##### 6.3.2.1 交互式登录

* 直接通过终端输入账号密码登录 
* 使用 su - UserName 切换的用户

**配置文件生效和执行顺序：**

```bash
#放在每个文件最前
/etc/profile
/etc/profile.d/*. sh
/etc/bashrc
~/ .bash_ profile
~/ .bashrc
/etc/bashrc

#放在每个文件最后
/etc/profile.d/*.sh
/etc/bashrc
/etc/profile
/etc/bashrc    #此文件执行两次
~/.bashrc
~/.bash_profile
```

**注意：文件之间的调用关系，写在同一个文件的不同位置，将影响文件的执行顺序**

##### 6.3.2.2 非交互式登录

* su UserName 
* 图形界面下打开的终端 
* 执行脚本 
* 任何其它的bash实例

执行顺序：

```bash
/etc/profile.d/*.sh
/etc/bashrc
~/.bashrc
```

#### 6.3.3 按功能划分分类

profile类和bashrc类

##### 6.3.3.1 Profile类 

profile类为交互式登录的shell提供配置

```bash
全局：/etc/profile, /etc/profile.d/*.sh
个人：~/.bash_profile
```

功用：

(1) 用于定义环境变量 

(2) 运行命令或脚本

##### 6.3.3.2 Bashrc类

bashrc类：为非交互式和交互式登录的shell提供配置

```
全局：/etc/bashrc
个人：~/.bashrc
```

功用： 

(1) 定义命令别名和函数 

(2) 定义本地变量

#### 6.3.4 编辑配置文件生效

修改profile和bashrc文件后需生效两种方法: 

1. 重新启动shell进程 

​	2. source|. 配置文件

注意:source 会在当前shell中执行脚本,所有一般只用于执行置文件,或在脚本中调用另一个脚本的场景

#### 6.3.5 Bash 退出任务

保存在~/.bash_logout文件中（用户）,在退出登录shell时运行

功能： 

* 创建自动备份 
* 清除临时文件

**练习** 

1、让所有用户的PATH环境变量的值多出一个路径，例如：/usr/local/apache/bin 

2、用户 root 登录时，将命令指示符变成红色，并自动启用如下别名： 

​	rm=‘rm -i’ 

​	cdnet=‘cd /etc/sysconfig/network-scripts/’ 

​	editnet=‘vim /etc/sysconfig/network-scripts/ifcfg-eth0’ 

​	editnet=‘vim /etc/sysconfig/network-scripts/ifcfg-eno16777736 或 ifcfg-ens33 ’ (如果系统是 CentOS7) 

3、任意用户登录系统时，显示红色字体的警示提醒信息“Hi,dangerous！” 

4、编写生成脚本基本格式的脚本，包括作者，联系方式，版本，时间，描述等

### 6.4 流程控制

#### 6.4.1 条件选择

##### 6.4.1.1 条件判断分绍

###### 6.4.1.1.1 单分支条件

![](C:\Users\Administrator\Desktop\SRE笔记图片\6.4.1.1.1 单分支条件.jpg)

###### 6.4.1.1.2 多分支条件

![](C:\Users\Administrator\Desktop\SRE笔记图片\6.4.1.1.2 多分支条件.jpg)

![](C:\Users\Administrator\Desktop\SRE笔记图片\6.4.1.1.2 多分支条件-2.jpg)

##### 6.4.1.2 选择执行 if 语句

格式：

```bash
if COMMANDS; then COMMANDS; [ elif COMMANDS; then COMMANDS; ]... [ else 
COMMANDS; ] fi
```

**单分支**

```bash
if 判断条件;then
   条件为真的分支代码
fi
```

**双分支**

```bash
if 判断条件; then
 条件为真的分支代码
else
 条件为假的分支代码
fi
```

**多分支**

```bash
if 判断条件1; then
 条件1为真的分支代码
elif 判断条件2; then
 条件2为真的分支代码
elif 判断条件3; then
 条件3为真的分支代码
...
else
 以上条件都为假的分支代码
fi
```

说明： 

* 多个条件时，逐个条件进行判断，第一次遇为“真”条件时，执行其分支，而后结束整个if语句

* if 语句可嵌套

##### 6.4.1.3 条件判断 case 语句

格式：

```bash
case WORD in [PATTERN [| PATTERN]...) COMMANDS ;;]... esac
```

```bash
case 变量引用 in
PAT1)
 分支1
 ;;
PAT2)
 分支2
 ;;
...
*)
 默认分支
 ;;
esac
```

case支持glob风格的通配符：

```
* 任意长度任意字符
? 任意单个字符
[] 指定范围内的任意单个字符
|   或者，如: a|b
```

**练习 **

1、编写脚本 createuser.sh，实现如下功能：使用一个用户名做为参数，如果指定参数的用户存在，就 显示其存在，否则添加之。并设置初始密码为123456，显示添加的用户的id号等信息，在此新用户第一 次登录时，会提示用户立即改密码，如果没有参数，就提示：请输入用户名 

2、编写脚本 yesorno.sh，提示用户输入yes或no,并判断用户输入的是yes还是no,或是其它信息 

3、编写脚本 filetype.sh，判断用户输入文件路径，显示其文件类型（普通，目录，链接，其它文件类 型） 

4、编写脚本 checkint.sh，判断用户输入的参数是否为正整数 

5、编写脚本 reset.sh，实现系统安装后的初始化环境，包括：1、别名 2、环境变量，如PS1等 3、 安装常用软件包，如：tree 5、实现固定的IP的设置，6、vim的设置等

#### 6.4.2 循环

##### 6.4.2.1 循环执行介绍

将某代码段重复运行多次，通常有进入循环的条件和退出循环的条件

重复运行次数 

* 循环次数事先已知 
* 循环次数事先未知

常见的循环的命令：for, while, until

![](C:\Users\Administrator\Desktop\SRE课程资料\SRE笔记图片\6.4.2.1 循环执行介绍.jpg)

##### 6.4.2.2 循环 for

**格式1：**

```bash
for NAME [in WORDS ... ] ; do COMMANDS; done
#方式1
for 变量名  in 列表;do
 循环体
done
#方式2
for 变量名  in 列表
do
 循环体
done
```

**执行机制：** 

* 依次将列表中的元素赋值给“变量名”; 每次赋值后即执行一次循环体; 直到列表中的元素耗尽，循环 结束 
* 如果省略 [in WORDS ... ] ，此时使用位置参数变量 in "$@"

**for 循环列表生成方式：**

* 直接给出列表 
* 整数列表：

```
{start..end}
$(seq [start [step]] end)
```

* 返回列表的命令:

```
$(COMMAND)
```

* 使用glob，如：*.sh *
* *变量引用，如：$@，$*，$#

**格式2**

双小括号方法，即((…))格式，也可以用于算术运算，双小括号方法也可以使bash Shell实现C语言风格

的变量操作

I=10;((I++)) 

![](C:\Users\Administrator\Desktop\SRE课程资料\SRE笔记图片\6.4.2.2循环for.jpg)

```bash
for ((: for (( exp1; exp2; exp3 )); do COMMANDS; done
for ((控制变量初始化;条件判断表达式;控制变量的修正表达式))
do
 循环体
done
```

说明：

* 控制变量初始化：仅在运行到循环代码段时执行一次

* 控制变量的修正表达式：每轮循环结束会先进行控制变量修正运算，而后再做条件判断

**练习：用 for 实现**

1、判断/var/目录下所有文件的类型

2、添加10个用户user1-user10，密码为8位随机字符

3、/etc/rc.d/rc3.d目录下分别有多个以K开头和以S开头的文件；分别读取每个文件，以K开头的输出为文件加stop，以S开头的输出为文件名加start，如K34filename stop S66filename start

4、编写脚本，提示输入正整数n的值，计算1+2+…+n的总和

5、计算100以内所有能被3整除的整数之和

6、编写脚本，提示请输入网络地址，如192.168.0.0，判断输入的网段中主机在线状态

7、打印九九乘法表

8、在/testdir目录下创建10个html文件,文件名格式为数字N（从1到10）加随机8个字母，如：1AbCdeFgH.html

9、打印等腰三角形10、猴子第一天摘下若干个桃子，当即吃了一半，还不瘾，又多吃了一个。第二天早上又将剩下的桃子吃掉一半，又多吃了一个。以后每天早上都吃了前一天剩下的一半零一个。到第10天早上想再吃时，只剩下一个桃子了。求第一天共摘了多少？

##### 6.4.2.3 循环 while

格式：

```bash
while COMMANDS; do COMMANDS; done
while CONDITION; do
 循环体
done
```

说明：

CONDITION：循环控制条件；进入循环之前，先做一次判断；每一次循环之后会再次做判断；条件为“true”，则执行一次循环；直到条件测试状态为“false”终止循环，因此：CONDTION一般应该有循环控制变量；而此变量的值会在循环体不断地被修正

进入条件：CONDITION为 true

退出条件：CONDITION为 false

**无限循环**

```bash
while true; do
 循环体
done
while : ; do
 循环体
done
```

**练习：用while实现**

1、编写脚本，求100以内所有正奇数之和

2、编写脚本，提示请输入网络地址，如:192.168.0.0，判断输入的网段中主机在线状态，并统计在线和离线主机各多少

3、编写脚本，打印九九乘法表

4、编写脚本，利用变量RANDOM生成10个随机数字，输出这个10数字，并显示其中的最大值和最小值

5、编写脚本，实现打印国际象棋棋盘6、后续六个字符串：efbaf275cd、4be9c40b8b、44b2395c46、f8c8873ce0、b902c16c8b、ad865d2f63是通过对随机数变量RANDOM随机执行命令： echo $RANDOM|md5sum|cut -c1-10 后的结果，请破解这些字符串对应的RANDOM值

##### 6.4.2.4 循环 until

格式：

```bash
until COMMANDS; do COMMANDS; done
until CONDITION; do
 循环体
done
```

说明：

进入条件： CONDITION 为false

退出条件： CONDITION 为true

**无限循环**

```bash
until false; do
 循环体
Done
```

##### 6.4.2.5 循环控制语句 continue

continue [N]：提前结束第N层的本轮循环，而直接进入下一轮判断；最内层为第1层

格式：

```bash
while CONDITION1; do
 CMD1
 ...
 if CONDITION2; then
 continue
 fi
 CMDn
 ...
done
```

##### 6.4.2.6 循环控制语句 break

break [N]：提前结束第N层整个循环，最内层为第1层

格式：

```bash
while CONDITION1; do
 CMD1
 ...
 if CONDITION2; then
 break
 fi
 CMDn
 ...
done
```

##### 6.4.2.7 循环控制 shift 命令

shift [n] 用于将参量列表 list 左移指定次数，缺省为左移一次。

参量列表 list 一旦被移动，最左端的那个参数就从列表中删除。while 循环遍历位置参量列表时，常用到 shift

**练习**

1、每隔3秒钟到系统上获取已经登录的用户的信息；如果发现用户hacker登录，则将登录时间和主机记录于日志/var/log/login.log中,并退出脚本

2、随机生成10以内的数字，实现猜字游戏，提示比较大或小，相等则退出

3、用文件名做为参数，统计所有参数文件的总行数

4、用二个以上的数字为参数，显示其中的最大值和最小值

##### 6.4.2.8 while 特殊用法 while read

while 循环的特殊用法，遍历文件或文本的每一行

格式：

```bash
while read line; do
 循环体
done < /PATH/FROM/SOMEFILE
```

说明：依次读取/PATH/FROM/SOMEFILE文件中的每一行，且将行赋值给变量line

##### 6.4.2.9 循环与菜单 select

格式：

```bash
select NAME [in WORDS ... ;] do COMMANDS; done
select variable in list ;do 
 循环体命令
done
```

说明：

* select 循环主要用于创建菜单，按数字顺序排列的菜单项显示在标准错误上，并显示 PS3 提示符，等待用输入

* 用户输入菜单列表中的某个数字，执行相应的命令

* 用户输入被保存在内置变量 REPLY 中

* select 是个无限循环，因此要用 break 命令退出循环，或用 exit 命令终止脚本。也可以按 ctrl+c 退出循环

* select 经常和 case 联合使用

* 与 for 循环类似，可以省略 in list，此时使用位置参量

### 6.5 函数 function

#### 6.5.1 函数介绍

函数function是由若干条shell命令组成的语句块，实现代码重用和模块化编程

它与shell程序形式上是相似的，不同的是它不是一个单独的进程，不能独立运行，而是shell程序的一部分

**函数和shell程序区别**

* Shell程序在子Shell中运行

* 函数在当前Shell中运行。因此在当前Shell中，函数可对shell中变量进行修改

#### 6.5.2 管理函数

函数由两部分组成：函数名和函数体

帮助参看：help function

##### 6.5.2.1 定义函数

```bash
#语法一：
func_name （）{
 ...函数体...
}#语法二：
function func_name {
 ...函数体...
} 
#语法三：
function func_name （） {
 ...函数体...
}
```

##### 6.5.2.2 查看函数

```bash
#查看当前已定义的函数名
declare -F
#查看当前已定义的函数定义
declare -f
#查看指定当前已定义的函数名
declare -f func_name 
#查看当前已定义的函数名定义
declare -F func_name
```

##### 6.5.2.3 删除函数

格式：

```bash
unset func_name
```

#### 6.5.3 函数调用

函数的调用方式

* 可在交互式环境下定义函数

* 可将函数放在脚本文件中作为它的一部分

* 可放在只包含函数的单独文件中

调用：函数只有被调用才会执行，通过给定函数名调用函数，函数名出现的地方，会被自动替换为函数代码

函数的生命周期：被调用时创建，返回时终止

##### 6.5.3.1 交互式环境调用函数

交互式环境下定义和使用函数

##### 6.5.3.2 在脚本中定义及使用函数

函数在使用前必须定义，因此应将函数定义放在脚本开始部分，直至shell首次发现它后才能使用，调用函数仅使用其函数名即可

##### 6.5.3.3 使用函数文件

可以将经常使用的函数存入一个单独的函数文件，然后将函数文件载入shell，再进行调用函数

函数文件名可任意选取，但最好与相关任务有某种联系，例如：functions

一旦函数文件载入shell，就可以在命令行或脚本中调用函数。可以使用delcare -f 或set 命令查看所有定义的函数，其输出列表包括已经载入shell的所有函数

若要改动函数，首先用unset命令从shell中删除函数。改动完毕后，再重新载入此文件

实现函数文件的过程：

​	1. 创建函数文件，只存放函数的定义

​	2. 在shell脚本或交互式shell中调用函数文件，格式如下：

```bash
. filename 
source   filename  
```

#### 6.5.4 函数返回值

函数的执行结果返回值：

* 使用echo等命令进行输出

* 函数体中调用命令的输出结果

函数的退出状态码：

* 默认取决于函数中执行的最后一条命令的退出状态码

* 自定义退出状态码，其格式为：

​		return 从函数中返回，用最后状态命令决定返回值

​		return 0 无错误返回

​		return 1-255 有错误返回

#### 6.5.5 环境函数

类拟于环境变量，也可以定义环境函数，使子进程也可使用父进程定义的函数

定义环境函数：

```bash
export -f function_name 
declare -xf function_name
```

查看环境函数：

```bash
export -f 
declare -xf
```

#### 6.5.6 函数参数

函数可以接受参数：

* 传递参数给函数：在函数名后面以空白分隔给定参数列表即可，如：testfunc arg1 arg2 ...

* 在函数体中当中，可使用$1, $2, ...调用这些参数；还可以使用$@, $*, $#等特殊变量

#### 6.5.7 函数变量

变量作用域：

* 普通变量：只在当前shell进程有效，为执行脚本会启动专用子shell进程；因此，本地变量的作用范围是当前shell脚本程序文件，包括脚本中的函数

* 环境变量：当前shell和子shell有效

* 本地变量：函数的生命周期；函数结束时变量被自动销毁

注意：

* 如果函数中定义了普通变量，且名称和局部变量相同，则使用本地变量

* 由于普通变量和局部变量会冲突，建议在函数中只使用本地变量

在函数中定义本地变量的方法

```bash
local NAME=VALUE
```

#### 6.5.8 函数递归

函数递归：函数直接或间接调用自身，注意递归层数，可能会陷入死循环

递归特点: 

* 函数内部自已调用自已

* 必须有结束函数的出口语句,防止死循环

**递归示例：**

阶乘是基斯顿·卡曼于 1808 年发明的运算符号，是数学术语

一个正整数的阶乘（factorial）是所有小于及等于该数的正整数的积，并且0和1的阶乘为1，自然数n的

阶乘写作n!

n!=1×2×3×...×n

阶乘亦可以递归方式定义：0!=1，n!=(n-1)!×n

n!=n(n-1)(n-2)...1

n(n-1)! = n(n-1)(n-2)!

**练习**

1. 编写函数，实现OS的版本判断

2. 编写函数，实现取出当前系统eth0的IP地址

3. 编写函数，实现打印绿色OK和红色FAILED

4. 编写函数，实现判断是否无位置参数，如无参数，提示错误

5. 编写函数，实现两个数字做为参数，返回最大值

6. 编写服务脚本/root/bin/testsrv.sh，完成如下要求

​		(1) 脚本可接受参数：start, stop, restart, status 

​		(2) 如果参数非此四者之一，提示使用格式后报错退出

​		(3) 如是start:则创建/var/lock/subsys/SCRIPT_NAME, 并显示“启动成功”

​			考虑：如果事先已经启动过一次，该如何处理？

​		(4) 如是stop:则删除/var/lock/subsys/SCRIPT_NAME, 并显示“停止完成”

​			考虑：如果事先已然停止过了，该如何处理？

​		(5) 如是restart，则先stop, 再start

​			考虑：如果本来没有start，如何处理？

​		(6) 如是status, 则如果/var/lock/subsys/SCRIPT_NAME文件存在，则显示“SCRIPT_NAME is running...”，如果/var/lock/subsys/SCRIPT_NAME文件不存在，则显示“SCRIPT_NAME is stopped...”

​		(7)在所有模式下禁止启动该服务，可用chkconfig 和 service命令管理

​			说明：SCRIPT_NAME为当前脚本名

7. 编写脚本/root/bin/copycmd.sh

​		(1) 提示用户输入一个可执行命令名称

​		(2) 获取此命令所依赖到的所有库文件列表

​		(3) 复制命令至某目标目录(例如/mnt/sysroot)下的对应路径下

​			 如：/bin/bash ==> /mnt/sysroot/bin/bash

 			/usr/bin/passwd ==> /mnt/sysroot/usr/bin/passwd

​		(4) 复制此命令依赖到的所有库文件至目标目录下的对应路径下： 如：/lib64/ld-linux-x86-64.so.2 ==> /mnt/sysroot/lib64/ld-linux-x86-64.so.2

​		(5)每次复制完成一个命令后，不要退出，而是提示用户键入新的要复制的命令，并重复完成上述功能；直到用户输入quit退出

8. 斐波那契数列又称黄金分割数列，因数学家列昂纳多·斐波那契以兔子繁殖为例子而引入，故又称为“兔子数列”，指的是这样一个数列：0、1、1、2、3、5、8、13、21、34、……，斐波纳契数列以如下被以递归的方法定义：F（0）=0，F（1）=1，F（n）=F(n-1)+F(n-2)（n≥2），利用函数，求n阶斐波那契数列

9. 汉诺塔（又称河内塔）问题是源于印度一个古老传说。大梵天创造世界的时候做了三根金刚石柱子，在一根柱子上从下往上按照大小顺序摞着64片黄金圆盘。大梵天命令婆罗门把圆盘从下面开始按大小顺序重新摆放在另一根柱子上。并且规定，在小圆盘上不能放大圆盘，在三根柱子之间一次只能移动一个圆盘，利用函数，实现N片盘的汉诺塔的移动步骤

### 6.6 其它脚本相关工具

#### 6.6.1 信号捕捉 trap

trap 命令可以捕捉信号,修改信号原来的功能,实现自定义功能

```bash
#进程收到系统发出的指定信号后，将执行自定义指令，而不会执行原操作
trap '触发指令' 信号
#忽略信号的操作
trap '' 信号
 
#恢复原信号的操作
trap '-' 信号
#列出自定义信号操作
trap -p
#当脚本退出时，执行finish函数
trap finish EXIT
```

#### 6.6.2 创建临时文件 mktemp

mktemp 命令用于创建并显示临时文件，可避免冲突

格式：

```
mktemp [OPTION]... [TEMPLATE]
说明：TEMPLATE: filenameXXX，X至少要出现三个
```

常见选项：

```
-d #创建临时目录
-p DIR或--tmpdir=DIR   #指明临时文件所存放目录位置
```

#### 6.6.3 安装复制文件 install

install 功能相当于cp，chmod，chown，chgrp ,mkdir 等相关工具的集合

install命令格式：

```bash
install [OPTION]... [-T] SOURCE DEST 单文件
install [OPTION]... SOURCE... DIRECTORY
install [OPTION]... -t DIRECTORY SOURCE...
install [OPTION]... -d DIRECTORY... #创建空目录
```

选项：

```
-m MODE，默认755
-o OWNER
-g GROUP
-d DIRNAME 目录
```

#### 6.6.4 交互式转化批处理工具 expect

expect 是由Don Libes基于 Tcl（ Tool Command Language ）语言开发的，主要应用于自动化交互式操作的场景，借助 expect 处理交互的命令，可以将交互过程如：ssh登录，ftp登录等写在一个脚本上，使之自动化完成。尤其适用于需要对多台服务器执行相同操作的环境中，可以大大提高系统管理人员的工作效率

expect 语法：

```
expect [选项] [ -c cmds ] [ [ -[f|b] ] cmdfile ] [ args ]
```

常见选项：

```
-c：从命令行执行expect脚本，默认expect是交互地执行的
-d：可以调试信息
```

示例：

```
expect  -c 'expect "\n" {send "pressed enter\n"}'
expect  -d ssh.exp
```

expect中相关命令

* spawn 启动新的进程

* expect 从进程接收字符串

* send 用于向进程发送字符串

* interact 允许用户交互

* exp_continue 匹配多个字符串在执行动作后加此命令

expect最常用的语法(tcl语言:模式-动作)

### 6.7 数组 array

#### 6.7.1 数组介绍

变量：存储单个元素的内存空间

数组：存储多个元素的连续的内存空间，相当于多个变量的集合

数组名和索引

* 索引的编号从0开始，属于数值索引

* 索引可支持使用自定义的格式，而不仅是数值格式，即为**关联索引**，bash 4.0版本之后开始支持

* bash的数组支持稀疏格式（索引不连续）

#### 6.7.2 声明数组

```bash
#普通数组可以不事先声明,直接使用
declare -a ARRAY_NAME
#关联数组必须先声明,再使用
declare -A ARRAY_NAME
```

注意：两者不可相互转换

#### 6.7.3 数组赋值

数组元素的赋值

(1) 一次只赋值一个元素

```
ARRAY_NAME[INDEX]=VALUE
```

(2) 一次赋值全部元素

```
ARRAY_NAME=("VAL1" "VAL2" "VAL3" ...)
```

(3) 只赋值特定元素

```
ARRAY_NAME=([0]="VAL1" [3]="VAL2" ...)
```

(4) 交互式数组值对赋值

```
read -a ARRAY
```

#### 6.7.4 显示所有数组

显示所有数组：

```bash
declare -a
```

#### 6.7.5 引用数组

引用特定的数组元素

```
${ARRAY_NAME[INDEX]}
#如果省略[INDEX]表示引用下标为0的元素
```

引用数组所有元素

```
${ARRAY_NAME[*]}
${ARRAY_NAME[@]}
```

数组的长度，即数组中元素的个数

```
${#ARRAY_NAME[*]}
${#ARRAY_NAME[@]}
```

#### 6.7.6 删除数组

删除数组中的某元素，会导致稀疏格式

```
unset ARRAY[INDEX]
```

删除整个数组

```
unset ARRAY
```

#### 6.7.7 数组数据处理

数组切片：

```
${ARRAY[@]:offset:number}
offset #要跳过的元素个数
number #要取出的元素个数
#取偏移量之后的所有元素 
{ARRAY[@]:offset}
```

向数组中追加元素：

```
ARRAY[${#ARRAY[*]}]=value
ARRAY[${#ARRAY[@]}]=value
```

#### 6.7.8 关联数组

```
declare -A ARRAY_NAME 
ARRAY_NAME=([idx_name1]='val1' [idx_name2]='val2‘...)
```

注意：关联数组必须先声明再调用

#### 6.7.9 范例

### 6.8 字符串处理

#### 6.8.1 字符串切片

##### 6.8.1.1 基于偏移量取字符串

```bash
#返回字符串变量var的字符的长度,一个汉字算一个字符
${#var} 
#返回字符串变量var中从第offset个字符后（不包括第offset个字符）的字符开始，到最后的部分，
offset的取值在0 到 ${#var}-1 之间(bash4.2后，允许为负值)
${var:offset} 
#返回字符串变量var中从第offset个字符后（不包括第offset个字符）的字符开始，长度为number的部分
${var:offset:number}
#取字符串的最右侧几个字符,取字符串的最右侧几个字符, 注意：冒号后必须有一空白字符
${var: -length}
#从最左侧跳过offset字符，一直向右取到距离最右侧lengh个字符之前的内容,即:掐头去尾
${var:offset:-length}
#先从最右侧向左取到length个字符开始，再向右取到距离最右侧offset个字符之间的内容,注意：-
length前空格,并且length必须大于offset
${var: -length:-offset}
```

##### 6.8.1.2 基于模式取子串

```bash
#其中word可以是指定的任意字符,自左而右，查找var变量所存储的字符串中，第一次出现的word, 删除字
符串开头至第一次出现word字符串（含）之间的所有字符,即懒惰模式,以第一个word为界删左留右
${var#*word}
#同上，贪婪模式，不同的是，删除的是字符串开头至最后一次由word指定的字符之间的所有内容,即贪婪模
式,以最后一个word为界删左留右
${var##*word}
```

```
#其中word可以是指定的任意字符,功能：自右而左，查找var变量所存储的字符串中，第一次出现的word, 
删除字符串最后一个字符向左至第一次出现word字符串（含）之间的所有字符,即懒惰模式,以从右向左的第
一个word为界删右留左
${var%word*}
#同上，只不过删除字符串最右侧的字符向左至最后一次出现word字符之间的所有字符,即贪婪模式,以从右向
左的最后一个word为界删右留左
${var%%word*}
```

#### 6.8.2 查找替换

```
#查找var所表示的字符串中，第一次被pattern所匹配到的字符串，以substr替换之
${var/pattern/substr}
#查找var所表示的字符串中，所有能被pattern所匹配到的字符串，以substr替换之
${var//pattern/substr}
#查找var所表示的字符串中，行首被pattern所匹配到的字符串，以substr替换之
${var/#pattern/substr}
#查找var所表示的字符串中，行尾被pattern所匹配到的字符串，以substr替换之
${var/%pattern/substr}
```

#### 6.8.3 查找并删除

```
#删除var表示的字符串中第一次被pattern匹配到的字符串
${var/pattern}
删除var表示的字符串中所有被pattern匹配到的字符串
${var//pattern}
删除var表示的字符串中所有以pattern为行首匹配到的字符串
${var/#pattern}
删除var所表示的字符串中所有以pattern为行尾所匹配到的字符串
${var/%pattern}
```

#### 6.8.4 字符大小写转换

```
#把var中的所有小写字母转换为大写
${var^^}
#把var中的所有大写字母转换为小写
${var,,}
```

### 6.9 高级变量

#### 6.9.1 高级变量赋值

#### 6.9.2 高级变量用法-有类型变量

Shell变量一般是无类型的，但是bash Shell提供了declare和typeset两个命令用于指定变量的类型，两个命令是等价的

```
declare [选项] 变量名
选项：
-r 声明或显示只读变量
-i 将变量定义为整型数
-a 将变量定义为数组
-A 将变量定义为关联数组
-f 显示已定义的所有函数名及其内容
-F 仅显示已定义的所有函数名
-x 声明或显示环境变量和函数,相当于export 
-l 声明变量为小写字母 declare -l var=UPPER
-u 声明变量为大写字母 declare -u var=lower
-n  make NAME a reference to the variable named by its value
```

#### 6.9.3 变量间接引用

##### 6.9.3.1 eval命令

eval命令将会首先扫描命令行进行所有的置换，然后再执行该命令。该命令适用于那些一次扫描无法实现其功能的变量,该命令对变量进行两次扫描

##### 6.9.3.2 间接变量引用

如果第一个变量的值是第二个变量的名字，从第一个变量引用第二个变量的值就称为间接变量引用variable1的值是variable2，而variable2又是变量名，variable2的值为value，间接变量引用是指通过variable1获得变量value的行为

```
variable1=variable2
variable2=value
#示例: i=1
$1=wang
```

bash Shell提供了两种格式实现间接变量引用

```
#方法1 #变量赋值
eval tempvar=\$$variable1
#显示值
eval echo \$$variable1
eval echo '$'$variable1
#方法2 #变量赋值
tempvar=${!variable1}
#显示值
echo ${!variable1}
```

##### 6.9.3.3 变量引用 reference