# babel 转码器

将es6转为es5

配置文件.babelrc, 放在项目的根目录下

```json
{
	"presets": [],
	"plugins": []
}
```

babel-cli 命令行转码


# 块级作用域

es5中只有全局作用域和函数作用域
es6新增块级作用域

允许块级作用域任意嵌套
外层作用域无法读取内层作用域的变量
内存作用域可以定义外层作用域的同名变量(外层代码块不受内存代码块的影响)

使用let，不需要使用IIFE了
IIFE(immediately-invoked function expression，即：立即调用函数表达式)(使用IIFE以避免污染全局环境并只是在全局定义)


### 函数声明

es6允许在块级作用域之中声明函数
函数声明语句行为类似let, 在块级作用域之外不可引用
ES6 的块级作用域允许声明函数的规则，只在使用大括号的情况下成立，如果没有使用大括号，就会报错
块级作用域不返回值

为兼容老代码, es6的浏览器遵守

* 允许在块级作用域内声明函数
* 函数声明类似var， 会提升到全局作用域或函数作用域的头部
* 函数声明还会提升到所在的会计作用域的头部

尽量避免咋块级作用域内声明函数，尽量写成函数表达式而不是函数声明语句

### let 

let命令声明的变量只在let命令所在的代码块{}内有效
let命令声明的变量，不会发生变量提升，一定要声明后再使用, 否则报错(referenceError)(var 变量会发生变量提升，undefined)
不允许在相同作用域内重复声明同一个变量

"暂时性死区"(temporal dead zone，简称 TDZ)
指从代码块开始到let命令声明变量之间的区域
在代码块内，使用let命令声明变量之前，该变量都是不可用的


在let命令声明变量之前使用typeof会报错(referenceError)
没有声明的变量typeof会输出undefined
没有let之前，typeof运算符是百分之百安全的，永远不会报错	
	

```js
for (let i = 0; i < 3; i++) {
  let i = 'abc';
  console.log(i);
}
// abc
// abc
// abc
```
for循环中

* 变量i是let声明的，当前的i只在本轮循环有效，每一次循环的i其都是一个新的变量
* 循环语句部分是一个父作用域，而循环体内部是一个单独的子作用域

### const

声明一个只读常量，一旦声明，不能改变
对const声明的常量赋值会报错
对const来说，只声明不复制会报错
const一旦声明变量，就必须立即初始化，不能留到以后赋值		

实质

const保证的不是变量不得改动
而是变量指向的那个内存地址不得改动

简单类型的数据(数值，字符串，布尔值),值保存在变量指向的那个内存地址
符合型数据(对象和数组)，变量指向的内存地址，保存的是一个指针，const只能保证这个指针是固定的，不能保证指针指向的数据结构

将对象冻结，使用Object.freeze

```js
const foo = Object.freeze({})

foo.prop = 123;
//常规模式会不起作用
//严格模式，会报错
```
将对象本身和对象的属性都冻结
```js
var constantize = (obj) => {
  Object.freeze(obj);
  Object.keys(obj).forEach( (key, i) => {
    if ( typeof obj[key] === 'object' ) {
      constantize( obj[key] );
    }
  });
};
```

# 变量

ES6为了保持兼容性，var命令和function命令声明的全局变量，依旧是顶层对象的属性
let命令、const命令、class命令声明的全局变量，不属于顶层对象的属性


解构(Destructuring)

es6允许从数据和对象中提取值，对变量进行赋值

如果解构不成功，变量的值为undefined
不完全解构也可以成功

结构赋值允许指定默认值
es6内部使用严格相等运算符(===)，判断一个位置是否有值。如果一个数组成员不严格等于undefined，默认值不会生效
默认值可以引用解构赋值的其它变量，但改变量必须已经声明
默认值是一个表达式，这个表达式是惰性求职的，只有在用到的时候，才会求值

### 数组解构赋值

只要某种数据结构具有Iterator接口，都可以采用数组的形式进行结构赋值

```js
let [a, b, c] = [1, 2, 3];
let [x, y = 'b'] = ['a', undefined]  // x = 'a', y = 'b'
let [x = 1] = [null]  //x = null
let [x = 1, y = x] = []  //x = 1, y =  1
let [x = y, y = 1] = []  // referenceError
```

### 对象的解构赋值

数组的元素是按次序排列的
对象的属性没有次序， 变量必须与属性同名，才能取到正确的值
先找到同名属性， 然后再赋值给对应的变量，真正被赋值的是后者不是前者， 前者是匹配模式，后者才是变量

重复声明会报错
圆括号是必须的，解析器会将起首的大括号理解成一个代码块而不是赋值语句

结构模式是嵌套的对象， 而且子对象需哦在的父属性不存在，会报错

```js
let { bar, foo } = { foo: 'a', bar: 'b'}  //foo = a, bar = b
let obj = { first: 'hello', last: 'world'};
let {first: f, last: l} = obj;  // f= 'hello', l = 'world'
let foo;
let {foo} = {foo: 1}  //syntaxError
let baz;
({bar: baz} = { bar:1})　// baz = 1

let obj = {
	p: [
		'hello',
		{y: 'world'}
	]
}
//p是模式不是变量，不会被赋值
let {p: [x, {y}]} = obj;  // x = 'hello', y = 'world'

var {x, y = 5} = { x: 1, y: undefined}  //x = 1, y = 5
var {x, y = 5} = { x: 1, y: null}  //x = 1, y = null

let { foo: {bar}} = = {baz: 'baz'}  //foo = undefinded, 再取子属性会报错

let x;
({x} = {x: 1})

//毫无意义， 但是语法合法， 可以执行
({} = [true, false]);
({} = 'abc');
({} = []);

//将Math对象的对数，正弦，余弦三个方法，赋值到对应的变量上
let { log, sin, cos } = Math;

let arr = [1, 2, 3];
let {0 : first, [arr.length - 1] : last} = arr;  // first = 1, last = 3
```

