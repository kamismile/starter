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

# 数值运算及处理

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

# 字符串处理

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

# 条件测试

​	











