---
title: 'Shell编程四剑客'
date: 2019-03-20 13:52:09
tags:
	- 操作系统
	- Shell
categories: 
	- 操作系统
	- Shell
---



## Shell编程四剑客之Find

通过如上基础语法的学习，读者对Shell编程有了更近一步的理解，Shell编程不再是简单命令的堆积，而是演变成了各种特殊的语句、各种语法、编程工具、各种命令的集合。

在Shell编程工具中，四剑客工具的使用更加的广泛，Shell编程四剑客包括：find、sed、grep、awk，熟练掌握四剑客会对Shell编程能力极大的提升。

四剑客之Find工具实战，Find工具主要用于操作系统文件、目录的查找，其语法参数格式为：

```bash
find path -option [ -print ] [ -exec -ok command ] { } \；
```

#### 其option常用参数详解如下：

```bash
-name filename #查找名为filename的文件；
-type b/d/c/p/l/f #查是块设备、目录、字符设备、管道、符号链接、普通文件；
-size n[c] #查长度为n块[或n字节]的文件；
-perm #按执行权限来查找；
-user username #按文件属主来查找；
```

#### Find工具-name参数案列：

```bash
find /data/ -name “*.txt” #查找/data/目录以.txt结尾的文件；
find /data/ -name “[A-Z]*” #查找/data/目录以大写字母开头的文件；
find /data/ -name “test*” #查找/data/目录以test开头的文件；
```

#### Find工具-type参数案列：

```bash
find /data/ -type d #查找/data/目录下的文件夹；
find /data/ ! -type d #查找/data/目录下的非文件夹；
find /data/ -type l #查找/data/目录下的链接文件。
find /data/ -type d|xargs chmod 755 -R #查目录类型并将权限设置为755；
find /data/ -type f|xargs chmod 644 -R #查文件类型并将权限设置为644；
```

#### Find工具-size参数案列：

```bash
find /data/ -size +1M #查文件大小大于1Mb的文件；
find /data/ -size 10M #查文件大小为10M的文件；
find /data/ -size -1M #查文件大小小于1Mb的文件；
```

## **Shell编程四剑客之SED**

**SED是一个非交互式文本编辑器**，它可对文本文件和标准输入进行编辑，标准输入可以来自键盘输入、文本重定向、字符串、变量，甚至来自于管道的文本，与VIM编辑器类似，它一次处理一行内容，Sed可以编辑一个或多个文件，简化对文件的反复操作、编写转换程序等。

在处理文本时把当前处理的行存储在临时缓冲区中，称为“模式空间”（pattern space），紧接着用SED命令处理缓冲区中的内容，处理完成后把缓冲区的内容输出至屏幕或者写入文件。

逐行处理直到文件末尾，然而如果打印在屏幕上，实质文件内容并没有改变，除非你使用重定向存储输出或者写入文件。其语法参数格式为：

```bash
sed [-Options] [‘Commands’] filename；

sed工具默认处理文本，文本内容输出屏幕已经修改，但是文件内容其实没有修改，需要加-i参数即对文件彻底修改；
x #x为指定行号；
x,y #指定从x到y的行号范围；
/pattern/ #查询包含模式的行；
/pattern/pattern/ #查询包含两个模式的行；
/pattern/,x #从与pattern的匹配行到x号行之间的行；
x,/pattern/ #从x号行到与pattern的匹配行之间的行；
x,y! #查询不包括x和y行号的行；
r #从另一个文件中读文件；
w #将文本写入到一个文件；
y #变换字符；
q #第一个模式匹配完成后退出；
l #显示与八进制ASCII码等价的控制字符；
```

常用SED工具企业演练案列：

替换jfedu.txt文本中old为new：

```bash
sed ‘s/old/new/g’ jfedu.txt
```

打印jfedu.txt文本第一行至第三行：

```bash
sed -n ‘1,3p’ jfedu.txt
```

打印jfedu.txt文本中第一行与最后一行：

```bash
sed -n ‘1p；$p’ jfedu.txt
```

删除jfedu.txt第一行至第三行、删除匹配行至最后一行：

```bash
sed ‘1,3d’ jfedu.txt
sed ‘/jfedu/,$d’ jfedu.txt
```

删除jfedu.txt最后6行及删除最后一行：

