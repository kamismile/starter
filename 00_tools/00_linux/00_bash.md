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







































































