[TOC]



# 01 读取远程binlog

```shell
mysqlbinlog -R --host=192.168.56.102 --port=3306 --user=root --password=root foo.000001 # --stop-never
```

# 02 maven 高级打包assembly

## 01 过滤文件或文件属性

```xml
<assembly xmlns="http://maven.apache.org/ASSEMBLY/2.0.0"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/ASSEMBLY/2.0.0 http://maven.apache.org/xsd/assembly-2.0.0.xsd">
  <id>distribution</id>
  <formats>
    <format>jar</format>
  </formats>
  <files>
    <file>
      <source>README.txt</source>
      <outputDirectory>/</outputDirectory>
      <filtered>true</filtered>
    </file>
    <file>
      <source>LICENSE.txt</source>
      <outputDirectory>/</outputDirectory>
    </file>
    <file>
      <source>NOTICE.txt</source>
      <outputDirectory>/</outputDirectory>
      <filtered>true</filtered>
    </file>
  </files>
</assembly>

<project>
  [...]
  <build>
    [...]
    <plugins>
      [...]
      <plugin>
        <artifactId>maven-assembly-plugin</artifactId>
        <version>3.0.0</version>
        <configuration>
          <filters>
            <filter>src/assembly/filter.properties</filter>
          </filters>
          <descriptors>
            <descriptor>src/assembly/distribution.xml</descriptor>
          </descriptors>
        </configuration>
      </plugin>
   [...]
</project>
```

## 02 过滤文件

```xml

<assembly xmlns="http://maven.apache.org/ASSEMBLY/2.0.0"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/ASSEMBLY/2.0.0 http://maven.apache.org/xsd/assembly-2.0.0.xsd">
  <id>distribution</id>
  <formats>
    <format>jar</format>
  </formats>
  <fileSets>
    <fileSet>
      <directory>${basedir}</directory>
      <includes>
        <include>*.txt</include>
      </includes>
      <excludes>
        <exclude>README.txt</exclude>
        <exclude>NOTICE.txt</exclude>
      </excludes>
    </fileSet>
  </fileSets>
  <files>
    <file>
      <source>README.txt</source>
      <outputDirectory>/</outputDirectory>
      <filtered>true</filtered>
    </file>
    <file>
      <source>NOTICE.txt</source>
      <outputDirectory>/</outputDirectory>
      <filtered>true</filtered>
    </file>
  </files>
</assembly>
```

```shell
mvn clean assembly:single
```

## 03 添加依赖包

```xml
  <formats>
    <format>zip</format>
  </formats>
  <dependencySets>
    <dependencySet>
      <outputDirectory>/lib</outputDirectory>
      <includes>
        <include>application:logging</include>
        <include>application:core</include>
        <include>application:utils</include>
        <include>application:appserverB</include>
      </includes>
    </dependencySet>
  </dependencySets>
```

## 04 提示[WARNING] Cannot include project artifact

```shell
# 不要用mvn clean assembly:simple
# 用mvn clean package
修改pom
assembly插件绑定single与package
```

# 03 module.exports 和 exports

每个node.js文件执行时，都会自动创建module对象，同时module对象还会有一个exports属性，初始值为{}, exports是引用module.exports的值，module.exports被改变的时候，exports不会被改变，而模块导出的时候，真正导出的是module.exports,而不是exports

https://cnodejs.org/topic/52308842101e574521c16e06

exports只是module.exports的别名，也就是一个指针，指向module.exports对象所在的地址，如果你直接将一个对象赋给exports,那么改变的是exports指向的值，而module.exports对象没有改变

# 04 javascript this

http://www.ruanyifeng.com/blog/2010/04/using_this_keyword_in_javascript.html

根本原则: 指向调用函数的那个对象

http://jeffjade.com/2015/08/03/2015-08-03-javascript-this/

























