# 00 危机意识

你的对手在强大，甚至你瞧不上的某些人也在偷偷地努力超越, 当其它人在努力时你要比其它人更努力

# 01 gradle

```shell
# gradle init --type java-library
mkdir foo-proj
cd foo-proj
touch build.gradle
# vim build.gradle 如下
# vim中保存并执行bash
# :!mkdir -p src/{main,test}/{java,resources}
# :!gradle build
```



```groovy
buildscript {
  repositories {
    mavenLocal()
    jcenter()
  }
  dependencies {  
    classpath("org.springframework.boot:spring-boot-gradle-plugin:1.5.6.RELEASE")
  }
}

apply plugin: 'java'
apply plugin: 'idea'
apply plugin: 'eclipse'
apply plugin: 'org.springframework.boot'

jar {
    baseName = 'foo-proj'
  	version = '0.1.0'
}

repositories {
    mavenLocal()
  	jcenter()
}

sourceCompatibility = 1.8
targetCompatibility = 1.8

dependencies {  
  compile("org.springframework.boot:spring-boot-starter-jdbc")
  runtime("com.h2database:h2")
}
```

# 02 guava eventbus / localcache 

TODO

# 03 maven plugin开发

[guide-java-plugin-development](https://maven.apache.org/guides/plugin/guide-java-plugin-development.html)

1. 新建maven工程

   1. 常规方式创建

   2. 或使用archetype原型进行创建

      ```shell
      maven archetype:generate \
      	-DgroupId=sample.plugin \
      	-DartifactId=hello-maven-plugin \
      	-DarchetypeGroupId=org.apache.maven.archetypes \
      	-DarchetypeArtifactId=maven-archetype-plugin
      ```

      ​

2. 修改pom

   1. packaging: maven-plugin
   2. 依赖apache maven 依赖项maven-plugin-api, maven-plugin-annotations(provided)
   3. clean install or deploy用于安装依赖项

3. 编写插件

   ```java
   package sample.plugin;
    
   import org.apache.maven.plugin.AbstractMojo;
   import org.apache.maven.plugin.MojoExecutionException;
   import org.apache.maven.plugins.annotations.Mojo;

   @Mojo(name = "sayhi")
   public class GreetingMojo extends AbstractMojo {
     public void execute() throws MojoExecutionException {
       getLog().info("hello world");
     }
   }
   ```

4. 编译打包并安装到本地或deploy到远程repository

5. 使用

   1. 新建maven工程

   2. 引入插件

      ```xml
      ...
      <build>
      	<plugins>
        		<plugin>
            		<groupId>sample.plugin</groupId>
                	<artifactId>hello-maven-plugin</artifactId>
                	<version>1.0-SNAPSHOT</version>
            	</plugin>
        	</plugins>
      </build>
      ...
      ```

      或使用下面方式绑定到lifecycle中

   3. ```xml
        <build>
          <plugins>
            <plugin>
              <groupId>sample.plugin</groupId>
              <artifactId>hello-maven-plugin</artifactId>
              <version>1.0-SNAPSHOT</version>
              <executions>
                <execution>
                  <phase>compile</phase>
                  <goals>
                    <goal>sayhi</goal>
                  </goals>
                </execution>
              </executions>
            </plugin>
          </plugins>
        </build>
      ```

   4. ​

   5. 使用

      ```shell
      # mvn groupId:artifactId:version:goal
      mvn sample.plugin:hello-maven-plugin:1.0-SNAPSHOT:sayhi
      ```

   6. 简便操作

      1. 如果命名符合**\${prefix}-maven-plugin**或**maven-${prefix}-plugin**的话，可通过*`hello:sayhi`*短命令格式

      2. 也可以在\${user.home}\.m2/settings.xml文件中添加如下内容

         ```xml
         <pluginGroups>
         	<pluginGroup>sample.plugin</pluginGroup>
         </pluginGroups>
         ```

   7. 参数parameters

      参数提供了调整对插件操作的口子

      提供了从pom中获取参数值的方法

      1. 在mojo中声明parameters

         ```java
         // defaultValue可以使用表达式，如${project.version}
         @Parameter(property = "sayhi.greeting", defaultValue = "hello world")
         private String greeting;
         ```

      2. 在项目中配置参数parameters

         ```xml
         <plugin>
           <groupId>sample.plugin</groupId>
           <artifactId>hello-maven-plugin</artifactId>
           <version>1.0-SNAPSHOT</version>
           <configuration>
         	<!-- 元素名称greeting对应参数名称，元素值对应参数值 -->    
             <greeting>Welcome</greeting>
           </configuration>
         </plugin>
         ```

         ​

      ​