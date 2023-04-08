## 循环表单 el-form
之前在做循环表单，从接口中获取数据回显在 el-input 中的时候，input无法修改的问题

最后是用 this.$set 显示的，问题是解决了，但是为什么却不清楚

## el-table 的 header 插槽
业务需求 el-table 表头加上图标

两种实现方式
### 组件内统一用插槽做

直接在 dom 中写 和用 组件 render 都行
```js
<!-- 递归组件内 -->
<template>
    <el-table-column>   
        <!-- 表头加上图标的插槽 -->
        <template v-if="columnItem.icon" slot="header">
            <span>{{columnItem.title}}
                <el-tooltip class="item" effect="dark" content="测试测试" placement="top-start">
                    <i class="el-icon-question" style="color:#606266;" />
                </el-tooltip>
            </span>
            <!-- <Test></Test> -->
        </template>
    </el-table-column>
</template>

<script>
export default {
    name: 'ColumnItem',
    components: {
        Test: {
            render() {
                return (
                    <span>用户名
                        <el-tooltip class="item" effect="dark" content="测试测试" placement="top-start">
                        <i class="el-icon-question" style="color:#606266;" />
                        </el-tooltip>
                    </span>
                )
            } 
        }
    }
}
</script>

```
### 组件外 render-header 重写表头做
```vue
<!-- 递归组件内 -->
<template>
    <el-table-column
        :render-header="columnItem.icon"
    >   
    </el-table-column>
</template>
```

```js
// 外部传入时使用
column: [
  {
    title: "列名",
    dataIndex: "列index",
    icon: () => {
      return (
        <span>用户状态
            <el-tooltip class="item" effect="dark" placement="top-start" >
              <div slot="content">
                1.内容;<br/> 2.内容;<br/> 3.内容
              </div>
                <i class="el-icon-question" style="color:#6893bf;margin: 0px 3px;font-size: 18px; cursor: pointer;" />
            </el-tooltip>
        </span>
      )
    },
  }
]
```

## 父组件传含有函数的复杂对象给子组件
父组件
```html
<template>
  <div id="parents">
    <Child :testObj="test"></Child>
  </div>
</template>

<script>
import Test from './test.vue'
export default {
  components: { Child },
  data() {
    return {
      test: {
      return {
          value: '',
          event: {
            type: 'change',
            func: this.testFunc
          }
        }
      }
    }
  },
  computed: {
    
  },
  methods: {
    testFunc() {
      console.log('内部调用了')
    }
  }
};
</script>
```
子组件
```html
<template>
    <el-input
        :value="testObj.value" 
        @input="$emit('update:testObj', {...testObj, value: $event})"
        @[testObj.event.type]="innerFunc"  
    ></el-input>
</template>

<script>
export default {
    props: {
        testObj: {
            type: Object
        }
    },
    methods: {
        innerFunc() {
            this.testObj.event.func()
        }
    }

}
</script>
```

## jsx el-form
常规写法会报错
```
Invalid handler for event "input": got undefined
```
重点在于 el-form 的 model 要特殊写法

用 props 传进去就好了
```js
<el-form
  props={{
    model: this.elProps
  }}>
  ...
</el-form>
```
下面是比较经典也是比较复杂的 jsx 处理 el-from 同时 解决 bug 的完整事例
```js
<div class="file_image_box">
    <img src={this.getIconByFileNameSuffix(row)}/>
    <div class="content">
        <el-form 
            props={{ model: rowCopy}} 
            rules={rules}
            nativeOnsubmit={(event) => {
                // jsx 阻止表单默认的 submit 事件触发
                // 来解决 当 el-form 中只有一个 el-input 时，enter 会直接刷新页面的 bug  
                event.preventDefault() 
            }}
        >
            <el-form-item label="" prop="name">
                <el-input 
                    size="mini" 
                    value={rowCopy.name} 
                    onInput={(value) => { rowCopy.name = value }}
                    nativeOnkeyup={(event) => {
                        // 用判断 代替 .enter 事件修饰符
                        if(event.keyCode === 13) {
                            this.confirm(rowCopy, row)
                        } 
                    }} 
                    placeholder=""
                    style={`width: ${document.querySelector('.file_image_box').clientWidth - 80}px`}
                >
                    <el-button 
                        size="mini" 
                        slot="suffix" 
                        class="input-inner-btn-cancel el-icon-circle-close" 
                        type="text"
                        onClick={() => { this.cancel() }}
                    ></el-button>
                    <el-button 
                        size="mini" 
                        slot="suffix" 
                        class="input-inner-btn-confirm el-icon-circle-check" 
                        type="text"
                        onClick={() => { this.confirm(rowCopy, row) }}
                    ></el-button>
                </el-input>
            </el-form-item>
        </el-form>
    </div>
</div>
```

## el-upload 的几种使用情况
[自动上传与修改默认的上传方式](https://www.jianshu.com/p/b141b968723c)

## el-menu 的左侧菜单栏收起导致的一系列问题

element-ui 的左侧菜单栏如果使用它自带的收起和展开功能，需要配上如下样式才能解决动画不流畅的问题

```css
// 官网上 解决菜单收缩不顺畅的问题
.el-menu-vertical:not(.el-menu--collapse) {
  border: 0;
  width: 200px;
  min-height: 400px;
}
```
但是如果你在左侧菜单栏的上面或下面加上你自定义的元素

你模仿 el-menu 的动画效果，在收起菜单的时候，隐藏文字，只显示图片，在展开菜单的时候你会发现

你展开显示的文字会在你点击的一瞬间也就是菜单还没完全展开的时候就显示了

文体会撑起左侧菜单的宽度，导致整个左侧菜单的动画不流畅

最简单的解决办法就是为这个 `<span>` 标签`设置宽度为 0px`
