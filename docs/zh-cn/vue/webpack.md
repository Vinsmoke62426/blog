#### sass 配置
之前用 sass 做主题切换的时候，涉及到要混入 sass 变量，具体如何混入，vue-cli2.x 和 vue-cli3.x 还是有区别的

1. 无论是 2.x 还是 3.x，都需要提前安装三个依赖
```json
"dependencies": {
  "node-sass": "^4.11.1",
  "sass-loader": "^7.3.0",
  "sass-resources-loader": "^1.3.3",
}
```
一般你选择 sass 新建的项目，都会帮你安装好，没有的就自己装
2. 如果是3.x 直接在 vue.config.js 文件中加上
```js
module.exports = {
    css: {
        sourceMap: true,
        loaderOptions: {
            sass: {
                //公共的scss变量和混入(可加多个，用 ；号分隔)
                prependData: `@import "./src/style/base"; 
                            @import "./src/style/primaryChange";`,
            }
        }
    }
}
```