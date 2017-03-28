#gulp	

###	安装		
 	npm install --global gulp	//全局安装	
 	npm install --save-dev gulp	//开发依赖,安装到当前项目并在package.json中添加依赖		
				
### package.json	
```json
	{
	  "name": "vueDemo", //项目名称
	  "version": "1.0.0",
	  "description": "txbManager: An Ionic project",
	  "dependencies": {},
	  "devDependencies": {
	    "gulp-less": "^3.3.0",
	    "gulp-uglify": "^2.0.0"
	  }
	}
```		
### gulpfile.js	
```javascript
	var gulp = require('gulp');

	gulp.task('default', function() {
	  // 将你的默认的任务代码放在这
	});
```		
				
### 执行
```javascript
	gulp	//默认的名为 default 的任务（task）将会被运行
	gulp <task> <othertask>		//单独执行特定的任务（task）
```
				
##### 参数标记	
* -v 或 --version	会显示全局和项目本地所安装的 gulp 版本号		
* --require <module path> 	将会在执行之前 reqiure一个模块。这对于一些语言编译器或者需要其他应用的情况来说来说很有用。你可以使用多个--require		
* --gulpfile <gulpfile path> 	手动指定一个 gulpfile 的路径，这在你有很多个 gulpfile 的时候很有用。这也会将 CWD 设置到该 gulpfile 所在目录		
* --cwd <dir path\> 	手动指定 CWD,定义 gulpfile 查找的位置，此外，所有的相应的依赖（require）会从这里开始计算相对路径		
* -T 或 --tasks 	会显示所指定 gulpfile 的 task 依赖树		
* --tasks-simple	会以纯文本的方式显示所载入的 gulpfile 中的 task 列表		
* --color	强制 gulp 和 gulp 插件显示颜色，即便没有颜色支持		
* --no-color	强制不显示颜色，即便检测到有颜色支持		
* --silent 	禁止所有的 gulp 日志		
				
### API	
##### gulp.src(globs[, options])	
输出（Emits）符合所提供的匹配模式（glob）或者匹配模式的数组（array of globs）的文件。 将返回一个 `Vinyl files` 的 `stream` 它可以被 `piped` 到别的插件中。

###### globs    
类型： `String` 或 `Array`      
所要读取的 glob 或者包含 globs 的数组。

###### options   
类型： `Object `    
通过 glob-stream 所传递给 node-glob 的参数。
除了 node-glob 和 glob-stream 所支持的参数外，gulp 增加了一些额外的选项参数：

###### options.buffer   
类型： Boolean 默认值： true
如果该项被设置为 false，那么将会以 stream 方式返回 file.contents 而不是文件 buffer 的形式。这在处理一些大文件的时候将会很有用。
**注意：**插件可能并不会实现对 stream 的支持。

###### options.read    
类型： Boolean 默认值： true
如果该项被设置为 false， 那么 file.contents 会返回空值（null），也就是并不会去读取文件。

###### options.base    
类型： String 默认值： 将会加在 glob 之前 (请看 glob2base)

##### gulp.dest(path[, options])	
被 pipe 进来，并且将会写文件。并且重新输出（emits）所有数据，因此你可以将它 pipe 到多个文件夹。如果某文件夹不存在，将会自动创建它	

###### path
类型： String or Function
文件将被写入的路径（输出目录）。也可以传入一个函数，在函数中返回相应路径，这个函数也可以由 vinyl 文件实例 来提供。

###### options
类型： Object

###### options.cwd
类型： String 默认值： process.cwd()
输出目录的 cwd 参数，只在所给的输出目录是相对路径时候有效。

###### options.mode
类型： String 默认值： 0777
八进制权限字符，用以定义所有在输出目录中所创建的目录的权限。	

##### gulp.task(name[, deps], fn)	
定义一个使用 Orchestrator 实现的任务（task）	

###### name
任务的名字，如果你需要在命令行中运行你的某些任务，那么，请不要在名字中使用空格。

###### deps
类型： Array
一个包含任务列表的数组，这些任务会在你当前任务运行之前完成。
**注意：** 你的任务是否在这些前置依赖的任务完成之前运行了？请一定要确保你所依赖的任务列表中的任务都使用了正确的异步执行方式：使用一个 callback，或者返回一个 promise 或 stream。

###### fn
该函数定义任务所要执行的一些操作。通常来说，它会是这种形式：`gulp.src().pipe(someplugin())`。

异步任务支持

任务可以异步执行，如果 fn 能做到以下其中一点：

+ 接受一个 callback
+ 返回一个 stream
+ 返回一个 promise

注意： 默认的，task 将以最大的并发数执行，也就是说，gulp 会一次性运行所有的 task 并且不做任何等待。如果你想要创建一个序列化的 task 队列，并以特定的顺序执行，你需要做两件事：

+ 给出一个提示，来告知 task 什么时候执行完毕，并且再给出一个提示，来告知一个 task 依赖另一个 task 的完成。
+ 对于这个例子，让我们先假定你有两个 task，"one" 和 "two"，并且你希望它们按照这个顺序执行：


##### gulp.watch(glob [, opts], tasks)	
监视文件，并且可以在文件发生改动时候做一些事情。它总会返回一个 EventEmitter 来发射（emit） change 事件		

###### glob
类型： String or Array
一个 glob 字符串，或者一个包含多个 glob 字符串的数组，用来指定具体监控哪些文件的变动。

###### opts
类型： Object
传给 gaze 的参数。

###### tasks
类型： Array
需要在文件变动后执行的一个或者多个通过 gulp.task() 创建的 task 的名字

##### gulp.watch(glob[, opts, cb])

###### glob
类型： String or Array
一个 glob 字符串，或者一个包含多个 glob 字符串的数组，用来指定具体监控哪些文件的变动。

###### opts
类型： Object
传给 gaze 的参数。

###### cb(event)
类型： Function
每次变动需要执行的 callback。

callback 会被传入一个名为 event 的对象。这个对象描述了所监控到的变动：

###### event.type
类型： String
发生的变动的类型：added, changed 或者 deleted。

###### event.path
类型： String
触发了该事件的文件的路径。	

### node-glob
```javascript	
	js/app.js    //精确匹配文件		
	js/*.js      //仅匹配js目录下的所有后缀为.js的文件		
	js/*/.js     //匹配js目录及其子目录下所有后缀为.js的文件		
	!js/app.js   //从匹配结果中排除js/app.js，这种方法在你想要匹配除了特殊文件之外的所有文件时非常管用		
	*.+(js|css)  //匹配根目录下所有后缀为.js或者.css的文件	
```	

				
### 插件	
* gulp-rename	重命名		
* gulp-uglify	js压缩		
* gulp-clean-css	css压缩		
* gulp-minify-html	html压缩		
* gulp-concat	文件合并		
* gulp-jshint	js代码检查		
* gulp-less	编译less		
* gulp-autoprefixer			
* gulp-imagemin	图片压缩	imagemin-pngquan	png图片压缩插件		
* 静态资源防缓存使用gulp-rev和gulp-rev-collecto			
* browser-sync	require('browser-sync').create(); 	实时刷新	
* require('gulp-clean');  	清空文件夹里所有的文件	
				 
