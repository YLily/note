# nodejs

	node -v	查看版本号
	npm -v	
	node xx.js	执行js
		
## node.js组成	

1. **引入 required 模块**,我们可以使用 require 指令来载入 Node.js 模块。
2. **创建服务器**,服务器可以监听客户端的请求，类似于 Apache 、Nginx 等 HTTP 服务器。
3. **接收请求与响应请求**,服务器很容易创建，客户端可以使用浏览器或终端发送 HTTP 请求，服务器接收请求后返回响应数据。

## 明确全局模块的默认安装位置

```
npm root -g
e:\Program Files\nodejs\node_modules
```
### package.json
```
npm init	//创建模块,根据具体的情况输入生成 package.json 文件
```
```json
	{
		"name": "",  //项目名称
		"version": "",   //包的版本号
		"description": "",  //包的描述, npm search的时候会用到
		"homepage" :"",  //包的官网 url, 没有http://等带协议前缀的URL
		"author": "",    //包的作者姓名
		"contributors": "", //包的其他贡献者姓名
		"scripts": {
		    "start": "node build/dev.js",//node 文件名 npm start 
		    "build": "node build/dev.js"  //npm run build
		  },
		"dependencies": {}, //依赖包列表。如果依赖包没有安装，npm 会自动将依赖包安装在 node_module 目录下
		"repository": "",   //包代码存放的地方的类型，可以是 git 或 svn，git 可在 Github 上
		"main": "",     //程序主入口模块的ID, 如果其他用户需要你的包，当用户调用require()方法时，返回的就是这个模块的导出（exports）
		"keywords": ""   //关键字, npm search的时候会用到
	}
```		
name: 必须字段

* 不要在name中包含js, node字样；
* 这个名字最终会是URL的一部分，命令行的参数，目录名，所以不能以点号或下划线开头；
* 这个名字可能在require()方法中被调用，所以应该尽可能短；

### 安装
```javascript	
	npm install -g	//在package.json所在目录下使用npm install . -g可先在本地安装当前命令行程序，可用于发布前的本地测试	
	npm install npm -g	//升级旧版npm
	npm install <module name>	//使用 npm 命令安装模块,本地安装
	npm ls -g	//全局安装 -g	将安装包放在 /usr/local 下或者你 node 的安装目录，可以直接在命令行里使用
	npm uninstall <module name>	//卸载模块
	npm upstate <module name>	//更新模块
	npm search <module name>	//搜索模块
	
	npm adduser	//注册用户
	npm publish	//发布模块
	npm help	//查看所有命令
	pm help <command>	//查看某条命令的详细帮助
	npm cache clear	//可以清空NPM本地缓存，用于对付使用相同版本号发布新版本代码的人
	npm unpublish <package>@<version>	//撤销自己发布过的某个版本代码

	npm install -g cnpm --registry=https://registry.npm.taobao.org	//淘宝npm镜像	
	cnpm //命令来安装模块	
```	
出现错误
```javascript		
	npm err! Error: connect ECONNREFUSED 127.0.0.1:8087 	
	npm config set proxy null	
	//将安装包放在 ./node_modules 下（运行 npm 命令时所在的目录），如果没有 node_modules 目录，会在当前执行 npm 命令的目录下生成 node_modules 目录。可以通过 require() 来引入本地安装的包
```	


### nodejs REPL(Read Eval Print Loop:交互式解释器) 		 
表示一个电脑的环境，类似 Window 系统的终端或 Unix/Linux shell，我们可以在终端中输入命令，并接收系统的响应。

* 读取:读取用户输入，解析输入了Javascript 数据结构并存储在内存中。
* 执行:执行输入的数据结构
* 打印:输出结果
* 循环:循环操作以上步骤直到用户两次按下 ctrl-c 按钮退出。
		
###### REPL 命令	

* 退出当前终端	ctrl + c
* 退出 Node REPL	ctrl + d // 两次 ctrl + c
* 列出当前命令	tab键
* 列出使用命令	.hep
* 退出多行表达式	.break  //  .clear
* 保存当前的 Node REPL 会话到指定文件	.save filename
* 载入当前 Node REPL 会话的文件内容。	.load filename
		
### nodejs 回调函数	
nodejs的异步编程用回调函数来实现	
回调函数在任务完成后就会被调用	
node所有的api都支持回调函数	
没有阻塞或等待文件 I/O 操作。这就大大提高了 Node.js 的性能，可以处理大量的并发请求	
		
### nodejs事件循环	
nodejs是单线程应用程序，通过事件和回调支持并发，性能得以提高	
每个api都是异步的，作为一个独立线程运行，使用异步函数调用，处理并发	
所有的事件机制都是用设计模式中观察者实现	
每个异步事件都生成一个事件观察者，如果有事件发生就调用改回调函数	
		
##### 事件驱动模型(非阻塞是IO//事件驱动IO)	
web server接收到请求，就把它关闭进行处理，然后去服务下一个web请求。当这个请求完成，被放回到处理队列，到达处理队列的开头，结果被返回给用户	
		
##### events.EventEmitter		核心就是事件触发与事件监听器功能的封装
require('events')来访问EventEmitter	
添加新的监听器时，newListerner事件被触发	
监听器被移除时，removeListerner事件被触发	
#####event事件	 events.EventEmitter.on('eventName',function(arg1,args){});
由一个事件名和若干个参数组成，事件名是一个字符串,对于每个事件，EventEmitter 支持若干个事件监听器。当事件触发时，注册到这个事件的事件监听器被依次调用，事件参数作为回调函数参数传递	
on 函数用于绑定事件函数，emit 属性用于触发一个事件	
		
###### 方法	
```javascript
	addListerner(event, listener)	//为指定事件添加一个监听器到监听器数组的尾部。
	on(event, listener)	//指定事件注册一个监听器，接受一个字符串 event 和一个回调函数。
	once(event, listener)	//为指定事件注册一个单次监听器，即 监听器最多只会触发一次，触发后立刻解除该监听器。
	removerListener(event, listerner)	//移除指定事件的某个监听器，监听器 必须是该事件已经注册过的监听器。
	removerAllListeners([event])	//移除所有事件的所有监听器， 如果指定事件，则移除指定事件的所有监听器。
	setMaxListeners(n)	//默认情况下， EventEmitters 如果你添加的监听器超过 10 个就会输出警告信息。 setMaxListeners 函数用于提高监听器的默认限制的数量。
	listeners(event)	//返回指定事件的监听器数组。
	emit(event, arg1, args, ...)	//按参数的顺序执行每个监听器，如果事件有注册监听返回 true，否则返回 false。
```	
###### 类方法	
```javascript
	listernerCount(emitter, event)	//返回指定事件的监听器数量。
```	
	
