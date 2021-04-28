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