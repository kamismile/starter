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

