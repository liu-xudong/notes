# Typescript

## 基础变量

### 布尔值

最基本的数据类型就是 `true/false` 值，**boolean**。

```typescript
let isDone: boolean = false
```

### 数字

Typescript 中的所有数字都是浮点数，这些浮点数的类型是 **number**。除了支持十进制和十六进制字面量，Typescript还支持ECMAScript 2015中引入的二进制和八进制字面量。

```typescript
let decLiteral: number = 6
let hexLiteral: number = 0xf00d
let binaryLiteral: number = 0b1010
let octalLiteral: number = 0o744
```

### 字符串

**string** 表示文本数据类型，可以使用双引号（ “ ）或单引号（ ‘ ）表示字符串。

```typescript
let name:string = "bob"
name = "smith"
```

**模板字符串**，可以定义多行文本和内嵌表达式。这种字符串是被反引号包围（ ` ），并且以 *${ expr }*这种形式嵌入表达式

```typescript
let name: string = `Gene`
let age: number = 37
let sentence: string = `Hello, my name is ${ name }.

I'll be ${ age + 1 } years old next month.`
```

这与下面定义的 `sentence` 相同效果

```typescript
let sentence: string = "Hello, my name is " + name + ".\n\n" +
    "I'll be " + (age + 1) + "years old next month."
```

### 数组

TypeScript有两种方式定义数组。第一种，可以在元素类型后面接上 `[]` ，表示由此类型元素组成的一个数组：

```typescript
let list: number[] = [1,2,3]
```

第二中方式是使用数组泛型，Array<元素类型>：

```typescript
let list: Array<number> = [1,2,3]
```

### 元组 Tuple

元组类型允许表示一个已知元素数量和类型的数组，各元素的类型不必相同。比如，你可以定义一对值分别为 **string** 和 **number** 类型的元组。

```typescript
let x: [string,number]
x = ['hello',10]	// OK
x = [10,'hello']	// Error
```

当访问一个已知索引的元素，会得到正确的类型：

```typescript
console.log(x[0].substr(1))	// OK
console.log(x[1].substr(1))	// Error,'number' does not have 'substr'
```

当访问一个越界的元素，会使用联合类型替代：

```typescript
x[3] = 'world'	// OK,字符串可以赋值给（string | number)类型

console.log(x[5].toString())	// OK,'string' 和 'number' 都有 toString

x[6] = true	// Error,布尔不是(string | number)类型
```

### 枚举

**enum** 类型是对JavaScript标准数据类型的补充。像C#等其它语言一样，使用枚举类型可以为一组数值赋予友好的名字。

```typescript
enum Color {Red, Green, Blue}
let c: Color = Color.Green
```

默认情况下，从 `0` 开始为元素编号。你也可以手动的指定成员的数值。例如，我们将上面的例子改成从 `1` 开始编号：

```typescript
enum Color {Red = 1, Green, Blue}
let c: Color = Color.Green
```

或者，全部都采用手动赋值：

```typescript
enum Color {Red = 1, Green = 2, Blue = 4}
let c: Color = Color.Green
```

枚举类型提供的一个便利是你可以由枚举的值得到它的名字。例如，我们知道数值为2，但不确定它映射到Color里的哪个名字，我们可以查找相应的名字：

```typescript
enum Color {Red = 1, Green, Blue}
let colorName: string = Color[2]

console.log(colorName)	// 显示'Green'因为上面代码里它的值是2
```

### Any

有时候，我们会想要为那些在编程阶段还不清楚类型的变量指定一个类型。 这些值可能来自于动态的内容，比如来自用户输入或第三方代码库。 这种情况下，我们不希望类型检查器对这些值进行检查而是直接让它们通过编译阶段的检查。 那么我们可以使用 **any** 类型来标记这些变量:

```typescript
let notSure: any = 4
notSure = 'maybe a string instead'
notSure = false	// okay,definitely a boolean
```

在对现有代码进行改写的时候，`any`类型是十分有用的，它允许你在编译时可选择地包含或移除类型检查。 你可能认为 `Object`有相似的作用，就像它在其它语言中那样。 但是 `Object`类型的变量只是允许你给它赋任意值 - 但是却不能够在它上面调用任意的方法，即便它真的有这些方法:

```typescript
let notSure: any = 4
notSure.ifItExists()	// okay, ifItExists might exist at runtime
notSure.toFixed()	// okay, toFixed exists (but the compiler doesn't check)

