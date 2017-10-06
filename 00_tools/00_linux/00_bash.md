# 01 清屏

clear 或ctrl + l

# 02 ln

硬连接有同步功能，占磁盘空间

常用用软连接

# 03 重定向2>, && ||, ;

```shell
mkdir /mulu/a 2> /dev/null && echo "success"
mkdir /mulu/a 2> /dev/null || echo "fail"
cd /boot/grub; ls -lh grub.conf
```

# 04 变量

## 定义及赋值

​	格式: 变量名=变量值

## 引用变量

​	格式: \$变量名 ${变量值}

## 双引号

​	允许引用 / 转义

## 单引号

​	禁止引用， 转义

## 反撇号，或者$()

​	以命令输出进行替换

```shell
ver=`uname -r`

```



## 环境变量

​	系统赋值: USER, LOGNAME, HOME, SHELL

​	用户操作: PATH, LANG, CLASSPATH

```shell
env # show environments
echo $USER $HOME $SHELL
echo $LANG $USER $SHELL $HOME

```

 	其它变量

​	由系统或脚本操控，不可直接赋值

​	$?: 前一条命令的状态值，0为正常，非0异常

​	$0: 脚本自身的程序名称 (路径)

​	\$1-$9: 第1至第9个参数

​	$*: 命令行的所有位置参数的内容

​	\$#: 命令行的位置参数个数

```shell
cat test.sh
#!/bin/bash
echo "本程序名: $0"
echo "执行时一共输入$#个参数"
echo "其中第一个参数是: $1"
echo "所有的参数如下: $*"

```

# 05 数值运算及处理

​	整数运算操作

​		使用expr命令， 计算表达式

​			格式: expr 数值1 操作符 数据2

​		使用$[]表达式，算式替换

​			格式: $[数值1 操作符 数值2]

​	几个数据处理技巧

​		变量的递更处理

​			格式 let 变量名++ let 变量名--

​		使用随机数

​			Random 变量

​		生成数字序列

​			格式:  seq 末数，seq 首数 末数， seq 首数 增量 末数

​	小数运算操作

​		将表达式给bc命令处理

```shell
expr 45 + 21
expr 45 - 21
expr 45 \* 21
expr 45 / 21
expr 45 % 21
x=45; y=21; expr $x - $y
echo $[45 + 21]
echo $[45*21]
x=45;y=21;echo $[x-y] # 等同$[$x - $y]

x=45; y=21
let x++; echo $x
let y--; echo $y
let x+=2; echo $x

echo $RANDOM
echo $[RANDOM % 100]
expr $RANDOM % 100

seq 3 # 1 2 3
seq 3 5 # 3 4 5
seq 3 2 10 # 3 5 7 9

echo "45.67-21.05" | bc # 24.62
echo "scale=4;10/3" | bc # 3.3333

```

# 06 字符串处理

​	子串截取操作

​		路径分割

​			dirname命令， basename命令

​		使用expr命令

​			格式: expr substr $var1 起始位置 截取位置 # 起始位置从1开始

​		使用${}表达式

​			格式: \${var1:起始位置:截取长度} # 起始位置从0开始

​	字符串替换

​		使用${}表达式

​			格式1: ${var1/oldnew}   # 仅替换第一个

