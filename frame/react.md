# React	

* <script type="text/babel"></script>	 React 独有的 JSX 语法，跟 JavaScript 不兼容	
* react.js	react核心库	
* react-dom.js	提供与dom相关功能	
* browser.js	将jsx语法转换成javascript语法(比较耗时，实际上线放到服务器完成)	 babel src --out-dir build
		
ReactDOM.render();	将模板转为 HTML 语言，并插入指定的 DOM 节点	
			
### jsx语法	
HTML 语言直接写在 JavaScript 语言之中，不加任何引号		

* 遇到 HTML 标签（以 < 开头），就用 HTML 规则解析		
* 遇到代码块（以 { 开头），就用 JavaScript 规则解析		
* JSX 允许直接在模板插入 JavaScript 变量。如果这个变量是一个数组，则会展开这个数组的所有成员	{变量名}	

### 组件（component）	
React 允许将代码封装成组件（component），然后像插入普通 HTML 标签一样，在网页中插入这个组件	

* React.createClass({}) 方法就用于生成一个组件类	
* 所有组件类都必须有自己的 render 方法，用于输出组件	
* 组件类的第一个字母必须大写，否则会报错	React Component：upper-case letter
* 组件类只能包含一个顶层标签，否则也会报错	
* 组件的用法与原生的 HTML 标签完全一致，可以任意加入属性	
* class 属性需要写成 className	保留字和关键字
* for 属性需要写成 htmlFor	
* 组件的属性可以在组件类的 this.props 对象上获取	

#### this.props 对象的属性与组件的属性一一对应	**this.props.children**例外	

* this.props.children表示组件的所有子节点	
* 如果当前组件没有子节点，它就是 undefined
* 如果有一个子节点，数据类型是 object
* 如果有多个子节点，数据类型就是 array
* React.Children 来处理 this.props.children	
* React.Children.map 来遍历子节点	

### React.PropTypes

组件的属性可以接受任意值，字符串、对象、函数等等都可以	
用来验证组件实例的属性是否符合要求	
```javascript
	optionalArray: React.PropTypes.array,
	optionalBool: React.PropTypes.bool,
	optionalFunc: React.PropTypes.func,
	optionalNumber: React.PropTypes.number,
	optionalObject: React.PropTypes.object,
	optionalString: React.PropTypes.string,
	optionalSymbol: React.PropTypes.symbol,
	optionalNode: React.PropTypes.node,  
	optionalMessage: React.PropTypes.instanceOf(Message),  
	optionalEnum: React.PropTypes.oneOf(['News', 'Photos']),  
	optionalUnion: React.PropTypes.oneOfType([
	    React.PropTypes.string,
	    React.PropTypes.number,
	    React.PropTypes.instanceOf(Message)
	 ]),
	 optionalArrayOf: React.PropTypes.arrayOf(React.PropTypes.number),
	 optionalObjectOf: React.PropTypes.objectOf(React.PropTypes.number),

	 optionalObjectWithShape: React.PropTypes.shape({
	  color: React.PropTypes.string,
	  fontSize: React.PropTypes.number
	}),
	requiredFunc: React.PropTypes.func.isRequired,
	requiredAny: React.PropTypes.any.isRequired 

```

* getDefaultProps	设置组件属性的默认值	
* ref 属性	获取真实的DOM节点	
this.refs.[refName] 就会返回这个真实的 DOM 节点	
由于 this.refs.[refName] 属性获取的是真实 DOM ，所以必须等到虚拟 DOM 插入文档以后，才能使用这个属性，否则会报错

### this.state 
* getInitialState	定义初始状态	
* this.state 属性读取	
* this.setState	修改状态值	
每次修改以后，自动调用 this.render 方法，再次渲染组件	
* this.props 和 this.state 都用于描述组件的特性this.props 表示那些一旦定义，就不再改变的特性，而 this.state 是会随着用户互动而产生变化的特性
* 表单	用户在表单填入的内容，属于用户跟组件的互动，所以不能用 this.props 读取	
		定义一个 onChange 事件的回调函数，通过 event.target.value 读取用户输入的值	

### 组件的生命周期	
* Mounting	已插入真实 DOM	
* Updating	正在被重新渲染	
* Unmounting	已移出真实 DOM	

##### React 为每个状态都提供了两种处理函数，will 函数在进入状态之前调用，did 函数在进入状态之后调用		

* componentWillMount()		
* componentDidMount()		
* componentWillUpdate(object nextProps, object nextState)		
* componentDidUpdate(object prevProps, object prevState)		
* componentWillUnmount()		
两种特殊状态的处理函数

* componentWillReceiveProps(object nextProps)	已加载组件收到新的参数时调用	
* shouldComponentUpdate(object nextProps, object nextState)	组件判断是否重新渲染时调用	

### The Component Lifecycle组件生命周期

##### Mounting

* constructor()
* componentWillMount()
* render()
* componentDidMount()

##### Updataing

* compenentWillReceiveProps()
* shouldComponentUpdate()
* componentWillUpdata()
* render()
* componentDidUpdate()

##### Unmounting

* componentWillUnmount()

#### other api

* setState()
* forceUpdate()

#### Class Properties

* defultProps
* displayName
* propTypes

#### Instance Properties

* props
* state

### ReactDOM

es6 

import ReactDOM from 'react-dom';

es5 

var ReactDOM = require('react-dom');


* render()
* unmountComponentAtNode()
* findDOMNode()

### Dom Elemetns 

#### differences in attribute

* checked 
* className
* dangerouslySetInnerHTML (__html)
* htmlFor
* onChange
* selected
* style {{}} camelCased, not autoprefixed
* suppressContentEditableWarning
* value
			
### JSX 陷阱	

* style属性	style={{opacity: this.state.opacity}}	第一重大括号表示这是 JavaScript 语法，第二重大括号表示样式对象
* HTML转义	React默认会进行HTML的转义，避免XSS攻击，如果要不转义	`<div dangerouslySetInnerHTML={{__html: content}}></div>`	
* 自定义HTML属性	data-dd='xxx' aria-dd='xxx'	

### 语法区别	
```javascript		
	//React.createClass	会正确绑定this
	//以在创建组件时添加一个叫做 mixins 的属性，并将可供混合的类的集合以数组的形式赋给 mixins
	var Item = React.createClass({
		//通过getInitialState()方法返回一个包含初始值的对象
	    getInitialState(){ 
	        return {
	            isEditing: false
	        }
	    }
	    //通过proTypes对象和getDefaultProps()方法来设置和获取props.
	    propTypes: {
	        name: React.PropTypes.string
	    },
	    getDefaultProps() {
	        return {

	        };
	    },
	    handleClick() {
	        console.log(this); // React Component instance
	    },
	    render(){
	         return(<div onClick={this.handleClick}></div>);//会切换到正确的this上下文
	    }
	});
	export default Item;
```
```javascript	
	//React.Component	由于使用了 ES6，属性并不会自动绑定到 React 类的实例上
	//React mixins 的特性将不能被使用了
	clsss Item extends React.Component {  
		//通过设置两个属性propTypes和defaultProps
	    static propTypes = { // as static property
	        name: React.PropTypes.string
	    };
	    static defaultProps = { // as static property
	        name: ''
	    };
	    //通过constructor设置初始状态
	    constructor(props){
	        super(props);
	        this.state = { // define this.state in constructor
	            isEditing: false
	        } 
	    }
	    handleClick(){
	        console.log(this); // null
	    }
	    handleFocus(){  // manually bind this
	        console.log(this); // React Component Instance
	    }
	    handleBlur: ()=>{  // use arrow function
	        console.log(this); // React Component Instance
	    }
	    render(){
	        return(<div onClick={this.handleClick} 
                onFocus={this.handleFocus.bind(this)}  
                onBlur={this.handleBlur}></div>);
	    }
	}
	export default Item;
```	
			
			
			
# Redux			
Web 应用是一个状态机，视图与状态是一一对应的		
所有的状态，保存在一个对象里面		
			
###Store	
Store	保存数据的地方，可以把它看成一个容器,整个应用只能有一个 Store。包含所有数据	

createStore	生存store	

### State	
某个时点的数据
```javascript		
	store.getState()	//获取当前时刻的 State	,只要 State 相同，View 就相同，State 的变化，会导致 View 的变化	
	store.dispatch()	//View 发出 Action 的唯一方法	
	//store.dispatch接受一个 Action 对象作为参数，将它发送出去	
	//store.dispatch方法会触发 Reducer 的自动执行。为此，Store 需要知道 Reducer 函数，做法就是在生成 Store 的时候，将 Reducer 传入createStore方法。	
	store.subscribe()	//一旦 State 发生变化，就自动执行这个函数,store.subscribe方法返回一个函数，调用这个函数就可以解除监听		
```		
### action	
Action 就是 View 发出的通知，表示 State 应该要发生变化了	
改变 State 的唯一办法，就是使用 Action。它会运送数据到 Store	
action是一个对象，type属性是必须的，表示action的名称	
Action Creator	生成 Action

###　Reducer	
store 收到 Action 以后，必须给出一个新的 State，这样 View 才会发生变化。这种 State 的计算过程就叫做 Reducer。	
Reducer 是一个函数，它接受 Action 和当前 State 作为参数，返回一个新的 State				
Reducer 函数负责生成 State。由于整个应用只有一个 State 对象，包含所有数据，对于大型应用来说，这个 State 必然十分庞大，导致 Reducer 函数也十分庞大。	

####　Reducer 的拆分		
combineReducers	用这个方法，将拆分的函数合成一个大的 Reducer	
State 的属性名与子 Reducer 同名:省略子reducer
不同名	属性名：子reducer			
			
	let store = createStore(todoApp, window.STATE_FROM_SERVER)	
	//window.STATE_FROM_SERVER就是整个应用的状态初始值。注意，如果提供了这个参数，它会覆盖 Reducer 函数的默认初始值	
		
### Redux Flow	
```javascript	
	store.dispatch(action);	//首先，用户发出 Action。	
	let nextState = todoApp(previousState, action);	//然后，Store自动调用Reducer，并且传入两个参数：当前State和收到的Action，Reducer会返回新的State	
	store.subscribe(listener);	//State 一旦有变化，Store 就会调用监听函数。	
	store.getState();//listener可以通过store.getState()得到当前状态。如果使用的是 React，这时可以触发重新渲染 View。	
```		

### 中间件(middleware)		
中间件就是一个函数，对store.dispatch方法进行了改造，在发出 Action 和执行 Reducer 这两步之间，添加了其他功能		
		
createStore方法可以接受整个应用的初始状态作为参数，那样的话，applyMiddleware就是第三个参数了	
```javascript	
	const store = createStore(
		reducer,
		initial_state,
		applyMiddleware(logger)
	);	
```
中间件的次序有讲究:logger就一定要放在最后，否则输出结果会不正确	
		
applyMiddlewares():Redux 的原生方法，作用是将所有中间件组成一个数组，依次执行	
			
异步操作:

* 操作发起时的 Action		
* 操作成功时的 Action		
* 操作失败时的 Action		
			
操作开始时，送出一个 Action，触发 State 更新为"正在操作"状态，View 重新渲染		
操作结束后，再送出一个 Action，触发 State 更新为"操作结束"状态，View 再一次重新渲染		
			
##### redux-thunk 中间件	

* fetchPosts	
fetchPosts返回了一个函数，而普通的 Action Creator 默认返回一个对象	
返回的函数的参数是dispatch和getState这两个 Redux 方法，普通的 Action Creator 的参数是 Action 的内容。	
在返回的函数之中，先发出一个 Action（requestPosts(postTitle)），表示操作开始	
异步操作结束之后，再发出一个 Action（receivePosts(postTitle, json)），表示操作结束	
			
* Action 是由store.dispatch方法发送的。而store.dispatch方法正常情况下，参数只能是对象，不能是函数		
使用redux-thunk中间件，改造store.dispatch，使得store.dispatch可以接受函数作为参数		
		
* 异步操作的第一种解决方案就是，写出一个返回函数的 Action Creator，然后使用redux-thunk中间件改造store.dispatch		
* 异步操作的第二种解决方案就是，让 Action Creator 返回一个 Promise 对象。这就需要使用redux-promise中间件		
			
##### redux-promise 中间件		
redux-promise中间件使得store.dispatch方法可以接受 Promise 对象作为参数		
Action Creator 有两种写法:	

* 返回值是一个 Promise 对象		
* Action 对象的payload属性是一个 Promise 对象。这需要从redux-actions模块引入createAction方法		
			
			
### React-Redux 		
##### UI 组件	

* 只负责 UI 的呈现，不带有任何业务逻辑		
* 没有状态（即不使用this.state这个变量）		
* 所有数据都由参数（this.props）提供		
* 不使用任何 Redux 的 API	

##### 容器组件	

* 负责管理数据和业务逻辑，不负责 UI 的呈现		
* 带有内部状态		
* 使用 Redux 的 API		
		
UI 组件负责 UI 的呈现，容器组件负责管理数据和逻辑		
如果一个组件既有 UI 又有业务逻辑，那怎么办？
回答是，将它拆分成下面的结构：外面是一个容器组件，里面包了一个UI 组件。前者负责与外部的通信，将数据传给后者，由后者渲染出视图		
React-Redux 规定，所有的 UI 组件都由用户提供，容器组件则是由 React-Redux 自动生成。也就是说，用户负责视觉层，状态管理则是全部交给它		
		
		
##### connect	
connect方法，用于从 UI 组件生成容器组件。connect的意思，就是将这两种组件连起来	
connect方法接受两个参数：
mapStateToProps和mapDispatchToProps
前者负责输入逻辑，即将state映射到 UI 组件的参数（props），后者负责输出逻辑，即将用户对 UI 组件的操作映射成 Action	
		
##### mapStateToProps()	
建立一个从（外部的）state对象到（UI 组件的）props对象的映射关系	
执行后应该返回一个对象，里面的每一个键值对就是一个映射	
当state更新的时候，就会自动执行，重新计算 UI 组件的参数，从而触发 UI 组件的重新渲染	
mapStateToProps的第一个参数总是state对象，还可以使用第二个参数，代表容器组件的props对象	
connect方法可以省略mapStateToProps参数，那样的话，UI 组件就不会订阅Store，就是说 Store 的更新不会引起 UI 组件的更新。	
		
##### mapDispatchToProps()	
建立 UI 组件的参数到store.dispatch方法的映射	
可以是一个函数，也可以是一个对象	
mapDispatchToProps是一个函数，会得到dispatch和ownProps（容器组件的props对象）两个参数	
mapDispatchToProps作为函数，应该返回一个对象，该对象的每个键值对都是一个映射，定义了 UI 组件的参数怎样发出 Action	
apDispatchToProps是一个对象，它的每个键名也是对应 UI 组件的同名参数，键值应该是一个函数，会被当作 Action creator ，返回的 Action 会由Redux 自动发出	
		
##### <Provider> 组件	
让容器组件拿到state	

### react 调试

chrome 扩展 React Developer Tools			


		
# React-Router 路由库	

router是react的一个组件

路由的切换由url的hash变化决定，即url的#部分发生变化
Route组件定义了url路径与组件的对应关系

```js
//安装
npm install -s react-router	

import {Router, Route, hashHistory} from 'react-router';

render((
	<Router history={hashHistory}>
		<Route path="/" component={App} />
	</Router>
	), document.getElementById('app')
);
```

Route组件还可嵌套
用户访问/repos时，会先加载App组件，然后在其内部加载Repos组件

```js
<Router history={hashHistory}>
	<Route path="/" component={App}>
		<Route path="/repos" component={Repos} />
		<Route path="/about" component={About} />
	</Route>
</Router>

//app组件

render(){
	return (
		<div>{this.props.children}</div>
	)
}

//this.props.childrend属性就是子组件

//子路由也可以传入Router组件的routes属性
let routes = <Route path="/" component={App}>
	<Route path="/repos" component={Repos}/>
	<Route path="/about" component={About}/>
<Route>

<Router routes={routes} history={browserHistroy} />
```
### path属性

指定路由的匹配规则
该属性可以忽略，这样的话不管路径是否匹配，总是会加载指定组件
url的查询字符`/foo?bar=baz`，可以用this.props.location.query.bar获取

```js
<Route path="inbox" component={Inbox}>
	<Route path="messages/:id" component={Message} />
</Route>
```
path属性可以使用通配符

* :paramName匹配一个url部分，直到遇到下一个/、?、#为止，这个路径参数可以通过`this.props.params.paramName`取出
* ()表示url的这个部分是可选的
* *匹配任意字符，直到模式里面的下一个字符为止，匹配方式是非贪婪模式
* **匹配任意字符，直到下一个/、?、#为止，匹配方式是贪婪模式

path属性可以使用相对路径(不以/开头), 匹配时会相对于父组件的路径，嵌套路由如果想要摆脱这个规则，可以使用绝对路由

路由匹配规则是从上到下执行，一旦发现匹配，就不再执行其余规则了

```js
<Router>
	<Route path="/:userName/:id" component={UserPage}/>
	<Route path="/about/me" component={About} />
</Router>
```
用户访问`/about/me`时，不会触发第二个路由规则，因为它会匹配`/:userName/:id`这个规则
因此带参数的路径一般要写在路由规则的底部

### IndexRoute组件

IndexRoute显示指定根路由的子组件，即默认情况下加载的子组件
IndexRoute组件没有路径参数path

```js
<Router>
	<Route path="/" component={App}>
		<IndexRoute component={Home} />
		<Route path="accounts" component={Accounts} />
		<Route path="statements" component={Statements} />
	</Route>
</Router>
```

### Redirect组件

用于路由的跳转,即用户访问一个路由会自动跳转到另一个路由

```js
<Route path="inbox" component={Inbox}>
	//从 /inbox/messages:id 跳转到 /messages/:id
	<Redirect from="messages/:id" to="/messages/:id" />
</Route>
```

### IndexRedirect组件

用于访问根路由的时候，将用户重定向到某个子组件

```js
<Route path="/" component={App}>
	<IndexRedirect to="/welcome" />
	<Route path="welcome" component={welcome} />
	<Route path="about" component={About} />
</Route>
```
用户访问根路径时，将自动重定向到子组件welcome

### Link

Link组件取代`<a>`元素，生成一个连接，允许用户点击后跳转到另一个路由
当前的路由可以只用Link组件的activeStyle属性
也可以实用activeClassName指定当前路由的Class

```js
render(){
	return <div>
		<ul role="nav">
			<li><Link to="/about" activeClassName="active">About</Link></li>
			<li><Link to="/repos" activeClassName="active">Repos</Link></li>
		</ul>
	</div>
}
```

Router组件之外，导航到路由页面，可以使用浏览器的History API

```js
impot {browserHistory} from 'react-router';

browserHisstory.push('/some/path');
```

### IndexLink

链接到根路由/， 要使用IndexLink组件
或者使用Link组件的onlyActiveOnIndex属性

IndexLink是对Link组件的onlyActiveOnIndex属性的包装

```js
<IndexLink to="/" activeClassName="active">Home</IndexLink>

<Link to="/" activeClassName="active" onlyActiveOnIndex={true}>Home</Link>
```

### history属性

用来监听浏览器地址栏的变化，并将url解析成一个地址对象，供React Router匹配

* browserHistory       路由显示正常路径example.come/some/path, 背后调用的是浏览器的History API
* hashHistory          路由通过url的hash部分(#)切换，url的形式类似于example.com/#/some/path
* createMemoryHistory  主要用于服务器渲染，创建一个内存中的history对象，不与练篮球的url互动

```js
import {browserHistory} from 'react-touter';
render(
	<Router history={broserHistroy} routes={routes} />,
	document.getElementById('app')
)
//需要对服务器改造，否则用户直接向服务器请求某个子路由，会显示找不到404的错误
//开发服务器使用的是webpack-dev-server,加上参数--history-api-fallback就行了

import {hashHistory} from 'react-router';
render(
	<Router history={hashHistory} routes={routes} />,
	document.getElementById('app')
)

const history = createMemoryHistory(location)
```

### 表单

broserHistory.push
```js
import {broserHistory} from 'react-router';

handleSubmit(event){
	event.preventDefault();
	const userName = event.target.elements[0].value
	const repo = event.target.elements[1].value
	const path = `/repos/${userName}/${repo}`;
	browserHistory.push(path);
}
```

context对象
```js
export default React.createClass({
	contextTypes: {
		router: React.PropTypes.object
	},
	handleSubmit(event){
		this.context.router.push(path)
	}
})
```

### 路由的钩子

Enter 用户进入路由时触发
Leave 用户离开路由时触发

