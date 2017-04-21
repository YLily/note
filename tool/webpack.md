# 模块化

遵守commonjs规范，解决不同js模块之间相互调用的问题	

* **CommonJS**	a.js文件中调用b.js，一定要在a.js中引入b.js	require(b.js); 被引用的b.js一定要对外提供接口	module.exports = b;
* **AMD规范**	Async Module Define异步模块声明	
* **CMD规范**	Common Module Define 公共模块声明	
* **核心价值**	于解决不同文件之间的分工和调用问题，即依赖关系	

# webpack 

* 以 commonJS 的形式来书写脚本滴，但对 AMD/CMD 的支持也很全面，方便旧项目进行代码迁移
* 扩展性强，插件机制完善

### 安装	
	npm install webpack --save-dev	
	
### 执行

切换到有 webpack.config.js 的目录然后运行:
	
```js	
	webpack     // 执行一次开发的编译
	webpack --config XXX.js   //使用另一份配置文件（比如webpack.config2.js）来打包
	webpack --display-error-details			
	webpack --watch	//监听变动并自动打包		
	webpack -p  // 针对发布环境编译(压缩代码)
	webpack -w  // 进行开发过程持续的增量编译(飞快地!)
	webpack -d  // 生成map映射文件，告知哪些模块被最终打包到哪里了
```

### package.json  (npm init)

```json
	{
	  "name": "name",
	  "version": "1.0.0",
	  "description": "lily",
	  "main": "",
	  "scripts": {
	    "start": "webpack"
	  },
	  "author": "Wlily",
	  "license": "ISC",
	  "devDependencies": {
	  }
	}

```

### webpack.config.js	

```js
var webpack = require('webpack');
module.exports = {
	//页面入口文件配置
	entry: {
		
	},
	//出口文件输出配置
	output: {

	},
	//插件项
	plugins: [

	],	
	module: {

	},
	//其它解决方案配置
	resolve: {

    }
};
```
Loader是在打包前或打包的过程中作用于单个文件。插件通常在打包过程结束后，作用于包或者chunk级别


### entry,output

entry 页面入口文件配置
```js
entry: {
    //entry节点对象的属性是别名,会打包成相应的路径(带路径)
    index : './src/js/index.js'
}
```

output 入口文件输出配置

```js
output: {
    path: './build',
    publicPath: '/build/static',
    filename: '[name].js', //[name].[chunkhash:6].js
    chunkFilename: "[id].chunk.js?[hash:8]"  //[id].[chunkhash:6].js
}
``` 
path 生成文件的根目录，需要传入一个绝对路径
publicPath 表示的是一个URL路径（指向生成文件的根目录），用于生成css/js/图片/字体文件等资源的路径
filename 单文件输出默认bundle.js
[name].[chunkhash:6].js

* `[name]` 指代入口文件的name，entry参数的key，在name里利用/达到控制文件目录结构的效果
* `[hash]`指代本次编译的一个hash版本,在同一次编译过程中生成的文件[hash]的值就是一样的,在缓存的层面来说，相当于一次全量的替换
* `[chunkhash]`，指代的是当前chunk的一个hash版本，在同一次编译中，每一个chunk的hash都是不一样的。在两次编译中某个chunk根本没有发生变化，那么该chunk的hash也就不会发生变化。这在缓存的层面上来说，就是把缓存的粒度精细到具体某个chunk，只要chunk不变，该chunk的浏览器缓存就可以继续使用。

 chunkFilename 定义生成文件的命名方式的chunkFilename参数指定的是除入口文件外的chunk（这些chunk通常是由于webpack对代码的优化所形成的，比如因应实际运行的情况来异步加载）的命名
      

单入，单出

```js
module.exports = {
    entry: 'index.js',
    output: 'bundle.js'
}
```

多入，单出

```js
module.exports = {
	//支持数组形式，将加载数组中的所有模块，但以最后一个模块作为输出
    entry: ['detail.js', 'home.js'],
    output: {
        filename: '[name].js'
    }
}
```

多入，多出

```js
module.exports = {
	entry: {
	    A: "./a.js",
	    B: "./b.js",
	    C: "./c.js",
	    D: "./d.js"
	},
    output: {
        path: './build',
        filename: '[name].js'
    },
```

