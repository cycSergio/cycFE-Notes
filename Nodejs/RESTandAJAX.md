**传统的服务器**：浏览器给服务器发送请求，服务器返回的响应是一个响应报文，实际上是直接给浏览器返回一个html页面，浏览器只需要显示这个html页面即可，不需要再做其他的操作——这就是历史悠久的MVC模型，model指数据模型，view是用来呈现数据的视图，controller是控制器，负责加载数据并选择视图来呈现数据（负责调度）。

但是这样的模式并不能适用于现在的应用场景。现在的应用场景是，一个应用通常会有多个客户端（client）存在，比如web端、移动端（app)、pc端，这些都是客户端。但是在MVC模式下，服务器给客户端返回的是一个html页面，其实只能给web端提供服务，另外其他类型的客户端消化不了。这样一来，其他类型的客户端还需要单独开发服务端，大大提高了开发和维护的成本。

换句话说，这是耦合的问题。传统的服务器需要做两件事：加载数据，然后将数据模型渲染进视图。解决方案就是将渲染视图的工作从服务器中剥离出来，服务器只负责向客户端返回数据，渲染视图的工作由客户端自行完成。

分离以后，服务器只提供数据，一个服务器可以同时为多种客户端提供服务器，同时将视图渲染的工作交给客户端以后，也简化了服务器代码的编写。

从传统服务器到现代服务器的过渡，是前后端分离的过程。

# REST

Rest实际上是一种服务器的设计风格， 现在的服务器大多都是Rest风格的服务器，它的主要特点就是服务器只负责返回数据，实现前后端的分离。

服务器和客户端传输数据时通常使用 JSON 格式的数据。后端给前端的是JSON格式的数据，前端最好也给后端JSON格式的数据，这样处理起来比较统一。

传统服务器中客户端请求的方式只有GET和POST，Rest中请求的方法更多一些。

- GET：加载数据。想从服务器加载数据时，发GET请求
- POST：用来给服务器新建或者添加数据
- PUT：添加或修改数据
- PATCH：修改数据
- DELETE：删除数据
- OPTION：一般由浏览器自动发送，检查请求的一些权限

要根据不同的场景选择合适的方式，写路由的时候要注意，按照规范去写，否则api就变混乱了（应该有统一的api，统一的方法，统一的返回结果这些方面的约定/规范）。

此处含义的路由，也会被人叫成API（接口）/Endpoint（端点）。

此处路由是由两个东西所定义的，请求的方法 + 路径。

## postman

可以模拟各种方式的请求，用来对接口进行调试/测试的小工具。

## 常见的状态码

The status codes are divided into five categories.

