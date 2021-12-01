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

tips: 因为递归组件需要 name，所以和函数式组件不能同时存在

> [!Danger] 这里不对，函数式组件是可以写 name 的, 比如下面这样
```js
<script>
export default {
  name: 'MenuItem',
  functional: true,
  props: {
    icon: {
      type: String,
      default: ''
    },
    title: {
      type: String,
      default: ''
    }
  },
  render(h, context) {
    // console.log(h, context)
    const { icon, title } = context.props
    const vnodes = []

    if (icon) {
      if (icon.includes('el-icon')) {
        vnodes.push(<i class={[icon, 'sub-el-icon']} />)
      } else {
        vnodes.push(<svg-icon icon-class={icon}/>)
      }
    }

    if (title) {
      vnodes.push(<span slot='title'>{(title)}</span>)
    }
    return vnodes
  }
}
</script>
```

### 什么时候要用 Vue.set()

Vue2.x 实现双向数据绑定原理，是通过 es5 的 Object.defineProperty，手动去定义一个 getter 和 setter，根据具体的key去读取和修改。

其中的 setter 方法来实现数据劫持的，getter 实现数据的修改。

但是必须先知道想要拦截和修改的 key 是什么，所以 vue2 对于新增的属性无能为力，

比如无法监听属性的添加和删除、数组索引和长度的变更，

vue2 的解决方法是使用 Vue.set(object, propertyName, value) 等方法向嵌套对象添加响应式。

### `$ref` 和 `$el`  

`this.$ref['标签上的ref名称']` 可以获取到一个 dom 元素。

如果 `this.$ref['子组件上的ref名称']` 得到的就是子组件的实例，你可以在后面接着使用子组件的方法。

`但是这样做你无法获得子组件的 dom`，这个时候就要用到 `$el`。

`this.$ref['子组件上的ref名称'].$el.offsetTop` (用来获取子组件的 offsetTop)

### computed 和 methods

computed 和 methods 里面函数的区别在于:

computed 里面的函数调用的时候不需要带括号，带有缓存的能力，初始的时候会执行一次，只要其监听的数据不改变，则不改变，执行时直接返回上一次的值。

methods 里面的函数，只要你调用，就执行。

vue 中的 computed 和 methods 都`混入了 vue 实例中`，所以使用箭头函数指向的不是当前组件，要用常规函数

### keepAlive 生命周期

正常生命周期：beforeRouteEnter --> created --> mounted --> updated --> destroyed

使用keepAlive后生命周期：

首次进入缓存页面：beforeRouteEnter --> created --> mounted --> activated --> deactivated

再次进入缓存页面：beforeRouteEnter --> activated --> deactivated

### vue修饰符

#### .sync

`当父组件传值进子组件，子组件想要改变这个值时，可以这么做`
```html
父组件里
<children :foo="bar" @update:foo="val => bar = val"></children>

子组件里
this.$emit('update:foo', newValue)
```
简写 
```html
父组件里
<children :foo.sync="bar"></children>

子组件里
this.$emit('update:foo', newValue)
```
#### .keyCode
```html
当我们这么写事件的时候，无论按什么按钮都会触发事件
<input type="text" @keyup="shout(4)">

那么想要限制成某个按键触发怎么办？这时候keyCode修饰符就派上用场了
<input type="text" @keyup.keyCode="shout(4)">
```
Vue提供的keyCode：

//普通键
- .enter 
- .tab
- .delete //(捕获“删除”和“退格”键)
- .space
- .esc
- .up
- .down
- .left
- .right

//系统修饰键
- .ctrl
- .alt
- .meta
- .shift