### 字符串解构赋值

```js
const [a, b, c, d, e] = 'hello';
// a = h, b = 3, c = l, d = l, e = o

let {length : len} = 'hello'  // len = 5
```

### 数值和布尔值的解构赋值

解构赋值的规则是
只要等号右边的值不是对象或数组，就先将其转为对象
由于undefined和null无法转为对象，所以对它们进行解构赋值，都会报错。

```js
let {toString: s} = 123  
s === Number.prototype.toString  //true

let {toString: s} = true
s === Boolean.prototype.toString  //true

let {prop: x} = undefined  //typeError

let {prop: y} = null  //typeError
```

### 函数参数的解构赋值

```js
function add([x, y]){
	return x + y;
}
add([1, 2]);

[[1, 2], [3, 4]].map(([a, b] => a + b));  // [3, 7]

function move({x = 0, y = 0} = {}){
	return [x, y];
}
move({x: 3, y: 2})  //[3, 2]
move({})  // {0, 0}

[1, undefined, 3].map((x = 'yes') => x)  // [1, 'yes', 3]
```

### 圆括号

变量声明语句中，不能带有圆括号
函数参数中，不能带有圆括号
赋值语句中，不能将整个模式，或嵌套模式中的一层，放在圆括号之内

```js
let [(a)] = [1];
let {(x): c} = {}

function f([(z)]){return z;}

({p: a}) = {p: 4};
([a]) = [5]
```

赋值语句的非模式部分， 可以使用圆括号

```js
[(b)] = [3];
({p: (d)} = {})
[(parseInt.prop)] = [3]
```

### 用途

交换变量的值

```js
let x = 1;
let y = 2;
[x, y] = [y, x];
```
从函数返回多个值

```js
function example(){
	return [1, 2, 3]
}
let [a, b, c] = example();

function example(){
	return {
		foo: 1,
		bar: 2
	}
}
let {foo, bar} = example();
```

函数参数的定义

```js
function f([x, y, z]){ }
f([1, 2, 3])

function f({x, y, z}){ }
f({z: 3, y: 2, x:1})
```

提取JSON数据

```js
let josnData = {
	id: 42,
	status: 200,
	data: [12, 22]
}

let {id, status, data: number} = jsonData;
//id = 42, status = 200, number = [12, 22]
```

函数参数的默认值

```js
jQuery.ajax = function(url, {
	async = ture,
	beforeSend = functjion(){},
	cache = true,
	complete = function(){},
	crossdomain = false,
	global = true,
	...
}){
	
}
```

遍历Map结构

```js
var map = new Map();
map.set('first', 'hello');
map.set('second', 'world');

for(let [key, value] of map){
	...
}

// 获取键名
for (let [key] of map) {
  // ...
}

// 获取键值
for (let [,value] of map) {
  // ...
}
```

输入模块的指定方法

加载模块时，往往需要指定输入哪些方法。解构赋值使得输入语句非常清晰。
```
const { SourceMapConsumer, SourceNode } = require("source-map");
```

# 扩展

###　字符串

字符的 Unicode 表示法 
\u0000 ~ \uFFFF

es6对超过的字符，只要放入大括号里就能正确解读
\u{20BB7}

\u{1F680} === \uD83D\uDE80  //true

大括号表示法和四字节的UTF-16编码是等价的


js内部，字符以utf-16的格式存储， 每个字符固定2个字节
需要4个字节存储的字符，js认为是2个字符

“𠮷”（不是”吉祥“的”吉“）的码点是0x20BB7
UTF-16编码为0xD842 0xDFB7（十进制为55362 57271）
需要4个字节储存

##### codePointAt

```js
var s = "𠮷";

s.length  //2
s.charAt(0)  // ''
s.charAt(1)  // ''
s.charCodeAt(0)  // 55362
s.charCodeAt(1)  // 57271


var s = '𠮷a';
s.codePointAt(0) // 134071 返回回了它的十进制码点134071（即十六进制的20BB7）
s.codePointAt(1) // 57271
s.codePointAt(2) //97

s.codePointAt(0).toString(16) // "20bb7"

for (let ch of s) {
  console.log(ch.codePointAt(0).toString(16));
}
// 20bb7
// 61

//测试一个字符由两个字节还是由四个字节组成的最简单方法
function is32Bit(c) {
  return c.codePointAt(0) > 0xFFFF;
}
```
codePointAt方法会正确返回32位的UTF-16字符的码点。对于两个字节储存的常规字符，返回结果与charCodeAt方法相同

##### String.fromCodePoint()

String.freomCharCode,用于从码点返回对应的字符，但是不能识别32位的utf-16字符
大于0xFFFF的字符，最高位会被舍弃

String.fromCodePoint可以识别大于0xFFFF的字符
String.fromCodePoint方法有多个参数，则会被合并成一个字符串再返回

formCodePoint方法定义在String对象上
codePoint方法定义在字符串的实例对象上

```js
String.fromCodePoint(0x20bb7)  // '𠮷'

String.fromCodePoint(0x78, 0x1f680, 0x79) === 'x\uD83D\uDe80y'  // true
```
##### 字符串遍历器接口

```js
for(let codePoint of 'foo'){
	console.log(codePoint)
}
// f, o, o

var text = String.fromCodePoint(0x20BB7);

for (let i = 0; i < text.length; i++) {
  console.log(text[i]);
}
// " "
// " "

for (let i of text) {
  console.log(i);
}
// "𠮷"
```

