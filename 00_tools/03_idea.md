[TOC]

# 视频

1. [IntelliJ IDEA Video Tutorials](https://www.youtube.com/playlist?list=PLPZy-hmwOdEXdOtXdFzyx_XCnrF_oD2Ft)
2. [IntelliJ IDEA demos](https://www.youtube.com/playlist?list=PLQ176FUIyIUY8x6f6JvEav8BQSyp6f1Ro)


- ctrl + alt + v 提取变量 
  - 比如System.out.println("hello")提取变量"hello"到字符串
- ctrl + alt + f提取变量
- ctrl + alt + c提取常量
- alt + home 跳转面包屑 
  - 全屏状态下可在面包屑上导航不用alt + 1到侧边栏
- psf public static final
- prsf private static final 
- iter 
- ctrl + b查看声明，也可以查看被使用情况
- 尽量不使用tabs,
- ctrl + n  file:lines
- ctrl + shift + n
- ctrl + alt + shit + n
- double shift : show recent files comands + tab navgate
- ctrl + e : recent files
- ctrl + shift + e: recent editing files
- ctrl + alt + left / right
- esc / alt + 1 / f4
- alt + shift + up / down
- alt + shift + j
- ctrl + alt + g
- ctrl + alt + shift + j : show all occurency
- ctrl + alt  + l : reformat
- ctrl + alt + i : indent
- ctrl + shift + left / right ： 调整侧边栏大小
- language inject (json, sql, regex)
- person.notnull tab
- ctrl + h hierarchy






# idea vim

: e 编辑文件

: find 查找并打开文件(ctrl + shift + n)



# idea tools

- mps : dsl tools amazing
- pycharm edu : tutor

# create class

If you are already in the Project View, press Alt+Insert (**New**) | **Class**. Project View can be activated via Alt+1.

To create a new class in the **same directory** as the current one use Ctrl+Alt+Insert (**New...**).

You can also do it from the Navigation Bar, press Alt+Home, then choose package with arrow keys, then press Alt+Insert.

Another useful shortcut is View | Select In (Alt+F1), Project (1), then Alt+Insert to create a class near the existing one or use arrow keys to navigate through the packages.

And yet another way is to just type the class name in the existing code where you want to use it, IDEA will highlight it in red as it doesn't exist yet, then press Alt+Enter for the Intention Actions pop-up, choose **Create Class**.



# terminal

>**Setup JetBrains(InteliJ, WebStorm, PHPStorm) IDE terminal to use GIT bash**
>
>File-->Settings-->(Enter 'Terminal' in search) Change Shell path to:
>
>"C:\Program Files (x86)\Git\bin\sh.exe" --login -i