## 路由及权限拦截
```js
import Vue from 'vue'
import Router from 'vue-router'
import store from "@/store";
import nprogress from "nprogress";
import "nprogress/nprogress.css";
nprogress.configure({ showSpinner: false });
Vue.use(Router)

// 解决重复跳转的警告
// const originalPush = Router.prototype.push
// Router.prototype.push = function push(location) {
//   return originalPush.call(this, location).catch((err) => err)
// }

import Login from '../views/login/index.vue'

// 静态路由
export const constantRouterMap = [
  {
    path: '/login',
    name: 'login',
    component: Login,
    hidden: true,
    meta: {
      title: '登录'
    }
  },
  {
    path: "/404",
    component: resolve => require(["../views/notFound/index.vue"], resolve),
    name: "无法访问",
    hidden: true
  },
]

const router = new Router({
  mode: 'history',
  routes: constantRouterMap,
  scrollBehavior: () => ({ y: 0 })
})
// 清空路由
// export function resetRouter() {
//   const newRouter = createRouter
//   router.matcher = newRouter.matcher // reset router
// }

// 权限控制
const whiteList = ["/login", "/404"];
router.beforeEach(async(to, from, next) => {
  //开启导航条
  nprogress.start();
  const hasToken =  localStorage.getItem('accessToken')
  const addRouters = store.state.permission.addRouters
  if(hasToken){
    if(to.path == "/login"){
      next("/")
    }
    if (!whiteList.includes(to.path) && addRouters.length === 0) {
      await store.dispatch("generateRoutes", {'router':router})
      next({ ...to, replace: true })
    }else{      
      next()
    }  
  }else{
    //* 在免登录白名单，直接进入
    whiteList.some(item => item === to.path) ?  next() :  next({path: whiteList[0]})
      // 在hash模式下 改变手动改变hash 重定向回来 不会触发afterEach 暂时hack方案 ps：history模式下无问题，可删除该行！
    nprogress.done(); 
  }
});
router.afterEach(() => {
  nprogress.done()
});

export default router



```