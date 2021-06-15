### 将扁平数组转化为树状结构
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
### Array.from 将维数组转换为真数组
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

### Array.of 用于生成一个数组对象，主要是用来弥补Array()的不足
```js
let res = Array.of(1, 2, 3)

console.log(res) // [1, 2, 3]
```

### 判断是数字
```js
const isNum = num =>  num !== '' && !isNaN(num)
```

### toLocaleString() 多国语言价格数字格式化
```js
(10000000).toLocaleString('en')
=》  10,000,000
```

### ~~ 运算符
[将变量转化为Number类型](https://blog.csdn.net/weixin_37710888/article/details/82587296) 同时可以用于向下取整

### 二维数组浅拷贝
```js
二维数组想浅拷贝，必须要对内层对数组进行解构，对外层解构无效，还是会改变原数组
const [...newArray] = oldArray
```

### 使你的网址在1分钟内提高1%
```js
<script src="//instant.page/5.1.0" type="module" integrity="sha384-by67kQnR+pyfy8yWP4kPO12fHKRLHZPfEsiSXR8u2IKcTdxD805MGUXBzVPnkLHw"></script>
```
引用自[https://instant.page/](https://instant.page/)

### 注册全局监听事件用 window.addEventListener()

### vue 全局注册事件竟然需要 this.$nextTick(() => {})
### 获取一张图片的实际尺寸
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

```
为什么在post请求前面会多一个options请求

因为这是处理跨域的一种方式，先去试探一下请求，如果成功（既运行跨域），则正常post请求
```

```js
可选链操作符 ?. 也可以处理函数

xxxx?.() => {}

当时困扰了很久
```

### package-lock.json
```
package-lock.json 文件是用来锁 package.json 里面依赖的版本的

在 npm5 以前没有这个文件的时候，你手动修改了 package.json 里面的依赖，然后在 npm install， 相对的依赖的版本就会直接被修改

有了 package-lock.json 之后，必须要 npm install xxx@x.x.x （@后面对应的版本号）去更新依赖，然后package-lock.json也能随之更新
```
> [!TIP] 团队开发时，当拉取代码，发现 package.json 和 package-lock.json 两个文件都更新了某一个依赖的版本时
> 
> 你必须 npm install 一下才能覆盖掉 node_modules 里面之前版本的这个依赖，这很重要

### 关于时间格式
```js
new Date( ).toLoacleDateString

=> 2021/4/23

快速获取时间戳

+ new Date()  或者  Date.now()

```
### 检测当前页面是否被隐藏
```js
document.addEventListener("visibilitychange", function() {
   console.log(document.hidden);
})

当切换页面时显示true， false就是打开状态, 一般在工作用主要用到用户在页面停留了多长时间
```

### addEventListener 和 removeEventListener

之前在实际项目中，涉及到在 vue 中进行事件的监听和销毁，

最开始写的时候，没有涉及到事件的销毁，所以都是使用箭头函数，在箭头函数内部进行判断

但是如果要清除监听，因为是匿名函数，所以直接就歇菜了
```js
document.addEventListener('DOMMouseScroll', this.cbScrollFirefox)

cbScrollFirefox(e) {
    console.log(e)
}

document.removeEventListener('DOMMouseScroll', this.cbScrollFirefox)
```
注意`上面的命名函数默认传参自己的 dom 本身` 这很重要。

### arguments


### JSX @click.native

在项目中写 table 表格中的 下拉菜单 dropdown 时，因为用的是 jsx 的缘故，出现了问题

本身 dropdown 中的下拉选项的点击事件是用它自带的 command 的，这个在 jsx 中不好操作，根本触发不了事件

这个时候需要 用到 `.native`，为自定义组件添加原生事件(阻止事件冒泡)，在 jsx 中要这样写：

```js
<el nativeOnClick={this.nativeOnClickHandler} />
```

### let that = this

老生常谈的问题，`存储 this 指向`，之前在常规函数中见的比较多

也可以遍历 classList，做样式上的处理，也挺方便的