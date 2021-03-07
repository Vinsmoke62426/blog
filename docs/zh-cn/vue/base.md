## Vue实例
### vue.use() 和 new vue() 的联系
```
vue.use() 是让你能在.vue文件中使用 this 访问对应的插件，如 this.$router, this.$store
(打印 this 的时候里面是没有 $router 和 $store的)

但是在main.js文件中，一般会有下面的代码
```
```js
new Vue({
  router,
  store,
  render: (h) => h(App)
}).$mount('#app')
```
```
new Vue() 中的 router 和 store 是将这两个插件挂载到 vue 实例中。

将你在 router.js 文件和 store.js 文件中定制化的 router 和 store 挂载到 vue 实例中。
(打印 this 的时候里面有 $router 和 $store 但是是没有经过修改的)

同理 new VueRouter() new Vuex.Store() 是一个道理，定制化你的路由和vuex。

所以你的前期工作，那些定制化的操作就相当于，你给电脑店一个配置表，告诉老板你想买什么什么配置的电脑，但是你还没买，

当你使用 new Vue() 去挂载你引入的插件时，才相当于你买了这台电脑😎。

tips: vue.use() 必须在 new vue() 之前，use() 的时候括号里面插件的代码会运行一次。

```