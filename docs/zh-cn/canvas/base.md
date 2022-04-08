# canvas
## 开发中遇到的问题
### 画图时相对路径图片无法显示
想用canvas绘制一个图片一般都是用下面的方法
```js
const image = new Image() // 新建一个img标签（还没嵌入DOM节点)
// 这里拿到图片的地址
image.src = 'static/images/structure/equip.png';
image.onload = () => {
    // 一定要等图片加载完成后再渲染
    ctx.drawImage(image, 0, 0);
}
```
但是却遇到了如果图片时相对路径的话无法显示的问题

类似于 `'@/asset/....xxx.png'`, `'../../...xx.png'`

所以最后只好将图片放在了static中才解决😒

> [!TIP]
> 最后找到解决方案 src 用 require 引入地址，可以用相对路径

原因：这里设计到 webpack 的对于图片的编码