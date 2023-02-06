# 封装组件相关内容
## js 模块化
JS模块化，主要存在以下几类。

### CommonJS

诞生于Node社区，只能在服务端使用

特点：一个文件为一个模块，通过module.exports方式暴露出模块接口，通过require方式引用

由于在node环境中引用，相当于引用本地文件，属于同步执行方式

CommonJS代码示例：
```js
var EventEmitter = require('events').EventEmitter;

var mixin = reuqire('merge-descriptors');

var Route = require('./router');

exports = module.exports = createApplication; // 对外暴露该方法
```
 

### AMD
Async Module Definition;

使用define定义模块

使用require加载模块

AMD规范代表 RequireJS

特点：依赖前置，提前执行

AMD代码示例：
```js
define(

　　'alpha', // 模块名, 模块名称可省略

　　['require', 'exports', 'beta'], // 依赖

　　// 模块输出

　　function (require, export, beta) {

　　　　exports.verb =  function() {

　　　　　　return beta.verb();

　　　　　　// 或者

　　　　　　return require('beta').verb();

　　}

);
```
### CMD
Common Module Definition

特点为： 一个文件为一个模块， 尽可能的懒执行

定义方式： 使用define来定义一个模块

使用方式：通过require来加载一个模块

CMD规范代表：SeaJS

CMD代码示例：
```js
define(function(require, exports, module) {

　　var $ = require('jquery);

　　var Spining = require('./spinning');

　　使用exports.doSomething = ...

　　或module.exports = ...

});
```
### UMD
Universal Module Definition

是一个通用解决方案

UMD实质为做了三件事

　　1.判断是否支持AMD

　　2.判断是否支持CommonJS

　　3.如果都没有，则使用全局变量

UMD代码示例：
```js
(function (root, factory) {
  if  (typeof define === 'function' && define.amd) {
    // 如果满足AMD规范，则使用AMD规范　
    define([], factory);
  } else if (typeof exports === 'object') {
    // 判断是否满足CommonJS
    module.exports = factory();
  } else {
    // Global (browser) root is window
    root.returnExports = factory();　
  }
}(this, function() {
  return {};
}))
```
### ES6 module
EcmaScript Module

一个文件一个模块

通过export暴露，import 引入

代码示例：
```js
import theDefault, {named1, named2} from 'src/mylib';

export const Test = ‘test’

export default Named = 'Named';
```
ps: webpack 支持三种类型： AMD， ES Modules（推荐）， CommonJs