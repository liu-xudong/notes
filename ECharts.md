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