let prettySure: Object = 4
prettySure.toFixed()	// Error: Property 'toFixed' doesn't exist on type 'Object'.
```

当你只知道一部分数据类型时，`any` 类型也是有用的。比如，你有一个数组，它包含了不同类型的数据：

```typescript
let list: any[] = [1, true, 'free']

list[1] = 100
```

### Void

某种程度上来说，`void` 类型像是与 `any` 类型相反，它表示没有任何类型。当一个函数没有返回值时，你通常会见到其返回值类型是**void**:

```typescript
function warnUser(): void {
    console.log('This is my warning message')
}
```

声明一个 `void` 类型变量没有什么大用，因为你只能为它赋予 `undefined` 和 `null` :

```typescript
let unusable: void = undefined
```

### Null 和 Undefined

TypeScript中， `undefined` 和 `null` 两者各自有自己的类型分别叫做 **undefined** 和 **null** 。 和 `void` 相似，它们的本身的类型用处不是很大:

```typescript
let u: undefined = undefined
let n: null = null
```

默认情况下 `null` 和 `undefined` 是所有类型的子类型。 就是说你可以把  `null `和 `undefined` 赋值给 `number` 类型的变量。

然而，当你指定了 `--strictNullChecks` 标记， `null` 和 `undefined `只能赋值给 `void` 和它们各自。 这能避免 *很多*常见的问题。 也许在某处你想传入一个   `string` 或 `null` 或 `undefined`，你可以使用联合类型 `string | null | undefined`。

### Never

**never**类型表示的是那些永不存在的值的类型。 例如， `never`类型是那些总是会抛出异常或根本就不会有返回值的函数表达式或箭头函数表达式的返回值类型； 变量也可能是 `never`类型，当它们被永不为真的类型保护所约束时。

`never`类型是任何类型的子类型，也可以赋值给任何类型；然而，*没有*类型是`never`的子类型或可以赋值给`never`类型（除了`never`本身之外）。 即使 `any`也不可以赋值给`never`。

下面是一些返回`never`类型的函数：

```typescript
// 返回 never 的函数必须存在无法达到的终点
function error(message: string): never {
    throw new Error(message)
}

// 推断的返回值类型为 never
function fail() {
    return error('Something failed')
}

// 返回 never 的函数必须存在无法达到的终点
function infiniteLoop(): never {
    while(true) {}
}
```

### Object

**object**表示非原始类型，也就是除`number`，`string`，`boolean`，`symbol`，`null`或`undefined`之外的类型。

使用`object`类型，就可以更好的表示像`Object.create`这样的API。例如：

```typescript
declare function create(o: object | null): void

create({ prop: 0})	// OK
create(null)	// OK

create(42)	// Error
create('string')	// Error
create(false) 	// Error
create(undefined) // Error
```

### 类型断言

有时候你会遇到这样的情况，你会比TypeScript更了解某个值的详细信息。 通常这会发生在你清楚地知道一个实体具有比它现有类型更确切的类型。

通过*类型断言*这种方式可以告诉编译器，“相信我，我知道自己在干什么”。 类型断言好比其它语言里的类型转换，但是不进行特殊的数据检查和解构。 它没有运行时的影响，只是在编译阶段起作用。 TypeScript会假设你，程序员，已经进行了必须的检查。

类型断言有两种形式。 其一是“尖括号”语法:

```typescript
let someValue: any = 'this is a string'

let strLength; number = (<string>someValue).length
```

另一个为 `as` 语法：

```typescript
let someValue: any = 'this is a string'

let strLength: number = (someValue as string).length
```

两种形式是等价的。当你在TypeScript里使用JSX时，只有 `as` 语法断言是被允许的。

## 变量声明

### `var` 声明

一直以来我们都是通过 `var` 关键字定义 JavaScript 变量。

```typescript
var a = 10
```

我们也可以在函数内部定义变量：

```typescript
function f() {
    var message = 'Hello,world!'
    
    return message
}
```

并且我们也可以在其它函数内部访问相同的变量。

```typescript
function f() {
    var a = 10
    return function g() {
        var b = a + 1
        return b
    }
}

var g = f()
g()	// return 11
```

上面的例子中，`g` 可以获取到 `f` 函数里定义的 `a` 变量。每当 `g` 被调用时，它都可以访问到 `f` 里的 `a` 变量。即使当 `g` 在 `f` 已经执行完后才被调用，它仍然可以访问及修改 `a` 。

```typescript
function f() {
    var a = 1
    
    a = 2
    var b = g()
    a = 3
    
    return b
    
    function g() {
        return a
    }
}

