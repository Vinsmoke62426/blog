# dom 处理
## 将domString转为dom同时添加进dom中
```js
// 创建一个外壳用来添加和转化  domstring 为 dom，后续要删掉
const wrap = document.createElement("div")
wrap.innerHTML = '<div>123</div>'
// 弄清楚 innerHTML 和 outerHTML 的区别。这步相当于去掉外壳
wrap.outerHTML = wrap.innerHTML
```

## appendChild 和 insertBefore
`parantDom.appendChild(newDom)`

将 newDom 插入到 parantDom 的 childNode 的最后面

`parantDom.insertBefore(newDom, nextDom)`

将 newDom 插入到 parantDom 的 childNode 下面的 nextDom 前面

## 用ResizeObserver替代resize
[参考](https://blog.csdn.net/qq_32615575/article/details/122607120)

## 原生js添加dom和 vue用jsx添加dom的区别
```js
const div = document.createElement("div")
    const tabHeader = document.querySelector(".el-tabs__header")
    tabHeader.insertBefore(div, tabHeader.firstChild)
    // const btn = document.createElement("button")
    // div.appendChild(btn)
    const btn = {
      render: (h) => {
        return (
          <el-button icon="el-icon-back" size="small" class="back-btn">
            返回
          </el-button>
        )
      }
    }
    // 全局注册组件
    const infoLabel = new Vue(btn)
    const infoLabelComponent = infoLabel.$mount()
    // 注册后拿到原始dom
    const dom = infoLabelComponent.$el
    div.appendChild(dom)
    // btn.innerHTML = "返回"
    // btn.classList.add("el-button")
    // btn.classList.add("back-btn")
    // btn.classList.add("el-button--small")
```
上面是一个比较典型的案例，包含了原生js添加dom

两种常见方法 appendChild 和 insertBefore

以及用 vue 的 render 去代替 原生的 createElement 去创建 dom