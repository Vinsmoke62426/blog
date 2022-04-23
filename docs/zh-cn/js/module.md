# ES6 Module
[先看官方文档,阮一峰老师写的很清楚](https://es6.ruanyifeng.com/#docs/module)
## ES6模块和 CommonJs模块的区别
```js
// CommonJS模块
let { stat, exists, readfile } = require('fs');

// 等同于
let _fs = require('fs');
let stat = _fs.stat;
let exists = _fs.exists;
let readfile = _fs.readfile;
```
```js
// ES6模块
import { stat, exists, readFile } from 'fs';
```
`静态加载`以及`按需引入`

CommonJs 因为`只能运行时加载`,无法静态优化,效率较低

## CommonJs 模块中使用 window document 报错
可以使用,浏览器的控制台也可以打印,但是如果启动项目的话,nodejs直接报错

原因在于CommonJs是运行时加载,要先提前判断 `window`, `document` 存在才行
```js
if(typeof window !== "undefined"){
    //do something
}
```