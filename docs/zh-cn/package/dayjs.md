# day.js

[官网](https://day.js.org/docs/zh-CN/display/display)
## 使用
```
cnpm install dayjs -S
```
```js
// 引入
import dayjs from 'dayjs'

// 默认获取当前时间，但是不是时间戳格式的
dayjs()

// 获取毫秒级时间戳
dayjs().valueOf()

// 获取秒级时间戳
dayjs().unix()
```
