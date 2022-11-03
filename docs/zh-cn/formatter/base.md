# 格式化代码
## Prettier 介绍
### Prettier 的作用
[Prettier](https://prettier.io/)的作用是对代码进行格式化，并不关注代码质量潜在问题的检查，这点和 ESLint 是有很大区别的

Prettier 倾向于团队的代码风格的规范或统一，例如每行最大长度，单引号还是双引号，等号左右空格，使用tab还是空格等等

### 有了 ESLint 为啥还要用 Prettier
- ESLint 安装和配置比较麻烦，而且 lint 的速度并不快
  
- Prettier 并不只针对 JavaScript，它可以格式化各种流行语言，ts,vue,html.,css,less,scss,json,jsx等等

- Prettier 的配置选项没那么眼花缭乱，比 ESLint 少很多

### 如何在 vscode 中启用 Prettier
- 首先安装 Prettie r插件 `Prettier - Code formatter`
   
- 在用户级别的 `settings.json` 中加入如下配置
```json
// settings.json
{
  // 开启保存文件自动格式化代码
  "editor.formatOnSave": true,
  // 默认的代码格式化工具 
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  // 这项如果为 true ，必须要有对应的配置文件，否则代码保存不会进行格式化
  "prettier.requireConfig": true
}
```
第二个配置我们设置的是全局的代码格式化，如果相对特定的语言进行格式化，可以在下面单独配置
```json
// settings.json
{
  "[json]": {
    "editor.defaultFormatter": "HookyQR.beautify"
  },
  "[vue]": {
    // 这里不用 volar 来格式化，我认为用 prettier 就很好了
    "editor.defaultFormatter": "Vue.volar"
  }
}
```

重点是第三个配置，需要在项目根目录添加配置文件 `.prettierrc` [文件名具体是 .prettierrc .prettierrc.js还是.prettierrc.json 可以看官方说明](https://prettier.io/docs/en/configuration.html)
```js
// .prettierrc.js
// https://prettier.io/docs/en/configuration.html
// 以下为最常用的配置
module.exports = {
  // 行尾逗号,默认none,可选 none|es5|all
  // es5 包括es5中的数组、对象
  // all 包括函数对象等所有可选
  trailingComma: "none",
  // tab缩进大小,默认为2
  tabWidth: 2,
  // 使用分号, 默认true
  semi: false,
  // 使用单引号, 默认false(在jsx中配置无效, 默认都是双引号)
  singleQuote: false,
  // 对象中的空格 默认true
  // true: { foo: bar }
  // false: {foo: bar}
  bracketSpacing: true,
  // JSX标签闭合位置 默认false
  // false: <div
  //          className=""
  //          style={{}}
  //       >
  // true: <div
  //          className=""
  //          style={{}} >
  jsxBracketSameLine: false,
  // 箭头函数参数括号 默认avoid 可选 avoid| always
  // avoid 能省略括号的时候就省略 例如x => x
  // always 总是有括号
  arrowParens: "always"
}
```
 

## ESLint 介绍