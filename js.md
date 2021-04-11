# JS

### 给DOM对象添加监听事件

```vue
// 监听滚动条事件 此示例为vue示例
<script>
    export default {
        mounted() {
            this.$refs.list.addEventListener('scroll', this.handleScroll)
        },
        methods: {
            handleScroll (arg) {
              // this.$refs.list.scrollTop = this.$refs.list.scrollHeight
            }
        }
    }
</script>


```

### js中 `new Date()` 日期格式处理

```javascript
let myDate = new Date();

myDate.getYear(); //获取当前年份(2位)	=> 121

myDate.getFullYear(); //获取完整的年份(4位,1970-????)	=> 2021

myDate.getMonth(); //获取当前月份(0-11,0代表1月) // 所以获取当前月份是myDate.getMonth()+1;	=> 2 

myDate.getDate(); //获取当前日(1-31)	=> 22

myDate.getDay(); //获取当前星期X(0-6,0代表星期天)	=> 1

myDate.getTime(); //获取当前时间(从1970.1.1开始的毫秒数)	=> 1616399180000

myDate.getHours(); //获取当前小时数(0-23)	=> 15

myDate.getMinutes(); //获取当前分钟数(0-59)	=> 46

myDate.getSeconds(); //获取当前秒数(0-59)	=> 20

myDate.getMilliseconds(); //获取当前毫秒数(0-999)	=> 0

myDate.toLocaleDateString(); //获取当前日期	=> 2021-3-22

myDate.toLocaleTimeString(); //获取当前时间	=> 3:46:20 ├F10: PM┤

myDate.toLocaleString( ); //获取日期与时间	=> 2021-3-22 3:46:20 ├F10: PM┤
```

### js中 `Math` 对象的使用

```javascript
// Math.PI—>Π

// Math.abs(值);—>取绝对值

// Math.ceil(值);—>向上取整

// Math.floor(值);—>向下取整

// Math.round(值);—>四舍五入

// Math.max(值1，值2，值3…);—>取最大值

// Math.min(值1，值2，值3…);—>取最小值

// Math.pow(值1，值2);—>次方

// Math.sqrt(值1);—>开方

// Math.random();—>随机数
```

### 数组方法

```javascript
// push()	把参数添加到数组的最后

// unshift()	把参数添加到数组的前面

// pop()	删除最后一个元素，返回被删除的那个元素

// shift()	删除第一个元素，返回被删除的那个元素

// join()	将数组中的所有元素放入一个字符串，返回字符串

// reverse()	颠倒数组中的元素顺序并返回颠倒后的数组

// sort()	对数组的元素进行排序（按照字符串比较），返回排序后的数组

// concat()	连接两个或者多个数组，返回连接后的数组

// slice()	截取，从已有的数组中返回选定的元素	语法： arrayObject.slice(start,end)

// splice()	删除、插入、替换
	// 删除 语法：arrayObject.splice(index,count)	删除从 index 处开始的零个或多个元素，返回含有被删除的元素的数组。如果 count 为 0，不删除任何值，如果 count 不设置，删除从 index 开始的所有值
	// 插入 语法：arrayObject.splice(index,0,item1,...itemx)	在指定位置插入值
	// 替换 语法： arrayObject.splice(index,count,itemq,...itemx)	在指定位置插入值，同时删除任意数量的项
	// index 为起始位置， count 为要删除的项数， item1...itemx 为要插入的项
```

### 字符串方法

