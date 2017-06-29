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