###### 事件	
```javascript
	newListener(event, listerner)	//该事件在添加新监听器时被触发。
	removerListener(event, listerner)	//从指定监听器数组中删除一个监听器。需要注意的是，此操作将会改变处于被删监听器之后的那些监听器的索引。
```	

###### error 事件	
遇到 异常的时候通常会触发 error 事件，当 error 被触发时，EventEmitter 规定如果没有响 应的监听器，Node.js 会把它当作异常，退出程序并输出错误信息	
	
###### 继承 EventEmitter	
不会直接使用 EventEmitter，而是在对象中继承它	
原因:	

+ 具有某个实体功能的对象实现事件符合语义， 事件的监听和发射应该是一个对象的方法。	
+ JavaScript 的对象机制是基于原型的，支持 部分多重继承，继承 EventEmitter 不会打乱对象原有的继承关系	
		
### Node.js Stream(流)		
Stream 是一个抽象接口	
		
Stream四种流类型:

+ Readable - 可读操作。	
+ Writable - 可写操作。	
+ Duplex - 可读可写操作.	
+ Transform - 操作被写入数据，然后读出结果。	
		
所有的 Stream 对象都是 EventEmitter 的实例	

* data - 当有数据可读时触发。	
* end - 没有更多的数据可读时触发。	
* error - 在接收和写入过程中发生错误时触发。	
* finish - 所有数据已被写入到底层系统时触发。	
		
* 从流中读取数据:fs.createReadStream('input.txt');	
* 写入流:fs.createWriteStream('output.txt');	
* 管道流:管道提供了一个输出流到输入流的机制。通常我们用于从一个流中获取数据并将数据传递到另外一个流中	
* 链式流:链式是通过连接输出流到另外一个流并创建多个对个流操作链的机制。链式流一般用于管道操作	
		
### Node.js模块系统	
模块是Node.js 应用程序的基本组成部分，文件和模块是一一对应的。换言之，一个 Node.js 文件就是一个模块，这个文件可能是JavaScript 代码、JSON 或者编译过的C/C++ 扩展。	

* 模块让Node.js的文件可以相互调用	
* ./ 为当前目录，node.js默认后缀为js	
* exports 是模块公开的接	
* require 用于从外部获取一个模块的接口，即所获取模块的 exports 对象	
* module.exports = moduleName	
* require('./moduleName');	
		
### Node.js 函数		
可以先定义一个函数，然后传递，也可以在传递参数的地方直接定义函数。	
		
### Node.js 路由		
- url:
- var url = require("url");	
- url.parse(string).pathname;	
- url.parse(string).query;	
		
- querystring:	
- var querystring = require('querystring');	
- querystring(string)["name"]  	
- querystring.parse(post);	
		
### node.js全局对象		
global	最根本的作用是作为全局变量的宿主	

+ 全局变量条件:
+ 在最外层定义的变量；	
+ 全局对象的属性；	
+ 隐式定义的变量（未定义直接赋值的变量）

在 Node.js 中你不可能在最外层定义变量，因为所有用户代码都是属于当前模块的， 而模块本身不是最外层上下文	
永远使用 var 定义变量以避免引入全局变量，因为全局变量会污染 命名空间，提高代码的耦合风险。	

```javascript		
	__filename	//当前正在执行的脚本的文件名。它将输出文件所在位置的绝对路径，且和命令行参数所指定的文件名不一定相同。 如果在模块中，返回的值是模块文件的路径。	用绝对路径显示一个文件的位置
	__dirname	//当前执行脚本所在的目录,该文件所在目录的绝对路径	
	setTimeout(cb, ms)	//"在指定的毫秒(ms)数后执行指定函数(cb)。：setTimeout() 只执行一次指定函数。返回一个代表定时器的句柄值"	
	setInterval(cb, ms)	//"在指定的毫秒(ms)数后执行指定函数(cb)。返回一个代表定时器的句柄值。可以使用 clearInterval(t) 函数来清除定时器。
	setInterval() //方法会不停地调用函数，直到 clearInterval() 被调用或窗口被关闭"	
	clearTimeout(t)	//停止一个之前通过 setTimeout() 创建的定时器。 参数 t 是通过 setTimeout() 函数创建的定时器	
```

###### console	
```javascript
	console.log([data][, ...])	//向标准输出流打印字符并以换行符结束。该方法接收若干 个参数，如果只有一个参数，则输出这个参数的字符串形式。如果有多个参数，则 以类似于C 语言 printf() 命令的格式输出。
	console.info([data][, ...])	//该命令的作用是返回信息性消息，这个命令与console.log差别并不大，除了在chrome中只会输出文字外，其余的会显示一个蓝色的惊叹号。
	console.error([data][, ...])	//输出错误消息的。控制台在出现错误时会显示是红色的叉子
	console.warn([data][, ...])	//输出警告消息,控制台出现有黄色的惊叹号
	console.dir(obj[, options])	//用来对一个对象进行检查（inspect），并以易于阅读和打印的格式显示
	console.time(label)	//输出时间，表示计时开始
	console.timeEnd(label)	//结束时间，表示计时结束
	console.trace(message[, ...])	//当前执行的代码在堆栈中的调用路径，这个测试函数运行很有帮助，只要给想测试的函数里面加入 console.trace 就行了
	console.assert(value[, message][, ...])	//用于判断某个表达式或变量是否为真，接手两个参数，第一个参数是表达式，第二个参数是字符串。只有当第一个参数为false，才会输出第二个参数，否则不会有任何结果
```
###### process	
```javascript
exi  //当进程准备退出时触发。
beforeExit	//当 node 清空事件循环，并且没有其他安排时触发这个事件。通常来说，当没有进程安排时 node 退出，但是 'beforeExit' 的监听器可以异步调用，这样 node 就会继续执行。
uncaughtException	//当一个异常冒泡回到事件循环，触发这个事件。如果给异常添加了监视器，默认的操作（打印堆栈跟踪信息并退出）就不会发生
Signal	//进程接收到信号时就触发。信号列表详见标准的 POSIX 信号名，如 SIGINT、SIGUSR1 等。
```

