## ECharts

### 快速上手简单实例

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>echarts快速上手</title>
    <!-- 步骤一：引入 echarts.js 文件 -->
    <script src="./lib/echarts.min.js"></script>
</head>
<body>
    <!-- 步骤一：引入 echarts.js 文件
    步骤二：准备一个呈现图表的盒子
    步骤三：初始化 echarts 实例对象
    步骤四：准备配置项
    步骤五：将配置项设置给 echarts 实例对象 -->

    <!-- 步骤二：准备一个呈现图表的盒子    -->
    <div style="width: 600px;height: 400px;"></div>
    <script>
        // 步骤三：初始化 echarts 实例对象
        // 参数，dom， 决定图表最终呈现的位置
        let mCharts = echarts.init(document.querySelector('div'))
        // 步骤四：准备配置项
        let option = {
            xAxis: {
                type:'category',
                data: ['小明','小红','小王']
            },
            yAxis: {
                type: 'value'
            },
            series: [
                {
                    name: '语文',
                    type: 'bar',
                    data: [70, 92, 87]
                }
            ]
        }
        // 步骤五：将配置项设置给 echarts 实例对象
        mCharts.setOption(option)
    </script>
</body>
</html>
```

### 配置项

`echarts` 使用过程中最重要的就是配置项，图表的展示也是根据配置项的不同而进行改变的。

```js
// xAxis: 直角坐标系中的 x 轴
// yAxis: 直角坐标系中的 y 轴
// series: 系列列表。每个系列通过 type 决定自己的图标类型
let option = {
    xAxis: {
        type:'category',	// 类目轴
        data: ['小明','小红','小王']
    },
    yAxis: {
        type: 'value'	// 数值轴
    },
    series: [
        {
            name: '语文',
            type: 'bar',	// 根据不同参数展示不同图表类型
            data: [70, 92, 87]
        }
    ]
}
```

### 通用配置

​	通用配置指的就是任何图表都能使用的配置

- 标题：`title`

  - 文字样式

    `textStyle`

  - 标题边框

    `borderWidth` 、 `borderColor` 、`borderRadius`

  - 标题位置

    `left` 、 `top` 、 `right` 、 `bottom`

- 提示：`tooltip`

  ​	提示框组件，用于配置鼠标滑过或点击图表时的显示框

  - 触发类型： `trigger`

    `item` `axis`

  - 触发时机： `triggerOn`

    `mouseover` `click`

  - 格式化： `formatter`

    `字符串模板` `回调函数`

- 工具按钮：`toolbox`

  `ECharts` 提供的工具栏

  - 内置有导出图片，数据视图，动态类型切换，数据区域缩放，重置五个工具

  - 显示工具栏按钮 `feature`

    `saveAsImage` 、`dataView` 、`restore` 、`dataZoom` 、`magicType`

- 图例：`legend`

  图例，用于筛选系列，需要和 `series` 配合使用

  - `legend` 中的 `data` 是一个数组
  - `legend` 中的 `data` 的值需要和 `series` 数组中某组数据的 `name` 值一致

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>柱状图的实现</title>
    <script src="./lib/echarts.min.js"></script>
</head>
<body>
    <div style="width: 600px;height: 400px;"></div>

    <script>
        const myChart = echarts.init(document.querySelector('div'))

        const xDataArr = ['张三','李四','王五','闰土','小明','茅台','二妞','大强']

        const yDataArr = [88, 92, 63, 77, 94, 80, 72, 86]

        const yDataArr1 = [93, 60, 61, 62, 85, 79, 92, 30]

        let option = {
            title: {
                text: '成绩展示',
                textStyle: {
                    color: 'red'
                },
                borderWidth: 5,
                borderColor: 'blue',
                borderRadius: 5
            },
            tooltip: {
                // trigger: 'item'
                trigger: 'axis',
                triggerOn: 'click',
                // formatter: '{b} 的成绩是 {c}'
                formatter: function(arg) {
                    return arg[0].name + '的分数是：' + arg[0].data
                }
            },
            toolbox: {
                feature: {
                    saveAsImage: {},    // 导出图片
                    dataView: {},   // 数据视图
                    restore: {},    // 重置
                    dataZoom: {},   // 区域缩放
                    magicType: {
                        type: ['bar', 'line']
                    }   // 动态图标类型的切换
                }
            },
            legend: {
                data: ['语文', '数学']
            },
            xAxis: {
                type: 'category',
                data: xDataArr  
            },
            yAxis: {
                type: 'value',  
            },
            series: [
                {
                    name: '语文',
                    type: 'bar',
                    data: yDataArr
                },
                {
                    name: '数学',
                    type: 'bar',
                    data: yDataArr1
                }
            ]
        }

        myChart.setOption(option)
    </script>
</body>
</html>
```

