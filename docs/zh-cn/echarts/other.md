## 自定义工具栏
```js
toolbox: {
  feature: {
    dataZoom: { show: false, yAxisIndex: "none" }, // 数据区域缩放
    restore: { show: false }, // 重置
    saveAsImage: { show: false }, // 导出图片
    myFull: {
      // 全屏
      show: !this.dialog,
      title: "放大",
      // echarts引入外部图片或图标有多种方式，详见官网
      // https://echarts.apache.org/v4/zh/option.html#legend.data.name
      icon: "path://M184 789.088L389.309 583.78a8 8 0 0 1 11.313 0l39.598 39.598a8 8 0 0 1 0 11.313L234.912 840H384c8.837 0 16 7.163 16 16v48a8 8 0 0 1-8 8H144c-17.673 0-32-14.327-32-32V632a8 8 0 0 1 8-8h48c8.837 0 16 7.163 16 16v149.088z m656-554.422L634.569 440.098a8 8 0 0 1-11.314 0L583.657 400.5a8 8 0 0 1 0-11.314L788.843 184H640c-8.837 0-16-7.163-16-16v-48a8 8 0 0 1 8-8h248c17.673 0 32 14.327 32 32v248a8 8 0 0 1-8 8h-48c-8.837 0-16-7.163-16-16V234.666z",
      onclick: () => {
        
      }
    }
  }
}
```