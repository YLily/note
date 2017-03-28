


### transform

	transform-style: flat | preserve-3d;
	
flat: 所有子元素在2D平面呈现
preserve-3d: 所有子元素在3D空间呈现
transform-style属性设置在父元素中，并高于任何嵌套的变形元素

	perspective: none | 500px;
	
none: 没有透视变换
length: 用户到z=0平面的距离
persperctive取值越小，3D效果越明显。取值无穷大，或为0或负值时，与取值为none效果一样















没有设置定位值的absolute元素
依旧在DOM tree中，对margin等属性敏感；
实际的高宽都丢失了



### 多行文字垂直居中

```
<div><span>文字</span></div>

div{
	display: table-cell;
	vertical-align: middle;
}

sapn{
	display: inline-block;
}
```



高消耗的css

position:relative;


### 页面布局

* 无宽度
	position: absolute;/float: left;  padding-left: 76px;
	display: table-cell;

* 宽度分离：width属性不与影响宽度的border/padding(有时候包括margin)属性共存
	牺牲一层标签为代价，将width属性放在外层标签上，其余的放在内层标签上


### 优化建议

* 去掉多余样式，省去多余的样式匹配
合并多个文件到一个大文件可以减少请求数
* 减少 reflow
* 性能消耗最大选择器应该是 * 和多 class 选择器(比如 .foo.bar, .foo.bar.baz qux)`(从右向左匹配原则)`
* [type="…""] 比 input[type="…""] 更加耗时
* ::selection 和 :active 是最为耗时的选择器
* border-radius 是最耗时的属性之一，比 shadow 和 gradient 更耗时，要注意的是它只影响 repaint 时间
* text-shadow 和 linear-gradient 是最不耗时的
* Opacity 和透明 rgba() 颜色相对好一些
* box-shadow 带inset one (0 1px 1px 0) 稍微比(0 2px 3px 0) 好一点
* transform: rotate(1deg)，得到的数据非常高，不应该轻易使用 transform


### 移动设备

	<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no">