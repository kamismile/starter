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

