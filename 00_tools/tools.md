[TOC]

# 1. conemu/cmder

windows下终端工具，支持多tabs, panes等,结合`git-bash`,`vagrant`等，极大的提高效率

配置bash

```shell
git-cmd.exe --no-cd --command=usr/bin/bash.exe -l -i
```

设置默认启动项在startup中选取对应的bash一项

右键启动设置在Integration一栏

# 2. atom plugins

- file-icons
- atom-beautify

# 3. visual studio code plugins

- react code snippets
- vs code great icons
- bookmark
- project manager
- guides


# 4. ubuntu 无gui安装virtualbox

https://www.virtualbox.org/wiki/Linux_Downloads

更新virtualbox源

```shell
sudo apt-add-repository "deb http://download.virtualbox.org/virtualbox/debian $(lsb_release -sc) contrib"
```

添加secure key:

```shell
sudo apt-key add oracle_vbox_2016.asc
sudo apt-key add oracle_vbox.asc
```

安装virtualbox

```shell
sudo apt-get update
sudo apt-get install virtualbox-5.1
```

