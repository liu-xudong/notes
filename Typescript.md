# Typescript

## 基础变量

- 布尔值

  最基本的数据类型就是 `true/false` 值，**boolean**。

  ```typescript
  let isDone: boolean = false
  ```

- 数字

  Typescript 中的所有数字都是浮点数，这些浮点数的类型是 **number**。除了支持十进制和十六进制字面量，Typescript还支持ECMAScript 2015中引入的二进制和八进制字面量。

  ```typescript
  let decLiteral: number = 6
  let hexLiteral: number = 0xf00d
  let binaryLiteral: number = 0b1010
  let octalLiteral: number = 0o744
  ```

- 字符串

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

- 数组

  

- 元组 Tuple

- 枚举

- Any

- Void

- Null 和 Undefined

- Never

- Object

- 类型断言