###### 退出状态码	
```javascript
	Uncaught Fatal Exception	//有未捕获异常，并且没有被域或 uncaughtException 处理函数处理。
	Unused	//保留
	Internal JavaScript Parse Error	//JavaScript的源码启动 Node 进程时引起解析错误。非常罕见，仅会在开发 Node 时才会有。
	Internal JavaScript Evaluation Failure	//JavaScript 的源码启动 Node 进程，评估时返回函数失败。非常罕见，仅会在开发 Node 时才会有。Fatal Error	V8 里致命的不可恢复的错误。通常会打印到 stderr ，内容为： FATAL ERROR
	Non-function Internal Exception Handler	//未捕获异常，内部异常处理函数不知为何设置为on-function，并且不能被调用。
	Internal Exception Handler Run-Time Failure	//未捕获的异常， 并且异常处理函数处理时自己抛出了异常。例如，如果 process.on('uncaughtException') 或 domain.on('error') 抛出了异常。
	Invalid Argument	//可能是给了未知的参数，或者给的参数没有值。
	Internal JavaScript Run-Time Failure	//javaScript的源码启动 Node 进程时抛出错误，非常罕见，仅会在开发 Node 时才会有
	Invalid Debug Argument 	//设置了参数--debug 和/或 --debug-brk，但是选择了错误端口。
	Signal Exits	//如果 Node 接收到致命信号，比如SIGKILL 或 SIGHUP，那么退出代码就是128 加信号代码。这是标准的 Unix 做法，退出信号代码放在高位。
```

###### Process 属性	
```javascript
	stdout	//标准输出流。
	stderr	//标准错误流
	stdin	//标准输入流。
	argv	//argv 属性返回一个数组，由命令行执行脚本时的各个参数组成。它的第一个成员总是node，第二个成员是脚本文件名，其余成员是脚本文件的参数。
	execPath	//返回执行当前脚本的 Node 二进制文件的绝对路径。
	execArgv	//返回一个数组，成员是命令行下执行脚本时，在Node可执行文件与脚本文件之间的命令行参数
	env			//返回一个对象，成员为当前 shell 的环境变量
	exitCode	//进程退出时的代码，如果进程优通过 process.exit() 退出，不需要指定退出码。
	version		//Node 的版本，比如v0.10.18。
	versions	//一个属性，包含了 node 的版本和依赖.
	config		//一个包含用来编译当前 node 执行文件的 javascript 配置选项的对象。它与运行 ./configure 脚本生成的 "config.gypi" 文件相同。
	pid			//当前进程的进程号。
	title	//进程名，默认值为"node"，可以自定义该值。
	arch	//当前 CPU 的架构：'arm'、'ia32' 或者 'x64'。
	platform	//运行程序所在的平台系统 'darwin', 'freebsd', 'linux', 'sunos' 或 'win32'
	mainModule	//require.main 的备选方法。不同点，如果主模块在运行时改变，require.main可能会继续返回老的模块。可以认为，这两者引用了同一个模块。
```		
###### Process 方法	
```javascript
	abort()		//这将导致 node 触发 abort 事件。会让 node 退出并生成一个核心文件
	chdir(directory)	//改变当前工作进程的目录，如果操作失败抛出异常。
	cwd()	//返回当前进程的工作目录
	exit([code])	//使用指定的 code 结束进程。如果忽略，将会使用 code 0。
	getgid()	//获取进程的群组标识（参见 getgid(2)）。获取到得时群组的数字 id，而不是名字。注意：这个函数仅在 POSIX 平台上可用(例如，非Windows 和 Android)。
	setgid(id)	//设置进程的群组标识（参见 setgid(2)）。可以接收数字 ID 或者群组名。如果指定了群组名，会阻塞等待解析为数字 ID 。注意：这个函数仅在 POSIX 平台上可用(例如，非Windows 和 Android)。
	getuid()	//获取进程的用户标识(参见 getuid(2))。这是数字的用户 id，不是用户名。注意：这个函数仅在 POSIX 平台上可用(例如，非Windows 和 Android)
	setuid(id)	//设置进程的用户标识（参见setuid(2)）。接收数字 ID或字符串名字。果指定了群组名，会阻塞等待解析为数字 ID 。
	getgroups()	//返回进程的群组 iD 数组。POSIX 系统没有保证一定有，但是 node.js 保证有。注意：这个函数仅在 POSIX 平台上可用(例如，非Windows 和 Android)。
	setgroups(groups)	//设置进程的群组 ID。这是授权操作，所有你需要有 root 权限，或者有 CAP_SETGID 能力。注意：这个函数仅在 POSIX 平台上可用(例如，非Windows 和 Android)。
	initgroups(user, extra_group)	//读取 /etc/group ，并初始化群组访问列表，使用成员所在的所有群组。这是授权操作，所有你需要有 root 权限，或者有 CAP_SETGID 能力。注意：这个函数仅在 POSIX 平台上可用(例如，非Windows 和 Android)。
	kill(pid[, signal])	//发送信号给进程. pid 是进程id，并且 signal 是发送的信号的字符串描述。信号名是字符串，比如 'SIGINT' 或 'SIGHUP'。如果忽略，信号会是 'SIGTERM'。
	memoryUsage()	//返回一个对象，描述了 Node 进程所用的内存状况，单位为字节。
	nextTick(callback)	//一旦当前事件循环结束，调用回到函数。
	umask([mask])	//设置或读取进程文件的掩码。子进程从父进程继承掩码。如果mask 参数有效，返回旧的掩码。否则，返回当前掩码。
	uptime()	//返回 Node 已经运行的秒数。
	hrtime()	//返回当前进程的高分辨时间，形式为 [seconds, nanoseconds]数组。它是相对于过去的任意事件。该值与日期无关，因此不受时钟漂移的影响。主要用途是可以通过精确的时间间隔，来衡量程序的性能。你可以将之前的结果传递给当前的 process.hrtime() ，会返回两者间的时间差，用来基准和测量时间间隔。
```

### nodejs常用工具

##### util	
Node.js 核心模块，提供常用函数的集合，用于弥补核心JavaScript 的功能 过于精简的不足	

	util.inherits(constructor, superConstructor)
是一个实现对象间原型继承 的函数。	javaScript 的面向对象特性是基于原型的，与常见的基于类的不同。JavaScript 没有 提供对象继承的语言级别特性，而是通过原型复制来实现的。	
		
	util.inspect(object,[showHidden],[depth],[colors])
是一个将任意对象转换 为字符串的方法，通常用于调试和错误输出。它至少接受一个参数 object，即要转换的对象。	

* showHidden 是一个可选参数，如果值为 true，将会输出更多隐藏信息。	
* depth 表示最大递归的层数，如果对象很复杂，你可以指定层数以控制输出信息的多 少。如果不指定depth，默认会递归2层，指定为 null 表示将不限递归层数完整遍历对象。 
* 如果color 值为 true，输出格式将会以ANSI 颜色编码，通常用于在终端显示更漂亮 的效果。	
特别要指出的是，util.inspect 并不会简单地直接把对象转换为字符串，即使该对 象定义了toString 方法也不会调用。	
```javascript		
	util.isArray(object)	//如果给定的参数 "object" 是一个数组返回true，否则返回false
	util.isRegExp(object)	//如果给定的参数 "object" 是一个正则表达式返回true，否则返回false。
	util.isDate(object)		//如果给定的参数 "object" 是一个日期返回true，否则返回false。
	util.isError(object)	//如果给定的参数 "object" 是一个错误对象返回true，否则返回false。
```

