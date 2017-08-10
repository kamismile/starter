[TOC]

# 01 maven重新下载依赖项

mvn clean compile -U 强制更新snapshot

有时候依赖项因为网络原因没有下载成功，项目运行不起来，很难排查，可以通过

mvn dependency:purge-local-repository重新下载项目依赖项

不用强记 mvn dependency:help