```javascript
// indexOf()	语法： arrayObject.indexOf(searchvalue,startIndex)	从数组的开头（位置为0）开始向后查找	返回值为 number，没有找到的话返回 -1，查找到返回在数组中的位置

// lastIndexOf()	语法： arrayObject.lastIndexOf(searchvalue,startIndex)	从数组的末尾开始向前查找

// charAt()	语法： stringObject.charAt(index)	返回 stringObject 中的 index 位置的字符

// charCodeAt()	语法： stringObject.charCodeAt(index) 返回 stringObject 中的 index 位置字符的字符编码

// slice()	语法：stringObject.slice(start,end)	截取字符串
	// 参数： start 指定字符串的开始位置		end 字符串到哪里结束，end本身不在截取范围之内

// substring()	语法：stringObject.substring(start,end)	提取字符串中两个指定的索引号之间的字符
	// 当参数为负数时，自动转换为0	会将小的数当做开始位置，把较大的数当作结束位置

// substr()	语法：stringObject.substr(start, len)	从起始索引号提取字符串中指定数目的字符
	// start 为指定字符串开始的位置， len为表示截取的字符总数，省略时截取到字符串的末尾	当 start 为负数时，会将传入的负值与字符串的长度相加。而 len 为负值时，返回字符串

// split()	语法： stringObject.split(separator)	把一个字符串分割成字符串数组，返回数组	separator 为分隔符

// replace()	语法： stringObject.replace(regexp/substr,replacement)	在字符串中用一些字符替换另一些字符，或替换一个与正则表达式匹配的子串。返回 string

// toUpperCase()	语法： stringObject.toUpperCase()	把字符串转换为大写

// toLowerCase()	语法：	stringObject.toLowerCase()	把字符串转换为小写
```

### offset 偏移

包括这个元素在文档中占用的所有显示宽度，包括滚动条、`padding`、`border`，不包括`overflow`隐藏的部分

1. `offsetParent`属性返回一个对象的引用，这个对象是距离调用`offsetParent`的父级元素中最近的（在包含层次中最靠近的），并且是已进行过CSS定位的容器元素。 如果这个容器元素未进行CSS定位, 则`offsetParent`属性的取值为根元素的引用。
   - 如果当前元素的父级元素中没有进行CSS定位（`position`为absolute/relative），`offsetParent`为body
   - 如果当前元素的父级元素中有CSS定位（`position`为absolute/relative），`offsetParent`取父级中最近的元素
2. **obj.offsetWidth** 指 obj 控件自身的绝对宽度，不包括因 `overflow` 而未显示的部分，也就是其实际占据的宽度，整型，单位：像素。
   `offsetWidth` = `border-width`*2+`padding-left`+`width`+`padding-right`
3. **obj.offsetHeight** 指 obj 控件自身的绝对高度，不包括因 overflow 而未显示的部分，也就是其实际占据的高度，整型，单位：像素。
   `offsetHeight` = `border-width`*2+`padding-top`+`height`+`padding-bottom`
4. **obj.offsetTop** 指 obj 相对于版面或由 `offsetParent` 属性指定的父坐标的计算上侧位置，整型，单位：像素。
   `offsetTop`= `offsetParent`的padding-top + 中间元素的offsetHeight + 当前元素的margin-top
5. **obj.offsetLeft** 指 obj 相对于版面或由 `offsetParent` 属性指定的父坐标的计算左侧位置，整型，单位：像素。
   `offsetLeft`= `offsetParent`的padding-left + 中间元素的offsetWidth + 当前元素的margin-left

### scroll 滚动

包括这个元素没显示出来的实际宽度，包括`padding`，不包括滚动条、`border`

1. **scrollHeight** 获取对象的滚动高度，对象的实际高度；
2. **scrollLeft** 设置或获取位于对象左边界和窗口中目前可见内容的最左端之间的距离
3. **scrollTop** 设置或获取位于对象最顶端和窗口中可见内容的最顶端之间的距离
4. **scrollWidth** 获取对象的滚动宽度

### client 元素本身的**可视**内容

不包括`overflow`被折叠起来的部分，不包括滚动条、`border`，包括`padding`

1. **clientWidth** 对象可见的宽度，不包括滚动条等边线，会随窗口的显示大小改变
2. **clientHeight** 对象可见的高度
3. **clientTop**、**clientLeft** 这两个返回的是元素周围边框的厚度，一般它的值就是0。因为滚动条不会出现在顶部或者左侧

### 事件

```javascript
// onload	页面加载时触发

// onclick	鼠标点击时触发

// onmouseover	鼠标滑过时触发

// onmouseout	鼠标离开时触发

// onfoucs	获取焦点时触发

// onblur	失去焦点时触发

// onchange	域的内容改变时触发

// onsubmit	表单中的确认按钮被点击时触发

// onmousedown	鼠标按钮在元素上按下时触发

// onmousemove	在鼠标指针移动时触发

// onmouseup	在元素上松开鼠标按钮时触发

// onresize	当调整浏览器窗口的大小时触发

// onscroll	拖动滚动条滚动时触发
```

