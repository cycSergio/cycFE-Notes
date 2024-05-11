# Object in JS

JS有8种数据类型，其中7种是原始类型，最后一种就是对象。

JS中的对象可以看作是属性的集合，一个属性就是一个`key:value`的键值对，其中key是一个字符串或symbel（也叫做属性名），value可以是任何类型（比如可以是函数，这样的属性就叫做方法）。

在JS中有两种创建对象的方法：

第一种是通过调用类的构造函数来创建，就像在Java中那样：

```javascript
let myCat = new Object(); //调用类的构造函数
```

第二种是用JS的字面量语法：

```javascript
let cat = {}; //"字面量"的语法
```

创建完一个对象以后，有两种访问其属性的方法：dot notation（`cat.key`）和方括号（`cat[key]`）。其中，方括号的方式可以从变量中获取key。

若要遍历对象的key，用`for ... in`：

```javascript
for (let key in cat) {
    alert(key);
    alert(cat[key]);
}
```

# JS中的对象引用和复制

> 对象与原始值的根本区别之一在于，对象是“通过引用”存储和复制的，而原始值等总是“作为一个整体”复制。
>

## JS中对象的应用

一开始我觉得这就是pass by reference（其实是指针的赋值） 和 pass by value的区别，查了一下发现还有pass by sharing的区分？开始疯狂挠头。关于JS中的这一点（Python也一样），感觉比较清楚的回答是[下面这个](https://stackoverflow.com/a/38533677/24989889)（像是刚学Python时，去tutor上跑一下就能回忆起来）：

>
> Function arguments are passed either by-value or by-sharing, but never **ever** by reference in JavaScript!
>
> ## Call-by-Value
>
> Primitive types are passed by-value:
>
> ```js
> var num = 123, str = "foo";
> 
> function f(num, str) {
>   num += 1;
>   str += "bar";
>   console.log("inside of f:", num, str);
> }
> 
> f(num, str);
> console.log("outside of f:", num, str);
> ```
>
> **Reassignments** inside a function scope are not visible in the surrounding scope.
>
> ## Call-by-Sharing
>
> Objects, that is to say all types that are not primitives, are passed by-sharing. A variable that holds a reference to an object actually holds merely a copy of this reference. If JavaScript would pursue a *call-by-reference* evaluation strategy, the variable would hold the original reference. This is the crucial difference between by-sharing and by-reference.
>
> What are the practical consequences of this distinction?
>
> ```js
> var o = {x: "foo"}, p = {y: 123};
> 
> function f(o, p) {
>   o.x = "bar"; // Mutation
>   p = {x: 456}; // Reassignment
>   console.log("o inside of f:", o);
>   console.log("p inside of f:", p);
> }
> 
> f(o, p);
> 
> console.log("o outside of f:", o);
> console.log("p outside of f:", p);
> ```
>
> **Mutating** means to modify certain properties of an existing `Object`. The reference copy that a variable is bound to and that refers to this object remains the same. Mutations are thus visible in the caller's scope.
>
> **Reassigning** means to replace the reference copy bound to a variable. Since it is only a copy, other variables holding a copy of the same reference remain unaffected. Reassignments are thus not visible in the caller's scope like they would be with a *call-by-reference* evaluation strategy.

上文Call-by-Sharing中的例子，在Python中也是同样的结果。感觉很合理，这个时候看下面这个回答也觉得是合理的但没必要把概念绕到这个样子：

> Primitives are passed by value, and Objects are passed by "copy of a reference".
>
> Specifically, when you pass an object (or array) you are (invisibly) passing a reference to that object, and it is possible to modify the *contents* of that object, but if you attempt to overwrite the reference it will not affect the copy of the reference held by the caller - i.e. the reference itself is passed by value:
>
> ```js
> function replace(ref) {
>     ref = {};           // this code does _not_ affect the object passed
> }
> 
> function update(ref) {
>     ref.key = 'newvalue';  // this code _does_ affect the _contents_ of the object
> }
> 
> var a = { key: 'value' };
> replace(a);  // a still has its original value - it's unmodfied
> update(a);   // the _contents_ of 'a' are changed
> ```

## 2. JS中对象的复制

普通数据类型拷贝的是值本身，而对象类型拷贝的是对象的引用。

对象通过引用被赋值和拷贝。此时一个变量存储的不是“对象的值”，而是一个对值的“引用”（内存地址）。因此，浅拷贝此类变量或将其作为函数参数传递时，拷贝的是引用而非对象本身。所有通过浅拷贝引用的操作（如添加、删除属性）都作用在同一个对象上。

如何在JS中做深拷贝似乎算是常见的面试题，真正的深拷贝API是`_.cloneDeep(obj)`，用递归来检查`Obj[key]`（想要拷贝的对象）的每个值是不是还是对象。关于JS中对象复制更详细的解释，详见[JS现代教程](https://zh.javascript.info/object-copy)。

## *关于pass by value/reference更多的讨论

![image-20240511223042845](E:\搓一搓前端笔记\JavaScript\assets\image-20240511223042845.png)

好吧，我能很明白pass by value和所谓pass by sharing，但还是感觉很奇怪，或许应该通过C/C++来更深刻地理解pass by reference（今天学到了值语义（改变一个对象的副本不应该改变这个对象及其其他副本）&引用语义）。不过[这个回答]()让我非常满意和安心。















