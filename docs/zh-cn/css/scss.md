# scss相关
## 小技巧及注意事项
### 打印变量
```scss
@debug $variable;

@debug '88888' $variable
```

### 合并两个map
```scss
$themes: map-merge($dark, $light);
```

### 将一个内部变量提升为全局使用
```scss
@mixin func {
  /* 将内部变量 $theme-map-inner 命名为 全局变量 $theme-map */
  $theme-map: $theme-map-innner !global;
}
```
### 和js一样，定义的同名变量后面的会覆盖前面的
```scss
@import "./dark-one.scss";
@import "./dark-two.scss";

// 无论是引入的文件内部的两个同名变量 还是 当前文件中的两个同名变量

$theme: (color: 'red')
$theme: (color: 'blue')

```
### 关于scss文件中定义函数和使用函数
```scss
@function themed($key) {
  @return map-get($theme-map, $key);
}

```

### @mixin 和 @include
```scss
// @mixin 用来向全局混入样式(记住，是样式，不是函数)
@mixin func {}

// @include 用来在任何地方使用被混入了的样式
@include func{}
```

## 主题切换
### 