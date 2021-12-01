### 打包后发布出错

#### 'loading chunk XX failed'

router 的 `mode` 有两种类型， 默认的 `hash` 和 `history` 模式

之前在做 web 转为 移动端 app 的时候，报错
```
loading chunk XX failed
```
将 mode 改为 默认的 `hash` 模式即可解决

#### 'Unexpected token '<''

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