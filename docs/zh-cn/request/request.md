## Promise.all([])
```js
数组里面循环请求，直接 return 这个请求，不需要 await

将异步变同步，后续再做其他处理

例子：

getSearchfile(params).then(res => {
    if(res.code === 0) {
        // 用Promise.all处理 等所有的taskName请求到再做操作
        Promise.all(res.data.records.map((v) => {
            return getTaskTypeid(v.taskType)
        })).then(resAll => {
            resAll.forEach(v => {
                res.data.records.map(b => {
                    if(b.taskType == v.data.id) {
                        b.taskType = v.data.taskName
                    }
                })
            })
            this.tableData = this.tableData.concat(res.data.records)
        }).finally(() => {
            this.loading = false
        })
    }
})
```

## 用Prommise将同步变异步
```js
function createAsyncTask(syncTask){
    return Promise.resolve(syncTask).then(syncTask => syncTask())
}

createAsyncTask(() =>{
    console.log('我变成了异步任务')  //第二输出
    return 1+1
}).then(res => {
    console.log(res);  //最后输出
})

console.log('我是同步任务')   //最先输出
```

## windows查看端口并杀死进程

搜索端口，并通过 id 杀死
```
netstat -aon|findstr "8105"
taskkill /f /pid 18832
```

## 掘金快速抽奖
```js
var ajax = new XMLHttpRequest();
ajax.open('post','https://api.juejin.cn/growth_api/v1/lottery/draw',true);
ajax.withCredentials = true;
ajax.setRequestHeader('content-type','application/json; charset=utf-8');
ajax.send(JSON.stringify({}));
ajax.onreadystatechange = function (){
    if(ajax.readyState==4&&ajax.status==200){
        var c = ajax.responseText;
        console.log(c)
    }
}
```

## blob
英文 Binary large Object 二进制大对象

mdn 上的解释是 Blob 对象表示`不可变的类似文件对象的原始数据`

blob 的 三个方法 [https://developer.mozilla.org/zh-CN/docs/Web/API/Blob](https://developer.mozilla.org/zh-CN/docs/Web/API/Blob)

其中 Blob.text() 返回一个 promise 且包含 blob 所有内容的 UTF-8 格式的 USVString。

用 Response 对象读取 blob 中的内容：

var text = await (new Response(blob)).text() 

### file类型转base64
以 el-upload 上传的图片为例
```js
const reader = new FileReader();
reader.readAsDataURL(file.raw);
reader.onload = function(e){
  console.log(e)
}
```
另外附上[base64，File，arraybuffer互相转化操作](https://blog.csdn.net/Dalin0929/article/details/127265602)


### url转dataUrl，dataUrl转blob
```
前端想将 <img :src=""/> 的 src 的图片传给后端，就必须要转成 File 格式传

因为 src 只是一个 url 地址，不是文件， blob 则是一种特殊的File类型
```


## 关于接口参数以及url参数处理

[官方自带的方法，最好也是最简单的 `URLSearchParams`](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams)

## 关于浏览器缓存
分为`强缓存`和`协商缓存`，目前组织架构图片接口用的就是`强缓存`

[这篇文章讲的很详细](https://blog.csdn.net/weixin_43788494/article/details/108093729)但是协商缓存我没有仔细看

## 将svg文件流转化为svg标签
[借鉴了这个人的部分操作](https://juejin.cn/post/6844903769885376520)，他写的更加详细一些

创建一个原生js请求
```js
const xhr = new XMLHttpRequest()
    xhr.addEventListener("load", () => {
      const resXML = xhr.responseXML
      // 这步是关键，这个svgdom就是svg具体的标签内容
      let svgDom = resXML.documentElement.cloneNode(true)
      // 找到并删除掉指定的dom
      svgDom.querySelectorAll("image")[0].remove()
      // 添加到指定dom下
      document.getElementById("fullScreen").appendChild(svgDom)
    })
    // 第二个参数接收文件地址
    xhr.open("GET", filePreview(this.subData.mongData.svgMongoId))
    xhr.send()

    // ==================================
    // 到上面就结束了，后面是一些额外的操作

    // 将 dom 转换成字符串：通过 XMLSerializer 将 XML 转换成 String 类型
    const svgStr = new XMLSerializer().serializeToString(svgDom);

    // 选择了 Blob 类型进行转换，先用 new Blob([svgStr]) 转换成 Blob 类型，再通过 URL.createObjectURL(blob) 方法将字符串转换成 Blob 类型 URL 传入
    const blob = new Blob([svgStr], {type: 'image/svg+xml'});
    const blobStr = URL.createObjectURL(blob);
    const template = `<img src="${blobStr}">`;

```