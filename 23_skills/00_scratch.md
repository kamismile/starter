[TOC]

# vscode 创建java + spring boot + maven项目

- 创建目录结构(在vscode terminal中操作)

  - ```shell
    mkdir -p projectName/src/{main,test}/{java,resources}
    ```

- 使用vscode project manager保存成工程

- 生成pom.xml

  - ```shell
    curl https://start.spring.io/pom.xml -d dependences=web,devtools -d type=maven-build -o pom.xml
    ```

- 编写application主类和controller测试类，`mvn spring-boot:run` 或 vscode debug栏中运行，结束

# 在vscode中创建maven工程

> terminal中输入maven archetype:generate -Dfilter=quickstart 进行交互式操作, 如果网络不畅，可以在user.home目录下的.m2目录下的settings.xml设置阿里的maven mirro地址
>
> 或者使用`mvn archetype:generate -DgroupId=XXX -DartifactId=XXX -DinteractiveMode=false` batch mode的方式创建基于quickstart的项目

