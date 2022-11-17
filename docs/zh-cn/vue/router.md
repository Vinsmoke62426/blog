# 路由
## router-view 和 component:is
功能相同,适用不同业务场景


## 路由跳转传参
vue路由跳转的三种方式及区别
[先贴地址，后续再写](https://blog.csdn.net/pig_pig32/article/details/119464990)
### 路由地址跳转
```js
this.$router.replace({
  path: "/configs/historyRollback",
  query: { row: JSON.stringify(row) }
})
```

参数如果是对象，最好用 JSON 发送和接收

及用 `JSON.stringify()` 发送数据，用 `JSON.parse()` 接收数据