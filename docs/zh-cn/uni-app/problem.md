# uni-app 初识踩坑
## 全平台以及编译器
### ui库的选择
- 为了不必要的麻烦，使用 uni-app 自己研发的 uni-ui 吧。本来想用 vant-weapp 的，但是这个组件库没有表单验证


- 调用接口也用自带的 uni-request，再手动封装成 axios 的形式，axios 内部使用了 formData 会报错，uni-app想使用 axios 必须要额外进行配置，比较麻烦。uni-request 没有 params 参数，这点要注意

- 涉及到的所有 window.xxx 的调用，最好使用 uni-app 自带的 api，如 window.sessionStorage 用 uni.getStorageSync 代替

- css 单位 rpx 替代 px

## 微信小程序
> [!Danger]
> 天坑，遇到了非常多的问题

- 

## app

## h5