### 直角坐标系中的常用配置

直角坐标系图表：柱状图 折线图 散点图

- 配置1： 网格 `grid`

  `grid` 是用来控制直角坐标系的布局和大小，x 轴和 y 轴就是在 `grid` 的基础上进行绘制的

  - 显示 `grid`

    `show`

  - `grid` 的边框

    `borderWidth`、`borderColor`

  - `grid` 的位置和大小

    `left` 、`top` 、`right` 、`bottom` 、`width`、`height` 

- 配置2： 坐标轴 `axis`

  坐标轴分为 x 轴和 y 轴，一个 `grid` 中最多有两种位置的 x 轴和 y 轴

  - 坐标轴类型 `type`
    - `value`： 数值轴，自动会从目标数据中读取数据
    - `category`：类目轴，该类型必须通过 `data` 设置类目数据

  - 显示位置 `position`
    - `xAxis`：可取值为 `top` 或者 `bottom`
    - `yAxis` 可取值为 `left` 或者 `right`

- 配置3： 区域缩放 `dataZoom`

  `dataZoom` 用于区域缩放，对数据范围过滤， x 轴和 y 轴都可以拥有，`dataZoom` 是一个数组，意味着可以配置多个区域缩放器

  - 类型 `type`

    `slider`：滑块

    `inside`：内置，依靠鼠标滚轮或者双指缩放

  - 指明产生作用的轴

    `xAxisIndex`：设置缩放组件控制的是哪个 x 轴，一般写 0 即可

    `yAxisIndex`：设置缩放组件控制的是哪个 y 轴，一般写 0 即可

### 常用的七种图表

#### 1. 柱状图

- 基本的柱状图

  + 基本的代码结构
  + x 轴和 y 轴的数据
  + series 中的 type 设置为 bar

- 常见效果

  + 标记：最大值、最小值、平均值

    `markPoint` 最大值、最小值

    `markLine ` 平均值

  + 显示：数值显示、柱宽度、横向柱状图

    `label` 数值显示

    `barWidth` 柱宽度

    `更改 x 轴与 y 轴的角色` 横向柱状图

+ 柱状图特点
  + 柱状图描述的是分类数据，呈现的是每一个分类中<font color=#f66>有多少</font>，通过柱状图，可以很清晰的看出每个分类数据的排名情况

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>柱状图的实现</title>
    <script src="./lib/echarts.min.js"></script>
</head>
<body>
    <div style="width: 600px;height: 400px;"></div>

    <script>
        const myChart = echarts.init(document.querySelector('div'))

        const xDataArr = ['张三','李四','王五','闰土','小明','茅台','二妞','大强']

        const yDataArr = [88, 92, 63, 77, 94, 80, 72, 86]

        let option = {
            xAxis: {
                type: 'category',
                data: xDataArr
            },
            yAxis: {
                type: 'value'
            },
            series: [
                {
                    name: '语文',
                    type: 'bar',
                    markPoint:{
                        data: [
                            {
                                type: 'max', name: '最大值'
                            },
                            {
                                type: 'min', name: '最小值'
                            }
                        ]
                    },
                    markLine: {
                        data: [
                            {
                                type: 'average', name: '平均值'
                            }
                        ]
                    },
                    label: {
                        show: true,
                        rotate: 60,
                        position: 'top'
                    },
                    barWidth: '30%',
                    data: yDataArr

                }
            ]
        }

        myChart.setOption(option)
    </script>
