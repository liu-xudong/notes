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

具体详见： [https://uniapp.dcloud.io/api/ui/font?id=loadfontface](https://uniapp.dcloud.io/api/ui/font?id=loadfontface)

#### 下拉刷新

- onPullDownRefresh

  在 js 中定义 onPullDownRefresh 处理函数（和onLoad等生命周期函数同级），监听该页面用户下拉刷新事件。

- uni.startPullDownRefresh

  开始下拉刷新，调用后出发下拉刷新动画，效果与用户手动下拉刷新一致。

- uni.stopPullDownRefresh

  停止当前页面下拉刷新。

示例：

pages.json

```json
{
    "pages": [
        {
            "path": "pages/index/index",
            "style": {
                "navigationBarTitleText": "uni-app",
                "enablePullDownRefresh": true
            }
        }
    ],
    "globalStyle": {
        "navigationBarTextStyle": "white",
        "navigationBarBackgroundColor": "#0faeff",
        "backgroundColor": "#fbf9fe"
    }
}
```

index.vue

```js
// 仅做示例，实际开发中延时根据需求来使用
export default {
	data() {
		return {
			text: 'uni-app'
		}
	},
    onLoad: function(option) {
        setTimeout(function() {
            console.log('start pulldown')
        },1000)
        uni.startPullDownRefresh()
    },
    onPullDownRefresh() {
        console.log('refresh')
        setTimeout(function() {
            uni.stopPullDownRefresh()
        },1000)
    }
}
```

具体详见： [https://uniapp.dcloud.io/api/ui/pulldown?id=onpulldownrefresh](https://uniapp.dcloud.io/api/ui/pulldown?id=onpulldownrefresh)

#### 节点信息

- uni.createSelectorQuery 

  返回一个 `SelectorQuery` 对象实例。可以在这个实例上使用 `select` 等方法选择节点，并使用 `boundingClientRect` 等方法选择需要查询的信息。

- SelectorQuery

  查询节点信息的对象

  + selectorQuery.in(component)

    将选择器的选取范围更改为自定义组件 `component` 内，返回一个 `SelectorQuery` 对象实例。（初始时，选择器仅选取页面范围的节点，不会选取任何自定义组件中的节点）。

    ```js
    const query = uni.createSelectorQuery().in(this)
    query.select('#id').boundingClientRect(data => {
        console.log("得到布局位置信息" + JSON.stringify(data))
        console.log("节点离页面顶部的距离为" + data.top)
    }).exec()
    ```

  + selectorQuery.select(selector)

    在当前页面下选择第一个匹配选择器 `selector` 的节点，返回一个 `NodesRef` 对象实例，可以用于获取节点信息。

  + selectorQuery.selectAll(selector)

    在当前页面下选择匹配选择器 `selector` 的所有节点，返回一个 `NodesRef` 对象实例，可以用于获取节点信息。

  + selectorQuery.selectViewport()

    选择显示区域，可用与获取显示区域的尺寸、滚动位置等信息，返回一个 `NodesRef` 对象实例。

  + selectorQuery.exec(callback)

    执行所有的请求。请求结果按请求次序构成数组，在 callback 的第一个参数中返回。

- NodesRef

  用于获取节点信息的对象

  + nodesRef.fields(object,callback)

    获取节点的相关信息。第一个参数是节点相关信息配置(必选)；第二参数是方法的回调函数，参数是指定的相关节点信息。

  + nodesRef.boundingClientRect(callback)

    添加节点的布局位置的查询请求。相对于显示区域，以像素为单位。其功能类似于 DOM 的 `getBoundingClientRect` 。返回 `NodeRef` 对应的 `SelectorQuery`。

  + nodesRef.scrollOffset(callback)

    添加节点的滚动位置查询请求。以像素为单位。节点必须是 `scroll-view` 或者 `viewport` 。返回 `NodesRef` 对应的 `SelectorQuery` 。

  + nodesRef.context(callback)

    添加节点的 Context 对象查询请求。支持 `VideoContext` 、 `CanvasContext` 和 `MapContext` 等的获取。

  + nodesRef.node(callback)

    获取 `Node` 节点实例。目前支持 `Canvas` 的获取。

具体详见：[https://uniapp.dcloud.io/api/ui/nodes-info?id=createselectorquery](https://uniapp.dcloud.io/api/ui/nodes-info?id=createselectorquery)

#### 节点布局相交状态

- uni.createIntersectionObserver 

  创建并返回一个 `IntersectionObserver` 对象实例。

- IntersectionObserver 对象的方法列表

  | 方法                                                | 说明                                                         |
  | --------------------------------------------------- | ------------------------------------------------------------ |
  | IntersectionObserver.relativeTo(selector,[margins]) | 使用选择器指定一个节点，作为参照区域之一                     |
  | IntersectionObserver.relativeToViewport([margins])  | 指定页面显示区域作为参照区域之一                             |
  | IntersectionObserver.observe(selector,[callback])   | 指定目标节点并开始监听相交状态变化情况。回调函数 `callback` 包含一个参数 `result` |
  | IntersectionObserver.disconnect()                   | 停止监听。回调函数将不再触发                                 |

```js
uni.createIntersectionObserver(this).relativeTo('.scroll',{bottom: 100}).observe('.test',(res) => {
    console.log(res)
})
```

具体详见： [https://uniapp.dcloud.io/api/ui/intersection-observer?id=createintersectionobserver](https://uniapp.dcloud.io/api/ui/intersection-observer?id=createintersectionobserver)

#### 媒体查询

- uni.createMediaQueryObserver([this])

  创建并返回一个 `MediaQueryObserver` 对象实例。

- MediaWueryObserver 对象的方法列表

  | 方法                                                         | 说明                              |
  | ------------------------------------------------------------ | --------------------------------- |
  | MediaQueryObserver.observe(Object descriptor,function callback) | 开始监听页面 media query 变化情况 |
  | MediaQueryObserver.disconnect()                              | 停止监听，回调函数将不再触发      |

具体详见： [https://uniapp.dcloud.io/api/ui/media-query-observer?id=createmediaqueryobserver](https://uniapp.dcloud.io/api/ui/media-query-observer?id=createmediaqueryobserver)

#### 自定义组件

nextTick(function callback)

在小程序自定义组件，如wxcomponents中使用。延迟一部分操作到下一个时间片再执行。（类似于 setTimeout） 。其他平台无此概念。

- 微信小程序：[规范详情](https://developers.weixin.qq.com/miniprogram/dev/api/wx.nextTick.html)
- 百度小程序：[规范详情](https://smartprogram.baidu.com/docs/develop/api/custom_component/#swan-nextTick/)
- QQ小程序：[规范详情](https://q.qq.com/wiki/develop/miniprogram/API/interface/interface_nexttick.html#qq-nexttick)

#### 菜单

getMenuButtonBoundingClientRect()

在小程序平台，如果原生导航栏被隐藏，仍然在右上角会有一个悬浮按钮，微信下也被成为胶囊按钮。本 API 用于获取小程序下菜单按钮的布局位置信息，方便开发者布局顶部内容时避开该按钮。

```js
let menuButtonInfo = uni.getMenuButtonBoundingClientRect()
```

### 页面和窗口

#### 页面

- getCurrentPages()

  `getCurrentPages()` 函数用于获取当前页面栈的实例，以数组形式按栈的顺序给出，第一个元素为首页，最后一个元素为当前页面。

- $getAppWebview()

  `uni-app` 在 `getCurrentPages()` 获得的页面里内置一个方法 `$getAppWebview()` 可以得到当前 webview 的对象实例，从而实现对 webview 更强大的控制。

具体详见： [https://uniapp.dcloud.io/api/window/window?id=getcurrentpages](https://uniapp.dcloud.io/api/window/window?id=getcurrentpages)

#### 页面通讯

- uni.$emit(eventName,OBJECT)

  触发全局的自定义事件，附加参数都会传给监听器回调函数

  ```js
  uni.$emit('update',{msg: '页面更新'})
  ```

- uni.$on(eventName,callback)

  监听全局的自定义事件，事件由 `uni.$emit` 触发，回调函数会接收事件触发函数的传入参数。

  ```js
  uni.$on('update',function(data) {
      console.log('监听到事件来自 update, 携带参数 msg 为：' + data.msg)
  })
  ```

- uni.$once(eventName,callback)

  监听全局的自定义事件，事件由 `uni.$emit` 触发，但仅触发一次，在第一次触发之后移除该监听器。

  ```js
  uni.$once('update',function(data) {
      console.log('监听到事件来自 update,携带参数 msg 为：' + data.msg)
  })
  ```

- uni.$off([eventName,callback])

  移除全局自定义事件监听器。

具体详见： [https://uniapp.dcloud.io/api/window/communication?id=emit](https://uniapp.dcloud.io/api/window/communication?id=emit)

#### subNVue原生子窗体

- uni.getSubNVueById(subNvueId)

  通过 `ID` 获取 `subNVues` 原生子窗体的实例。

  ```js
  const subNVue = uni.getSubNVueById('popup')
  ```

- uni.getCurrentSubNVue()

  在一个 subnvue 窗体的 nvue 页面代码中，获取当前 `subNVues` 原生子窗体的实例。

  ```js
  const subNVue = uni.getCurrentSubNVue()
  ```

- subNVue.show(aniShow,duration,showedCB)

  显示原生子窗体。

  ```js
  subNVue.show('slide-in-left',200,()=>{
      console.log('subNVue 原生子窗体显示成功')
  })
  ```

- subNVue.hide(aniShow,duration)

  隐藏原生子窗体。

  ```js
  subNVue.hide('slide-out-left',200)
  ```

- subNVue.setStyle(style)

  设置原生子窗体的样式。

  ```js
  subNVue.setStyle({
      "position": "absolute",	//除 popup 外，其他值域参考 5+ webview position 文档
      "width": "50%",
      "height": "50%",
      "left": "20px",
      "top": "100px"
  })
  ```

- 动画类型

  显示动画与关闭动画，会有默认的对应规则。但是如果通过 API 原生子窗体的关闭动画类型，则不会使用默认的类型。

具体详见： [https://uniapp.dcloud.io/api/window/subNVues](https://uniapp.dcloud.io/api/window/subNVues)

### 文件

#### uni.saveFile(OBJECT)

保存文件到本地

```js
uni.chooseImage({
    success: function(res) {
        let tempFilePaths = res.tempFilePaths
        uni.saveFile({
            tempFilePath: tempFilePaths[0],
            success: function(res) {
                let savedFilePath = res.savedFilePath
            }
        })
    }
})
```

#### uni.getSaveFileList(OBJECT)

获取本地已保存的文件列表

```js
uni.getSavedFileList({
    success: function(res) {
        console.log(res.fileList)
    }
})
```

#### uni.getSavedFileInfo(OBJECT)

获取本地文件的文件信息。此接口只用于获取已保存到本地的文件。

```js
uni.getSavedFileInfo({
    filePath: 'unifile://somefile',	// 仅做示例用，非真正的文件路径
    success: function(res) {
        console.log(res.size)
        console.log(res.createTime)
    }
})
```

#### uni.removeSavedFile(OBJECT)

删除本地存储的文件。

```js
uni.getSaveFileList({
    success: function(res) {
        if(res.fileList.length > 0) {
            uni.removeSavedFile({
                filePath: res.fileList[0].filePath,
                complete: function(res) {
                    console.log(res)
                }
            })
        }
    }
})
```

#### uni.getFileInfo(OBJECT)

获取文件信息

#### uni.openDocument(OBJECT)

新开页面打开文档，支持格式： doc, xls, ppt, pdf, docx, xlsx, pptx。

```js
uni.downloadFile({
    url: 'https://example.com/somefile.pdf',
    success: function(res) {
        let filePath = res.tempFilePath
        uni.openDocument({
            filePath: filePath,
            success: function(res) {
                console.log('打开文档成功')
            }
        })
    }
})
```

#### uni.getFileSystemManager()

获取全局唯一的文件管理器

- 微信小程序平台，[规范详情](https://developers.weixin.qq.com/miniprogram/dev/api/wx.getFileSystemManager.html)
- 字节跳动小程序平台，[规范详情](https://developer.toutiao.com/dev/cn/mini-app/develop/api/file/getfilesystemmanager)
- QQ小程序平台，[规范详情](https://q.qq.com/wiki/develop/miniprogram/API/file/qq.getFileSystemManager.html)

具体详见： [https://uniapp.dcloud.io/api/file/file?id=savefile](https://uniapp.dcloud.io/api/file/file?id=savefile)

### 绘画

#### uni.createOffscreenCanvas()

创建离屏 canvas 实例

仅微信小程序平台支持，[规范详情](https://developers.weixin.qq.com/miniprogram/dev/api/wx.createOffscreenCanvas.html)

#### uni.createCanvasContext(canvasId,this)

具体详见： [https://uniapp.dcloud.io/api/canvas/createCanvasContext](https://uniapp.dcloud.io/api/canvas/createCanvasContext)

#### uni.canvasToTempFilePath(object,component)

把当前画布指定区域的内容导出生成指定大小的图片，并返回文件路径。在自定义组件下，第二个参数传入自定义组件实例，以操作组件内 `<canvas>` 组件。

```js
uni.canvasToTempFilePath({
    x: 100,
    y: 200,
    width: 50,
    height: 50,
    destWidth: 100,
    destHeight: 100,
    canvasId: 'myCanvas',
    success: function(res) {
        // 在 H5 平台下， tempFilePath 为 base64
        console.log(res.tempFilePath)
    }
})
```

具体详见： [https://uniapp.dcloud.io/api/canvas/canvasToTempFilePath](https://uniapp.dcloud.io/api/canvas/canvasToTempFilePath)

#### uni.canvasPutImageData(OBJECT,this)

将像素数据绘制到画布的方法，在自定义组件下，第二个参数传入自定义组件实例 this，以操作组件内的 `<canvas>` 组件。

```js
const data = new Uint8ClampedArray([255,0,0,255])
uni.canvasPutImageData({
    canvasId: 'myCanvas',
    x: 0,
    y: 0,
    width: 1,
    data: data,
    success(res) {}
})
```

具体详见： [https://uniapp.dcloud.io/api/canvas/canvasPutImageData](https://uniapp.dcloud.io/api/canvas/canvasPutImageData)

#### uni.canvasGetImageData(OBJECT,this)

返回一个数组，用来描述 canvas 区域隐含的像素数据，在自定义组件下，第二个参数传入自定义组件实例 this， 以操作组件内 `<canvas>` 组件。

```js
uni.canvasGetImageData({
    canvasId: 'myCanvas',
    x: 0,
    y: 0,
    width: 100,
    height: 100,
    success(res) {
        console.log(res.width)	// 100
        console.log(res.height)	// 100
        console.log(res.data instanceof Uint8ClampedArray)	// true
        console.log(res.data.length)	// 100 * 100 * 4
    }
})
```

具体详见： [https://uniapp.dcloud.io/api/canvas/canvasGetImageData](https://uniapp.dcloud.io/api/canvas/canvasGetImageData)

#### CanvasContext

- CanvasContext.fillStyle string

  填充颜色

- CanvasContext.strokeStyle string

  边框颜色

- CanvasContext.shadowOffsetX number

  阴影相对于形状在水平方向的偏移

- CanvasContext.shadowOffsetY number

  阴影相对于形状在竖直方向的偏移

- CanvasContext.shadowColor number

  阴影的颜色

- CanvasContext.shadowBlur number

  阴影的的模糊级别

- CanvasContext.lineWidth number

  线条的宽度

- CanvasContext.lineCap number

  线条的端点样式

- CanvasContext.lineJoin number

  线条的交点样式

- CanvasContext.miterLimit number

  最大斜接长度

- CanvasContext.lineDashOffset number

  虚线偏移量，初始值为0

- CanvasContext.font string

  当前字体样式的属性，符合 [CSS font 语法](https://developer.mozilla.org/zh-CN/docs/Web/CSS/font) 的 DOMString 字符串，至少需要提供字体大小和字体族名。默认值为 10px sans-serif。

- CanvasContext.globalAlpha number

  全局画笔透明度。范围 0 - 1 ，0 表示完全透明，1 表示完全不透明

- CanvasContext.globalCompositeOperation string

  在绘制新形状时应用的合成操作的类型。目前安卓版本只适用于 `fill` 填充块的合成，用于 `stroke` 线段的合成效果都是 `source-over`

- CanvasContext.arc

  画一条弧线。创建一个圆可以用 `arc()` 方法指定起始弧度为 0，终止弧度为 `2 * Math.PI` 。用 `stroke()` 或者 `fill()` 方法来在 `canvas` 中画弧线。	

  ```js
  const ctx = uni.createCanvasContext('myCanvas')
  
  // Draw coordinates
  ctx.arc(100,75,50,0,2 * Math.PI)
  ctx.setFillStyle('#EEEEEE')
  ctx.fill()
  
  ctx.beginPath()
  ctx.moveTo(40, 75)
  ctx.lineTo(160, 75)
  ctx.moveTo(100, 15)
  ctx.lineTo(100, 135)
  ctx.setStrokeStyle('#AAAAAA')
  ctx.stroke()
  
  ctx.setFontSize(12)
  ctx.setFillStyle('black')
  ctx.fillText('0', 165, 78)
  ctx.fillText('0.5 * PI', 15, 78)
  ctx.fillText('1.5 * PI', 83, 10)
  
  // Draw points
  ctx.beginPath()
  ctx.arc(100, 75, 2, 0, 2 * Math.PI)
  ctx.setFillStyle('lightgreen')
  ctx.fill()
  
  ctx.beginPath()
  ctx.arc(100, 25, 2, 0, 2 * Math.PI)
  ctx.setFillStyle('blue')
  ctx.fill()
  
  ctx.beginPath()
  ctx.arc(150, 75, 2, 0, 2 * Math.PI)
  ctx.setFillStyle('red')
  ctx.fill()
  
  // Draw arc
  ctx.beginPath()
  ctx.arc(100, 75, 50, 0, 1.5 * Math.PI)
  ctx.setStrokeStyle('#333333')
  ctx.stroke()
  
  ctx.draw()
  ```

- CanvasContext.arcTo

  根据控制点和半径绘制圆弧路径。

  ```js
  CanvasContext.arcTo(x1, y1, x2, y2, radius)
  ```

- CanvasContext.beginPath

  开始创建一个路径，需要调用 fill 或者 stroke 才会适用路径进行填充或描边

  ```js
  const ctx = uni.createCanvasContext('myCanvas')
  // begin path
  ctx.rect(10, 10, 100, 30)
  ctx.setFillStyle('yellow')
  ctx.fill()
  
  // begin another path
  ctx.beginPath()
  ctx.rect(10, 40, 100, 30)
  
  // only fill this rect, not in current path
  ctx.setFillStyle('blue')
  ctx.fillRect(10, 70, 100, 30)
  
  ctx.rect(10, 100, 100, 30)
  
  // it will fill current path
  ctx.setFillStyle('red')
  ctx.fill()
  ctx.draw()
  ```

- CanvasContext.bezierCurveTo

  创建三次方贝塞尔曲线路径

  ```js
  const ctx = uni.createCanvasContext('myCanvas')
  
  // Draw points
  ctx.beginPath()
  ctx.arc(20, 20, 2, 0, 2 * Math.PI)
  ctx.setFillStyle('red')
  ctx.fill()
  
  ctx.beginPath()
  ctx.arc(200, 20, 2, 0, 2 * Math.PI)
  ctx.setFillStyle('lightgreen')
  ctx.fill()
  
  ctx.beginPath()
  ctx.arc(20, 100, 2, 0, 2 * Math.PI)
  ctx.arc(200, 100, 2, 0, 2 * Math.PI)
  ctx.setFillStyle('blue')
  ctx.fill()
  
  ctx.setFillStyle('black')
  ctx.setFontSize(12)
  
  // Draw guides
  ctx.beginPath()
  ctx.moveTo(20, 20)
  ctx.lineTo(20, 100)
  ctx.lineTo(150, 75)
  
  ctx.moveTo(200, 20)
  ctx.lineTo(200, 100)
  ctx.lineTo(70, 75)
  ctx.setStrokeStyle('#AAAAAA')
  ctx.stroke()
  
  // Draw quadratic curve
  ctx.beginPath()
  ctx.moveTo(20, 20)
  ctx.bezierCurveTo(20, 100, 200, 100, 200, 20)
  ctx.setStrokeStyle('black')
  ctx.stroke()
  
  ctx.draw()
  ```

- CanvasContext.clearRect

  清除画布上在该矩形区域内的内容

  ```html
  <canvas canvas-id="myCanvas" id="myCanvas" style="border: 1px solid; background: #123456;"/>
  ```

  ```js
  const ctx = uni.createCanvasContext('myCanvas')
  ctx.setFillStyle('red')
  ctx.fillRect(0, 0, 150, 200)
  ctx.setFillStyle('blue')
  ctx.fillRect(150, 0, 150, 200)
  ctx.clearRect(10, 10, 150, 75)
  ctx.draw()
  ```

- CanvasContext.clip

  从原始画布中剪切任意形状和尺寸。一旦剪切了某个区域，则所有之后的绘图都会被限制在被剪切的区域内(不能访问画布上的其他区域)。可以在使用 clip() 方法前通过使用 save() 方法对当前画布区域进行保存，并在以后的任意时间对其进行恢复(通过 restore()方法)

  ```js
  const context = uni.createCanvasContext('myCanvas')
  
  uni.downloadFile({
      url: 'https://img-cdn-qiniu.dcloud.net.cn/uniapp/images/uni@2x.png',
      success: function(res) {
          context.save()
          context.beginPath()
          context.arc(96, 96, 48, 0, 2 * Math.PI)
          context.clip()
          context.drawImage(res.tempFilePath, 48, 48)
          context.restore()
          context.draw()
      }
  })
  ```

- CanvasContext.closePath

  关闭一个路径

- CanvasContext.createCircularGradient

  创建一个从圆心开始的渐变，返回的 CanvasGradient 对象，需要使用 `CanvasGradient.addColorStop()` 来指定渐变点，至少要两个

  ```js
  const ctx = uni.createCanvasContext('myCanvas')
  
  // Create circular gradient
  const grd = ctx.createCircularGradient(75, 50, 50)
  grd.addColorStop(0, 'red')
  grd.addColorStop(1, 'white')
  
  // Fill with gradient
  ctx.setFillStyle(grd)
  ctx.fillRect(10, 10, 150, 80)
  ctx.draw()
  ```

- CanvasContext.createLinearGradient

  创建一个线性的渐变颜色。返回的 CanvasGradient 对象，需要使用 `CanvasGradient.addColorStop()` 来指定渐变点，至少要两个。

  ```js
  const ctx = uni.createCanvasContext('myCanvas')
  
  // Create linear gradient
  const grd = ctx.createLinearGradient(0, 0, 200, 0)
  grd.addColorStop(0, 'red')
  grd.addColorStop(1, 'white')
  
  // Fill with gradient
  ctx.setFillStyle(grd)
  ctx.fillRect(10, 10, 150, 80)
  ctx.draw()
  ```

- CanvasContext.createPattern

  对指定的图像创建模式的方法，可在指定的方向上重复元图像

  ```js
  const ctx = uni.createCanvasContext('myCanvas')
  const pattern = ctx.createPattern('/path/to/image','repeat-x')
  ctx.fillStyle = pattern
  ctx.fillRect(0, 0, 300, 150)
  ctx.draw()
  ```

- CanvasContext.draw

  将之前在绘图上下文中的描述（路径、变形、样式）画到 canvas 中

  ```js
  const ctx = uni.createCanvasContext('myCanvas')
  
  ctx.setFillStyle('red')
  ctx.fillRect(10, 10, 150, 100)
  ctx.draw()
  ctx.fillRect(50, 50, 150, 100)
  ctx.draw()
  ```

  ```js
  const ctx = uni.createCanvasContext('myCanvas')
  
  ctx.setFillStyle('red')
  ctx.fillRect(10, 10, 150, 100)
  ctx.draw()
  ctx.fillRect(50, 50, 150, 100)
  ctx.draw(true)
  ```

- CanvasContext.drawImage

  绘制图像到画布

  ```js
  const = uni.createCanvasContext('myCanvas')
  
  uni.chooseImage({
      success: function(res) {
          ctx.drawImage(res.tempFilePaths[0], 0, 0, 150, 100)
          ctx.draw()
      }
  })
  ```

- CanvasContext.fill

  对当前路径中的内容进行填充。默认的填充色为黑色。

  ```js
  const ctx = uni.createCanvasContext('myCanvas')
  ctx.moveTo(10, 10)
  ctx.lineTo(100, 10)
  ctx.lineTo(100, 100)
  ctx.fill()
  ctx.draw()
  ```

  ```js
  const ctx = uni.createCanvasContext('myCanvas')
  // begin path
  ctx.rect(10, 10, 100, 30)
  ctx.setFillStyle('yellow')
  ctx.fill()
  
  // begin another path
  ctx.beginPath()
  ctx.rect(10, 40, 100, 30)
  
  // only fill this rect, not in current path
  ctx.setFillStyle('blue')
  ctx.fillRect(10, 70, 100, 30)
  
  // it will fill current path
  ctx.setFillStyle('red')
  ctx.fill()
  ctx.draw()
  ```

- CanvasContext.fillRect

  填充一个矩形

  ```js
  const ctx = uni.createCanvasContext('myCanvas')
  ctx.setFillStyle('red')
  ctx.fillRect(10, 10, 150, 75)
  ctx.draw()
  ```

- CanvasContext.fillText

  在画布上绘制被填充的文本

  ```js
  const ctx = uni.createCanvasContext('myCanvas')
  
  ctx.setFontSize(20)
  ctx.fillText('Hello', 20, 20)
  ctx.fillText('MINA', 100, 100)
  
  ctx.draw()
  ```

- CanvasContext.lineTo

  增加一个新点，然后创建一条从上次指定点到目标点的线

  ```js
  const ctx = uni.createCanvasContext('myCanvas')
  ctx.moveTo(10, 10)
  ctx.rect(10, 10, 100, 50)
  ctx.lineTo(110, 60)
  ctx.stroke()
  ctx.draw()
  ```

- CanvasContext.measureText

  测量文本尺寸信息，目前仅返回文本宽度。同步接口

  ```js
  const ctx = uni.createCanvasContext('myCanvas')
  ctx.font = 'italic bold 20px cursive'
  const metrics = ctx.measureText('Hello World')
  console.log(metrics.width)
  ```

- CanvasContext.moveTo

  把路径移动到画布中的指定点，不创建线条。用 `stroke()` 方法来画线条

  ```js
  const ctx = uni.createCanvasContext('myCanvas')
  ctx.moveTo(10, 10)
  ctx.lineTo(100, 10)
  
  ctx.moveTo(10, 50)
  ctx.lineTo(100, 50)
  ctx.stroke()
  ctx.draw()
  ```

- CanvasContext.quadraticCurveTo

  创建二次贝塞尔曲线路径。曲线的起始点为路径中前一个点

  ```js
  const ctx = uni.createCanvasContext('myCanvas')
  
  // Draw points
  ctx.beginPath()
  ctx.arc(20, 20, 2, 0, 2 * Math.PI)
  ctx.setFillStyle('red')
  ctx.fill()
  
  ctx.beginPath()
  ctx.arc(200, 20, 2, 0, 2 * Math.PI)
  ctx.setFillStyle('lightgreen')
  ctx.fill()
  
  ctx.beginPath()
  ctx.arc(20, 100, 2, 0, 2 * Math.PI)
  ctx.setFillStyle('blue')
  ctx.fill()
  
  ctx.setFillStyle('black')
  ctx.setFontSize(12)
  
  // Draw guides
  ctx.beginPath()
  ctx.moveTo(20, 20)
  ctx.lineTo(20, 100)
  ctx.lineTo(200, 20)
  ctx.setStrokeStyle('#AAAAAA')
  ctx.stroke()
  
  // Draw quadratic curve
  ctx.beginPath()
  ctx.moveTo(20, 20)
  ctx.quadraticCurveTo(20, 100, 200, 20)
  ctx.setStrokeStyle('black')
  ctx.stroke()
  
  ctx.draw()
  ```

- CanvasContext.rect

  创建一个矩形

  ```js
  const ctx = uni.createCanvasContext('myCanvas')
  ctx.rect(10, 10, 150, 75)
  ctx.setFillStyle('red')
  ctx.fill()
  ctx.draw()
  ```

- CanvasContext.restore

  恢复之前保存的绘图上下文

  ```js
  const ctx = uni.createCanvasContext('myCanvas')
  
  // save the default fill style
  ctx.sava()
  ctx.setFillStyle('red')
  ctx.fillRect(10, 10, 150, 100)
  
  // restore to the previous saved state
  ctx.restore()
  ctx.fillRect(50, 50, 150, 100)
  
  ctx.draw()
  ```

- CanvasContext.rotate

  以原点为中心，原点可以用 translate 方法修改。顺时针旋转当前坐标轴。多次调用 rotate，旋转的角度会叠加

  ```js
  const ctx = uni.createCanvasContext('myCanvas')
  
  ctx.strokeRect(100, 10, 150, 100)
  ctx.rotate(20 * Math.PI / 180)
  ctx.strokeRect(100, 10, 150, 100)
  ctx.rotate(20 * Math.PI / 180)
  ctx.strokeRect(100, 10, 150, 100)
  
  ctx.draw()
  ```

- CanvasContext.save

  保存当前的绘画上下文

  ```js
  const ctx = uni.createCanvasContext('myCanvas')
  
  // save the default fill style
  ctx.save()
  ctx.setFillStyle('red')
  ctx.fillRect(10, 10, 150, 100)
  
  // restore to the previous saved state
  ctx.restore()
  ctx.fillRect(50, 50, 150, 100)
  
  ctx.draw()
  ```

- CanvasContext.scale

  在调用 `scale` 方法后，之后创建的路径其横纵坐标会被缩放。多次调用 `scale` ，倍数会相乘

  ```js
  const ctx = uni.createCanvasContext('myCanvas')
  
  ctx.strokeRect(10, 10, 25, 15)
  ctx.scale(2, 2)
  ctx.strokeRect(10, 10, 25, 15)
  ctx.scale(2, 2)
  ctx.strokeRect(10, 10, 25, 15)
  
  ctx.draw()
  ```

- CanvasContext.setFillStyle

  设置填充色，如果没有设置 fillStyle ，默认颜色为 black

  ```js
  const ctx = uni.createCanvasContext('myCanvas')
  ctx.setFillStyle('red')
  ctx.fillRect(10, 10, 150, 75)
  ctx.draw()
  ```

- CanvasContext.setFontSize

  设置字体的字号

  ```js
  const ctx = uni.createCanvasContext('myCanvas')
  
  ctx.setFontSize(20)
  ctx.fillText('20', 20, 20)
  ctx.setFontSize(30)
  ctx.fillText('30', 40, 40)
  ctx.setFontSize(40)
  ctx.fillText('40', 60, 60)
  ctx.setFontSize(50)
  ctx.fillText('50', 90, 90)
  
  ctx.draw()
  ```

- CanvasContext.setGlobalAlpha

  设置全局画笔透明度

  ```js
  const ctx = uni.createCanvasContext('myCanvas')
  
  ctx.setFillStyle('red')
  ctx.fillRect(10, 10, 150, 100)
  ctx.setGlobalAlpha(0.2)
  ctx.setFillStyle('blue')
  ctx.fillRect(50, 50, 150, 100)
  ctx.setFillStyle('yellow')
  ctx.fillRect(100, 100, 150, 100)
  
  ctx.draw()
  ```

- CanvasContext.setLineCap

  设置线条的端点样式

  ```js
  const ctx = uni.createCanvasContext('myCanvas')
  ctx.beginPath()
  ctx.moveTo(10, 10)
  ctx.lineTo(150, 10)
  ctx.stroke()
  
  ctx.beginPath()
  ctx.setLineCap('butt')
  ctx.setLineWidth(10)
  ctx.moveTo(10, 30)
  ctx.lineTo(150, 30)
  ctx.stroke()
  
  ctx.beginPath()
  ctx.setLineCap('round')
  ctx.setLineWidth(10)
  ctx.moveTo(10, 50)
  ctx.LineTo(150, 50)
  ctx.stroke()
  
  ctx.beginPath()
  ctx.setLineCap('square')
  ctx.setLineWidth(10)
  ctx.moveTo(10, 70)
  ctx.lineTo(150, 70)
  ctx.stroke()
  
  ctx.draw()
  ```

- CanvasContext.setLineDash

  设置线条宽度

  ```js
  const ctx = uni.createCanvasContext('myCanvas')
  
  ctx.setLineDash([10, 20], 5)
  
  ctx.beginPath()
  ctx.moveTo(0, 100)
  ctx.lineTo(400, 100)
  ctx.stroke()
  
  ctx.draw()
  ```

- CanvasContext.setLineJoin

  设置线条的交点样式

  ```js
  const ctx = uni.createCanvasContext('myCanvas')
  ctx.beginPath()
  ctx.moveTo(10, 10)
  ctx.lineTo(100, 50)
  ctx.lineTo(10, 50)
  ctx.stroke()
  
  ctx.beginPath()
  ctx.setLineJoin('bevel')
  ctx.setLineWidth(10)
  ctx.moveTo(50, 10)
  ctx.lineTo(140, 50)
  ctx.lineTo(50, 90)
  ctx.stroke()
  
  ctx.beginPath()
  ctx.setLineJoin('round')
  ctx.setLineWidth(10)
  ctx.moveTo(90, 10)
  ctx.lineTo(180, 50)
  ctx.lineTo(90, 90)
  ctx.stroke()
  
  ctx.beginPath()
  ctx.setLineJoin('miter')
  ctx.setLineWidth(10)
  ctx.moveTo(130, 10)
  ctx.lineTo(220, 50)
  ctx.lineTo(130, 90)
  ctx.stroke()
  
  ctx.draw()
  ```

- CanvasContext.setLineWidth

  设置线条的宽度

  ```js
  const ctx = uni.createCanvasContext('myCanvas')
  ctx.beginPath()
  ctx.moveTo(10, 10)
  ctx.lineTo(150, 10)
  ctx.stroke()
  
  ctx.beginPath()
  ctx.setLineWidth(5)
  ctx.moveTo(10, 30)
  ctx.lineTo(150, 30)
  ctx.stroke()
  
  ctx.beginPath()
  ctx.setLineWidth(10)
  ctx.moveTo(10, 50)
  ctx.lineTo(150, 50)
  ctx.stroke()
  
  ctx.beginPath()
  ctx.setLineWidth(15)
  ctx.moveTo(10, 70)
  ctx.lineTo(150, 70)
  ctx.stroke()
  
  ctx.draw()
  ```

- CanvasContext.setMiterLimit

  设置最大斜接长度，斜接长度指的是在两条线交汇处内角和外角之间的距离。当 `setLineJoin()` 为 miter 时才有效。超过最大倾斜长度的，连接处将以 lineJoin 为 bevel 来显示。

  ```js
  const ctx = uni.createCanvasContext('myCanvas')
  ctx.beginPath()
  ctx.setLineWidth(10)
  ctx.setLineJoin('miter')
  ctx.setMiterLimit(1)
  ctx.moveTo(10, 10)
  ctx.lineTo(100, 50)
  ctx.lineTo(10, 90)
  ctx.stroke()
  
  ctx.beginPath()
  ctx.setLineWidth(10)
  ctx.setLineJoin('miter')
  ctx.setMiterLimit(2)
  ctx.moveTo(50, 10)
  ctx.lineTo(140, 50)
  ctx.lineTo(50, 90)
  ctx.stroke()
  
  ctx.beginPath()
  ctx.setLineWidth(10)
  ctx.setLineJoin('miter')
  ctx.setMiterLimit(3)
  ctx.moveTo(90, 10)
  ctx.lineTo(180, 50)
  ctx.lineTo(90, 90)
  ctx.stroke()
  
  ctx.beginPath()
  ctx.setLineWidth(10)
  ctx.setLineJoin('miter')
  ctx.setMiterLimit(4)
  ctx.moveTo(130, 10)
  ctx.lineTo(220, 50)
  ctx.lineTo(130, 90)
  ctx.stroke()
  
  ctx.draw()
  ```

- CanvasContext.setShadow

  设置阴影样式。如果没有设置，offsetX 默认值为0，offsetY 默认值为0，blur 默认值为0，color 默认值为 black

  ```js
  const ctx = uni.createCanvasContext('myCanvas')
  ctx.setFillStyle('red')
  ctx.setShadow(10, 50, 50, 'blue')
  ctx.fillRect(10, 10, 150, 75)
  ctx.draw()
  ```

- CanvasContext.setStrokeStyle

  设置边框颜色。如果没有设置 fillStyle ，默认颜色为 black

  ```js
  const ctx = uni.createCanvasContext('myCanvas')
  ctx.setStrokeStyle('red')
  ctx.strokeRect(10, 10, 150, 75)
  ctx.draw()
  ```

- CanvasContext.setTextAlign

  用于设置文字的对齐

  ```js
  const ctx = uni.createCanvasContext()
  
  ctx.setStrokeStyle('red')
  ctx.moveTo(150, 20)
  ctx.lineTo(150, 170)
  ctx.stroke()
  
  ctx.setFontSize(15)
  ctx.setTextAlign('left')
  ctx.fillText('textAlign=left', 150, 60)
  
  ctx.setTextAlign('center')
  ctx.fillText('textAlign=center', 150, 80)
  
  ctx.setTextAlign('right')
  ctx.fillText('textAlign=right', 150, 100)
  
  ctx.draw()
  ```

- CanvasContext.setTextBaseline

  用于设置文字的水平对齐

  ```js
  const ctx = uni.createCanvasContext('myCanvas')
  
  ctx.setStrokeStyle('red')
  ctx.moveTo(5, 75)
  ctx.lineTo(295, 75)
  ctx.stroke()
  
  ctx.setFontSize(20)
  
  ctx.setTextBaseline('top')
  ctx.fillText('top', 5, 75)
  
  ctx.setTextBaseline('middle')
  ctx.fillText('middle', 50, 75)
  
  ctx.setTextBaseline('bottom')
  ctx.fillText('bottom', 120, 75)
  
  ctx.setTextBaseline('normal')
  ctx.fillText('normal', 200, 75)
  
  ctx.draw()
  ```

- CanvasContext.setTransform

  使用矩阵重新设置（覆盖）当前变换的方法

- CanvasContext.stroke

  画出当前路径的边框。默认颜色为黑色

  ```js
  const ctx = uni.createCanvasContext()
  ctx.moveTo(10, 10)
  ctx.lineTo(100, 10)
  ctx.lineTo(100, 100)
  ctx.stroke()
  ctx.draw()
  ```

  ```js
  const ctx = uni.createCanvasContext('myCanvas')
  // begin path
  ctx.rect(10, 10, 100, 30)
  ctx.setStrokeStyle('yellow')
  ctx.stroke()
  
  // begin another path
  ctx.beginPath()
  ctx.rect(10, 40, 100, 30)
  
  // only stoke this rect, not in current path
  ctx.setStrokeStyle('blue')
  ctx.strokeRect(10, 70, 100, 30)
  
  ctx.rect(10, 100, 100, 30)
  
  // it will stroke current path
  ctx.setStrokeStyle('red')
  ctx.stroke()
  ctx.draw()
  ```

- CanvasContext.strokeRect

  画一个矩形（非填充）。用 `setFillStroke()` 设置边框颜色，如果没有设置默认是黑色

  ```js
  const ctx = uni.createCanvasContext('myCanvas')
  ctx.setStrokeStyle('red')
  ctx.strokeRect(10, 10, 150, 75)
  ctx.draw()
  ```

- CanvasContext.strokeText

  给定的（x,y）位置绘制文本描边的方法

- CanvasContext.transform

  使用矩阵多次叠加当前变幻的方法

- CanvasContext.translate

  对当前坐标系的原点 (0,0) 进行变换，磨人的坐标系原点为页面左上角

  ```js
  const ctx = uni.createCanvasContext('muCanvas')
  
  ctx.strokeRect(10, 10, 150, 100)
  ctx.translate(20, 20)
  ctx.strokeRect(10, 10, 150, 100)
  ctx.translate(20, 20)
  ctx.strokeRect(10, 10, 150, 100)
  
  ctx.draw()
  ```

具体详见： [https://uniapp.dcloud.io/api/canvas/CanvasContext](https://uniapp.dcloud.io/api/canvas/CanvasContext)

#### CanvasGradient

CanvasGradient.addColorStop(stop,color)

创建一个颜色的渐变点

```js
const ctx = uni.createCanvasContext('myCanvas')

// Create circular gradient
const grd = ctx.createLinearGradient(30, 10, 120, 10)
grd.addColorStop(0, 'red')
grd.addColorStop(0.16, 'orange')
grd.addColorStop(0.33, 'yellow')
grd.addColorStop(0.5, 'green')
grd.addColorStop(0.66, 'cyan')
grd.addColorStop(0.83, 'blue')
grd.addColorStop(1, 'purple')

// Fill with gradient
ctx.setFillStyle(grd)
ctx.fillRect(10, 10, 150, 80)
ctx.draw()
```

具体详见： [https://uniapp.dcloud.io/api/canvas/CanvasGradient?id=canvasgradientaddcolorstop](https://uniapp.dcloud.io/api/canvas/CanvasGradient?id=canvasgradientaddcolorstop)

### 广告

#### 激励视频广告

具体详见： [https://uniapp.dcloud.io/api/a-d/rewarded-video](https://uniapp.dcloud.io/api/a-d/rewarded-video)

#### 全屏视频广告

具体详见： [https://uniapp.dcloud.io/api/a-d/full-screen-video?id=%e5%85%a8%e5%b1%8f%e8%a7%86%e9%a2%91%e5%b9%bf%e5%91%8a](https://uniapp.dcloud.io/api/a-d/full-screen-video?id=%e5%85%a8%e5%b1%8f%e8%a7%86%e9%a2%91%e5%b9%bf%e5%91%8a)

#### 插屏广告

具体详见： [https://uniapp.dcloud.io/api/a-d/interstitial](https://uniapp.dcloud.io/api/a-d/interstitial)

### 第三方服务

#### 获取服务供应商

uni.getProvider(OBJECT)

获取服务供应商

```js
uni.getProvider({
    service: 'oauth',
    success: function(res) {
        console.log(res.provider)
        if(res.provider.indexOf('qq')) {
            uni.login({
                provider: 'qq',
               	success: function(loginRes) {
                    console.log(JSON.stringify(loginRes))
                }
            })
        }
    }
})
```

具体详见： [https://uniapp.dcloud.io/api/plugins/provider](https://uniapp.dcloud.io/api/plugins/provider)

#### 登录

- uni.login(OBJECT)

  登录

  ```js
  uni.login({
      provider: 'weixin', 
      success: function(loginRes) {
          console.log(loginRes.authResult)
      }
  })
  ```

- uni.checkSession

  检查登录状态是否过期

- uni.getUserIfo

  获取用户信息

  ```js
  uni.login({
      provider: 'weixin',
      success: function(loginRes) {
          console.log(loginRes.authResult)
          // 获取用户信息
          uni.getUserInfo({
              provier: 'weixin',
              success: function(infoRes) {
                  console.log('用户昵称为：' + infoRes.userInfo.nickName)
              }
          })
      }
  })
  ```

具体详见： [https://uniapp.dcloud.io/api/plugins/login?id=login](https://uniapp.dcloud.io/api/plugins/login?id=login)

#### 分享

- uni.share(OBJECT)

  uni-app的App引擎已经封装了微信、QQ、微博的分享SDK，开发者可以直接调用相关功能。

  可以分享到微信、QQ、微博，每个社交平台被称为分享服务提供商，即provider。

  可以分享文字、图片、图文横条、音乐、视频等多种形式。同时注意，分享为小程序也使用本API。即在App里可以通过本API把一个内容以小程序（通常为内容页）方式直接分享给微信好友。

  + 分享到微信聊天界面

    * 分享文字

      ```js
      uni.share({
          provider: 'weixin',
          scene: 'WXSceneSession',
          type: 1,
          summary: '我正在使用HBuilderX开发uni-app，赶紧跟我一起来体验！'，
          success: function(res) {
          	console.log('success:' + JSON.stringify(res))
      	},
          fail: function(err) {
              console.log('fail:' + JSON.stringify(err))
          }    
      })
      ```

    * 分享图片

      ```js
      uni.share({
          provider: 'weixin',
          scene: 'WXSceneSession',
          type: 2,
          imageUrl: 'https://img-cdn-qiniu.dcloud.net.cn/uniapp/images/uni@2x.png',
          success: function(res) {
          	console.log('success:' + JSON.stringify(res))
      	},
          fail: function(err) {
              console.log('fail:' + JSON.stringify(err))
          }    
      })
      ```

    * 分享图文

      href、imageUrl 为必选参数，title/summary 二选一，最好将这四个参数都选上

      ```js
      uni.share({
          provider: 'weixin',
          scene: 'WXSceneSession',
          type: 0,
          href: 'http://uniapp.dcloud.io/',
          title: 'uni-app分享',
          summary: '我正在使用HBuilderX开发uni-app,赶紧跟我一起体验！',
          imageUrl: 'https://img-cdn-qiniu.dcloud.net.cn/uniapp/images/uni@2x.png',
          success: function(res) {
              console.log('success:' + JSON.stringify(res))
          },
          fail: function(err) {
              console.log('fail:' + JSON.stringify(err))
          }
      })
      ```

  + 分享到微信朋友圈

    * 分享文字

      ```js
      uni.share({
          provider: "weixin",
          scene: "WXSenceTimeline",
          type: 1,
          summary: "我正在使用HBuilderX开发uni-app，赶紧跟我一起来体验！",
          success: function (res) {
              console.log("success:" + JSON.stringify(res));
          },
          fail: function (err) {
              console.log("fail:" + JSON.stringify(err));
          }
      });
      ```

    * 分享图片

      ```js
      uni.share({
          provider: "weixin",
          scene: "WXSenceTimeline",
          type: 2,
          imageUrl: "https://img-cdn-qiniu.dcloud.net.cn/uniapp/images/uni@2x.png",
          success: function (res) {
              console.log("success:" + JSON.stringify(res));
          },
          fail: function (err) {
              console.log("fail:" + JSON.stringify(err));
          }
      });
      ```

    * 分享图文

      ```js
      uni.share({
          provider: 'weixin',
          scene: 'WXSenceTimeline',
          type: 0,
          href: 'http://uniapp.dcloud.io/',
          title: 'uni-app分享',
          summary: '我正在使用HBuilderX开发uni-app,赶紧跟我一起体验！',
          imageUrl: 'https://img-cdn-qiniu.dcloud.net.cn/uniapp/images/uni@2x.png',
          success: function(res) {
              console.log('success:' + JSON.stringify(res))
          },
          fail: function(err) {
              console.log('fail:' + JSON.stringify(err))
          }
      })
      ```

- uni.shareWithSystem(OBJECT)

  调用系统分享组件发送分享消息，不需要配置分享 SDK

  ```js
  uni.shareWithSystem({
      summary: '',
      href: 'https://uniapp.dcloud.io',
      success() {
          // 分享完成，请注意此时不一定是分享成功
      },
      fail() {
          // 分享失败
      }
  })
  ```

- plus.share.sendWithSystem(msg, successCB, errorCB)

  Android和iOS都有应用注册分享接口的机制，基本上所有有接收分享内容功能的应用，都会注册分享接口。

  App端可调用手机的系统分享，实现所有注册分享的应用的呼起，比如短信、邮件、蓝牙(仅Android)、隔空投送(仅iOS)，或其他注册系统分享的应用，比如钉钉。

  与`uni.share`相比，调用系统分享不需要集成三方sdk。但有些功能上的限制，比如无法分享为微信小程序。

  ```js
  plus.share.sendWithSystem({content:'分享内容',href:'https://www.dcloud.io/'}, function(){
      console.log('分享成功');
  }, function(e){
      console.log('分享失败：'+JSON.stringify(e));
  });
  ```

- onShareAppMessage(OBJECT)

  小程序中用户点击分享后，在 js 中定义 onShareAppMessage 处理函数（和 onLoad 等生命周期函数同级），设置该页面的分享信息。

  - 用户点击分享按钮的时候会调用。这个分享按钮可能是小程序右上角原生菜单自带的分享按钮，也可能是开发者在页面中放置的分享按钮（\）；
  - 此事件需要 return 一个Object，用于自定义分享内容。

  微信小程序平台的分享管理比较严格，请参考 [小程序分享指引](https://developers.weixin.qq.com/miniprogram/dev/framework/open-ability/share.html)。

  ```js
  export default {
    onShareAppMessage(res) {
      if (res.from === 'button') {// 来自页面内分享按钮
        console.log(res.target)
      }
      return {
        title: '自定义分享标题',
        path: '/pages/test/test?id=123'
      }
    }
  }
  ```

- uni.showShareMenu(OBJECT)

  小程序的原生菜单中显示分享按钮

- uni.hideShareMenu(OBJECT)

  小程序的原生菜单中隐藏分享按钮

  ```js
  uni.hideShareMenu()
  ```

具体详见： [https://uniapp.dcloud.io/api/plugins/share](https://uniapp.dcloud.io/api/plugins/share)

#### 支付

uni.requestPayment(OBJECT)

支付

具体详见： [https://uniapp.dcloud.io/api/plugins/payment](https://uniapp.dcloud.io/api/plugins/payment)

#### 推送

具体详见： [https://uniapp.dcloud.io/api/plugins/push](https://uniapp.dcloud.io/api/plugins/push)

#### 语音

具体详见： [https://uniapp.dcloud.io/api/plugins/voice](https://uniapp.dcloud.io/api/plugins/voice)

### 平台扩展

#### App原生插件

- uni.requireNativePlugin(PluginName)

  引入 App 原生插件

- 内置原生插件

- 本地插件

- 云端插件

具体详见： [https://uniapp.dcloud.io/api/extend/native-plugin](https://uniapp.dcloud.io/api/extend/native-plugin)

### 其他

#### 授权

uni.authorize(OBJECT)

提前向用户发起授权请求。调用后会立刻弹窗询问用户是否同意授权小程序使用某项功能或获取用户的某些数据，但不会实际调用对应接口。如果用户之前已经同意授权，则不会出现弹窗，直接返回成功。如果用户之前拒绝了授权，此接口会直接进入失败回调，一般搭配 `uni.getSetting` 和 `uni.openSetting` 使用

```js
uni.authorize({
    scope: 'scope.userLocation',
    success() {
        uni.getLocation()
    }
})
```

具体详见： [https://uniapp.dcloud.io/api/other/authorize?id=authorize](https://uniapp.dcloud.io/api/other/authorize?id=authorize)

#### 设置

- uni.openSetting(OBJECT)

  调起客户端小程序设置界面，返回用户设置的操作结果

  ```js
  uni.openSetting({
      success(res) {
          console.log(res.authSetting)
      }
  })
  ```

- uni.getSetting(OBJECT)

  获取用户的当前设置

  ```js
  uni.getSetting({
      success(res) {
          console.log(res.authSetting)
      }
  })
  ```

具体详见： [https://uniapp.dcloud.io/api/other/setting?id=opensetting](https://uniapp.dcloud.io/api/other/setting?id=opensetting)

#### 收货地址

uni.chooseAddress(OBJECT)

获取用户收货地址。调起用户编辑收货地址原生界面，并在编辑完成后返回用户选择的地址，需要用户授权 scope.address

```js
uni.chooseAddress({
    success(res) {
        console.log(res.userName)
        console.log(res.postalCode)
        console.log(res.provinceName)
        console.log(res.cityName)
        console.log(res.countyName)
        console.log(res.detailInfo)
        console.log(res.nationalCode)
        console.log(res.telNumber)
    }
})
```

具体详见： [https://uniapp.dcloud.io/api/other/choose-address](https://uniapp.dcloud.io/api/other/choose-address)

#### 获取发票抬头

uni.chooseInvoiceTitle(OBJECT)

选择用户的发票抬头，需要用户授权 scope.invoiceTitle

在微信小程序中，当前当前小程序必须关联一个公众号，且这个公众号是完成了[微信认证](https://mp.weixin.qq.com/wiki?t=resource/res_main&id=mp1496554031_RD4xe)的，才能调用 chooseInvoiceTitle。

```js
uni.chooseInvoiceTitle({
    success(res) {
        console.log(res.type);
        console.log(res.title);
        console.log(res.taxNumber);
        console.log(res.companyAddress);
        console.log(res.telephone);
        console.log(res.bankName);
        console.log(res.bankAccount);
  }
})
```

具体详见： [https://uniapp.dcloud.io/api/other/invoice-title](https://uniapp.dcloud.io/api/other/invoice-title)

#### 小程序跳转

- uni.navigateToMiniProgram(OBJECT)

  打开另一个小程序

  ```js
  uni.navigateToMiniProgram({
      appId: '',
      path: 'pages/index/index?id=123',
      extraData: {
          'datal': 'test'
      },
      success(res) {
          // 打开成功
      }
  })
  ```

- uni.navigateBackMiniProgram(OBJECT)

  跳转回上一个小程序，只有当另一个小程序跳转到当前小程序时才会能调用成功

  ```js
  uni.navigateBackMiniProgram({
      extraData: {
          'datal': 'test'
      },
      success(res) {
          // 返回成功
      }
  })
  ```

具体详见： [https://uniapp.dcloud.io/api/other/open-miniprogram?id=navigatetominiprogram](https://uniapp.dcloud.io/api/other/open-miniprogram?id=navigatetominiprogram)

#### 账号信息

uni.getAccountInfoSync()

获取当前账号信息，可以返回小程序的Appid。如果使用了微信小程序的云端插件，还可以反馈插件的id和版本

```js
const accountInfo = uni.getAccountInfoSync()
console.log(accountInfo.miniProgram.appId)	// 小程序 appId
console.log(accountInfo.plugin.appId)	// 插件 appId
console.log(accountInfo.plugin.version)	// 插件版本号, 'a.b.c' 这样的形式
```

具体详见： [https://uniapp.dcloud.io/api/other/getAccountInfoSync](https://uniapp.dcloud.io/api/other/getAccountInfoSync)

#### 运动(计步器)

sport 运动

此功能为计步器，用于获取手机用户的运动步数。

各平台开发方式暂未统一，使用时需注意用[条件编译](https://uniapp.dcloud.io/platform)调用不同平台的代码。

- App 平台：需使用原生插件，详见[计步器插件](https://ext.dcloud.net.cn/search?q=计步器)
- 微信小程序平台：[规范详情](https://developers.weixin.qq.com/miniprogram/dev/api/open-api/werun/wx.getWeRunData.html)
- 支付宝小程序平台：[规范详情](https://docs.alipay.com/mini/api/gxuu7v)

#### 统计

uni.report( eventName , options )

```js
// 内容统计
// 当 eventName 为 title 时, options 只能为 String 类型
uni.report('title', '首页')

// 登录
uni.report('login', {
    'name': 'uni-app',
    'age': '21',
    // ...
})

// 分享
uni.report('share', '分享')

// 支付成功
uni.report('pay_success', '支付成功')
// or
uni.report('pay_success', {
    '订单金额': '20元',
    '订单名称': '鼠标',
    // ...
})

// 支付失败
uni.report('pay_fail', '支付失败')
// or
uni.report('pay_fail', {
    '订单金额': '20元',
    '订单名称': '鼠标'
    // ...
})

// 注册
uni.report('register', {
    'name': 'uni-app',
    'age': '21',
    // ...
})

// 搜索
uni.report('search', '搜索内容')
// or
uni.report('search', {
    '内容': '搜索内容'
})
```

具体详见： [https://uniapp.dcloud.io/api/other/report](https://uniapp.dcloud.io/api/other/report)

#### 卡卷

仅微信小程序、支付宝小程序支持，各平台开发方式暂未统一，使用时需注意用[条件编译](https://uniapp.dcloud.io/platform)调用不同平台的代码。

- 微信小程序：[规范详情](https://developers.weixin.qq.com/miniprogram/dev/api/wx.openCard.html)
- 支付宝小程序：[规范详情](https://docs.alipay.com/mini/api/card-voucher-ticket)

#### 模板消息

- addTemplate

  组合模板并添加至账号下的个人模板库

- deleteTemplate

  删除账号下的某个模板

- getTemplateLibraryById

  获取模板库中某个模板标题下关键词库

- getTemplateLibraryList

  获取APP模板库标题列表

- getTemplateList

  获取账号下已存在的模板列表

- sendTemplateMessage

  发送模板消息

- alipay.open.app.mini.templatemessage.send

  小程序通过 openapi 给用户触达消息，主要为支付后的触达（通过消费id）和用户提交表单后的触达（通过formId）

具体详见： [https://uniapp.dcloud.io/api/other/template](https://uniapp.dcloud.io/api/other/template)

#### 订阅消息

uni.requestSubscribeMessage(Object object)

```js
uni.requestSubscribeMessage({
    tmplIds: [''],
    success(res) { }
})
```

具体详见： [https://uniapp.dcloud.io/api/other/requestSubscribeMessage](https://uniapp.dcloud.io/api/other/requestSubscribeMessage)

#### 小程序更新

uni.getUpdateManager()

本API返回**全局唯一**的版本更新管理器对象： updateManager，用于管理小程序更新

```js
const updateManager = uni.getUpdateManager()

updateManager.onCheckForUpdate(function(res) {
    // 请求完整版本信息的回调
    console.log(res.hasUpdate)
})

updateManager.onUpdateReady(function(res) {
    uni.showModal({
        title: '更新提示'，
        content: '新版本已经准备好，是否重启应用？',
        success(res) {
        	if(res.confirm) {
                // 新的版本已经下载好，调用 applyUpdate 应用新版本并重启
                updateManager.applyUpdate()
            }
    	}
    })
})

updateManager.onUpdateFailed(function(res) {
    // 新的版本下载失败
})
```

具体详见： [https://uniapp.dcloud.io/api/other/update?id=getupdatemanager](https://uniapp.dcloud.io/api/other/update?id=getupdatemanager)

#### 调试

uni.setEnableDebug(OBJECT)

设置是否打开开关。此开关对应正式版也能生效

```js
// 打开调试
uni.setEnableDebug({
    enableDebug: true
})
// 关闭调试
uni.setEnableDebug({
    enableDebug: false
})
```

具体详见： [https://uniapp.dcloud.io/api/other/set-enable-debug?id=setenabledebug](https://uniapp.dcloud.io/api/other/set-enable-debug?id=setenabledebug)

#### 获取第三方平台数据

- uni.getExtConfig(OBJECT)

  获取第三方平台自定义的数据字段

  ```js
  if(uni.getExtConfig) {
      uni.getExtConfig({
          success(res) {
              console.log(res.extConfig)
          }
      })
  }
  ```

- uni.getExtConfigSync

  `uni.getExtConfig` 的同步版本

  ```js
  const extConfig = uni.getExtConfigSync ? uni.getExtConfigSync() : {}
  console.log(extConfig)
  ```

具体详见： [https://uniapp.dcloud.io/api/other/get-extconfig?id=getextconfig](https://uniapp.dcloud.io/api/other/get-extconfig?id=getextconfig)

