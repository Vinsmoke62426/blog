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