# 运算符优先级

| 运算符 							| 描述 |
| --------------------------------- | : -------: |
| . [] ()							| |
|  ++ -- ~ ! delete new typeof void | 前缀自增、自减 |
|  * / % 							| |
|  + - + 							| |
|  < < >> >>>						| 位移|
|  < <= > >= instanceof				| |
|  == != === !== 					| |
|  & 								| 按位与 |
|  ^								| 按位异或 |
|  `|`								| 按位或 |
|  && 								| 逻辑与(前面为true返回后面, 前面为false返回前面) |
|  `||`							| 逻辑或(前面为true返回前面, 前面未false返回后面) |
|  ?:								| 三目运算 |
|  = += -= 							|  |
|  , 								| 对每个操作对象从左至右求职, 返回最后一个操作对象的值 |


# 判断是IE浏览器的方法

```js
var isIE = function(){
    var b = document.createElement('b')
    b.innerHTML = '<!--[if IE]><i></i><![endif]-->'
    return b.getElementsByTagName('i').length === 1
}
```

# json操作

```js
	JSON.stringify(obj)  //json对象转为json字符串,obj => str
	JSON.parse(str) 	 //json字符串转化为json对象,str => obj
```

# window.onbeforeunload

onbeforeunload 事件在即将离开当前页面（刷新或关闭）时触发
该事件可用于弹出对话框, 提示用户是继续浏览页面还是离开当前页面
没有在 `<body>` 元素上指定 onbeforeunload 事件, 则需要在 window 对象上添加事件, 并使用 returnValue 属性创建自定义信息
在 Firefox 浏览器中, 只显示默认提醒信息（不显示自定义信息）

# void

void()是运算符,对任何值都返回undefined
void function main(){}; 申明此函数返回的是 undefined
没有 return 的函数默认也是返回 undefined

# jQuery ajax

getScript() 方法通过 HTTP GET 请求载入并执行 JavaScript 文件
```js
	$.getScript(url, success(response, status))
	$.ajax({
	  url: url,
	  dataType: "script",
	  success: success
	});
```

#location.protocol

location.protocol 可读可写的字符串, 可设置或返回当前 URL 的协议  http || https

# window.WebSocket

检测浏览器是否支持WebSocket
```js
	window.WebSocket = window.WebSocket || window.MozWebSocket;  
	if (!window.WebSocket){  
	    alert("WebSocket not supported by this browser");  
	    return;  
	}  
```

```js
var websocket = new WebSocket(url, [protocols]);

//事件 onopen, onmessage, onclose, onerror

websocket.onopen = function(){

    //当readyState的值变为OPEN的时候会触发该事件,该事件表明这个连接已经准备好接受和发送数据

}

websocket.onmessage = function(){

    //当有消息到达的时候该事件会触发。这个Listener会被传入一个名为"message"的MessageEvent对象

}

websocket.onclose = function(){

    //当WebSocket对象的readyState状态变为CLOSED时会触发该事件

}

websocket.onerror = function(){

    //当错误发生时用于监听error事件的事件监听器。会接受一个名为error的event对象

}  

//方法 send, close

websocket.send(JSON.stringify({
	action: "node.remove", 
	id: "001"
}))

websocket.close([code], [reason])

```

# cookie

```js
/*------ setCookie(name,value) -----------*/
function setCookie(name,value)
{
	var Days = 30; //此 cookie 将被保存 30 天
	var exp = new Date();
	exp.setTime(exp.getTime() + Days*24*60*60*1000);
	document.cookie = name + "="+ escape (value) + ";expires=" + exp + ";path=/";
}
/*-------- getCookie(name) ----------*/
function getCookie(name)
{
	var arr = document.cookie.match(new RegExp("(^| )"+name+"=([^;]*)(;|$)"));
	if(arr !=null) return unescape(arr[2]); return null;
}

/*获取请求参数 */
(function($) {
    $.getUrlParam = function(name) {
        var reg = new RegExp("(^|&)" + name + "=([^&]*)(&|$)");
        var r = window.location.search.substr(1).match(reg);
        if (r != null)
            return decodeURIComponent(r[2]);
        return null;
    };
})(jQuery);
```

# 面向对象编程

内部使用this变量, 
使用new运算符, 生成实例
实例对象自动含有一个constructor属性, 指向它们的构造函数
instanceof运算符, 验证原型对象与实例对象之间的关系

```js
function Cat(name,color){
　　this.name=name;
	this.color=color;
}

var s = new Cat("cat1", "blue");
var t = new Cat("cat2", "green");
s.constructor == Cat  //true
s instanceof Cat	  //true

```

不变的属性和方法会浪费内存
每生成一个实例, 都为重复的内容

prototype方法
每个构造函数都有一个prototype属性, 指向一个对象
这个对象的所有属性和方法, 都会被构造函数实例继承
把不变的属性和方法, 直接定义在prototype对象上

```js
Cat.prototype.type = "sss";
s.type === t.type   ///true
```

Prototype模式的验证方法

isPrototypeOf()判断prototype对象和某个实例之间的关系
hasOwnProperty()判断某一个属性是本地属性还是继承自prototype对象的属性
in用来判断, 某个实例是否有某个属性, 不管是不是本地属性,可以用来变量某个对象的所有属性

```js
Cat.prototype.isPrototypeOf(s)   //true
s.hasOwnProperty("name")  //true
s.hasOwnProperty("type")  //false

"name" in s  //true
"type" in s  //true

for(var prop in s){
	//
}  

```

构造函数的继承

Cat 继承 Animal

