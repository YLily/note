#　vue		

Vue.js 不支持 IE8 及其以下版本，因为 Vue.js 使用了 IE8 不能模拟的 ECMAScript 5 特性。 Vue.js 支持所有兼容 ECMAScript 5 的浏览器。		
开发版本包含完整的警告和调试模式		
```
	<script src="https://unpkg.com/vue/dist/vue.js"></script>		
	npm install vue		
```
			
#### 独立构建	
包含模板编译器	
用于编译 Vue 模板字符串成纯 JavaScript 渲染函数。如果你想用 template 选项， 你需要编译	

#### 运行构建	
不包含模板编译器	
默认 NPM 包导出的是 运行时 构建	

使用独立构建，在 webpack 配置中添加下面的别名
```	
	resolve: {
	  alias: {
	    'vue$': 'vue/dist/vue.js'
	  }
	}	
```
`不要用` import Vue from 'vue/dist/vue.js' 	
用一些工具或第三方库引入 Vue ，这可能会导致应用程序在同一时间加载运行时和独立构建并造成错误	
			
### CSP 环境	
有些环境，如 Google Chrome Apps ，强制应用内容安全策略 (CSP) ，不能使用 new Function() 对表达式求值。这时可以用 CSP 兼容版本。独立的构建取决于该功能编译模板，所以无法使用这些环境。		
运行时构建的是完全兼容 CSP 的		
			
###Vue.js	
构建用户界面的 渐进式框架， 采用自底向上增量开发的设计，Vue 的核心库只关注视图层		
			
### 构造器	
每个 Vue.js 应用都是通过构造函数 Vue 创建一个 Vue 的根实例 启动的	
```
var vm = new Vue({
  // 选项
})
```
扩展 Vue 构造器	
```
var MyComponent = Vue.extend({
  // 扩展选项
})
//所有的 `MyComponent` 实例都将以预定义的扩展选项被创建
var myComponentInstance = new MyComponent()
```
属性与方法	

* data 属性：只有这些被代理的属性是响应的。如果在实例创建之后添加新的属性到实例上，它不会触发视图更新
* **$**：除了data 属性，其它属性与方法都有前缀**$**，以便与代理的 data 属性区分
* 不要在实例属性或者回调函数中（如 vm.$watch('a', newVal => this.myMethod())）使用箭头函数	
* 因为箭头函数绑定父上下文，所以 this 不会像预想的一样是 Vue 实例，而是 this.myMethod 未被定义。	
			
### 插值

##### 文本	

数据绑定最常见的形式就是使用 “Mustache” 语法（双大括号）的文本插值	
Mustache 标签将会被替代为对应数据对象上 msg 属性的值。无论何时，绑定的数据对象上 msg 属性发生了改变，插值处的内容都会更新	
v-once 指令，执行一次性地插值，当数据改变时，插值处的内容不会更新。但请留心这会影响到该节点上所有的数据绑定	

##### 纯 HTML	

双大括号会将数据解释为纯文本，而非 HTML 	
v-html 指令，输出真正的 HTML	
不能使用 v-html 来复合局部模板，因为 Vue 不是基于字符串的模板引擎	
你的站点上动态渲染的任意 HTML 可能会非常危险，因为它很容易导致 XSS 攻击。请只对可信内容使用 HTML 插值，绝不要对用户提供的内容插值。	

##### 属性	

v-bind 指令	
布尔值的属性 —— 如果条件被求值为 false 的话该属性会被移除	

##### JavaScript 表达式	

表达式会在所属 Vue 实例的数据作用域下作为 JavaScript 被解析	
每个绑定都只能包含单个表达式	

##### 过滤器	

被添加在 mustache 插值的尾部，由“管道符”指示	
过滤器函数总接受表达式的值作为第一个参数	
过滤器可以串联	
过滤器是 JavaScript 函数，因此可以接受参数	

### 指令	

指令（Directives）是带有 v- 前缀的特殊属性		
指令属性的值预期是单一 JavaScript 表达式		
指令的职责就是当其表达式的值改变时相应地将某些行为应用到 DOM 上		
参数

* v-bind 指令能接受一个“参数”，在指令后以冒号指明	被用来响应地更新 HTML 属性
* v-on 指令	监听 DOM 事件

修饰符	