f()	// return 2
```

#### 作用于规则

对于熟悉其它语言的人来说， `var` 声明有些奇怪的作用域规则。看下面的例子：

```typescript
function f(shouldInitialize: boolean) {
    if(shouldInitialize) {
        var x = 10
    }
    
    return x
}

f(true)	// return '10'
f(false)	// return 'undefined'
```

变量 `x`是定义在*`if`语句里面*，但是我们却可以在语句的外面访问它。 这是因为 `var`声明可以在包含它的函数，模块，命名空间或全局作用域内部任何位置被访问（我们后面会详细介绍），包含它的代码块对此没有什么影响。 有些人称此为* `var`作用域*或*函数作用域*。 函数参数也使用函数作用域。

这些作用域规则可能会引发一些错误。 其中之一就是，多次声明同一个变量并不会报错:

```typescript
function sumMatrix(matris: number[][]) {
    var sum = 0
    for(var i = 0; i < matrix.length; i++) {
        var currentRow = matrix[i]
        for(var i = 0; i < currentRow.length; i++) {
            sum += currentRow[i]
        }
    }
}
```

这里很容易看出一些问题，里层的`for`循环会覆盖变量`i`，因为所有`i`都引用相同的函数作用域内的变量。 有经验的开发者们很清楚，这些问题可能在代码审查时漏掉，引发无穷的麻烦。

#### 捕获变量怪异之处

下面代码会返回什么：

```typescript
for (var i = 0; i < 10; i++) {
    setTimeout(function() { console.log(i) },100 * i)
}
```

`setTimeout` 会在若干毫秒的延时后执行一个函数（等待其他代码执行完毕）。

结果如下： 

```typescript
10
10
10
10
10
10
10
10
10
10
```

期望的结果是：

```typescript
0
1
2
3
4
5
6
7
8
9
```

> 我们传给 `setTimeout` 的每一个函数表达式实际上都引用了相同作用域里的同一个 `i` 。

`setTimeout` 在若干毫秒后执行一个函数，并且是在 `for` 循环结束后。`for` 循环结束后， `i` 的值为 `10` 。所以当函数被调用的时候，它会打印出 `10` ！

一个通常的解决办法是使用立即执行的函数表达式（IIFE）来捕获每次迭代时 `i` 的值：

```typescript
for (var i = 0; i < 10; i++) {
    (function(i) {
        setTimeout(function() { console.log(i) }, 100 * i)
    })(i)
}
```

参数 `i` 会覆盖 `for` 循环里的 `i` 。

### `let` 声明

`let` 与 `var` 的写法一致。

```typescript
let hello = 'Hello!'
```

主要的区别不在语法上，而是语义。

#### 块作用域

当用 `let` 声明一个变量，它使用的是 *词法作用域*  或 *块作用域*  。不同于使用 `var` 声明的变量那样可以在包含它们的函数外访问，块作用域变量在包含它们的块或 `for` 循环外是不能访问的。

```typescript
function f(input: boolean) {
    let a = 100
    
    if(input) {
        let b = a + 1	// Still okay to reference 'a'
        return b 
    }
    
    return b	// Error: 'b' doesn't exist here
}
```

这里我们定义了2个变量 `a` 和 `b` 。 `a` 的作用域是 `f` 函数体内，而 `b` 的作用域是 `if` 语句块里。

在 `catch` 语句里声明的变量也具有同样的作用域规则。

```typescript
try {
    throw 'oh no!'
}
catch(e) {
    console.log('Oh well.')
}

console.log(e)	// Error: 'e' doesn't exist here
```

拥有块级作用域的变量的另一个特点是，它们不能在被声明之前读或写。虽然这些变量始终“存在”于它们的作用域里，但在直到声明它的代码之前的区域都属于 *暂时性死区* 。它只是用来说明我们不能在 `let` 语句之前访问它们，幸运的是Typescript可以告诉我们这些信息。

```typescript
a++	// illegal to use 'a' before it's declared
let a
```

注意一点，我们任然可以在一个拥有块作用域变量被声明前 *获取它* 。只是我们不能在变量声明前去调用那个函数。

``` 
function foo() {
    return a	// okay to capture 'a'
}

// 不能在'a'被声明前调用'foo'
// 运行时应该抛出错误
foo()

let a
```

#### 重定义及屏蔽

`var` 声明时，它不在乎你声明多少次，你只会得到1个。

```typescript
function f(x) {
    var x
    var x
    
    if(true) {
        var x
    }
}
```

在上边的例子中，所有 `x` 的声明实际上都引用一个相同的 `x` ，并且这是完全有效的代码。这经常会成为bug的来源。好的时 `let` 声明就不这么宽松了。

```typescript
let x = 10
let x = 20	// 错误，不能在1个作用域里多次声明`x`
```

并不是要求两个均时块级作用域的声明Typescript才会给出一个错误的警告。

```typescript
function f(x) {
    let x = 100	// error: interferes with parameter declaration
}

