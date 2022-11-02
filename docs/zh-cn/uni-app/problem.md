# uni-app 初识踩坑
## 全平台以及编译器本身
### 开发工具选型
- 为了不必要的麻烦，使用 uni-app 自己研发的 uni-ui 吧。本来想用 vant-weapp 的，但是这个组件库没有表单验证

- 调用接口也用自带的 uni-request，再手动封装成 axios 的形式，axios 内部使用了 formData 会报错，uni-app想使用 axios 必须要额外进行配置，比较麻烦。uni-request 没有 params 参数，这点要注意

- css 单位我认为绝大部分情况没必要使用 rpx，px足够了 [这里对rpx的讲解比较详细](https://www.zhuige.com/index/news/detail/id/629.html)

### 常见问题
- 涉及到的所有 window.xxx 的调用，最好使用 uni-app 自带的 api，如 window.sessionStorage 用 uni.getStorageSync 代替

- npm install 和 cnpm install 下载下来的东西不一样，cnpm 要多很多。这会让 pages.json 中的 easycom 配置的自定义组件路径不对，最终导致报错没有找到组件

- 条件编译 如果写在 json 文件中， 记得一定要将前一个对象后面的`逗号`包含进去，否则会报错，json 文件的格式要求太严格了

## 微信小程序
> [!Danger]
> 天坑，遇到了非常多的问题
### 运行以及调试
- 首先一定要有 AppID 这个要去申请，填写在`manifest.json`中。如果是团队开发，记得拉取代码后要手动修改(或许需要一个公司的 AppID？)

- 运行到微信开发者工具 首先需要设置路径，定位到中文路径即可 `D:/Tencent/微信web开发者工具`，同时打开微信开发者工具的`设置→安全设置→服务端口`

- 如果调用了接口 务必在运行到微信开发者工具后 勾选 `详情→本地设置→不校验合法域名，web-wiew....`。 `在你修改了代码热更新到开发工具后，可能还要再次勾选😥`

- 没有找到微信开发者工具配置代理的方式，不能向 h5 一样配置代理。最终的做法是加了条件编译，当 `MP-WEIXIN` 时，手动为 url 拼接上原本需要代理的地址 

- 真机调试 似乎无法预览 代理后的域名，比如 `my.com, eureka`，最终是直接写的ip地址

- 真机调试 勾选 `局域网调试`

- 上传发布 测试 AppID 无法发布，需要申请正式的

### 常见问题
- 自定义顶部导航栏被刘海挡住的问题[官方有比较完整的介绍](https://uniapp.dcloud.net.cn/collocation/pages.html#customnav) 简单的讲就是配置 css 变量 `var(--status-bar-height)`。[自定义导航栏的配置方式](https://blog.csdn.net/m0_55258023/article/details/125780939)

- 开发阶段不需要设置 `request合法域名`，这个是发布上线后的后端地址

## app

## h5

