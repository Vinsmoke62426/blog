## 环境变量配置

### vue-cli 2.x

在`config`文件夹中的`prod.env.js`和`dev.env.js`文件中，写好变量

在`main.js`里面加上`Vue.prototype.baseURL = process.env.BASE_API`

这个`BASE_API`是你配的变量名称，因为是常量，所以用全大写

### vue-cli 3.x-

![](../../images/params-1.png)