##### at()

```js
'abc'.charAt(0)  // a
'𠮷'.charAt(0)  // \uD842

'abc'.at(0)  // a
'𠮷'.at(0)  // '𠮷'
```

##### includes(), startsWith(), endsWith()

includes() 返回布尔值，表示是否找到了参数字符串
startsWith() 返回布尔值，表示参数字符串是否在源字符串的头部
endsWith() 返回布尔值，表示参数字符串是否在源字符串的尾部

第二个参数n，表示开始搜索的位置从第n个位置直到字符串结束
endsWith表示针对前n个字符

```js
var s = 'Hello world!';

s.startsWith('Hello')  //true
s.endsWith("!")  //true
s.includes("o")  //true

s.endsWith('Hello', 5);  // true
s.includes('Hello', 5);  // false

s.indexOf('e')  //1
```

##### repeat()

返回一个新的字符串，表示将源字符串重复n次
小数会被取整
是负数或是Infinity会报错
0 ~ -1之间的小数取整以后等于-0，repeat视为0
NaN等同与0
是字符串会先转换成数字

```js
'x'.repeat(3)  // xxx
'hello'.repeat(2)  // hellohello

'n'.repeat(0)  // ''
'n'.repeat(2.6) // nn
'n'.repeat(-1) // rangeError
'n'.repeat(Infinity) // rangeError
'n'.repeat(-0.9) // ''
'n'.repeat(NaN) // ''
'n'.repeat('str') // ''
```

##### 字符串模板

模板字符串,用反引号`标识
可以当做普通字符串使用
可以定义多行字符串(多行字符串所有的空格和缩进都会被保留在输出中，可以用trime来消除 ``.trim())
可以再字符串中嵌入变量(变量名要写在${}之中)
在模板字符串中使用反引号需要用反斜杠转义

模板字符串中还可以调用函数
大括号中可以放入任意的javascript表达式，可以进行计算，引用对象属性
大括号中的值不是字符串将转为字符串
模板字符串中的变量没有声明，会报错
```js
` common string`
`multi-line
string`

var name = 'Lily', time = 'today';
`Hello ${name}, how are you ${time}?`

var greeting = `\`Yo\` World!`

function fn(){
	return 'hello world'
}
`foo ${fn()} bar`
```

### 正则表达式

### 数值

二进制 0b表示
八进制 0o表示(不能用0表示)

Number(num) 转为10进制

##### Number.isFinite() 检查一个数值是否有限
#####  Number.isNaN()  检查一个数值是否为NaN

```js
Number.isFinite(15)  //true
Number.isFinite(NaN)  //false
Number.isFinite(Infinity) //false

Number.isNaN(NaN)  //true
Number.isNaN(15)   //false
Number.isNaN('true'/0)  //true
```
与传统ifFinite()和isNaN()区别在于，传统先调用Number()将非数值转为数值后再进行判断
新方法只对数值有效

```js
isFinite(25)  //true
isFinite('25')  //true
Number.isFinite('25')  //false

isNaN('NaN')  //true
Number.isNaN('NaN')  // false
```

### Number.parseInt()  Number.parseFloat()

减少全局性方法，使语言逐步模块化

##### Number.isInteger()  判断一个值是否为整数

```js
Number.isInteger(25)  //true
Number.isInteger(25.0)  //true
Number.isInteger(25.1)  //false
Number.isInteger('25')  //false
```

##### Number.EPSILON  极小的常量

是一个可以接受的误差范围
为浮点数的计算这是一个误差范围

##### Number.isSafeInteger() 用来判断一个整数是否在安全范围之内

-2^53 ~ 2^53 之间的整数能精确表示
Number.MAX_SAFE_INTEGER 和 Number.MIN_SAFE_INTEGER 来表示这个范围的上下限

```js
Number.isSafeInteger(null)  //false
Number.isSafeInteger('a')   //false
Number.isSafeInteger(Infinity)  /false

Number.isSafeInteger(4)  //true
Number.isSafeInterger(1.2)  //false

Number.isSafeInteger(Number.MAX_SAFE_INTEGER)  //true
Number.isSafeInteger(Number.MIN_SAFE_INTEGER)  //true
Number.isSafeInteger(Number.MAX_SAFE_INTEGER + 1)  //false
Number.isSafeINteger(Number.MIN_SAFE_INTEGER - 1)  //false
```

验证运算是否落在安全整数的范围内
不仅要验证运算结果
还要验证参与运算的每个值

##### Math 

###### Math.trunc() 去除一个数的小数部分，返回整数部分
非数值内部使用Number转为数值后再运算
空值和无法截取整的值返回NaN

###### Math.sign() 判断一个数是正数，负数还是零

正数 返回+1
负数 返回 -1
为0  返回0
未-0 返回-0
其他值 返回NaN

###### Math.cbrt()  计算一个数的立方根

非数值，内部使用Number先转为数值

###### Math.clz32() 返回一个数的32位无符号整数形式有多少个前导0

对于小数，Math.clz32只考虑整数部分
其它值先转为数值，再计算

左移运算(<<)

```js
Math.clz32(0)  //32
Math.clz32(1)  //31

Math.clz32(3.2)  //30
Math.clz32([])  //32
Math.clz32(NaN)  //32

Math.clz32(1 << 1)  //30
Math.clz32(1 << 2)  //29
```
###### Math.imul() 返回两个数以32位带符号整数形式相乘的结果，返回的也是一个32未的带符号整数

超过32位的部分溢出 
可以返回正确的低位数的值, 等同于(a * b)|0

```js
Math.imul(2, 4)  //8
Math.imul(-1, 8)  //-8
Math.imul(-2, -2)  //4

