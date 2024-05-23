# npm

用包是为了引入一些现成的代码帮我们更方便地开发。包太多就需要包管理器。node中的包管理器叫npm (node package manage)。npm有以下三部分的东西：

1. npm网站：可以查找包，也可以把自己开发的包提交
2. npm CLI：通过CLI来操作npm中的各种包
3. 仓库

## package.json

这是包的描述文件，每一个node项目必须有该文件，从而对项目进行描述。

package.json中对于包的描述必须有的属性是name和version：

- name：包的名称
- version：包的版本，需要遵从x.x.x的格式。更具体来说：
  - 版本从1.0.0开始
  - 修复错误，兼容旧版（补丁）：1.0.1，1.0.2
  - 添加功能，兼容旧版（小更新）：1.1.0
  - 更新功能，影响兼容（重大更新）：2.0.0



此外，在package.json的scripts属性中还可以可以自定义一些命令，定义以后可以直接通过npm来执行这些命令：

- `start`和`test`可以直接通过`npm start` ,`npm test`执行
- 其他命令需要通过`npm run xxx`执行

## 初始化 & 包的安装

```bash
//初始化项目
npm init //初始化项目，创建package.json文件，需要手动选择问题答案
npm init -y //初始化项目，创建package.json文件，所有问题采用默认值

//安装包
npn install 包名 //将指定的包下载到当前项目中
```

在执行`npm install`命令时发生了什么？

- 将包下载到当前项目的node_modules目录下
- 在package.json的dependencies属性中添加一个新属性`"包名": "^版本号（假设是3.2.12）"`：
  - 版本号前的`^`表示匹配最新的3.x.x版本
  - 不加任何前缀就只会匹配到当前版本
- 会自动添加package-lock.json文件，这个文件是用来帮助加速npm下载的，不用动它哦

所以如果想把自己的项目发给别人，比如某个代号为燕双鹰的神秘人物，那么并不需要把node_modules这个文件夹也发给他。只要在package.json里面把依赖设置好，燕双鹰在他的本地执行`npm i`就会自动把所有依赖的包都安装好的。

```bash
npm install //这会自动安装所有依赖

npm uninstall 包名 //卸载

npm install 包名 -g //这是全局安装
npm uninstall 包名 -g//卸载全局安装的包
```

全局安装是将包安装到计算机中而不是这个项目下，通常都是一些工具，比如安装之后会成为命令行的一条命令。

```bash
//也可以在安装时指定包的版本
npm install 包名@3.3.3
npm install 包名@”>3.3.3“

npm install 包名 --no-save //禁止该包出现在package.json的依赖中
```

## 引入npm的包

引入从npm下载的包时，不需要书写路径，直接写包名即可。

```javascript
const _ = require("lodash");
console.log(_);
```

# pnpm

关于pnpm和npm的区别：https://www.jianshu.com/p/6c695a0692e0。

# yarn

不喜欢yarn，虽然它的名字很幽默，Yet Another Resource Navigator.

还没用过yarn，以后要用再说。