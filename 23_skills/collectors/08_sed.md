# sed基础应用

[TOC]

## sed介绍

- sed是什么
  - 面向字符流的编辑器，非交互编辑器，linux最重要的自动作运维工具之一
- sed作用
  - 在一个或多个文件上自动实现编辑操作
  - 编写转换程序
  - 实现自动化运维
- sed学习难点
  - 理解工作方式
  - 正则表达式
  - 与shell的配合工作
  - 编写技巧

## sed原理

- 模式空间与缓存空间
  - 模式空间(pattern space): 处理一行内容时存放该行内容的区域,处理完以后会被打印到标准输出
  - 缓存空间(hold space): 不会自动清空
- 工作流程
  - 行是处理的单位，一次处理一行

## 正则表达式

- 基础
  - 计算机编程中的一个概念，不属于特定的编程语言
  - 元字符 
    - . 匹配除换行符以外的任意字符
    - \w 字母、数字、下划线或汉字
    - \s 空白符
    - \d 数字
    - \b 单词的开始或结束
    - ^ 字符串开始
    -  $ 字符串结束
  - 限定符
    - \*  >=0
    - \+ >=1
    - ? <=1
    - {n} n
    - {n, } >=n
    - {n,m} >=n <=m
    - [1-9] 指定范围内的字符
  - 反义
    - \W 非字母、数字、下划线、汉字
    - \S 不是空白符的字符
    - \D 非数字的字符
    - \B 不是单词开头或结束的位置
    - [^x] 除x以外的任意字符
    - [^aeiou]匹配除了aeiou这几个字符以外的任意字符
  - 捕获
    - (exp) 匹配exp,并捕获文本到自动命名的组里
    - (?<name>ex) 匹配ex, 并捕获文本到名称为name的组里
    - (?:exp) 匹配exp,不捕获匹配的方本，也不分配组号
  - 零宽断言
    - (?=exp) 匹配exp前面的位置
    - (?<=exp) 匹配exp后面的位置
    - (?<!exp) 匹配后面跟的不是exp的位置
    - (?<!exp) 匹配前面不是exp的位置
  - 贪婪与懒惰
    - *? 重复任意次，但尽可能少的重复
    - +? 1到多次，尽可能少重复
    - ?? 0到1次，但尽可能少
    - {n,m}? n到m次，但尽可能少
    - {n,}? n次以上，但尽可能少
  - sed
    - 默认为贪婪模式，不支持懒惰模式
    - 使用正则表达式时要注意shell特殊字符冲突问题， 比如\\(exp\\)
    - 不支持\\d和反义中的\\W等表达方式
- 进阶
- sed中的正则表达式

## sed基础命令

- 命令格式
  - sed [-options]     \[ commands]  filename
  - command格式:
    - [address-range]  \[pattern-to-match]  \[sed-command]
    - sed -n '5, 8p' passwd
    - sed -n '/bash$/p' passwd
    - sed -n '/^test/d' passwd
    - sed -i '/^test/d' passwd
    - sed -n '3,8p' passwd
    - sed -nr '/bash$/s/(\w+):.*/\1/p' passwd
    - sed -nr '/bash$/s/(\w+):\w:[0-9]+:.*:(.*):.*/\1 \2/p' passwd
    - sed -n '/bash$/\\(.*\):\w\+:[0-9]\\+:.*:\(.*\\):.*/\1:\2/p' passwd
    - sed -i 's/nologin/jike' passwd
    - sed -i '5i\hello world' passwd
    - sed -i '5a\hello world2' passwd
    - sed -i '/hello/d' passwd
    - sed -i '5\c\hello world' passwd
    - echo abcdef | sed 'y/abcdef/ABCDEF/'
    - echo fedcba | sed 'y/abcdef/ABCDEF/'
    - sed '5q' passwd
- 查找、删除、打印
- 提取、替换
- 插入、追加、更改
- 转换、退出

## sed自动化实战应用

- 一键安装脚本

  - 安装PPTPD服务，并配置用户为guest,密码为123456

  - ```shell
    sudo apt-get install pptpd
    sed -i '/logwtmp/# logwtmp/' /etc/pptpd.conf
    touch auto.sh
    vim auto.sh
    ifconfig eth0 | grep "inet addr:" | cut -d: -f2 | cut -d" " -f1
    ifconfig eth0 | grep "inet addr:" | awk -F'[ :]+' '{print $4}'
    ifconfig eth0 | grep "inet addr:" | sed 's/\s+inet addr:([0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}).*/\1/p'
    ```

  - ​























