Math.imul(0x7fffffff, 0xfffffff)  //1
```

###### Math.fround() 返回一个数的单精度浮点数形式

整数返回的结果不会有任何不同
无法用64个二进制位精确表示的小数，返回最接近这个小数的单精度浮点数

```js
Math.fround(0)  //0
Math.fround(1)  //1
Math.fround(1.5)  //1.5
Math.fround(1.337)  //1.3370000123977661
```

##### Math.hypot() 返回所以参数的平方和的平方根

只要有一个参数无法转为数值，就好返回NaN

```js
Math.hypot(3, 4)  //5  
Math.hypot(-3)  //3
```

##### 对数

Math.expml()  返回e^x - 1, 即Math.exp(x) -1
Math.log1p()  返回1 + x 的自然对数，即Math.log(1 + x), 如果x小于-1，返回NaN
Math.log10()  返回以10为底x的对数，如果x小于0，返回NaN
Math.log2()  返回以2为底的x的对数， 如果x小于0，返回NaN

##### 三角函数

Math.sinh(x)  返回x的双曲正弦
Math.cosh(x)  返回x的双曲余弦
Math.tanh(x)  返回x的双曲正切
Math.asinh(x) 返回x的反双曲正弦
Math.acosh(x) 返回x的反双曲余弦
Math.atanh(x) 返回x的反双曲正切

##### Math.signbit() 判断一个数的符号位是否设置了

第一位是符号位，0表示正数， 1表示负数

NaN 返回false
-0  返回true
负值 返回true
其它情况返回false

Math.sign() 判断一个值的正负，参数为-0返回-0

```js
Math.sign(-0)  //-0

Math.signbit(0)  //false
Math.signbit(-0)  //true
Math.signbit(2)  //false
Math.signbit(-2)  //true
```

##### 指数运算**

与Math.pow实现不同，特别大的运算结果，会有细微的差异
```js
2**2  // 4
2**3  // 8

let a = 1.5;
a **= 2 等同于 a = a * a
```

### 数组

##### Array.from() 将一个类似数组的对象转为正值的数组

类似数组的对象 ， 必须有length属性

* DOM操作返回的NodeList集合
* 函数内部的arguments对象

扩展运算符（...）也可以将某些数据结构转为数组
有length属性的对象都可以用Array.form转为数组， 扩展运算符无法转换
扩展运算符调用的是遍历器接口(symbol.iterator)

部署了Iterator接口的数据结构，Array.from都能将其转为数组
参数是一个真正的数组，Array.from会返回一个一模一样的新数组

Array.from可以接收第二个参数，作用类似于数组的map方法, 对每个元素进行处理后，将处理后的值放入返回的数组
```js
let arrayLike = {
    '0': 'a',
    '1': 'b',
    '2': 'c',
    length: 3
};

// ES5的写法
var arr1 = [].slice.call(arrayLike); // ['a', 'b', 'c']

// ES6的写法
let arr2 = Array.from(arrayLike); // ['a', 'b', 'c']

// NodeList对象
let ps = document.querySelectorAll('p');
Array.from(ps).forEach(function (p) {
  console.log(p);
});

// arguments对象
function foo() {
  var args = Array.from(arguments);
  // ...
}

Array.from('hello')
// ['h', 'e', 'l', 'l', 'o']

let namesSet = new Set(['a', 'b'])
Array.from(namesSet) // ['a', 'b']

// arguments对象
function foo() {
  var args = [...arguments];
}

// NodeList对象
[...document.querySelectorAll('div')]

Array.from({length:3}); //扩展运算符无法转换

Array.from(arrayLike, x => x * x)

//将字符串转为数组，然后返回字符串的长度，避免将大于\uFFFF的Unicode字符，算作2个字符的bug
function countSymbols(string){
	return Array.from(string).length
}
```

##### Array.of()  将一组值转换为数组

总是返回参数值组成的数组，没有参数，返回一个空数组
弥补构造函数Array()的不足

```js
Array.of()  //[]
Array.of(3, 6, 9)  //[3, 6, 9]

//参数不小于2个时，Array才会返回有参数组成的新数组
Array()  //[]
Array(3) //[, , ,]
Array(3, 10, 12) //[3, 10, 12]
```

##### copyWithin() 将制定位置的成员复制到其它位置(会覆盖原有成员)，然后返回当前数组

target 必需， 从改位置开始替换数据
start	可选 从该位置开始读取数据，默认为0，如果为负值，表示倒数
end  可选， 到该位置前停止读取数据，默认等于数组的长度，如果为负值表示倒数

这3个值如果不是数值会自动转为数值

```js
Array.prototype.copywithin(target, start=0, end=this.length)

[1, 2, 3, 4, 5].copywithin(0, 3)  //[4, 5, 3, 4, 5]

[].copyWithin.call({length: 5, 3: 1}, 0, 3)
// {0: 1, 3: 1, length: 5}
```

##### find()  findIndex()

find() 找出第一个符合条件的数组成员
参数是一个回调函数，依次执行该回调函数，直到找出第一个返回值为true的成员，没有符合条件的成员返回undefined
回调函数接受三个参数，依次为当前值，当前的委会和原数组
第二个参数用来绑定回调函数的this对象

```js
[1, 4, -5, 10].find((n) => n < 0)  //-5
[1, 4, -5, 10].find((value, index, arr) => { return value > 9 })  //10
```

findIndex() 返回第一个符合条件数组成员的位置
所有的成员都不符合条件，返回-1
可以借助Object.is方法识别NaN，弥补indexOf方法的不足

```js
[1, 4, -5, 10].findIndex( n => n < 0)  //2

