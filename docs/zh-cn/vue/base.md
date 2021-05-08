## Vue实例
### vue.use() 和 new vue() 的联系

`vue.use()` 是让你能在 .vue 文件中使用 this 访问对应的插件，如 `this.$router`, `this.$store`, 否则打印`this`的时候里面是没有`$router`和`$store`的

但是在main.js文件中，一般会有下面的代码

```js
new Vue({
  router,
  store,
  render: (h) => h(App)
}).$mount('#app')
```
`new Vue()` 中的 `router` 和 `store` 是将这两个插件挂载到 vue 实例中。

将你在 `router.js` 文件和 `store.js` 文件中定制化的 `router` 和 `store` 挂载到 `vue` 实例中(打印 this 的时候里面有 $router 和 $store 但是是没有经过修改的)。

同理`new VueRouter()`, `new Vuex.Store()`是一个道理，定制化你的路由和`vuex`。

所以你的前期工作，那些定制化的操作就相当于，你给电脑店一个配置表，告诉老板你想买什么什么配置的电脑，但是你还没买，

当你使用`new Vue()`去挂载你引入的插件时，才相当于你买了这台电脑😎。

tips: `vue.use()`必须在`new vue()`之前，`use()`的时候括号里面插件的代码会运行一次。


### Vue项目的文件执行顺序
```js
  new Vue({
    el: "#app",  //告诉该实例要挂载的地方
    store,
    router,
    components: { App },  //实例中注册了一个局部组件App, 这个局部组件是当前目录下的App.vue
    template: "<App/>"  //而初始模板是什么呢？模板就是组件App.vue中的template中的内容。（template会替代原来的的挂载点处的内容）
  });
```
在项目运行中，`main.js`作为项目的入口文件，

运行中，找到其实例需要挂载的位置，即`index.html`中，

刚开始，`index.html`的挂载点处的内容会被显示，但是随后就被实例中的组件中的模板中的内容所取代，

所以我们会看到有那么一瞬间会显示出`index.html`中正文的内容。

### 函数式组件

常规写法：
```vue
<template>
  <div class="cell">
    <div v-if="value" class="on"></div>
    <section v-else class="off"></section>
  </div>
</template>

<script>
export default {
  props: ['value'],
}
</script>
```
函数式组件写法：
```vue
<template functional>
  <div class="cell">
    <div v-if="props.value" class="on"></div>
    <section v-else class="off"></section>
  </div>
</template>
```
特性：

- 无状态
- 无法实例化
- 内部没有任何生命周期处理函数
- `轻量，渲染性能高，适合只依赖于外部数据传递而变化的组件(展示组件，无逻辑和状态修改)`
- 在 template 标签里标明 functional
- 只接受 props 值
- 不需要 script 标签