拼接entry

```js
var pageArr = [
    'index/login',
    'index/index',
    'alert/index',
  ];
  var configEntry = {};
  pageArr.forEach((page) => {
    configEntry[page] = path.resolve(pagesDir, page + '/page');
  });
```
### module
preloader

* 把eslint-loader放在preLoader里，preLoader是先于loader的，因此当ESLint审查到问题报了warning/error的时候就会停掉
* 不关心编译后生成的代码，只需要管好我们自己手写的代码即可

loader

* test参数用来指示当前配置项针对哪些资源，该值应是一个条件值(condition)。
* exclude参数用来剔除掉需要忽略的资源，该值应是一个条件值(condition)。
* include参数用来表示本loader配置仅针对哪些目录/文件，该值应是一个条件值(condition)
* loader/loaders参数，用来指示用哪个loader来处理目标资源,``从右向左``工作,"-loader"其实是可以省略不写的，多个loader之间用"!”
```js
module: { 
   preLoaders: [{
       test: /\.js$/, // 只针对js文件
       loader: 'eslint', // 指定启用eslint-loader
       include: dirVars.srcRootDir, // 指定审查范围仅为自己团队写的业务代码
       exclude: [/bootstrap/], // 剔除掉不需要利用eslint审查的文件
   }],
   loaders: [
       {
           test: /\.js$/, loader: 'babel-loader', // ES6
           exclude: /(node_modules|bower_components|ppaweb\\libs\\webpack)/
       },
       // CSS,LESS打包进JS
       { test: /\.css$/, loader: 'style-loader!css-loader' },
       { test: /\.less$/, loader: 'style-loader!css-loader!less-loader' },  

       // CSS,LESS单独打包
       { test: /\.css$/, loader: ExtractTextPlugin.extract("style-loader", "css-loader") },
       { test: /\.less$/, loader: ExtractTextPlugin.extract('style-loader', 'css-loader!less-loader') },

       { test: /\.tpl$/, loader: 'ejs'}, // artTemplate/ejs 's tpl

		//'?limit=8192'表示将所有小于8kb的图片转为data url形式  
        { 
       		test: /\.(png|jpg|gif)$/, 
       		loader: 'url-loader?limit=8192'
       	}
   ]
},
```

项目中babel的配置很复杂，最好把所有的babel-loader配置query写在.babelrc文件里
babel-loader会自动加载.babelrc文件如果该文件存在的话
```js
//.babelrc
 "presets": ["react", "es2015"]
 
//babel-core, babel-loader, babel-preset-es2015, babel-preset-react
```

图片文件使用 url-loader 来处理，小于8kb的直接转为base64
把图片和字体都转化成DataUrl收纳在HTML/CSS/JS文件里，以减少HTTP连接数

limit参数，数据类型为整型，表示目标文件的体积大于多少字节就换用file-loader来处理了，不填则永远不会交给file-loader处理
mimetype参数，通过这个参数来强行设置mimetype，不填写的话则默认从目标文件的后缀名进行判断
一切file-loader的参数，这些参数会在启用file-loader时传参给file-loader，比如最重要的name参数。
```js
{
    test: /\.(png|jpg|gif)$/,
    loader: 'url-loader', 
    query: {
        name: '[path][name].[ext]?[hash:8]',
        limit: 8192 
    }
}
```        
file-loader的主要功能是：把源文件迁移到指定的目录并返回新文件的路
file-loader需要传入name参数，该参数接受以下变
[ext]：文件的后缀名，示例为'jpg'
[name]：文件名本身，示例为'login-bg'
[path]：相对于当前执行webpack命令的目录的相对路径（不含文件名本身）
[hash]：源文件内容的hash，用于缓存解决方案

### resolve 配置

```js
resolve: {
    //查找module的话从这里开始查找
    root: 'E:/github/flux-example/src', //绝对路径

    //resolve.extensions自动扩展文件后缀名，require模块可以省略不写后缀名
    extensions: ['', '.js', '.json', '.scss'],

    //模块别名定义，方便后续直接引用别名，无须多写长长的地址
    alias: {
        AppStore : 'js/stores/AppStores.js',//后续直接 require('AppStore') 即可
        ActionType : 'js/actions/ActionType.js',
        AppAction : 'js/actions/AppAction.js'
    }
}
```       