### Node.js 文件系统		
文件模块方法
```javascript
	fs.rename(oldPath, newPath, callback)	//异步 rename().回调函数没有参数，但可能抛出异常。
	fs.ftruncate(fd, len, callback)			//异步 ftruncate().回调函数没有参数，但可能抛出异常。
	fs.ftruncateSync(fd, len)				//同步 ftruncate()
	fs.truncate(path, len, callback)		//异步 truncate().回调函数没有参数，但可能抛出异常。
	fs.truncateSync(path, len)				//同步 truncate()
	fs.chown(path, uid, gid, callback)		//异步 chown().回调函数没有参数，但可能抛出异常。
	fs.chownSync(path, uid, gid)			//同步 chown()
	fs.fchown(fd, uid, gid, callback)		//异步 fchown().回调函数没有参数，但可能抛出异常。
	fs.fchownSync(fd, uid, gid)				//同步 fchown()
	fs.lchown(path, uid, gid, callback)		//异步 lchown().回调函数没有参数，但可能抛出异常。
	fs.lchownSync(path, uid, gid)			//同步 lchown()
	fs.chmod(path, mode, callback)			//异步 chmod().回调函数没有参数，但可能抛出异常。
	fs.chmodSync(path, mode)				//同步 chmod().
	fs.fchmod(fd, mode, callback)			//异步 fchmod().回调函数没有参数，但可能抛出异常。
	fs.fchmodSync(fd, mode)					//同步 fchmod().
	fs.lchmod(path, mode, callback)			//异步 lchmod().回调函数没有参数，但可能抛出异常。Only available on Mac OS X.
	fs.lchmodSync(path, mode)				//同步 lchmod().
	fs.stat(path, callback)					//异步 stat(). 回调函数有两个参数 err, stats，stats 是 fs.Stats 对象。
	fs.lstat(path, callback)				//异步 lstat(). 回调函数有两个参数 err, stats，stats 是 fs.Stats 对象。
	fs.fstat(fd, callback)					//异步 fstat(). 回调函数有两个参数 err, stats，stats 是 fs.Stats 对象。
	fs.statSync(path)//同步 stat(). 返回 fs.Stats 的实例。
	fs.lstatSync(path)//同步 lstat(). 返回 fs.Stats 的实例。
	fs.fstatSync(fd)//同步 fstat(). 返回 fs.Stats 的实例。
	fs.link(srcpath, dstpath, callback)//异步 link().回调函数没有参数，但可能抛出异常。
	fs.linkSync(srcpath, dstpath)//同步 link().
	fs.symlink(srcpath, dstpath[, type], callback)//异步 symlink().回调函数没有参数，但可能抛出异常。 type 参数可以设置为 'dir', 'file', 或 'junction' (默认为 'file') 。
	fs.symlinkSync(srcpath, dstpath[, type])//同步 symlink().
	fs.readlink(path, callback)				//异步 readlink(). 回调函数有两个参数 err, linkString。
	fs.realpath(path[, cache], callback)	//异步 realpath(). 回调函数有两个参数 err, resolvedPath。
	fs.realpathSync(path[, cache])          //同步 realpath()。返回绝对路径。
	fs.unlink(path, callback)              //异步 unlink().回调函数没有参数，但可能抛出异常。
	fs.unlinkSync(path)                		 //同步 unlink().
	fs.rmdir(path, callback)                 //异步 rmdir().回调函数没有参数，但可能抛出异常。
	fs.rmdirSync(path)                 		//同步 rmdir().
	fs.mkdir(path[, mode], callback)        //异步 mkdir(2).回调函数没有参数，但可能抛出异常。 mode defaults to 0777.
	fs.mkdirSync(path[, mode])              //同步 mkdir().
	fs.readdir(path, callback)             //异步 readdir(3). 读取目录的内容。
	fs.readdirSync(path)                 //同步 readdir().返回文件数组列表。
	fs.close(fd, callback)                 //异步 close().回调函数没有参数，但可能抛出异常。
	fs.closeSync(fd)                 //同步 close().
	fs.open(path, flags[, mode], callback)     //异步打开文件。
	fs.openSync(path, flags[, mode])          //同步 version of fs.open().
	fs.utimes(path, atime, mtime, callback)
	fs.utimesSync(path, atime, mtime)           //修改文件时间戳，文件通过指定的文件路径。
	fs.futimes(fd, atime, mtime, callback)
	fs.futimesSync(fd, atime, mtime)          //修改文件时间戳，通过文件描述符指定。
	fs.fsync(fd, callback)                 //异步 fsync.回调函数没有参数，但可能抛出异常。
	fs.fsyncSync(fd)                 //同步 fsync.
	fs.write(fd, buffer, offset, length[, position], callback)                 //将缓冲区内容写入到通过文件描述符指定的文件。
	fs.write(fd, data[, position[, encoding]], callback)                 //通过文件描述符 fd 写入文件内容。
	fs.writeSync(fd, buffer, offset, length[, position])                 //同步版的 fs.write()。
	fs.writeSync(fd, data[, position[, encoding]])                 //同步版的 fs.write().
	fs.read(fd, buffer, offset, length, position, callback)                 //通过文件描述符 fd 读取文件内容。
	fs.readSync(fd, buffer, offset, length, position)                 //同步版的 fs.read.
	fs.readFile(filename[, options], callback)                 //异步读取文件内容。
	fs.readFileSync(filename[, options])
	fs.writeFile(filename, data[, options], callback)                 //异步写入文件内容。
	fs.writeFileSync(filename, data[, options])                 //同步版的 fs.writeFile。
	fs.appendFile(filename, data[, options], callback)                 //异步追加文件内容。
	fs.appendFileSync(filename, data[, options])                 //The 同步 version of fs.appendFile.
	fs.watchFile(filename[, options], listener)                 //查看文件的修改。
	fs.unwatchFile(filename[, listener])                 //停止查看 filename 的修改。
	fs.watch(filename[, options][, listener])                 //查看 filename 的修改，filename 可以是文件或目录。返回 fs.FSWatcher 对象。
	fs.exists(path, callback)                 //检测给定的路径是否存在。
	fs.existsSync(path)                 //同步版的 fs.exists.
	fs.access(path[, mode], callback)                 //测试指定路径用户权限。
	fs.accessSync(path[, mode])                 //同步版的 fs.access。
	fs.createReadStream(path[, options])                 //返回ReadStream 对象。
	fs.createWriteStream(path[, options])                 //返回 WriteStream 对象。
	fs.symlink(srcpath, dstpath[, type], callback)                 //异步 symlink().回调函数没有参数，但可能抛出异常。

```		
Node.js 提供一组类似 UNIX（POSIX）标准的文件操作API	
var fs = require("fs");	
		
