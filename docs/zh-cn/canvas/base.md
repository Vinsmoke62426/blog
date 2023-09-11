# canvas
## 基本事例
### 在图上画图 
```html
<div class="img-box">
    <canvas class="canvas" id="canvas"></canvas>
    <img src="@/assets/images/structure/1-2-origin.png" @load="loadSuccess" alt="" class="main-image"/>
</div>

<script>
/* 
注意一定是要在图片资源加载完成后，根据图片大小渲染同样大小的canvas
而不能用宽高100%来定义canvas的大小，这很重要。 
否则画出来的所有东西都是模糊的（虽然看着canvas是和底图一样大，但是却被等比例放大了）
*/ 
export default {
    loadSuccess() {
        // 拿canvas实例
        let canvas = document.getElementById("canvas");
        // 画笔
        let ctx = canvas.getContext("2d");
        const elImage = document.querySelector('.main-image')

        //! 获取图片的大小，设置画布的大小与图片大小相同,这步非常重要
        canvas.width = elImage.clientWidth
        canvas.height = elImage.clientHeight

        // 测试手绘图形
        ctx.lineWidth = 2;
        ctx.strokeStyle = 'red'
        ctx.beginPath();
        ctx.moveTo(70, 70);
        ctx.lineTo(10, 10);
        ctx.lineTo(15, 30);
        ctx.lineTo(40, 90);
        ctx.closePath();
        ctx.stroke();
        
        // 测试渲染文字
        ctx.fillStyle = '#fff'
        ctx.font = '13px serif'
        ctx.fillText("101", 100, 80);
        
        // 测试渲染图片
        const image = new Image() // 新建一个img标签（还没嵌入DOM节点)
        image.src = require('@/assets/images/structure/equip.png');
        image.onload = () => {
            ctx.drawImage(image, 100, 85);
        }
    }
}
</script>
```

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

原因：这涉及到 webpack 的对于图片的编码