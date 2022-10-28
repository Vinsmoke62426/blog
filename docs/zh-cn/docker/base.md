# 连接并启动 docker 中的容器
## 连接
### 用 ssh 连接服务器
```
ssh -p 端口号 账号@ip地址
```
### 查看 docker 是否启动
```
docker ps
```

### 启动 docker
```
systemctl restart docker
```

### 模糊查询所有 portainer 的容器 
```
docker ps -a | grep portainer
```

### 启动指定容器
```
docker start dqgs-portainer
```