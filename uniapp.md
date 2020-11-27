# uni-app

## API

### 基础

#### 日志打印

- console
- debug
- log
- info
- warn
- error

#### 定时器

- setTimeout(callback,delay,rest)

  设定一个定时器。在定时到期以后执行注册的回调函数

- clearTimeout(timeoutID)

  取消由 setTimeout 设置的定时器

- setInterval(callback,delay,rest)

  设定一个定时器。按照指定的周期（以毫秒计）来执行注册的回调函数

- clearInterval(intervalID)

  取消由 setInterval 设置的定时器

#### uni.base64ToArrayBuffer(base64)

将 Base64 字符串转成 ArrayBuffer 对象

示例：

```javascript
const base64 = 'test'
const arrayBuffer = uni.base64ToArrayBuffer(base64)
```

#### uni.arrayBufferToBase64(arrBuffer)

将 ArrayBuffer 对象转成 Base64 字符串

```javascript
const arrayBuffer = new Uint8Array([55,55,55])
const base64 = uni.arrayBufferToBase64(arrayBuffer)
```

#### 生命周期

- 应用生命周期

  | 函数名            | 说明                                           |
  | ----------------- | ---------------------------------------------- |
  | onLaunch          | 当`uni-app` 初始化完成时出发（全局只触发一次） |
  | onShow            | 当`uni-app` 启动，或从后台进入前台显示         |
  | onHide            | 当`uni-app` 从前台进入后台                     |
  | onUniNViewMessage | 当`nvue` 页面发送的数据进行监听                |

  注： 应用生命周期仅可在 `App.vue` 中监听，在其它页面监听无效。

- 页面生命周期

  | 函数名                              | 说明                                                         | 平台差异说明                                         |
  | ----------------------------------- | ------------------------------------------------------------ | ---------------------------------------------------- |
  | onLoad                              | 监听页面加载，参数为上个页面传递的数据，参数类型为Object（用于页面传参） |                                                      |
  | onShow                              | 监听页面显示，页面每次出现在屏幕上都触发，包括从下级页面点返回露出当前页面 |                                                      |
  | onReady                             | 监听页面初次渲染完成。注意如果渲染速度快，会在页面进入动画完成前触发 |                                                      |
  | onHide                              | 监听页面隐藏                                                 |                                                      |
  | onUnload                            | 监听页面卸载                                                 |                                                      |
  | onResize                            | 监听窗口尺寸变化                                             | App、微信小程序                                      |
  | onPullDownRefresh                   | 监听用户下拉动作，一般用于下拉刷新                           |                                                      |
  | onReachBottom                       | 页面滚动到底部的事件（不是scroll-view滚到底），常用于上拉加载下一页数据。如使用scroll-view导致页面级没有滚动，则触底事件不会被触发 |                                                      |
  | onTabItemTap                        | 点击 tab 时触发，参数为 Object                               | 微信小程序、百度小程序、H5、App（自定义组件模式）    |
  | onShareAppMessage                   | 用户点击右上角分享                                           | 微信小程序、百度小程序、字节跳动小程序、支付宝小程序 |
  | onPageScroll                        | 监听页面滚动，参数为 Object                                  |                                                      |
  | onNavigationBarButtonTap            | 监听原声标题栏按钮点击事件，参数为 Object                    | App、H5                                              |
  | onBackPress                         | 监听页面返回，返回 event = {from:backbutton、navigateBack}，，backbutton 表示来源是左上角返回按钮或 android 返回键；navigateBack表示来源是uni.nabigateBack； | App、H5                                              |
  | onNavigationBarSearchInputChanged   | 监听原生标题栏搜索输入框输入内容变化事件                     | App、H5                                              |
  | onNavigationBarSearchInputConfirmed | 监听原生标题栏搜索输入框搜索事件，用户点击软键盘上的“搜索”按钮时触发 | App、H5                                              |
  | onNavigationBarSearchInputClicked   | 监听原生标题栏搜索输入框点击事件                             | App、H5                                              |

