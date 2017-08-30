[TOC]

# 01 maven重新下载依赖项

mvn clean compile -U 强制更新snapshot

有时候依赖项因为网络原因没有下载成功，项目运行不起来，很难排查，可以通过

mvn dependency:purge-local-repository重新下载项目依赖项

不用强记 mvn dependency:help

# 02  windows下git bash查询树

```shell
cmd //c tree
# cmd //c tree //F 显示文件
```

# 03 创建项目目录结构

```shell
mkdir -p src/{main,test}/{java,resources}
```

# 04 JIT && JMH

https://www.ibm.com/developerworks/library/j-benchmark1/index.html