### plugin 配置

```js
plugins: [
    //公共代码自动抽离
    new webpack.optimize.CommonsChunkPlugin({
        name: 'common',
        filename: 'common/[name].js', 
        minChunks: 4
    }),
    // 全局变量
    new webpack.DefinePlugin({
        __DEV__: JSON.stringify(JSON.parse(process.env.BUILD_DEV||'false')), //通过环境变量设置
        __DEV__: 'false' // 开发调试时把它改为true
    }),

    //js中调用

    if(__DEV__) {
        console.log(__DEV__);
    }
] 
```

自定义公共模块提取

* name，给这个包含公共代码的chunk命个名（唯一标识）
* filename，如何命名打包后生产的js文件，[name]、[hash]、[chunkhash]
* minChunks，公共代码的判断标准：某个js模块被多少个chunk加载了才算是公共代码。
* chunks，表示需要在哪些chunk（也可以理解为webpack配置中entry的每一项）里寻找公共代码进行打包。不设置此参数则默认提取范围为所有的chunk
```js
var CommonsChunkPlugin = require("webpack/lib/optimize/CommonsChunkPlugin");
module.exports = {
	entry: {
		A: "./a.js",
        B: "./b.js",
        C: "./c.js",
        D: "./d.js",
	},
	output: '[name].js',
	plugins: [
		new CommonsChunkPlugin("AC-commons.js", ["A", "C"]),
		new CommonsChunkPlugin("BD-commons.js", ["B", "D"])
	]
}
```
Html文件处理
HTML文件编译，自动引用JS/CSS

* title - 生成的html文档的标题
* filename - 输出文件名，相对路径output.path,默认为index.html，可以利用`/`来控制文件目录结构的
* template - HTML模板，相对配置文件目录,支持加载器(如handlebars、ejs、undersore、html等)
* chunks - 只包含指定的文件（打包后输出的JS/CSS）,不指定的话，它会包含生成的所有js和css文件
* excludeChunks - 排除指定的文件（打包后输出的JS/CSS），比如：excludeChunks: ['dev-helper']
* hash - 是否为所有注入的静态资源添加webpack每次编译产生的唯一hash值
* inject -   
1.true或者body:所有JavaScript资源插入到body元素的底部
2.head: 所有JavaScript资源插入到head元素中
3.false: 所有静态资源css和JavaScript都不会注入到模板文件中
* favicon - 添加特定favicon路径到输出的html文档中，这个同title配置项，需要在模板中动态获取其路径值
* minify - {....}|false；传递 html-minifier 选项给 minify 输出，false就是不使用html压缩
* cache - 默认true,如果为true表示在对应的thunk文件修改后就会emit文件
* showErrors - 默认true,是否将错误信息输出到html页面中

```js
var HtmlWebpackPlugin = require('html-webpack-plugin');
plugins: [
    new HtmlWebpackPlugin({
    	filename: 'views/home.html', 
    	template: 'views/home.html', 
    	chunks: ['common', 'Home'], 
    	hash: true
    }),
    new HtmlWebpackPlugin({
    	filename: 'views/detail.html', 
    	template: 'views/detail.html', 
    	chunks: ['common', 'Detail'], 
    	hash: true
    })        
]
```
单独打包CSS
可以在name里利用/达到控制文件目录结构的效果

```js
var ExtractTextPlugin = require("extract-text-webpack-plugin"); 

new ExtractTextPlugin(options: filename | object)
//options.allChunks: boolean
//options.disable: boolean
//options.id: string

module.export = {
	module: {
		loaders: [// CSS,LESS单独打包
			{ test: /\.css$/, loader: ExtractTextPlugin.extract("style-loader", "css-loader") },
			{ test: /\.less$/, loader: ExtractTextPlugin.extract('style-loader', 'css-loader!less-loader') }
		]
	},
	plugins: [
		new ExtractTextPlugin('[name].css', {allChunks: true})
	]
}   

```


### 兼容老式jQuery插件

