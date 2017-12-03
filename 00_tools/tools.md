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
- c/c++
- plantUML
- code runner
- debugger for chrome
- eslint
- go
- npm
- npm intellisense
- path intellisense
- python
- reactjs code sinippets
- vue 2 snippets
- bootstrap 3 snippets
- react standard style code snippets
- bootstrap4 & font awesome snippets
- vscode_hexdump [vim 中使用%!xxd命令也可查看]
- java


## [php programming in vs code](https://code.visualstudio.com/docs/languages/php) 

### plugins

1. code runner
2. php intellisense
3. html css support
4. php debug

可以使用`php.suggest.basic`禁用内置的php语言提示功能来支持php插件的提示功能

### snippets

ctrl + space可以查看提示 （快捷键可修改）

### linting

- php.validate.enable
- php.validate.executablePath
- php.validate.run

### debugging

php debug extension xdebug

## Python on Visual Studio Code

### install python extension

- python
- code runner


### code completion

### linting

don jayamanne's python extension 有三种不同的linters， pylint, pep8, flake8

### debug

不要再使用print，设置断点，观察数据，使用debug控制台

### snippets

ctrl + space 且可自定义snippets

## Go programming in vs code

安装vscode go extension之后，可以获得如下好处，intellisense, code navigation, symbol search, bracket matching, snippets等

### intellisense

#### auto completions

#### hover information

### code navigation

- go to definition f12
- peek definition alt + f12
- find all references shift + f12
- go to symbol in file ctrl + shift + o
- go to symbol in workspace ctrl + t
- 可以使用go:toggle test file指令(ctrl + shift + p)

### build lint and vet

### formatting

### test

### debug

## CPP

plugins: c/c++ 、 c++ intellisense



​


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

# 5. eclipse templates / idea live templates

