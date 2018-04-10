# Linux 命令 #

## 1. lsattr和chattr命令 ##

注：为了允许添加数据，防止更改或者删除等，文件和文件夹可以设定了特定的控制属性。例如，你可以在关键的系统文件或者文件夹中启用属性，然后没有任何用户，包括root，可以删除或者修改它，比如不允许使用像dump这样的命令等备份工具去备份一个特定的文件或者文件夹，等等。这些属性只可以在ext2，ext3或者ext4文件系统中的文件和文件夹上设定。

### 1.1 lsattr命令 ###

lsattr命令用于显示文件属性。

语法：lsattr [-adlRvV][文件或目录...]

| 属性 | 描述 |
| --- | --- |
|a|显示所有文件和目录，包括以"."为名称开头字符的额外内建，现行目录"."与上层目录".."。|
|d|显示，目录名称，而非其内容。|
|l|此参数目前没有任何作用。|
|R|递归处理，将指定目录下的所有文件及子目录一并处理。|
|v|显示文件或目录版本。|
|V|显示版本信息。|

>注：lsattr 命令显示文件系统属性与ls 显示的UNIX 文件系统属性是两个不同的概念。lsattr实现的属性是文件系统的物理属性，而ls显示的文件属性是操作系统进行管理文件系统的逻辑属性。

### 1.2 chattr命令 ###

chattr命令改变文件或目录的属性。

语法：chattr [-RV] [-+=AacDdijsSu] [-v version] 文件或目录

| 属性 | 描述 |
| --- | --- |
|a|只允许在文件中进行追加操作|
|A|这个属性不允许更新文件的访问时间|
|c|启用这个属性时，文件在磁盘上会自动压缩|
|D|设置了文件夹的D属性时，更改会在同步保存在磁盘上|
|d|不能使用dump命令备份文件|
|e|它表明，该文件使用磁盘上的块的映射扩展|
|i|在文件上启用这个属性时，我们不能更改、重命名或者删除这个文件|
|j|设置了这个属性时，文件的数据首先保存在日志中，然后再写入文件|
|s|设置这个属性时，文件将会完全的移除这个硬盘空间；无法找回|
|S|设置了这个属性时，变更或更改同步保存到磁盘上|
|u|设置这个属性时，删除文件，会保存其内容。这允许用户要求其取消删除|
|R|递归地修改文件夹和子文件夹的属性|
|V|chattr命令会输出带有版本信息的冗余信息|
|f|忽略大部分错误信息|

在chattr中用于设置或者取消属性的 操作符

* '+' 符号用来为文件和文件夹设置属性，
* '-' 符号用来移除或者取消属性
* '=' 使它们成为文件有的唯一属性。