</body>
</html>
```

#### 2.折线图

- 基本的折线图
  + 基本的代码结构
  + x 轴和 y 轴的数据
  + `series` 中的 `type` 设置为 `line`

- 常见效果

  + 标记：最大值 最小值 平均值 标注区间

    `markPoint`、`markLine` 、`markArea` 

  + 线条控制：平滑 风格

    `smooth` 、`lineStyle`

  - 填充风格

    `areaStyle`

  + 紧挨边缘

    `boundaryGap`

  + 缩放：脱离 0 值比例

    `scale`

  + 堆叠图

    `stack` 

- 折线图特点
  
  + 折线图常用来分析数据随时间的变化趋势

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>折线图的实现</title>
    <script src="./lib/echarts.min.js"></script>
</head>
<body>
    <div style="width: 600px; height: 400px;"></div>

    <script>
        const myChart = echarts.init(document.querySelector('div'))

        let xDataArr = ['1月','2月','3月','4月','5月','6月','7月','8月','9月','10月','11月','12月']

        let yDataArr = [3000, 2800, 900, 1000, 800, 700, 1400, 1300, 900, 1000, 800, 600] 
        let yDataArr1 = [2000, 3800, 1900, 1500, 1800, 1700, 2400, 1600, 1900, 1500, 1800, 1600]

        const option = {
            xAxis: {
                type: 'category',
                data: xDataArr,
                boundaryGap: false
            },
            yAxis: {
                type: 'value',
                scale: true
            },
            series: [
                {
                    name: '康师傅',
                    data: yDataArr,
                    type: 'line',
                    markPoint: {
                        data: [
                            {
                                name: '最大值',
                                type: 'max'
                            },
                            {
                                name: '最小值',
                                type: 'min'
                            }
                        ]
                    },
                    markLine: {
                        data: [
                            {
                                type: 'average'
                            }
                        ]
                    },
                    markArea: {
                        data: [
                            [
                                {
                                    xAxis: '1月'
                                },
                                {
                                    xAxis: '2月'
                                }
                            ],
                            [
                                {
                                    xAxis: '7月'
                                },
                                {
                                    xAxis: '8月'
                                }
                            ]
                        ]
                    },
                    // smooth: true,
                    lineStyle: {
                        color: 'green',
                        type: 'solid'  // dashed dotted solid
                    },
                    areaStyle: {
                        color: 'red'
                    },
                    stack: 'all'
                },
                {
                    name: '统一',
                    data: yDataArr1,
                    type: 'line',
                    markPoint: {
                        data: [
                            {
                                name: '最大值',
                                type: 'max'
                            },
                            {
                                name: '最小值',
                                type: 'min'
                            }
                        ]
                    },
                    markLine: {
                        data: [
                            {
                                type: 'average'
                            }
                        ]
                    },
                    markArea: {
                        data: [
                            [
                                {
                                    xAxis: '1月'
                                },
                                {
                                    xAxis: '2月'
                                }
                            ],
                            [
                                {
                                    xAxis: '7月'
                                },
                                {
                                    xAxis: '8月'
                                }
                            ]
                        ]
                    },
                    // smooth: true,
                    lineStyle: {
                        color: 'green',
                        type: 'solid'  // dashed dotted solid
                    },
                    areaStyle: {
                        color: 'blue'
                    },
                    stack: 'all'
                }
                
            ]
        }
        myChart.setOption(option)
    </script>
</body>
</html>
```

#### 3.散点图

- 基本的散点图
  + 基本的代码结构
  + x 轴和 y 轴的数据，是一个二维数组
  + `series` 中的 `type` 设置为 `scatter`
  + `xAxis` 和 `yAxis` 中的 `type` 设置为 `value`

- 常见效果

  + 气泡图效果

    * 散点的大小不同 `symbolSize`
    * 散点的颜色不同 `itemStyle.color`

  + 涟漪动画效果 

    `type:'effectScatter`'

    `showEffectOn: 'emphasis'`

    `rippleEffect: {}`

- 散点图的特点
  
  + 散点图可以帮助我们推断出不同维度数据之间的相关性
  + 散点图也经常用在地图的标注上

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>散点图的实现</title>
    <script src="./lib/echarts.min.js"></script>
</head>

