## Express

#### 起步

##### 安装：

``` shell
npm install express --save
```

##### hello world:

```javascript
const express = require('express')
const app = express()

app.get('/',(req,res) => res.send('hello world!'))

app.listen(3000,() => console.log('Example app listening on port 3000!'))
```

##### 基本路由：

get：

```javascript
// 当以 GET 方法请求 / 的时候，执行对应的处理函数
app.get('/',function(req,res){
    res.send('hello world!')
})
```

post:

```javascript
// 当以 POST 方法请求 / 的时候，执行对应的处理函数
app.post('/',function(req,res){
    res.send('Got a POST request')
})
```

##### 静态服务

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

##### 在Express中配置使用`art-template`模板引擎

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

##### 在 Express 获取表单 POST 请求提数据

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

app.use(function (req, res) {
  res.setHeader('Content-Type', 'text/plain')
  res.write('you posted:\n')
  res.end(JSON.stringify(req.body, null, 2))
})
```