* 修饰符（Modifiers）是以半角句号 . 指明的特殊后缀，用于指出一个指定应该以特殊方式绑定	
* .prevent 修饰符告诉 v-on 指令对于触发的事件调用 event.preventDefault()	

缩写	

* v-bind 缩写	
```
	<a v-bind:href="url"></a>
	<a :href=url></a><!-- 缩写 -->
```
* v-on 缩写	
```
	<a v-on:click="doSomething"></a>
	<a @click="doSomething"></a><!-- 缩写 -->
```

###　计算属性	

在模板中绑定表达式是非常便利的，但是它们实际上只用于简单的操作。在模板中放入太多的逻辑会让模板过重且难以维护		
任何复杂逻辑，都应当使用计算属性		
计算缓存 vs Methods	

* 不经过计算属性，我们可以在 method 中定义一个相同的函数来替代它	
* 计算属性是基于它的依赖缓存。计算属性只有在它的相关依赖发生改变时才会重新取值	
* method 调用总会执行函数	
* 不希望有缓存，用 method 替代

计算属性 vs Watched Property	

* $watch ，它用于观察 Vue 实例上的数据变动	
* 使用计算属性而不是一个命令式的 $watch 回调	
* 计算 setter	计算属性默认只有 getter ，不过在需要时也可以提供一个 setter 	
			
### 观察 Watchers	

计算属性在大多数情况下更合适，但有时也需要一个自定义的 watcher 		
在数据变化响应时，执行异步操作或昂贵操作		
			
### 绑定 HTML Class	

##### 对象语法	

给 v-bind:class 一个对象，以动态地切换 class	
可以在对象中传入更多属性用来动态切换多个 class 	
v-bind:class 指令可以与普通的 class 属性共存	
可以绑定返回对象的计算属性

##### 数组语法	

可以把一个数组传给 v-bind:class ，以应用一个 class 列表	
可以用三元表达式，切换列表中的 class 	
可以在数组语法中使用对象语法


### 绑定内联样式	
##### 对象语法	

v-bind:style 的对象语法是一个 JavaScript 对象	
CSS 属性名可以用驼峰式（camelCase）或短横分隔命名（kebab-case）	
对象语法常常结合返回对象的计算属性使用	

##### 数组语法	

v-bind:style 的数组语法可以将多个样式对象应用到一个元素上	

##### 自动添加前缀	

当 v-bind:style 使用需要特定前缀的 CSS 属性时，如 transform ，Vue.js 会自动侦测并添加相应的前缀	

### 条件渲染 v-if	

v-else 指令给 v-if 或 v-show 添加一个 “else” 块,v-else 元素必须紧跟在 v-if 或 v-show 元素的后面——否则它不能被识别	
template v-if 把一个 `<template>` 元素当做包装元素，并在上面使用 v-if，最终的渲染结果不会包含它	
v-show	

* 有 v-show 的元素会始终渲染并保持在 DOM 中。v-show 是简单的切换元素的 CSS 属性 display	
* v-show 不支持 `<template>` 语法	

v-if 是真实的条件渲染，因为它会确保条件块在切换当中适当地销毁与重建条件块内的事件监听器和子组件		
v-if 也是惰性的：如果在初始渲染时条件为假，则什么也不做——在条件第一次变为真时才开始局部编译（编译会被缓存起来）		
v-show 简单得多——元素始终被编译并保留，只是简单地基于 CSS 切换		
v-if 有更高的切换消耗而 v-show 有更高的初始渲染消耗		

### 列表渲染 v-for

v-for 指令根据一组数组的选项列表进行渲染		
v-for 指令需要以 item in items 形式的特殊语法， items 是源数据数组并且 item 是数组元素迭代的别名		
在 v-for 块中，我们拥有对父作用域属性的完全访问权限		
v-for 还支持一个可选的第二个参数为当前项的索引(item, index) in items		
可以用 of 替代 in 作为分隔符，因为它是最接近 JavaScript 迭代器的语法		
Template v-for	可以用带有 v-for 的 `<template>` 标签来渲染多个元素块	
对象迭代 v-for

* 可以用 v-for 通过一个对象的属性来迭代value in object	
* 可以提供第二个的参数为键名(value, key) in object	
* 第三个参数为索引(value, key, index) in object	

