# nexus
## 配置私有的npm下载和publish地址
![](../../images/npmrc.png)

加了@的是加了命名空间的registry，可以在你publish带@epnc或者@penc-test前缀的组件时，自动识别

跟在下面一行的是去掉http前缀的registry

然后冒号后面跟的是对应的registry的`账号:密码`的base64。可以在publish的时候免登录

给下面人用的时候，只需要第一行就行