异步和同步	
```javascript
	fs.readFile()	//异步
	fs.readFileSync()	//同步
```
异步的方法函数最后一个参数为回调函数，回调函数的第一个参数包含了错误信息(error)。
建议大家是用异步方法，比起同步，异步方法性能更高，速度更快，而且没有阻塞。"	

```javascript		
//打开文件	
fs.open(path, flags[, mode], callback)	
//path - 文件的路径。
//flags - 文件打开的行为。具体值详见下文。
//mode - 设置文件模式(权限)，文件创建默认权限为 0666(可读，可写)。
//callback - 回调函数，带有两个参数如：callback(err, fd)。
//flags 参数可以是以下值：
//r	以读取模式打开文件。如果文件不存在抛出异常。
//r+	以读写模式打开文件。如果文件不存在抛出异常。
//rs	以同步的方式读取文件。
//rs+	以同步的方式读取和写入文件。
//w	以写入模式打开文件，如果文件不存在则创建。
//wx	类似 'w'，但是如果文件路径存在，则文件写入失败。
//w+	以读写模式打开文件，如果文件不存在则创建。
//wx+	类似 'w+'， 但是如果文件路径存在，则文件读写失败。
//a	以追加模式打开文件，如果文件不存在则创建。
//ax	类似 'a'， 但是如果文件路径存在，则文件追加失败。
//a+	以读取追加模式打开文件，如果文件不存在则创建。
//ax+	类似 'a+'， 但是如果文件路径存在，则文件读取追加失败。

//获取文件信息	
fs.stat(path, callback)	
//path - 文件路径。
//callback - 回调函数，带有两个参数如：(err, stats), stats 是 fs.Stats 对象。
//stats类中的方法有：
//stats.isFile()	如果是文件返回 true，否则返回 false。
//stats.isDirectory()	如果是目录返回 true，否则返回 false。
//stats.isBlockDevice()	如果是块设备返回 true，否则返回 false。
//stats.isCharacterDevice()	如果是字符设备返回 true，否则返回 false。
//stats.isSymbolicLink()	如果是软链接返回 true，否则返回 false。
//stats.isFIFO()	如果是FIFO，返回true，否则返回 false。FIFO是UNIX中的一种特殊类型的命令管道。
//stats.isSocket()	如果是 Socket 返回 true，否则返回 false。


//写入文件	
fs.writeFile(filename, data[, options], callback)	
//path - 文件路径。
//data - 要写入文件的数据，可以是 String(字符串) 或 Buffer(流) 对象。
//options - 该参数是一个对象，包含 {encoding, mode, flag}。默认编码为 utf8, 模式为 0666 ， flag 为 'w'
//callback - 回调函数，回调函数只包含错误信息参数(err)，在写入失败时返回。

//读取文件	
fs.read(fd, buffer, offset, length, position, callback)	
//fd - 通过 fs.open() 方法返回的文件描述符。
//buffer - 数据写入的缓冲区。
//offset - 缓冲区写入的写入偏移量。
//length - 要从文件中读取的字节数。
//position - 文件读取的起始位置，如果 position 的值为 null，则会从当前文件指针的位置读取。
//callback - 回调函数，有三个参数err, bytesRead, buffer，err 为错误信息， bytesRead 表示读取的字节数，buffer 为缓冲区对象。"

//关闭文件
fs.close(fd, callback)	
//fd - 通过 fs.open() 方法返回的文件描述符。
//callback - 回调函数，没有参数。

//截取文件	
fs.ftruncate(fd, len, callback)	
//fd - 通过 fs.open() 方法返回的文件描述符。
//len - 文件内容截取的长度。
//callback - 回调函数，没有参数。

//删除文件	
fs.unlink(path, callback)	
//path - 文件路径。
//callback - 回调函数，没有参数。
		
//创建目录	
fs.mkdir(path[, mode], callback)	
//path - 文件路径。
//mode - 设置目录权限，默认为 0777。
//callback - 回调函数，没有参数。

//读取目录	
fs.readdir(path, callback)	
//path - 文件路径。
//callback - 回调函数，回调函数带有两个参数err, files，err 为错误信息，files 为 目录下的文件数组列表。

//删除目录	
fs.rmdir(path, callback)	
//path - 文件路径。
//callback - 回调函数，没有参数。
```

### get/post请求		
#####　get	
由于GET请求直接被嵌入在路径中，URL是完整的请求路径，包括了?后面的部分，可以手动解析后面的内容作为GET请求的参数。
node.js中url模块中的parse函数提供了这个功能。	
		
#####　post
http.ServerRequest并没有一个属性内容为请求体，原因是等待请求体传输可能是一件耗时的工作	恶意的POST请求会大大消耗服务器的资源，所有node.js默认是不会解析请求体的， 当你需要的时候，需要手动来做	
```javascript
var post = '';     //定义了一个post变量，用于暂存请求体的信息
request.on('data', function(chunk){    //通过req的data事件监听函数，每当接受到请求体的数据，就累加到post变量中
    post += chunk;
});
request.on('end', function(){    //在end事件触发后，通过querystring.parse将post解析为真正的POST请求格式，然后向客户端返回。
    post = querystring.parse(post);
    res.end(util.inspect(post));
});	
```

### Node.js 工具模块		
		
#### OS 模块	提供基本的系统操作函数。	var os = require("os")
```javascript
	os.tmpdir()//返回操作系统的默认临时文件夹。
	os.endianness()//返回 CPU 的字节序，可能的是 "BE" 或 "LE"。
	os.hostname()//返回操作系统的主机名。
	os.type()//返回操作系统名
	os.platform()//返回操作系统名
	os.arch()//返回操作系统 CPU 架构，可能的值有 "x64"、"arm" 和 "ia32"。
	os.release()//返回操作系统的发行版本。
	s.uptime()//返回操作系统运行的时间，以秒为单位。
	os.loadavg()//返回一个包含 1、5、15 分钟平均负载的数组。
	os.totalmem()//返回系统内存总量，单位为字节。
	os.freemem()//返回操作系统空闲内存量，单位是字节。
	os.cpus()//返回一个对象数组，包含所安装的每个 CPU/内核的信息：型号、速度（单位 MHz）、时间（一个包含 user、nice、sys、idle 和 irq 所使用 CPU/内核毫秒数的对象）。
	os.networkInterfaces()//获得网络接口列表。
```

