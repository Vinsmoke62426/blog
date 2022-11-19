# axios
## 关于传参
### 不做任何处理的 data 参数
![Image text](./../../images/request/axios1.png)

### 加了 headers: { "Content-Type": "application/x-www-form-urlencoded" } 的 data 参数
![Image text](./../../images/request/axios2.png)

将整个 JSON 作为表单的第一项的`键名`来传递，一定要注意到最后面的`冒号`

### 用了 qs.stringify(data) 处理后的 data 参数
![Image text](./../../images/request/axios3.png)

qs.stringify() 处理相当于将 对象格式转化为了表单形式，去掉了外部的括号，然后会被 axios 自动识别为

`"Content-Type": "application/x-www-form-urlencoded"` 形式
```js
const obj = {a: 1}
qs.stringify(obj) // 'a=1'

const string = 'a=1'
qs.parse(obj) // '{a: '1'}'
```

> [!TIP]
> 造成以上情况的原因是：默认情况下，axios将JavaScript对象序列化为JSON也就是 `application/json;charset=UTF-8`
>
> 要以`application/x-www-form-urlencoded`格式发送数据
> 
> 最简单的方式就是使用 `qs.stringify()`了
>
> 这点[在官网上写的比较详细了](http://www.axios-js.com/zh-cn/docs/#%E4%BD%BF%E7%94%A8-application-x-www-form-urlencoded-format)




