# CommonJS模块化

将所有的代码写在一个文件中是难以维护的。为此，需要模块化：按照不同的功能将代码拆分为多个代码片段，每个代码片段就是一个模块。

CommonJS并不是ES官方的规范，而是社区第三方自己定义的模块化规范。直到2015年，JavaScript中一直都没有一个内置的模块化系统。commonJS是社区涌现的众多模块化规范中的佼佼者，**同时它也是Node.js中默认使用的模块化标准**。

在CommonJS中，一个JS文件就是一个模块。在定义模块时，模块中的内容默认是不能被外部看到的，也就是模块内部的变量默认都是私有的，不能被外部访问。可以通过`exports`来设置要向外暴露的内容。**CommonJS实际上就是一个闭包。**

## `exports.xx = `, `module.exports = {}`

导出模块，访问exports的方式有两种:
`exports`
`module.exports`

- 当我们在其他模块中引入当前模块时，require函数返回的就是exports
- 可以将希望暴露给外部模块的内容设置为exports的属性，比如`exports.要暴露的对象 = {}`就是在导出这个对象
- 可以通过`module.exports`同时导出多个值，

  ```javascript
  module.exports = {
  	a:"哈哈",
  	b:[1, 2, 3],
  	c:()=> {
  		console.log(111)
  	}
  }
  ```

  ## 引入模块

- 使用`const obj = require("模块的路径”)`函数来引入模块。这里没有别名的问题，因为是用变量来接收模块的。因此也可以解构赋值。
- 引入自定义模块时
- 模块名要以`./`或 `../`开头
- 扩展名可以省略
  - 在CommonJS中，如果省略JS文件的扩展名,node会自动为文件补全扩展名；如果没有同名的JS文件，它会寻找Json
  优先级JS > json > node (特殊)

- 引入核心模块时，直接写核心模块的名字即可，也可以在核心模块前添加`node:`
- require()是同步加载模块的方法，所以无法用来加载ES6（异步的）的模块。当我们需要在CommonJS中加载ES模块时，需要通过`import()`方法来加载
- 如果要引入一个文件夹作为一个模块：`index.js`是文件夹默认的入口文件。当我们使用一个文件夹作为模块时，文件夹中必须有一个模块的主文件。如果文件夹中含有`package.json`文件且文件中设置`main`属性，则`main`属性指定的文件会成为主文件，导入模块时就是导入该文件。如果没有`package.json`，则node会按照`index.js`, `index.node`的顺序寻找主文件。

## CommonJS的执行原理：

每一个CommonJS模块在执行时，外层都会被套上一个函数：

```js
(function(exports, require, module, __filename, __dirname) {
    //模块内的代码会被放到这里
})

//exports, 用来设置模块向外部暴露的内容
//require, 用来引入模块的方法
//module, 当前模块的引用
//__filename,模块的路径
//__dirname，模块所在目录的路径
```

这也是我们能够在CommonJS模块中使用`exports`, `reuqire`的原因，它们并不是全局变量，而是以参数的形式被传递进了模块中。

# ES模块化

Node.js同样也支持ES。现在还是用CommonJS比较多，因为用得时间长。不过ES Module实际上更好一些，性能更好，不过使用起来会比CommonJS更麻烦一点。默认情况下，node中的模块化标准是CommonJS，要想使用ES的模块化，可以采用以下两种方式：

1. 使用mjs作为扩展名，这种方式适用于单个文件；
2. 修改package.json将模块化规范设置为ES模块。当我们设置"type":"module" 当前项目下所有的js文件都默认为ES Module。



## ES模块导出

```javascript
export let a = 10;
export const b = '狗仔';
export const c = {name: '猫仔'};

//一个模块中，只能设置一个默认导出
export default function sum(a, b) {
    return a + b;
}
```

## ES模块引入

```javascript
import mySum, {a, b, c as obj} from "./xx.mjs"; //非默认导出的引入可用as来指定别名
//默认导出的任容，可以随意命名，也不需要放进{}

//用*来引用所有，但是要尽量避免import *这种情况，要做到按需引用
```

通过ES模块化导入的内容，都是常量。常量锁的是变量，但是不影响对对象的修改。比如`obj.name = 莎士比亚`还是可以的。

以及，ES模块都是运行在严格模式下的。最后，ES模块化在浏览器中也支持，但是考虑到兼容性的问题，我们通常不会直接使用，而是结合打包工具去使用哦。

# node.js的核心模块

核心模块是node中的内置模块，这些模块有的可以直接在node中使用，有的直接引入即可使用。
`window` 是浏览器的宿主对象,`global` 是node中的全局对象，作用类似于`window`。ES标准下，全局对象的标准名是 `globalThis`。

## process 

process模块用来表示和控制当前的node进程，通过该独享可以获取进程的信息，或者对进程进行各种操作。`process`是一个全局变量。

- `process.exit([code]) `结束当前进程，终止node

- `process.nextTick(callback[, .args])`向tick任务队列中添加任务。tick队列中的代码，会在下一次事件循环之前执行，也就是会在微任务队列和宏任务队列之前执行。因此此处的执行顺序是：调用栈，然后tick队列，然后微任务队列，最后宏任务队列。

  tick队列是node中独有的，在早期还没有微任务队列的时候，node整了这个tick队列，所以相当于老版的微任务队列。现在用得不多了。真是超前的node，挠头。见下例：

  ```javascript
  setTimeout(()=> {
      console.log(1); //宏任务队列
  })
  
  queueMicrotask(()=> {
      console.log(2); //微任务队列
  })
  
  process.nextTick(()=> {
  	console.log(3);//tick队列
  })
  
  console.log(4); //调用栈
  ```

## Path 

path模块可以帮助我们获取文件 (夹)的路径。
`path.resolve([..paths])`生成绝对路径。如果将一个相对路径作为参数，resolve会自动将其转换为绝对路径。此时根据工作目录的不同，产生的绝对路径也不同。所以，通常都用`__dirname`作为第一个参数，一个相对路径作为第二个参数，它会自动计算出最终的路径。也就是实际中要使用如下写法：

```javascript
const result = path.resolve(__dirname, "./hello.js")
```

## Fs

fs模块让node能够操作磁盘中的文件，文件操作也就是I/O, input， output这些。使用fs模块需要引入。

当我们用fs模块读取磁盘中给的数据时，所读取到的数据总以Buffer对象的形式返回，Buffer是一个临时用来存储数据的缓冲区。

`fs.readFileSync()`是同步地读取文件的方法，会阻塞后边代码的执行，不好。

``fs.readFil() `是异步地读取文件：

```javascript
//readFile()异步的读取文件的方法
fs.readFile(
	path.resolve(__dirname, "./hello.txt"),
    (err, buffer) => {
        if (err) {
            console.log("出错了")
        } else {
            console.log(buffer.toString())
        }
    }
)
```

异步地读取文件写起来比较长，以及只要用回调，就会有回调地狱的问题。所以以上两种写法都不是常用的写法，应该用Promise来写fs：

```javascript
const fs = require("node: fs/promises");

fs.readFile(path.resolve(__dirname, "./hello.txt"))
	.then(buffer => {
    	console.log(buffer.toString())
	})
	.catch(e => {
    console.log("出错了")
	})

//也可以写成语法糖
;(async () => {
    try {
        const buffer = await fs.readFile(path.resolve(__dirname, "./hello.txt"));
        console.log(buffer.toString())
    } catch (e) {
        console.log("出错了")
    }
}
)()
```