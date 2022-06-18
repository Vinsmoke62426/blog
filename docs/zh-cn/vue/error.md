## 打包后发布出错
### 'loading chunk XX failed'
router 的 `mode` 有两种类型， 默认的 `hash` 和 `history` 模式

之前在做 web 转为 移动端 app 的时候，报错
```
loading chunk XX failed
```
将 mode 改为 默认的 `hash` 模式即可解决

### 'Unexpected token '<''
打包发布到远程，控制台报错
```
Unexpected token '<'
```
原因：实际上就是没有获取到文件资源，网上说的解决办法是把 assets 中的文件移到 public/static 中，不解决根本问题。
解决办法：配置文件 vueconfig.js 中的 publicPath 写错了，要根据环境变量改变
```js
publicPath: IS_PROD ? './' : '/',
```
疑问：之前发布打包都没有遇到这种问题，根本不需要配置 publicPath,不知道为什么有时候就一定要手动去配置

## tsconfig.json 文件报错
在给 vue2.x 项目加入 ts 时，tsconfig.json 文件头部报错
```
在配置文件tsconfig.json中找不到任何输入。指定的 "include" 路径为"["**/*"]"，"exclude" 路径为"[]"
```
原因：创建tsconfig.json配置文件时，vscode会自动检测当前项目当中是否有ts文件，若没有则报错，提示用户需要创建一个ts文件后，再去使用typescript。

解决方法：随便加一个空的 .ts 文件 或者改一个 .ts 文件。(sb报错)

## 项目中切换不同角色进行登录，进去显示的路由还是上一个角色的
之前以为是动态路由写的有问题，

但是如果我退出登录，在登录页面刷新一下之后，再登另一个角色的话

又不会出现这种情况

所以`排除动态路由的问题`

原因：罪魁祸首
```js
// AppMain.vue
<router-view :key="key" class="router-class"/>
```
由于我统一任何角色的第一个路由在地址栏为 `'/'`,然后重定向它实际的路由

这会导致不同角色的`router-view`的`key`相同，都是`'/'`,存在缓存

解决方案：

1. 通过抛出重置路由实例的方法，在退出登录的时候调用

这种方式比较万金油，专门用来处理重置路由实例的场景
```js
// 创建路由
const createRouter = () => 
  new Router({
    mode: "history",
    base: baseURL,
    routes: constantRouterMap,
    scrollBehavior: () => ({ y: 0 })
  });

const router = createRouter()

// 抛出 重置路由实例的方法
export function resetRouter() {
  const newRouter = createRouter()
  router.matcher = newRouter.matcher // reset router
}
```

2. 不统一地址栏，直接跳转原始路由地址

这种方式也可以

## 修改 vueconfig.js 的静态资源前缀导致资源报 304
本身是没有静态资源304的情况的

改了静态资源的前缀后出现了，然后还原更改后还有同样的问题

最后发现，这种涉及到 nodejs 的很多问题，都要开启禁用缓存才行

`都是缓存的锅`

但是最后还留有一个问题，这些 304 资源 虽然是 200 的状态码，但是都没有请求到资源，不只是是什么原因