整数迭代 v-for	n in 10	

组件 和 v-for	

* 不能自动传递数
据到组件里，因为组件有自己独立的作用域。为了传递迭代数据到组件里，我们要用 props	
* 不自动注入 item 到组件里的原因是，因为这使得组件会紧密耦合到 v-for 如何运作	

key	

* 用 v-for 正在更新已渲染过的元素列表时，它默认用 “就地复用” 策略	
* 如果数据项的顺序被改变，而不是移动 DOM 元素来匹配数据项的顺序， Vue 将简单复用此处每个元素，并且确保它在特定索引下显示已被渲染过的每个元素	
* 只适用于不依赖子组件状态或临时 DOM 状态（例如：表单输入值）的列表渲染输出	
* 为了重用和重新排序现有元素，你需要为每项提供一个唯一 key 属性	
* 需要用 v-bind 来绑定动态值	

### 数组更新检测	

##### 变异方法(mutation method):会改变被这些方法调用的原始数组
```
push()
pop()
shift()
unshift()
splice()
sort()
reverse()
```

##### 非变异方法(non-mutating method):不会改变原始数组，但总是返回一个新数组
```
filter()
concat()
slice()	
```
**注意事项**:由于 JavaScript 的限制， Vue 不能检测以下变动的数组	

当你直接设置一个项的索引时，例如： 
```
vm.items[indexOfItem] = newValue	
Vue.set(example1.items, indexOfItem, newValue)
example1.items.splice(indexOfItem, 1, newValue)
```
当你修改数组的长度时，例如： 
```
vm.items.length = newLength	
example1.items.splice(newLength)
```
##### 显示过滤/排序结果	

创建返回过滤或排序数组的计算属性	
计算属性是不可行的 method 方法	

事件处理器	

监听事件v-on		
方法事件处理器	v-on 接收一个定义的方法来调用	
内联处理器方法	

* 除了直接绑定到一个方法，也可以用内联 JavaScript 语句	
* 需要在内联语句处理器中访问原生 DOM 事件。可以用特殊变量 $event 把它传入方法	

事件修饰符	

* methods 只有纯粹的数据逻辑，而不是去处理 DOM 事件细节	
* 通过由点(.)表示的指令后缀来调用修饰符	
* .stop    	event.stopPropagation()
* .prevent	event.preventDefault() 
* .capture	添加事件侦听器时使用事件捕获模式
* .self	只当事件在该元素本身（而不是子元素）触发时触发回调
*  修饰符可以串联	

按键修饰符	

* .keyCode	
* 全部的按键别名
```	
enter 
tab 
delete (捕获 “删除” 和 “退格” 键) 
esc 
space 
up 
down 
left 
right
```
* 通过全局 config.keyCodes 对象自定义按键修饰符别名	

	Vue.config.keyCodes.f1 = 112

ViewModel 代码可以是非常纯粹的逻辑，和 DOM 完全解耦，更易于测试		
当一个 ViewModel 被销毁时，所有的事件处理器都会自动被删除	

### 表单控件绑定	
v-model 指令在表单控件元素上创建双向数据绑定,它会根据控件类型自动选取正确的方法来更新元素	
v-model 并不关心表单控件初始化所生成的值,因为它会选择 Vue 实例数据来作为具体的值。		
文本		
多行文本：在文本区域插值( `<textarea></textarea>` ) 并不会生效，应用 v-model 来代替	
复选框：单个勾选框，逻辑值，多个勾选框，绑定到同一个数组	
单选按钮		
选择列表：多选列表（绑定到一个数组）,动态选项，用 v-for 渲染
绑定 value

* 对于单选按钮，勾选框及选择列表选项， v-model 绑定的 value 通常是静态字符串（对于勾选框是逻辑值）	
* 可以用 v-bind绑定 value 到 Vue 实例的一个动态属性上，且这个属性的值可以不是字符串	
修饰符	

* .lazy	v-model 在 input 事件中同步输入框的值与数据，添加lazy 转变为在 change 事件中同步
* .number	自动将用户的输入值转为 Number 类型（如果原值的转换结果为 NaN 则返回原值）
* .trim	自动过滤用户输入的首尾空格

