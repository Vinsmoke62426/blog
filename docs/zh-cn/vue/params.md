## 环境变量配置

### vue-cli 2.x

在`config`文件夹中的`prod.env.js`和`dev.env.js`文件中，写好变量

在`main.js`里面加上`Vue.prototype.baseURL = process.env.BASE_API`

这个`BASE_API`是你配的变量名称，因为是常量，所以用全大写

### vue-cli 3.x-

![](../../images/params-1.png)

### 多环境打包配置
[这个是vuecli2的操作方式](https://www.cnblogs.com/dianzan/p/13151950.html)
详细的，可以查看天津项目的配置，就是按照这个来的

vuecli3 的多环境打包 很简单，关键点是 后面的 --mode xxx, 然后再创建对应的 .env.xxx 的文件
```js
"build:stage": "vue-cli-service build --mode staging",
```
