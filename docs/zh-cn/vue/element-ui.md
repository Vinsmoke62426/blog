## 循环表单 el-form
之前在做循环表单，从接口中获取数据回显在 el-input 中的时候，input无法修改的问题

最后是用 this.$set 显示的，问题是解决了，但是为什么却不清楚

## el-table 的 header 插槽
业务需求 el-table 表头加上图标

两种实现方式
### 组件内统一用插槽做

直接在 dom 中写 和用 组件 render 都行
```vue
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