###　组件	
组件可以扩展 HTML 元素，封装可重用的代码。在较高层面上，组件是自定义元素， Vue.js 的编译器为它添加特殊功能。在有些情况下，组件也可以是原生 HTML 元素的形式，以 is 特性扩展		

**注册**	Vue.component(tagName, options)	注册一个全局组件
组件在注册之后，便可以在父实例的模块中以自定义元素 `<my-component></my-component>` 的形式使用。要确保在初始化根实例 之前 注册了组件		
**局部注册** 不必在全局注册每个组件。通过使用组件实例选项注册，可以使组件仅在另一个实例/组件的作用域中可用	

DOM 模版解析	

会受到 HTML 的一些限制，因为 Vue 只有在浏览器解析和标准化 HTML 后才能获取模版内容	
使用特殊的 is 属性加载自定义组件	
```
<table>
	<tr is="my-row"></tr>
</table>
```
使用其一字符串模板，这些限制将不适用	

* `<script type="text/x-template">`
* JavaScript内联模版字符串
* .vue 组件

data 必须是函数	多个组件共享了同一个 data ，改变data会影响所有组件！我们可以通过为每个组件返回新的 data 对象来解决这个问题	
构成组件	

* 父组件通过 props 向下传递数据给子组件	
* 子组件通过 events 给父组件发送消息

### Props	

使用Props传递数据	

* 组件实例的作用域是孤立的，不应该在子组件的模板内直接引用父组件的数据
* prop 是父组件用来传递数据的一个自定义属性。子组件需要显式地用 props 选项 声明 “prop”	

camelCase vs. kebab-case	HTML 特性不区分大小写。当使用非字符串模版时，prop的名字形式会从 camelCase 转为 kebab-case（短横线隔开）	
动态 Props	可以用 v-bind 动态绑定 props 的值到父组件的数据中。每当父组件的数据变化时，该变化也会传导给子组件	
字面量语法 vs 动态语法	需要使用 v-bind ，让它的值被当作 JavaScript 表达式计算	
单向数据流	

* prop 是单向绑定的：当父组件的属性变化时，将传导给子组件，但是不会反过来	
* 每次父组件更新时，子组件的所有 prop 都会更新为最新值。这意味着你不应该在子组件内部改变 prop 	
* 两种改变 prop 的情况	prop 作为初始值传入，子组件之后只是将它的初始值作为本地数据的初始值使用(定义一个局部 data 属性，并将 prop* 的初始值作为局部数据的初始值)
* prop 作为需要被转变的原始值传入(定义一个 computed 属性，此属性从 prop 的值计算得出)
* 注意在 JavaScript 中对象和数组是引用类型，指向同一个内存空间，如果 prop 是一个对象或数组，在子组件内部改变它会影响父组件的状态	

Prop 验证	

* prop 是一个对象而不是字符串数组时，它包含验证要求	
* type 可以是原生构造器	`String 、Number 、Boolean 、Function 、Object 、Array`
* type 也可以是一个自定义构造器，使用 instanceof 检测	
* 当 prop 验证失败了， Vue 将拒绝在子组件上设置此值，如果使用的是开发版本会抛出一条警告	

### 自定义事件	

使用 v-on 绑定自定义事件	

* $on(eventName) 监听事件	
* $emit(eventName) 触发事件	

父组件可以在使用子组件的地方直接用 v-on 来监听子组件触发的事件		
给组件绑定原生事件	使用 .native 修饰 v-on	
使用自定义事件的表单输入组件	

* 自定义事件也可以用来创建自定义的表单输入组件，使用 v-model 来进行数据双向绑定	
* 要让组件的 v-model 生效	
* * 接受一个 value 属性
* * 在有新的 value 时触发 input 事件
* 非父子组件通信	非父子关系的组件通信,在简单的场景下，使用一个空的 Vue 实例作为中央事件总线	

###　使用 Slots 分发内容	

`<app>` 组件不知道它的挂载点会有什么内容。挂载点的内容是由`<app>`的父组件决定的		
`<app>` 组件很可能有它自己的模版		
为了让组件可以组合，我们需要一种方式来混合父组件的内容与子组件自己的模板。这个过程被称为 内容分发，使用特殊的 `<slot>` 元素作为原始内容的插槽		
编译作用域	父组件模板的内容在父组件作用域内编译；子组件模板的内容在子组件作用域内编译。	
单个 Slot