```bash
for i in `seq 1 6`；do sed -i ‘$d’ jfedu.txt ；done
sed ‘$d’ jfedu.txt
```

删除jfedu.txt最后一行：

```bash
sed ‘$d’ jfedu.txt
```

通常而言，SED将待处理的行读入模式空间，脚本中的命令逐行进行处理，直到脚本执行完毕，然后该行被输出，模式空间请空；然后重复刚才的动作，文件中的新的一行被读入，直到文件处理完备。

如果用户希望在某个条件下脚本中的某个命令被执行，或者希望模式空间得到保留以便下一次的处理，都有可能使得sed在处理文件的时候不按照正常的流程来进行。这时可以使用SED高级语法来满足用户需求。总的来说，SED高级命令可以分为三种功能：

**N、D、P：处理多行模式空间的问题；**

**H、h、G、g、x：将模式空间的内容放入存储空间以便接下来的编辑；**

**:、b、t：在脚本中实现分支与条件结构。**



在jfedu.txt每行后加入空行，也即每行占永两行空间，每一行后边插入一行空行、两行空行及前三行每行后插入空行：

```bash
sed ‘/^$/d；G’ jfedu.txt
sed ‘/^$/d；G；G’ jfedu.txt
sed ‘/^$/d；1,3G；’ jfedu.txt
```

将jfedu.txt偶数行删除及隔两行删除一行：

```bash
sed ‘n；d’ jfedu.txt
sed ‘n；n；d’ jfedu.txt
```

在jfedu.txt匹配行前一行、后一行插入空行以及同时在匹配前后插入空行：

```bash
sed ‘/jfedu/{x；p；x；}’ jfedu.txt
sed ‘/jfedu/G’ jfedu.txt
sed ‘/jfedu/{x；p；x；G；}’ jfedu.txt
```

在jfedu.txt每行后加入空行，也即每行占永两行空间，每一行后边插入空行：

```bash
sed ‘/^$/d；G’ jfedu.txt
```

在jfedu.txt每行后加入空行，也即每行占永两行空间，每一行后边插入空行：

```bash
sed ‘/^$/d；G’ jfedu.txt
```

在jfedu.txt每行前加入顺序数字序号、加上制表符\t及.符号：

```bash
sed = jfedu.txt| sed ‘N；s/\n/ /’
sed = jfedu.txt| sed ‘N；s/\n/\t/’
sed = jfedu.txt| sed ‘N；s/\n/\./’
```

删除jfedu.txt行前和行尾的任意空格：

```bash
sed ‘s/^[ \t]*//；s/[ \t]*$//’ jfedu.txt
```

打印jfedu.txt关键词old与new之间的内容：

```bash
sed -n ‘/old/,/new/’p jfedu.txt
```

打印及删除jfedu.txt最后两行：

```bash
sed ‘$!N；$!D’ jfedu.txt
sed ‘N；$!P；$!D；$d’ jfedu.txt
```

合并上下两行，也即两行合并：

```bash
sed ‘$!N；s/\n/ /’ jfedu.txt
sed ‘N；s/\n/ /’ jfedu.txt
```

## Shell编程四剑客之AWK

AWK是一个优良的文本处理工具，Linux及Unix环境中现有的功能最强大的数据处理引擎之一，以Aho、Weinberger、Kernighan三位发明者名字首字母命名为AWK，AWK是一个行级文本高效处理工具，AWK经过改进生成的新的版本有Nawk、Gawk，一般Linux默认为Gawk，Gawk是 AWK的GNU开源免费版本。

**AWK基本原理是逐行处理文件中的数据，查找与命令行中所给定内容相匹配的模式，**如果发现匹配内容，则进行下一个编程步骤，如果找不到匹配内容，则 继续处理下一行。其语法参数格式为，AWK常用参数、变量、函数详解如下：

```bash
awk ‘pattern + {action}’ file
```

1. **AWK基本语法参数详解：**

- **单引号’ ‘是为了和shell命令区分开；**
- **大括号{ }表示一个命令分组；**
- **pattern是一个过滤器，表示匹配pattern条件的行才进行Action处理；**
- **action是处理动作，常见动作为Print；**
- **使用#作为注释，pattern和action可以只有其一，但不能两者都没有。**

1. **AWK内置变量详解：**

