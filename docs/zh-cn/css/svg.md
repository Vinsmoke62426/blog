# svg

## 常见问题
### svg 中的 xmlns 属性是干什么用的
```js
<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">...</svg>
```

我们在代码中使用 svg ，如果 不加上 xmlns 会导致 svg 图片裂开

原因是：在 XML 文档中,属性和元素属于命名空间，这是为了防止来自不同技术的元素发生冲突，xmlns定义了默认命名空间

例如SVG <a>元素和HTML <a>元素可以区分

### 从接口地址获取的 svg 图片怎么以 dom 的形式显示在代码中

```js
<embed
  :src="mongSvg"
  class="embed"
  type="image/svg+xml"
/>
```

## 手动创建一个 svg 中的 dom
[createElementNS](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/createElementNS)

```js
// 该方式创建的svg元素不显示，一定要用 createElementNS 且加上命名空间
// let rect = svgDoc.createElement("rect"); 

let rect = document.createElementNS( "http://www.w3.org/2000/svg", "rect" );
rect.setAttribute("id", "myrect");
rect.setAttribute("x", 0);
rect.setAttribute("y", 0);
rect.setAttribute("width", 100);
rect.setAttribute("height", 100);
rect.setAttribute("style", "stroke:black;stroke-size:1;stroke-dasharray:1;fill:none;");
// 最终再添加到指定的dom下
svg.appendChild(rect)
```

### svg 渲染在页面上的各种方法以及差异
[这篇文章写的很详细](https://segmentfault.com/a/1190000010942431)

整体可以分为两大类

一类是 img标签，div背景图片，picture标签

特点：可以用文件地址直接渲染，但是svg内部的image标签都无法显示

第二类是 iframe标签， embed标签， object标签

特点：可以显示image标签，但是同时也会请求image标签上的href地址，这不是我们想要的

解决办法在[这里](../request/request.md)

也可使用第三方依赖`svg-injector`,我没有详细看

### 修改img 标签中 的svg颜色
```html
<div class="svg-img">
  <img src="~@/assets/images/logo.svg" >
</div>
```
```less
.svg-img {
  width: 40px;
  height: 40px;
  text-indent: -40px;
  overflow: hidden;
  img {
    filter: drop-shadow(40px 0px #0159AA);
  }
}
```