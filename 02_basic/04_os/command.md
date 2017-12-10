# 查看linux 上mysql连接数

```shell
sudo netstat -antlup | grep :3306 | wc -l
```

