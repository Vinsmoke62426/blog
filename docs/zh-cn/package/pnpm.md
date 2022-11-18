# pnpm
[基础的东西官网讲的很详细，这里不在赘述](https://www.pnpm.cn/installation)，这里讲一下要注意的细节以及常见的问题
## 从npm->pnpm
全局安装 pnpm 后，先删除掉项目中 package-lock.json 以及 node_modules 文件，然后直接 install 就行

在你安装好依赖后 pnpm 会标记出你依赖之间的缺少的依赖，同时推荐给你对应的安装办法，这点很好
## 常见问题
### 安装好了项目跑不起来
`报找不到 qs 的错误`，死活找不到原因

[最后在这篇文章下面看到作者的回复，才恍然大悟](https://zhuanlan.zhihu.com/p/546400909)

因为 pnpm 依赖访问严格，规则清晰，界限分明

这样带来的好处是不再如以前一样容易出现依赖冲突，纠正我们之前的一些错误认知

自定义构建脚本中 `require('webpack')`，`require('qs')` 是典型的访问幽灵依赖的行为

pnpm 在默认配置下，你需要把 webpack 声明在项目的 package.json -> devDependencies 中才允许使用

我们看似利用依赖之间内部使用了相同的依赖，而直接引入 webpack 或者 qs 之类的依赖，`这在 pnpm 的规范下都是不允许的`

解决办法：老老实实的在 package.json 中声明好 qs

值得一提的是，网上的很多文章会让你用 `pnpm install --shamefully-hoist` 这个命令去解决问题

问题可能会被解决，但是它会将你的 node_modules 目录结构变得和 npm yarn 一样，那样将失去使用 pnpm 的意义，官方也不推荐