# TypeScript

## TypeScript 是什么
TypeScript 是一种由微软开发的自由和开源的编程语言。它是 JavaScript 的一个超集，而且本质上向这个语言添加了可选的静态类型和基于类的面向对象编程。

TypeScript 提供最新的和不断发展的 JavaScript 特性，包括那些来自 2015 年的 ECMAScript 和未来的提案中的特性，比如异步功能和 Decorators，以帮助建立健壮的组件。

### TypeScript 与 JavaScript 的区别
| TypeScript | JavaScript |
| :---: | :---: |
| JavaScript 的超集用于解决大型项目的代码复杂性 | 一种脚本语言，用于创建动态网页 |
| 可以在编译期间发现并纠正错误 | 作为一种解释型语言，只能在运行时发现错误 |
| 强类型，支持静态和动态类型 | 弱类型，没有静态类型选项 |
| 最终被编译成 JavaScript 代码，使浏览器可以理解 | 可以直接在浏览器中使用 |
| 支持模块、泛型和接口 | 不支持模块，泛型或接口 |
| 社区的支持仍在增长，而且还不是很大 | 大量的社区支持以及大量文档和解决问题的支持|

## TypeScript 基础类型
**1. Boolean 类型**
```typescript
let isDone: boolean = false;
// ES5：var isDone = false;
```
**2. Number 类型**
```typescript
let count: number = 10;
// ES5：var count = 10;
```
**3. String 类型**
```typescript
let name: string = "semliker";
// ES5：var name = 'semlinker';
```
**4. Symbol 类型**
```typescript
const sym = Symbol();
let obj = {
  [sym]: "semlinker",
};
console.log(obj[sym]); // semlinker
```
**5. Array 类型**
```typescript
let list: number[] = [1, 2, 3];
// ES5：var list = [1,2,3];

let list: Array<number> = [1, 2, 3]; // Array<number>泛型语法
// ES5：var list = [1,2,3];
```
**6. Enum 类型**  
使用枚举我们可以定义一些带名字的常量。 使用枚举可以清晰地表达意图或创建一组有区别的用例。 TypeScript 支持数字的和基于字符串的枚举。
> 6.1 数字枚举
> ```typescript
> enum Direction {
>   NORTH,
>   SOUTH,
>   EAST,
>   WEST,
> }
> 
> let dir: Direction = Direction.NORTH;
> ```
> 默认情况下，NORTH 的初始值为 0，其余的成员会从 1 开始自动增长。换句话说，Direction.SOUTH 的值为 1，Direction.EAST 的值为 2，Direction.WEST 的值为 3。
> 
> 以上的枚举示例经编译后，对应的 ES5 代码如下：
> ```javascript
> "use strict";
> var Direction;
> (function (Direction) {
>   Direction[(Direction["NORTH"] = 0)] = "NORTH";
>   Direction[(Direction["SOUTH"] = 1)] = "SOUTH";
>   Direction[(Direction["EAST"] = 2)] = "EAST";
>   Direction[(Direction["WEST"] = 3)] = "WEST";
> })(Direction || (Direction = {}));
> var dir = Direction.NORTH;
> ```
> 当然我们也可以设置 NORTH 的初始值，比如：
> ```typescript
> enum Direction {
>   NORTH = 3,
>   SOUTH,
>   EAST,
>   WEST,
> }
> ```

> 6.2 字符串枚举
> 在 TypeScript 2.4 版本，允许我们使用字符串枚举。在一个字符串枚举里，每个成员都必须用字符串字面量，或另外一个字符串枚举成员进行初始化。
> ```typescript
> enum Direction {
>   NORTH = "NORTH",
>   SOUTH = "SOUTH",
>   EAST = "EAST",
>   WEST = "WEST",
> }
> ```
> 以上代码对应的 ES5 代码如下：
> ```javascript
> "use strict";
> var Direction;
> (function (Direction) {
>   Direction["NORTH"] = "NORTH";
>   Direction["SOUTH"] = "SOUTH";
>   Direction["EAST"] = "EAST";
>   Direction["WEST"] = "WEST";
> })(Direction || (Direction = {}));
> ```
> 通过观察数字枚举和字符串枚举的编译结果，我们可以知道数字枚举除了支持 从成员名称到成员值 的普通映射之外，它还支持 从成员值到成员名称 的反向映射：
> ```typescript
> enum Direction {
>   NORTH,
>   SOUTH,
>   EAST,
>   WEST,
> }
> 
> let dirName = Direction[0]; // NORTH
> let dirVal = Direction["NORTH"]; // 0
> ```
> 另外，对于纯字符串枚举，我们不能省略任何初始化程序。而数字枚举如果没有显式设置值时，则会使用默认规则进行初始化。  
**7. Any 类型**  
**8. Unknown 类型**  
**9. Tuple 类型**  
**10. Void 类型**  
**11. Null 和 Undefined 类型**  
**12. object, Object 和 {} 类型**  
**13. Never 类型**  