### 键盘事件与 keyCode 属性

```javascript
// onkeydown	在用户按下一个键盘按键时触发

// onkeypress	在按下键盘按键时触发

// onkeyup	在键盘按键被松开时触发

// keyCode 返回 onkeypress，onkeydown 和 onkeyup 事件触发的键的字符代码，或键的代码
```

### event对象事件

```javascript
// target	点击谁谁就是target，事件源

// currentTarget	事件绑定在谁身上，就指向谁

// clientY	就是指浏览器顶部底边到鼠标的位置

// pageY	就是指浏览器顶部底边到鼠标的位置

// screenY	就是指屏幕顶部到鼠标位置
```

### window对象

#### window对象的属性

`Window` 对象表示浏览器中打开的窗口。

如果文档包含框架`（<frame> 或 <iframe> 标签）`，浏览器会为 `HTML` 文档创建一个 `window` 对象，并为每个框架创建一个额外的 `window` 对象。

```javascript
// window.name	设置或返回窗口的名称

// window.open().opener	打开当前窗口的父窗口,如果当前窗口没有父窗口则返回 null

// window.closed	返回窗口是否已被关闭	

// window.frames	返回窗口中所有命名的框架。该集合是 Window 对象的数组，每个 Window 对象在窗口中含有一个框架	

// window.length	设置或返回窗口中的框架数量,如果当前网页不包含frame和iframe元素，那么window.length就返回0	

// window.frameElement	主要用于当前窗口嵌在另一个网页的情况（嵌入<object>、<iframe>或<embed>元素），返回当前窗口所在的那个元素节点。如果当前窗口是顶层窗口，或者所嵌入的那个网页不是同源的，该属性返回null	

// window.top	指向最顶层窗口，主要用于在框架窗口（frame）里面获取顶层窗口	

// window.parent	指向父窗口。如果当前窗口没有父窗口，window.parent指向自身	

// window.devicePixelRatio	返回一个数值，表示一个 CSS 像素的大小与一个物理像素的大小之间的比率。也就是说，它表示一个 CSS 像素由多少个物理像素组成。它可以用于判断用户的显示环境，如果这个比率较大，就表示用户正在使用高清屏幕，因此可以显示较大像素的图片	

// window.screenX 和 window.screenY	返回浏览器窗口左上角相对于当前屏幕左上角的水平距离和垂直距离（单位像素）。这两个属性只读	

// window.innerHeight和window.innerWidth	返回网页在当前窗口中可见部分的高度和宽度，即“视口”（viewport）的大小（单位像素）。这两个属性只读。这两个属性值包括滚动条的高度和宽度

// window.outerHeight和window.outerWidth	返回浏览器窗口的高度和宽度，包括浏览器菜单和边框（单位像素）。这两个属性只读

// window.scrollX	返回页面的水平滚动距离，单位都为像素。属性只读 返回值不是整数，而是双精度浮点数。如果页面没有滚动，值就是0

// window.scrollY	返回页面的垂直滚动距离，单位都为像素。属性只读 返回值不是整数，而是双精度浮点数。如果页面没有滚动，值就是0

// window.pageXOffset和window.pageYOffset	是window.scrollX和window.scrollY别名

// window.locationbar	地址栏对象	visible属性是一个布尔值，表示这些组件是否可见。属性只读

// window.menubar	菜单栏对象	visible属性是一个布尔值，表示这些组件是否可见。属性只读

// window.scrollbars	窗口的滚动条对象	visible属性是一个布尔值，表示这些组件是否可见。属性只读

// window.toolbar	工具栏对象	visible属性是一个布尔值，表示这些组件是否可见。属性只读

// window.statusbar	状态栏对象	visible属性是一个布尔值，表示这些组件是否可见。属性只读

// window.personalbar	用户安装的个人工具栏对象	visible属性是一个布尔值，表示这些组件是否可见。属性只读

// window.document	指向document对象。注意，这个属性有同源限制。只有来自同源的脚本才能读取这个属性

// window.location	指向Location对象，用于获取当前窗口的 URL 信息。它等同于document.location属性

// window.navigator	指向Navigator对象，用于获取环境信息

// window.history	指向History对象，表示浏览器的浏览历史

// window.localStorage	指向本地储存的localStorage数据

// window.sessionStorage	指向本地储存的sessionStorage数据

// window.console	指向console对象，用于操作控制台

// window.screen	指向Screen对象，表示屏幕信息

// window.isSecureContext	返回一个布尔值，表示当前窗口是否处在加密环境。如果是 HTTPS 协议，就是true，否则就是false
```

