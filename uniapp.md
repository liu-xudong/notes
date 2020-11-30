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

#### 系统信息

- uni.getSystemInfo(OBJECT)

  获取系统信息

  ```js
  uni.getSystemInfo({
      success: function(res) {
          console.log(res.model)	// 手机型号
          console.log(res.pixelRatio)	// 设备像素比
          console.log(res.windowWidth)	// 可使用窗口宽度
          console.log(res.windowHeight)	// 可使用窗口高度
          console.log(res.language)	// 应用设置的语言
          console.log(res.version)	// 引擎版本号
          console.log(res.platform)	// 客户端平台，值域为：'ios'、'android'
      }
  })
  ```

- uni.getSystemInfoSync()

  获取系统信息同步接口（使用同上getSystemInfo）

  ```js
  try {
      const res = uni.getSystemInfoSync()
      console.log(res.model)	// 手机型号
      console.log(res.pixelRatio)	// 设备像素比
      console.log(res.windowWidth)	// 可使用窗口宽度
      console.log(res.windowHeight)	// 可使用窗口高度
      console.log(res.language)	// 应用设置的语言
      console.log(res.version)	// 引擎版本号
      console.log(res.platform)	// 客户端平台，值域为：'ios'、'android'
  } catch(e) {
      // error
  }
  ```

- uni.canIUser(String)

  判断应用的 API ，回调，参数，组件等是否在当前版本可用。

  ```js
  uni.canIUse('getSystemInfoSync.return.screenWidth')
  uni.canIUse('getSystemInfo.success.screenWidth')
  uni.canIUse('showToast.object.image')
  uni.canIUse('requset.object.method.GET')
  
  uni.canIUse('live-player')
  uni.canIUse('text.selectable')
  uni.canIUse('button.open-type.contact')
  ```

