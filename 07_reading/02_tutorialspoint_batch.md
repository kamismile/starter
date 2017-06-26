batch script tutorial

[TOC]

# home

# overview

批处理脚本特征:

- 可以读取用户 输入值并保留待后续处理 
- 有流程控制语句诸如 for, if, while, switch
- 支持函数和数组等高级特性
- 支持正则表达式
- 可以包含其它语言的代码比如perl

批处理常用场景：

- 启动服务器以便不同的需求
- 自动化清理工作比如删除不想要的文件或日志文件
- 从一个环境到另一个环境的自动化的应用部署
- 在不同机器上同时安装程序

批处理脚本存储在包含按序执行的命令行的文本中。这些文件有特定的扩展名bat或cmd,这种类型的文件会被识别和执行于一种叫作命令解释器的接口中(有时也叫sehll)。在windows系统中，这种解释器也叫cmd.exe。

运行批处理文件只需要双击文件执行即可。batch文件也可以在命令提示符中运行。这种情况下，全路径文件名要用到，除非文件路径在环境变量中。以下是一个简单的例子。这个脚本在运行时会删除当前目录下的所有文件

```powershell
:: delete all files in the current directory with prompts and warnings
:: (hidden, system, and read-only files are not affected)
:: @ECHO OFF
DEL . DR
```

# environment

为了在cmd.exe中运行脚本，可以敲入脚本全路径或将脚本所在目录加到环境变量中去

# commands

常用的batch命令

| commands | descriptions                 |                              |
| -------- | :--------------------------- | ---------------------------- |
| ver      | 查看ms-dos 版本version           |                              |
| assoc    | 关联文件类型                       | assoc 查看所有文件扩展名              |
|          |                              | assoc  \| find ".ext"        |
| cd       |                              |                              |
| cls      |                              |                              |
| copy     | copy [source] \[destination] | copy c:\dirA dirB            |
| DEL      | 删除文件而非目录                     | del /s *.txt 递归删除            |
|          |                              | del /p /s *.txt 递归删除但要求用户 确认 |
| DIR      |                              | dir /s 递归查看目录及子目录            |
|          |                              | dir /s /b递归且 展示完整目录          |
| DATE     |                              | date 或echo %date%            |
| ECHO     |                              | on off                       |
| exit     |                              |                              |



echo off 不显示执行过的命令，连echo off也不想显示就用@echo off

| md   | md [new directory name]      |                           |
| ---- | ---------------------------- | ------------------------- |
| move | move [source] \[destination] | 移到文件到指定目录 move file c:\tp |
|      |                              | 修改目录名称 mvoe dir1 dir2     |
| PATH |                              | echo %PATH%               |
|      |                              |                           |
|      |                              |                           |
|      |                              |                           |
|      |                              |                           |
|      |                              |                           |
|      |                              |                           |
|      |                              |                           |
|      |                              |                           |
|      |                              |                           |
|      |                              |                           |
|      |                              |                           |
|      |                              |                           |
|      |                              |                           |
|      |                              |                           |
|      |                              |                           |
|      |                              |                           |
|      |                              |                           |
|      |                              |                           |
|      |                              |                           |
|      |                              |                           |
|      |                              |                           |
|      |                              |                           |
|      |                              |                           |
|      |                              |                           |
|      |                              |                           |
|      |                              |                           |
|      |                              |                           |
|      |                              |                           |
|      |                              |                           |
|      |                              |                           |
|      |                              |                           |
|      |                              |                           |
|      |                              |                           |
|      |                              |                           |
|      |                              |                           |
|      |                              |                           |
|      |                              |                           |
|      |                              |                           |
|      |                              |                           |
|      |                              |                           |
|      |                              |                           |
|      |                              |                           |
|      |                              |                           |
|      |                              |                           |
|      |                              |                           |
|      |                              |                           |
|      |                              |                           |































