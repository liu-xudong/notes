## MongoDB

#### 关系型数据库和非关系型数据库

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

#### 启动和关闭数据库

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

#### 连接和退出数据库

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

#### 基本命令

- `show dbs`
  + 查看显示所有数据库

- `db`
  + 查看当前操作的数据库
- `use 数据库名称`
  + 切换到指定的数据(如果没有会新建)

- 插入数据

#### 在 Node 中如何操作 MongoDB 数据

##### 使用官方的 ` mongodb` 包来操作

[https://github.com/mongodb/node-mongodb-native](https://github.com/mongodb/node-mongodb-native)

##### 使用第三方 mongoose 来操作 MongoDB 数据库

第三方包： `mongoose` 基于 MongoDB 官方的 `mongodb` 包再一次做了封装

#### mongoose

- 官网： [https://mongoosejs.com/](https://mongoosejs.com/)

- 官方指南： [https://mongoosejs.com/docs/guide.html](https://mongoosejs.com/docs/guide.html)

- 官方 API 文档： [https://mongoosejs.com/docs/api.html](https://mongoosejs.com/docs/api.html)

##### MongoDB 数据库的基本概念

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



##### 起步

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

##### 官方指南

###### 设计Scheme发布Model

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

###### 增加数据

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

###### 查询

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

###### 删除数据

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

###### 更新数据

根据条件更新所有：

```javascript
Model.update(conditions,doc,[options],[callback])
```

根据指定条件更新一个：

```javascript
Model.findOneAndUpdate([conditions],[update],[options],[callback])
```

