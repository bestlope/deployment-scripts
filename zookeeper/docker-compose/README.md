## 启动zookeeper容器示例
```
docker run --name zookeeper --restart always -d zookeeper --restart always
```

## 使用zk 命令客户端连接zk
```
docker run -it --rm --link zookeeper:zookeeper zookeeper zkCli.sh -server zookeeper
```

## 使用docker-compose启动zookeeper集群
```
# 启动
docker-compose up -d

# 停止
docker-compose stop

# 拆卸
docker-compose down

```

##  参考网站
https://blog.csdn.net/myNameIssls/article/details/81561975
