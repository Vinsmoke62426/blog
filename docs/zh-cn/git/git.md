### 公司私有git 新建仓库注意事项

新建仓库时，设置为私有的，同时还要新建组织，将仓库和人员添加进组织

### GitHub 可以直接在网页开启代码编辑

在 对应的仓库主页点击按钮 `.`, 会进入一个 vscode 的编辑页面

### git clone 失败，文件太大

遇到过代码中有 3D建模的情况，clone 的时候报错

解决方法：只 clone 最新的 commit 
```
git clone  --depth=1 ssh://git@104.XXX.XXX.XXX:27681/home/git/warehouse/roysue.git
```

### 解决 github 访问慢的问题
[直接用里面的host就行，很方便](https://github.com/521xueweihan/GitHub520)

### github fork 其他人代码的全套流程

fork 别人的仓库到自己的仓库中，方便自己更改

如果别人的仓库更改了， 你想拉取更改，点击 Fetch upstream，然后 Fetch and merge

这样就把别人仓库的代码同步到你自己的远程仓库了，然后在本地 pull 拉去自己的远程仓库的最新改动

如果你想将自己的修改提交到别人的仓库中，合作开发

先 New pull request， 新建一个提交请求，可以对比你当前远程仓库和别人仓库的代码区别

新建之后，别人可以看到你的请求，如果别人觉得合适，采纳了，就可以在别人的仓库看到我们自己的提交记录了😊

### git worktree
```
git worktree add <新路径> -b <新分支名>
```
![](../../images/worktree.png)

[其他注意事项](https://www.cnblogs.com/jasongrass/p/11178079.html)


### git 文件名大小写问题
git默认配置是忽略大小写的，这可能会导致在多人开发的时候出现奇怪的问题

解决方法: 修改默认配置
```
git config core.ignorecase false
```

上面的方法也不好，建议删除远程仓库Header文件夹下的所有文件，然后再正常提交
```
git rm --cached src/components/Header -r
```

### 撤销本地分支与远程分支的映射关系
```
git branch --unset-upstream
```

### 回退代码
尽量用 git soft reset 不要用 git hard reset

soft 回退后被回退的代码会暂时放在暂存区，然后执行 stash 将这些代码存起来

hard 回退后被回退的代码直接就没有了

`回退后，要push，只能 git push --force 去覆盖远程的仓库代码`

### 修改远程仓库地址
```
git remote set-url origin ssh://git@xx.xx.xxx.xx:223/DQGS/zts-web.git
```

### 清理已经提交了的大文件

找出最大的三个文件
```
$ git verify-pack -v .git/objects/pack/*.idx | sort -k 3 -n | tail -3
# 输出：
4d2ae4c4413740d81019aa65691a2f75f00a5d3b blob 657413784 136436872 5330845
4d2ae4c4413740d81019aa65691a2f75f00a5d3b blob 657413784 136436872 5668892
4d2ae4c4413740d81019aa65691a2f75f00a5d3b blob 657413784 136436872 6313927
```
查看大文件是什么文件
```
$ git rev-list --objects --all | grep 4d2ae4c4
# 4d2ae4c4413740d81019aa65691a2f75f00a5d3b 15504.hprof
```
[借助java的工具，要提起配置jdk以及下载软件](https://rtyley.github.io/bfg-repo-cleaner/)

点击下载完成后将这个jar包复制到您到项目根目录下与.git目录同级

删除某个文件
```
java -jar bfg-1.14.0.jar --delete-files WxUtils.java
```
删除完成后提交
```
git push --force
```