* 子组件模板包含至少一个 <slot> 插口，否则父组件的内容将会被丢弃	
* 当子组件模板只有一个没有属性的 slot 时，父组件整个内容片段将插入到 slot 所在的 DOM 位置，并替换掉 slot 标签本身	

具名Slots	

* <slot> 元素可以用一个特殊的属性 name 来配置如何分发内容。多个 slot 可以有不同的名字。具名 slot 将匹配内容片段中有对应 slot 特性的元素	
* 仍然可以有一个匿名 slot ，它是默认 slot ，作为找不到匹配的内容片段的备用插槽。如果没有默认的 slot ，这些找不到匹配的内容片段将被抛弃	

### 动态组件	

多个组件可以使用同一个挂载点，然后动态地在它们之间切换。使用保留的 `<component>` 元素，动态地绑定到它的 is 特性		
keep-alive	如果把切换出去的组件保留在内存中，可以保留它的状态或避免重新渲染。为此可以添加一个 keep-alive 指令参数	

### 杂项	

##### 编写可复用组件	

Vue 组件的 API 来自三部分 - props, events 和 slots 	Props 允许外部环境传递数据给组件
Events 允许组件触发外部环境的副作用
Slots 允许外部环境将额外的内容组合在组件中。

##### 子组件索引	

尽管有 props 和 events ，但是有时仍然需要在 JavaScript 中直接访问子组件。为此可以使用 ref 为子组件指定一个索引 ID 	
当 ref 和 v-for 一起使用时， ref 是一个数组或对象，包含相应的子组件	
`$refs 只在组件渲染完成后才填充，并且它是非响应式的。它仅仅作为一个直接访问子组件的应急方案——应当避免在模版或计算属性中使用 $refs 	

##### 异步组件	

将组件定义为一个工厂函数，动态地解析组件的定义。Vue.js 只在组件需要渲染时触发工厂函数，并且把结果缓存起来，用于后面的再次渲染	
工厂函数接收一个 resolve 回调，在收到从服务器下载的组件定义时调用。也可以调用 reject(reason) 指示加载失败	

##### 组件命名约定	

当注册组件（或者 props）时，可以使用 kebab-case ，camelCase ，或 TitleCase 	
在 HTML 模版中，请使用 kebab-case 形式	
当使用字符串模式时，可以不受 HTML 的 case-insensitive 限制。这意味实际上在模版中，你可以使用 camelCase 、 PascalCase 或者 kebab-cas来引用你的组件和 prop	
如果组件未经 slot 元素传递内容，你甚至可以在组件名后使用 / 使其自闭合	

##### 递归组件	

只有当它有 name 选项时才可以在它的模板内递归地调用自己	
当你利用Vue.component全局注册了一个组件, 全局的ID作为组件的 name 选项，被自动设置.	
要确保递归调用有终止条件 (比如递归调用时使用 v-if 并让他最终返回 false )	

##### 内联模版	

如果子组件有 inline-template 特性，组件将把它的内容当作它的模板，而不是把它当作分发内容。	
inline-template 让模板的作用域难以理解。最佳是使用 template 选项在组件内定义模板或者在 .vue 文件中使用 template 元素	

##### X-Templates	

在 JavaScript 标签里使用 text/x-template 类型，并且指定一个id	
```
<script type="text/x-template" id="hello-world-template">
	<p>Hello hello hello</p>
