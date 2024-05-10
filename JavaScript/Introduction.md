# Defining Functions in JS

在JS中创建函数有3种方式：函数声明、函数表达式，和箭头函数。

**函数声明**：类似于Python中用`def`来声明函数，见下

```python
def sum(a, b) {
    print(a + b);
}
```

在JS中用`function`来声明函数，见下例：

```javascript
sum(123, 456);

function sum(a, b) {
    return a + b;;
}
```

在上例中，我们在函数声明被定义之前就调用了函数，这是可以的，因为*函数声明是可以提升的*。因为JavaScript引擎会在运行脚本前先创建完所有的全局的函数声明，然后才执行脚本。

此外，函数声明具有块级作用域，声明的函数在该代码块内的任何位置都可见，但是在代码块外部是不可见的。

**函数表达式**：上面所说的函数声明是独立的声明语句，但是函数表达式不是，它是在一个表达式中被创建的，见下例：

```javascript
let sum = function(a, b) {
    return a + b;
}
```

不同于函数声明，函数表达式不会提升。函数表达式，只有在代码执行到达时才会被创建。

**箭头函数**：语法上，箭头函数中的`=>`是用来代替函数关键字的，箭头的左侧是要接收的参数，右侧是函数的返回值（如果只需要一个表达式，直接写即可，不需要加大括号和`return`；不然还是需要在`{}`里写，就像写普通的函数体一样。）。根据参数数量`>1` or `=1` or `=0`， 箭头函数的写法分别如下：

```javascript
//函数参数数量 > 1
let sum = (a, b) => a + b;

//函数只有1个参数
let square = n => n * n;

//函数没有参数
let test = () => console.log("test");
```

# 严格模式

JS中运行代码的模式有两种：

- 正常模式，这是默认的模式，语法检查不严格，语法不规范可能导致代码运行性能较差；
- 严格模式，语法检查更严格，性能能够提升。可以在脚本最顶部或者是函数体开头（这样就只在该函数中启用严格模式）通过`"use strict"`来在全局或者局部开启严格模式。在开发中应尽量使用严格模式，不仅可以避免一些隐蔽的问题，也可以提升代码的运行性能。

此外，JS中更高级的语言结构"class"和"module"会自动启用严格模式。

# OOP, FP

面向对象编程：程序中所有操作都是通过对象来完成。做任何事情之前都需要先找到它的对象。

<a href="http://denero.org/" style="color:#c05b4d;">John DeNero</a>在他的<a href="https://www.composingprograms.com/pages/25-object-oriented-programming.html" style="color:#c05b4d;">Composing Programs 2.5.8 The Role of Objects</a>中是这样说的：

> Object-oriented Programming is particularly well-suited to programs that model systems that have separate but interacting parts. ... On the other hand, classes may not provide the best mechanism for implementing certain abstractions. Functional abstractions provide a more natural metaphor for representing relationships between inputs and outputs.
>
> Multi-paradigm languages such as Python allow programmers to match organizational paradigms to appropriate problems. Learning to identify when to introduce a new class, as opposed to a new function, in order to simplify or modularize a program, is an important design skill in software engineering the deserves cafeful attention.

