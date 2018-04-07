# IDEA 技巧

[TOC]

## templates

有些时候,有些配置很繁琐,记忆会存在困难,比如mybatis配置,logback配置

以mybatis为例,只要引入相应的DTD即可实现自动补全

因此

```xml
<!DOCTYPE configuration
PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-config.dtd">

```

可创建xml,并保存为templates(tools->save templates)或保存为(key: mybatis-dtd)live templates,前者可以在创建文件时指定为此模板内容的文件,后者可以在文件创建后键入mybatis-dtd自动补全