Linux手册：[chattr](https://linux.die.net/man/1/chattr)

## 2. expr命令 ##

expr命令是一个手工命令行计数器，expr一般用于整数值，也可用于字符串。

用法：expr 表达式

表达式说明:
* 用空格隔开每个项；
* 用 / (反斜杠) 放在 shell 特定的字符前面；
* 对包含空格和其他特殊字符的字符串要用引号括起来

可用的表达式有：
```
ARG1 | ARG2       若ARG1 的值不为0 或者为空，则返回ARG1，否则返回ARG2

ARG1 & ARG2       若两边的值都不为0 或为空，则返回ARG1，否则返回 0

ARG1 < ARG2       ARG1 小于ARG2
ARG1 <= ARG2      ARG1 小于或等于ARG2
ARG1 = ARG2       ARG1 等于ARG2
ARG1 != ARG2      ARG1 不等于ARG2
ARG1 >= ARG2      ARG1 大于或等于ARG2
ARG1 > ARG2       ARG1 大于ARG2

ARG1 + ARG2       计算 ARG1 与ARG2 相加之和
ARG1 - ARG2       计算 ARG1 与ARG2 相减之差

ARG1 * ARG2       计算 ARG1 与ARG2 相乘之积
ARG1 / ARG2       计算 ARG1 与ARG2 相除之商
ARG1 % ARG2       计算 ARG1 与ARG2 相除之余数
```

对字符串的表达式：
```
match 字符串 表达式		等于"字符串 :表达式"
substr 字符串 偏移量 长度	替换字符串的子串，偏移的数值从 1 起计
index 字符串 字符		在字符串中发现字符的地方建立下标，或者标0
length 字符串			字符串的长度
```
实例：
```
#!/bin/bash

str="ABCDEFGHIJKLMN"

#match匹配字符串，找到返回匹配到的字符串长度，找不到返回0。
#".*"表示匹配除换行符以外的任何字符
echo `expr match $str .*G`
#substr截取字符串
echo `expr substr $str 5 1`
#index查找字符在字符串中首次出现的位置
echo `expr index $str J`
#length计算字符串长度
echo `expr length $str`
```

## 3. cut命令 ##

cut命令是将每一行的选定部分打印到标准输出。

用法：cut [选项]... [文件]...

```
-b, --bytes=列表		只选中指定的这些字节
 -c, --characters=列表		只选中指定的这些字符
 -d, --delimiter=分界符	使用指定分界符代替制表符作为区域分界
 -f, --fields=列表		只选中指定的这些域；并打印所有不包含分界符的
       行，除非-s 选项被指定
 -n				(忽略)
     --complement		补全选中的字节、字符或域
 -s, --only-delimited		不打印没有包含分界符的行
     --output-delimiter=字符串	使用指定的字符串作为输出分界符，默认采用输入
       的分界符
     --help		显示此帮助信息并退出
     --version		显示版本信息并退出

----------------------------------------------------

N：只取第N项
N-：从第N项一直到行尾
N-M：从第N项到第M项（包括M项）
-M：从第一项到第M项（包括M项）
-：从第一项开始到结束的所有项
```

```
-b ：以字节为单位进行分割。
-c ：以字符为单位进行分割。
-f ：以字段为单位进行分割。
缺省以TAB为字段分隔符，可以用-d指定分隔符。
```

实例：

```
#!/bin/bash
echo $PATH
#选取第三个路径
echo `echo $PATH | cut -d ":" -f 3`
#选取第三个及以后路径
echo `echo $PATH | cut -d ":" -f 3-`
#选取第三个及之前路径
echo `echo $PATH | cut -d ":" -f -3`
#选取第三个和第五个路径
echo `echo $PATH | cut -d ":" -f 3,5`
#选取第三个到第五个路径
echo `echo $PATH | cut -d ":" -f 3-5`

ls -l
#注：不同编码汉字所占的字节数是不同的，（zh_CN.utf8）一个汉字占三个字节。
#提取每一行的第2个字节
echo `ls -l | cut -c 2`
#提取每一行的第4个字节及后面
echo `ls -l | cut -c 4-`
#提取每一行的第2个和第4个字节
echo `ls -l | cut -c 2,4`
#提取每一行的第4个及之前字节
echo `ls -l | cut -c -4`

```

## 4. awk命令 ##



## 5. sed命令 ##

实例：

1. sed批量替换多个文件的字符串

```
sed -i 's/old_string/new_string/g'  `grep old_string -rl ./`                           //一般的替换用这条足以实现

sed -i 's/old_string/new_string/g'  `grep old_string -rl ./ | grep -vE "tags|svn"`     //特殊要求的替换：此命令中要求过滤掉含有tags和svn的文件
```

2. 如何获取字符串的后几位

```
取后三位
echo "SYBASE4"|sed 's/.*\(...\)$/\1/'
取后四位
echo "SYBASE4"|sed 's/.*\(....\)$/\1/'
```

3. 删除空行（包括由空格组成的空行）

```
[[:space:]]表示空格或者tab的集合
sed 's/[[:space:]]//g' file
```

## 6. shift命令 ##



## 7. xargs命令 ##

xargs命令是给其他命令传递参数的一个过滤器，也是组合多个命令的一个工具。它擅长将标准输入数据转换成命令行参数，xargs能够处理管道或者stdin并将其转换成特定命令的命令参数。xargs也可以将单行或多行文本输入转换为其他格式，例如多行变单行，单行变多行。xargs的默认命令是echo，空格是默认定界符。这意味着通过管道传递给xargs的输入将会包含换行和空白，不过通过xargs的处理，换行和空白将被空格取代。xargs是构建单行命令的重要组件之一。

用法：xargs [选项]... [命令]...

|属性|描述|
|---|---|
| -0 --null |输入的文件名以null字符结尾，而不是空格，引号和反斜杠并不特殊处理(所有字符都以字面意思解释)。禁止文件尾字符串，当另一个参数处理。当参数含有空格、引号、反斜杠时很方便。GNUfind的-print0选项产生适合这种模式的输出。|
| -e[eof-str] --eof[=eof-str]|把文件尾字符串设置成eof-str。如果文件尾字符串出现在输入中的某行，余下的行将被忽略。如果没有eof-str，就没有文件尾字符串。如果没有这个选项,文件尾字符串默认是 "_"。|
| -i[replace-str] --replace[=replace-str]|把initial-arguments里的所有replace-str替换为从标准输入里读入的名称。同时，没有用引号括起来的空格不会结束参数。如果没有replace-str，它默认为"{}"(同`find -exec'一样)。此选项隐含有-x和-l 1选项。|
| -l[max-lines] --max-lines[=max-lines]|每个命令行最多可以有max-lines行非空格输入；max-lines默认是1。后面跟着的空格会使后面一行逻辑上是一个输入行的继续。此选项隐含有-x选项。|
| -n max-args --max-args=max-args|每个命令行最多可以有max-args个参数。如果大小超出了(见 -s 选项)那么参数个数将会用比max-args小；除非用了-x选项，那么xargs将退出。|
| -p --interactive|提示用户是否运行每个命令行，然后从终端读入一行。只有当此行以'y'或'Y'开头才会运行此命令行。此选项隐含有 -t 选项。|
| -r --no-run-if-empty|如果标准输入不包含任何非空格，将不运行命令。一般情况下，就算没有输入，命令也会运行一次。|
| -s max-chars --max-chars=max-chars|每个命令行最多可以有max-chars个字符，包括命令和初始参数，还包括参数后面结尾的null。默认是尽可能的大，有20k个字符。|
| -t --verbose|在执行之前在标准错误输出显示命令行。|
| --version|显示xargs的版本号，然后退出。|
| -x --exit|如果大小超出(见 -s 选项)就退出。|
| -P max-procs --max-procs=max-procs|同时最多运行max-procs个进程；默认是1。如果max-procs为0，xargs将同时运行尽可能多的进程。最好同时用-n选项；不然很可能只会做一次exec。|

xargs退出可以有如下状态:

|||
|---|---|
|0|成功|
|1|发生其它错误|
|123|任何一个被调用的命令command退出状态为1-125|
|124|命令command退出状态为255|
|125|命令command被信号终止|
|126|不能执行命令command|
|127|命令command没有找到|


## 8. sort命令 ##



## 9. wget命令 ##

## 10. wc命令 ##

## 11. find命令 ##

实例

1. find不递归查找子目录的方法

```
find .  ! -name "." -type d -prune -o -type f -name "*.jpg" -print

find . -name "*.jpg" -maxdepth 1 -print
```


## 12. iconv命令 ##

## 13. rpm命令 ##

## 14. rpm2cpio命令 ##

## 15. rpmbuild命令 ##

### 15.1 rpmbuild介绍 ###

顾名思义创建rpm包，它是用来指示转换的源码不定编译成二进制文件的包，在centos下默认目录为/usr/src/redhat

### 15.2 目录 ###

>/usr/src/redhat  
--BUILD #编译之前，如解压包后存放的路径  
--BUILDROOT #编译后存放的路径  
--RPMS #打包完成后rpm包存放的路径  
--SOURCES #源包所放置的路径  
--SPECS #spec文档放置的路径  
--SPRMS #源码rpm包放置的路径  
注：一般我们都把源码打包成tar.gz格式然后存放于SOURCES路径下，而在SPECS路径下编写spec文档，通过命令打包后，默认会把打包后的rpm包放在RPMS下，而源码包会被放置在SRPMS下

### 15.3 rpmbuild相关命令 ###

基本格式：rpmbuild [options] [spec文档|tarball包|源码包]

1.  从spec文档建立有以下选项：  
-bp  #只执行spec的%pre 段(解开源码包并打补丁，即只做准备)  
-bc  #执行spec的%pre和%build 段(准备并编译)  
-bi  #执行spec中%pre，%build与%install(准备，编译并安装)  
-bl  #检查spec中的%file段(查看文件是否齐全)  
-ba  #建立源码与二进制包(常用)  
-bb  #只建立二进制包(常用)  
-bs  #只建立源码包  

2.  从tarball包建立，与spec类似  
-tp #对应-bp  
-tc #对应-bc  
-ti #对应-bi  
-ta #对应-ba  
-tb #对应-bb  
-ts #对应-bs  

3.  从源码包建立  
--rebuild  #建立二进制包，通-bb  
--recompile  #同-bi  

4.  其他的一些选项  
--buildroot=DIRECTORY   #确定以root目录建立包  
--clean  #完成打包后清除BUILD下的文件目录  
--nobuild  #不进行%build的阶段  
--nodeps  #不检查建立包时的关联文件  
--nodirtokens  
--rmsource  #完成打包后清除SOURCES  
--rmspec #完成打包后清除SPEC  
--short-cricuit  
--target=CPU-VENDOR-OS #确定包的最终使用平台  

## 16. expect命令 ##

描述：

我们通过Shell可以实现简单的控制流功能，如：循环、判断等。但是对于需要交互的场合则必须通过人工来干预，有时候我们可能会需要实现和交互程序如telnet服务器等进行交互的功能。而expect就使用来实现这种功能的工具。

expect是一个免费的编程工具语言，用来实现自动和交互式任务进行通信，而无需人的干预。expect是不断发展的，随着时间的流逝，其功能越来越强大，已经成为系统管理员的的一个强大助手。expect需要Tcl编程语言的支持，要在系统上运行expect必须首先安装Tcl。

用法：

expect [ -dDinN ] [ -c cmds ] [ [ -[f|b] ] cmdfile ] [ args ]

选项：

| 属性 | 描述 |
| :---: | :---: |
| -c | 执行脚本前先执行的命令，可多次使用。|
| -d | debug模式，可以在运行时输出一些诊断信息，与在脚本开始处使用exp_internal 1相似。|
| -D | 启用交换调式器,可设一整数参数。|
| -f | 从文件读取命令，仅用于使用#!时。如果文件名为"-"，则从stdin读取(使用"./-"从文件名为-的文件读取)。|
| -i | 交互式输入命令，使用"exit"或"EOF"退出输入状态。|
| -- | 标示选项结束(如果你需要传递与expect选项相似的参数给脚本时)，可放到#!行:#!/usr/bin/expect --。|
| -v | 显示expect版本信息。|

使用：

```
#!/usr/bin/expect

set timeout 20
spawn ssh root@10.20.24.103
expect "root"
send "paic1234\n"
interact
```

>set timeout 20 这个是用来设置相应的时间，如果里面的脚本执行或者网络问题超过了这个时间将不执行，默认这个timeout模式是10   
spawn 表示在expect下面需要执行的shell脚本   
expect 是捕获执行shell以后系统返回的提示框内容。”“这个表示提示框里面是否包括这个内容   
send 如果expect监测到内容了，那么就将send后的内容发送出去 \n表示回车   
interact 表示结束expect回话，可以继续输入，但是不会返回终端   
send_user用来发送内容给用户  
exp_continue 继续执行下面的匹配(类似continue)   
expect eof 捕捉eof结束  
expect {}，多行期望，匹配到哪条执行哪条，有时执行shell后预期结果是不固定的

## 17. sshpass命令 ##

描述：

sshpass是设计用于运行程序的ssh使用被称为“键盘交互”密码验证方式，但在非交互模式。

SSH使用直接TTY访问，以确保密码确实是一个交互式键盘用户发出。Sshpass运行在一个专用的TTY SSH，欺骗它，以为它是从一个交互式用户获取密码。

后sshpass自己的选项指定要运行的命令。通常，这将是“SSH”带参数，但它可以一样好是任何其他命令。通过SSH使用的密码提示，然而，目前硬编码到sshpass

用法：

sshpass [ -f 文件名 | -d NUM | -p 密码 | -e] [ 选项 ] 命令参数

选项：

<table border="2">
  <tr>
    <td>属性</td>
    <td>参数</td>
    <td>描述</td>
  </tr>
  <tr>
    <td>-p</td>
    <td>密码</td>
    <td>密码是在命令行中给出。请注意，标题为“一节安全考虑 ”。</td>
  </tr>
  <tr>
    <td>-f</td>
    <td>文件名</td>
    <td>密码是该文件的第一行的文件名。</td>
  </tr>
  <tr>
    <td>-d</td>
    <td>号</td>
    <td>数字是sshpass从亚军继承的文件描述符。密码从打开的文件描述符读取。</td>
  </tr>
  <tr>
    <td>-e</td>
    <th colspan="2">数字是sshpass从亚军继承的文件描述符。密码从打开的文件描述符读取。</th>
  </tr>
</table>