[useful eclipse java code templates](https://stackoverflow.com/questions/1028858/useful-eclipse-java-code-templates)

```java
System.out.println(${text});

// logger
private static final Logger logger = Logger.getLogger(${enclosing_type}.class.getName());

// loglevel
if (${logger:var(java.util.logging.Logger)}.isLoggable(Level.${LEVEL})) {
 ${logger:var(java.util.logging.Logger)}.${level}(${}); 
}
${cursor}

// readfile
BufferedReader in;
try {
  in = new BufferedReader(new FileReader(${file_name}));
  String str;
  while ((str = in.readLine()) != null) {
    ${process}
  }
} catch (IOException e) {
  ${handle}
} finally {
  in.close();
}
${cursor}

// slf4j
${:import(org.slf4j.Logger,org.slf4j.LoggerFactory)}
private static final Logger LOG = LoggerFactory.getLogger(${enclosing_type}.class);

// log4j2
${:import(org.apache.logging.log4j.LogManager,org.apache.logging.log4j.Logger)} 
private static final Logger LOG = LogManager.getLogger(${enclosing_type}.class);

// log4j
${:import(org.apache.log4j.Logger)}
private static final Logger LOG = Logger.getLogger(${enclosing_type}.class);

// jul
${:import(java.util.logging.Logger)}
private static final Logger LOG = Logger.getLogger(${enclosing_type}.class.getName());

// jul
 ${:import(java.io.BufferedReader,  
           java.io.FileNotFoundException,  
           java.io.FileReader,  
           java.io.IOException)}  
 BufferedReader in = null;  
 try {  
    in = new BufferedReader(new FileReader(${fileName}));  
    String line;  
    while ((line = in.readLine()) != null) {  
       ${process}  
    }  
 }  
 catch (FileNotFoundException e) {  
    logger.error(e) ;  
 }  
 catch (IOException e) {  
    logger.error(e) ;  
 } finally {  
    if(in != null) in.close();  
 }  
 ${cursor} 

// java 7 readfile
${:import(java.nio.file.Files,
          java.nio.file.Paths,
          java.nio.charset.Charset,
          java.io.IOException,
          java.io.BufferedReader)}
try (BufferedReader in = Files.newBufferedReader(Paths.get(${fileName:var(String)}),
                                                 Charset.forName("UTF-8"))) {
    String line = null;
    while ((line = in.readLine()) != null) {
        ${cursor}
    }
} catch (IOException e) {
    // ${todo}: handle exception
}

// createsingleton
static enum Singleton {
    INSTANCE;

    private static final ${enclosing_type} singleton = new ${enclosing_type}();

    public ${enclosing_type} getSingleton() {
        return singleton;
    }
}
${cursor}

// getsingleton
${type} ${newName} = ${type}.Singleton.INSTANCE.getSingleton();
```

```xml
// maven dependency
<dependency>
	<groupId>${groupId}</groupId>
  	<artifactId>${artifactId}</artifactId>
  	<version>${version}</version>
</dependency>

// maven parent
<parent>
	<artifactId>${artifactId}</artifactId>
  	<groupId>${groupId}</groupId>
  	<version>${version}</version>
  	<relativePath>${path}/pom.xml</relativePath>
</parent>

// web.xml servlet
<servlet>
	<servlet-name>${servlet_name}</servlet-name>
  	<servlet-class>${servlet_class}</servlet-class>
  	<load-on-startup>${0}</load-on-startup>
</servlet>

<servlet-mapping>
	<servlet-name>${servlet_name}</servlet-name>
  	<url_pattern>*.html</url_pattern>
</servlet-mapping>
${cursor}
```

# 6. npm plugins

live-server 比 http-server好用的一点在于，多了websocket服务器推送，在文件修改后，自动刷新页面，需要手动刷新浏览器

# 7. idea plugins

translator / translation 中英互译， 阅读代码注释时更方便

java stream debugger : jdk8 stream 可视化调试工具

grep console： 控制台日志颜色区分，查看控制台输出更直观

ide features trainer 

key promoter x

lombok

maven helper

jmh

jprofiler

ideaVim

plantUML integration

visualvm launcher

flow

docker integration

presentation assistant 快捷键使用显示

 - 使用方式 集成jmh-generator-annprocess依赖项，win alt+insert添加jmh测试项，在方法上运行ctrl+shift+f10即可运行

# 8. windows caps ctrl交换 sharpkeys

windows下caps键不怎么实用，可以和ctrl交换，这样剪切粘贴操作会方便很多，一般windows下通过修改注册表完成，如果嫌麻烦，可以使用sharpkeys这款小工具完成。

# 9. chrome plugins

- css used 可方便查看某个甚至整个页面使用到的样式,可方便拷站点

# 10. vagrant box download offline

vagrant 镜像下载方式，一般是通过命令`vagrant box add [boxname]`来操作，有时网络原因，可以通过http方式下载再离线导入

以ubuntu/trusty64为例，地址为https://app.vagrantup.com/ubuntu/boxes/trusty64/versions/20170619.0.0/

可以在浏览器后输入https://app.vagrantup.com/ubuntu/boxes/trusty64/versions/20170619.0.0/providers/virtualbox.box下载virtualbox版本的镜像或通过**wget**命令也可

# 11. docker 镜像下载方式 offline

docker hub或最新的docker store上下载镜像时由于是国外的站点可能会有很多问题，可使用国内的如阿里云或灵雀云，配置registry地址等，如果有洁癖可以在下载完成后使用tag 重新设置成官方tag方式 **docker tag source[:tag] target[:tag]**, 当然也可以使用docker hub 上的dockerfile文件及对应材料本地build构建，docker hub上的镜像说明页面一般附有dockerfile的git地址，选定版本使用[downgit](https://minhaskamal.github.io/DownGit/#/home)即可下载sources material,然后本地解压，进入对应目录运行 `docker build . -t name:tag`即可。有时会有额外的项或父镜像的下载，有必要的话可以按此方式先构建父镜像再构建当前镜像

# 12. java工具

- spring boot admin
- dropwizard metric-core (microservices)
- btrace

# 13. jetbrains 使用jetbrain toolbox取代直接下载jetbrain idea zip绿色版

下载完成toolbox.exe后修改扩展名为.exe.zip然后解压后直接使用即可

# 14. linux tools

- dstat -cmdlns
- screen / tmux


# 15 remote control

- teamviewer


# 16 eclipse plugin

jbenchx

# 17 java查询所有 vm ops参数

```shell
java -XX:+UnlockDiagnosticVMOptions -XX:+PrintFlagsFinal -version
java -X # 查看所有-X参数
# 查看默认堆大小
java -XX:+PrintFlagsFinal -version | findstr HeapSize
java -XX:+PrintFlagsFinal -version | grep HeapSize
```

# 18 kindle azw3

Desktop: azw3文件放到tools下的配置的文件路径下

android pad: /kindle目录下就好

# 19 Intel VTune性能监测工具

# 20 btrace / jmc （JDK7自带）

# 21 ngrok内网代理

# 22 pstree 查看所有进程

```shell
pstree -p <pid>
top -Hp <pid>
ps -eT | grep <pid>
```

# 23 downgit下载git上指定目录rawgit 下载git上单独文件

# 24 code flow visualization

 - eclipse / intellij idea:  ctrl + alt + h
 - jtracert <http://code.google.com/p/jtracert/>
 - jsonde <https://github.com/bedrin/jsonde>
 - doxygen https://www.stack.nl/~dimitri/doxygen/manual/starting.html
 - btrace https://github.com/btraceio/btrace
 - zeta code https://www.kickstarter.com/projects/adamldavis/zeta-code
 - jeliot https://cs.joensuu.fi/jeliot/downloads/jeliot372.php
 - intellij idea plugin: code explorer 
 - jgrasp: http://spider.eng.auburn.edu/user-cgi/grasp/grasp.pl
 - heatlamp
 - http://findtheflow.io

btrace download https://github.com/btraceio/btrace/releases/tag/v1.3.9

## 25 btrace使用

- 下载  :https://github.com/btraceio/btrace/releases/
- 使用


#26 mac

- Copy: cmd + c
- Paste: cmd + v
- Cut: cmd + x
- select all: cmd + a
- Undo: cmd + z
- redo: cmd + shift + z
- switch programs: cmd + tab
- close window: cmd + w
- new document: cmd + n
- save document: cmd + s
- save as : cmd + shift + s
- open file: cmd + o
- Print: cmd + p
- quit the program: cmd + q
- Spotlight: cmd + space
- new folder: cmd + shift + n
- Delete: cmd + delete
- empty the trash: cmd + shift + delete
- open preferences: cmd  + ,
- move to right or left: cmd + < or cmd + >
- move to top or bottom: cmd + down or cmd + up
- capture entile screen: cmd + shift + 3
- capture custom area: cmd + shift + 4
- capture window: cmd + shift + 4 + space

# 27 mac softs

- alfred
- keycastr
- cleanmymac 3
- spectacle
- keyboard maestro
- mindnote
- iterm
- homebrew
- zsh
- sketchup
- cheatsheet for mac
- screenflow
- transmit
- dash
- 1password
- appcleaner
- keymo
- vagrant / vagrant manager
- staruml
- axure rp 8
- omnigraffle

# 28 Vagrant CentOS安装报Rsync不在path中

[vagrant centos安装常见报错](http://blog.csdn.net/shilei_zhang/article/details/72811274)

```ruby
# 添加sync folder type为virtualbox
# vagrant plugin install vagrant-vbguest
config.vm.synced_folder "E:/stone/month11_null", "/vagrant", type: "virtualbox"
```

# 29 vim /emacs plugin

- org mode 

- vim

  - 入门

    - vimtotor命令

  - vundle

    - [地址](https://github.com/VundleVim/Vundle.vim)

  - vim-plug

    - ```shell
      curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
          https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
      ```

    - ​

    - PlugInstall

    - PlugUpdate

    - PlugClean

    - PlugUpgrade

    - PlugStatus

    - PlugDiff

  - PathOgen

    - ```shell
      cd ~/.vim
      mkdir bundle
      cd bundle
      git clone https://github.com/vimwiki/vimwiki.git


      # launch vim run :HelpTags then :help vimwiki to verify it wasy installed
      ```

  - NERDTree

    ```shell
    vim .
    # 开启NERDTree
    :NERDTree
    # 查看帮助文档
    ?
    ```

  - tab

    ```shell
    vim . 
    # 查看tab相关
    :help tabnew
    :help tabprev
    :help tabnext
    :help tab

    ```

- emacs

  - 入门
    - c-h t

- presentation展示用工具

  - [html presentation framework](https://www.sitepoint.com/5-free-html5-presentation-systems/)
    - revealjs
    - deck
    - slides
    - impress.js
  - ide / editor presentation tool
    - vscode-reveal
    - [remark](https://github.com/gnab/remark)