function g() {
    let x = 100
    var x = 100	// error; can't have both declarations of 'x'
}
```

并不是说块级作用域变量不能用函数作用域变量来声明。而是块级作用域变量需要在明显不同的块里声明。

```typescript
function f(condition, x) {
    if(condition) {
        let x = 100
        return x
    }
    
    return x
}

f(false, 0)	// return 0
f(true, 0)	// return 100
```

在一个嵌套作用域里引入一个新名字的行为称做屏蔽。它是一把双刃剑，它可能会不小心的引入新问题，同时也可能会解决一些错误。例如：

```typescript
function sumMatrix(matrix: number[][]) {
    let sum = 0
    for (let i = 0; i < matrix.length; i++) {
        var currentRow = matrix[i]
        for (let i = 0; i < currentRow.length; i++) {
            sum += currentRow[i]
        }
    }
    
    return sum
}
```

这个版本的循环能得到正确的结果，因为内层循环的 `i` 可以屏蔽掉外层循环的 `i` 。

#### 块级作用域变量的获取

每次进入一个作用域时，它创建了一个变量的 *环境* 。就算作用域内代码已经执行完毕，这个环境与其捕获的变量依然存在。

```typescript
function theCityThatAlwaysSleeps() {
    let getCity
    
    if(true) {
        let city = 'Seattle'
        getCity = function() {
            return city
        }
    }
    
    return getCity()
}
```

因为我们已经在 `city` 的环境里获取到了 `city` ，所以就算 `if` 语句执行结束后我们依然可以访问它。

当 `let` 声明出现在循环体里时拥有完全不同的行为。不仅是在循环里引入了一个新的变量，而是针对 *每次迭代* 都会创建这样一个新作用域。这就是我们在使用立即执行的函数表达式时做的事，所以在 `setTimeout` 例子里我们仅使用 `let` 声明就可以了。

```typescript
for (let i = 0; i <  10; i++) {
    setTimeout(function() { console.log(i) }, 100 * i)
}
```

结果为：

```typescript
0
1
2
3
4
5
6
7
8
9
```

### `const` 声明

`const` 声明是声明变量的另一种方式。

```typescript
const numLivesForCat = 9
```

它们与 `let` 声明相似，但是就像它的名字所表达的，它们被赋值后不能再改变。换句话说，它们拥有与 `let` 相同的作用域规则，但是不能对它们重新赋值。

它们引用的值是 *不可变的*

```typescript
const numLivesForCat = 9
const kitty = {
    name: 'Aurora',
    numLives: numLivesForCat
}

kitty = {
    name: 'Danielle',
    numLives: numLivesForCat
}	// Error

// all 'okay'
kitty.name = 'Rory'
kitty.name = 'Kitty'
kitty.name = 'Cat'
kitty.numLives--
```

除非你使用特殊的方法避免，实际上 `const` 变量的内部状态是可修改的。

### `let` VS. `const`

现在我们有两种作用域相似的声明方式，我们自然会问到底应该使用哪个。与大多数泛泛的问题一样，答案是：依情况而定。

所有变量除了你计划去修改的都应该使用 `const` 。基本原则就是如果一个变量不需要对它写入，那么其它使用这些代码的人都不能够写入它们，并且要思考为什么会需要对这些变量重新赋值。使用 `const` 也可以让我们更容易的推测数据的流动。

#### 解构

Another Typescript已经可以解析其他 ECMAscript 2015 特性了。

##### 解构数组

最简单的解构莫过于数组的结构赋值了：

```typescript
let input = [1, 2]
let [first, second] = input
console.log(first)	// outputs 1
console.log(second)	// outputs 2
```

这创建了2个命名变量 `first` 和 `second` 。相当于使用了索引，但更为方便：

```typescript
first = input[0]
second = input[1]
```

解构作用于已声明的变量会更好：

```typescript
// swap variables
[first, second] = [second, first] 
```

作用于函数参数：

```typescript
function f([first, second]: [number, number]) {
    console.log(first)
    console.log(second)
}
f(input)
```

你可以在数组里使用 `...` 语法创建剩余变量：

 ```typescript