#### Path 模块	提供了处理和转换文件路的工具。	var path = require("path")
```javascript
	//方法
	path.normalize(p)//规范化路径，注意'..' 和 '.'。
	path.join([path1][, path2][, ...])//用于连接路径。该方法的主要用途在于，会正确使用当前系统的路径分隔符，Unix系统是"/"，Windows系统是"\"。
	path.resolve([from ...], to)//将 to 参数解析为绝对路径。
	path.isAbsolute(path)//判断参数 path 是否是绝对路径。
	path.relative(from, to)//用于将相对路径转为绝对路径。
	path.dirname(p)//返回路径中代表文件夹的部分，同 Unix 的dirname 命令类似。
	path.basename(p[, ext])//返回路径中的最后一部分。同 Unix 命令 bashname 类似。
	path.extname(p)//返回路径中文件的后缀名，即路径中最后一个'.'之后的部分。如果一个路径中并不包含'.'或该路径只包含一个'.' 且这个'.'为路径的第一个字符，则此命令返回空字符串。
	path.parse(pathString)//返回路径字符串的对象。
	path.format(pathObject)//从对象中返回路径字符串，和 path.parse 相反。
	//属性
	path.sep//平台的文件路径分隔符，'\\' 或 '/'。
	path.delimiter//平台的分隔符, ; or ':'.
	path.posix//提供上述 path 的方法，不过总是以 posix 兼容的方式交互。
	path.win32//提供上述 path 的方法，不过总是以 win32 兼容的方式交互。
```
#### Net 模块	用于底层的网络通信。提供了服务端和客户端的的操作。	var net = require("net")

#### DNS 模块	用于解析域名。	var dns = require("dns")
```javascript
	dns.lookup(hostname[, options], callback)//将域名（比如 'runoob.com'）解析为第一条找到的记录 A （IPV4）或 AAAA(IPV6)。参数 options可以是一个对象或整数。如果没有提供 options，IP v4 和 v6 地址都可以。如果 options 是整数，则必须是 4 或 6。
	dns.lookupService(address, port, callback)//使用 getnameinfo 解析传入的地址和端口为域名和服务。
	dns.resolve(hostname[, rrtype], callback)//将一个域名（如 'runoob.com'）解析为一个 rrtype 指定记录类型的数组。
	dns.resolve4(hostname, callback)//和 dns.resolve() 类似, 仅能查询 IPv4 (A 记录）。 addresses IPv4 地址数组 (比如，['74.125.79.104', '74.125.79.105', '74.125.79.106']）。
	dns.resolve6(hostname, callback)//和 dns.resolve4() 类似， 仅能查询 IPv6( AAAA 查询）
	dns.resolveMx(hostname, callback)//和 dns.resolve() 类似, 仅能查询邮件交换(MX 记录)。
	dns.resolveTxt(hostname, callback)//和 dns.resolve() 类似, 仅能进行文本查询 (TXT 记录）。 addresses 是 2-d 文本记录数组。(比如，[ ['v=spf1 ip4:0.0.0.0 ', '~all' ] ]）。 每个子数组包含一条记录的 TXT 块。根据使用情况可以连接在一起，也可单独使用。
	dns.resolveSrv(hostname, callback)//和 dns.resolve() 类似, 仅能进行服务记录查询 (SRV 记录）。 addresses 是 hostname可用的 SRV 记录数组。 SRV 记录属性有优先级（priority），权重（weight）, 端口（port）, 和名字（name） (比如，[{'priority': 10, 'weight': 5, 'port': 21223, 'name': 'service.example.com'}, ...]）。
	dns.resolveSoa(hostname, callback)//和 dns.resolve() 类似, 仅能查询权威记录(SOA 记录）。
	dns.resolveNs(hostname, callback)//和 dns.resolve() 类似, 仅能进行域名服务器记录查询(NS 记录）。 addresses 是域名服务器记录数组（hostname 可以使用） (比如, ['ns1.example.com', 'ns2.example.com']）。
	dns.resolveCname(hostname, callback)//和 dns.resolve() 类似, 仅能进行别名记录查询 (CNAME记录)。addresses 是对 hostname 可用的别名记录数组 (比如，, ['bar.example.com']）。
	dns.reverse(ip, callback)//反向解析 IP 地址，指向该 IP 地址的域名数组。
	dns.getServers()//返回一个用于当前解析的 IP 地址数组的字符串。
	dns.setServers(servers)//指定一组 IP 地址作为解析服务器
```

#### Domain 模块	简化异步代码的异常处理，可以捕捉处理try catch无法捕捉的。	var domain = require("domain")
```javascript
	domain.run(function)//在域的上下文运行提供的函数，隐式的绑定了所有的事件分发器，计时器和底层请求。
	domain.add(emitter)//显式的增加事件
	domain.remove(emitter)//删除事件。
	omain.bind(callback)//返回的函数是一个对于所提供的回调函数的包装函数。当调用这个返回的函数被时，所有被抛出的错误都会被导向到这个域的 error 事件。
	domain.intercept(callback)//和 domain.bind(callback) 类似。除了捕捉被抛出的错误外，它还会拦截 Error 对象作为参数传递到这个函数。
	domain.enter()//进入一个异步调用的上下文，绑定到domain。
	domain.exit()//退出当前的domain，切换到不同的链的异步调用的上下文中。对应domain.enter()。
	domain.dispose()//释放一个domain对象，让node进程回收这部分资源。
	domain.create()//返回一个domain对象。
```

		
### nodejs Buffer(缓冲区)	
javaScript 语言自身只有字符串数据类型，没有二进制数据类型。	
Buffer 类用来创建一个专门存放二进制数据的缓存区。

##### 创建Buffer类	
```javascript
	new Buffer(size)	//分配一个新的 size 大小单位为8位字节的 buffer。 注意, size 必须小于 kMaxLength，否则，将会抛出异常 RangeError。
	new Buffer(buffer)	//拷贝参数 buffer 的数据到 Buffer 实例。
	new Buffer(str[,encoding])	//分配一个新的 buffer ，其中包含着传入的 str 字符串。 encoding 编码方式默认为 'utf8'。
```

##### 缓冲区长度	
	buf.length	
返回这个 buffer 的 bytes 数。注意这未必是 buffer 里面内容的大小。length 是 buffer 对象所分配的内存数，它不会随着这个 buffer 对象内容的改变而改变。