例如（具体的键码请看[键码对应表](https://zhidao.baidu.com/question/266291349.html)）
```html
按 ctrl 才会触发
<input type="text" @keyup.ctrl="shout(4)">

也可以鼠标事件+按键
<input type="text" @mousedown.ctrl.="shout(4)">

可以多按键触发 例如 ctrl + 67
<input type="text" @keyup.ctrl.67="shout(4)">
```

### 作用域插槽

插槽一般就作用域插槽用的多

作用域插槽实际运用场景: 提供的组件可以从子组件获取数据

```html
//子组件
<div class="box" v-for="item of list" :key="item.id">
  <div class="box-title">
    <p>
      <span>{{item.deNo}}</span>
      <!--我们为每个item准备了一个插槽，将item对象作为一个插槽的prop传入-->
        <slot class="genre" v-bind:item="item"></slot>
    </p>
    <p>{{item.deDate}}</p>
  </div>
</div>
//父组件
<div>
  <!--1.作用域插槽必须是template开头和结尾的内容-->
  <!--2.slot-scope="props"声明从子组件传递的数据都放在props里-->
  <template slot-scope="props">
    <!--告诉子组件模板的信息是以<span>标签的形式-->
    <span class="genre">{{props.item.state}}</span>
  </template>
</div>
```

### router mode
router 的 `mode` 有两种类型， 默认的 `hash` 和 `history` 模式

之前在做 web 转为 移动端 app 的时候，报错
```
loading chunk XX failed
```
将 mode 改为 默认的 `hash` 模式即可解决

坑啊😥

### el-dialog 组件的 sync 关闭

遇到 dialog 组件时，关闭弹框的时候会报，不能改变 props 中的内容，

解决方法是，用 :before-close 回调方法去关闭，不用 @close 等事件

### baseURL

vue 项目配置代理的目的，一版都是为了解决跨越问题，如果是 dev 环境的话，直接配置 proxy 就行

但是如果是 prod 环境，配置 axios 的 baseURL 就行，

之前遇到过 线上环境有两个源的时候，要配置动态 baseURL
```js
const service = axios.create({
  timeout: 5000,
  baseURL: baseURL, // api的base_url
});

service.interceptors.request.use(
  (config) => {
    if(config.url.includes('/pinqi')) {
      // 用判断去修改baseURL，之前用的是上面这种写法，说是不能直接修改 baseURL，会不起作用
      // 但是我试了一下下面这种直接修改的方式，上线之后也是成功的
      
      // config.url = baseURL2 + config.url
      config.baseURL = baseURL2 
    }
    return config
  },
  (err) => { }
);
```

### 嵌套命名视图
用嵌套命名视图做页面的布局是可行的：
```js
Viewer 是一个视图组件，里面可以放多个 <router-view></router-view>

<template>
    <div>
        <router-view></router-view> //这里放的是Layout组件
        <router-view name="owner" class="owner"></router-view> //这里放的是路由组件
    </div>
</template>

// 单个
{
  path: '/home',
  name: 'home',
  component: Viewer,
  meta: {
    title: '首页'
  },
  children: [
    {
      path: '/home',
      name: 'home',
      components: {
        default: Layout,
        owner: () => import('../views/home/index.vue') 
      },
      meta: { title: 'index' }
    }
  ]
}
// 嵌套
{
  path: '/nested',
  name: 'nested',
  component: Viewer,
  meta: {
    title: '嵌套'
  },
  children: [
    {
      path: '/nested/menu2',
      name: 'menu2',
      components: {
        default: Layout,
        owner: Menu2
      },
      meta: { title: 'menu2' }
    },
    {
      path: '/nested/menu1',
      name: 'menu1',
      components: {
        default: Layout,
        owner: Menu1
      },
      meta: { title: 'menu1' },
      children: [
        {
          path: '/nested/menu1/menu11',
          name: 'menu11',
          components: {
            default: Layout,
            owner: Menu1
          },
          meta: { title: 'menu11' }
        }
      ]
    }
  ]
}
```

### beforeRouteLeave(to, from, next)

第一次在组件内部使用 `beforeRouteLeave`, 这个本身是一个路由离开页面的钩子

但是在组件内部中直接使用时，是作为一个生命周期来使用的，然后用的时候出现了不生效的问题

原因是：这个生命周期只能在上级的父组件中使用，子组件使用无效，这个挺坑的