[NaN].indexOf(NaN)  //-1

[NaN].findIndex( y => Object.is(NaN, y))  //0
```

##### fill() 使用定值，填充一个数组
第二个参数 指定填充的起始值
第三个参数 指定结束的位置
```js
['a', 'b', 'c'].fill(3)  //[3, 3, 3]
['a', 'b', 'c'].fill(3, 1, 2)  //['a', 3, 'c']
new Array(3).fill(2)  //[2, 2, 2]
```

##### entries()  keys()  values()

entries() 对键值对遍历
keys()  对键名遍历
values() 对键值遍历

```js
for(let [index, elem] of ['a', 'b'].entries()){
	
}

for(let index of ['a', 'b'].keys(){
	
})

for(let elem of ['a', 'b'].values()){
	
}

let entries = ['a', 'b', 'c'].entries();
console.log(entries.next().value)  // [0, 'a']
console.log(entries.next().value)  // [1, 'b']
console.log(entries.next().balue)  // [2, 'c']
```

##### includes()

返回一个布尔值

第二个参数表示搜索的起始位置，默认为0
为负数，则表示倒数, 负数数值大于数组长度，则为0
indexOf 内部使用更严格(===)进行判断，会对NaN误判

```js
[1, 2, 3].includes(2)  //true
[1, 2, NaN].includes(NaN)  //true

[1, 2, 3].includes(3, 3)  //false
[1, 2, 3].includes(3, -1)  // true

arr.indexOf(el) !== -1

[NaN].indexOf(NaN)  //-1
[NaN].includex(NaN)  //true
```

##### 空位

空位时没有值的
undefined依然是有值的

```js
0 in [undefined, undefined, undefined]  //true  在0号位置有值
0 in [,,,]  // false 在0号位置无值
```
es5

* forEach(),filter(),every(),some() 会跳过空位
* map()会跳过空位，但会保留这个值
* join(),toString()会将空位视为undefined,而undefined和null会被处理成空字符串

es6

明确将空位转为undefined

```js
Array.from(['a,,'b])  //['a', undefined, 'b']
[...['a',,'b']]  //['a', undefined, 'b']
[,'a','b',,].copywithin(2, 0)  //[,'a',,'a']
new Array(3).fill('a')

let arr = [,,];
for(let i of arr){
	console.log(1)
}
//1
//1
```

### 函数

##### 默认值

```js
//es5
function welcome(x, y){
	y = y || 'world';
	console.log(x, y)
}
welcome('Hello')  //Hello world
welcome('Hello', 'China')  //Hello China
welcome('Hello', '')  //Hello world

//es6
function welcome(x, y = 'world'){
	console.log(x, y)
}
welcome('Hello')  //Hello world
welcome('Hello', 'China')  //Hello China
welcome('Hello', '')  //Hello
```

参数变量是默认声明的，不能用let或const再次声明
默认值是惰性求值的
如果是变量，每次都重新计算表达式的值

```js
let x = 99;
function add(p = x + 1){
	console.log(p)
}
add()  // 100
x = 100;
add()  //101
```

默认值使用解构赋值
当参数是一个对象时才能通过解构赋值生成值，否则会报错

```js
function fetch(url, {body = '', method = 'GET', headers = {}}){
	console.log(method)
}
fetch('http://example.com', {})   // 'GET'
getch('http://example.com')  //referenceError

function fetch(url, {method = 'GET'} = {}){}
fetch('http://example.com')  //'GET'
```

```js
//函数参数的默认值是空对象，但设置了对象解构赋值的默认值
function m1({x = 0, y = 0} = {}){
	return [x, y];
}
//函数参数默认值是一个有具体属性的对象，但是没有设置解构赋值的默认值
function m2({x, y} = {x: 0, y:0}){
	return [x, y]
}

m1()  //[0, 0]
m2()  //[0, 0]

m1({x:2, y:3})  //[2, 3]
m2({x:2, y:3})  //[2, 3]

m1({x:2})  //[2, 0]
m2({x: 2})  //[2, undefined]

m1({})  //[0, 0]
m2({})  //[undefined, undefined]

m1({z: 1})  //[0, 0]
m2({z: 1})  //[undefined, undefined]
```
##### 参数默认值赢在尾参数

不在尾部，该参数不能胜率，除非显示输入undefined
undefined触发参数等于默认值，null不行

```js
function fff(x = 5, y = 6){
	console.log(x, y)
}
foo(undefined, null)  // 5, null
```

##### 函数的length属性  等于函数参数的个数 - 指定了默认值的参数的个数

默认值不是尾参数，length属性不计入后面的参数了
rest参数不会计入length属性

```js
(function(a){}).length //1
(function(a = 5){}).length //0
(function(a=5, b){}).length //0

(function(...args){}).length  //0
```

##### 作用域

参数设置了默认值，函数进行声明的初始化会形成一个单独的作用域，初始化结束，这个作用域会消失

```js
var x = 1;
function f(x, y = x){
	console.log(y)
}
f(2)  //2

//默认值x未定义,指向外层的全局变量x
//全局变量x不存在，会报错
//函数体内部的局部变量x不影响默认值x
//默认值是一个函数，也遵守这个规则
let x = 1;
function f(y = x){
	let x = 2;
	console.log(y)
}
f()  //1

var x = 1;
function foo(x, y = function() { x = 2; }) {
  x = 3;
  y();
  console.log(x);
}