<body>
    <div style="width: 600px; height: 400px;"></div>
    <script>


        let data = [{ "height": 179.1, "weight": 90 }, { "height": 170, "weight": 60 }, { "height": 175, "weight": 75 }, { "height": 179.1, "weight": 79.1 }, { "height": 179.1, "weight": 79.1 }, { "height": 169.1, "weight": 65 }]

        let axisData = []
        for (let i = 0; i < data.length; i++) {
            let height = data[i].height
            let weight = data[i].weight
            let newArr = [height, weight]
            axisData.push(newArr)
        }
        let myChart = echarts.init(document.querySelector('div'))
        let option = {
            xAxis: {
                type: 'value',
                scale: true
            },
            yAxis: {
                type: 'value',
                scale: true
            },
            series: [
                {
                    // type: 'scatter', // 非涟漪效果
                    type: 'effectScatter',  // 涟漪效果
                    showEffectOn: 'emphasis',   // render 涟漪的触发条件
                    rippleEffect: {
                        scale: 10   // 涟漪大小
                    },
                    data: axisData,
                    // symbolSize: 30
                    symbolSize: function (arg) {
                        // console.log(arg);
                        let height = arg[0] / 100
                        let weight = arg[1]
                        // bmi = 体重kg / （身高m * 身高m） 大于28，就代表肥胖
                        let bmi = weight / height / height
                        if (bmi > 28) {
                            return 20
                        } else {
                            return 10
                        }
                    },
                    itemStyle: {
                        // color: 'green'
                        color: function (arg) {
                            let height = arg.data[0] / 100
                            let weight = arg.data[1]
                            // bmi = 体重kg / （身高m * 身高m） 大于28，就代表肥胖
                            let bmi = weight / height / height
                            if (bmi > 28) {
                                return 'red'
                            } else {
                                return 'green'
                            }
                        }
                    }
                }
            ]
        }

        myChart.setOption(option)
    </script>
</body>

</html>
```

#### 4.饼图

- 基本的饼图

  + 基本的代码结构
  + 数据是由 `name` 和 `value` 组成的对象所形成的数组
  + `series` 中的 `type` 设置为 `pie`
  + 无须设置 `xAxis` 和 `yAxis`

- 常见效果

  + 显示数值

    `label.formatter`

  + 圆环

    设置两个半径 `radius:['50%','70%']`

  + 南丁格尔图

    `roseType:'radius'`

  + 选中效果

    选中模式 `selectedMode: single/multiple`

    选中偏移量 `selectedOffset: 30`

- 饼图的特点
  
  + 饼图可以很好的帮助用户快速了解不同分类的数据的<font color="ff6666">占比情况</font>

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>饼图的实现</title>
    <script src="./lib/echarts.min.js"></script>
</head>
<body>
    <div style="width: 600px; height: 400px;"></div>
    <script>
        const myChart = echarts.init(document.querySelector('div'))

        let data = [{
            "name": "淘宝",
            "value": 11231
        },{
            "name": "京东",
            "value": 22673
        },{
            "name": "唯品会",
            "value": 6123
        },{
            "name": "1号店",
            "value": 8989
        },{
            "name": "聚美优品",
            "value": 6700
        }]

        let option = {
            series: [
                {
                    type: 'pie',
                    data: data,
                    label: {    // 饼图文字的显示
                        show: true, // 显示文字
                        // formatter: 'hehe'    // 决定文字显示的内容
                        formatter: function(arg) {
                            return arg.name + '平台' + arg.value + '元\n' + arg.percent + '%'
                        }
                    },
                    // radius: 100     // 饼图的半径
                    // radius: '40%'   // 百分比参照的是宽度和高度中较小的那一部分的一般来进行百分比设置
                    // radius: ['50%', '75%']  // 第0个元素代表的是内圆的半径，第一个元素是外圆的半径
                    roseType: 'radius',  // 南丁格尔图 饼图的每个区域的半径是不同的
                    // selectedMode: 'single', // 选中的效果，能够将选中的区域偏离圆点一小段距离
                    selectedMode: 'multiple',    // 同上，可多选
                    selectedOffset: 30  // 选中时的偏移量
                }
            ]
        }

        myChart.setOption(option)
    </script>
</body>
</html>
```

#### 5.地图

地图图表的使用方式

- 百度地图 `API`

  需要申请百度地图`ak`

- 矢量地图

  需要准备矢量地图数据

常用配置

- 缩放拖动： `roam`                           
- 名称显示： `label`
- 初始缩放比例： `zoom`
- 地图中心点： `center`

常见效果

- 显示某个区域：同地图的渲染方式渲染区域地图
- 不同城市颜色不同
  1. 显示基本的中国地图
  2. 城市的空气质量数据设置给 `series`
  3. 将 `series` 下的数据和 `geo` 关联起来
  4. 结合 `visualMap` 配合使用