```js
function Animal(){
	this.species = "animal"
}
function Cat(name, color){
	this.name = name;
	this.color = color;
}

//构造函数绑定, call/apply(array)
function Cat(name, color){
	Animal.apply(this, arguments);  //未绑定原型

	this.name = name;
	this.color = color;
}

var s = new Cat("cat1", "blue");
s.species  //animal


//prototype模式
Cat.prototype = new Animal();  //删除了原先的值, 赋予一个新值 Cat.prototype.constructor指向Animal
var s = new Cat("cat1", "blue");
Cat.prototype.constructor == Animal //true
s.constructor == Animal				//true
Cat.prototype.constructor = Cat; //将Cat.prototype对象的constructor值改为Cat

o.prototype = {}
o.prototype.constructor = o //替换了prototype对象, 为新的prototype催下加上constructor属性, 并指回原来的构造函数

//直接继承prototype
function Animal(){}
Animal.prototype.species = "animal";

Cat.prototype = Animal.prototype;
Cat.prototype.constructor = Cat;  //Animal.prototype对象的constructor属性也改了
Animal.prototype.constructor     // Cat
//效率比较高（不用执行和建立Animal的实例了）,比较省内存
//Cat.prototype和Animal.prototype现在指向了同一个对象, 那么任何对Cat.prototype的修改, 都会反映到Animal.prototype
var s = new Cat("cat1", "blue");
s.species  //animal

//空对象为中介
var F = function(){}
F.prototype = Animal.prototype;
Cat.prototype = new F();  //F是空对象, 所以几乎不占内存
Cat.prototype.constructor = Cat;//修改Cat的prototype对象, 就不会影响到Animal的prototype对象

//封装
function extend(Child, Parent){  //YUI库实现继承的方法
	var F = function(){}
	F.prototype = Parent.prototype;
	Child.prototype = new F();  
	Child.prototype.constructor = Child; 
	Child.uber = Parent.prototype;//uber属性直接指向父对象的prototype属性,实现继承的完备性, 备用性质
}

extend(Cat, Animal);
var s = new Cat("cat1", "blue");
s.speices //animal


//拷贝继承
function extend2(Child, Parent){
	var p = Parent.prototype;
	var c = Child.prototype;
	for(var i in p){
		c[i] = p[i]  //浅拷贝的方法, c[i] 是指向到 p[i], 而非赋值, 如果改动 c[i], 会影响到 p[i]
	}
	c.uber = p;
}

```

非构造函数的继承

```js
var Chinese = {
	nation: "china"
}
var Doctor = {
	career: "doctor"
}

//Doctor继承Chinese

//object方法
function object(o){
	function F(){}
	F.prototype = o;
	return new F();
}

var Doctor = object(Chinese);
Doctor.career = "doctor";
Doctor.nation  //chinese

//浅拷贝
function extendCopy(p){
	var c = {};
	for(var i in p){
		c[i] = p[i];
	}
	c.uber = p;
	return c;
}
var Doctor = extendCopy(Chinese);
Doctor.career = "doctor";
Coctor.nation  //chinese
//子对象获得的只是一个内存地址, 而不是真正的拷贝
Chinese.birthPlaces = ['beijing', 'shanghai'];
Doctor.birthPlaces.push('xiamen');

Doctor.birthPlaces  //beijing, shanghai, xiamen
Chinese.birthPlaces //beijing, shanghai, xiamen

//深拷贝
function deepCopy(p, c){
	var c = c || {};
	for(var i in p){
		if(typeof p[i] === 'object'){
			c[i] = (p[i].constructor === Array) ? [] : {};
			deepCopy(p[i], c[i]);
		}else{
			c[i] = p[i];
		}
	}
	return c;
}
var Doctor = deepCopy(Chinese);
Chinese.birthPlaces = ['beijing', 'shanghai'];
Doctor.birthPlaces.push('xiamen');

Doctor.birthPlaces  //beijing, shanghai, xiamen
Chinese.birthPlaces //beijing, shanghai

```

# atob base64位编码

```js
var encodeData = window.atob(str); //encode a string
var decodeData = window.atob(encodeData) // decode the string
```

# 函数式编程

* 函数与其他数据类型一样,可以赋值给其他变量,也可以作为参数,传入另一个函数,或者作为别的函数的返回值
* 函数式编程要求,只使用表达式,不使用语句, 每一步都是单纯的运算,而且都有返回值(表达式(expression)是一个单纯的运算过程,总是有返回值;语句(statement)是执行某种操作,没有返回值)
* 函数要保持独立,所有功能就是返回一个新的值,没有其他行为,尤其是不得修改外部变量的值, 函数内部与外部互动,不产生运算以外的其他结果
* 函数式编程只是返回新的值, 不修改系统变量, 函数式编程使用参数保存状态, 最好的例子就是递归
* 函数的运行不依赖于外部变量或"状态", 只依赖于输入的参数, 任何时候只要参数相同, 引用函数所得到的返回值总是相同的

优点：
1. 代码简洁，开发快速
2. 接近自然语言，易于理解
3. 更方便的代码管理
4. 易于"并发编程"
5. 代码的热升级

# 过程式编程


# js事件

```js
oncontextmenu		  //鼠标右击事件,e.returnValue=false 阻止页面鼠标右击事件

```

# Object.defineProperty

	Object.defineProperty(obj, prop, descriptor)

* obj 目标对象
* prop 需要定义的属性或方法的名
* descriptor 目标属性所拥有的特性
	* value 属性的值
	* writable 为false则属性的值不能被重写
	* configurable 为false则删除或修改属性特性(writable, configurable, enumerable)的行为无效
	* enumerable 是否能在for in循环中遍历出来，或Object.keys中列举出来
	* get 目标属性被访问会被调用
	* set 目标属性被复制会被调用
