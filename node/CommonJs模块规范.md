## CommonJs模块规范

在 Node 中的 JavaScript 还有一个很重要的概念： 模块概念。

- 模块作用域
- 使用 require 方法用来加载模块
- 使用 exports 接口对象用来到处模块中的成员

#### 加载 `require`

语法：

```javascript
let 自定义变量名称 = require('模块')
```

两个作用：

- 执行被加载模块中的代码
- 得到被加载模块中的`exprots`导出接口对象

#### 导出 `exports`

- Node 中是模块作用域，默认文件中所有的成员只在当前文件模块有效
- 对于希望可以被其他模块访问的成员，就需要把这些公开的成员都挂载到`exports` 接口对象中就可以了

导出多个成员（必须在对象中）：

```javascript
exports.a = 123
exports.b = 'hello'
exports.c = function () {
    console.log('ccc')
}
exports.d = {
    foo: 'bar'
}
```



导出单个成员（拿到的就是：函数、字符串）：

```javascript
module.exports = 'hello'
```

以下情况会覆盖：

```javascript
module.exports = 'hello'

// 以这个为准
module.exports = function (x, y) {
    return x + y
}
```

也可以这样来导出多个成员：

```javascript
module.exports = {
    add:function () {
        return x + y
    },
    str: 'hello'
}
```

#### 原理解析

exports 和 `module.exports` 的一个引用

```javascript
// let module = {
//   exports = {}
// }
// let exports = module.exports

console.log(exports === module.exports) // => true

exports.foo = 'bar'

// 等价于
module.exports.foo = 'bar'

// return module.exports
```

#### exports 和 module.exports 的区别

- exports 和 module.exports 的区别
  + 每个模块中都有一个 module 对象
  + module 对象中有一个 exports 对象
  + 我们可以把需要导出的成员都挂载到 module.exports 接口对象中
  + 也就是： `module.exports.xxx = xxx` 的方式
  + 但是每次都 `module.exports.xxx = xxx` 很麻烦，点儿的太多了
  + 所以 Node 为了方便，同时在每个模块中都提供一个成员叫： `exports`
  + `exports === module.exports` 结果为 `true`
  + 所以对于： `module.exports.xxx = xxx` 的方式完全可以：`exports.xxx = xxx`
  + 当一个模块需要导出单个成员的时候，这个时候必须使用： `module.exports = xxx` 的方式
  + 使用 `exports = xxx` 不管用
  + 因为每个模块最终向外 `return` 的是 `module.exports`
  + 而 `exports` 只是 `module.exports` 的一个引用
  + 所以即便为 `exports = xxx` 重新赋值，也不会影响 `module.exports`
  + 但是有一种赋值方式比较特殊： `exports = module.exports` 这个用来重新建立引用关系的 

#### require 方法加载规则

[深入浅出Node.js(三)，深入Node.js的模块机制](https://www.infoq.cn/article/nodejs-module-mechanism)

《深入浅出Node.js》

- 核心模块
  + 模块名

- 第三方模块
  + 模块名

- 用户自己写的
  + 路径

- 优先从缓存加载
- 判断模块标识
  + 核心模块
  + 第三方模块
  + 自己写的模块

### npm

- node package manager

#### npm 网站

​	[npmjs.com](https://www.npmjs.com/)

#### npm 命令行工具

​	npm 的第二层含义就是一个命令行工具，只要你安装了 node 就已经安装了 npm。

npm 也有版本的概念。

可以通过命令行输入：

```javascript
npm --version
```

升级npm：

```javascript
npm install --global npm
```

#### 常用命令

- npm init
  + npm init -y 可以跳过向导，快速生成

- npm install
- npm install 包名
  + 只下载
  + npm i 包名
- npm install --save 包名
  + 下载并保存依赖项（package.json文件中的 dependencies 选项）
  + npm i --S

- npm uninstall 包名
  + 只删除，如果有依赖项会依然保存

- npm uninstall --save 包名
  + 删除的同时也会把依赖信息也删除
  + npm un -S 包名

- npm help
  + 产看使用帮助
- npm 命令 --help
  + 产看指定命令的使用帮助
  + 例如忘记uninstall命令的简写了，这个时候，可以输入 `npm uninstall --help` 来查看使用帮助

#### 解决npm被墙问题

[淘宝镜像](http://npm.taobao.org)淘宝的开发团队把npm在国内做了一个备份。

安装淘宝的cnpm：

```shell
# 在任意目录制定都可以
# --global表示安装到全局，而非当前目录
# --global 不能省略，否则不管用
npm install --global cnpm
```

之后使用将`npm`替换成`cnpm`。

```shell
# 这里还是走国外 npm 服务器，速度比较慢
npm install jquery

# 使用 cnpm 就会通过淘宝的服务器来下载 jquery
cnpm insatall jquery
```

如果不想安装 `cnpm` 又想使用淘宝的服务器来下载：

```shell
npm install jquery --registry=https://registry.npm.taobao.org
```

但每次手动加参数很麻烦，所以可以把这个选项加在配置文件中：

```shell
npm config set registry https://registry.npm.taobao.org

# 查看 npm 配置信息
npm config list
```

只要经过上面命令的配置，则以后所有的 `npm install` 都会默认通过淘宝的服务器来下载

### package.json

建议每一个项目都要有一个 `package.json` 文件（包描述文件）。

这个文件可以通过 `npm init` 的方式来自动初始化出来

```javascript
D:\demo\node\npm-demo>npm init
This utility will walk you through creating a package.json file.
It only covers the most common items, and tries to guess sensible defaults.

See `npm help init` for definitive documentation on these fields
and exactly what they do.

Use `npm install <pkg>` afterwards to install a package and
save it as a dependency in the package.json file.

Press ^C at any time to quit.
package name: (npm-demo)
version: (1.0.0) 0.0.1
description: 这是一个测试项目
entry point: (index.js) main.js
test command:
git repository:
keywords:
author: liuxudong
license: (ISC)
About to write to D:\demo\node\npm-demo\package.json:

{
  "name": "npm-demo",
  "version": "0.0.1",
  "description": "这是一个测试项目",
  "main": "main.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "liuxudong",
  "license": "ISC"
}


Is this OK? (yes) yes
```

`dependencies`  选项，保存第三方包的依赖信息

如果 `node_modules` 删除了也不用担心，只需要执行： `npm install`  就会自动把 `package.json` 中所有的依赖项都下载回来

-  建议每个项目的根目录下都有一个 `package.json`  文件

- 建议执行 `npm install 包名` 的时候加上 `--save`  这个选项，目的是用来保存依赖项信息 

##### package.json 和 package-lock.json

npm 5 以前是不会有 `package-lock.json` 文件的。

npm 5 以后才加入的。

当安装包的时候，npm 都会生成或者更新 `package-lock.json` 这个文件。

- npm 5 以后的版本安装包不需要加 `--save` 参数，它会自动保存依赖信息
- 当安装包的时候，会自动创建或者更新 `package-lock.json` 这个文件
- `package-lock.json` 这个文件会保存 `node_modules` 中所有包的信息（版本、下载地址）
  + 这样的话重新 `npm install` 的时候速度就可以提升

- 从文件来看，有一个 `lock` 称之为锁
  + 这个 `lock` 是用来锁定版本的、
  + 如果项目依赖了 `1.1.1` 版本
  + 如果你重新 install 其实会下载最新版本，而不是 `1.1.1`
  + 我们的目的就是希望可以锁住 `1.1.1` 版本
  + 所以 `package-lock.json` 这个文件的另一个作用就是锁定版本号，防止自动升级新版

