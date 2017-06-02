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