jQuery插件是通过jQuery提供的jQuery.fn.extend(object)和jQuery.extend(object这俩方法，来把插件本身实现的方法挂载到jQuery（也即$）这个对象上的
jQuery会把jQuery对象设置为全局变量（当然也包括了$）

webpack作为一个遵从模块化原则的构建工具，要把各模块的上下文环境给分隔开以减少相互间的影响
require jQuery的时候，并不会把jQuery对象设置为全局变量
jQuery插件们找不到jQuery对象了，因为在它们各自的上下文环境里, 没有局部变量jQuery, 也没有全局变量jQuery

##### ProvidePlugin + expose-loader

所有的jQuery插件都是用webpack来加载的话，用ProvidePlugin就足够了
用`<script>`来加载就需要expose-loader

ProvidePlugin的机制是：当webpack加载到某个js模块里，出现了未定义且名称符合（字符串完全匹配）配置中key的变量时，会自动require配置中value所指定的js模块
```js
var providePlugin = new webpack.ProvidePlugin({
    $: 'jquery',
    jQuery: 'jquery',
    'window.jQuery': 'jquery',
    'window.$': 'jquery',
  });
```
expose-loader，这个loader的作用是，将指定js模块export的变量声明为全局变量。下面来看下expose-loader的配置
```js
{
	test: require.resolve('jquery'), //npm中的jquery
	loader: 'expose!$!wxpose?jQuery' //先把jQuery对象声明成全局变量'jQuery'，再通过管道进一步声明成全局变量$
}
```
##### externals

script引入jquery
当某个js显示调用 var $ = require('jquery')时, 把window.jQuery返回给它

```js
{
	externals: {
		'jquery':　'window.jQuery'
	}
}
```
##### imports-loader

手动版的ProbidePlugin

```js
{
	test: require.resolve("some-module"),
	loader: 'imports?$=jquery&jQuery=jquery'
	//相当于var $ = require("jquery"); var jQuery = require("jquery");
}
```


### 异步加载(require.ensure)

require.ensure在模块被下载下来后（模块还没被执行）便立即执行回调函数. 
另外require.ensure可以指定构建后chunk名，如果之前已有require.ensure指定了该名称，webpack会将这些模块统一合并到一个模块集里

```js
require.ensure(dependencies: String[],
        callback: function([require]),
        [chunkName: String])
```

```js
if (window.location.pathname === '/feed') {
  showLoadingState();
  require.ensure([], function() {
    hideLoadingState();
    require('./feed').show(); // 函数调用后, 模块保证在同步请求下可用
  });
} else if (window.location.pathname === '/profile') {
  showLoadingState();
  require.ensure([], function() {
    hideLoadingState();
    require('./profile').show();
  });
}
```
定义异步加载文件名字
```js
output: {
    chunkFilename: "[id].chunk.[hash:8].js"
},
```
			
### shimming	 

AMD/CMD 中，我们需要对不符合规范的模块（比如一些直接返回全局变量的插件）进行 shim 处理，这时候我们需要使用 exports-loader	
```js		
	{ test: require.resolve("./src/js/tool/swipe.js"),  loader: "exports?swipe"}	

	//之后在脚本中需要引用该模块的时候，这么简单地来使用就可以了
	require('./tool/swipe.js');		
	swipe();
```		
				
### 公共模块提取  common.js的生成

common.js是将各个页面中的公共模块提取出来组成的集合，减少了模块的重复调用

使用 CommonsChunkPlugin 插件来提取多个页面之间的公共模块，并将该模块打包为 common.js		

```js		
	new CommonsChunkPlugin("commons.js", ["p1", "p2", "admin-commons.js"]);		
	new CommonsChunkPlugin("admin-commons.js", ["ap1", "ap2"]);
```			

### 独立打包样式文件 extract-text-webpack-plugin	

样式不要被打包到脚本中，而是独立出来作为.css，然后在页面中以<link\>标签引入。这时候我们需要 extract-text-webpack-plugin	
需要在js中引入css文件
```js
	var webpack = require('webpack');
    var commonsPlugin = new webpack.optimize.CommonsChunkPlugin('common.js');
    var ExtractTextPlugin = require("extract-text-webpack-plugin");
 
 	module.exports = {
 	    module: {
 	        loaders: [
 	            { test: /\.css$/, loader: ExtractTextPlugin.extract("style-loader", "css-loader") }
 	        ]
 	    },
 	    plugins: [
 	        plugins: [commonsPlugin, new ExtractTextPlugin("[name].css")],
 	        //...省略其它配置
 	    ]
 	}
```
api 
```
new ExtractTextPlugin([id: string], filename: string, [options])
//filename: [name], [id] and [contenthash]
//options: allChunks, disable 
```


				
### externals CDN/远程文件	
某些模块走CDN并以`<script\>`的形式挂载到页面上来加载，但又能在webpack模块中使用,CDN 文件必须在 webpack 打包文件引入之前先引入externals
```js
	{
	    externals: {
	        // require("jquery") 是引用自外部模块的
	        // 对应全局变量 jQuery
	        "jquery": "jQuery"
	    }
	}
```						
	
### 模块组件打包

模块组件一般会发布到NPM或者其他地方提供给他人使用的，这里可以使用libraryTarget字段来控制webpack打包后输出为模块组件。

```js
module.exports = {
    entry: {
        pca: './src/main.js'
    },
    output: {
        path: './dist',
        filename: '[name].js',
        libraryTarget: "umd" // 组件采用UMD格式打包
    },

    module: {
        loaders: [
            {
                test: /\.js$/, loader: 'babel-loader',
                exclude: /(node_modules|libs)/
            }
        ]
    }
};

```
打包后发布到npm
可以直接 npm install xxx 来安装后，可以 var a = require('xxx'); 来使用了。


###　引入CSS模块

CSS模块引入
	require("style!css!./style.css");

安装style-loader，css-loader

	npm install style-loader css-loader

在项目配置中配置.css类型的加载器

	//在配置文件的module属性的loaders数组中加入一个配置
	{
		test: /\.css$/,
		loader: "style!css"
	}


###　调试工具

启用devtool插件，并用source-map跟踪模块

	webpack --devtool source-map

在配置文件中插入devtool的配置属性

//在webpack.config.js返回的配置模块中加入一对键值
devtool: "source-map"

### 服务器插件

安装webpack-dev-server

	npm install webpack-dev-server

在项目目录下运行

	webpack-dev-server

这样执行时会默认的使用iframe模式，即在服务器提供的框架内进行自动刷新页面，需要访问localhost:8080/webpack-dev-server目录下的对应网页，如果要在localhost:8080下执行自动刷新，应当使用--inline模式，在命令行输入

	webpack-dev-server --inline

### [webpack-dev-middle](https://npm.taobao.org/package/webpack-dev-middleware)

```js
var webpackMiddleware = require("webpack-dev-middleware"),
	webpack = require('webpack'),
	config = require('./webpack.config.js');
app.use(webpackMiddleware(webpack(config),{
	noInfo: false,
    quiet: false,
    lazy: true,
    watchOptions: {
        aggregateTimeout: 300,
        poll: true
    },
    publicPath: "/assets/", //必须required
    index: "index.html",
    headers: { "X-Custom-Header": "yes" },
    stats: {
        colors: true
    },
    reporter: null,
    serverSideRender: false
}));
```
### [webpack-hot-middleware](https://npm.taobao.org/package/webpack-hot-middleware)
```js
var webpack = require('webpack');
var webpackConfig = require('./webpack.config');
var compiler = webpack(webpackConfig);

app.use(require("webpack-dev-middleware")(compiler, {
    noInfo: true, publicPath: webpackConfig.output.publicPath
}));
app.use(require("webpack-hot-middleware")(compiler));


plugins: [
    new webpack.optimize.OccurrenceOrderPlugin(),
    new webpack.HotModuleReplacementPlugin(),
    new webpack.NoErrorsPlugin()
]
```


### 支持ES6

安装babel插件

	//将babel相关的模块都安装
	npm install babel-loader babel-core babel-preset-es2015 babel-preset-react --save-dev

在配置文件中的进行配置

	//在配置文件中加入一个新的loader字段
	{
		test: /\.js$/,
		loader: "babel"
	}

建立并配置.babelrc文件

	//在.babelrc文件中直接输入
	{
	"presets": ["es2015", "react"]
	}

PS：这样配置会使所有的.js文件进行一次转换

###　图片压缩

这里的图片压缩是将超过限制（自定义）的图片转换成base64编码的文档。

需要的插件为url-loader,而该插件依赖于file-loader，如果没有，二者都需要安装。

//先安装file-loader（如果没有）
	npm install file-loader --save-dev
//安装url-loader
	npm install url-loader --save-dev
添加loaders字段

	{
		test: /\.(jpg/png/gif)$/,
		loader: "url-loader?limit=10000"
	}//limit为文件大小限制,单位为bit



### ejs模板语法

```ejs
	<title><%= htmlWebpackPlugin.options.title %></title>

	<% for (var css in htmlWebpackPlugin.files.css) { %>
    <link href="<%=htmlWebpackPlugin.files.css[css] %>" rel="stylesheet">
    <% } %>
	<!--    -->
	<% for (var chunk in htmlWebpackPlugin.files.chunks) { %>
	<script type="text/javascript" src="<%=htmlWebpackPlugin.files.chunks[chunk].entry %>"></script>
	<% } %>
```

### browser-sync

Browsersync能让浏览器实时、快速响应您的文件更改（html、js、css、sass、less等）并自动刷新页面

npm install --save-dev browser-sync
npm install --save-dev browser-sync-webpack-plugin 

静态网站

多个类型的文件，用逗号隔开
	browser-sync start --server --files "**/*.css, **/*.html"  // **（表示任意目录）匹配

动态网站

通过代理URL(localhost:3000)来查看网站
	browser-sync start --proxy "主机名" "css/*.css"

webpack
```js
	new BrowserSyncPlugin({
		host: 'localhost',
		port: 4000,
		proxy: 'localhost:8888',
		files: 'src/*',
		files: 'index.html',
	})
```

## 复制静态文件 copy-webpack-plugin

```js
	new copyWebpackPlugin([{
		context: config.commonPath.static,
		from: '**/*',
		ignore: ['*.md']
	}])
```

### 清除文件 clean-webpack-plugin

```js
	 new CleanWebpackPlugin(['dist', 'build'], {
      root: '/full/project/path',   // An absolute path for the root.
      verbose: true, 				// Write logs to console
      dry: false,					// Use boolean "true" to test/emulate delete. (will not remove files)
      exclude: ['shared.js'] 		// Instead of removing whole path recursively,
                                   // remove all path's content with exclusion of provided immediate children.
                                   // Good for not removing shared files from build directories.
    })
```

### 压缩文件

```js
	new webpack.optimize.UglifyJsPlugin({
		compress: {
			warnings: false
		}
	})
```

	
### 与 gulp 配合	
只需要把配置写到 webpack({ ... }) 中去即可，无须再写 webpack.config.js 了	
```js
	gulp.task("webpack", function(callback) {
	    // run webpack
	    webpack({
	        // configuration
	    }, function(err, stats) {
	        if(err) throw new gutil.PluginError("webpack", err);
	        gutil.log("[webpack]", stats.toString({
	            // output options
	        }));
	        callback();
	    });
	});
```		
				
### React 相关	
使用 npm install react 的形式来安装并引用 React 模块，而不是直接使用编译后的 react.js，这样最终编译出来的 React 部分的脚本会减少 10-20 kb左右的大小			
react-hot-loader:	
React 热插拔的加载插件，可以实现修改-运行同步的效果，配合 webpack-dev-server 使用		

### 整合gulp&&webpack

区别		

* gulp是基于流的构建工具：all in one的打包模式，输出一个js文件和一个css文件，优点是减少http请求，万金油方案。
* webpack是模块化管理工具，使用webpack可以对模块进行压缩、预处理、打包、按需加载等。
	
webpack-stream方案  不可用		

* 关闭webpack的监听模式，那么每次文件变动就会全量编译JS/CSS文件，非常耗时。
* 打开webpack的监听模式，那么会阻塞其他gulp任务，导致其他gulp任务的监听失效。
	
webpack原生方案
代码较复杂



# error

at Parser....

loaders中test匹配正则错误，去掉引号


# 加速

exclude: path.resolve(__dirname, 'node_modules')
