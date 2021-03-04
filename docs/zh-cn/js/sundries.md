## toLocaleString() 多国语言价格数字格式化
```js
(10000000).toLocaleString('en')
=》  10,000,000
```

## ~~ 运算符
[将变量转化为Number类型](https://blog.csdn.net/weixin_37710888/article/details/82587296)

```js
二维数组想浅拷贝，必须要对内层对数组进行解构，对外层解构无效，还是会改变原数组
const [...newArray] = oldArray
```