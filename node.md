# NodeJs

## 文件操作路径和模块操作路径

文件操作路径：

```javascript
// 在文件操作的相对路径中
//  ./data/a.txt 相对与当前目录
//  data/a.txt 相对与当前目录
//  /data/a.txt 绝对路径，当前文件模块所处磁盘根目录
//  c:/xx/xx... 绝对路径
// fs.readFile('./data/a.txt',function(err,data){
//	 if(err){
//        console.log(err)
//        return console.log('读取失败')
//    }
//    console.log(data.toString())
// })
// 
```

模块操作路径：

```javascript
// 这里如果忽略了  . 则也是磁盘根目录
require('/data/foo.js')

// 相对路径
require('./data.foo.js')

// 模块加载的路径中的相对路径不能省略
```

## 异步编程

### 回调函数

不成立情况：

```javascript
function add(x,y){
    console.log(1)
    setTimeout(function(){
        console.log(2)
        let ret = x + y
        return ret
    },1000)
    console.log(3)
    // 到这里执行就结束了，不会等到前面的定时器，所以直接就返回默认值undefined
}

console.log(add(10,20)) // => undefined
```

不成立情况：

```javascript
function add(x,y){
    let ret
    console.log(1)
    setTimeout(function(){
        console.log(2)
        ret = x + y
        return ret
    },1000)
    console.log(3)
    return ret
}

console.log(add(10,20)) // => undefined
```

回调函数：

```javascript
function add(x,y,callback){
    // callback 就是回调函数
    // let x = 10
    // let y = 20
    // let callback = function (ret) { console.log(ret) }
    console.log(1)
    setTimeout(function(){
        let ret = x + y
        callback(ret)
    },1000)
}

add(10,20,function(ret){
    console.log(ret)
})
```

基于原生 XMLHTTPRequest 封装 get 方法：

```javascript
function get(url,callback){
    let oReq = new XMLHttpRequest()
    // 当请求加载成功之后要调用指定的函数
    oReq.onload = function(){
        // 需要得到这里的 oReq.responseText
        callback(oReq.responseText)
    }
    oReq.open('get',url,true)
    oReq.send()
}

get('data.json',function(data){
    console.log(data)
})
```

### Promise

