## 将扁平数组转化为树状结构
```js
const getTree = arr => {
    // 定义空的set，set类似于数组，但是具有唯一性
    const set = new Set()
    const res = []
    arr.forEach((v, i) => {
        // 基于key进行分类
        const key = v.comType
        const item = {
            label: v.comOwn,
            value: v.id
        }
        if(!set.has(key)){
            res.push({
                label: key,
                children: [item]
            })
            set.add(key)
        }else{
            // 获取到对应的key然后再push
            const t = res.find(v => v.label === key)
            t.children.push(item)
        }
    })
    return res
}
```
## Array.from 将维数组转换为真数组
```js
类似于 Array.from(document.querySelectorAll('.item.d2')) 

之后就可以使用正常的数组方法了，不用forEach了

Array.from还可以接受第二个参数，作用类似于数组的map方法，用来对每个元素进行处理，将处理后的值放入返回的数组

Array.from(arrayLike, x => x * x);

// 等同于
Array.from(arrayLike).map(x => x * x);

Array.from([1, 2, 3], (x) => x * x)
// [1, 4, 9]
```
## 判断是数字
```js
const isNum = num =>  num !== '' && !isNaN(num)
```

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

但是这个在火狐和chrome上有区别，chrome无法点出img的属性，导致获取不到想要要的宽高？？？

可能和 两个浏览器的 window.onload 和 img.onload 的加载顺序有关
```

```js
console.time()

console.timeEnd()

可以用来打印代码执行时间
```