具体详见： [https://uniapp.dcloud.io/api/system/info?id=getsysteminfo](https://uniapp.dcloud.io/api/system/info?id=getsysteminfo)

#### 内存

uni.onMemoryWarning(CALLBACK)

监听内存不足警告事件

```js
uni.onMemoryWarning(function() {
    console.log('onMemoryWarningReceive')
})
```

具体详见： [https://uniapp.dcloud.io/api/system/memory](https://uniapp.dcloud.io/api/system/memory)

#### 网络状态

- uni.getNetworkType(OBJECT)

  获取网络类型

  ```js
  uni.getNetworkType({
      success: function(res) {
          console.log(res.networkType)
      }
  })
  ```

- uni.onNetworkStatusChange(CALLBACK)

  监听网络状态变化。

  ```js
  uni.onNetworkStatusChange(function(res) {
      console.log(res.isConnected)
      console.log(res.networkType)
  })
  ```

具体详见： [https://uniapp.dcloud.io/api/system/network](https://uniapp.dcloud.io/api/system/network)

#### 系统主题

uni.onThemeChange(CALLBACK)

监听系统主题状态变化。

```js
uni.onThemeChange(function(res) {
    console.log(res.theme)
})
```

具体详见： [https://uniapp.dcloud.io/api/system/theme](https://uniapp.dcloud.io/api/system/theme)

#### 加速度计

- uni.onAccelerometerChange(CALLBACK)

  监听加速度数据，频率： 5次/秒，接口调用后自动开始监听，可使用 `uni.offAccelerometer` 取消监听。

  ```js
  uni.onAccelerometerChange(function(res) {
      console.log(res.x)
      console.log(res.y)
      console.log(res.z)
  })
  ```

- uni.offAccelerometerChange(CALLBACK)

  取消监听加速度数据。

- uni.startAccelerrometer(OBJECT)

  开始监听加速度数据。

  ```js
  uni.startAccelerometer()
  ```

- uni.stopAceelerometer(OBJECT)

  停止监听加速度数据。

  ```js
  uni.stopAccelerometer()
  ```

具体详见 ： [https://uniapp.dcloud.io/api/system/accelerometer?id=onaccelerometerchange](https://uniapp.dcloud.io/api/system/accelerometer?id=onaccelerometerchange)

#### 罗盘

- uni.onCompassChange(CALLBACK)

  监听罗盘数据，频率： 5次/秒，接口调用后会自动开始监听，可使用 `uni.offCompassChange` 取消监听。

  ```js
  uni.onCompassChange(function(res) {
      console.log(res.direction)
  })
  ```

- uni.offCompassChange(CALLBACK)

  取消监听罗盘数据。

- uni.startCompass(OBJECT)

  开始监听罗盘数据。

  ```js
  uni.startCompass()
  ```

- uni.stopCompass(OBJECT)

  停止监听罗盘数据。

  ```js
  uni.stopCompass()
  ```

具体详见: [https://uniapp.dcloud.io/api/system/compass?id=oncompasschange](https://uniapp.dcloud.io/api/system/compass?id=oncompasschange)

#### 陀螺仪

- uni.onGyroscopeChange(CALLBACK)

  监听陀螺仪数据变化事件。

- uni.startGyroscope(OBJECT)

  开始监听陀螺仪数据。

- uni.stopGyroscope(OBJECT)

  停止监听陀螺仪数据。

```vue
<template>
	<view>
    	<button @click="start">开始监听陀螺仪变化</button>
        <button @click="stop">停止监听陀螺仪变化</button>
    </view>
</template>
```

```js
export default {
    methods: {
        start() {
            uni.onGyroscopeChange((res) => {
                console.log('gyroData.rotationRate.x = ' + res.x)
                console.log('gyroData.rotationRate.y = ' + res.y)
                console.log('gyroData.rotationRate.z = ' + res.z)
            })
            uni.startGyroscope({
                interval: 'normal',
                success() {
                    console.log('success')
                },
                fail() {
                    console.log('fail')
                }
            })
        },
        stop() {
            uni.stopGyroscope({
                success() {
                    console.log('stop success!')
                },
                fail() {
                    console.log('stop fail!')
                }
            })
        }
    }
}
```

具体详见： [https://uniapp.dcloud.io/api/system/gyroscope?id=ongyroscopechange](https://uniapp.dcloud.io/api/system/gyroscope?id=ongyroscopechange)

#### 拨打电话

uni.makePhoneCall(OBJECT)

拨打电话。

```js
uni.makePhoneCall({
    phoneNumber: '114'	// 示例
})
```

具体详见： [https://uniapp.dcloud.io/api/system/phone](https://uniapp.dcloud.io/api/system/phone)

#### 扫码

调起客户端扫码界面，扫码成功后返回对应的结果。

```js
// 允许从相机和相册扫码
uni.scanCode({
    success: function(res) {
        console.log('条码类型' + res.scanType)
        console.log('条码内容' + res.result)
    }
})

// 只允许通过相机扫码
uni.scanCode({
    onlyFromCamera: true,
    success:function(res) {
        console.log('条码类型' + res.scanType)
        console.log('条码内容' + res.result)
    }
})

// 调起条码扫描
uni.scanCode({
    scanType: ['barCode'],
    success: function(res) {
        console.log('条码类型' + res.scanType)
        console.log('条码内容' + res.result)
    }
})
```

具体详见： [https://uniapp.dcloud.io/api/system/barcode?id=scancode](https://uniapp.dcloud.io/api/system/barcode?id=scancode)

#### 剪贴板

- uni.setClipboardData(OBJECT)

  设置系统剪贴板的内容。

  ```js
  uni.setClipboardData({
      data: 'hello',
      success: function() {
          console.log('success')
      }
  })
  ```

- uni.getClipboardData(OBJECT)

  获取系统剪贴板内容。

  ```js
  uni.getClipboardData({
      success:function(res) {
          console.log(res.data)
      }
  })
  ```

具体详见： [https://uniapp.dcloud.io/api/system/clipboard?id=setclipboarddata](https://uniapp.dcloud.io/api/system/clipboard?id=setclipboarddata)

#### 屏幕

- uni.setScreenBrightness(OBJECT)

  设置屏幕亮度

  ```js
  uni.setScreenBrightness({
      value: 0.5,
      success: function() {
          console.log('success')
      }
  })
  ```

- uni.getScreenBrightness(OBJECT)

  获取屏幕宽度

  ```js
  uni.getScreenBrightness({
      success: function(res) {
          console.log('屏幕亮度值' + res.value)
      }
  })
  ```

- uni.setKeepScreenOn(OBJECT)

  设置是否保持常亮状态。仅在当前应用生效，离开应用后设置失效。

  ```js
  // 保持屏幕常亮
  uni.setKeepScreenOn({
      keepScreenOn: true
  })
  ```

具体详见： [https://uniapp.dcloud.io/api/system/brightness?id=setscreenbrightness](https://uniapp.dcloud.io/api/system/brightness?id=setscreenbrightness)

#### 用户截屏事件

uni.onUserCaptureScreen(CALLBACK)

监听用户主动截屏事件，用户使用系统截屏按键截屏时触发此事件。

```js
uni.onUserCaptureScreen(function() {
    console.log('用户截屏了')
})
```

#### 振动

- uni.vibrate(OBJECT)

  使手机发生振动。

  ```js
  uni.vibrate({
      success: function() {
          console.log('success')
      }
  })
  ```

- uni.vibrateLong(OBJECT)

  使手机发生较长时间的振动（400ms）。

  ```js
  uni.vibrateLong({
      success: function() {
          console.log('success')
      }
  })
  ```

- uni.vibrateShort(OBJECT)

  是手机发生较短时间的振动（15ms）。

  ```js
  uni.vibrateShort({
      success: function() {
          console.log('success')
      }
  })
  ```

具体详见： [https://uniapp.dcloud.io/api/system/vibrate?id=vibrate](https://uniapp.dcloud.io/api/system/vibrate?id=vibrate)

#### 手机联系人

uni.addPhoneContact(OBJECT)

调用后，用户可以选择该表单以“新增联系人”或“添加到已有联系人”的方式，写入手机系统通讯录，完成手机通讯录联系人和联系方式的增加。

```js
uni.addPhoneContact({
    nickName: '昵称',
    lastName: '姓',
    firstName: '名',
    remark: '备注',
    mobilePhoneNumber: '114',
    success: function() {
        console.log('success')
    },
    fail: function() {
        console.log('fail')
    }
})
```

具体详见： [https://uniapp.dcloud.io/api/system/contact?id=addphonecontact](https://uniapp.dcloud.io/api/system/contact?id=addphonecontact)

#### 蓝牙

- uni.openBluetoothAdapter(OBJECT)

  初始化蓝牙模块

  ```js
  uni.openBluetoothAdapter({
      success(res) {
          console.log(res)
      }
  })
  ```

- uni.startBluetoothDebicesDiscovery(OBJECT)

  开始搜寻附近的蓝牙外围设备。

  ```js
  // 以微信硬件平台的蓝牙智能灯为例，主服务的 UUID 是 FEE7. 传入这个参数，只搜索主服务 UUID 为 FEE7 的设备
  uni.startBluetoothDevicesDiscobery({
      services: ['FEE7'],
      success(res) {
          console.log(res)
      }
  })
  ```

- uni.onBluetoothDevicesFound(CALLBACK)

  监听寻找到新设备的事件

  ```js
  // ArrayBuffer 转 16 进度字符串示例
  function ab2hex(buffer) {
      const hexArr = Array.prototype.map.call(
      	new Uint8Array(buffer),
          function(bit) {
              return('00' + bit.toString(16)).slice(-2)
          }
      )
      return hexArr.join('')
  }
  uni.onBluetoothDeviceFound(function(devices) {
      console.log('new device list has founded')
      console.dir(devices)
      console.log(ab2hex(devices[0].advertisData))
  })
  ```

- uni.stopBluetoothDevicesDiscovery(OBJECT)

  停止搜索附近的蓝牙外围设备。若已经找到需要的蓝牙设备并不需要继续搜索时，建议调用该接口停止蓝牙搜索。

  ```js
  uni.stopBluetoothDevicesDiscovery({
      success(res) {
          console.log(res)
      }
  })
  ```

- uni.onBluetoothAdapterStateChange(CALLBACK)

  监听蓝牙适配器状态变化事件

  ```js
  uni.onBluetoothAdapterStateChange(function(res) {
      console.log('adapterState changed,now is', res)
  })
  ```

- uni.getConnectedBluetoothDevices(OBJECT)

  根据 uuid 获取处于已连接状态的设备

  ```js
  uni.getConnecteBluetoothDevices({
      success(res) {
          console.log(res)
      }
  })
  ```

- uni.getBluetoothDevices(OBJECT)

  获取在蓝牙模块生效期间所有已发现的蓝牙设备。包括已经和本机处于连接状态的设备。

  ```js
  // ArrayBuffer转16进度字符串示例
  function ab2hex(buffer) {
      const hexArr = Array.prototype.map.call(
      	new Uint8Array(buffer)
          function(bit) {
          	return ('00' + bit.toString(16)).slice(-2)
      	}
      )
      return hexArr.join('')
  }
  uni.getBluetoothDevices({
      success(res) {
          console.log(res)
          if(res.devices[0]) {
              console.log(ab2hex(res.devices[0].advertisData))
          }
      }
  })
  ```

- uni.getBluetoothDdapterState(OBJECT)

  获取本机蓝牙适配器状态。

  ```js
  uni.getBluetoothAdapterState({
      success(res) {
          console.log(res)
      }
  })
  ```

- uni.closeBluetoothAdapter(OBJECT)

  关闭蓝牙模块。调用该方法将断开所有已建立的连接并释放系统资源。建议在使用蓝牙流程后，与 `uni.openBluetoothAdapter` 成对调用。

  ```js
  uni.closeBluetoothAdapter({
      success(res) {
          console.log(res)
      }
  })
  ```

具体详见： [https://uniapp.dcloud.io/api/system/bluetooth](https://uniapp.dcloud.io/api/system/bluetooth)

#### 低功耗蓝牙

- uni.setBLEMTU(OBJECT)

  设置蓝牙最大传输单元。需在 uni.createBLEConnection 调用成功后调用， mtu 设置范围 (22,512)。

- uni.writeBLECharacteristicValue(OBJECT)

  向低功耗蓝牙设备特征值中写入二进制数据。

  ```js
  // 向蓝牙设备发送一个 0x00 的16进制数据
  const buffer = new ArrayBuffer(1)
  const dataView = new DataView(buffer)
  dataView.setUint8(0,0)
  uni.writeBLECHaracteristicValue({
      // 这里的 deviceId 需要在 getBluetoothDevices 或 onBluetoothDeviceFound 接口中获取
      deviceId,
      // 这里的 serviceId 需要在 getBLEDeviceServices 接口中获取
      serviceId,
      // 这里的 characteristicId 需要在 getBLEDeviceCharacteristics 接口中获取
      characteristicId,
      // 这里的 value 是 ArrayBuffer 类型
      value: buffer,
      success(res) {
          console.log('writeBLECHaracteristicValue success', res.errMsg)
      }
  })
  ```

- uni.readBLECharacteristicValue(OBJECT)

  读取低功耗蓝牙设备的特征值的二进制数据值。注意：必须设备的特征值支持 read 才可以成功调用。

  ```js
  // 必须在这里的回调才能获取
  uni.onBLECHaracteristicValueChange(function(characteristic) {
      console.log('characteristic value comed:', characteristic)
  })
  uni.readBLECHaracteristicValue({
      // 这里的 deviceId 需要已经通过 createBLEConnection 与对应设备建立连接
      deviceId,
      // 这里的 serviceId 需要在 getBLEDeviceServices 接口中获取
      serviceId,
      // 这里的 characteristicId 需要在 getBLEDeviceCharacteristics 接口中获取
      characteristicId,
      success(res) {
          console.log('readBLECHaracteristicValue', res.errCode)
      }
  })
  ```

- uni.onBLEConnectionStateChange(CALLBACK)

  监听低功耗蓝牙连接状态的改变事件。包括开发者主动连接或断开连接，设备丢失，连接异常断开等等。

  ```js
  uni.onBLEConnectionStateChange(function(res) {
      // 该方法回调中可以用于处理连接意外断开等异常情况
      console.log(`device ${res.deviceId} state has changed, connected: ${res.connected}`)
  })
  ```

- uni.onBLECharacteristicValueChange(CALLBACK)

  监听低功耗蓝牙设备的特征值变化事件。必须先启用 `notifyBLECharacteristicValueChange` 接口才能接受到设备推送的 notification。

  ```js
  // ArrayBuffer 转 16 进度字符串示例
  function ab2hex(buffer) {
      const hexArr = Array.prototype.map.call(
      	new Uint8Array(buffer),
          function(bit) {
              return ('00' + bit.toString(16)).slice(-2)
          }
      )
      return hexArr.join('')
  }
  uni.onBLECharacteristicValueChange(function(res) {
      console.log(`characteristic ${res.characteristicId} has changed,now is ${res.value}`)
      console.log(ab2hex(res.value))
  })
  ```

- uni.notifyBLECharacteristicValueChange(OBJECT)

  启用低功耗蓝牙设备特征值变化时的 notify 功能，订阅特征值。注意：必须设备的特征值支持 notify 或者 indicate 才可以成功调用。另外，必须先启用 `notifyBLECharacteristicValueChange` 才能监听到设备 `characteristicValueChange` 事件。

  ```js
  uni.notifyBLECharacteristicValueChange({
      state: true,	// 启用 notify 功能
      // 这里的 deviceId 需要已经通过 createBLEConnection 与对应设备建立连接
      deviceId,
      // 这里的 serviceId 需要在 getBLEDeviceServices 接口中获取
      serviceId,
      // 这里的 characteristicId 需要在 getBLEDeviceCharacteristics 接口中获取
      characteristicId，
      success(res) {
          console.log('notifyBLECharacteristicValueChange success', res.errMsg)
      }
  })
  ```

- uni.getBLEDeviceServices(OBJECT)

  获取蓝牙设备所有服务(service)。

  ```js
  uni.getBLEDebiceServices({
      // 这里的 deviceId 需要已经通过 createBLEConnection 与对应设备建立连接
      deviceId,
      success(res) {
          console.log('device services:',res.services)
      }
  })
  ```

- uni.getBLEDeviceRSSI(OBJECT)

  获取蓝牙设备信号强度

- uni.getBLEDeviceCharacteristics(OBJECT)

  获取蓝牙设备某个服务中所有特征值(characteristic)。

  ```js
  uni.getBLEDebiceCharacteristics({
      // 这里的 deviceId 需要已经通过 createBLEConnection 与对应设备建立连接
      deviceId,
      // 这里的 serviceId 需要在 getBLEDeviceServices 接口中获取
      serviceId,
      success(res) {
          console.log('device getBLEDebiceCharacteristics:', res.characteristics)
      }
  })
  ```

- uni.createBLEConnection(OBJECT)

  连接低功耗蓝牙设备。

  ```js
  uni.createBLEConnection({
      // 这里的 deviceId 需要已经通过 createBLEConnection 与对应设备建立联系
      deviceId,
      success(res) {
          console.log(res)
      }
  })
  ```

- uni.closeBLEConnection(OBJECT)

  断开与低功耗蓝牙设备的连接。

  ```js
  uni.closeBLEConnection({
      deviceId,
      success(res) {
          console.log(res)
      }
  })
  ```

具体详见： [https://uniapp.dcloud.io/api/system/ble?id=setblemtu](https://uniapp.dcloud.io/api/system/ble?id=setblemtu)

#### iBeacon

- uni.onBeaconServiceChange(CALLBACK)

  监听 iBeacon 服务状态变化事件

- uni.onBeaconUpdata(CALLBACK)

  监听 iBeacon 设备更新事件

- uni.getBeacons(OBJECT)

  获取所有已搜索到的 iBeacon 设备

- uni.startBeaconDiscovery(OBJECT)

  开始搜索附近的 iBeacon 设备

  ```js
  uni.startBeaconDiscovery({
      success(res) { }
  })
  ```

- uni.stopBeaconDiscovery(OBJECT)

  停止搜索附近的 iBeacon 设备

- IBeaconInfo

- 注意事项

具体详见： [https://uniapp.dcloud.io/api/system/ibeacon?id=onbeaconservicechange](https://uniapp.dcloud.io/api/system/ibeacon?id=onbeaconservicechange)

#### Wi-Fi

仅微信小程序平台、App平台、字节跳动小程序支持，各平台开发方式暂未统一，使用时需注意用[条件编译](https://uniapp.dcloud.io/platform)调用不同平台的代码。

微信小程序平台实现参考：[规范详情](https://developers.weixin.qq.com/miniprogram/dev/api/wx.startWifi.html)

字节跳动小程序的wifi API参考：[规范详情](https://developer.toutiao.com/dev/cn/mini-app/develop/api/device/wi-fi/getconnectedwifi)

App 平台实现参考

**安卓：**

- [获取WIFI列表](https://ask.dcloud.net.cn/question/12113)

**ios：**

- [打开ios的WIFI设置页面](https://ask.dcloud.net.cn/question/7797)

#### 电量

电量API暂未统一，需分平台条件编译编写。

- 微信小程序平台：[规范详情](https://developers.weixin.qq.com/miniprogram/dev/api/wx.getBatteryInfoSync.html)
- 百度小程序平台：[规范详情](https://smartprogram.baidu.com/docs/develop/api/device_battery/#swan-getBatteryInfo/)
- 支付宝小程序平台：[规范详情](https://docs.alipay.com/mini/api/nrnziy)
- QQ小程序平台：[规范详情](https://q.qq.com/wiki/develop/miniprogram/API/equipment/ibeacon_battery.html)
- App平台：使用 Native.js，[参考](https://ask.dcloud.net.cn/article/992)
- H5平台：使用 navigator.battery API

#### NFC

仅微信小程序平台、App平台支持，各平台开发方式暂未统一，使用时需注意用[条件编译](https://uniapp.dcloud.io/platform)调用不同平台的代码。

- 微信小程序平台实现参考：[规范详情](https://developers.weixin.qq.com/miniprogram/dev/api/wx.startHCE.html)
- App 平台通过Native.js实现，**安卓：**[NFC数据读取](https://ask.dcloud.net.cn/question/6726)

#### 设备方向

[监听设备方向变化](https://uniapp.dcloud.io/api/system/deviceMotion?id=监听设备方向变化)

仅微信小程序平台支持，[规范详情](https://developers.weixin.qq.com/miniprogram/dev/api/device/motion/wx.startDeviceMotionListening.html)

在App平台，也可以通过监听窗体大小变化onResize来实现此类需求。[详见](https://uniapp.dcloud.io/collocation/frame/lifecycle?id=页面生命周期)

#### 生物认证

- 生物认证说明

  生物认证，又称活体检测。它包含指纹识别、人脸识别两部分。即通过人体身体特征来进行身份认证识别。

- uni.startSoterAuthentication(OBJECT)

  开始 SOTER 生物认证。

- uni.checkIsSupportSoterAuthentication(OBJECT)

  获取本机支持的 SOTER 生物认证方式

- uni.checkIsSoterEnrolledInDevice(OBJECT)

  获取设备内是否录入如指纹等生物信息的接口

```vue
<template>
	<view class="content">
    	<button type="primary" @click="checkIsSupportSoterAuthentication">检查支持的认证方式</button>
        <button type="primary" @click="checkIsSoterEnrolledInDeviceFingerPrint">检查是否录入指纹</button>
        <button type="primary" @click="checkIsSoterEnrolledInDeviceFaceID">检查是否录入FaceID</button>
        <button type="primary" @click="startSoterAuthenticationFingerPrint">开始指纹认证</button>
        <button type="primary" @click="startSoterAuthenticationFaceID">开始FaceID认证</button>
    </view>
</template>

<script>
	export default {
        data() {
            return{}
        },
        onLoad() {
            
        },
        methods: {
            checkIsSupportSoterAuthentication() {
                uni.checkIsSupportSoterAuthentication({
                    success(res) {
                        console.log(res)
                    },
                    fail(err) {
                        console.log(err)
                    },
                    complete(res) {
                        console.log(res)
                    }
                })
            },
            checkIsSoterEnrolledInDeviceFingerPrint() {
                uni.checkIsSoterEnrolledInDevice({
                    checkAuthMode: 'fingerPrint',
                    success(res) {
                        console.log(res)
                    },
                    fail(err) {
                        console.log(err)
                    },
                    complete(res) {
                        console.log(res)
                    }
                })
            },
            checkIsSoterEnrolledInDeviceFaceID() {
                uni.checkIsSoterEnrolledInDevice({
                    checkAuthMode: 'facial',
                    success(res) {
                        console.log(res)
                    },
                    fail(err) {
                        console.log(err)
                    },
                    complete(res) {
                        console.log(res)
                    }
                })
            },
            startSoterAuthenticationFingerPrint() {
                uni.startSoterAuthentication({
                    requestAuthModes:['fingerPrint'],
                    challenge: '123456',
                    authContent: '请用指纹解锁',
                    success(res) {
                        console.log(res)
                    },
                    fail(err) {
                        console.log(err)
                    },
                    complete(res) {
                        console.log(res)
                    }
                })
            },
            startSoterAuthenticationFaceID() {
                uni.startSoterAuthentication({
                    requestAuthModes: ['facial'],
                    challenge: '123456',
                    authContent: '请用FaceID解锁'，
                    success(res) {
                        console.log(res)
                    },
                    fail(err) {
                        console.log(err)
                    },
                    complete(res) {
                        console.log(res)
                    }
                })
            }
        }
    }
</script>
```

具体详见： [https://uniapp.dcloud.io/api/system/authentication?id=%e7%94%9f%e7%89%a9%e8%ae%a4%e8%af%81%e8%af%b4%e6%98%8e](https://uniapp.dcloud.io/api/system/authentication?id=%e7%94%9f%e7%89%a9%e8%ae%a4%e8%af%81%e8%af%b4%e6%98%8e)

### Worker

目前需分平台编写

- 微信小程序：[规范详情](https://developers.weixin.qq.com/miniprogram/dev/api/worker/wx.createWorker.html)
- 字节跳动小程序：[规范详情](https://developer.toutiao.com/docs/game/worker/tt.createWorker.html)
- QQ小程序：[规范详情](https://q.qq.com/wiki/develop/miniprogram/API/worker/worker.html)
- H5：标准H5的worker仍然可以使用
- App：App的js是在独立的jscore运行的，如果需要在另一个线程运行js，可以使用web-view组件或renderjs，这样的js运行在webview里，和jscore里的js是两个线程。但注意多个webview之间的js是一个进程，使用webview里的js时注意会影响视图层的渲染。

### 键盘

#### uni.hideKeyboard

隐藏软键盘

#### uni.onKeyboardHeightChange(CALLBACK)

监听键盘高度变化

```js
uni.onKeyboardHeightChange(res => {
    console.log(res.height)
})
```

具体详见： [https://uniapp.dcloud.io/api/key?id=hidekeyboard](https://uniapp.dcloud.io/api/key?id=hidekeyboard)

### 界面

#### 交互反馈

- uni.showToast(OBJECT)

  显示消息提示

  ```js
  uni.showToast({
      title: '标题',
      duration: 2000
  })
  ```

- uni.hideToast()

  隐藏消息提示框

  ```js
  uni.hideToast()
  ```

- uni.shwoLoading(OBJECT)

  显示 loading 提示框，需主动调用 `uni.hideLoading` 才能关闭提示框。

  ```js
  uni.showLoading({
      title: '加载中'
  })
  ```

- uni.hideLoading()

  隐藏 loading 提示框

  ```js
  uni.showLoding({
      title: '加载中'
  })
  
  setTimeout(function() {
      uni.hideLoading()
  },2000)
  ```

- uni.showModal(OBJECT)

  显示模态弹窗，类似于标准 html 的消息框： alert、confirm

  ```js
  uni.showModal({
      title: '提示',
      content: '这是一个模态弹窗'，
      success: function(res) {
      	if(res.confirm) {
              console.log('用户点击确定')
          } else if(res.cancel) {
              console.log('用户点击取消')
          }
  	}
  })
  ```

- uni.showActionSheet(OBJECT)

  显示操作菜单

  ```js
  uni.showActionSheet({
      itemList: ['A','B','C'],
      success: function(res) {
          console.log('选中了第' + (res.tapIndex + 1) + '个按钮')
      }，
      fail: function(res) {
          console.log(res.errMsg)
      }
  })
  ```

具体详见： [https://uniapp.dcloud.io/api/ui/prompt?id=showtoast](https://uniapp.dcloud.io/api/ui/prompt?id=showtoast)

#### 设置导航条

- uni.setNavigationBarTitle(OBJECT)

  动态设置当前页面的标题

  ```js
  uni.setNavigationBarTitle({
      title: '新的标题'
  })
  ```

- uni.setNavigationBarColor(OBJECT)

  设置页面导航条颜色。如果需要进入页面就设置颜色，请延迟执行，防止被框架内设置的颜色逻辑覆盖

  ```js
  uni.setNavigationBarColor({
      frontColor: '#ffffff',
      backgroundColor: '#ff0000',
      animation: {
          duration: 400,
          timingFunc: 'easeIn'
      }
  })
  ```

- uni.showNavigationBarLoading(OBJECT)

  在当前页面显示导航条加载动画

  ```js
  uni.showNavigationBarLoading()
  ```

- uni.hideNavigationBarLoading(OBJECT)

  在当前页面隐藏导航条加载动画

  ```js
  uni.hideNabigationBarLoading()
  ```

- uni.hideHomeButton(OBJECT)

  隐藏返回首页按钮

  ```js
  uni.hideHomeButton()
  ```

具体详见： [https://uniapp.dcloud.io/api/ui/navigationbar?id=setnavigationbartitle](https://uniapp.dcloud.io/api/ui/navigationbar?id=setnavigationbartitle)

#### 设置 TabBar

- uni.setTabBarItem(OBJECT)

  动态设置 tabBar 某一项的内容

  ```js
  uni.setTabBarItem({
      index: 0,
      text: 'text',
      iconPath: '/path/to/iconPath',
      selectedIconPath: '/path/to/selectedIconPath'
  })
  ```

- uni.setTabBarStyle(OBJECT)

  动态设置 tabBar 的整体样式

  ```js
  uni.setTabBarStyle({
      color: '#ff0000',
      selsectedColor: '#00ff00',
      backgroundColor: '#0000ff',
      borderStyle: 'white'
  })
  ```

- uni.hideTabBar(OBJECT)

  隐藏 tabBar

- uni.showTabBar(OBJECT)

  显示 tabBar

- uni.setTabBarBadge(OBJECT)

  为 tabBar 某一项的右上角添加文本

  ```js
  uni.setTabBarBadge({
      index: 0,
      text: '1'
  })
  ```

- uni.removeTabBarBadge(OBJECT)

  移除 tabBar 某一项右上角文本。

- uni.showTabBarRedDot(OBJECT)

  显示 tabBar 某一项的右上角的红点。

- uni.hideTabBarRedDot(OBJECT)

  隐藏 tabBar 某一项的右上角的红点。

- uni.onTabBarMidButtonTap(CALLBACK)

  监听中间按钮的点击事件。

具体详见： [https://uniapp.dcloud.io/api/ui/tabbar?id=settabbaritem](https://uniapp.dcloud.io/api/ui/tabbar?id=settabbaritem)

#### 背景

- uni.setBackgroundColor(OBJECT)

  动态设置窗口的背景色。

  ```js
  uni.setBackgroundColor({
      backgroundColor: '#ffffff',
      backgroundColorTop: '#222222',
      backgroundColorBottom: '#333333'
  })
  ```

- uni.setBackgroundTextStyle(OBJECT)

  动态设置下拉背景字体、loading 图的样式

  ```js
  uni.setBackgroundTextStyle({
      textStyle: 'dark'	// 下拉背景字体、loading 图的样式为dark
  })
  ```

具体详见： [https://uniapp.dcloud.io/api/ui/bgcolor?id=setbackgroundcolor](https://uniapp.dcloud.io/api/ui/bgcolor?id=setbackgroundcolor)

#### 动画

uni.createAnimation(OBJECT)

创建一个动画示例 `animation` 。调用实例的方法来描述动画。最后通过动画实例的export方法导出动画数据传递给组建的animation属性。

```js
let animation = uni.createAnimation({
    transformOrigin: '50% 50%',
    duration: 1000,
    timingFunciton: 'ease',
    delay: 0
})
```

具体详见： [https://uniapp.dcloud.io/api/ui/animation?id=createanimation](https://uniapp.dcloud.io/api/ui/animation?id=createanimation)

#### 滚动

uni.pageScrollTo(OBJECT)

将页面滚动到目标位置。

```js
uni.pageScrollTo({
    csrollTop: 0,
    duration: 300
})
```

具体详见： [https://uniapp.dcloud.io/api/ui/scroll?id=pagescrollto](https://uniapp.dcloud.io/api/ui/scroll?id=pagescrollto)

#### 窗口

- uni.onWindowResize(CALLBACK)

  监听窗口尺寸变化事件

  ```js
  uni.onWindowResize(res => {
      console.log('变化后的窗口宽度=' + res.size.windowWidth)
      console.log('变化后的窗口高度=' + res.size.windowHeight)
  })
  ```

- uni.offWindowResize(CALLBACK)

  取消监听窗口尺寸变化事件

  ```js
  uni.offWindowResize(() => {
      console.log('取消监听窗口尺寸变化事件')
  })
  ```

具体详见： [https://uniapp.dcloud.io/api/ui/window?id=onwindowresize](https://uniapp.dcloud.io/api/ui/window?id=onwindowresize)

#### 字体

uni.loadFontFace(OBJECT object)

动态加载网络字体，文件地址需为下载类型。

```js
uni.loadFontFace({
    family: 'Bitstream Vera Serif Bold',
    source: 'url("https://sungd.github.io/Pacifico.ttf")',
    success() {
        console.log('success')
    }
})
```

#### 下拉刷新