- 地图和散点图结合

  1. 给 `series` 下增加新的对象

  2. 准备好散点数据，设置给新对象的 `data`

  3. 配置新对象的 `type` 

     ```json
     type: effectScatter
     ```

  4. 让散点图使用地图坐标系统

     ```json
     coordinateSystem: 'geo'
     ```

  5. 让涟漪的效果更加明显

     ```json
     rippleEffect: {
         scale: 10
     }
     ```

地图特点

- 地图主要可以帮助我们从宏观的角度快速看出不同<font color="#f66">地理位置</font>上数据的差异

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>地图的实现</title>
    <script src="./lib/echarts.min.js"></script>
    <script src="./map/china.js"></script>
</head>
<body>
    <div style="width: 600px; height: 400px; margin: 100px auto;"></div>

    <script>
        // 1. ECharts 最基本的代码结构
        // 2. 准备中国地图的矢量数据
        // 3. 使用 Ajax 获取矢量地图数据
        // 4. 在 Ajax 的回调中注册地图矢量数据 echarts.registerMap('chinaMap', 矢量地图数据)
        // 5. 配置 geo 的 type 为 'map',map 为 'chinaMap'
        const airData = [
            { name: '北京', value: 39.92 },
            { name: '天津', value: 39.13 },
            { name: '上海', value: 31.22 },
            { name: '重庆', value: 66 },
            { name: '河北', value: 147 },
            { name: '河南', value: 113 },
            { name: '云南', value: 25.04 },
            { name: '辽宁', value: 50 },
            { name: '黑龙江', value: 114 },
            { name: '湖南', value: 175 },
            { name: '安徽', value: 117 },
            { name: '山东', value: 92 },
            { name: '新疆', value: 84 },
            { name: '江苏', value: 67 },
            { name: '浙江', value: 84 },
            { name: '江西', value: 96 },
            { name: '湖北', value: 273 },
            { name: '广西', value: 59 },
            { name: '甘肃', value: 99 },
            { name: '山西', value: 39 },
            { name: '内蒙古', value: 58 },
            { name: '陕西', value: 61 },
            { name: '吉林', value: 51 },
            { name: '福建', value: 29 },
            { name: '贵州', value: 71 },
            { name: '广东', value: 38 },
            { name: '青海', value: 57 },
            { name: '西藏', value: 24 },
            { name: '四川', value: 58 },
            { name: '宁夏', value: 52 },
            { name: '海南', value: 54 },
            { name: '台湾', value: 88 },
            { name: '香港', value: 66 },
            { name: '澳门', value: 77 },
            { name: '南海诸岛', value: 55 },
        ]

        const scatterData = [
            { value: [117.283042, 31.86119] }
        ]
        
        const myChart = echarts.init(document.querySelector('div'))

        let option = {
            geo: {
                type: 'map',
                map: 'china', 
                roam: true, // 设置允许缩放以及拖动的效果
                // label: {
                //     show: true  // 展示标签
                // },
                zoom: 1, // 设置初始化的缩放比例
                // center: [87.617733,43.792818]   // 设置地图中心点的坐标
            },
            series: [
                {
                    data: airData,
                    geoIndex: 0,    // 将空气质量的数据和第0个geo配置关联起来
                    type: 'map'
                }, 
                {
                    data: scatterData,   // 配置散点的坐标数据
                    type: 'effectScatter',
                    coordinateSystem: 'geo', // 指明散点使用的坐标系统   geo的坐标系统
                    rippleEffect: {
                        scale: 10   // 设置涟漪动画的缩放比例
                    }
                }
            ],
            visualMap: {
                min: 0,
                max: 300,
                inRange: {
                    color: ['white', 'red'] // 控制颜色渐变的范围
                },
                calculable: true    // 出现滑块
            }
        }

        myChart.setOption(option)
    </script>
