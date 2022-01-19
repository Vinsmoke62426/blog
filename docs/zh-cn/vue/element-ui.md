## 循环表单 el-form
之前在做循环表单，从接口中获取数据回显在 el-input 中的时候，input无法修改的问题

最后是用 this.$set 显示的，问题是解决了，但是为什么却不清楚

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