​			格式2: ${var1//old/new} # 替换所有

​	使用随机字符串

​		/dev/urandom -> /usr/bin/md5sum -> /bin/cut 

​		随机字符 -> ascii字符

​			head -1 /dev/urandom | md5sum

​		使用cut切割字符串

​			echo $var1 | cut -b 起始位置-结束位置 # 位置为首尾时可省略



```shell
var1="/etc/httpd/conf/httpd.conf"
dirname $var1 # /etc/httpd/conf
basename $var1 # httpd.conf
var1=BeidaQingNiao
expr substr $var1 4 6 # daQing
echo ${var1:4:6} # aQing
echo ${var1::5} # Beida

head -1 /dev/urandom
head -1 /dev/urandom | md5sum

head -1 /dev/urandom | md5sum | cut -b -8 # 随机截取8字符
cat /dev/urandom | tr -cd 'a-f0-9' | head -c 32
uuidgen

```

# 07 条件测试

​	测试操作规范

​		测试的本质

​			是一条操作命令

​			根据$?返回值来判断条件是否成立

​		操作规范

​			格式1:  test 条件表达式

​			格式2: [ 条件表达式 ]

​		测试操作的练习方法

​			直接跟 && echo YES 判断结果

​			用法: [ 条件表达式 ] && echo YES

​	文件状态的检测

​		存在及识别

​			-e: 目录是否存在(exist)

​			-d: 是否为目录(directory) # 判断是否存在且是否是目录

​			-f: 是否文件(file)

​		权限的检测

​			-r: 是否有读取权限 (read)

​			-w: 是否有写入权限 (write)  # 文件所有者即使没有显示有写权限 实际也是有的

​			-x: 是否有可执行权限 (execute)

​		整数值比较

​			-eq: 等于(equal)

​			-ne: 不等于(not equal)

​			-gt: 大于(greater than)

​			-lt: 小于(less than)

​			-ge: 大于或等于（greater or equal)

​			-le: 小于或等于(less or equal)

​	整数值比较，字串比较

​		字符串比较:

​			=： 两个字串相同

​			!=: 两个字串不相同

```shell
ls -dl /etc/grub /boot/grub
[ -d "/etc/grub" ] && echo yes
[ -d "/etc/grub" ] || echo no

[ -d "/etc" ] && echo yes
[ -f "/etc/fstab" ] && echo yes 

who | wc -l
[ $(who | wc -l) -eq 2 ] && echo yes
[ $(who | wc -l) -gt 5 ] && echo yes


echo $USER
[ $USER = "root" ] && echo yes
[ $USER != "nobody" ] && echo yes


```

# 08 使用if判断结构

## 程序流控制

​	执行过程的顺序化/自动化

​		智能化的选择及处理

​	让重复操作更聪明一些

​	对于。。。多个对象。。。如何逐个处理 for

​	在。。。取值是。。。的情况下怎么办？ case

​	如果。。。条件是。。。时程序需要做什么？ if



## 单分支/双分支的if应用

if 条件测试

​	then 命令序列

fi

​	检查备份目录 /opt/mrepo, 若不存在则创建



双分支

if 条件测试

​	then 命令序列1

​	else 命令测试2

fi

```shell
cat chkdir.sh
BACKUP_DIR="/opt/mrepo"
if [ ! -d $BACKUP_DIR ]
then
	mkdir -p $BACKUP_DIR
fi

```

判断目录主机是否存活，显示 测试结果

```shell
cat chkhost.sh
#!/bin/bash
ping -c 3 -i 0.2 -W 3 $1 &> /dev/null
if [ $? -eq 0 ]
then
	echo "host $1 is up."
else 
	echo "host $1 is down."
fi
```



## 多重分支的if应用

if 条件测试1

​	then 命令序列1

elif 条件测试2

​	then 命令序列2

。。。

else 

​	命令序列n

fi

判断机试分数，区分优秀，合格，不及格

```shell
cat gradediv.sh
#!/bin/bash
read -p "请输入你的分数 (0-100):" GRADE
if [ $GRADE -ge 85 ] && [ $GRADE -le 100 ] ; then
	echo "$GRADLE 分！优秀"
	
```



# 09 使得for循环

## for语名结构

​	语法格式

​	for 变量名 in 取值列表

​	do

​		命令操作

​	done

```shell
#!/bin/bash
for i in "1st" "2nd" "3rd"
do
	echo $i
done


#!/bin/bash
for i in $(cat /etc/host.conf)
do
	echo $i
done
```

目标: 批量加用户账号

​	用户列表 文件 users.txt, 每行一个

​	初始口令设为123456，首次登陆后必须更改

```shell
cat uad.sh
#!/bin/bash
for i in $(cat /root/users.txt)
do
	useradd $i
	echo "123456" | passwd --stdin $i
	chage -d 0 $i
done
```

