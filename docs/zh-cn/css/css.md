### vue中样式不起作用
原因： 
- 组件内部使用组件，添加了 scoped 属性
- 动态引入 html，也添加了 scoped 属性

vue 中的 scoped 属性的效果主要是通过 PostCss 实现的
```css
/* 转义前 */
<style lang="scss" scoped>
    .example{
        color: red;
    }
</style>
<template>
    <div class="example">scoped测试案例</div>
</template>

/* 转义后 */
.example[data-v-469af010] {
  color: red;
}
<template>
    <div class="example" data-v-469af010>scoped测试案例</div>
</template>
```

由此可知，添加 scoped 属性的组件，为了达到不污染全局，做了如下处理：

- 给 HTML 的 DOM 节点加一个不重复属性`data-v-469af010`标志唯一性。
- 在添加 scoped 属性的组件的每个样式选择器后添加一个等同与`不重复属性`相同的字段，实现类似于`作用域`的作用，不影响全局。
- 如果组件内部还有组件，`只会给最外层的组件里的标签加上唯一属性字段`，不影响组件内部引用的组件。


解决方法：
    样式穿透，从官方文档了解到，我们所谓的穿透，官方叫做深度选择器。
    怎么用的呢 ?就是在我们想穿透的选择器前边添加 >>> 或者 /deep/ 或者 ::v-deep。
    官方还说 >>> 可能存在问题 vue 2.x 无效，建议用后两者，就选择 /deep/ 。  

```css
/* 没加穿透前 */
<style scoped lang="scss">
    .wrap .example {
        color: red;
    }
</style>

.wrap .example[data-v-469af010] {
  color: red;
}

/* 加了穿透后 */
<style scoped lang="scss">
    .wrap /deep/ .example {
        color: red;
    }
</style>

.wrap[data-v-469af010] .example {
  color: red;
}

改变了唯一性标志的位置
```
动态加入的的 html 没有进过转义，没有对应的 data 属性，所以也不会起作用

### 控制浏览器缩放
```css
/* 可直接缩放页面，有意思 */
body {
    transform: scale(0.8) !important;
}
```