#### window 对象的方法

```javascript
// window.alert()	弹出的对话框，只有一个“确定”按钮，往往用来通知用户某些信息

// window.prompt()	弹出的对话框，提示文字的下方，还有一个输入框，要求用户输入信息，并有“确定”和“取消”两个按钮。它往往用来获取用户输入的数据

// window.confirm()	弹出的对话框，除了提示信息之外，只有“确定”和“取消”两个按钮，往往用来征询用户是否同意

// window.open()	新建另一个浏览器窗口，类似于浏览器菜单的新建窗口选项。它会返回新窗口的引用，如果无法新建窗口，则返回null

// window.close()	关闭当前窗口，一般只用来关闭window.open方法新建的窗口。该方法只对顶层窗口有效，iframe框架之中的窗口使用该方法无效

// window.stop()	完全等同于单击浏览器的停止按钮，会停止加载图像、视频等正在或等待加载的对象

// window.moveTo()	用于移动浏览器窗口到指定位置。它接受两个参数，分别是窗口左上角距离屏幕左上角的水平距离和垂直距离，单位为像素

// window.moveBy()	将窗口移动到一个相对位置。它接受两个参数，分布是窗口左上角向右移动的水平距离和向下移动的垂直距离，单位为像素

// window.resizeTo()	用于缩放窗口到指定大小。它接受两个参数，第一个是缩放后的窗口宽度（outerWidth属性，包含滚动条、标题栏等等），第二个是缩放后的窗口高度（outerHeight属性）

// window.resizeBy()	用于缩放窗口。它与window.resizeTo()的区别是，它按照相对的量缩放，window.resizeTo()需要给出缩放后的绝对大小。它接受两个参数，第一个是水平缩放的量，第二个是垂直缩放的量，单位都是像素

// window.scrollTo()	用于将文档滚动到指定位置。它接受两个参数，表示滚动后位于窗口左上角的页面坐标。它也可以接受一个配置对象作为参

// window.scrollBy()	用于将网页滚动指定距离（单位像素）。它接受两个参数：水平向右滚动的像素，垂直向下滚动的像素

// window.print()	会跳出打印对话框，与用户点击菜单里面的“打印”命令效果相同

// window.focus()	会激活窗口，使其获得焦点，出现在其他窗口的前面

// window.blur()	将焦点从窗口移除。
	// 当前窗口获得焦点时，会触发focus事件；当前窗口失去焦点时，会触发blur事件

// window.getSelection()	返回一个Selection对象，表示用户现在选中的文本。使用Selection对象的toString方法可以得到选中的文本

// window.getComputedStyle()	接受一个元素节点作为参数，返回一个包含该元素的最终样式信息的对象

// window.matchMedia()	用来检查 CSS 的mediaQuery语句

// window.requestAnimationFrame() 跟setTimeout类似，都是推迟某个函数的执行。不同之处在于，setTimeout必须指定推迟的时间，window.requestAnimationFrame()则是推迟到浏览器下一次重流时执行，执行完才会进行下一次重绘。重绘通常是 16ms 执行一次，不过浏览器会自动调节这个速率，比如网页切换到后台 Tab 页时，requestAnimationFrame()会暂停执行。如果某个函数会改变网页的布局，一般就放在window.requestAnimationFrame()里面执行，这样可以节省系统资源，使得网页效果更加平滑。因为慢速设备会用较慢的速率重流和重绘，而速度更快的设备会有更快的速率。window.requestAnimationFrame()的返回值是一个整数，这个整数可以传入window.cancelAnimationFrame()，用来取消回调函数的执行

// window.requestIdleCallback()	setTimeout类似，也是将某个函数推迟执行，但是它保证将回调函数推迟到系统资源空闲时执行。也就是说，如果某个任务不是很关键，就可以使用window.requestIdleCallback()将其推迟执行，以保证网页性能。它跟window.requestAnimationFrame()的区别在于，后者指定回调函数在下一次浏览器重排时执行，问题在于下一次重排时，系统资源未必空闲，不一定能保证在16毫秒之内完成；window.requestIdleCallback()可以保证回调函数在系统资源空闲时执行。该方法接受一个回调函数和一个配置对象作为参数。配置对象可以指定一个推迟执行的最长时间，如果过了这个时间，回调函数不管系统资源有无空虚，都会执行。callback参数是一个回调函数。该回调函数执行时，系统会传入一个IdleDeadline对象作为参数。IdleDeadline对象有一个didTimeout属性（布尔值，表示是否为超时调用）和一个timeRemaining()方法（返回该空闲时段剩余的毫秒数）。options参数是一个配置对象，目前只有timeout一个属性，用来指定回调函数推迟执行的最大毫秒数。该参数可选。window.requestIdleCallback()方法返回一个整数。该整数可以传入window.cancelIdleCallback()取消回调函数
```