foo() // 2
x // 1
```
##### 可以省略的参数和不能省略的参数

```js
function throwIfMissing(){
	throw new Error('Missing parameter')
}

function foo(mutBeProvided = throIfMissing()){
	return mustBeProvided;
}

foo()  //Error:Missing parameter


//undefined 表示参数可以省略
function foow(optional = undefined){}
```

### rest参数

...变量名 用于获取多余的参数，不需要使用arguments
rest参数中的变量代表一个数组
rest参数之后不能再有其他参数
函数的length属性不包括rest参数

```js
function add(...values){
	let sum = 0;
	for(var val of values){
	  	sum += val;
	}
	return sum;
}
add(1, 2, 3)  //6
```

##### 扩展运算符 ...

rest参数的逆运算，将一个数组转为用逗号分隔的参数序列

```js
console.log(...[1, 2, 3])  //1, 2, 3

function push(array, ...items){
	array.push(...items)
}
function add(x, y){
	return x + y;
}
add(...[2, 4])  //6

//替代数组的apply方法
function f(x, y, z){}
//es5
f.apply(null, [1, 2, 3]);
//es6
f(...[1, 2, 3])

Math.max(2, 4, 8);
//es5
Math.max.apply(null, [2, 4, 8]);
//es6
Math.max(...[2, 4, 8]);

//es5
Array.prototype.push.apply([1, 2, 3], [2, 3, 4])
//es6
[1, 2, 3].push(...[2, 3, 4])

//es5
new (Date.bind.apply(Date, [null, 2015, 1, 1]))
//es6
new Date(...[2015, 1, 1])
```

```js
//合并数组
[1, 2].concat(more)
[1, 2, ...more]

//解构赋值
a = list[0], rest = list.slice(1)
[a, ...rest] = list

//函数返回值

//字符串转为真正的数组
[...'hello']  //['h', 'e', 'l', 'l', 'o']

//js会将32位Unicode字符，识别为2个字符
//扩展运算符可以正确识别
'x\uD83D\uDE80y'.length // 4
[...'x\uD83D\uDE80y'].length // 3

function length(str) {
  return [...str].length;
}

length('x\uD83D\uDE80y') // 3

//实现了Iterator接口的对象
//nodeList对象
//Map, Set, Generator函数
var nodeList = document.querySelectorAll('div');
var arr = [...nodeList];
```
##### 严格模式

函数参数使用了默认值，解构赋值， 扩展运算符
函数内部不能显示('use strict')设定为严格模式

原因
函数执行时， 先执行函数参数代码，再执行函数体代码
从函数体代码中才能知道参数代码是否应该以严格模式执行

```js
//全局设定严格模式
'use strict';
function do(a, b = a){
	
}

//把函数包在一个立即执行的函数里面
cost do = (function(){
	'use strict';
	ruturn function(value = 40){
		return value;
	}
})
```

### name属性  返回该函数的函数名

匿名函数赋给变量，返回这个变量
具名函数赋给变了，返回这个具名函数原本的名字

Function构造函数返回的函数实例，返回anonymous
bind返回的函数，返回的属性值会加上bound前缀

```js
function foo(){}
foo.name  //foo

var f = function(){}
//es5
f.name  //''
//es6
f.name  //f

const f = function foo(){}
//es5
f.name  //foo
//es6
f.name  //foo

(new Function).name  //anonymous

function foo() {}
foo.bind({}).name  //bound foo

(function(){}).bind({}).name //'bound '
```

##### 箭头函数 =>

不需要参数，或需要多个参数，用一个圆括号代表参数部分
代码块部分多余1条语句，要使用大括号括起来，并且使用return语句返回
大括号{}代表代码块，如果要返回一个对象，必须在对象外面加上括号()

```js
var getTempItem = id => ({id: id, name: 'temp'})

//简化回调函数
[1, 2, 3].map(function(x){ return x*x })
[1, 2, 3].map(x => x * x);
```

* 函数体内的this对象，是定义时所在的对象，不是使用时所在的对象
* 不可用当做构造函数，即不可用使用new命令
* 不可使用arguments对象，该对象在函数体内不存在，用rest参数代替
* 不可用使用yield命令，不能用作Generator函数

```js
function foo() {
  setTimeout(() => {
    console.log('id:', this.id);
  }, 100);
}

var id = 21;

foo.call({ id: 42 });
// id: 42

function Timer() {
  this.s1 = 0;
  this.s2 = 0;
  // 箭头函数
  setInterval(() => this.s1++, 1000);
  // 普通函数
  setInterval(function () {
    this.s2++;
  }, 1000);
}

var timer = new Timer();

setTimeout(() => console.log('s1: ', timer.s1), 3100);
setTimeout(() => console.log('s2: ', timer.s2), 3100);
// s1: 3
// s2: 0
```
this指向的固定化
箭头函数根本没有自己的this，导致内部的this就是外层代码块的this
因为它没有this，所以也就不能用作构造函数
没有自己的this，也就不能用call()、apply()、bind()这些方法去改变this的指向
除了this，aguments， super，new.target也不存在，执行外层函数对应的变量

```js
function foo() {
  setTimeout(() => {
    console.log('args:', arguments);
  }, 100);
}

foo(2, 4, 6, 8)
// args: [2, 4, 6, 8]
```
部署管道机制（pipeline），即前一个函数的输出是后一个函数的输入

```js
const pipeline = (...funcs) => 
	val => funcs.reduce((a, b) => b(a), val);

const plus1 = a => a + 1;
const mult2 = a => a * 2;
const addThenMult = pipeline(plus1, mult2);
addThenMult(5)  //6
```
λ演算

```js
// λ演算的写法
fix = λf.(λx.f(λv.x(x)(v)))(λx.f(λv.x(x)(v)))

