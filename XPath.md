## XPath

### XPath常用规则

|  表达式  |           描述           |
| :------: | :----------------------: |
| nodename |  选取此节点的所有子节点  |
|    /     | 从当前节点选取直接子节点 |
|    //    |  从当前节点选取子孙节点  |
|    .     |       选取当前节点       |
|    ..    |   选取当前节点的父节点   |
|    @     |         选取属性         |

常用匹配规则示例：

```python
'//title[@lang='eng']'
```

### 所有节点

```python
'//* '
```

*代表匹配所有节点

### 父节点

```python
# 获得 href 属性为 link4.html 的 a 节点的父节点的 class 属性

'//a[@href="link4.html"]/../@class'  

'//a[@href="link4.html"]/parent::*/@class'
```

### 属性匹配

```python
'//li[@class="item-inactive"]'
```

### 文本获取

```python
'//li[@class="item-0"]/a/text()'
```

### 属性获取

```python
'//li/a/@href'
```

### 属性多值匹配

```python
'//li[contains(@class, "li")]/a/text()'
```

### 多属性匹配

```python
'//li[contains(@class, "li") and @name="item"]/a/text()'
```

### XPath运算符

| 运算符 | 描述           | 实例              | 返回值                               |
| :----- | :------------- | :---------------- | :----------------------------------- |
| or     | 或             | age=18 or age=20  | age=18：True；age=21：False          |
| and    | 与             | age>18 and age<21 | age=20：True；age=21：False          |
| mod    | 计算除法的余数 | 5 mod 2           | 1                                    |
| \|     | 计算两个节点集 | //book \| //cd    | 返回所有拥有 book 和 cd 元素的节点集 |
| +      | 加法           | 5 + 3             | 8                                    |
| -      | 减法           | 5 - 3             | 2                                    |
| *      | 乘法           | 5 * 3             | 15                                   |
| div    | 除法           | 8 div 4           | 2                                    |
| =      | 等于           | age=19            | 判断简单，不再赘述                   |
| !=     | 不等于         | age!=19           | 判断简单，不再赘述                   |
| <      | 小于           | age<19            | 判断简单，不再赘述                   |
| <=     | 小于等于       | age<=19           | 判断简单，不再赘述                   |
| >      | 大于           | age>19            | 判断简单，不再赘述                   |
| >=     | 大于等于       | age>=19           | 判断简单，不再赘述                   |

### 按续选择

```python
# 获取第一个
	'//li[1]/a/text()'
# 获取最后一个
	'//li[last()]/a/text()'
# 获取前两个
	'//li[position()<3]/a/text()'      
# 获取倒数第三个
	'//li[last()-2]/a/text()'
```

### 节点轴选择

```python
# 获取所有祖先节点
	'//li[1]/ancestor::*
# 获取 div 祖先节点
	'//li[1]/ancestor::div'
# 获取当前节点所有属性值
	'//li[1]/attribute::*'
# 获取 href 属性值为 link1.html 的直接子节点
	'//li[1]/child::a[@href="link1.html"]'
# 获取所有的的子孙节点中包含 span 节点但不包含 a 节点
	'//li[1]/descendant::span'
# 获取当前所有节点之后的第二个节点
	'//li[1]/following::*[2]'
# 获取当前节点之后的所有同级节点
	'//li[1]/following-sibling::*'
```

| 轴名称             | 结果                                                   |
| ------------------ | ------------------------------------------------------ |
| ancestor           | 选取当前节点的所有先辈（父、祖父等）                   |
| ancestor-or-self   | 选取当前节点的所有先辈（父、祖父等）以及当前节点本身   |
| attribute          | 选取当前节点的所有属性                                 |
| child              | 选取当前节点的所有子元素                               |
| descendant         | 选取当前节点的所有后代元素（子、孙等）                 |
| descendant-or-self | 选取当前节点的所有后代元素（子、孙等）以及当前节点本身 |
| following          | 选取文档中当前节点的结束标签之后的所有节点             |
| namespace          | 选取当前节点的所有命名空间节点                         |
| parent             | 选取当前节点的父节点                                   |
| preceding          | 选取文档中当前节点的开始标签之前的所有节点             |
| preceding-sibling  | 选取当前节点之前的所有同级节点                         |
| self               | 选取当前节点                                           |