##### window.open()

```javascript
// window.open(url, windowName, [windowFeatures])
// url: 字符串，表示新窗口的网址。如果省略，默认网址就是about:blank
// windowName：字符串，表示新窗口的名字。如果该名字的窗口已经存在，则占用该窗口，不再新建窗口。如果省略，就默认使用_blank，表示新建一个没有名字的窗口。另外还有几个预设值，_self表示当前窗口，_top表示顶层窗口，_parent表示上一层窗
// windowFeatures：字符串，内容为逗号分隔的键值对（详见下文），表示新窗口的参数，比如有没有提示栏、工具条等等。如果省略，则默认打开一个完整 UI 的新窗口。如果新建的是一个已经存在的窗口，则该参数不起作用，浏览器沿用以前窗口的参数

var popup = window.open(
  'somepage.html',
  'DefinitionsWindows',
  'height=200,width=200,location=no,status=yes,resizable=yes,scrollbars=yes'
);

// 上面代码表示，打开的新窗口高度和宽度都为200像素，没有地址栏，但有状态栏和滚动条，允许用户调整大小
// 第三个参数可以设定如下属性
```

- `left`：新窗口距离屏幕最左边的距离（单位像素）。注意，新窗口必须是可见的，不能设置在屏幕以外的位置。

- `top`：新窗口距离屏幕最顶部的距离（单位像素）。

- `height`：新窗口内容区域的高度（单位像素），不得小于100。

- `width`：新窗口内容区域的宽度（单位像素），不得小于100。

- `outerHeight`：整个浏览器窗口的高度（单位像素），不得小于100。

- `outerWidth`：整个浏览器窗口的宽度（单位像素），不得小于100。

- `menubar`：是否显示菜单栏。

- `toolbar`：是否显示工具栏。

- `location`：是否显示地址栏。

- `personalbar`：是否显示用户自己安装的工具栏。

- `status`：是否显示状态栏。

- `dependent`：是否依赖父窗口。如果依赖，那么父窗口最小化，该窗口也最小化；父窗口关闭，该窗口也关闭。

- `minimizable`：是否有最小化按钮，前提是`dialog=yes`。

- `noopener`：新窗口将与父窗口切断联系，即新窗口的`window.opener`属性返回`null`，父窗口的`window.open()`方法也返回`null`。

- `resizable`：新窗口是否可以调节大小。

- `scrollbars`：是否允许新窗口出现滚动条。

- `dialog`：新窗口标题栏是否出现最大化、最小化、恢复原始大小的控件。

- `titlebar`：新窗口是否显示标题栏。

- `alwaysRaised`：是否显示在所有窗口的顶部。

- `alwaysLowered`：是否显示在父窗口的底下。

- `close`：新窗口是否显示关闭按钮。

对于那些可以打开和关闭的属性，设为`yes`或`1`或不设任何值就表示打开，比如`status=yes`、`status=1`、`status`都会得到同样的结果。如果想设为关闭，不用写`no`，而是直接省略这个属性即可。也就是说，如果在第三个参数中设置了一部分属性，其他没有被设置的`yes/no`属性都会被设成`no`，只有`titlebar`和关闭按钮除外（它们的值默认为`yes`)