- **[1xx: Informational](https://restfulapi.net/http-status-codes/#1xx)** – Communicates transfer protocol-level information.
- **[2xx: Success](https://restfulapi.net/http-status-codes/#2xx)** – Indicates that the client’s request was accepted successfully.
- **[3xx: Redirection](https://restfulapi.net/http-status-codes/#3xx)** – Indicates that the client must take some additional action in order to complete their request.
- **[4xx: Client Error](https://restfulapi.net/http-status-codes/#4xx)** – This category of error status codes points the finger at clients.
- **[5xx: Server Error](https://restfulapi.net/http-status-codes/#5xx)** – The server takes responsibility for these error status codes.

可以详见[这个](https://restfulapi.net/http-status-codes/)。

| Status                    | Description                                                  |
| ------------------------- | ------------------------------------------------------------ |
| 400 Bad Request           | 表示由于客户端发出的请求存在明显错误/无效，服务器不会对该请求进行处理 |
| 401 Unauthorized          | Indicates that the request requires user authentication information. The client MAY repeat the request with a suitable Authorization header field |
| 404 Not Found             | 请求的资源在服务器上未找到                                   |
| 405 Method Not Allowed    | 表示客户端使用的HTTP请求方法在目标资源上不允许。服务器收到了请求，但不支持客户端使用的请求方法（例如GET、POST、PUT等） |
| 418 I’m a teapot          | 表示服务器拒绝冲泡咖啡，因为它是teapot。（RFC 2324超文本Coffee Pot控制协议，幽默） |
| 500 Internal Server Error | The server encountered an unexpected condition that prevented it from fulfilling the request. 没有给出具体错误信息 |
| 502 Bad Gateway           | 作为网关或代理的服务器在尝试执行请求时，从上游服务器收到的是无效响应 |
| 503 Service Unavailable   | 表示服务器暂时无法处理请求，通常是由于服务器过载或正在维护，过一段时间会恢复 |
| 504 Gateway Timeout       | 作为网关或代理的服务器在尝试执行请求时，未能在预期时间内从上游服务器接收到响应 |



# AJAX

在项目上线的时候，客户端和服务器一般不会部署到一起，而是分开部署到两个服务器上。

那么在客户端部分的代码中，就会需要向服务器发送请求。这就需要用到AJAX（Asynchronous JavaScript and XML），其功能就是通过JS向服务器发送请求来加载数据。XML是早期AJAX使用的数据格式，比较大，不好，现在都是用JSON，JSON小很多，传输速度快。

因为AJAX是异步的、非阻塞的，所以在通过AJAX来向服务器发送请求时，不会影响到页面上的其他功能。

现在可选择的方案有：

- XMLHttpRequest (xhr)
- Fetch
- Axios：这个用得比较多，前两个都比较老了



## 跨域 CORS

> [!NOTE]
>
> **Same-Origin Policy**: A rule that prevents one website from tampering with other unrelated websites. This rule is enforced by the *web browser*.
>
> 但是，websites can agree to allow some limited sharing，比如Cross-origin resource sharing (CORS).

**origin**: Every webpage has an origin defined by its URL  with 3 parts: protocol + domain + port.

- protocol: HTTP还是HTTPS
- domain: 就是域名呀
- port: if no port is specified, the default is 80 for HTTP and 443 for HTTPS

eg, `http://cs161.org/evan/bot`和`http://cs161.org/mallory`就是same origin的。



如果两个网站的协议、域名以及端口号这三个中任一不同，就算跨域。当我们通过AJAX向服务器发送跨域请求时，浏览器为了服务器的安全，会阻止JS读取到服务器的数据。解决方案：在服务器中设置一个允许跨域的头：**Access-Control-Allow-Origin**，允许哪些客户端/哪些路径访问我们的服务器。这个响应头应该设置在一个中间件中。

```javascript
//设置响应头
//*表示任何客户端都可以访问我
res.setHeader("Access-Control-Allow-Origin", "*");
//Access-Control-Allow-Origin设置指定值的时候只能设置一个，不能设置多条路径
res.setHeader("Access-Control-Allow-Origin", "http://denero.example");
//如果要允许多条路径来访问，那可以用一个数组，然后在请求到来以后，先检查这个路径是否在我的数组中，然后再动态地去设置Access-Control-Allow-Origin这个响应头。
```

跨域相关的头不止这一个，见下：

```javascript
app.use((req, res, next) => {
    res.setHeader("Access-Control-Allow-Origin", "*"");
    res.setHeader("Access-Control-Allow-Methods", "GET, POST, ");//表示跨域时允许的请求方式
    res.setHeader("Access-Control-Allow-Headers", "Content-type");//表示跨域时允许传递的请求头
 
    next();
})
```

有关跨域更详细的信息可见[MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)。

## xhr

向服务器发请求，也就是在向服务器发送请求报文，既然如此，我们需要先获取到表示请求的这个对象。此外，要读取响应的时候，要注意AJAX是异步的：

```javascript
//创建一个新的xhr对象，xhr表示请求信息
const xhr = new XMLHttpRequest();
//设置请求的信息
xhr.open("GET", "http://localhost:3000/hi");
//发送请求
xhr.send();

//设置响应体的类型，设置后会自动对数据进行类型转换
xhr.responseType = 'json';

//xhr.response表示响应信息，注意ajax是异步的，
//可以为xhr对象绑定一个load事件
xhr.onload = function(){
    //xhr.status表示响应状态码
    if (xhr.status === 200) { //如果响应成功就读取
        const result = xhr.response;
        if (result.status === "ok") {//这里的status是api中设置的status，不是响应状态码
            //此处对拿到的数据进行渲染
        }
    }
}
```

以上，xhr比较老了，它是异步的，需要用到回调，回调多了以后就比较乱，总之现在用得不多了，了解一下思路。

## Fetch 

所以我们希望有一个基于Promise的发送请求的方式，这就是Fetch。

fetch是xhr的升级版，采用的是Promise API，作用和xhr是一样的，但是使用起来更加友好。fetch和xhr都是原生JS就支持的一种AJAX请求的方式。

fetch是一个函数，使用方法：

```javascript
const btn = document.getElementById("btn");

btn.onclick = () => {
    fetch("http://xxx/xx", {
        method: "post", //也可以填别的请求方法
        headers: {
            "Content-type": "application/json" //通过body去发送数据时，必须通过请求头来指定数据的类型
        },
        body: JSON.stringify({
            name: "xxx",
            age: 2,
        }),//这里面是请求体
    })
}
}
```

fetch是Promise API，那么只要是promise API，那么读取结果就用then.

```javascript
fetch("http://xxx/xx")
	.then((res) => {
    	if (res.status === 200) {
            //res.json()可以用来读取json格式的数据
            return res.json()
        } else {
            throw new Error("加载失败！")
        }
})
	.then(res => {
    //获取到数据后，将数据渲染到页面中，渲染的过程略去，麻烦
    	if (res.status === "ok")
})
	.catch((err) => {
        console.log("出错了", err)
    })
```



一个用fetch写的登录功能：

我的client如下：

```html
<div id="root">
	<h1>请登录以后再做操作</h1>
    <h2 id="info"></h2>
    <form> //因为要用fetch，所以form标签中的action之类的属性也都不用写了
    	<div>
            <input id="username" type="text" />
        </div>
        <div>
            <input id="password" type="password" />
        </div>
        <div>
            <button id="login-btn" type="button">登录</button> //否则此处的默认是type="submit",点击按钮后表达会自动提交/跳转，虽然这里已经没有写action了，但这是它的默认行为
         </div>
	</form>
</div>

<script>
    //点击login-btn后实现登录功能
    const loginBtn = document.getElementById("login-btn");
    const root = document.getElementById("root")
    
    loginBtn.onclick = () => {
        //获取用户输入的用户名和密码
        cosnt username = document.getElementById("username").value.trim();
        cosnt password = document.getElementById("password").value.trim();
        
        //调用fetch来发送请求、完成登录
        fetch("某个服务器路径", {
            method:"POST",
            headers:{
                "Content-type": "application/json",
            }
            body:JSON.stringify({username, password})
        }).then(res => {
            res.json()
        }).then(res => {
             if(res.status !== "ok"){
                            throw new Error("用户名或密码错误")
                        }
             root.innerHTML = `
                            <h1>欢迎 ${res.data.nickname} 回来！</h1>
                            <hr>
                            <button id="load-btn">加载数据</button>
                        `
                    })
        }).catch(err => {
            console.log("出错了", err) //登录失败
            document.getElementById("info").innerText = "用户名或密码错误";
        })
    }
</script>
```

我的server如下：

```javascript
const express = require("express")

const app = express()
app.use(express.urlencoded({ extended: true }))
// 解析json格式请求体的中间件
app.use(express.json())

app.use((req, res, next) => {
    res.setHeader("Access-Control-Allow-Origin", "*")
    res.setHeader("Access-Control-Allow-Methods", "GET,POST,PUT,DELETE,PATCH")
    res.setHeader("Access-Control-Allow-Headers", "Content-type")

    next()
})

// 定义一个登录的路由
app.post("/login", (req, res) => {
    // 获取用户输入的用户名和密码
    const { username, password } = req.body
    // 验证用户名和密码
    if (username === "admin" && password === "123123") {
        // 登录成功
        res.send({
            status: "ok",
            data: { id: "12345", username: "admin", nickname: "超级管理员" }
        })
    } else {
        // 登录失败
        res.status(403).send({
            status: "error",
            data: "用户名或密码错误"
        })
    }
})

app.listen(3000, () => {
    console.log("服务器已经启动！")
})
```



有时候也会想要终止请求：

```javascript
// 获取按钮
const btn01 = document.getElementById("btn01") //用户点击这个button就会向服务器发请求
const btn02 = document.getElementById("btn02") //用户点击这个button就会取消刚刚发送的请求

let controller

btn01.onclick = () => {
    // 创建一个AbortController，用来终止请求
    controller = new AbortController()
    // setTimeout(()=>{
    //     controller.abort()
    // }, 3000) //超过3s后没有响应就终止请求，嘿

    // 终止请求
    // 点击按钮向test发送请求
    fetch("http://localhost:3000/test", {
        signal: controller.signal
    })
        .then((res) => console.log(res))
        .catch((err) => console.log("出错了", err))
}

btn02.onclick = () => {
    controller && controller.abort()
}

```

当然也可以把fetch写成Promise语法糖的样子：

```javascript
btn.onclick = async () => {
    // fetch("http://localhost:3000/test").then()...
    // 注意：将promise改写为await时，一定要写try-catch，否则就相当于Promise没写catch，那出了错就处理不了了

    try {
        const res = await fetch("http://localhost:3000/students")
        const data = await res.json()
        console.log(data)
    } catch (e) {
        console.log("出错了", e)
    }
}
```



## 本地存储

接上面登录的例子，我们现在不能保持住用户登录的状态，刷新一下页面，用户还是要重新登录。

> [!NOTE]
>
> cookie由服务器创建，由浏览器保存。但是cookie不能跨域。
>
> session是基于cookie的。

为了能够在用户登录成功以后，保持用户的登录状态，我们需要将用户的信息存储到本地（为什么需要本地存储，因为现在cookie和session都不好使了）。本地存储有两个：sessionStorage和localStorage。所谓本地存储就是指浏览器自身的存储空间，可以将用户的数据存储到浏览器内部。

sessionStorage和localStorage的区别在于，数据存储的有效期：sessionStorage中存储的数据，页面一关闭就会丢失；localStorage存储的时间比较长，简单来说不删就不会丢失。

sessionStorage和localStorage实际上是两个全局的变量，它们的方法都一样。因为前者的数据有效期太短了，所以一般都是用本地存储localStorage：

```javascript
btn.onclick = () => {
    //setItem()存储数据
    //getItem()获取数据
    //removeItem()删除数据
    //clear()清空数据
    localStorage.setItem("name", "齐天大圣");
    const name = localStorage.getItem("name");
}
```

那么，接着上面登录的例子，如果我们要能够保持用户登录的状态，就可以先根据localStorage中的数据判断用户是否登录过，如果存在该用户的信息，就直接给它进行该有的那些操作。不过现在这种写法存在两个问题，由于登录以后直接将用户信息存储到了localStorage，导致存在：

- 数据安全问题，明文了
- 服务器没有对用户权限进行验证，就直接给客户端返回数据了



## token

解决上面两个问题。

验证分为前端验证和后端验证，前端验证很容易被绕过。开发时不可以只有前端验证，甚至前端验证可以不要，后端验证是必须的，否则数据非常危险。

> **前端验证**发生在客户端（通常是用户的浏览器）上。它的主要作用包括：
>
> - **提高用户体验**：即时反馈用户输入错误，例如表单字段未填、格式错误等，避免用户提交不必要的请求。
> - **减少服务器负载**：通过在客户端过滤掉明显的错误输入，减少服务器需要处理的无效请求数量。
>
> **后端验证**发生在服务器端。它的主要作用包括：
>
> - **确保数据安全和完整性**：即使前端验证通过，也可能被绕过或篡改，后端验证作为最终防线，确保数据的有效性。
> - **处理复杂的业务逻辑**：后端可以访问数据库和其他资源，进行更复杂和全面的验证，例如查重、权限验证等。



那么在服务器端如何对用户的登录状态进行验证？或者说如何告诉服务器客户端的登录状态？

由于rest风格的服务器是无状态的服务器，所以不能再服务器中存储用户的数据，可以把用户信息存在数据库或者客户端。

既然服务器中不能存储用户信息，那么可以将用户信息发送给客户端保存。客户端每次访问服务器时，也同时将用户信息发送给服务器，服务器就可以根据这一信息来识别用户的身份。因为如果将数据直接发送给客户端同样会有数据安全的问题，所以**服务器端**对数据进行加密，加密以后再发送给客户端保存（客户端存在自己的本地存储里）。

在node中可以直接使用`jsonwebtoken`也就是JWT这个包来对数据进行加密，它是在对JSON加密后，生成一个web中使用的令牌。JWT是在服务器中使用的。JWT 由三部分组成：头部（Header）、载荷（Payload）和签名（Signature），以点（`.`）分隔。

```javascript
//主要过程就是加密然后解密
// 引入jwt
const jwt = require("jsonwebtoken")
 
// 创建一个对象
const obj = {
    name: "猴子",
    age: 188888,
    gender: "男"
}
 
// 使用jwt来对json数据进行加密
const token = jwt.sign(obj, "chaojianquanmima", {
    expiresIn: "1h"
})

try {
    //服务器收到客户端的token后
    const decodeData = jwt.verify(token, "chaojianquanmima")
    console.log(decodeData)
} catch (e) {
    // 说明token解码失败，token无效，可能是过期了，也可能就是瞎传的
    console.log("无效的token")
```

现在来应用一下token，首先，在客户端，只要我们访问的是需要权限的api时，必须在请求中附加有关权限的信息。token一般都是通过请求头来发送：

```javascript
//client/login.html

const token = localStorage.getItem("token")
fetch("http://xxxx/xxx", {
    headers: {
        "authorization":`Bearer ${token}`,//记得在服务器中把这个header加进允许跨域的头中
    }
})
```

然后服务器也要接收客户端发来的token：

```javascript
app.get("/xxxx", (req, res) => {
    
    try{
        //这个路由要检查权限，必须在用户登录以后才能访问
    	//为了检查用户是否登录，读取请求头
    	const token = req.get("Authorization").splic(" ")[1];//拿到token本身，去掉前面的Bearer
            //对token进行解码
        const decodeToken = jwt.verify(token, "某个密钥");
        //就开始做这个路由本身要提供的功能
    }catch(e){
        //解码错误，用户token无效
        res.status(403).send({
            status:"error",
            data:"token无效，哈哈"
        })
    }
})

//这样验证权限的代码，在真实开发中应该统一写到一个中间件里面。
```



## Axios

xhr是原始的基于事件回调的风格，麻烦；fetch基于Promise API，比较灵活。此外，现在还有Axios。

Axios也支持Promise API，在服务端使用原生node.js的`http`模块，在客户端使用XMLHttpRequests。我们在客户端使用Axios的话，它就相当于是对xhr的再封装。xhr是基于事件的，Axios对它封装之后就是基于Promise的了，使用起来更方便， 兼容性上也没有什么问题。

Axios和Fetch比较类似，不过Axios的话功能会多一些，有些地方也更方便一点，比如能够自动识别数据类型并转换成JSON对象。

不同于Fetch，Axios默认只会在响应状态码为2xx时才会调用then，原理和我们在用Fetch时手动在then中抛错是一样的，只不过此处Axios帮我们封装了。

- Axiox请求的配置对象：

```javascript
axios({
    //baseURL 指定服务器的根目录（路径的前缀）
    baseURL:"http://localhost:3000",
    //请求地址
    url:"students",
    
    //请求方法默认是get
    method:"post",
    //指定请求头：
    headers:{"Content-Type": "application/json"}
    
    timeout: 3000,
    
})
```

- 响应数据的结构：看文档
- Axios实例：Axios实例相当于是Axios的一个副本，它的功能和Axios一样，Axios的默认配置在实例也同样会生效。但是可以单独修改Axios实例的默认配置。
- Axios拦截器：Axios的拦截器可以对请求或响应进行拦截，在请求发送前和响应读取前处理数据。请求拦截器一般都是用来统一地设置请求头。另外，拦截器只对当前的实例有效，和实例是绑定的。拦截器也可以取消。







