# 飞流
## 踩坑
###

## 常见问题
### 部署服务器失败但是日志没有任何东西
一般是断电后导致飞流与飞流部署在我们内网服务器上的代理之间的关联断开

可以在`飞流→企业管理→主机组管理→新建主机组→自有主机`中查看主机状态

解决办法：重启对应服务器上的代理就行
```
/home/staragent/bin/staragentctl restart
```