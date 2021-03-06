### Promise.all([])
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

### 用Prommise将同步变异步
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

### blob

英文 Binary large Object 二进制大对象

mdn 上的解释是 Blob 对象表示`不可变的类似文件对象的原始数据`

在 axios 中设置 responseType: 'blob' ，字面意思，设置响应类型为 blob，

如果不设置，返回视频流的时候的数据就是乱码，用播放器打开无法观看

但是在做 svg 的下载时，返回的数据是正常的，而 axios 中加不加 responseType: 'blob' 的区别在于

加的话返回的是 blob 对象， 不加的话，返回的就是 实际的内容 `不可变的类似文件对象的原始数据`

blob 的 三个方法 [https://developer.mozilla.org/zh-CN/docs/Web/API/Blob](https://developer.mozilla.org/zh-CN/docs/Web/API/Blob)

其中 Blob.text() 返回一个 promise 且包含 blob 所有内容的 UTF-8 格式的 USVString。

用 Response 对象读取 blob 中的内容：

var text = await (new Response(blob)).text() 

### axios timeout

timeout 没有默认时间，默认是 0

### windows查看端口并杀死进程

搜索端口，并通过 id 杀死
```
netstat -aon|findstr "8105"
taskkill /f /pid 18832
```

### 上网的四个条件

- ip IP地址

- 子网掩码 解析获取对应局域网中的具体 主机

- dns 解析ip 为域名

- 网关 解析ip地址 获取 mac地址

### 网卡

一个机子有两个网卡，一个是自己自带的，一个是插上去的无限网卡，

一个机子还可以有多个网卡，类似于服务器