参考文档： [https://es6.ruanyifeng.com/#docs/promise](https://es6.ruanyifeng.com/#docs/promise)

callback hell:

![下载](C:\Users\z1760\Desktop\下载.jpg)

为了解决回调地狱，EcmaScript6 中新增了一个 API ： `Promise`

```javascript
const fs = require('fs')

let p1 = new Promise((resolve,reject) => {
	fs.readFile('./data/a.txt','utf8',(err,data) => {
		if(err) {
			reject(err)
		} else {
			resolve(data)
		}
	})
})

let p2 = new Promise((resolve,reject) => {
	fs.readFile('./data/b.txt','utf8',(err,data) => {
		if(err) {
			reject(err)
		} else {
			resolve(data)
		}
	})
})

let p3 = new Promise((resolve,reject) => {
	fs.readFile('./data/c.txt','utf8',(err,data) => {
		if(err) {
			reject(err)
		} else {
			resolve(data)
		}
	})
})

p1
	.then(data => {
		console.log(data)
		return p2
	},err => {
		console.log('读取文件失败',err)
	})
	.then(data => {
		console.log(data)
		return p3
	})
	.then(data => {
		console.log(data)
	})
```

封装 Promise 版本的 `readFile`:

```javascript
const fs = require('fs')

let pReadFile = filePath => {
	return new Promise((resolve,reject) => {
	fs.readFile(filePath,'utf8',(err,data) => {
		if(err) {
			reject(err)
		} else {
			resolve(data)
		}
	})
})
}

pReadFile('./data/a.txt')
	.then(data => {
		console.log(data)
		return pReadFile('./data/b.txt')
	})
	.then(data => {
		console.log(data)
		return pReadFile('./data/c.txt')
	})
	.then(data => {
		console.log(data)
	})
```

## 增删改查案例

### 设计操作数据的 API 文件模块

```javascript
/*
 * student.js
 * 数据操作文件模块
 * 职责： 操作文件中的数据，只处理数据，不关心业务
*/

/*
 * 获取所有学生列表
 * return []
*/
exports.find = () => {

}

/*
 * 添加保存学生
*/
exports.save = () => {
	
}

/*
 * 更新学生
*/
exports.updata = () => {
	
}

/*
 * 删除学生
*/
exports.delete = () => {
	
}
```

### 编写步骤

- 处理模板
- 配置开放静态资源
- 配置模板引擎
- 简单路由：/students渲染静态页面出来
- 路由设计
- 提取路由模块
- 由于业务操作需要处理文件数据，所以我们需要封装student.js
- 先写好 student.js 文件结构
  + 查询所有学生列表的 API find
  + findByid
  + save
  + updateById
  + deleteById

- 实现具体功能

  + 通过路由收到请求
  + 接收请求中的数据(get、post)
    * req.query
    * req.body

  + 调用数据操作 API 处理数据
  + 根据操作结果给客户端发送响应

- 业务功能顺序
  + 列表
  + 添加
  + 编辑
  + 删除

## CommonJs模块规范

在 Node 中的 JavaScript 还有一个很重要的概念： 模块概念。

- 模块作用域
- 使用 require 方法用来加载模块
- 使用 exports 接口对象用来到处模块中的成员

### 加载 `require`

语法：

```javascript
let 自定义变量名称 = require('模块')
```

两个作用：

- 执行被加载模块中的代码
- 得到被加载模块中的`exprots`导出接口对象

### 导出 `exports`

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

### 原理解析

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

### exports 和 module.exports 的区别

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

### require 方法加载规则

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

## npm

- node package manager

### npm 网站

​	[npmjs.com](https://www.npmjs.com/)

### npm 命令行工具

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

### 常用命令

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

### 解决npm被墙问题

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

-  建议执行 `npm install 包名` 的时候加上 `--save`  这个选项，目的是用来保存依赖项信息 

#### package.json 和 package-lock.json

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

## Express

### 起步

#### 安装：

``` shell
npm install express --save
```

#### hello world:

```javascript
const express = require('express')
const app = express()

app.get('/',(req,res) => res.send('hello world!'))

app.listen(3000,() => console.log('Example app listening on port 3000!'))
```

#### 基本路由

##### get

```javascript
// 当以 GET 方法请求 / 的时候，执行对应的处理函数
app.get('/',function(req,res){
    res.send('hello world!')
})
```

##### post

```javascript
// 当以 POST 方法请求 / 的时候，执行对应的处理函数
app.post('/',function(req,res){
    res.send('Got a POST request')
})
```

#### 静态服务

```javascript
// /public资源
app.use(express.static('public'))

// /files资源
app.use(express.static('files'))

// /public/xxx
app.use('/public',express.status('public'))

// /static/xxx
app.use('/static',express.static('public'))

app.use('/static',express.static(path.join(__dirname,'public')))
```

#### 在Express中配置使用`art-template`模板引擎

- [art-template - GitHub仓库](https://github.com/aui/art-template)
- [art-template 官方文档](https://aui.github.io/art-template/zh-cn/index.html)

安装：

```shell
npm install --save art-template
npm install --save express-art-template
```

配置：

```javascript
app.engine('html',require('express-art-template'))
```

使用：

```javascript
app.get('/',(req,res) => {
    // express 默认会去项目中的 views 目录找 index.html
	res.render('index.html',{
		title: 'hello world'
	})
})
```

如果希望修改默认的 `views` 视图渲染存储目录，可以：

```javascript
// 注意: 第一个参数 views 不能写错，表示要修改的是 views 目录
app.set('views',目录路径)
```

#### 在 Express 获取表单 POST 请求提数据

在 Express 中没有内置获取表单 POST 请求体的 API，这里我们需要使用一个第三方包：`body-parser`

安装：

```shell
npm install --save body-parser
```

配置：

```javascript
var express = require('express')
// 0. 引包
var bodyParser = require('body-parser')

var app = express()

// 配置 body-parser
// 只要加入这个配置，则在 req 请求对象上会多出来一个属性：body
// 可以通过 req.body 来获取表单 POST 请求数据了
// parse application/x-www-form-urlencoded
app.use(bodyParser.urlencoded({ extended: false }))

// parse application/json
app.use(bodyParser.json())
```

使用：

```javascript
app.use(function (req, res) {
  res.setHeader('Content-Type', 'text/plain')
  res.write('you posted:\n')
  // 可以通过 req.body 来获取表单 POST 请求体数据
  res.end(JSON.stringify(req.body, null, 2))
})
```

#### 提取路由模块

router.js:

```javascript
/*
 * router.js 路由模块
 * 职责：
 * 	 处理路由
 * 	 根据不同的请求方法 + 请求路径设置具体的请求方法
*/


let fs = require('fs')

// Express 提供了一种更好的方式
// 专门用来包装路由

let express = require('express')

// 1.创建一个路由容器
let router = express.Router()

// 2.把路由都挂载到 router 路由容器中
router.get('/students',(req,res)=>{
	res.render('index.html')
})

router.get('/students/new',(req,res)=>{
	res.render('add.html')
})

router.post('/students/new',(req,res)=>{
	res.render('index.html')
})

router.get('/students/edit',(req,res)=>{
	res.render('index.html')
})

router.post('/students/edit',(req,res)=>{
	res.render('index.html')
})

router.get('/students/delete',(req,res)=>{
	res.render('index.html')
})


// 3.把 router 导出
module.exports = router
```

app.js:

```javascript
let router = require('./router')

// 挂载路由
app.use(router)
```

## MongoDB

### 关系型数据库和非关系型数据库

表就是关系

或者说表与表之间存在关系

- 所有的关系型数据库都需要通过 `sql` 语言来操作
- 所有的关系型数据库在操作之前都需要设计表结构
- 而且数据表还支持约束
  + 唯一的
  + 主键
  + 默认值
  + 非空

- 非关系型数据库非常的灵活
- 有的非关系型数据库就是 key-value 对儿
- 但是 MongoDB 是长的最像关系型数据库的非关系型数据库
  + 数据库 -> 数据库
  + 数据表 -> 集合（数组）
  + 表记录 -> (文档对象)

- MongoDB 不需要设计表结构
- 也就是说你可以任意的往里面存数据，没有结构性这么一说

### 启动和关闭数据库

启动：

```shell
# mongodb 默认使用执行 mongod 命令所处盘符根目录下的 /data/db 作为自己的数据存储目录
# 所以在第一次执行该命令之前先自己手动新建一个 /data/db
mongod
```

如果想要修改默认的数据存储目录，可以：

```shell
mongod --dbpath=数据存储目录路径
```

停止：

```shell
在开启服务的控制台，直接 Ctrl+c 即可停止。
或者直接关闭开启服务的控制台也可以
```

### 连接和退出数据库

连接：

```shell
# 该命令默认连接本机的 MongoDB 服务
mongo
```

退出：

```shell
# 在连接状态输入 exit 退出连接
exit
```

### 基本命令

- `show dbs`
  + 查看显示所有数据库

- `db`
  + 查看当前操作的数据库
- `use 数据库名称`
  + 切换到指定的数据(如果没有会新建)

- 插入数据

### 在 Node 中如何操作 MongoDB 数据

#### 使用官方的 ` mongodb` 包来操作

[https://github.com/mongodb/node-mongodb-native](https://github.com/mongodb/node-mongodb-native)

#### 使用第三方 mongoose 来操作 MongoDB 数据库

第三方包： `mongoose` 基于 MongoDB 官方的 `mongodb` 包再一次做了封装

### mongoose

- 官网： [https://mongoosejs.com/](https://mongoosejs.com/)

- 官方指南： [https://mongoosejs.com/docs/guide.html](https://mongoosejs.com/docs/guide.html)

- 官方 API 文档： [https://mongoosejs.com/docs/api.html](https://mongoosejs.com/docs/api.html)

#### MongoDB 数据库的基本概念

- 可以有多个数据库
- 一个数据库可以有多个集合（表）
- 一个集合可以有多个文档（表记录）
- 文档结构很灵活，没有任何限制

```javascript
{
    qq: {
        users: [
            {name: '张三', age: 15},
            {name: '李四', age: 15},
            {name: '王五', age: 15},
            {name: '张三122', age: 15},
            {name: '张三231', age: 18},
            ...
        ],
        products: [
            
        ],
        ...
    },
    taobao: {
        
    },
    baidu: {
        
    }
}
```

#### 起步

安装：

```shell
npm i mongoose
```

hello world:

```javascript
const mongoose = require('mongoose');
mongoose.connect('mongodb://localhost:27017/test', {useNewUrlParser: true, useUnifiedTopology: true});


// 创建一个模型
// 就是在设计数据库
// MongoDB 是动态的，非常灵活，只需要在代码中设计你的数据库就可以了
// mongoose 这个包就可以让你的设计编写过程变得非常的简单
const Cat = mongoose.model('Cat', { name: String });

for (var i = 0; i < 100; i++) {
	// 实例化一个 Cat
	const kitty = new Cat({ name: '喵喵' + i });

	// 持久化保存 kitty 实例
	kitty.save().then(() => console.log('meow'));
}
```

#### 官方指南

##### 设计Scheme发布Model

```javascript
const mongoose = require('mongoose')

const Schema = mongoose.Schema

// 1.连接数据库
// 指定连接的数据库不需要存在，当你插入第一条数据之后就会自动被创建出来
mongoose.connect('mongodb://localhost/itcast')


// 2.设计集合结构（表结构）
// 字段名称就是表结构中的属性名称
// 约束的目的是为了保证数据的完整性，不要有脏数据
const userSchema = new Schema({
    username: {
    	type: String,
    	required: true // 必须有
    }，
    password: {
    	type: String,
    	required: true
    },
    email: {
    	type: String
    }
});


// 3.将文档结构发布为模型
	// mongoose.model 方法就是用来将一个架构发布为 model
	// 第一个参数： 传入一个大写名词单数字符串用来表示你的数据库名称
					// mongoose 会自动将大写名词的字符串生成 小写复数 的集合名称
					// 例如这里的 User 最终会变为 users 集合名称
	// 第二个参数： 架构 Schema

	// 返回值： 模型构造函数
let User = mongoose.model('User',userSchema)

// 4.当有了模型构造函数之后，就可以使用这个构造函数对 users 集合中的数据 增删改查


```

##### 增加数据

```javascript
let admin = new User({
	username: 'admin',
	password: '123456',
	email: 'admin@admin.com'
})

admin.save((err,ret) => {
	if(err){
		console.log('保存失败')
	} else {
		console.log('保存成功')
		console.log(ret)
	}
})
```

##### 查询

查询所有：

```javascript
User.find((err,ret) => {
	if(err){
		console.log('查询失败')
	} else {
		console.log('查询成功')
		console.log(ret)
	}
})
```

按条件查询所有：

```javascript
User.find({
    username: 'zs'
},(err,ret) => {
	if(err){
		console.log('查询失败')
	} else {
		console.log('查询成功')
		console.log(ret)
	}
})
```

按条件查询单个：

```javascript
User.findOne({
    username: 'zs'
},(err,ret) => {
	if(err){
		console.log('查询失败')
	} else {
		console.log('查询成功')
		console.log(ret)
	}
})
```

##### 删除数据

根据条件删除所有：

```javascript
User.remove({
	username: 'admin'
},(err,ret) => {
	if(err){
		console.log('删除失败')
	} else {
		console.log('删除成功')
		console.log(ret)
	}
})
```

根据条件删除一个：

```javascript
Model.findOneAndRemove(conditions,[options],[callback])
```

根据 id 删除一个：

```javascript
Model.findByIdAndRemove(id,[options],[callback])
```

##### 更新数据

根据条件更新所有：

```javascript
Model.update(conditions,doc,[options],[callback])
```

根据指定条件更新一个：

```javascript
Model.findOneAndUpdate([conditions],[update],[options],[callback])
```

根据 id 更新一个：

```javascript
User.findByIdAndUpdate('5f63008e88107fa8dcd0f302',{
	password: '123'
},(err,ret) => {
	if(err){
		console.log('更新失败')
	} else {
		console.log('更新成功')
		console.log(ret)
	}
})
```

## 使用Node操作MySQL数据库

安装：

```shell
npm install --save mysql
```

## Node中的其它成员

在每个模块中，除了 `require` 、`exports` 等模块相关 API 之外，还有两个特殊成员：

- `__dirname` **动态获取** 可以用来获取当前文件模块所属目录的绝对路径
- `__filename` **动态获取** 可以用来获取当前文件的绝对路径
- `__dirname` 和 `__filename` 是不受执行 node 命令所属路径影响的

在文件操作中，使用相对路径是不可靠的，因为在 Node 中文件操作的路径被设计为相对于执行 node 命令所处的路径（不是 bug，是存在使用场景的设计）。

为解决这个问题，需要将相对路径改成绝对路径就可以了。

因此就需要使用 `__dirname` 和 `filename` 来解决这个问题。

在拼接路径的过程中，为了避免手动拼接带来的一些低级错误，所以推荐多使用： `path.join()` 来辅助拼接。

在文件操作中使用的相对路径都统一转换为 **动态绝对路径**

> 补充：模块中的路径表示和这里的路径没关系，不受影响（相对于文件模块）

## path路径操作模块

- path.basename
  + 获取一个路径的文件名（默认包含扩展名）

- path.dirname
  + 获取一个路径中的目录部分

- path.extname
  + 获取一个路径中的扩展名部分

- path.parse
  + 把一个路径转为对象
    * root 根路径
    * dir 目录
    * base 包含后缀名的文件名
    * ext 后缀名
    * name 不包含后缀名的文件名

- path.join
  + 当你需要进行路径拼接的时候，推荐使用这个方法

- path.isAbsolute
  + 判断一个路径是否是绝对路径

## 修改完代码自动重启

可以使用第三方命令行工具： `nodemon` 来解决频繁修改代码重启服务器问题

`nodemon` 是一个基于Node.js开发的一个第三方命令行工具，我们使用的时候需要独立安装：

```shell
npm install --global nodemon
```

安装完成后，使用：

```shell
node app.js

# 使用 nodemen
nodemon app.js
```

只要是通过 `nodemon app.js` 启动服务，则它会监视文件变化，当文件发生变化的时候，自动帮你重启服务器

