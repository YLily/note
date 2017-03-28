# less

javascript 编写

/*我是被编译的*/
//我是不被编译的


@变量名:值;
@width: 300px;

@arguments多个参数时，可以用@arguments代替

```css
.border{
	border: 1px solid #ccc;
}
//在其他class名中调用.className

//带参数
.border(@border){
	border: @border solid #ccc;
}

//参数带值 调用 .className(3px);
.border(@border: 10px){
	border: @border solid #ccc;
}
```

.triangle(top,@w,@c){border-color:transparent transparent @c transparent;border-width:@w;}
.triangle(bottom,@w,@c){border-color:@c transparent transparent transparent;border-width:@w;}
.triangle(@_,@w,@c){height:0;width:0;overflow:hidden;}(@_无论匹配到谁，始终带有)

+-*/有一个数值带单位即可

下级直接在上级样式里面写
~'cacl(200px-10px)'
!important









# sass

Ruby编写

严格的缩进式语法规则来书写
不带大括号({})和分号(;)

/*我是被编译的*/
//我是不被编译的

$变量名: 值;
$width: 300px;