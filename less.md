##LESS笔记
===
引导表达式：

```
.mixin (@a) when (lightness(@a) >= 50%) {
  background-color: black;
}
.mixin (@a) when (lightness(@a) < 50%) {
  background-color: white;
}
.mixin (@a) {
  color: @a;
}
```

when关键字用以定义一个导引序列(此例只有一个导引)。接下来我们运行下列代码：

```
.class1 { .mixin(#ddd) }
.class2 { .mixin(#555) }
```

就会得到：

```
.class1 {
background-color: black;
color: #ddd;
}
.class2 {
background-color: white;
color: #555;
}
```

导引中可用的全部比较运算有： > >= = =< <。此外，关键字true只表示布尔真值，下面两个混合是相同的：

```
.truth (@a) when (@a) { ... }
.truth (@a) when (@a = true) { ... }
```

除去关键字true以外的值都被视示布尔假：

```
.class {
  .truth(40); // Will not match any of the above definitions.
}
```

导引序列使用逗号‘,’—分割，当且仅当所有条件都符合时，才会被视为匹配成功。

`.mixin (@a) when (@a > 10), (@a < -10) { ... }`

导引可以无参数，也可以对参数进行比较运算：

```
'@media: mobile;

.mixin (@a) when (@media = mobile) { ... }
.mixin (@a) when (@media = desktop) { ... }

.max (@a, @b) when (@a > @b) { width: @a }
.max (@a, @b) when (@a < @b) { width: @b }
```

最后，如果想基于值的类型进行匹配，我们就可以使用is*函式：

```
.mixin (@a, @b: 0) when (isnumber(@b)) { ... }
.mixin (@a, @b: black) when (iscolor(@b)) { ... }
```

下面就是常见的检测函式：

iscolor
isnumber
isstring
iskeyword
isurl

如果你想判断一个值是纯数字，还是某个单位量，可以使用下列函式：

ispixel
ispercentage
isem

最后再补充一点，在导引序列中可以使用and关键字实现与条件：

`.mixin (@a) when (isnumber(@a)) and (@a > 0) { ... }`

使用not关键字实现或条件

`.mixin (@b) when not (@b > 0) { ... }`