- **FS 分隔符，默认是空格；**

- **OFS 输出分隔符；**

- **NR 当前行数，从1开始；**

- **NF 当前记录字段个数；**

- **$0 当前记录；**

- **$1~$n 当前记录第n个字段（列）。**

  

  #### 常用AWK工具企业演练案列：

  AWK打印硬盘设备名称，默认以空格为分割：

```bash
df -h|awk ‘{print $1}’
```

AWK以空格、冒号、\t、分号为分割：

```bash
awk -F ‘[ :\t；]’ ‘{print $1}’ jfedu.txt
```

AWK以冒号分割，打印第一列，同时将内容追加到/tmp/awk.log下：

```bash
awk -F: ‘{print $1 >>”/tmp/awk.log”}’ jfedu.txt
```

打印jfedu.txt文件中的第3行至第5行，NR表示打印行，$0表示文本所有域：

```bash
awk ‘NR==3,NR==5 {print}’ jfedu.txt
awk ‘NR==3,NR==5 {print $0}’ jfedu.txt
```

打印jfedu.txt文件中，长度大于80的行号：

```bash
awk ‘length($0)>80 {print NR}’ jfedu.txt
```

AWK引用Shell变量，使用-v或者双引号+单引号即可：

```bash
awk -v STR=hello ‘{print STR,$NF}’ jfedu.txt
STR=”hello”；echo| awk ‘{print “‘${STR}'”；}’
```

Awk统计服务器状态连接数：

```bash
netstat -an | awk ‘/tcp/ {s[$NF]++} END {for(a in s) {print a,s[a]}}’
netstat -an | awk ‘/tcp/ {print $NF}’ | sort | uniq -c
```

## Shell编程四剑客之GREP(文本搜索)

全面搜索正则表达式（Global search regular expression(RE) ，GREP）是一种强大的**文本搜索工具，它能使用正则表达式搜索文本，并把匹配的行打印出来。**

Unix/Linux的grep家族包括grep、egrep和fgrep，其中egrep和fgrep的命令跟grep有细微的区别，egrep是grep的扩展，支持更多的re元字符， fgrep是fixed grep或fast grep简写，它们把所有的字母都看作单词，正则表达式中的元字符表示其自身的字面意义，不再有其他特殊的含义，一般使用比较少。

目前Linux操作系统默认使用GNU版本的grep。它功能更强，可以通过-G、-E、-F命令行选项来使用egrep和fgrep的功能。其语法格式及常用参数详解如下：

```bash
grep -[acinv]   ‘word’   Filename
```

Grep常用参数详解如下：

```bash
-a 以文本文件方式搜索；
-c 计算找到的符合行的次数；
-i 忽略大小写；
-n 顺便输出行号；
```

学习Grep时，需要了解通配符、正则表达式两个概念，很多读者容易把彼此搞混淆，通配符主要用在Linux的Shell命令中，常用于文件或者文件名称的操作，而正则表达式用于文本内容中的字符串搜索和替换，常用在AWK、GREP、SED、VIM工具中对文本的操作。

通配符类型详解：

```bash
* 0个或者多个字符、数字；
? 匹配任意一个字符；
# 表示注解；
| 管道符号；
；多个命令连续执行；
```

正则表达式详解：

```bash
* 前一个字符匹配0次或多次；
. 匹配除了换行符以外任意一个字符；
.* 代表任意字符；
^ 匹配行首，即以某个字符开头；
$ 匹配行尾，即以某个字符结尾；
\(..\) 标记匹配字符；
[] 匹配中括号里的任意指定字符，但只匹配一个字符；
[^] 匹配除中括号以外的任意一个字符；
```

常用GREP工具企业演练案列：

```bash
grep -c “test” jfedu.txt 统计test字符总行数；
grep -i “TEST” jfedu.txt 不区分大小写查找TEST所有的行；
grep -n “test” jfedu.txt 打印test的行及行号；
grep -v “test” jfedu.txt 不打印test的行；
grep “test[53]” jfedu.txt 以字符test开头，接5或者3的行；
grep “^[^test]” jfedu.txt 显示输出行首不是test的行；
grep “[Mm]ay” jfedu.txt 匹配M或m开头的行；
grep “K…D” jfedu.txt 匹配K，三个任意字符，紧接D的行；
```

