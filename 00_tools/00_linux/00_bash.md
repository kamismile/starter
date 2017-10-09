[Java架构师，年薪50万](http://www.guaishouxueyuan.net/thread-35704-1-1.html)

1。 算法

2。 架构 

3。 python

4。 CPP

# 01 清屏

clear 或ctrl + l

# 02 ln

硬连接有同步功能，占磁盘空间

常用用软连接

# 03 重定向2>, &>, 2>&1, && ||, ; 

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

# 12 sed文本处理工具

过滤及修改

## sed基本用法

sed流式编辑器/文本过滤

- stream editor
- 基于模式匹配过滤/修改文本

语法格式

- sed '编辑指令' 文件1 文件2 。。。
- sed -n ‘编辑指令' 文件1 文件2 。。。 # 仅输出匹配内容
- sed -i ’编辑指令’ 文件1 文件2。。。 # 编辑源文件

```shell
sed -n '3,5p' /etc/hosts  # 打印第3到5行
```

编辑指令的写法

- 格式: [地址1[,地址2]]操作类型
- 多条指令之间以分号隔开

```shell
sed -n '3p;5p' /etc/hosts
```

最常用操作类型

- p 输出，打印文本行
- n 取下一行文本 (跳过当前行)
- d 删除
- s 字符串替换
- a 追加新的文本

## 输出文本

```shell
cat file.txt
1 this is the first line.
2 hello, everybody!
3 192.168.4.2 w2k8.benet.com
4 hunter:x:504:504::/home/hunter:/bin/bash
```

```shell
sed -n 'p;n' file.txt # 输出奇数行
awk 'NR%2==1{print}' file.txt

sed -n 'n;p' file.txt # 输出偶数行
awk 'NR%2==0 {print}' file.txt
```

使用正则表达式

```shell
sed -n '/w2k8/,$p' file.txt # 输出从第1个包含w2k8的行到最后一行

sed -n '/\<This\>/p' file.txt # 输出包含单词This的行
```





## 删除及替换文本



删除符合条件的行

```shell
sed '2,3d' file.txt # 删除第2，3行文本

sed '/w2k8/d;$d' file.txt # 删除包含w2k8的行和最后一行
```

删除不符合条件的行

```shell
sed '/ne/!d' file.txt # 删除不包含字符串ne的行
```

替换符合条件的文本

```shell
sed '3,4s/hunter/BADBOY/g' file.txt # 将第3-4行的hunter都替换为BADBOY
```

替换的特殊效果

```shell
sed '1,2s/^/#/g' file.txt # 在第1-2行的行首插入一个# 可用于注释配置文件

sed 's/ter//g' file.txt # 删除字符串ter
```











# tmp

## 定时

```shell
date
at 20:43
> echo `date` > /tmp/foo.txt
> ^ + d


at 20:00 2015-2-23
at> echo aaa /tmp/a.txt
> ctrl + backspace

at now + 10min
at>
...

ll /var/spool/

# 查询
at -l
atq

# 删除
atrm
at -d
at -r

# at是一次性的

# 周期性任务
cat /etc/crontab

# 查看周期服务是否运行
/etc/init.d/crond status
/etc/init.d/cron status

# 查看是否开机启动
chkconfig --list crond

# 用户root级别计划任务
crontab -e # 创建一个计划任务
crontab -l # 显示
crontab -r # 删除

# 分 时 日 月 周 年
# java 秒 分 时 日 月 周 年

# 9，18,22这几天的3点1分开始执行备份脚本
# 1 3 9,18,22 * * /usr/bin/back.sh

# 每月9-18日，这几天，3:00执行
1 3 9-18 * * /usr/bin/back.sh

# 每5分钟执行一次
*/5 * * * * /usr/bin/back.sh

# 每天删除5天前的文件
1 1 * * *  find /home/log/ -type f -mtime +5 exec rm {} \;

# 使用root身份，给其它普通用户指定crontab
# crontab -u USERNAME -l/-e/-r # 清空操作会清空所有

anacron # 在机器重启后，检查已有cron任务是否有执行成功，没有则执行

vim /var/log/cron # 查看定时任务运行日志


```

## 实战定时开机

1. 进入bios, 进入电源管理power management setup
2. 唤醒事件设置 wakeup event setup
3. resume by rtc alarm -> date everyday

## 日志管理

```shell
# /var/log
# 核心启动日志: /var/log/dmesg
# 系统报错或重启服务等日志 /var/log/messages
# 邮件系统日志 /var/log/maillog
# cron定时任务日志 /var/log/cron
# /var/log/secure 验证系统用户登陆
# /var/log/wtmp 记录所有的登入登出 last
last # 查看所有登陆过系统的用户和ip
> /var/log/wtmp # 清空痕迹
# 文件/var/log/lastlog 记录每个用户最后的登入信息
lastlog
# /var/log/btmp 
lastb # 记录错误的登入尝试
```





## find

find [目录]\[条件][动作]

不输入目录 代表当前目录

```
find # 不指定目录 ll
find /boot

```

条件

​	用户和组: -user -group -nouser -nogroup

​	类型: -type (f: 文件, d：目录， l 连接，p: 管道，s: socket)

​	名称: -name 

​	大小: -size +NM 大于N兆 -NM 小于N兆 NM 等于N兆 NG

​	权限: -perm 755  0755 777 1777

​		-777 至少有777

​	深度 -maxdepth num

​	时间: -mtime 修改时间 -atime 访问时间 -ctime 改变时间

​			改变和修改的区别在于是改文件的属性还是内容

​			chmod a-w myfile 改变的是ctime

​			echo foo >> myfile 修改 mtime

​		ctime: 改变的是文件的索引节点发生了改变

​		mtime: 修改是文件本身内容发生了改变

​	查找 /home下拥有者为mk的文件

ls(l)命令可用来列出文件的atime, ctime和mtime

ls -lc filename  列出文件的ctime

ls -lu filename 列出文件的atime

ls -l filename 列出文件的mtime

```shell
find /home -user mk
id # 查看id

# 修改时间
date -s "2017-10-07"
find / -mtime +2 # 超过两天

find . -maxdepth 1 # 仅查找第1层

# 当前目录下其它人可写
find -type f -and -perm -002
find -type f -and -perm /o+w

# 其它人可执行的非普通文件
find ! -type f -and -perm -001
```

[动作]

-ls

-ok

-exec

-print

-printf



```shell
# 找到文件并删除
find /tmp -type f -exec -rm {} \;
# -exec 执行命令
# rm 要执行的命令
# {} 表示find -type f 查找出来的文件内容
# \; {}和\;之间要有空格。固定语法，就是以这个结尾
```

## 进程管理

进程和线程

> 一个程序至少有一个进程，一个进程至少有一个线程
>
> 进程之间内存是独立的
>
> 线程之间内存共享，高并发好一些。安全性差一些。一个线程挂了，共享内存会受影响

```shell
pstree
pstree -p # 显示pid
tree /foo # 显示树型结构

```

列出目前所有的正在内存当中的进程

- user: 运行此进程的用户 process属于哪个使用者账号
- PID: 该process的号码 
- %CPU: 该process使用掉的cpu资源百分比
- %MEM 该process所占用的物理内存百分比
- VSZ: 该process使用掉的虚拟内存量(Kbytes)
- RSS: 该process占用的固定的内存量(Kbytes)
- TTY：显示是在哪个终端机上运行，若与终端无关，显示?,另外tty1-tty6是本机上面的登入者程序，若为pts/0等等的，则表示为由网络连接进主机的程序
- STAT：该程序目前的状态， linux进程有5种基本状态
  - R: 正在运行或在运行队列中等待
  - S: 该程序目前正在睡眠中，但可被某些记号(signal)唤醒
  - T: 该程序目前暂停了
  - Z: 该程序应该已经终止，但是其父程序却无法正常的终止他，造成zombie(僵尸)程序的状态
  - D: 不可中断状态
  - < 高优先级
  - N 低优先级
  - L 有内存分页但是带锁
  - l 包含子进程
  - \+ 前台程序 ()
- START： 该process实际使用cpu运作的时间
- COMMAND: 该程序的实际指令

```shell
ps
ps -aux | more # a: all x: execute u: user
vim foo.sh # ctrl + z
ps -aux | grep vim # 查看到是stat是T状态
# ctrl + c是发送SIGINT信号，终止一个进程
# ctrl + z是发送SIGSTOP信号，挂起一个起程

```

ps aux 是用BSD的格式来显示进程

ps -ef  是用标准的格式来显示进程

## top 动态查看进程

[sysstat 黄金60秒](http://www.jianshu.com/p/fd6e35f529c1)

[Brendan linux performance](http://www.brendangregg.com/linuxperf.html)

```shell
uptime
dmesg | tail
vmstat 1
mpstat -P ALL 1
pidstat 1
iostat -xz 1
free -m
sar -n DEV 1
sar -n TCP,ETCP 1
top # ctrl - s 暂停 ctrl - q 继续 



```

ps静态，top动态

统计信息区前五行是系统整体的统计信息

13:22:30 up 8min, 4 users, load average: 0.14, 0.38, 0.25

第一行: 等同于uptime, 任务队列信息

- 当前时间  
- 系统运行时间，格式为时: 分
- 当前登陆用户数
- 系统负载，即任务队列的平均长度。三个数值分别为1分钟，5分钟，15分钟前到现在的平均值

一般来说，每个cpu内核当前活动进程数不大于3，则系统运行表现良好。这里说的是每个cpu内核，也就是如果你的主机是四核cpu的话，那么只要uptime最后输出的一串数值小于4 * 3 = 12即表示系统负载不是很严重

第二行: 进程信息

​	总进程数 正在运行的进程数 睡眠的进程数 停止的进程数 僵死的进程数

第三行:  cpu信息

​	cpu(s): 系统用户进程使用cpu百分比。不包括调高优先级的进程。cpu%是由每个核的cpu占用率之和算出来的。如果你是4核cpu,核1，cpu使用率为100%，核2，cpu使用百分率为100%，则会出现cpu高于100%，最终为200%, top中按下1可查看各cpu使用情况

​	sy: 	内核占用cpu百分比

​	ni: 	用户进程空间内改变过优先级的进程占用cpu百分比

​	id: 	空间cpu百分比

​	wa: 

第四五行信息： 内存

​	mem total: 	物理内存总量

​	used:		使用的物理内存总量

​	free:		空闲内存总量

​	buffers:		用作内核缓存的物理总量，和free -k 一个意思

​	swap: total  	交换区总量

​	OK used 	使用的交换区总量

​	free			空闲交换区总量

​	cached		缓冲的交换区总量。内存中的内容被换出到交换区，而后又被换入到内存，但使用过的交换区尚未被覆盖，该数值即为这些内容已存在于内存中的交换区的大小。相应的内存再次被换出时可不必再对交换区写入。 和free -k 中cache一样

### 进程信息

- pid
- user
- ni 进程优化级，nice值，负值表示高优先级，正值表示低优先级
- RES 实际 使用内存大小
- S 进程状态 D R S T Z
- %cpu  上次更新到现在的cpu时间占用百分比
- %mem 进程使用的物理内存百分比
- TIME+ 进程使用的CPU时间总计，单位1/100秒
- command 命令名/命令行

### top快捷键

​	默认3秒刷新一次

​	空格立即刷新

- q 退出 
- M 按内存排序
- P 按CPU排序
- <>翻页

### 控制关闭进程

kill 向进程发送信号（停止进程）

​	kill -9 pid

​	killall  name通过程序名称直接杀掉所有进程

​	pkill name

- 1 HUP 重新加载配置文件 类似重启
- 2 INT 和ctrl + c一样 一般用于通知进程组终止进程
- 9 KILL 强行中断
- 19 STOP 和ctrl + z一样

### 优先级控制 

nice值  -20 ~ 19 越小优先级越高，普通用户 0-19

```shell
vim foo.txt
ps aux | grep vim 
top -p 7869
nice -n -5 vim foo.txt
# 修改正在运行的进程的优先级
renice -n 5 pid
```



## nice-free-前后台进程切换-free-screen命令

nice -n num command

renice -n <pid>

前后台进程切换

 - jobs 列出所有后台进程
 - fg 后台程序改成前台  
    - fg 后台进程序列号

```shell
jobs
vim foo.txt &
jobs

```

free 

 - buffer 缓存从磁盘读出的内容
 - cached 缓存需要写入磁盘的内容

```shell
find / -type f # buffer会增加，cached不会
dd if=/dev/zero of=a.txt bs=10M count=10 # 写数据 10M 10次 cached会增长
file a.txt

```

linux为了提高性能，会使用buffer cache

实际使用 = used - buffers - cached 

可用 = free + buffers + cached



​			total 	used 	free 	shared		buffer 		cached

mem:		1164636	 A		B		0			C			D

-/+ buffers/cache		X		Y

Swap:		1023992	0	1023992

X = A - C - D

Y = B + C + D

```shell
vim foo.txt & # 在窗口关闭后会退出进程

```

### screen 实战后台实时执行命令备份命令

nohup, 在shell中提示了nohup成功后还需要按终端上键盘任意键退回到shell输入命令窗口，然后通过在shell中输入exit退出终端，这样启动的进程在关闭终端后依然可以运行。但如果在nohup执行成功后直接点关闭程序按钮关闭终端，会断调该命令所对应的session，导致nohup对应的进程被通知需要一起shutdown

nohup 

讲法: nohup command \[args... ][ &]

描述: nohup命令运行由command参数和任何相关的arg参数指定的命令，忽略所有挂断(SIGHUP)信号。在注销后用nohup命令运行后台中的程序。要运行后台中的nohup命令，添加& 到命令的尾部

如果使用nohup命令提交作业，那么在缺省情况下该作业所有输出都被重定向到一个名为nohup.out的文件中，除非另外指定了输出文件

disown -h %num

shopt huponexit

shopt -s huponexit

nohup command > myout.file 2>&1 &

```shell
screen
# ctrl + a + d detatch
screen -ls 
screen -r <pid|name>

```

## 输入输出重定向 文件查找 

文件描述符 0 1 2 

/dev/null 类似黑洞， 用于抛弃无用信息

\> 输出重定向 等同于1>

1> 标准输出重定向

2> 标准错误输出重定向

& 等同于

2>&1

```shell
ls /home /homee > /dev/null 2>&1
```

输入重定向

<, <<EOF

```shell
wc /etc/passwd
wc < /etc/passwd

cat > a.txt <<EOF
...
EOF
cat > a.txt <<FF

FF
```

管道

tee: 将标准输出的内容同时写到一个文件中

```shell
cat /etc/passwd | tee a.txt # 保存到文件且输出到控制台
cat /etc/passwd > a.txt # 保存到文件
```

## 文件查找 

which: 	查看可执行文件的位置

​	which ls

​	which find

whereis	查看可执行文件的位置及相关文件

​	whereis ls

locate 	配合数据库缓存快速查看文件位置

​		updatedb # 使用前使用此命令更新数据库，否则最新创建的文件找不到。另会在晚上2:00左右自动更新数据库。在计划任务中有

​		locate 

find		实际搜寻硬盘查询文件名称

grep	过滤

​	grep --color root /etc/passwd

## 文件系统

### 硬盘结构

CHS结构体系的磁盘 (Cylinder/header/sector)

> 硬盘盘片的每一条磁道有相同的扇区数，由此产生了所谓的3D参数(disk geometry), 磁头数(heads) 柱面数(cylinders) 扇区数(sectors)以及对应的3D寻址方式
>
> 缺点: 外圈浪费，但速度很平
>
> 每个磁道的扇区数都一样，这样外磁道整个弧长要大于内部的扇区弧长，因而其磁记录密度就要比内部磁道的密度要小。最终，导致了外部磁道的空间浪费
>
> 簇类似于linux系统中的block

例: 文本文件“新建文本文档.txt"中有只有aa两个字符， 大小2字节，占用空间4KB



ZBR (Zoned Bit Recording) 区位记录(zoned) 

> zoned-bit recording(ZBR 区位记录) 是一种物理优化磁盘存储空间的方法，此方法通过将更多的扇区放到磁盘的外部磁道而获取更多的存储空间
>
> 外磁道扇区多，速度外，向内逐渐变慢
>
> 磁盘大文件读写时速度会逐渐变慢就是这个原因 

操作系统读取硬盘的时候，不会一个个扇区地读取，这样效率太低，而是一次性连续读取多个扇区，即一次性读取一个”块“(block)。这种由多个扇区组成的”块“，是文件存取的最小单位。”块“的大小，最常见的是1KB, 即连2个sector组成一个block。

### 查看系统块大小

```shell
tune2fs -l /dev/sda1 | grep --color size
```

不同的分区可以有不同的块大小, 并非block越大越好

```shell
df -h # 查看mounted on 根分区/
ll -d # 仅显示目录
```

### 文件系统结构 

三部分: 文件名, inode, block(真正存储数据在block)

inode: 文件数据都存储在"块”中，那么很显然 ，我们还必须找到一个地方存储文件的元信息，比如文件的创建者，文件的创建日期，文件的大小等。这种存储文件元信息的区域就叫做inode，中文译名为"索引节点"

inode的内容:

 - 文件的字节数
 - 拥有者的uid
 - group id
 - 文件的读，写，执行权限 
 - 文件的时间戳，共有三个，ctime指上一次变动时间，mtime指文件内容上一次变动时间，atime指文件上一次打开时间。
 - 链接数，即有多少文件名指向这个inode
 - 文件数据block的位置

查看inode信息

```shell
stat /etc/passwd

```

inode的大小

inode也会消耗硬盘空间，所以硬盘格式化的时候，操作系统自动将硬盘分成两个区域，一个是数据区，存放文件数据；另一个是inode区(inode table)，存放inode所包含的信息。

```shell
df -i
```

每个文件至少有一个inode号。操作系统用inode号码来识别不同文件

```shell
ls -i /etc/passwd # 查看文件inode号
ls -id /etc # 查看目录的inode号
```

### 文件软硬链接

ln命令可以创建硬链接

ln 源文件 目标文件

硬连接:  不同的文件名相同的inode指向同一个块设备

文件名1 -> inode1 -> blockA

文件名2 -> inode1 -> blockA

硬链接特点: 创建时不能跨分区，不能给文件夹创建



软链接: 相当于windows中的快捷方式

ln -s 源文件或目录 目标文件或目录

为什么新建的目录链接数是2， 因为除自身外还有. 指向同一个inode, 再创建子目录时会变为3，是因为多了..

```shell
mkdir foo
cd !$
ls -id .
ls -id foo
mkdir bar
cd !$
ls -id bar/..

```

实例:

web服务器中小文件很多，导致硬盘有空间，但无法创建文件。

是因为inode数被用光了

```shell
df -i # 检查inode数
```

block设置大: 效率高，利用率低

block设置小: 效率低, 利用率高

一般系统默认就行。

### ext4 文件系统 比Ext3 文件系统强的方面

1.  ext4与ext3兼容
2.  更大的文件系统和更大的文件。较之ext3目前所支持的最大16TB文件系统和最大2TB文件，EXT4分别支持1EB(1048576TB, 1EB = 1024PE, 1PB = 1024TB)的文件系统，以及16TB的文件
3.  无限数量的子目录。ext3目前仅支持32000个子目录，而ext4支持无限数量的子目录
4.  “无日志”(no journaling) 模式。日志总归有一些开销，Ext4允许关闭日志，以便某些有特殊需求的用户可以借此提升性能



### 磁盘管理

mbr: master boot record 主引导扇区

硬盘的0柱面，0磁头，1扇区称为主引导扇区也叫主引导记录mbr, 它由三个部分组成，主引导程序/硬盘表dpt(dist partition table)和分区有效标志(55AA)

在总共512字节的主引导扇区里主引导程序 (boot loader)占446个字节，第二部分是partition table区(分区表)，即dpt，占64个字节，16 * 4 = 64，硬盘中分区有多少以及每一分区的大小都记在其中。第三部分是magic number，占2个字节，固定为55AA.



添加磁盘步骤

添加设备	分区 格式化(创建文件系统) [起名] 修改配置文件 创建挂载点 挂载

fdisk

```shell
fdisk -l # 查看信息
fdisk /dev/sdb # 创建/管理分区
fdisk m # 获取帮助
fdisk p # 打印分区表
fdisk n # 新建
fdisk e # 扩展 在mbr这亲的分区表中，只有一个扩展分区，最多4个主分区， p 主分区， e 扩展分区
		# 逻辑分区
fdisk q # 退出 
fdisk d # 删除
fdisk w # 保存
```

格式化

mkfs.ext3 /dev/sda5

mkfs.ext4 /dev/sda5

挂载

```shell
mkdir /sda5
mount /dev/sda5 /sda5/
ls /sda5/
df -h
```

卸载

umount /sda5 或 umount /dev/sda5

检查  mount -a

```shell
fuser -muv /sda5 # 查看文件正在被谁使用
```

GPT分区

GPT： 全局唯一标识分区表(GUID partition Table)， guid,与mbr最大4个分区表项的限制相比，GPT对分区数量没有限制，但windows最大仅支持128个分区。GPT可管理硬盘大小达到了18EB(1048576TB)， 不过NTFS格式最大仅支持256TB.(受限于文件系统 )

```shell
parted
> help
> quit
parted -l # fdisk -l
parted /dev/sdc 
> p
> gpt
> mkpart
...
> mkpart
...
> quit

```

## lvm

逻辑卷管理logical volume manager

是linux环境下对底层磁盘的一种管理机制，处在物理磁盘和文件系统之间的

pv physical volume (物理卷)

vg volume group (卷组)

lv logical volume (逻辑卷)

最小存储单位: PE

总结

名称： 最小存储单位

硬盘:  扇区(512字节)

文件系统 block (1K 或 4K)

raid: chunk (512K) mdadm -c

lVM: PE (16M 自己定义)



开始

在虚拟机中添加一块或多块硬盘 : sdb

准备分区

\# fdisk /dev/sdb 分三个分区1,2,3

制作PV

```shell
pvcreate /dev/sdb{1,2}

```

制作VG

```shell
vgcreate /dev/sdb1 /dev/sdb2

```

制作LV

```shell
lvcreate -n LV1 -L 1.5G Vg1 # -n 名称 -L 大小
```

pv -> 对应 分区

vg -> 对应  卷组

lv -> 逻辑卷

各种查看

pvs		pvscan 		pvdisplay

vgs 		vgscan 		vgdisplay

lvs 		lvscan		lvdisplay

创建时指定PE

```shell
vgcreate -s 16M vg1 /dev/sdb1 /dev/sdb2 # -s 指定PE大小
mkfs.ext4 /dev/Vg1/LV1
mount /dev/Vg1/LV1 /opt

```

lV扩展

```shell
lvextend -L +300M /dev/Vg1/LV1 # 扩展完之后需要再扩展文件系统 
resize2fs /dev/Vg1/LV1
df -h

```

VG扩展

```shell
vgextend Vg1 /dev/sdb3
```

### lvm 缩减 删除

lvm支持在线缩小，但ext4文件系统不支持在线缩小。btrfs支持在线缩小

btrfs的特性

首先，是扩展性。其整体性能不会随着系统容量的增加而降低

其次是数据一致性相关的特性

第三是多设备管理相关的特性。btrfs支持创建快照和克隆

这些特性都是比较先进的技术，能够显著提高文件系统的时间 / 空间性能，包括延迟分配，小文件的存储优化，目录索引等。

卸载umount, 缩减文件系统大小，缩减lv大小lvreduce





# 网络管理相关命令

## OSI七层模型和TCP/IP四层模型

osi七层模型

应用层

表示层 -> ascii

会话层

传输层 -> 防火墙

网络层 -> 三层交换机和路由器

数据链路 -> 二层交换机和网卡

物理层 -> 集线器



tcp/ip 四层模型

应用: http ftp

传输 tcp upd 数据包网络

网络层 路由器

链路接口层





## IP地址分类

分5类: 常见的是A, B, C类

A 类 范围从0-127， 0是保留的并且表示所有的IP地址，而127也是保留地址，并且是用于测试环回口用的。因此A类地址的可用范围其实是从1-126之间，以子网掩码来区别: 255.0.0.0

B类 范围从128-192， 如172.168.1.1，第一和第二段为网络号码，剩下的2段号码为本地计算机的号码，以子网掩码来进行区别: 255.255.0.0

C类地址: 范围从192-223,以子网掩码来进行区别: 255.255.255.0

例如: 192.168.1.254 (192.168.1.255是广播地址)

D：范围从224-239, 被用在多点广播(multicast)中，多点广播地址用来一次寻址一组计算机，它标识共享同一协议的一组计算机， 有组播的发况下，不需要多次发相同包到不同客户机

私有IP地址

A: 10.0.0.0/8 # 8, 16, 24 表示子网掩码

B: 172.16.0.0 ~ 172.31.0.0/16

C: 192.168.0.0 ~ 192.168.255.0/24

环回口一直是可用up状态

```shell
ifconfig lo
ping 127.23.23.23 # 所有127打头均可ping通
```



## 了解常见的网络相关协议

TCP/IP协议

​	tcp/ip协议是一个协议簇，里面包含很多协议

​	例如:

​		http： 超文本传输协议

​		tftp：文件传输

​		telnet: 远程登陆

​		snmp: 网络管理

​		tcp transmission control prototal 传输控制协议，面向连接的协议

​		udp user data protocol 用户数据报协议 非连接的协议

​		Internet ip协议

​		internet 控制信息协议 icmp

​		地址解析协议 arp

​		反向地址解析协议rarp



## TCP三次握手和四次挥手

​	tcp和udp的区别

  - 基于连接与无连接
  - 对系统资源的要求(tcp较多，udp少)
  - udp程序结构较简单
  - 流模式与数据报模式
  - tcp保证数据正确性，udp可能丢包，tcp保证数据顺序，udp不保证

tcp: 三次握手的状态

​	c								s

​	tcp 连接状态		建立过程		tcp连接状态

​									listen

​	SY N_SENT		---syn seq=a ---> 		SYN_RCVD

​	ESTABLISHED 	<--syn seq=b		ack=a+1

​				--ack=b+1--->			ESTABLISHED			



客户端发送syn报文，并置发送序号为X		

​									服务端发送syn+ack报文，并置发送序号为Y,在确认序号为X+1

客户端发送ack报文，并置发送序号为，在确认序号为Y+1

​		

TCP 四次挥手

主动方													被动方

主动发发送Fin+Ack报文并轩发送序号为X

​														被动方发送ACK报文，并置发送序号为Z,在确认序号为X+1

​														被动方发送Fin+ack报文，并置发送序号为Y,在确认序号为X

主动方发送ack报文，并置发送序号为X,再确认序号为Y



四次挥手状态

关闭连接: 四次挥手

c									c

FIN_WAIT		--fin seq=a -->		CLOSE_WAIT

FIN_WAIT2 		<--ack a + 1--

TIME_WAIT		<--fin b--				LAST_ACK

​				--ack b+1--> 			CLOSE		



## 网络相关的调试命令

## 网络接口类型表示方法

以太网(ethernet) 					ethN

回环口							lo

光纤网							fddiN

桥设备							br0

Linux ADSL宽带接口				ppp pppN如:ppp0

随道							tun0 tun1

## 查看网卡物理连接信息

```shell
mii-tool eth0
```

## 查看网卡详细参数

```shell
ethtool eth0
# speed 
# duplex
```

## 配置网络和IP地址

- 方法一

  - setup (TUI， 方式，文本用户界面) 配置IP地址

    - 在rhel6中增加了一个新的网络服务 /etc/init.d/NetworkManager stop
    - chkconfig NetworkManager off
    - service network restart
    - [for ubuntu](https://help.ubuntu.com/lts/serverguide/network-configuration.html)

  - 修改网卡配置文件

    ​

```shell
ifconfig all
ifconfig eth0
ifconfig eth0 up/down
ifdown eth0
ifconfig -a
ifup eth0


ifconfig eth0:1 192.168.1.80
ifconfig eth0:2 192.168.2.80

# 修改网卡配置文件
vim /etc/sysconfig/network-scripts/ifcft-eth0
# ubuntu /etc/network/interfaces





```

## 网络调试相关命令

设置主机名

```shell
vim /etc/sysconfig/network
```

ip和主机名(域名)对应关系

```shell
cat /etc/hosts
# hosts优先级高于dns 可更改 /etc/nsswitch.conf host项
cat /etc/resolv.conf
# !命令前缀可以快速补全历史记录
# 端口对应协议
cat /etc/services
```

查看路由信息

```shell
route -n
0.0.0.0 ****** 0.0.0.0 对应的是默认网关
```

- destination 目录 目标网络或目标主机
- gateway 网关 网关地址，如果没有就显示星号
- genmask 网络掩码 

添加路由(把linux做成路由器时或服务器有多个网卡，指定到不同的网段走哪个网卡)

删除路由

```shell
route add -net 192.168.2.0 netmask 255.255.255.0 dev eth1 # dev 设备
route del -net 192.168.2.0 netmask 255.255.255.0 
```

 -net 表示后面新年好的路由为一个网域

-host 表示后面接的为连接到单部主机的路由

netmask 与网域有关，可以设定netmask决定网域的大小

dev: 如果只是要指定由哪一块网络卡连线出去，则使用这个设定，后面接eth0等



查看连接状态

```shell
netstat -antup
```

-a, --all

-n, --numberic

-p, --programs

-t, 显示 tcp连接

-u 显示udp连接

-p 进程



ping 

```shell
ping -c 1 192.168.1.1 # 指定次数
ping -i 0.001 192.168.1.1 # 指定间隔周期
ping -I eth0 192.168.1.1 # 指定网卡


```

查看网络流量

```shell
iptraf

```

查看IP地址是否有冲突

```shell
arping 192.188.1.1 # arp
# arp

```

## 实战tcpdump和tshark抓包

tcpdump命令

指定接口 -i

指定IP地址(host) 可以辅加and, or !等逻辑以及src, dest等表示方向



```shell
tcpdump -i eth0
tcpdump port 22 -c 3 -n -S # tcpdump -h
# port 端口号
# -c 抓几个包
# -n 不解析端口号为协议名
# -S 打印tcp sequence numbers
telnet localhost 22

tshark -w filename =i eth0 -q
# -w 将抓包的数据写入到文件filename中
# -i 指定要抓包的接口名称

tshark -r filename
# -r 指定要读取的包文件
# -V 将包尽可能的解析，这个有时在包数量很多的情况下可以不使用，这样它会给出一个很简洁的报文解释

```



# shell脚本编程

# case-for-while

# shift命令



# 正则表达式-awk-sed使用方法

# DHCP服务器工作原理

# DNS服务器

# apache服务器

# lvs

# nginx

# keepalived

# apache

# kvm

# docker





TODO

-[ ] shell pdf
-[ ] python pdf
-[ ] storm
-[ ] elasticsearch
-[ ] hadoop
-[ ] spark
-[ ] hbase
-[ ] keepalived
-[ ] nginx
-[ ] mongo
-[ ] kafka
-[ ] redis
-[ ] mapreduce
-[ ] https://ke.qq.com/course/219551
-[ ] haproxy
-[ ] lvs
-[ ] 大数据就是这么任性第一季数据结构









[python algorithms and data structure](https://www.youtube.com/playlist?list=PLib7LoYR5PuDxi8TxxGKxMgf8b-jtoS3i)

[machine learning with python](https://www.youtube.com/playlist?list=PLQVvvaa0QuDfKTOs3Keq_kaG2P55YRn5v)









# 算法

-[ ] [C算法](http://edu.51cto.com/center/course/lesson/index?id=181152)

## 百钱百鸡

> 公鸡5元一只，母鸡3元一只，3只小鸡1元，100元买100只鸡，求组合

```c
#include <stdio.h>

int main() {
    int x, y, z; // x: 公，y: 母, z: 小
  	for (x = 0; x <= 20; x++) {
        for (y = 0; y <= 33; y++) {
            if (z % 3 == 0 && x * 5 + y * 3 + z / 3 == 100) {
                printf("公鸡: %d, 母鸡: %d, 小鸡: %d\n", x, y, z);
            }
        }
    }
  	return 0;
}
```

## 凑数

> 输入5个数，通过加减乘除，使前4个数

## 裴波纳契算法





















