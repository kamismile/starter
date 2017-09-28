[TOC]

# 1. zookeeper集群搭建

```shell
tar zxvf zookeeper-3.4.10.tar.gz
cd zookeeper-3.4.10/conf
cp zoo_cfg.cfg zoo.cfg
vim zoo.cnf 
# dataDir=/alidata1/admin/zookeeper-3.4.10/snapshot
# server.0=
cd .. && mkdir snapshot && echo 0 > snapshot/myid
server.0=server0:2888:3888
server.1=server1:2888:3888
server.2=server2:2888:3888


```