</script>
```
它将模版和组件的其他定义隔离了，在有很多模版或者小的应用中有用，否则应该避免使用

##### v-once 的低级静态组件(Cheap Static Component)	

当组件中包含大量静态内容时，可以考虑使用 v-once 将渲染结果缓存起来	
			
### 响应式原理	

Vue 不能检测到对象属性的添加或删除		
Vue 不允许在已经创建的实例上动态添加新的根级响应式属性（root-level reactive properties）。然而它可以使用 Vue.set(object, key, value)方法将响应属性添加到嵌套的对象上		
必须在初始化实例前声明根级响应式属性，哪怕只是一个空值		
您还可以使用 vm.$set 实例方法，这也是全局 Vue.set 方法的别名		
向已有对象上添加一些属性，使用 Object.assign() 或 _.extend() 方法来添加属性。但是，添加到对象上的新属性不会触发更新		
Vue 执行 DOM 更新是异步的，只要观察到数据变化，Vue 就开始一个队列，将同一事件循环内所有的数据变化缓存起来。如果一个 watcher被多次触发，只会推入一次到队列中。在事件循环中，Vue 刷新队列并仅执行必要的 DOM 更新		
在数据变化之后立即使用 Vue.nextTick(callback) ，回调在 DOM 更新完成后就会调用，它的回调 this 将自动绑定到当前的 Vue 实例上		
			
### 过渡效果	

#### 单元素/组件的过渡	

Vue 提供了 transition 的封装组件，可以给任何元素和组件添加 entering/leaving 过渡	

* 条件渲染 （使用 v-if）
* 条件展示 （使用 v-show）
* 动态组件
* 组件根节点	

#### 过渡的-CSS-类名

4 个(CSS)类名在 enter/leave 的过渡中切换	

* v-enter: 定义进入过渡的开始状态。在元素被插入时生效，在下一个帧移除。
* v-enter-active: 定义进入过渡的结束状态。在元素被插入时生效，在transition/animation 完成之后移除。
* v-leave: 定义离开过渡的开始状态。在离开过渡被触发时生效，在下一个帧移除。
* v-leave-active: 定义离开过渡的结束状态。在离开过渡被触发时生效，在transition/animation 完成之后移除。

v- 是这些类名的前缀。使用 `<name="my-transition">` 可以重置前缀， v-enter 替换为 my-transition-enter	

##### CSS 过渡	transition	

##### CSS 动画	animation	

在动画中 v-enter 类名在节点插入 DOM 后不会立即删除，而是在 animationend 事件触发时删除

##### 自定义过渡类名(优先级高于普通的类名)	

* enter-class	
* enter-active-class	
* leave-class	
* leave-active-class	

##### 同时使用 Transitions 和 Animations	

Vue 为了知道过渡的完成，必须设置相应的事件监听器。它可以是 transitionend 或 animationend 	
同一个元素同时设置两种过渡动效，使用 type 特性并设置 animation 或 transition 来明确声明你需要 Vue 监听的类型	

##### JavaScript 钩子	
```
v-on:before-enter="beforeEnter"
v-on:enter="enter"
v-on:after-enter="afterEnter"
v-on:enter-cancelled="enterCancelled"