##### 写	
```javascript
	buf.write(string, [, offset[,length]][,encoding])	
	buf.writeUlntLE(value, offset, byteLength[, noAssert])	//将value 写入到 buffer 里， 它由offset 和 byteLength 决定，支持 48 位计算
	buf.writeUlntBE(value, offset, byteLength[, noAssert])	
	buf.writelntLE(value, offset, byteLength[, noAssert])	
	buf.writelntBe(value, offset, byteLength[, noAssert])	
```
根据参数 offset 偏移量和指定的 encoding 编码方式，将参数 string 数据写入buffer。 offset 偏移量默认值是 0, encoding 编码方式默认是 utf8。 length 长度是将要写入的字符串的 bytes 大小。 返回 number 类型，表示写入了多少 8 位字节流。如果 buffer 没有足够的空间来放整个 string，它将只会只写入部分字符串。 length 默认是 buffer.length - offset。 这个方法不会出现写入部分字符。

##### 读
```javascript
	buf.readUlntLE(offset, byteLength[, noAssert])	//支持读取 48 位以下的数字。
	buf.readUlntBe(offset, byteLength[, noAssert)	
	buf.readlntLE(offset, byteLength[, noAssert])	
	buf.readlntBe(offset, byteLength[, noAssert)	
```

##### 从缓冲区读取数据	
```javascript
	buf.toString([encoding[, start[, end]]])	//根据 encoding 参数（默认是 'utf8'）返回一个解码过的 string 类型。还会根据传入的参数 start (默认是 0) 和 end (默认是 buffer.length)作为取值范围。
	buf.toJSON()	//将 Buffer 实例转换为 JSON 对象。
	buf[index]	//获取或设置指定的字节。返回值代表一个字节，所以返回值的合法范围是十六进制0x00到0xFF 或者十进制0至 255。
	buf.equals(otherBuffer)	//比较两个缓冲区是否相等，如果是返回 true，否则返回 false。
	buf.compare(otherBuffer)	//比较两个 Buffer 对象，返回一个数字，表示 buf 在 otherBuffer 之前，之后或相同。
	buf.copy(targetBuffer[, targetStart[, sourceStart[, sourceEnd]]])	//buffer 拷贝，源和目标可以相同。 targetStart 目标开始偏移和 sourceStart 源开始偏移默认都是 0。 sourceEnd 源结束位置偏移默认是源的长度 buffer.length 。
```

##### 缓冲区裁剪	
	buf.slice([start[, end]])	
剪切 Buffer 对象，根据 start(默认是 0 ) 和 end (默认是 buffer.length ) 偏移和裁剪了索引。 负的索引是从 buffer 尾部开始计算的。若参数 noAssert 为 true 将不会验证 offset 偏移量参数。 如果这样 offset 可能会超出buffer 的末尾。默认是 false。

```javascript
	//若参数 noAssert 为 true 将不会验证 offset 偏移量参数。 如果这样 offset 可能会超出buffer 的末尾。默认是 false。
	buf.readUInt8(offset[, noAssert])	//根据指定的偏移量，读取一个有符号 8 位整数。
	buf.readUInt16LE(offset[, noAssert])	//根据指定的偏移量，使用特殊的 endian 字节序格式读取一个有符号 16 位整数
	buf.readUInt16BE(offset[, noAssert])	//根据指定的偏移量，使用特殊的 endian 字节序格式读取一个有符号 16 位整数
	buf.readUInt32LE(offset[, noAssert])	//根据指定的偏移量，使用指定的 endian 字节序格式读取一个有符号 32 位整数
	buf.readUInt32BE(offset[, noAssert])	//根据指定的偏移量，使用指定的 endian 字节序格式读取一个有符号 32 位整数
	//sign
    buf.readInt8(offset[, noAssert])	//据指定的偏移量，读取一个 signed 8 位整数
	buf.readInt16LE(offset[, noAssert])	//根据指定的偏移量，使用特殊的 endian 格式读取一个 signed 16 位整数
	buf.readInt16BE(offset[, noAssert])	//根据指定的偏移量，使用特殊的 endian 格式读取一个 signed 16 位整数
	buf.readInt32LE(offset[, noAssert])	//根据指定的偏移量，使用指定的 endian 字节序格式读取一个 signed 32 位整数
	buf.readInt32BE(offset[, noAssert])	//根据指定的偏移量，使用指定的 endian 字节序格式读取一个 signed 32 位整数
	buf.readFloatLE(offset[, noAssert])	//根据指定的偏移量，使用指定的 endian 字节序格式读取一个 32 位浮点数
	buf.readFloatBE(offset[, noAssert])	//根据指定的偏移量，使用指定的 endian 字节序格式读取一个 32 位浮点数
	buf.readDoubleLE(offset[, noAssert])	//根据指定的偏移量，使用指定的 endian字节序格式读取一个 64 位double
	buf.readDoubleBE(offset[, noAssert])	//根据指定的偏移量，使用指定的 endian字节序格式读取一个 64 位double
	//若参数 noAssert 为 true 将不会验证 offset 偏移量参数。 这意味着 value 可能过大，或者 offset 可能会超出 buffer 的末尾从而造成 value 被丢弃。 除非你对这个参数非常有把握，否则不要使用。默认是 false。
	buf.writeUInt8(value, offset[, noAssert])	//根据传入的 offset 偏移量将 value 写入 buffer。注意：value 必须是一个合法的有符号 8 位整数。	
	buf.writeUInt16LE(value, offset[, noAssert])	//根据传入的 offset 偏移量和指定的 endian 格式将 value 写入 buffer。注意：value 必须是一个合法的有符号 16 位整数。
	buf.writeUInt16BE(value, offset[, noAssert])	//根据传入的 offset 偏移量和指定的 endian 格式将 value 写入 buffer。注意：value 必须是一个合法的有符号 16 位整数。
	buf.writeUInt32LE(value, offset[, noAssert])	//根据传入的 offset 偏移量和指定的 endian 格式将 value 写入buffer。注意：value 必须是一个合法的有符号 32 位整数。 
	buf.writeUInt32BE(value, offset[, noAssert])	//根据传入的 offset 偏移量和指定的 endian 格式将 value 写入buffer。注意：value 必须是一个合法的有符号 32 位整数。 
	buf.writeInt8(value, offset[, noAssert])	
	buf.writeInt16LE(value, offset[, noAssert])	//根据传入的 offset 偏移量和指定的 endian 格式将 value 写入 buffer。注意：value 必须是一个合法的 signed 16 位整数。
	buf.writeInt16BE(value, offset[, noAssert])	//根据传入的 offset 偏移量和指定的 endian 格式将 value 写入 buffer。注意：value 必须是一个合法的 signed 16 位整数。 
	buf.writeInt32LE(value, offset[, noAssert])	//据传入的 offset 偏移量和指定的 endian 格式将 value 写入 buffer。注意：value 必须是一个合法的 signed 32 位整数。
	buf.writeInt32BE(value, offset[, noAssert])	//根据传入的 offset 偏移量和指定的 endian 格式将 value 写入 buffer。注意：value 必须是一个合法的 signed 32 位整数。 
	buf.writeFloatLE(value, offset[, noAssert])	//根据传入的 offset 偏移量和指定的 endian 格式将 value 写入 buffer 。注意：当 value 不是一个 32 位浮点数类型的值时，结果将是不确定的。 若参数 noAssert 为 true 将不会验证 value 和 offset 偏移量参数。 这意味着 value可能过大，或者 offset 可能会超出 buffer 的末尾从而造成 value 被丢弃。 除非你对这个参数非常有把握，否则尽量不要使用。默认是 false。
	buf.writeFloatBE(value, offset[, noAssert])	//根据传入的 offset 偏移量和指定的 endian 格式将 value 写入 buffer 。注意：当 value 不是一个 32 位浮点数类型的值时，结果将是不确定的。 若参数 noAssert 为 true 将不会验证 value 和 offset 偏移量参数。 这意味着 value可能过大，或者 offset 可能会超出 buffer 的末尾从而造成 value 被丢弃。 除非你对这个参数非常有把握，否则尽量不要使用。默认是 false。
	buf.writeDoubleLE(value, offset[, noAssert])	//根据传入的 offset 偏移量和指定的 endian 格式将 value 写入 buffer。注意：value 必须是一个有效的 64 位double 类型的值。
	buf.writeDoubleBE(value, offset[, noAssert])	//根据传入的 offset 偏移量和指定的 endian 格式将 value 写入 buffer。注意：value 必须是一个有效的 64 位double 类型的值。 
	buf.fill(value[, offset][, end])	//使用指定的 value 来填充这个 buffer。如果没有指定 offset (默认是 0) 并且 end (默认是 buffer.length) ，将会填充整个buffer。
```