let [first, ...rest] = [1, 2, 3, 4]
console.log(first)	// outputs 1
console.log(rest)	// outputs [2, 3, 4]
 ```

当然，由于是JavaScript， 你可以忽略你不关心的尾随元素：

```typescript
let [first] = [1, 2, 3, 4]
console.log(first)	// outputs 1
```

或其它元素：

```typescript
let [, second, , fourth] = [1, 2, 3, 4]
```

##### 对象结构

你也可以结构对象：

```typescript
let o = {
    a: "foo",
    b: 12,
    c: "bar"
}
let { a, b } = o
```

这通过 `o.a` and `o.b` 创建了 `a` 和`b` 。注意，如果你不需要 `c` 你可以忽略它。

就像数组结构，你可以用没有声明的赋值：

```typescript
({ a, b } = { a: "baz", b: 101 })
```

注意，我们需要用括号将他括起来，因为 JavaScript 通常会以 `{` 起始的语句解析为一个块

你可以在对象里使用 `...` 语法创建剩余变量：

```typescript
let { a, ...passthrough } = o
let total = passthrough.b + passthrough.c.length
```

##### 属性重命名

你也可以给属性以不同的名字：

```typescript
let { a: newName1, b: newName2 } = o
```

这里的语法开始变得混乱。你可以将 `a: newName1` 读作 “`a`  作为 `newName1` ”。方向是从左到右，好像你写成了以下样子：

```typescript
let newName1 = o.a
let newName2 = o.b
```

令人困惑的是，这里的冒号不是指示类型的。如果你想指定它的类型，仍然需要在其后写上完整的模式。

```typescript
let {a, b}: {a: string, b: string} = o
```

##### 默认值

默认值可以让你在属性为 `undefined` 时使用缺省值：

```typescript
function keepWholeObject(wholeObject: { a: string, b?: number }) {
    let { a, b = 1001 } = wholeObject
}
```

现在，即使 `b` 为 `undefined` , `keepWholeObject` 函数的变量 `wholeObject` 的属性 `a`  和`b` 都会有值。

##### 函数声明

解构也能用于函数声明。看以下简单的情况：

```typescript
type c = { a: string, b?: number }
function f({ a, b }: c): void {
    // ...
}
```

但是，通常情况下更多的是指默认值，结构默认值有些棘手。首先，你需要在默认值之前设置其格式。

```typescript
function f( { a="", b=0 } = { a: "" }): void {
    // ...
}
f({ a: "yes" })	// ok, default b = 0
f()		// ok, default to {a: ""}, which then defaults b = 0
f({}) 	// error, 'a' is required if you supply an argument
```

要小心使用解构。从前面的例子中可以看出，就算是最简单的解构表达式也是难以理解的。尤其当存在深层嵌套解构的时候，就算这时没有堆叠在一起的重命名，默认值和类型注解，也是令人难以理解。解构表达式要尽量保持小而简单。你自己也可以直接使用解构将会生成的赋值表达式。

##### 展开

展开操作符正与解构相反。它允许你将一个数组展开为另一个数组，或将一个对象展开为另一个对象。例如：

```typescript
let first = [1, 2]
let second = [3, 4]
let bothPlus = [0, ...first, ...second, 5]
```

这会令 `bothPlus` 的值为 `[0, 1, 2, 3, 4, 5]` 。展开操作创建 `first` 和 `second` 的一份浅拷贝。它们不会被展开操作所改变。

你还可以展开对象：

```typescript
let defaults = { food: "spicy", price: "$$", ambiance: "noisy" }
let search = { ...defaults, food: "rich" }
```

`search` 的值为 `{ food: "rich", price: "$$", ambiance: "noisy" }` 。对象的展开比数组的展开要复杂的多。像数组展开一样，它是从左至右进行处理，但结果仍为对象。这意味着出现在展开对象后面的属性会覆盖前面的属性。因此，如果我们修改上面的例子，在结尾处进行展开的话：

```typescript
let defaults = { food: "spicy", price: "$$", ambiance: "noisy" }
let search = { food: "rich", ...defaults}
```

那么，`defaults` 里的 `food` 属性会重写 `food: "rich"` ，在这里这并不是我们想要的结果。

对象展开还有其他一些意想不到的限制。首先，它仅包含对象 <font color=blue>自身的可枚举属性</font>。大体上是说当你展开一个对象实例时，你会丢失其它方法：

```typescript
class C {
    p = 12
    m() {
        
    }
}
let c = new	C()
let clone = [ ...c ]
clone.p	// ok
clone.m()	// error!
```

其次，TypeScript 编译器不允许展开泛型函数上的类型参数。这个特性会在TypeScript的未来版本中考虑实现。



