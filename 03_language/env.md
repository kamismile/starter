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

   4. 根据提示安装必要插件或依赖项