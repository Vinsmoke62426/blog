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
qs.parse(string) // '{a: '1'}'
```

> [!TIP]
> 造成以上结果的原因是：默认情况下，axios将JavaScript对象序列化为JSON也就是 `application/json;charset=UTF-8`
>
> 要以`application/x-www-form-urlencoded`格式发送数据
> 
> 最简单的方式就是使用 `qs.stringify()`了
>
> 这点[在官网上写的比较详细了](http://www.axios-js.com/zh-cn/docs/#%E4%BD%BF%E7%94%A8-application-x-www-form-urlencoded-format)

## 接收二进制数据流并下载
用 axios 的话只需修改请求头参数 `responseType` 为 `blob`，然后手动创建 a 链接下载
```js
// 基本模板，
const res = await ApiGetConfigs(params, "blob")
// 获取文件后缀 这里可以根据文件名后缀，写键值对来做动态文件类型的下载
// 这里附上blob类型大全 https://blog.csdn.net/yin_you_yu/article/details/116261304
// let suffix = response.headers['content-disposition'].search(/[^\.]\w*$/)
let blob = new Blob([res], { type: "application/zip" })
// 创建下载的链接
let downloadElement = document.createElement('a');
let href = window.URL.createObjectURL(blob);
downloadElement.href = href;
// 下载后文件名
downloadElement.download = fileName;
document.body.appendChild(downloadElement);
// 点击下载
downloadElement.click();
// 下载完成移除元素
document.body.removeChild(downloadElement);
// 释放掉blob对象
window.URL.revokeObjectURL(href);
```
### 关于二进制文件流下载的一个补充
`实际上我们对于二进制流的下载，正常情况下直接使用 a 链接就可以直接下载的`

但是我在做 axios 封装的时候，基本所有的接口都会走我写好的 axios 拦截器

`这种情况我们在页面上拿到的接口就不是纯粹的 url 了，不能直接 a 链接下载了`

我们自己写的接口很少会遇到这种情况

之前在做注册中心的时候，调用阿里不同的人写的接口遇到了一个接口既是获取列表的又是下载列表的场景

我们才会用到上面写的方式，修改请求头参数 `responseType` 为 `blob`，然后手动创建 a 链接下载


## 常见问题以及报错
### mockjs 会导致 axios 请求失败
mockjs 和 axios 同时存在的情况下，mockjs 会修改 axios 的 `responseType`

导致在上传文件和下载文件的时候 axios 直接报错，走响应拦截的错误拦截器