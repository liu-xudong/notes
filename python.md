# Python

## Sublime Text

按住 `ctrl + shift + p` ，输入 `install` ，选择 `install package` 

### 安装

> `ChineseLoremlpsum` ： 中文插件
>
> `Anaconda` ： 自动匹配关键字等使用功能，有效提高开发效率
>
> `SublimeREPL` ： 直接运行当前文件，方便调试
>
> `AutoPep8` ： python 开发规范 pep8
>
> [软件风格网站](https://packagecontrol.io/)

## 格式化输出

```python
person = '大圣哥'
address = '西安市莲湖区唐延路唐延鑫苑'
phone = '17609200438'
num = 5

print('订单的收件人是：%s，收货地址是：%s，联系方式是：%s，商品数量是：%s' %(person,address,phone,num))

age = 18.5	# int(18.5) ---> 18 取整数
print('年龄是：%d' % age)

year = 2019
print('今年是：%02d' %year)


# %f  小数点后面的位数 而且是四舍五入
salary = 8899.32
print('我的薪水是：%.1f' % salary)

# %s 字符串
# %d 数字
# %f  小数点后面的位数 而且是四舍五入
```

