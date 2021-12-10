## 不要再用 vue2 的思维写 vue3 了
vue3 的`compostion api`，官方推荐的是更加细分`业务逻辑`的文件

这其实和 vue2，有很大的不同

vue2 的简单之处在于，你可以将所有的变量放在 data 中，计算属性放在 computed 中，方法放在 methods 中，直观易懂

但是，vue3 中，你如果也这么做，你的代码会像意大利面一样长

官方推荐的基于`业务逻辑`将，代码细分，`而不是优先考虑代码的复用性`

[推荐看看这篇文章讲的](https://juejin.cn/post/6946387745208172558)

## vue3和vue2响应式原理不同

Vue2.x实现双向数据绑定原理，是通过es5的 Object.defineProperty，根据具体的key去读取和修改。其中的setter方法来实现数据劫持的，getter实现数据的修改。但是必须先知道想要拦截和修改的key是什么，所以vue2对于新增的属性无能为力，比如无法监听属性的添加和删除、数组索引和长度的变更，vue2的解决方法是使用Vue.set(object, propertyName, value) 等方法向嵌套对象添加响应式。


Vue3.x使用了ES2015的更快的原生proxy 替代 Object.defineProperty。Proxy可以理解成，在对象之前架设一层“拦截”，外界对该对象的访问，都必须先通过这层拦截，因此提供了一种机制，可以对外界的访问进行过滤和改写。Proxy可以直接监听对象而非属性，并返回一个新对象，具有更好的响应式支持

## 组合式api
[先看官方](https://v3.cn.vuejs.org/guide/composition-api-introduction.html#%E4%BB%80%E4%B9%88%E6%98%AF%E7%BB%84%E5%90%88%E5%BC%8F-api)

## 组件选项
[先看官方](https://v3.cn.vuejs.org/guide/component-custom-events.html#%E4%BA%8B%E4%BB%B6%E5%90%8D)

## 单文件组件`<script setup>`
[先看官方](https://v3.cn.vuejs.org/api/sfc-script-setup.html#%E5%9F%BA%E6%9C%AC%E8%AF%AD%E6%B3%95)

这个挺有意思的