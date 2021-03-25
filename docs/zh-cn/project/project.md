## 河北项目左侧菜单有时有有时没有
```
用链接请求后端图标库的时候，忘记在地址前面加斜杠“/” 导致左侧菜单有时候显示图标，有时候不显示，

原因是：不加斜杠是相对路径，根本没有走代理，能显示图标是因为碰巧在首页，

如果在其他页面刷新，再请求到的图标地址前面就会是当前路径了，固然无法显示图标直接404，加了斜杠才是绝对路径，才是走的代理，粗心。
```
## el-menu-item选中时有残影
```
在给 el-menu-item 做选中时的圆角时，出现了圆角动画有延迟的情况（由矩形变成圆角，用户体验很差）

原因是 border-radius 写的位置错了，不应该写在 is-active里面，应该直接写 el-menu-item 下面
```
## vue-cli3.x 的cdn加速
```
之前遇到了，项目中没有引入 echarts 但是项目中可以使用的情况，实际上是因为在 vue.config.js 中配置了 cdn 加速的原因

相当于你用的是远程的路径，可以不用安装依赖，但是在离线部署的时候不行
```
## echarts 的 legend 鼠标一放上去就变白的问题
```
是因为 echarts 的 legend 无法识别 rgba颜色，必须要用 hex

包括 borderColor 也是必须要用 hex 颜色，否则就显示空白的 border
```
## elementui的表单样式，在自动填充之后变成白色的底色的问题
```
网上查到的方法都是
```
```css
input:-webkit-autofill{
    -webkit-text-fill-color: #74BFE6 !important;
    -webkit-box-shadow: 0 0 0px 1000px transparent  inset !important;
    background-color:transparent;
    background-image: none;
    transition: background-color 50000s ease-in-out 0s; //背景色透明  生效时长  过渡效果  启用时延迟的时间
}
```
```
因为我们用的是elementui的样式，所以前面应该改成
```
```css
.el-input__inner:-webkit-autofill{
    -webkit-text-fill-color: #74BFE6 !important;
    -webkit-box-shadow: 0 0 0px 1000px transparent  inset !important;
    background-color:transparent;
    background-image: none;
    transition: background-color 50000s ease-in-out 0s; //背景色透明  生效时长  过渡效果  启用时延迟的时间
}
```
```html
火狐的自动填充密码的样式，无法用 -moz- 的形式来改，但是显示起来相对正常。

如果真想修改，可以写两行 html 的形式去覆盖修改 

<el-input v-model="loginForm.username" type="text" style="position: fixed; bottom: -9999px; display: none;"></el-input>

有效果，但是不推荐
```
## url转dataUrl，dataUrl转blob
```
前端想将 <img :src=""/> 的 src 的图片传给后端，就必须要转成 File 格式传

因为 src 只是一个 url 地址，不是文件， blob 则是一种特殊的File类型
```

## classList是list格式，不能用数组的方法， 而 js 没有 contains 这个方法（类似与 includes ），为啥也可以用