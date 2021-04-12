## Node.js

### 模块，包

```js
// name.js 定义模块
const name = {
    surname: 'liu',
    sayname() {
        console.log(this.surname)
    }
}
const age = {
    age: 100
}
// 导出模块
module.exports = {
    name,
    age
}

// index.js	引入模块
const { name, age } = require('./name')
// 调用模块
name.sayname()
console.log(age.age)


```

### Express

```js
const express = require('express')

const app = express()

app.listen(8080,() => {
    console.log('localhost:8080')
})
```

### body-parser

```js
const express = require('express')
const bodyParser = require('body-parser')

const app = express()

// parse application/x-www-form-urlencoded 表单数据
app.use(bodyParser.urlencoded({ extended: false }))

// parse application/json	json数据
app.use(bodyParser.json())

app.listen(8080,() => {
    console.log('localhost:8080')
})
```

### router

```js
// server.js
const express = require('express')

const router = require('./router/index')

const app = express()

app.use('/', router)

app.listen(8080, () => {
    console.log('localhost:8080');
})
```

```js
// ./router/index.js
const express = require('express')

const router = express.Router()

router.get('/',(req,res) => {
    res.send('hello.')
})
```

### controller

```js
// ./router/index.js
const express = require('express')

const { list } = require('../controller/index')

const router = express.Router()

router.get('/home', list)

module.exports = router
```

```js
// ./controller/index.js
const list = (req, res) => {
    res.send('hello list.')
}

module.exports = {
    list
}
```

### public

```js
const express = require('express')

const app = express()

// 配置静态资源目录
app.use(express.static('public'))

app.listen(8080, () => {
    console.log('localhost:8080');
})
```

### art-template

```js
// server.js
const path = require('path')

const express = require('express')

const app = express()

// view engine setup	配置模板（art-template）
app.engine('art', require('express-art-template'));
app.set('view options', {   // 此处与官网不同
    debug: process.env.NODE_ENV !== 'production',
    escape: false   // 转化 HTML5 代码
});
app.set('views', path.join(__dirname, 'view'));
app.set('view engine', 'art');

app.listen(8080, () => {
    console.log('localhost:8080');
})
```

同时在根目录创建 `view` 视图目录，并创建对应的 `art` 文件

```art
{
    "ret": true,
    "data": {{data}}
}
```

```js
// ./controller/index.js
const list = (req, res) => {
	let dataArray = []

    for (let i = 0; i < 1000; i++) {
        dataArray.push('line' + i)
    }
	
    // 设置请求体类型
    res.set('content-type', 'application/json; charset=utf-8')
	
    // 当配置完成art-template后自动生成render方法
    res.render('list', {
        data: JSON.stringify(dataArray)
    })
}

module.exports = {
    list
}
```

### token

非对称加密： 首先在项目根目录创建一个文件夹 `keys` ，然后使用以下方式生成公钥私钥。

非对称加密密钥生成（openssl）：

```js
// 首先执行 需要在PowerShell(管理员)中运行
openssl
// 生成私钥
genrsa -out rsa_private_key.pem 2048
// 根据私钥生成公钥
rsa -in rsa_private_key.pem -pubout -out rsa_public_key.pem
```

```js
const jwt = require('jsonwebtoken')

const token = (req, res) => {
    // 对称解密
    const tk = jwt.sign({ username: 'liu' }, 'xudong')

    const decoued = jwt.verify(tk, 'xudong')
    res.send(decoued)
}

module.exports = {
    token
}
```

```js
const jwt = require('jsonwebtoken')

const fs = require('fs')

const path = require('path')

const token = (req, res) => {
    // 非对称加密
    const privateKey = fs.readFileSync(path.join(__dirname, '../keys/rsa_private_key.pem'))
    const publicKey = fs.readFileSync(path.join(__dirname, '../keys/rsa_public_key.pem'))
    const tk = jwt.sign({ username: 'liu' }, privateKey, { algorithm: 'RS256' })
    const decoued = jwt.verify(tk, publicKey)
    // res.set('content-type', "text/html; charset=UTF-8")
    res.send(decoued)
}

module.exports = {
    token
}
```

### webSocket

socket服务端

```js
// socketServer.js	后端页面
const WebSocket = require('ws');

const wss = new WebSocket.Server({ port: 8080 });

wss.on('connection', function connection(ws) {
  ws.on('message', function incoming(data) {
      wss.clients.forEach(function each(client) {
          // 公共聊天室
          client.send(data);
          // 一对一聊天
    //   if (client !== ws && client.readyState === WebSocket.OPEN) {
    //     client.send(data);
    //   }
    });
  });
});
```

socket客户端

```js
// webSocket.js	前端页面
const ws = new WebSocket('ws://localhost:8080/')	// 使用时浏览器必须支持HTML5

ws.onopen = () => {
    ws.send('hello server.')
}

ws.onmessage = (event) => {
    console.log(event);
    let data = event.data
    content.innerHTML += data + '<br/>'
}

ws.onerror = (err) => {
    ws.send(err)
}

ws.onclose = () => {
    console.log('连接已关闭');
}
```

界面展示

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="./webSocket.js"></script>
    <style>
        * {
            padding: 0;
            margin: 0;
        }

        #content {
            width: 80%;
            height: 30vh;
            margin: 10px auto;
            border: 1px solid #ff6666;
            border-radius: 5px;
        }

        .box {
            width: 80%;
            margin: 15px auto;
        }
    </style>
</head>
<body>
    <div id="content"></div>
    <div class="box">
        <input type="text" id="input">
        <button id="bt">send</button>
    </div>
    <script>
        let content = document.getElementById('content')
        let input = document.getElementById('input')
        let bt = document.getElementById('bt')

        bt.addEventListener('click',function() {
            ws.send(input.value)
            input.value = ''
        },false)
    </script>
</body>
</html>
```



