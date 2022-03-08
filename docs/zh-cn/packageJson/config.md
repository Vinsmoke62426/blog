### peerDependencies 并行依赖
vue3 的ui组件 element-plus 中这样写到
```json
{
    "peerDependencies": {
        "vue": "^3.2.0"
    }
}
```
[官方文档写的很详细](http://javascript.ruanyifeng.com/nodejs/packagejson.html)
`peerDependencies` 字段，就是用来供插件指定其所需要的主工具的版本。

```json
{
  "name": "chai-as-promised",
  "peerDependencies": {
    "chai": "1.x"
  }
}
```
上面代码指定，安装 `chai-as-promised` 模块时，主程序 `chai` 必须一起安装，而且 `chai` 的版本必须是 `1.x` 。如果你的项目指定的依赖是 `chai` 的 2.0 版本，就会报错。