v-on:before-leave="beforeLeave"
v-on:leave="leave"
v-on:after-leave="afterLeave"
v-on:leave-cancelled="leaveCancelled"	
```
当只用 JavaScript 过渡的时候， 在 enter 和 leave 中，回调函数 done 是必须的 。 否则，它们会被同步调用，过渡会立即完成。	
对于仅使用 JavaScript 过渡的元素添加 v-bind:css="false"，Vue 会跳过 CSS 的检测。这也可以避免过渡过程中 CSS 的影响	

#### 初始渲染的过渡	

通过 appear 特性设置节点的在初始渲染的过渡		
自定义 CSS 类名	
```
appear-class="custom-appear-class"
appear-active-class="custom-appear-active-class"
```
自定义 JavaScript 钩子	
```
v-on:before-appear="customBeforeAppearHook" 
v-on:appear="customAppearHook"
v-on:after-appear="customAfterAppearHook"
```

#### 多个元素的过渡	
原生标签可以使用 v-if/v-else		
当有相同标签名的元素切换时，需要通过 key 特性设置唯一的值来标记以让 Vue 区分它们，否则 Vue为了效率只会替换相同标签内部的内容。即使在技术上没有必要，给在 `<transition> `组件中的多个元素设置 key 是一个更好的实践		
也可以给通过给同一个元素的 key 特性设置不同的状态来代替 v-if 和 v-else		
过渡模式	

* 同时生效的进入和离开的过渡不能满足所有要求，所以 Vue 提供了 过渡模式	
* in-out: 新元素先进行过渡，完成之后当前元素过渡离开
* out-in: 当前元素先进行过渡，完成之后新元素过渡进入

#### 多个组件的过渡	
只需要使用动态组件	不需要使用 key 特性	

#### 列表过渡	

使用 `<transition-group>` 组件	

* 以一个真实元素呈现：默认为一个 <span>。你也可以通过 tag 特性更换为其他元素	
* 元素 一定需要 指定唯一的 key 特性值	

###### 列表的位移过渡	 v-move 特性	改变定位
* 可以通过 name 属性来自定义前缀，也可以通过 move-class 属性手动设置
* v-move 对于设置过渡的切换时机和过渡曲线非常有用
* 内部的实现，Vue 使用了一个叫 FLIP 简单的动画队列
* 使用 transforms 将元素从之前的位置平滑过渡新的位置
* 使用 FLIP 过渡的元素不能设置为 display: inline 。作为替代方案，可以设置为 display: inline-block 或者放置于 flex 中
* FLIP 动画不仅可以实现单列过渡，多维网格的过渡也同样简单

###### 列表的渐进过渡	
通过 data 属性与 JavaScript 通信 ，就可以实现列表的渐进过渡	

#### 可复用的过渡	

过渡可以通过 Vue 的组件系统实现复用		
将 `<transition>` 或者 `<transition-group>` 作为根组件，然后将任何子组件放置在其中就可以了	

#### 动态过渡	

动态过渡最基本的例子是通过 name 特性来绑定动态值		
所有的过渡特性都是动态绑定		
通过事件的钩子函数方法，可以在获取到相应上下文数据，可以根据组件的状态通过 JavaScript 过渡设置不同的过渡效果	

#### 过渡状态动态状态转换通过组件组织过渡

状态动画 与 watcher,通过 watcher 能监听到任何数值属性的数值更新	
像 Vue 的过渡组件一样，数据背后状态转换会实时更新		
					
### Render 函数	

Vue 推荐使用在绝大多数情况下使用 template 来创建你的 HTML		
render 函数，比 template 更接近编译器

* createElement 参数	
* 完整数据对象	
* 约束	
* - VNodes 必须唯一
* - 需要重复很多次的元素/组件，你可以使用工厂函数来实现

无论什么都可以使用原生的 JavaScript 来实现	template 中的 v-if 和 v-for在 render 函数中被 JavaScript 的 if/else 和 map 重写	
在 Vue 中使用 JSX 语法		

### 函数化组件	
标记组件为 functional， 这意味它是无状态（没有 data），无实例（没有 this 上下文）		
组件需要的一切都是通过上下文传递	

* props: 提供props 的对象
* children: VNode 子节点的数组
* slots: slots 对象
* data: 传递给组件的 data 对象
* parent: 对父组件的引用	

添加 functional: true 之后，锚点标题组件的 render 函数之间简单更新增加 context 参数，this.$slots.default 更新为 context.children，之后this.level 更新为 context.props.level		
函数化组件只是一个函数，所以渲染开销也低很多。但同样它也有完整的组件封装	

* 程序化地在多个组件中选择一个
* 在将 children, props, data 传递给子组件之前操作它们

slots() 和 children 对比	

* 同时拥有 children 和 slots() ，可以选择让组件通过 slot() 系统分发或者简单的通过 children 接收，让其他组件去处理

模板编译	Vue 的模板实际是编译成了 render 函数

### 自定义指令	
需要对纯 DOM 元素进行底层操作,会用到自定义指令	Vue.directive	
注册局部指令，组件中接受一个 directives 的选项	directives	
钩子函数	

* bind: 只调用一次，指令第一次绑定到元素时调用，用这个钩子函数可以定义一个在绑定时执行一次的初始化动作。	
* inserted: 被绑定元素插入父节点时调用（父节点存在即可调用，不必存在于 document 中）。	
* update: 被绑定元素所在的模板更新时调用，而不论绑定值是否变化。通过比较更新前后的绑定值，可以忽略不必要的模板更新	
* componentUpdated: 被绑定元素所在模板完成一次更新周期时调用。	
* unbind: 只调用一次， 指令与元素解绑时调用。	

钩子函数参数	

* el: 指令所绑定的元素，可以用来直接操作 DOM 	
* binding: 一个对象	name: 指令名，不包括 v- 前缀
* * value: 指令的绑定值， 例如： v-my-directive="1 + 1", value 的值是 2
* * oldValue: 指令绑定的前一个值，仅在 update 和 componentUpdated 钩子中可用。无论值是否改变都可用
* * expression: 绑定值的字符串形式。 例如 v-my-directive="1 + 1" ， expression 的值是 "1 + 1"
* * arg: 传给指令的参数。例如 v-my-directive:foo， arg 的值是 "foo"
* * modifiers: 一个包含修饰符的对象。 例如： v-my-directive.foo.bar, 修饰符对象 modifiers 的值是 { foo: true, bar: true }
* vnode: Vue 编译生成的虚拟节点	
* oldVnode: 上一个虚拟节点，仅在 update 和 componentUpdated 钩子中可用	

函数简写	在 bind 和 update 钩子上做重复动作，并且不想关心其它的钩子函数	省略 bind:和update:
对象字面量	指令需要多个值，可以传入一个 JavaScript 对象字面量。指令函数能够接受所有合法类型的 Javascript 表达式	

### 混合	
是一种灵活的分布式复用 Vue 组件的方式		
混合对象可以包含任意组件选项。以组件使用混合对象时，所有混合对象的选项将被混入该组件本身的选项		
选项合并	

* 当组件和混合对象含有同名选项时，这些选项将以恰当的方式混合	
* + 同名钩子函数将混合为一个数组，因此都将被调用
* + 混合对象的 钩子将在组件自身钩子 之前 调用 
* 值为对象的选项，例如 methods, components 和 directives，将被混合为同一个对象。 两个对象键名冲突时，取组件对象的键值对	
* Vue.extend() 也使用同样的策略进行合并	

全局混合	

* 全局注册混合对象	
* 一旦使用全局混合对象，将会影响到 所有 之后创建的 Vue 实例（包括第三方模板）	
* 使用恰当时，可以为自定义对象注入处理逻辑	
* 大多数情况下，只应当应用于自定义选项，也可以将其用作 Plugins 以避免产生重复应用	

自定义选项混合策略	

* 自定义选项将使用默认策略，即简单地覆盖已有值	
* 以自定义逻辑混合，向 Vue.config.optionMergeStrategies 添加一个函数	
* 大多数对象选项，可以使用 methods 的合并策略	

### 插件	
插件的范围没有限制	

* 添加全局方法或者属性，如: vue-element	
* 添加全局资源：指令/过滤器/过渡等，如 vue-touch	
* 通过全局 mixin方法添加一些组件选项，如: vuex	
* 添加 Vue 实例方法，通过把它们添加到 Vue.prototype 上实现。	
* 一个库，提供自己的 API，同时提供上面提到的一个或多个功能，如 vue-router

Vue.js 的插件应当有一个公开方法 install 。这个方法的第一个参数是 Vue 构造器 , 第二个参数是一个可选的选项对象		
使用插件	

* 全局方法 Vue.use() 使用插件	也可以传入一个选项对象
* Vue.use 会自动阻止注册相同插件多次，届时只会注册一次该插件	
* vue-router 如果 Vue 是全局变量则自动调用 Vue.use() 。不过在模块环境中应当始终显式调用 Vue.use()	

### 单文件组件	

使用 JavaScript 增强某个视图的中小型项目	

* 用 Vue.component 来定义全局组件，紧接着用 new Vue({ el: '#container '}) 在每个页面内指定一个容器元素	

更复杂的项目,弊端	

* 全局定义(Global definitions) 强制要求每个 component 中的命名不得重复	
* 字符串模板(String templates) 缺乏语法高亮，在 HTML 有多行的时候，需要用到丑陋的 \	
* 不支持CSS(No CSS support) 意味着当 HTML 和 JavaScript 组件化时，CSS 明显被遗漏	
* 没有构建步骤(No build step) 限制只能使用 HTML 和 ES5 JavaScript, 而不能使用预处理器，如 Pug (formerly Jade) 和 Babel	

文件扩展名为 .vue 的 single-file components(单文件组件) 	

* 完整语法高亮
* CommonJS 模块
* 组件化的 CSS

还可以使用 Webpack 或 Browserify 等构建工具
使用预处理器来构建简洁和功能更丰富的组件，比如 Jade，Babel (with ES2015 modules)，和 Stylus

### 路由	
官方支持的vue-router		
只需要非常简单的路由而不需要引入整个路由库，可以动态渲染一个页面级的组件		
整合第三方路由如Page.js或者 Director	

### 状态管理	
类 Flux 状态管理，Vue 提供 vuex		

