# 友好的React介绍

总结React的特点：

- 虚拟DOM（操作React要比操作原生的DOM api更容易。写React是在通过操作React Element来代替直接操作DOM，虚拟DOM还能解决兼容性的问题，并提升性能）
- 声明式
- 基于组件：方便复用 + 降低代码之间的耦合

```html
<body>
    <scripts>
        //通过DOM向页面添加一个div
        const div = document.createElement('div');
        div.innterText = '你好我是一个div';
        const root = document.getElementById('root'); //获取root
        root.appendChild(div);
    </scripts>
</body>
```

如果是通过react来写的话：

```html
<body>
    <scripts>
        //通过react向页面添加一个div
        const div = React.createElement('div', {}, '我是React创建的div'); //创建一个React元素
        //获取根元素对应的React元素
        const root = ReactDOM.createRoot(documnet.getElementById('root'));
        //将div渲染到根元素中
        root.render(div);
    </scripts>
</body>
```

## 1. 三个API

`React.createElement()`：用来创建一个React元素（虚拟DOM），这个方法接收如下3个参数：

1. 组件名称（如果是html标签那么需要全部小写）

2. 组件的属性（class属性要写成className，事件属性要写成驼峰命名法）

3. 组件的子元素/内容

   React元素最终会转换为真正的DOM元素。要注意的是，React元素一经创建就无法再修改，只能通过创建新的React元素进行替换（在实际开发中，这属于底层的过程，是由React自己帮我们完成的）。

`ReactDOM.createRoot()`：用来创建React的根容器，这个容器用来放React元素

`root.render()`：用来将React元素渲染到根元素中。在首次渲染时，根元素原先的所有内容都会被React元素替换掉。后续，当重复调用`.render()`来渲染页面时，会使用DOM差分算法（diff alg.）来进行高效的更新（React会比较两次渲染的React元素，在真实DOM中只会去更新发生了修改的那部分，没有发生变化的部分保持不变，也就是确保只对真实DOM做最小的修改）。

## 2. JSX (JavaScript Syntax Extension)

JSX是JS的语法扩展，让我们能够用类似HTML的形式写JS。JSX是React中声明式编程的体现。

我们写了JSX以后，JSX要被Babel翻译成原始的JS代码，才能被React执行。也就是说所有JSX最终都会转换为通过调用`React.createElement()`去创建React元素的代码。可以说，**JSX就是`React.createElement()`的语法糖**，JSX在被React执行之前都会被babel转换为JS代码。

```javascript
//命令式编程
const button = React.createElement('button', {}, '我是按钮嘿嘿');

//声明式编程，结果导向的
const button = <button>我是按钮嘿嘿</button>;
```

JSX的语法：

1. JSX中html标签要小写，React组件开头要大写；
2. 在JSX中可以用`{}`来嵌入表达式。如果表达式的值是Null、布尔值或undefined这些，这个标签就不会显示；
3. JSX中有且只能有一个根标签；
4. JSX标签必须正确闭合，自结束标签必须写`/`；
5. 在JSX中，属性可以直接在标签中设置，需要注意的是class属性要写成className、所有原始css样式中的`-`要写成驼峰命名、以及style中必须使用对象（`style={{}}`）来配置（原始css中是写成字符串，比如`background-color:"yellow"`）。

看一个例子：

```javascript
const div = <div id='box' onClick={()=>{alert('好饿')} className='box01' style={{backgroundColor:"yellow"}}}
			我是一个div
            </div>
```

## 3. 虚拟DOM

我们在React中操作的元素是React Elements，而非真正的原生DOM元素。React通过虚拟DOM 将React元素和原生DOM进行映射，虽然操作的是React Elements，但是这些操作最终会在真实DOM上体现出来。

使用虚拟DOM的优势：降低操作API的复杂度、解决浏览器兼容性的问题、提升性能（确保只对真实DOM做最小的修改）。

DOM diff算法，简单来说是在比较两次所需渲染的数据时，先比较父元素，父元素如果不同，直接替换所有元素；父元素一致，再逐个比较子元素，直至找到所有发生修改的元素为止。

## 4. 渲染列表

为了承接着前一段React渲染页面的问题，这里来看看为什么在JSX里面传数组时，数组的每个元素都要设置一个唯一的key。

因为在重新渲染页面时，React默认按照顺序依次比较对应的元素，那么如果我们渲染一个列表时不为数组元素指定key，在列表内元素顺序发生变化时，渲染性能会出现问题。比如，我在列表中最前面插入了一个新元素，其余元素内容不变，但是它们的位置都发生变化了，由于React默认按照位置顺序比较元素，此时重新渲染时所有元素都会被修改。为了解决这一问题，开发中一般会采用数据的id作为数组元素的key，再设置了key以后，React就会根据key来比较元素是否有修改，而不再按照顺序比较。

# 认识一下React项目

## 1. 项目目录

常规的React项目需要使用包管理器来对项目进行管理（通过npm管理的项目必须经过webpack打包才能在浏览器上运行），React官方也提供了react-scripts包，其中包含项目开发中所需要的大部分依赖（比如webpack及其配置），简化了开发过程。

React项目的目录结构：

```html
根目录
	- public //静态资源目录
		- index.html（添加标签<div id="root"></div>）
                               //index.html是首页模板。webpack编译时以此为模板生成index.html
	- src  //源码
		- App.js
		- index.js     //index.js是源码的入口文件，webpack在打包时以此为入口对js进行编译，然后再关联到index.html里面
```

## 2. 常用的命令

在package.json中加入以下代码，使我们调用命令更便捷：

```json
"scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build"
}
```

在开发的时候，运行测试服务器，用`start`

在开发完成之后，再用`build`打包，部署到服务器上。

此外，也可以在package.json中加入eslint来帮忙检查语法：

```json
"eslintConfig": {
    "extends": [
        "react-app"
    ]
}
```

## 3. 在React项目中编写样式

样式对应的js文件在哪里，样式文件也要在哪里，并且名称应该一致。如下所示，这样一来，我们就能快速判断出`index.css`是对`index.js`这个文件生效的，更容易维护。

```html
根目录
	- public
		- index.html（添加标签<div id="root"></div>）
	- src  
		- App.js
		- index.js     
		- index.css
```

在`index.js`中引入`import './index.css';`即可使样式生效。

# React的组件思想

组件是独立可复用的代码片段（降低耦合、方便复用），React允许我们将标签、CSS和JS组合成自定义组件。

React中定义组件的方式有两种：类组件和函数式组件。函数式组件就是一个返回JSX标签的普通JS函数，组件的首字母必须是大写的。

在开发中，每一个组件都应该是一个独立的文件。

```javascript
//这里是App.js文件
const APP = () => {
    return <div>我是App组件！</div>
}

export default App;
```

要使用这个组件时，通过`import App from "./App.js;"`引入并通过`</App>`或者`App()`的方式调用即可。这是 React 的神奇之处：可以只定义组件一次，然后按需多处和多次使用。

注意，其他的组件及其相应的样式文件一般都写在`./src/Components`目录下，并根据项目结构的情况再细分。











































