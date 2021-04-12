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

