# 路由
## router-view 和 component:is
功能相同,适用不同业务场景


## 路由跳转传参
路由传参分为两种形式 `query` 和 `params`
### 两者区别
- params 传递过去的参数不会显示在地址栏中，query 则会

- params 传递的参数，刷新一下页面就没了，query 则不会
  
- query 可以配合 name 和 path 两种形式跳转形式

- params 只能配合 name 使用，如果提供了 path，params 会失效

- 两者接收参数分别是 `this.$route.query` 和 `this.$route.params`

params 传参通过动态路由的形式，使得刷新后参数还存在

但是总体来说，不推荐 `params` 形式传参，用 `query` 更省心
### 关于传递数组或对象类型的参数
参数如果是对象，最好用 JSON 发送和接收

及用 `JSON.stringify()` 发送数据，用 `JSON.parse()` 接收数据
```js
this.$router.replace({
  path: "/configs/historyRollback",
  query: { row: JSON.stringify(row) }
})
```