目标： 检测一个IP范围内的主机状态

​	192.168.4.1 - 192.168.4.10

​	根据是否ping通来判断

```shell
#!/bin/bash
IP_PRE="192.168.4."
for IP in $(seq 1 5)
do
	ping -c 3 -i 0.2 -W 3 ${IP_PRE}$IP &> /dev/null
	if [ $? -eq 0 ]; then
		echo "${IP_PRE}$IP is up."
	else 
		echo "${IP_PRE}$IP is down."
	fi
done

```

# 10 使用case分支

## case语句结构

​	case 变量值 in

​	格式1)

​		命令序列1

​		;;

​	格式2)

​		命令序列2

​		;;

​	。。。

​	*)

​		默认命令序列

​	esac



​	语法格式

​	case 控制参数 in

​	start)

​		启动XX服务

​		;;

​	stop)

​		停止XX服务

​		;;

​	...

​	*) 显示服务脚本的用法

​	esac

## 基本用法示范

```shell
cat hitkey.sh
#!/bin/bash
read -p "请输入一个字符，并按enter键确认: " KEY
case "$KEY" in
	[a-z]|[A-Z])
		echo "你输入的是字母"
		;;
	[0-9])
		echo "你输入的是数字"
		;;
	*)
		echo "你输入的是空格/功能键或其它控制字符"
esac
```



## 案例实战

目标： 编写服务脚本 sleepd

​	能够响应start, stop控制参数

​	将服务交给chkconfig进行管理

```shell
cat /etc/init.d/sleepd
#!/bin/bash
case "$1" in 
start)
	sleep 3600 &
	;;
stop)
	pkill -x "sleep"
	;;
*)
	echo "用法: $0 [start|stop]"
esac
```

chkconfig处理

​	在脚本开头设计chkconfig参数

​	添加为系统服务

```shell
cat /etc/init.d/sleepd
#!/bin/bash
# chkconfig: - 90 10
# description: Daemon script for sleepd server
...
chkconfig --add sleep # 开机之后自动运行

```

# 11 awk文本处理工具

## 关于文本处理

​	shell输出为文本

​		面向过程，而非面向对象

​	非交互处理方式

​		重定向/管道/命令替换

​		head, more, cut, tr

​		grep, awk, sed

​	awk编程语言/数据处理引擎

​		创造者: aho, weiberger, kernighan

​		基于模式匹配检查输入

​		将期望的匹配结果print到屏幕

## awk基本命令格式

语法格式

​	awk '模式 {操作}' 文件1 文件2  ....

```shell
awk 'NR==1{print}' /etc/hosts
```

常见的内建变量

​	NR: 当前处理行的序数(行号)

​	FS: 字段分割，缺省为空格或Tab位

​	$n: 当前行的第n个字段

​	$0: 当前行的所有文本内容



```shell
cat file.txt
1 this is the first line
2 hello, everybody!
3 192.168.4.2 w2k8.benet.com
4 hunter:x:504:504:/home/hunter:bin/bash

awk 'NR==1,NR==3{print}' file.txt # 从第一行到第3行

awk '(NR==1) || (NR==3) {print}' file.txt # 输出第一行和第三行
```

使用比较运算

```shell
awk '(NR%2)==1{print}' file.txt # 输出所有奇数行

awk '(NR%2)==0{print}' file.txt # 输出所有偶数行
```

使用正则表达式 ,// 两斜杠包裹起来的

```shell
awk '/2/{print}' file.txt # 输出包含数字2的行

awk '/bash$/{print}' file.txt # 输出以bash结尾的行
```



## 常见用法示例

指定分隔/指定输出字段

```shell
awk 'NR==2,NR==3{print $1,$3}' file.txt

awk -F. '$5=="benet"{print $0}' file.txt # -F 指定分隔符， 以.分隔且第5字段为benet
```

# sed文本处理工具

过滤及修改

## sed基本用法

## 输出文本

## 删除及替换文本





































































