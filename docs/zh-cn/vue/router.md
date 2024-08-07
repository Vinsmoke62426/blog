# 路由
## 路由跳转方式
两种 `push` 和 `replace`

路由跳转默认形式是 push ，它会追加历史记录，可以回退

replace 形式 是替换当前记录，无法回退
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

## 关于路由懒加载
什么是路由懒加载？

### 路由加载的两种方式
1. `components: () => import('@/views/home')` 需要访问对应的路由的时候才加载(`懒加载`)
2. `components: () => Home` 会在页面访问之前就全部提前加载完成

`绝大部分情况建议使用懒加载的形式`

## router-view 和 component:is
功能相同,适用不同业务场景

## 通过依赖动态注册路由
```js
// main.js

import aaa from "@epnc-t/personal-vue"
let route = {
  path: "/aaa"
}
// route.meta = { ...route.meta }
route.component = aaa
router.addRoute(route)
```
后面即可实现 `router.push('/aaa')`跳转到系统中没有的页面，一般比较适合单独新建标签页的页面