##### 缓冲区合并	
	Buffer.concat(list[, totalLength])	
List - 用于合并的 Buffer 对象数组列表。
totalLength - 指定合并后Buffer对象的总长度。

### Node.js Express 框架		

* 核心特性	
* 可以设置中间件来响应 HTTP 请求。	
* 定义了路由表用于执行不同的 HTTP 请求动作。	
* 可以通过向模板传递参数来动态渲染 HTML 页面。	
		
安装 Express	npm install express --save	

```
var express = require('express');
var app = express();
```
##### 路由
```
app.method(path, [callback...], callback);
```

###### method 

http请求方法:get, post, put, head, delete, options, trace, copy, lock, mkcol, move, purge, propfind, proppatch, unlock, report, mkactivity, checkout, merge, m-search, notify, subscribe, unsubscribe, patch, search, 和 connect

有些路由方法名不是合规的 JavaScript 变量名，此时使用括号记法，比如： app['m-search']('/', function ...
app.all() 是一个特殊的路由方法，没有任何 HTTP 方法与其对应，它的作用是对于一个路径上的所有请求加载中间件

###### path 服务器上的路径

路由路径和请求方法一起定义了请求的端点，它可以是字符串、字符串模式或者正则表达式。

###### callback 路由匹配时要执行的函数

可以为请求处理提供多个回调函数，其行为类似 中间件。唯一的区别是这些回调函数有可能调用 next('route') 方法而略过其他路由回调函数。可以利用该机制为路由定义前提条件，如果在现有路径上继续执行没有意义，则可将控制权交给剩下的路径。

可以是一个函数、一个函数数组，或者是两者混合

响应对象（res）的方法向客户端返回响应，终结请求响应的循环。如果在路由句柄中一个方法也不调用，来自客户端的请求会一直挂起。
```
res.download()	//提示下载文件。
res.end()	   //终结响应处理流程。
res.json()	   //发送一个 JSON 格式的响应。
res.jsonp()   	//发送一个支持 JSONP 的 JSON 格式的响应。
res.redirect()	//重定向请求。
res.render()	//渲染视图模板。
res.send()	//发送各种类型的响应。
res.sendFile	//以八位字节流的形式发送文件。
res.sendStatus()	//设置响应状态代码，并将其以字符串形式作为响应体的一部分发送。
```

###### app.route()

app.route() 创建路由路径的链式路由句柄
```
app.route('/book')
   .get(function() {})
   .post(function() {})
   .put(function() {});
``` 

###### express.Router

使用 express.Router 类创建模块化、可挂载的路由句柄
```
var express = require('express');
var router = express.Router();
module.exports = router;
```

##### 中间件

Express 是一个自身功能极简，完全是由路由和中间件构成一个的 web 开发框架：从本质上来说，一个 Express 应用就是在调用各种中间件。

中间件的功能包括：

* 执行任何代码。
* 修改请求和响应对象。
* 终结请求-响应循环。
* 调用堆栈中的下一个中间件。

如果当前中间件没有终结请求-响应循环，则必须调用 next()方法将控制权交给下一个中间件，否则请求就会挂起。

Express 应用可使用如下几种中间件：

* 应用级中间件
* 路由级中间件
* 错误处理中间件
* 内置中间件
* 第三方中间件

###### 内置中间件

express.static(root, [options])

express.static 是 Express 唯一内置的中间件。它基于 serve-static，负责在 Express 应用中提托管静态资源。

参数 root 指提供静态资源的根目录。

可选的 options 参数拥有如下属性。

| 属性	     |描述	                                                                            |类型	|缺省值      |
| -----------|--------------------------------------------------------------------------------- |-------|----------- |
| dotfiles	 | 是否对外输出文件名以点（.）开头的文件。可选值为 “allow”、“deny” 和 “ignore”		|String |“ignore”    |
| etag	     | 是否启用 etag 生成																|Boolean|true		 |
| extensions | 设置文件扩展名备份选项															|Array	|[]          |
| index	     | 发送目录索引文件，设置为 false 禁用目录索引。									|Mixed	|“index.html”|
| lastModifie| 设置 Last-Modified 头为文件在操作系统上的最后修改日期。可能值为 true 或 false。	|Boolean|true		 |
| maxAge	 | 毫秒或者其字符串格式设置 Cache-Control 头的 max-age 属性。						|Number |0			 |
| redirect	 | 当路径为目录时，重定向至 “/”。													|Boolean|true		 |
| setHeaders | 设置 HTTP 头以提供文件的函数。													|Function	 		 |

###### webpack-dev-middleware

```
var express = require('express'),
	config = require('./webpack.config'),
	app = express(),
	devMiddleware = require('webpack-dev-middleware');

app.use(devMiddleware(webpack(config), {
	noInfo: true,
	publicPath: config.output.publicPath
}));
```