上面这些属性，属性名与属性值之间用等号连接，属性与属性之间用逗号分隔。

```javascript
'height=200,width=200,location=no,status=yes,resizable=yes,scrollbars=yes'
```

##### window.scrollTo()

```javascript
// window.scrollTo(options)

window.scrollTo({
  top: 1000,
  behavior: 'smooth'
});
```

配置对象`options`有三个属性

- `top`：滚动后页面左上角的垂直坐标，即`y`坐标。

- `left`：滚动后页面左上角的水平坐标，即`x`坐标。

- `behavior`：字符串，表示滚动的方式，有三个可能值（`smooth`、`instant`、`auto`），默认值为`auto`。

`window.scroll()`方法是`window.scrollTo()`方法的别名

#### window 事件

```javascript
// window.onload	可以指定 load 事件的回调函数。load发生在文档在浏览器窗口加载完毕时。

// window.onerror	对 error 事件指定回调函数。浏览器脚本发生错误时，会触发window对象的error事件
```

#### window对象的事件监听属性

- `window.onafterprint`：`afterprint`事件的监听函数。

- `window.onbeforeprint`：`beforeprint`事件的监听函数。

- `window.onbeforeunload`：`beforeunload`事件的监听函数。

- `window.onhashchange`：`hashchange`事件的监听函数。

- `window.onlanguagechange`: `languagechange`的监听函数。

- `window.onmessage`：`message`事件的监听函数。

- `window.onmessageerror`：`MessageError`事件的监听函数。

- `window.onoffline`：`offline`事件的监听函数。

- `window.ononline`：`online`事件的监听函数。

- `window.onpagehide`：`pagehide`事件的监听函数。

- `window.onpageshow`：`pageshow`事件的监听函数。

- `window.onpopstate`：`popstate`事件的监听函数。

- `window.onstorage`：`storage`事件的监听函数。

- `window.onunhandledrejection`：未处理的`Promise`对象的`reject`事件的监听函数。

- `window.onunload`：`unload`事件的监听函数。

### document 对象

当浏览器载入 HTML 文档, 它就会成为 **Document 对象**。Document 对象是 HTML 文档的根节点。Document 对象使我们可以从脚本中对 HTML 页面中的所有元素进行访问。Document 对象是 Window 对象的一部分，可通过 window.document 属性对其进行访问

#### document 对象合集

```javascript
// document.all	返回对文档中所有 HTML 元素的引用

// document.anchors[]	可返回对文档中所有 Anchor（a链接） 对象的引用

// document.forms[]	可返回对文档中所有 Form 对象的引用

// document.images[]	可返回对文档中所有 Image 对象的引用

// document.links[]	可返回对文档中所有 Area 和 Link 对象的引用
```

#### document 对象属性

```javascript
// document.body	用于设置或返回文档体

// document.cookie	可设置或查询与当前文档相关的所有 cookie

// document.domain	可返回下载当前文档的服务器域名

// document.lastModified	可返回文档最后被修改的日期和时间

// document.referrer	可返回载入当前文档的文档的 URL

// document.title	可返回当前文档的标题（ HTML title 元素中的文本）

// document.URL	可返回当前文档的 URL
```

#### document 对象方法

```javascript
// document.close()	可关闭一个由 document.open 方法打开的输出流，并显示选定的数据

// document.getElementById(id)	可返回对拥有指定 ID 的第一个对象的引用

// document.getElementsByName(name)	可返回带有指定名称的对象的集合

// document.getElementsByTagName(tagname)	可返回带有指定标签名的对象的集合

// document.open(mimetype,replace)	
	// mimetype	可选。规定正在写的文档的类型。默认值是 "text/html"。
	// replace	可选。当此参数设置后，可引起新文档从父文档继承历史条目。

// document.write(exp1,exp2,exp3,....)	可向文档写入 HTML 表达式或 JavaScript 代码

// document.writeln(exp1,exp2,exp3,....)	与 write() 方法作用相同，外加可在每个表达式后写一个换行符
```