// ES6的写法
var fix = f => (x => f(v => x(x)(v)))
               (x => f(v => x(x)(v)));
```

##### 函数绑定 ::

左边一个是对象
右边一个是函数
该运算符会自动将左边的对象作为上下文环境(即this对象)，绑定到右边的函数上面

双冒号左边为空，右边是一个对象的方法，则等于将该方法绑定到该对象上面
双冒号返回的还是原对象，可以采用链式写法

```js
foo::bar(...arguments);
//等同于
bar.apply(foo, arguments)

const hasOwnProperty = Object.prototype.hasOwnProperty;
function hasOwn(obj, key){
	return obj::hasOwnProperty(key)
}

var method = ::obj.foo;  //等同于 method = obj::obj.foo
var log = ::console.log  //等同于 log = console.log.bind(console);

let {find, html} = jake;
document.querySelectorAll("div.myClass")
::find("p")
::html("hahaha")
```

##### 尾调用优化

尾调用 一个函数的最后一步是调用另一个函数，不再有其它操作
尾调用不一定出现在函数尾部，只要是最后一步操作即可
return fun()

尾调用 由于是函数最后一步操作
所以不需要保留外层函数的调用栈
```js
function f() {
  let m = 1;
  let n = 2;
  return g(m + n);
}
f();

// 等同于
function f() {
  return g(3);
}
f();

// 等同于
g(3);
```
尾调用每次执行时，调用帧只有一项，会大大节省内存
只层内部函数不再用到外层函数的内部变量，内层函数的调用帧才会取代外层函数的调用帧

尾递归 函数尾调用自身

递归非常消耗内存，因为需要同时保存成千上百个调用帧，很容易发生'栈溢出'错误
尾递归只有一个调用帧，不会发生'栈溢出'错误
```js
function Fibonacci(n){
	if(n <= 1) return 1;
	return Fibonacci(n-1) + Fibonacci(n - 2);
}
//Fibonacci(100)  堆栈溢出

//尾递归优化
function Fibonacci2(n, ac1 = 1, ac2 = 1){
	if(n <= 1) return ac2;
	return Fibonacci2(n - 1, ac2, ac1 + ac2)
}
```
正常模式下，函数内部有两个变量，可以跟踪函数的调用栈
* func.arguments 返回调用时函数的参数
* func.caller  返回调用当前函数的那个函数

严格模式禁止调用这两个变量
所以尾调用优化只有在严格模式下开启，正常模式无效

递归函数改写

所有内部变量改写成函数参数

```js
function factorial(n , total = 1){
	if(n === 1) return total;
	retrun factorial(n-1, n * total);
}

factorail(5)  //120
```

### 对象

##### 在对象之中直接写入变量和函数

属性名为变量名
属性值为变量值

方法简写

函数的参数为属性名
函数的参数值为属性值

```js
let foo = 'bar';
var baz = {foo};
baz  //{foo: 'bar'}

function f(x, y){
	return {x, y};
}
f(1, 2)  //{x: 1, y:2}

var o = {
	name: 'Lily',
	age: 18,
	method(){
		return 'hello'
	}// method: function(){}
}
```

##### 表达式作为对象属性名

表达式放在方括号之内

```js
let propkey = 'key';
let obj = {
	[propkey]: true,
	['a' + 'bc']: 123,
	['h' + 'ello'](){
		retrun 'hi'
	}
}

//属性名表达式与简洁表示法，不能同时使用

// 报错
var foo = 'bar';
var bar = 'abc';
var baz = { [foo] };

// 正确
var foo = 'bar';
var baz = { [foo]: 'abc'};

```

如果表达式是一个对象，会自动转为字符串[object object]

```js
let a = {a: 1};
let b = {b: 2};
let obj = {
	[a]: 'valuea',
	[b]: 'valueb'
}

obj  //{[object object]: 'valueb'}
//前面的[object object]被前面的覆盖

```

##### 方法的name属性返回函数名

bind方法创造的函数name属性返回bound加上原函数名
Function构造的函数name属性返回anonymous

```js
let person = {
	sayName(){
		console.log('Lily')
	}
}

person.sayName.name  //Lily
```

##### Object.is()

== 会自动转换数据类型
=== NaN不等于自身 +0 等于 -0

Object.is() 严格比较  NaN等于自身 +0不等于-0

```js
+0 === -0  //true
NaN === NaN  //false

Object.is(+0, -0)  //false
Object.is(NaN, NaN)  //true
```

##### Object.assign() 对象的合并

第一个参数是目标对象，后面的参数都是源对象
目标对象和源对象有同名属性，后面的属性会覆盖前面的属性
只有一个参数，会直接返回该参数
参数不是对象，会先转为对象
undefined和null无法转为对象，在首参数，会报错；非首参数会跳过
字符串会以数组的形式，拷贝入目标对象，其他值不会产生效果

浅拷贝，而不是深拷贝,源对象的某个属性是对象，只是拷贝到这个对象的引用，对象的任何变化，都会反映到目标对象上面
只拷贝源对象的自身属性（不拷贝继承属性），也不拷贝不可枚举的属性（enumerable: false）

```js
let obj = {a: 1};
Object.assign(obj, undefined) === obj // true
Object.assign(obj, null) === obj // true

var v1 = 'abc';
var v2 = true;
var v3 = 10;

var obj = Object.assign({}, v1, v2, v3);
console.log(obj); // { "0": "a", "1": "b", "2": "c" }