</body>
</html>
```

#### 6.雷达图

- 实现步骤
  + 定义各个维度的最大值
  + 定义图表的类型 `radar`

- 常用配置
  + 显示数值：`label`
  + 区域面积： `areaStyle`
  + 绘制类型： `shape`

- 雷达图的特点
  - 雷达图可以用来分析多个维度的数据与标准数据的对比情况

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>雷达图的实现</title>
    <script src="./lib/echarts.min.js"></script>
</head>
<body>
    <div style="width: 600px; height: 400px;"></div>
    <script>
        const myChart = echarts.init(document.querySelector('div'))

        // 各个维度的最大值
        const dataMax = [{
            name: '易用性',
            max: 100
        },{
            name: '功能',
            max: 100
        },{
            name: '拍照',
            max: 100
        },{
            name: '跑分',
            max: 100
        },{
            name: '续航',
            max: 100
        }]

        let option = {
            radar: {    // 配置各个维度的最大值
                indicator: dataMax,
                shape: 'polygon'    // 配置雷达图最外层的图形 circle palygon
            },
            series: [{
                type: 'radar',  // radar 此图表是一个雷达图
                label: {    // 设置标签的样式
                    show: true  // 显示数值
                },
                areaStyle: {},  //  将每一个产品的雷达图形成阴影的面积
                data: [{
                    name: '华为手机1',
                    value: [80, 90, 80, 82, 90]
                },{
                    name: '中兴手机1',
                    value: [70, 82, 75, 70, 78]
                }]
            }]
        }

        myChart.setOption(option)
    </script>
</body>
</html>
```

#### 7.仪表盘

- 仪表盘的实现步骤
  + 准备数据
  + 定义图表的类型 `gauge`

- 常见效果
  + 数值范围： `max,min`
  + 多个指针： 增加 `data` 中的数组元素
  + 多个指针颜色差异： `itemStyle`

- 仪表盘的特点
  - 仪表盘可以更直观的表现出某个指标的<font color="#f66">进度</font>或实际情况

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>仪表盘的实现</title>
    <script src="./lib/echarts.min.js"></script>
</head>
<body>
    <div style="width: 600px; height: 400px;"></div>
    <script>
        const myChart = echarts.init(document.querySelector('div'))

        let option = {
            series: [{
                type: 'gauge',
                data: [{
                    value: 97,
                    itemStyle: {    // 指针样式
                        color: 'red'    // 指针颜色
                    }
                },{
                    value: 85,
                    itemStyle: {
                        color: 'blue'
                    }
                }],  // 每一个对象就代表一个指针
                min: 50,    // min max 控制仪表盘的数值范围
                max: 101
            }]
        }

        myChart.setOption(option)
    </script>
</body>
</html>
```

### 主题

- 内置主题

  + `ECharts` 中默认内置了两套主题： `light dark`

  + 在初始化对象方法 `init` 中可以指明

    ```js
    const chart = echarts.init(dom,'light')
    const chart = echarts.init(dom, 'dark')
    ```

- 自定义主题
  1. 在主题编辑器中 [<font color=#f99>编辑主题</font>](https://echarts.apache.org/zh/theme-builder.html)
  2. 下载主题，是一个 `js` 文件
  3. 引入主题 `js` 文件
  4. 在 `init` 方法中使用主体

### 调色盘

它是一组颜色，图形、系列会自动从其中选择颜色。(调色盘的作用遵循就近原则)

- 主题调色盘

- 全局调色盘

  ```json
  option: {
      color: ['red','green','blue']
  }
  ```

- 局部调色盘

  ```json
  series: [{
      type: 'bar',
      color: ['red','green','blue']
  }]
  ```

颜色渐变

- 线性渐变

  ```json
  color: {
      type: 'linear',
      x: 0,
      y: 0,
      x2: 0,
      y2: 1,
      colorStops: [{
          offset: 0,color: 'red'	// 0% 处的颜色
      },{
        offset: 1, color: 'blue'	// 100% 处的颜色  
      }],
      globalCoord: false	// 缺省为 false
  }
  ```

- 径向渐变

  ```json
  color: {
      type: 'radial',
      x: 0.5,
      y: 0.5,
      r: 0.5,
      colorStops: [{
          offset: 0, color: 'red'	// 0% 处的颜色
      }, {
          offset: 1, color: 'blue'	// 100% 处的颜色
      }],
      global: false	// 缺省为 false
  }
  ```

### 样式

- 直接样式： `itemStyle、textStyle、lineStyle、areaStyle、label`
- 高亮样式： 在 `emphasis` 中包裹 `itemStyle、textStyle、lineStyle、areaStyle、label`

### 自适应

当浏览器的大小发生变化的时候，如果想让图表也能随之适配变化

1. 监听窗口大小变化事件

2. 在事件处理函数中调用 `ECharts` 实例对象的 `resize` 即可

   ```js
   window.onresize = funciton() {
       myChart.resize()
   }
   ```

   