具体详见： [https://uniapp.dcloud.io/api/lifecycle](https://uniapp.dcloud.io/api/lifecycle)

#### 应用级事件

- uni.onPageNotFound(function callback)

  监听应用要打开的页面不存在事件

- uni.onError(function callback)

  监听小程序错误事件。如脚本错误或 `API` 调用报错等。

- uni.onAppShow(function callback)

  监听应用切前台事件。

- uni.onAppHide(function callback)

  监听应用切后台事件

- uni.offPageNotFound(function callback)

  取消监听应用要打开的页面不存在事件

- uni.offError(function callback)

  取消监听应用错误事件

- uni.offAppShow(function callback)

  取消监听小程序切前台事件

- uni.offAppHide(function callback)

  取消监听小程序切后台事件

具体详见： [https://uniapp.dcloud.io/api/application?id=onpagenotfound](https://uniapp.dcloud.io/api/application?id=onpagenotfound)

### 网络

#### 发送请求

uni.request(OBJECT)

发送网络请求

```javascript
uni.request({
    unl: 'https://www.example.com/request',
    data: {
        text: 'uni.request'
    },
    header: {
        'custom-header': 'hello'	// 自定义请求头信息
    },
    success: res => {
        console.log(res.data)
        this.text = 'request success'
    }
})
```

具体详情： [https://uniapp.dcloud.io/api/request/request?id=request](https://uniapp.dcloud.io/api/request/request?id=request)

#### 上传、下载

- uni.uploadFile

  将本地资源上传到开发者服务器，客户端发起一个 `POST` 请求，其中 `content-type` 为 `multipart/form-data` 。

  ```javascript
  uni.chooseImage({
      success: (chooseImageRes) => {
          const tempFilePaths = chooseImageRes.tempFilePaths;
          uni.uploadFile({
              url: 'https://www.example.com/upload',
              filePath: tempFilePaths[0],
              name: 'file',
              formData: {
                  'user': 'test'
              },
              success: (uploadFileRes) => {
                  console.log(uploadFileRes.data)
              }
          })
      }
  })
  ```

- uni.downloadFile

  下载文件资源到本地，客户端直接发起一个 HTTP GET  请求，返回文件的本地临时路径

  ```javascript
  uni.downloadFile({
      url: 'https://www.example.com/file/test',
      success: (res) => {
          if(res.statusCode === 200) {
              console.log('下载成功')
          }
      }
  })
  ```

具体详见： [https://uniapp.dcloud.io/api/request/network-file?id=uploadfile](https://uniapp.dcloud.io/api/request/network-file?id=uploadfile)

#### WebSocket

- uni.connectSocket(OBJECT)

  创建一个 **WebSocket** 连接

  ```javascript
  uni.connectSocket({
      url: 'wss://www.example.com/socket',
      data() {
          return {
              x: '',
              y: ''
          }
      },
      header: {
          'content-type': 'application/json'
      },
      protocols: ['protocol1'],
      method: 'GET'
  })
  ```

- uni.onSocketOpen(CALLBACK)

  监听 **WebSocket** 连接打开事件

  ```javascript
  uni.connectSocket({
      url: 'wws://ww.example.com/socket'
  })
  uni.onSocketOpen(function(res) {
      console.log('WebSocket连接已打开！')
  })
  ```

- uni.onSocketError(CALLBACK)

  监听 **WebSocke** 错误

  ```javascript
  uni.connectSocket({
    url: 'wss://www.example.com/socket'
  });
  uni.onSocketOpen(function (res) {
    console.log('WebSocket连接已打开！');
  });
  uni.onSocketError(function (res) {
    console.log('WebSocket连接打开失败，请检查！');
  });
  ```

- uni.sendSocketMessage(OBJECT)

  通过 **WebSocket** 连接发送数据，需要先 `uni.connectSocket` , 并在 `uni.onSocketOpen` 回调之后才能发送。

  ```javascript
  var socketOpen = false;
  var socketMsgQueue = [];
  
  uni.connectSocket({
      url: 'wss://www.example.com/socket'
  });
  
  uni.onSocketOpen(function(res) {
      socketOpen = true;
      for(let i = 0;i < socketMsgQueue.length;i++) {
          sendSocketMessage(socketMsgQueue[i])
      }
      socketMsgQueue = []
  })
  
  function sendSocketMessage(msg) {
      if(socketOpen) {
          uni.sendSocketMessage({
              data: msg
          })
      } else {
          socketMsgQueue.push(msg)
      }
  }
  ```

- uni.onSocketMessage(CALLBACK)

  监听 **WebSocket** 接受到服务器的消息事件。

  ```javascript
  uni.connectSocket({
      url: 'wws://www.example.com/socket'
  })
  
  uni.onSocketMessage(function(res) {
      console.log('收到服务器内容：' + res.data)
  })
  ```

- uni.closeSocket(OBJECT)

  关闭 **WebSocket** 连接

- uni.onSocketClose (CALLBACK)

  监听 **WebSocket** 关闭

  ```javascript
  uni.connectSocket({
      url: 'wws://www.example.com/socket'
  })
  
  // 注意这里有时序问题，
  // 如果 uni.connectSocket 还没回调 uni.onSocketOpen,而先调用 uni.closeSocket,那么就做不到关闭  WebSocket 的目的。
  // 必须在 WebSocket 打开期间调用 uni.closeSocket 才能关闭。
  uni.onSocketOpen(function() {
      uni.closeSocket()
  })
  
  uni.onSocketClose(function(res) {
      console.log('WebSocket 已关闭！')
  })
  ```

具体详见： [https://uniapp.dcloud.io/api/request/websocket?id=onsocketopen](https://uniapp.dcloud.io/api/request/websocket?id=onsocketopen)

#### SocketTask

- SocketTask.onMessage(CALLBACK)

  监听 **WebSocket** 接收到服务器的消息事件

- SocketTask.send(OBJECT)

  通过 **WebSocket** 连接发送数据

- SocketTask.close(OBJECT)

  关闭 **WebSocket** 连接

- SocketTask.onOpen(CALLBACK)

  监听 **WebSocket** 连接打开事件

- SocketTask.onClose(CALLBACK)

  监听 **WebSocket** 连接关闭事件

- SocketTask.onError(CALLBACK)

  监听 **WebSocket** 错误事件

具体详见： [https://uniapp.dcloud.io/api/request/socket-task?id=sockettaskonmessage](https://uniapp.dcloud.io/api/request/socket-task?id=sockettaskonmessage)

#### mDNS 服务

微信小程序： [详情](https://developers.weixin.qq.com/miniprogram/dev/api/network/mdns/wx.stopLocalServiceDiscovery.html)

#### UDP 通信

微信小程序： [详情](https://developers.weixin.qq.com/miniprogram/dev/api/network/udp/wx.createUDPSocket.html)

### 路由与页面跳转

具体详见： [https://uniapp.dcloud.io/api/router?id=navigateto](https://uniapp.dcloud.io/api/router?id=navigateto)

#### uni.navigateTo(OBJECT)

保留当前页面，跳转到应用内的某个页面，使用 `uni.navigateBack` 可以返回到原页面。

```javascript
// 在起始页面跳转到test.vue页面并传递参数
uni.navigateTo({
    url: 'test?id=1&name=uniapp'
})
```

```javascript
// 在 test.vue 页面接受参数
export default {
    onLoad: function(option) {	// option 为 object 类型，会序列化上个页面传递的参数
        console.log(option.id)
        console.log(option.name)
    }
}
```

url有长度限制，太长的字符串会传递失败，可使用 [窗体通信](https://uniapp.dcloud.io/collocation/frame/communication) 、[全局变量](https://ask.dcloud.net.cn/article/35021) 或 `encodeURIComponent` 等多种方式解决，如下是 `encodeURIComponent` 示例。

```vue
<navigator :url="'/pages/test/test?item='+encodeURIComponent(JSON.stringify(item))"></navigator>
```

```js
// 在 test.vue 页面接受参数
onLoad: function(option) {
	const item = JSON.parse(decodeURIComponent(option.item))
}
```

#### uni.redirectTo(OBJECT)

关闭当前页面，跳转到应用内的某个页面。

```javascript
uni.redirectTo({
    url: 'test?id=1'
})
```

注： 跳转到 tabBar 页面只能使用 `switchTab` 跳转

#### uni.reLaunch(OBJECT)

关闭所有页面，打开应用内的某个页面

```js
uni.reLaunch({
    url: 'test?id=1'
})
```

注： 如果调用了 `uni.preloadPage(OBJECT)`不会关闭，仅触发生命周期 `onHide`

#### uni.switchTab(OBJECT)

跳转到 **tabBar** 页面，并关闭其他所有非 tabBar 页面 

```js
uni.switchTab({
    url: '/pages/index/index'
})
```

#### uni.navigateBack(OBJECT)

关闭当前页面，返回上一页面或多级页面。可通过 `getCurrentPages()` 获取当前的页面栈，决定需要返回几层。

```js
// 注意： 调用 navigateTo 跳转时，调用该方法的页面会被加入堆栈，而 redirectTo 方法则不会。

// A 页面
uni.navigateTo({
    url: 'B?id=1'
})

// B 页面
uni.navigateTo({
    url: 'C?id=1'
})

// 在 C 页面内 navigateBack，将返回 A 页面
uni.nabigateBack({
    delta: 2
})
```



#### uni.preloadPage（OBJECT）

预加载页面，是一种性能优化技术。被预加载的页面，在打开时速度更快。（仅App-nvue、H5支持）

#### EventChannel

页面间事件通信通道

- EventChannel.emit(string eventName,any args)

  触发一个事件

  string eventName 事件名称

  any args 事件参数

- EventChannel.off(string eventName,function fn)

  取消监听一个事件。给出第二个参数时，只取消给出的监听函数，否则取消所有监听函数

  string eventName 事件名称

  function fn 事件监听函数

  参数 any args 触发事件参数

- EventChannel.on(string eventName,function fn)

  持续监听一个事件

  string eventName 事件名称

  function fn 事件监听函数

  参数 any args 触发事件参数

- EventChannel.once(string eventName,function fn)

  监听一个事件一次，触发后失效

  string eventName 事件名称

  function fn 事件监听函数

  参数 any args 触发事件参数

#### 窗口动画

窗口的显示/关闭动画效果，支持在 API、组件、pages.json 中配置，优先级为： `API = 组件 > pages.json`。(仅 APP 支持)

### 数据缓存

具体详见： [https://uniapp.dcloud.io/api/storage/storage?id=setstorage](https://uniapp.dcloud.io/api/storage/storage?id=setstorage)

#### uni.setStorage(OBJECT)

将数据存储在本地缓存中指定的 key 中，会覆盖掉原来该 key 对应的内容，这是一个异步接口。

```js
uni.setStorage({
    key: 'storage_key',
    data: 'hello',
    success: function() {
        console.log('success')
    }
})
```

#### uni.setStorageSync(KEY,DATA)

将 data 存储在本地缓存中指定的 key 中，会覆盖掉原来该 key 对应的内容，这是一个同步接口。

```js
try {
    uni.setStorageSync('storage_key','hello')
} catch(e) {
    // error
}
```

#### uni.getStorage(OBJECT)

从本地缓存中异步获取指定 key 对应的内容。

```js
uni.getStorage({
    key: 'storage_key',
    success: function(res) {
        console.log(res.data)
    }
})
```

#### uni.getStorageSync(KEY)

从本地缓存中同步获取指定 key 对应的内容。

```js
try {
    const value = uni.getStorageSync('storage_key')
    if(value) {
        console.log(value)
    }
} catch (e) {
    // error
}
```

#### uni.getStorageInfo(OBJECT)

异步获取当前 storage 的相关信息

```js
uni.getStorageInfo({
    success: function(res) {
        console.log(res.keys)	// 当前 storage 中所有的 key
        console.log(res.currentSize)	// 当前占用的空间大小，单位： kb
        console.log(res.limitSize)	// 限制的空间大小，单位： kb
    }
})
```

#### uni.getStorageInfoSync()

同步获取当前 storage 的相关信息。

```js
try {
    const res = uni.getStorageInfoSync()
    console.log(res.keys)	// 当前 storage 中所有的 key
    console.log(res.currentSize)	// 当前占用的空间大小，单位： kb
    console.log(res.limitSize)	// 限制的空间大小，单位： kb
} catch(e) {
    // error
}
```

#### uni.removeStorage(OBJECT)

从本地缓存中异步移除指定 key。

```js
uni.removeStorage({
    key: 'storage_key',
    success: function(res) {
        console.log('success')
    }
})
```

#### uni.removeStorageSync(KEY)

从本地缓存中同步移除指定 key。

```js
try {
    uni.removeStorageSync('storage_key')
} catch(e) {
    // error
}
```

#### uni.clearStorage()

清理本地数据缓存。

```js
uni.clearStorage()
```

#### uni.clearStorageSync()

同步清理本地数据缓存。

```js
try {
    uni.clearStorageSync()
} catch(e) {
    // error
}
```

### 位置

#### 获取位置

- uni.getLocation(OBJECT)

  获取当前的地理位置、速度。在微信小程序中，当用户离开应用后，此接口无法调用，除非申请后台持续定位权限；当用户点击“显示在聊天顶部”时，此接口可继续调用。

  ```js
  uni.getLocation({
      type: 'wgs84',
      success: function(res) {
          console.log('当前位置的经度' + res.longitude)
          console.log('当前位置的纬度' + res.latitude)
      }
  })
  ```

- uni.chooseLocation(OBJECT)

  打开地图选择位置。

  ```js
  uni.chooseLocation({
      success: function(res) {
          console.log('位置名称' + res.name)
          console.log('详细地址' + res.addiress)
          console.log('纬度' + res.latitude)
          console.log('经度' + res.longitude)
      }
  })
  ```

具体详见： [https://uniapp.dcloud.io/api/location/location?id=getlocation](https://uniapp.dcloud.io/api/location/location?id=getlocation)

#### 查看位置

uni.openLocation(OBJECT)

使用应用内置地图查看位置。

```js
uni.getLocation({
    type: 'gcj02',	// 返回可以用于 uni.openLocation 的经纬度
    success: function(res) {
        const latitude = res.latitude
        const longitude = res.longitude
        uni.openLocation({
            latitude:latitude,
            longitude: longitude,
            success: function() {
                console.log('success')
            }
        })
    }
})
```

具体详见： [https://uniapp.dcloud.io/api/location/open-location?id=openlocation](https://uniapp.dcloud.io/api/location/open-location?id=openlocation)

#### 地图组件控制

- uni.createMapContext(mapId,this)

  创建并返回 map 上下文 `mapContext` 对象。在自定义组件下，第二个参数传入组件实例 this ，以操作组件内 `<map>` 组件。

- mapSearch 模块(module)

  + reverseGeocode(Object,callback)

    >  反向地里边吗

  + poiSearchNearBy(Object,callback)

    > 周边检索

具体详见： [https://uniapp.dcloud.io/api/location/map?id=createmapcontext](https://uniapp.dcloud.io/api/location/map?id=createmapcontext)

### 媒体

#### 图片

- uni.chooseImage(OBJECT)

  从本地相册选择图片或使用相机拍照。

  ```js
  uni.chooseImage({
      count: 6,	// 默认 9
      sizeType: ['original','compressed'],	// 可以指定是原图还是压缩图，默认二者都有
      sourceType: ['album'],	// 从相册选择
      success: function(res) {
          console.log(JSON.stringify(res.tempFilePaths))
      }
  })
  ```

- uni.previewImage(OBJECT)

  预览图片。

  ```js
  // 从相册选择6张图
  uni.chooseImage({
      count: 6,
      sizeType: ['original','compressed'],
      sourceType: ['album'],
      success: function(res) {
          // 预览图片
          uni.previewImage({
              urls: res.tempFilePaths,
              longPressActions: {
                  itemList: ['发送给朋友','保存图片','收藏']，
                  success: function(data) {
              		console.log('选中了第' + (data.tapIndex + 1) + '个按钮，第' + 							(data.index + 1) + '张图片')
          		},
                  fail: function(err) {
                      console.log(err)
                  }
              }
          })
      }
  })
  ```

- uni.getImageInfo(OBJECT)

  获取图片信息。

  ```js
  uni.chooseImage({
      count: 1,
      courceType: ['album'],
      success: function(res) {
          uni.getImageInfo({
              src: res.tempFilePaths[0],
              success: function(image) {
                  console.log(image.width)
                  console.log(image.height)
              }
          })
      }
  })
  ```

- uni.saveImageToPhotosAlbum(OBJECT)

  保存图片到系统相册

  ```js
  uni.chooseImage({
      count: 1,
      sourceType: ['camera'],
      success: function(res) {
          uni.saveImageToPhotosAlbum({
              filePath: res.tempFilePaths[0],
              success: function() {
                  console.log('save success')
              }
          })
      }
  })
  ```

- uni.compressImage(OBJECT)

  压缩图片接口，可选压缩质量

  ```js
  uni.compressImage({
      src: '/static/logo.jpg',
      quality: 80,
      success: res => {
          console.log(res.tempFilePath)
      }
  })
  ```

具体详见： [https://uniapp.dcloud.io/api/media/image?id=chooseimage](https://uniapp.dcloud.io/api/media/image?id=chooseimage)

#### 文件

- uni.chooseFile(OBJECT)

  从本地选择文件

  ```js
  uni.chooseFile({
      count: 6,	//默认 100
      extension: ['.zip','.doc'],
      success: function(res) {
          console.log(JSON.stringify(res.tempFilePaths))
      }
  })
  
  // 选择图片文件
  uni.chooseFile({
      count: 10,
      type: 'image',	// 仅 H5 支持
      success: function(res) {
          // tempFilePaths 可以作为 img 标签的 src 属性显示图片
          const tempFilePaths = res.tempFiles
      }
  })
  ```

- uni.chooseMessageFile(OBJECT)

  从微信聊天会话中选择文件。

具体详见： [https://uniapp.dcloud.io/api/media/file?id=choosefile](https://uniapp.dcloud.io/api/media/file?id=choosefile)

#### 录音管理

uni.getRecorderManager()

获取 **全局唯一** 的录音管理器 `recorderManager`

```vue
<template>
	<view>
    	<button @tap="startRecord">开始录音</button>
        <button @tap="endRecord">停止录音</button>
        <button @tap="playVoice">播放录音</button>
    </view>
</template>
```

```js
const recorderManager = uni.getRecorderManager()
const innerAudioContext = uni.createInnerAudioContext()

innerAudioContext.autoplay = true

export default {
    data() {
        return {
            text: 'uni-app',
            voicePath: ''
        }
    },
    onLoad() {
        let self = this
        recorderManager.onStop(function(res) {
            console.log('recorder stop' + JSON.stringify(res))
            self.voicePath = res.tempFilePath
        })
    },
    methods: {
        startRecord() {
            console.log('开始录音')
            
            recorderManager.start()
        },
        endRecord() {
            console.log('录音结束')
            recorderManager.stop()
        },
        playVoice() {
            console.log('播放录音')
            
            if(this.voicePath) {
                innerAudioContext.src = this.voicePath
                innerAudioContext.play()
            }
        }
    }
}
```

具体详见： [https://uniapp.dcloud.io/api/media/record-manager?id=getrecordermanager](https://uniapp.dcloud.io/api/media/record-manager?id=getrecordermanager)

#### 背景音频播放管理

uni.getBackgroundAudioManager()

获取 **全局唯一** 的背景音频管理器 `backgroundAudioManager`

背景音频，不是游戏的背景音乐，而是类似 QQ 音乐那样，App在后台时，仍然在播放音乐。

```js
const bgAudioManager = uni.getBackgroundAudioManager()
bgAudioManager.title = '致爱丽丝'
bgAudioManager.singer = '暂无'
bgAudioManager.coverImgUrl = 'https://img-cdn-qiniu.dcloud.net.cn/uniapp/audio/music.jpg'
bgAudioManager.src = 'https://img-cdn-qiniu.dcloud.net.cn/uniapp/audio/music.mp3'
```

具体详见： [https://uniapp.dcloud.io/api/media/background-audio-manager?id=getbackgroundaudiomanager](https://uniapp.dcloud.io/api/media/background-audio-manager?id=getbackgroundaudiomanager)

#### 音频组件控制

创建并返回内部 audio 上下文 `innerAudioContext` 对象。

```js
const innerAudioContext = uni.createInnerAudioContext()
innerAudioContext.autoplay = true
innerAudioContext.src = 'https://vkceyugu.cdn.bspapp.com/VKCEYUGU-hello-uniapp/2cc220e0-c27a-11ea-9dfb-6da8e309e0d8'
innerAudioContext.onPlay(() => {
    console.log('开始播放')
})
innerAudioContext.onError((res) => {
    console.log(res.errMsg)
    console.log(res.errCode)
})
```

具体详见： [https://uniapp.dcloud.io/api/media/audio-context?id=createinneraudiocontext](https://uniapp.dcloud.io/api/media/audio-context?id=createinneraudiocontext)

#### 视频

- uni.chooseVideo(OBJECT)

  拍摄视频或从手机相册中选视频，返回视频的临时文件路径。

  ```vue
  <template>
  	<view>
      	<text>hello</text>
          <button @tap="test">click me</button>
          <video :src="src"></video>
      </view>
  </template>
  ```

  ```js
  export default {
      data() {
          return {
              src: ''
          }
      },
      methods: {
          test: function() {
              let self = this
              uni.chooseVideo({
                  count: 1,
                  sourceType: ['camera','album'],
                  success: function(res) {
                      self.src = res.tempFilePath
                  }
              })
          }
      }
  }
  ```

- uni.chooseMedia(OBJECT)

  拍摄或从手机相册中选择图片或视频。

  ```js
  uni.chooseMedia({
      count: 9,
      mediaType: ['image','video'],
      sourceType: ['album','camera'],
      maxDuration: 30,
      camera: 'back',
      success(res) {
          console.log(res.tempFilest)
      }
  })
  ```

- uni.saveVideoToPhotosAlbum(OBJECT)

  保存视频到系统相册。

  ```vue
  <template>
      <view>
          <text>hello</text>
          <button @tap="test">click me</button>
          <video :src="src"></video>
      </view>
  </template>
  ```

  ```js
  export default {
  	data() {
          return {
              src: ''
          }
      },
      methods: {
          test: function() {
              let self = this
              uni.chooseVideo({
                  count: 1,
                  sourceType: ['camera'],
                  success: function(res) {
                      self.src = res.tempFilePath
                      
                      uni.saveVideoToPhotosAlbum({
                          filePath: res.tempFilePath,
                          success: function() {
                              console.log('save success')
                          }
                      })
                  }
              })
          }
      }
  }
  ```

- uni.getVideoInfo(OBJECT)

  获取视频详情信

- uni.compressVideo(OBJECT)

  压缩视频接口。开发者可指定压缩质量 quality 进行压缩。当需要更精细的控制时，可指定 bitrate 、 fps 和 resolution，当 quality 传入时，这三个参数将被忽略。

- uni.openVideoEditor(OBJECT)

  打开视频编辑器

具体详见： [https://uniapp.dcloud.io/api/media/video?id=choosevideo](https://uniapp.dcloud.io/api/media/video?id=choosevideo)

#### 视频组件控制

uni.createVideoContext(videoId,this)

创建并返回 video 上下文 videoContext 对象。在自定义组件下，第二个参数传入组件实例this，以操作组件内 `<video>` 组件。

```vue
<template>
	<view>
    	<view class="page-body">
    		<view class="page-section">
                <video id="myVideo" src="http://img.cdn.qiniu.dcloud.net.cn/wap2appvsnative.mp4" @error="videoErrorCallback" :danmu-list="danmuList" enable-danmu danmu-btn controls></video>
                <view class="uni-list">
                    <view class="uni-list-cell">
    					<view>
                            <view class="uni-label">弹幕内容</view>
    					</view>
                        <view class="umi-list-cell-db">
                            <input @blur="bindInputBlur" class="uni-input" type="text" placeholder="在此输入弹幕内容" />
    					</view>
    				</view>
    			</view>
                <view class="btn-area">
                    <button @tap="bindSendDanmu" class="page-body-button" formType="submit">发送弹幕</button>
    			</view>
    		</view>
    	</view>
    </view>
</template>
```

```js
export default {
    data() {
        return {
            title: 'video',
            src: '',
            inputValue: '',
            danmuList: [{
                text: '第 1s 出现的弹幕',
                color: '#ff0000',
                time: 1
            },{
                text: '第 3s 出现的弹幕',
                color: '#ff00ff',
                time: 3
            }]
        }
    },
    onReady: function(res) {
        this.videoContent = uni.createVideoContent('myVideo')
    },
    methods: {
        bindInputBlur: function(e) {
            this.inputValue = e.target.value
        },
        bindButtonTap: function() {
            let that = this
            uni.chooseVideo({
                sourceType: ['album','camera'],
                maxDuration: 60,
                camera: ['front','back'],
                success: function(res) {
                    this.src = res.tempFilePath
                }
            })
        },
        bindSendDanmu: function() {
            this.videoContext.sendDanmu({
                text: this.inputValue,
                color: this.getRandomColor()
            })
        },
        videoErrorCallback: function(e) {
            console.log('视频错误信息：')
            console.log(e.target.errMsg)
        },
        getRandomColor: function() {
            const rgb = []
            for(let i = 0;i < 3;++i) {
                let color = Math.floor(Math.random() * 256).toString(16)
                color = color.length == 1 ? '0' + color : color
                rgb.push(color)
            }
            return '#' + rgb.join('')
        }
    }
}
```

具体详见： [https://uniapp.dcloud.io/api/media/video-context?id=createvideocontext](https://uniapp.dcloud.io/api/media/video-context?id=createvideocontext)

#### 相机组件控制

- uni.createCameraContext()

  创建并返回 camera 组件的上下文 cameraContext 对象。

- cameraContext.takePhoto

- cameraContext.setZoom

- cameraContext.startRecord

- cameraContext.stopRecord

具体详见： [https://uniapp.dcloud.io/api/media/camera-context?id=createcameracontext](https://uniapp.dcloud.io/api/media/camera-context?id=createcameracontext)

#### 直播组件控制

- uni.createLivePlayerContext(livePlayerId,this)

  创建 live-player 上下文 livePlayerContext 对象。注意是直播的播放而不是推流。

- uni.createLivePusherContext

  创建 live-pusher 上下文 livePusherContext 对象

具体详见： [https://uniapp.dcloud.io/api/media/live-player-context?id=createliveplayercontext](https://uniapp.dcloud.io/api/media/live-player-context?id=createliveplayercontext)

#### 富文本

- editorContext

  editor 组件对应的 editorContext 实例，可通过 [uni.createSelectorQuery](https://uniapp.dcloud.io/api/ui/nodes-info?id=createselectorquery) 获取。

  `editorContent` 通过 `id` 跟一个 `<editor>` 组件绑定，操作对应的 `<editor>` 组件。

- editorContext.format（name,value）

  修改样式

- editorContext.insertDivider(OBJECT)

  插入分割线

- editorContext.insertImage(OBJECT)

  插入图片

- editorContext.insertText(OBJECT)

  覆盖当前选取，设置一段文本

- editorContext.setContents(OBJECT)

  初始化编辑器内容，html 和 delta 同时存在时仅 delta 生效

- editorContext.getContents(OBJECT)

  获取编辑器内容

- editorContext.clear(OBJECT)

  清空编辑器内容

- editorContext.removeFormat(OBJECT)

  清除当前选区的样式

- editorContext.undo(OBJECT)

  撤销

- editorContext.redo(OBJECT)

  恢复

具体详见： [https://uniapp.dcloud.io/api/media/editor-context?id=editorcontext](https://uniapp.dcloud.io/api/media/editor-context?id=editorcontext)

#### 音视频合成

- uni.createMediaContainer

  创建音视频处理容器，最终可将容器中的轨道合成一个视频，返回 `MediaContainer` 对象

  + MediaContainer.addTrack(track)

    将音频或视频轨道添加到容器

  + MediaContainer.destroy()

    将容器销毁，释放资源

  + MediaContainer.export()

    将容器内的轨道合并并导出视频文件

  + MediaContainer.extractDataSource(object)

    将容器内的轨道合并并导出视频文件，返回 `MediaTrack` 对象

  + MediaContainer.removeTrack(track)

    将音频或视频轨道添加到容器

- MediaTrack

  可通过 `MediaContainer.extractDataSource` 返回。

  `MediaTrack` 音频或视频轨道，可以对轨道进行一些操作

具体详见： [https://uniapp.dcloud.io/api/media/media-container?id=createmediacontainer](https://uniapp.dcloud.io/api/media/media-container?id=createmediacontainer)

### 设备