Object(true) // {[[PrimitiveValue]]: true}
Object(10)  //  {[[PrimitiveValue]]: 10}
Object('abc') 
// {0: "a", 1: "b", 2: "c", length: 3, [[PrimitiveValue]]: "abc"}
//内部属性[[PrimitiveValue]]不会被Object.assign拷贝

Object.assign([1, 2, 3], [4, 5])
// [4, 5, 3]
```

为对象添加属性
为对象添加方法
拷贝对象
合并多个对象
为属性指定默认值

```js
function processContent(options) {
  options = Object.assign({}, DEFAULTS, options);
  console.log(options);
  // ...
}
const DEFAULTS = {
  url: {
    host: 'example.com',
    port: 7070
  },
};

processContent({ url: {port: 8000} })
//存在深拷贝的问题,options.url会覆盖DEFAULTS.url，所以url.hot不存在了
```

##### 属性的可枚举性

每个对象都有一个描述对象Descriptor
Object.getOwnPropertyDescriptor获取属性的描述对象

```js
let obj = {foo: 123}

Object.gteOwnPropertyDescriptor(obj, 'foo')
/*
{
	value: 123,
	writable: true,
	enumerable: true
	configurable: true
}
*/
```
enumerable 可枚举性 为false，某些操作会怒略当前属性

es5有3个操作会忽略enumerable为false的属性

* for ... in循环 只遍历对象的自身和继承的可枚举属性
* Object.keys()  返回对象自身的所有可枚举属性的键名
* JSON.stringfy() 只串行化对象自身的可枚举的属性

es6

Object.assign() 会忽略enumerable为false的属性  只拷贝对象自身可枚举的属性

##### 属性的遍历

* for in  循环遍历对象自身和`继承`的可枚举属性(不含Symbol属性)
* Object.keys(obj)  返回一个数组，包括自身的(不含继承)所以可枚举属性(不含Symbol属性)
* Object.getOwnPropertyNames(obj)  返回一个数组，包含对象自身的所有属性(不含Symbol属性，但是包括不可枚举属性)
* Object.getOwnPropertySymbols(obj) 返回一个数组，包含对象自身的所有Symbol属性
* Reflect.ownKeys(obj)  返回一个数组，包含对象自身所有属性(包括Symbol，字符串，不可枚举)

都遵守

* 首先遍历所有属性名为数组的属性，按照数字排列
* 其次遍历所有属性为字符串的属性，按照生成时间排序
* 最后遍历所有属性名未Symbol值的属性，按照生成世界排序

##### __proto__属性

前后各2个下划线
读取或设置当前对象的prototype对象

只有浏览器支持
其它运行环境不支持

Object.setPrototypeOf()  设置一个对象的prototype对象，返回参数对象本身
Object.getPrototypeOf()  读取一个对象的原型对象
Object.create()  生成操作

##### Object.keys()  Object.values()  Object.entries()

Object.keys() 返回一个数组，成员是参数对象自身的(不含继承的)所有可遍历(enumerable)属性的键名
Object.values()  返回一个数组，成员是参数对象自身的(不含继承的)所有可遍历(enumerable)属性的键值
Object.entries() 返回一个数组，成员是参数对象自身的(不含继承的)所有可遍历(enumerable)属性的键值对数组

##### 对象的扩展运算符...

null和undefined无法转为对象，会报错
解构赋值必须是最后一个参数
解构赋值的拷贝是浅拷贝
解构赋值不会拷贝继承自原型对象的属性

```js
let {x, y, ...z} = {x:1, y:2, a: 3, b: 4}
x  //1
y  //2
z  //{a: 3, b: 4}

let {x, y, ...z} = null;  //error
let {x, y, ...z} = undefined;  //error

let {...x, y, z} = obj  //error
let {x, ...y, z} = boj  //error

var o = Object.create({ x: 1, y: 2 });
o.z = 3;
let { x, ...{ y, z } } = o;
x // 1  x是单纯的解构赋值，所以可以读取对象o继承的属性
y // undefined  y和z是双重解构赋值，只能读取对象o自身的属性
z // 3
```
... 取出参数对象的所有可遍历属性，拷贝到当前对象之中 等同于Object.assign()

```js
let z = { ..a}  //z = Object.assign({}, a)

let z = { ...a, ...b}  //z = Object.assign({}, a, b)

let z = { ...a, x: 1, y: 2}
// z = { ...a, ...{x: 1, y: 2}}
//let x = 1, y = 2, z = { ...a, x, y}
//z = Object.assign({}, a, {x: 1, y: 2})
```

##### Object.getOwnPropertyDescriptors() 返回对象所有自身属性(非继承属性)的描述对象

##### Null 传导运算符 ?.

只要返回null或undefined，就不再往下运算, 而是返回undefined

* obj?.prop  读取对象属性
* obj?.[expr]  读取对象属性
* func?.(...args)  函数或对象方法的调用
* new C?.(...args)  构造函数的调用

# Symbol  原始数据类型

独一无二
不能有new
接收一个字符串参数，不是字符串先转为字符串
参数只是表示对当前Symbol值的描述，相同参数的Symbol函数的返回值不相等

不能与其他类型的值精选运算
可以显式转为字符串
可以转为布尔值，但是不能转为数值

```js
let s = Symbol()
typeof s  //symbol

var s1 = Symbol();
var s2 = Symbol();
s1 === s2  //false

var s1 = Symbol('foo')
var s2 = Symbol('foo')
s1 === s2  //false

var sym = Symbol('Lily');
String(sym)  //'Symbol(Lily)'
sym.toString()  //'Symbol(Lily)'

var sym = Symbol();
Boolean(sym)  //true
!sym  //false

Number(sym)  //typeError
sym + 2  //typeError
```
 


