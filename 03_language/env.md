环境安装

[TOC]

# 01 GO

## win

1. zip archive下载解压

2. 环境变量配置 

   1. GOROOT: 解压包bin parent目录，即根目录
   2. PATH, 将GOROOT/bin添加到path中
   3. GOPATH， 项目路径地址，1.8以后默认为%HOME/bin， 可设定，vscode debug工具运行时需要用到

3. vscode 使用

   1. go plugin, code runner 安装

   2. ```go
      package main

      import "fmt"

      func main() {
        fmt.Printf("hello wornd\n")
      }
      ```

   3. 运行code runner，若无错，则继续加入断点

   4. 根据提示安装必要插件或依赖项，比如"analysis tools missing"

   5. GOPATH multiple

      ```json
      // 有时需要因为多个项目的需要要设定多个GOPATH, 在vscode中安装go插件后，可设定下列项为true,则当前根目录会作为GOPATH
          "go.inferGopath": true

      ```

# 02 PYTHON

win

1. 下载安装msi到指定路径

2. pip install virtualenv

3. 运行virtualenv  ENV 创建特定的ENV环境 比如virtualenv virtualenvs

4. vscode 使用

   ```json
       "python.pythonPath": "D:\\devtools\\Python\\virtualenvs\\py36\\Scripts\\python.exe",

   ```

   ​