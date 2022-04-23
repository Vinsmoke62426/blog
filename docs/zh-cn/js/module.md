# ES6 Module
[先看官方文档,阮一峰老师写的很清楚](https://es6.ruanyifeng.com/#docs/module)
## ES6模块和 CommonJs的区别
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