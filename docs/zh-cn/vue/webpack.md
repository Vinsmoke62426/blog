#### sass 配置全局混入
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
如果是2.x 找到 build 文件夹中的 utils.js 文件，修改 sass 的配置
```js
// build/utils.js 中 修改scss的配置如下：
return {
    css: generateLoaders(),
    postcss: generateLoaders(),
    less: generateLoaders('less'),
    sass: generateLoaders('sass', { indentedSyntax: true }),
    scss: generateLoaders('sass').concat(
        {
            loader: 'sass-resources-loader',
            options: {
                resources: [
                    path.resolve(__dirname, '../src/styles/variables.scss'),
                    path.resolve(__dirname, '../src/styles/mixins.scss'),
                    path.resolve(__dirname, '../src/styles/primaryChange.scss')
                ]
            }
        }
    ),
    stylus: generateLoaders('stylus'),
    styl: generateLoaders('stylus')
}
// 详细 webpack 配置查看 https://vue-loader.vuejs.org/en/configurations/extract-css.html
```

3. 然后记得重启项目，就可以直接使用 你在 common.scss 文件中定义的全局变量了 如 `$hb-dark-theme-deep`, `$eisp-menu-bg`
   