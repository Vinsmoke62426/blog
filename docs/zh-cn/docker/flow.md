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
> [!TIP]
> 如果服务器没有网络，会报错提示找不到这个文件

### 关于是否需要重启center容器
为什么发布到euraka本地的时候，需要重启center？

本地的center是放在容器中

重新发布项目后，对应的ip地址会变，所以要重启center

而epnc18，测试环境下的 nginx 是做成一个软件的，不需要重启

epnc18 重启center命令
```
sudo /home/epnc/app/nginx/sbin/nginx -c /home/epnc/app/nginx/conf/nginx-center.conf
```