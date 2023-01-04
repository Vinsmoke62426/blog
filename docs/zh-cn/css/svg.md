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