### location 对象

`location`对象提供了与当前窗口中加载的文档有关的信息，还提供了一些导航的功能，它既是`window`对象的属性，也是`document`对象的属性。

#### location 对象属性

```javascript
// location.hash = anchorname	是一个可读可写的字符串，该字符串是 URL 的锚部分（从 # 号开始的部分）

// location.host	是一个可读可写的字符串，可设置或返回当前 URL 的主机名称和端口号

// location.hostname	是一个可读可写的字符串，可设置或返回当前 URL 的主机名

// location.href = URL	是一个可读可写的字符串，可设置或返回当前显示的文档的完整 URL

// location.pathname = path	是一个可读可写的字符串，可设置或返回当前 URL 的路径部分

// location.port = portnumber	是一个可读可写的字符串，可设置或返回当前 URL 的端口部分

// location.protocol = path	是一个可读可写的字符串，可设置或返回当前 URL 的协议

// location.search = path_from_questionmark	是一个可读可写的字符串，可设置或返回当前 URL 的查询部分（问号 ? 之后的部分）
```

#### location 对象方法

```javascript
// location.assign(URL)	可加载一个新的文档

// location.reload(force)	用于重新加载当前文档

// location.replace(newURL)	可用一个新文档取代当前文档
```

### navigator 对象

包含有关访问者的信息。

```javascript
// navigator.cookieEnabled	返回 true，如果 cookie 已启用，否则返回 false

// navigator.appName	返回浏览器的应用程序名称

// navigator.appCodeName	返回浏览器的应用程序代码名称

// navigator.product 返回浏览器引擎的产品名称

// navigator.appVersion	返回有关浏览器的版本信息

// navigator.userAgent	返回由浏览器发送到服务器的用户代理报头（user-agent header）

// navigator.platform	返回浏览器平台（操作系统）

// navigator.language	返回浏览器语言

// navigator.onLine	返回 true，假如浏览器在线

// navigator.javaEnabled()	返回 true，如果 Java 已启用
```

### history 对象

包含浏览器历史。

#### history 对象属性

```javascript
// history.length	当前窗口访问过的网址数量（包括当前网页）

// history.state	History堆栈最上层的状态值
```

#### history 对象方法

```javascript
// history.back()	加载历史列表中前一个 URL

// history.forward()	加载历史列表中下一个 URL

// history.go(number|URL)	加载历史列表中的某个具体的页面	（-1上一个页面，1前进一个页面)或一个字符串，字符串必须是局部或完整的URL，该函数会去匹配字符串的第一个URL
```

#### history 对象 popstate 事件

每当同一个文档的浏览历史（即`history`对象）出现变化时，就会触发`popstate`事件。只有用户点击浏览器倒退按钮和前进按钮，或者使用 JavaScript 调用`History.back()`、`History.forward()`、`History.go()`方法时才会触发。

```javascript
window.onpopstate = function(event) {
  console.log('location: ' + document.location);
  console.log('state: ' + JSON.stringify(event.state));
}
```

### localStorage 对象

长久保存整个网站的数据，保存的数据没有过期时间，直到手动去删除。

```javascript
// localStorage.setItem("key", "value")	保存数据

// localStorage.getItem("key")	读取数据

// localStorage.removeItem("key")	删除数据
```

### sessionStorage 对象

临时保存同一窗口(或标签页)的数据，在关闭窗口或标签页之后将会删除这些数据。

```javascript
// sessionStorage.setItem("key", "value")	保存数据

// sessionStorage.getItem("key")	读取数据

// sessionStorage.removeItem("key")	删除指定键的数据

// sessionStorage.clear()	删除所有数据
```

### console 对象



### screen 对象

```javascript
// screen.width 返回以像素计的访问者屏幕宽度

// screen.height 返回以像素计的访问者屏幕的高度

// screen.availWidth 返回访问者屏幕的宽度，以像素计，减去诸如窗口工具条之类的界面特征

// screen.availHeight 返回访问者屏幕的高度，以像素计，减去诸如窗口工具条之类的界面特征

// screen.colorDepth 返回用于显示一种颜色的比特数

// screen.pixelDepth 返回屏幕的像素深度
```



