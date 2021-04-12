## toLocaleString() 多国语言价格数字格式化
```js
(10000000).toLocaleString('en')
=》  10,000,000
```

## ~~ 运算符
[将变量转化为Number类型](https://blog.csdn.net/weixin_37710888/article/details/82587296)

## 二维数组浅拷贝
```js
二维数组想浅拷贝，必须要对内层对数组进行解构，对外层解构无效，还是会改变原数组
const [...newArray] = oldArray
```

## 使你的网址在1分钟内提高1%
```js
<script src="//instant.page/5.1.0" type="module" integrity="sha384-by67kQnR+pyfy8yWP4kPO12fHKRLHZPfEsiSXR8u2IKcTdxD805MGUXBzVPnkLHw"></script>
```
引用自[https://instant.page/](https://instant.page/)

## 注册全局监听事件用 window.addEventListener()

## vue 全局注册事件竟然需要 this.$nextTick(() => {})
## 获取一张图片的实际尺寸
```js
用 dom 获取到的 clientWidth 和 clientHeight 都是字面意思，客户端尺寸。

想要获取一张图片的实际尺寸，要用 url 来获取

let img = new Image()

img.src = url //图片url

const width = img.width

const height = img.height

这样拿到的才是实际尺寸
```

```js
console.time()
console.timeEnd()
可以用来打印代码执行时间
```
