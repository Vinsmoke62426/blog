# 项目上遇到的一些注意事项
## 添加标签
[关于三种不同类型标签的展示效果](https://blog.csdn.net/bobo789456123/article/details/129464847)

大体分一下几步走
1. 创建 Mesh
2. 创建对应的 2d 或 3d dom
3. 将 dom 添加到 Mesh 中
4. 创建对应的 2d 或 3d 渲染器
5. 将渲染器中 dom 元素添加到整个画布中

```js
setCSS2DDOM({ position, dom }) {
  const { x, y, z } = position
  const earth = new Mesh()

  this.scene.add(earth)

  dom.style.pointerEvents = "auto"
  dom.style.cursor = "pointer"
  const earthLabel = new CSS2DObject(dom)
  earthLabel.position.set(x, y, z)
  earth.add(earthLabel)

  this.labelRenderer = new CSS2DRenderer()
  this.labelRenderer.setSize(
    this.canvas.offsetWidth,
    this.canvas.offsetHeight
  )
  this.labelRenderer.domElement.style.position = "absolute"
  this.labelRenderer.domElement.style.top = "0px"
  this.labelRenderer.domElement.style.pointerEvents = "none"
  this.canvas.parentElement.appendChild(this.labelRenderer.domElement)
}

// 连续渲染
animate2D() {
  if (this.labelRenderer !== null) {
    requestAnimationFrame(() => {
      this.animate2D()
    })
    // 渲染 CSS2DRenderer 图层
    this.labelRenderer.render(this.scene, this.camera)
  }
}

animate3D() {
  ...
}

animateOther() {
  ...
}

clear() {
  this.canvas = null
  this.scene = null
  this.camera = null

  this.renderer = null
  this.labelRenderer = null
  
  // 其他渲染器
  ...
}
```
两点注意事项：

- 创建好渲染器后，要配置对应渲染器的连续渲染，否则没有3d效果

创建多个渲染器以及对应的多个连续渲染，就可以做到在相同的`场景`和`视角`下，渲染不同的东西

- 离开 3d 页面，或切换 3d 渲染的时候一定要 clear(), 否则页面会变卡 

## 关于threejs的学习
找完整源码最好去[官方github中的demo](https://github.com/mrdoob/three.js/tree/master/examples)