# 01 简易安装

1. 镜像安装
2. docker tag source:version target:version
3. docker run -p 9200:9200 -p 9300:9300 -d -it -h elasticsearch --name elasticsearch -e "ES_JAVA_OPTS=-Xms512m -Xmx512m" elasticsearch:5.5.0

