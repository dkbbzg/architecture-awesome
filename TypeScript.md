# TypeScript

## 一、TypeScript 是什么
TypeScript 是一种由微软开发的自由和开源的编程语言。它是 JavaScript 的一个超集，而且本质上向这个语言添加了可选的静态类型和基于类的面向对象编程。

TypeScript 提供最新的和不断发展的 JavaScript 特性，包括那些来自 2015 年的 ECMAScript 和未来的提案中的特性，比如异步功能和 Decorators，以帮助建立健壮的组件。

### TypeScript 与 JavaScript 的区别
|                   TypeScript                   |                 JavaScript                 |
| :--------------------------------------------: | :----------------------------------------: |
| JavaScript 的超集用于解决大型项目的代码复杂性  |       一种脚本语言，用于创建动态网页       |
|          可以在编译期间发现并纠正错误          |  作为一种解释型语言，只能在运行时发现错误  |
|           强类型，支持静态和动态类型           |          弱类型，没有静态类型选项          |
| 最终被编译成 JavaScript 代码，使浏览器可以理解 |           可以直接在浏览器中使用           |
|              支持模块、泛型和接口              |           不支持模块，泛型或接口           |
|       社区的支持仍在增长，而且还不是很大       | 大量的社区支持以及大量文档和解决问题的支持 |

## 二、TypeScript 基础类型

### 1. Boolean 类型  
```typescript
let isDone: boolean = false;
// ES5：var isDone = false;
```

### 2. Number 类型  
```typescript
let count: number = 10;
// ES5：var count = 10;
```

### 3. String 类型  
```typescript
let name: string = "semliker";
// ES5：var name = 'semlinker';
```

### 4. Symbol 类型  
```typescript
const sym = Symbol();
let obj = {
  [sym]: "semlinker",
};
console.log(obj[sym]); // semlinker
```

### 5. Array 类型  
```typescript
let list: number[] = [1, 2, 3];
// ES5：var list = [1,2,3];

let list: Array<number> = [1, 2, 3]; // Array<number+泛型语法
// ES5：var list = [1,2,3];
```

### 6. Enum 类型  
使用枚举我们可以定义一些带名字的常量。 使用枚举可以清晰地表达意图或创建一组有区别的用例。 TypeScript 支持数字的和基于字符串的枚举。

+ **6.1 数字枚举**  
  ```typescript
  enum Direction {
    NORTH,
    SOUTH,
    EAST,
    WEST,
  };
  
  let dir: Direction = Direction.NORTH;
  ```
  默认情况下，NORTH 的初始值为 0，其余的成员会从 1 开始自动增长。换句话说，Direction.SOUTH 的值为 1，Direction.EAST 的值为 2，Direction.WEST 的值为 3。

  以上的枚举示例经编译后，对应的 ES5 代码如下：
  ```javascript
  "use strict";
  var Direction;
  (function (Direction) {
    Direction[(Direction["NORTH"] = 0)] = "NORTH";
    Direction[(Direction["SOUTH"] = 1)] = "SOUTH";
    Direction[(Direction["EAST"] = 2)] = "EAST";
    Direction[(Direction["WEST"] = 3)] = "WEST";
  })(Direction || (Direction = {}));
  
  var dir = Direction.NORTH;
  ```
  当然我们也可以设置 NORTH 的初始值，比如：
  ```typescript
  enum Direction {
    NORTH = 3,
    SOUTH,
    EAST,
    WEST,
  }
  ```

+ **6.2 字符串枚举**  
  在 TypeScript 2.4 版本，允许我们使用字符串枚举。在一个字符串枚举里，每个成员都必须用字符串字面量，或另外一个字符串枚举成员进行初始化。
  ```typescript
  enum Direction {
    NORTH = "NORTH",
    SOUTH = "SOUTH",
    EAST = "EAST",
    WEST = "WEST",
  }
  ```
  以上代码对应的 ES5 代码如下：
  ```javascript
  "use strict";
  var Direction;
  (function (Direction) {
    Direction["NORTH"] = "NORTH";
    Direction["SOUTH"] = "SOUTH";
    Direction["EAST"] = "EAST";
    Direction["WEST"] = "WEST";
  })(Direction || (Direction = {}));
  ```
  通过观察数字枚举和字符串枚举的编译结果，我们可以知道数字枚举除了支持 从成员名称到成员值 的普通映射之外，它还支持 从成员值到成员名称 的反向映射：
  ```typescript
  enum Direction {
    NORTH,
    SOUTH,
    EAST,
    WEST,
  }
  let dirName = Direction[0]; // NORTH
  let dirVal = Direction["NORTH"]; // 0
  ```
  另外，对于纯字符串枚举，我们不能省略任何初始化程序。而数字枚举如果没有显式设置值时，则会使用默认规则进行初始化。  

+ **6.3 常量枚举**  
  除了数字枚举和字符串枚举之外，还有一种特殊的枚举 —— 常量枚举。它是使用 const 关键字修饰的枚举，常量枚举会使用内联语法，不会为枚举类型编译生成任何 JavaScript。为了更好地理解这句话，我们来看一个具体的例子：
  ```typescript
  enum Direction {
    NORTH,
    SOUTH,
    EAST,
    WEST,
  }
  let dirName = Direction;
  ```
  以上代码对应的 ES5 代码如下：
  ```javascript
  "use strict";
  var dir = 0 /* NORTH */;
  ```

+ **6.4 异构枚举**  
  异构枚举的成员值是数字和字符串的混合：
  ```typescript
  enum Direction {
    A,
    B,
    C = "C",
    D = "D",
    E = 8,
    F,
  }
  ```
  以上代码对于的 ES5 代码如下：
  ```javascript
  "use strict";
  var Enum;
  (function (Enum) {
    Enum[Enum["A"] = 0] = "A";
    Enum[Enum["B"] = 1] = "B";
    Enum["C"] = "C";
    Enum["D"] = "D";
    Enum[Enum["E"] = 8] = "E";
    Enum[Enum["F"] = 9] = "F";
  })(Enum || (Enum = {}));
  ```
  通过观察上述生成的 ES5 代码，我们可以发现数字枚举相对字符串枚举多了 “反向映射”：
  ```javascript
  console.log(Enum.A) //输出：0
  console.log(Enum[0]) // 输出：A
  ```

### 7. Any 类型  
在 TypeScript 中，任何类型都可以被归为 `any` 类型。这让 `any` 类型成为了类型系统的顶级类型（也被称作全局超级类型）。
```typescript
let notSure: any = 666;
notSure = "semlinker";
notSure = false;
```
`any` 类型本质上是类型系统的一个逃逸舱。TypeScript允许我们对 `any` 类型的值执行任何操作，而无需事先执行任何形式的检查。比如：
```typescript
let value: any;

value.foo.bar; // OK
value.trim(); // OK
value(); // OK
new value(); // OK
value[0][1]; // OK
```
在许多场景下，这太宽松了。使用 `any` 类型，可以很容易地编写类型正确但在运行时有问题的代码。如果我们使用 `any` 类型，就无法使用TypeScript提供的大量的保护机制。为了解决 `any` 带来的问题，TypeScript 3.0引入了 `unknown` 类型。

### 8. Unknown 类型  
就像所有类型都可以赋值给 `any` ，所有类型也都可以赋值给 `unknown` 。这使得 `unknown` 成为TypeScript类型系统的另一种顶级类型（另一种是 `any` ）。
```typescript
let value: unknown;

value = true; //  OK
value = 42; //  OK
value = "Hello World"; // OK
value = []; //  OK
value = {}; //  OK
value = Math.random; // OK
value = null; //  OK
value = undefined; // OK
value = new TypeError(); // OK
value = Symbol("type"); //  OK
```
对 value 变量的所有赋值都被认为是类型正确的。但是，当我们尝试将类型为 `unknown` 的值赋值给其他类型的变量时会发生什么？
```typescript
let value: unknown;

let value1: unknown = value; // OK
let value2: any = value; // OK
let value3: boolean = value; // Error
let value4: number = value; // Error
let value5: string = value; // Error
let value6: object = value; // Error
let value7: any[] = value; // Error
let value8: Function = value; // Error
```
`unknown` 类型只能被赋值给 `any` 类型和 `unknown` 类型本身。直观地说，这是有道理的：只有能够保证任意类型值的容器才能保存 `unknown` 类型的值。毕竟我们不知道变量 value 中存储了什么类型的值。

对类型为 `unknown` 的值执行操作时：
```typescript
let value: unknown;

value.foo.bar; // Error
value.trim(); // Error
value(); // Error
new value(); // Error
value[0][1]; // Error
```
将 value 变量类型设置为 `unknown` 后，这些操作都不再被认为是类型正确的。通过将 `any` 类型改变为 `unknown` 类型，我们已将允许所有更改的默认设置，更改为禁止任何更改。

### 9. Tuple 类型  
众所周知，数组一般由同种类型的值组成，但有时我们需要在单个变量中存储不同类型的值，这时候我们就可以使用元组。在 JavaScript 中是没有元组的，元组是 TypeScript 中特有的类型，其工作方式类似于数组。

元组可用于定义具有有限数量的未命名属性的类型。每个属性都有一个关联的类型。使用元组时，必须提供每个属性的值。
```typescript
let tupleType: [string, boolean];
tupleType = ["semlinker", true];
```
与数组一样，可以通过下标来访问元组中的元素：
```typescript
console.log(tupleType[0]);  //  semliker
console.log(tupleType[1]);  //  true
```
在元组初始化的时候，如果出现类型不匹配的话，TypeScript编译器会提示错误信息：
```typescript
tupleType = [true, "semliker"];

[0]: Type 'true' is not assingable to type 'string'.
[1]: Type 'string' is not assingable to type 'boolean'.
```
这是因为类型不匹配导致的。在元组初始化的时候，我们必须提供每个属性的值，不然也会出现错误：
```typescript
tupleType = ["semliker"];

Property '1' is missing in type '[string]' but required in type '[string, boolean]'.
```

### 10. Void 类型  
某种程度上来说，`void` 类型像是 `any` 类型相反，它表示没有任何类型。当一个函数没有返回值时，通常会见到其返回值类型是 `void` ：
```typescript
//  声明返回函数值为void
function warnUser(): void {
  console.log("This is my warning message");
}
```
以上代码编译器生成的ES5代码如下：
```javascript
"use strict";
function warnUser() {
  console.log("This is my warning message");
}
```
需要注意的是，声明一个 `void` 类型的变量没有什么作用，因为在严格模式下，它的值只能为 `undefined` ：
```typescript
let unusable: void = undefined;
```

### 11. Null 和 Undefined 类型  
```typescript
let u: undefined = undefined;
let n: null = null;
```

### 12. object, Object 和 {} 类型  
+ **12.1 object 类型**  
  `object` 类型是：TypeScript 2.2 引入的新类型，它用于表示非原始类型。  
  ```typescript
  // node_modules/typescript/lib/lib.es5.d.ts
  interface ObjectConstructor {
    create(o: object | null): any;
    // ···
  }

  const proto = {};

  Object.create(proto); //  OK
  Object.create(null); //  OK
  Object.create(undefined); //  Error
  Object.create(1111);  //  Error
  Object.create(true);  //  Error
  Object.create("string");  //  Error
  ```
+ **12.2 Object 类型**  
  `Object`类型：它是所有`Object`类的实例的类型，它由以下两个接口来定义：

  - `Object`接口定义了`Object.prototype`原型对象上的属性。  
    ```typescript
    // node_modules/typescript/lib/lib.es5.d.ts
    interface Object {
      constructor: Function;
      toString(): string;
      toLocaleString(): string;
      valueOf(): Object;
      hasOwnProperty(v: PropertyKey): boolean;
      isPrototypeOf(v: Object): boolean;
      propertyIsEnumerable(v: PropertyKey): boolean;
    }
    ```

  - `ObjectConstructor`接口定义了`Object`类的属性。 
    ```typescript
    // node_modules/typescript/lib/lib.es5.d.ts
    interface ObjectConstructor {
      /** Invocation via `new` */
      new(value?: any): Object;
      /** Invocation via function calls */
      (value?: any): any;
      readonly prototype: Object;
      getPrototypeOf(o: any): any;
      // ···
    }

    declare var Object: ObjectConstructor;
    ``` 
    Object 类的所有实例都继承了 Object 接口中的所有属性。
+ **12.3 {} 类型**  
  {} 类型描述了一个没有成员的对象。当你试图访问这样一个对象的任意属性时，TypeScript 会产生一个编译时错误。
  ```typescript
  // Type {}
  const obj = {};

  // Error: Property 'prop' does not exist on type '{}'.
  obj.prop = "semlinker";
  ```
  但是，你仍然可以使用在 Object 类型上定义的所有属性和方法，这些属性和方法可通过 JavaScript 的原型链隐式地使用：
  ```typescript
  // Type {}
  const obj = {};

  // "[object Object]"
  obj.toString();
  ```

### 13. Never 类型  
`never` 类型表示的是那些永不存在的值的类型。 例如，`never` 类型是那些总是会抛出异常或根本就不会有返回值的函数表达式或箭头函数表达式的返回值类型。
```typescript
// 返回never的函数必须存在无法达到的终点
function error(message: string): never {
  throw new Error(message);
}

function infiniteLoop(): never {
  while (true) {}
}
```
在 TypeScript 中，可以利用 never 类型的特性来实现全面性检查，具体示例如下：
```typescript
type Foo = string | number;

function controlFlowAnalysisWithNever(foo: Foo) {
  if (typeof foo === "string") {
    // 这里 foo 被收窄为 string 类型
  } else if (typeof foo === "number") {
    // 这里 foo 被收窄为 number 类型
  } else {
    // foo 在这里是 never
    const check: never = foo;
  }
}
```
注意在 else 分支里面，我们把收窄为 never 的 foo 赋值给一个显示声明的 never 变量。如果一切逻辑正确，那么这里应该能够编译通过。但是假如后来有一天你的同事修改了 Foo 的类型：
```typescript
type Foo = string | number | boolean;
```
然而他忘记同时修改 `controlFlowAnalysisWithNever` 方法中的控制流程，这时候 `else` 分支的 `foo` 类型会被收窄为 `boolean` 类型，导致无法赋值给 `never` 类型，这时就会产生一个编译错误。通过这个方式，我们可以确保 `controlFlowAnalysisWithNever` 方法总是穷尽了 `Foo` 的所有可能类型。 通过这个示例，我们可以得出一个结论：使用 `never` 避免出现新增了联合类型没有对应的实现，目的就是写出类型绝对安全的代码。

## 三、TypeScript 断言

### 类型断言  
有时候你会遇到这样的情况，你会比 TypeScript 更了解某个值的详细信息。通常这会发生在你清楚地知道一个实体具有比它现有类型更确切的类型。

通过类型断言这种方式可以告诉编译器，“相信我，我知道自己在干什么”。类型断言好比其他语言里的类型转换，但是不进行特殊的数据检查和解构。它没有运行时的影响，只是在编译阶段起作用。

类型断言有两种形式：

**1. “尖括号” 语法**  
```typescript
let someValue: any = "this is a string";
let strLength: number = (<string>someValue).length;
```

**2. as 语法**   
```typescript
let someValue: any = "this is a string";
let strLength: number = (someValue as string).length;
```

### 非空断言  
在上下文中当类型检查器无法断定类型时，一个新的后缀表达式操作符 ! 可以用于断言操作对象是非 null 和非 undefined 类型。具体而言，x! 将从 x 值域中排除 null 和 undefined 。

**1. 忽略 undefined 和 null 类型**  
```typescript
function myFunc(maybeString: string | undefined | null) {
  // Type 'string | null | undefined' is not assignable to type 'string'.
  // Type 'undefined' is not assignable to type 'string'. 
  const onlyString: string = maybeString; // Error
  const ignoreUndefinedAndNull: string = maybeString!; // Ok
}
```

**2. 调用函数时忽略 undefined 类型**  
```typescript
type NumGenerator = () => number;

function myFunc(numGenerator: NumGenerator | undefined) {
  // Object is possibly 'undefined'.(2532)
  // Cannot invoke an object which is possibly 'undefined'.(2722)
  const num1 = numGenerator(); // Error
  const num2 = numGenerator!(); //OK
}
```

因为 `!` 非空断言操作符会从编译生成的 JavaScript 代码中移除，所以在实际使用的过程中，要特别注意。比如下面这个例子：

```typescript
const a: number | undefined = undefined;
const b: number = a!;
console.log(b); 
```

以上 TS 代码会编译生成以下 ES5 代码：

```javascript
"use strict";
const a = undefined;
const b = a;
console.log(b);
```

虽然在 TS 代码中，我们使用了非空断言，使得 `const b: number = a!;` 语句可以通过 TypeScript 类型检查器的检查。但在生成的 ES5 代码中，`!` 非空断言操作符被移除了，所以在浏览器中执行以上代码，在控制台会输出 `undefined`。

### 确定赋值断言  
在 TypeScript 2.7 版本中引入了确定赋值断言，即允许在实例属性和变量声明后面放置一个 `!` 号，从而告诉 TypeScript 该属性会被明确地赋值。

```typescript
let x: number;
initialize();
// Variable 'x' is used before being assigned.(2454)
console.log(2 * x); // Error

function initialize() {
  x = 10;
}
```

很明显该异常信息是说变量 x 在赋值前被使用了，要解决该问题，我们可以使用确定赋值断言：

```typescript
let x!: number;
initialize();
console.log(2 * x); // Ok

function initialize() {
  x = 10;
}
```

通过 `let x!: number;` 确定赋值断言，TypeScript 编译器就会知道该属性会被明确地赋值。

## 四、类型守卫

类型保护是可执行运行时检查的一种表达式，用于确保该类型在一定的范围内。 

类型保护可以保证一个字符串是一个字符串，尽管它的值也可以是一个数值。类型保护与特性检测并不是完全不同，其主要思想是尝试检测属性、方法或原型，以确定如何处理值。

目前主要有四种的方式来实现类型保护：

### in 关键字  
```typescript
interface Admin {
	name: string;
	privileges: string[];
}

interface Employee {
	name: string;
	startDate: Date;
}

type UnknownEmployee = Employee | Admin;

function printEmployeeInformation(emp: UnknownEmployee) {
	console.log("Name: " + emp.name);
	if ("privileges" in emp) {
		console.log("Privileges: " + emp.privileges);
	}
	if ("startDate" in emp) {
		console.log("Start Date: " + emp.startDate);
	}
}
```

### typeof 关键字  
```typescript
function padLeft(value: string, padding: string | number) {
	if (typeof padding === "number") {
		return Array(padding + 1).join("") + value;
	}
	if (typeof padding === "string") {
		return padding + value;
	}
	throw new Error(`Expected string or number, got '${padding}'`);
}
```
`typeof` 类型保护只支持两种形式： `typeof v === "typename"` 和 `typeof v !== typename` , "typename" 必须是 "number" , "string", "boolean" 或 "Symbol"。但是 TypeScript 并不会阻止你与其他字符串比较，语言不会把那些表达式识别为类型保护。

### instanceof  关键字  
```typescript
interface Padder {
  getPaddingString(): string;
}

class SpaceRepeatingPadder implements Padder {
  constructor(private numSpaces: number) {}
  getPaddingString() {
    return Array(this.numSpaces + 1).join(" ");
  }
}

class StringPadder implements Padder {
  constructor(private value: string) {}
  getPaddingString() {
    return this.value;
  }
}

let padder: Padder = new SpaceRepeatingPadder(6);

if (padder instanceof SpaceRepeatingPadder) {
  // padder的类型收窄为 'SpaceRepeatingPadder'
}
```

### 自定义类型保护的类型谓词  
```typescript
function isNumber(x: any): x is number {
  return typeof x === "number";
}

function isString(x: any): x is string {
  return typeof x === "string";
}
```

## 五、联合类型和类型别名  

### 联合类型  
联合类型通常与 null 或 undefined 一起使用：  
```typescript
const sayHello = (name: string | undefined) => {
  /* ... */
};
```
例如，这里 `name` 的类型是 `string | undefined` 意味着可以将 `string` 或 `undefined` 的值传递给 `sayHello` 函数。
```typescript
sayHello("semlinker");
sayHello(undefined);
```
通过这个示例，你可以凭直觉知道类型 A 和类型 B 联合后的类型是同时接受 A 和 B 值的类型。此外，对于联合类型来说，你可能会遇到以下的用法：
```typescript
let num: 1 | 2 = 1;
type EventNames = 'click' | 'scroll' | 'mousemove';
```
以上示例中的 `1`、`2` 或 `'click'` 被称为字面量类型，用来约束取值只能是某几个值中的一个

### 可辨识联合  
TypeScript 可辨识联合（Discriminated Unions）类型，也称为代数数据类型或标签联合类型。它包含 3 个要点：可辨识、联合类型和类型守卫。

这种类型的本质是结合联合类型和字面量类型的一种类型保护方法。如果一个类型是多个类型的联合类型，且多个类型含有一个公共属性，那么就可以利用这个公共属性，来创建不同的类型保护区块。

+ **1. 可辨识**  
	可辨识要求联合类型中的每个元素都含有一个单例类型属性，比如：  
	```typescript
	enum CarTransmission {
		Automatic = 200,
		Manual = 300
	}

	interface Motorcycle {
		vType: "motorcycle"; // discriminant
		make: number; // year
	}

	interface Car {
		vType: "car"; // discriminant
		transmission: CarTransmission
	}

	interface Truck {
		vType: "truck"; // discriminant
		capacity: number; // in tons
	}
	```

	在上述代码中，我们分别定义了 `Motorcycle`、 `Car` 和 `Truck` 三个接口，在这些接口中都包含一个 `vType` 属性，该属性被称为可辨识的属性，而其它的属性只跟特性的接口相关。

+ **2. 联合类型**  
  基于前面定义了三个接口，我们可以创建一个 `Vehicle` 联合类型：  
	```typescript
	type Vehicle = Motorcycle | Car | Truck;
	```
	现在我们就可以开始使用 `Vehicle` 联合类型，对于 `Vehicle` 类型的变量，它可以表示不同类型的车辆。

+ **3. 类型守卫**  
	下面我们来定义一个 evaluatePrice 方法，该方法用于根据车辆的类型、容量和评估因子来计算价格，具体实现如下：  
	```typescript
	const EVALUATION_FACTOR = Math.PI; 

	function evaluatePrice(vehicle: Vehicle) {
		return vehicle.capacity * EVALUATION_FACTOR;
	}

	const myTruck: Truck = { vType: "truck", capacity: 9.5 };
	evaluatePrice(myTruck);
	```
	对于以上代码，TypeScript 编译器将会提示以下错误信息：  
	```typescript
	Property 'capacity' does not exist on type 'Vehicle'.
	Property 'capacity' does not exist on type 'Motorcycle'.
	```
	原因是在 `Motorcycle` 接口中，并不存在 `capacity` 属性，而对于 `Car` 接口来说，它也不存在 `capacity` 属性。那么，现在我们应该如何解决以上问题呢？这时，我们可以使用类型守卫。下面我们来重构一下前面定义的 `evaluatePrice` 方法，重构后的代码如下：
	```typescript
	function evaluatePrice(vehicle: Vehicle) {
		switch(vehicle.vType) {
			case "car":
				return vehicle.transmission * EVALUATION_FACTOR;
			case "truck":
				return vehicle.capacity * EVALUATION_FACTOR;
			case "motorcycle":
				return vehicle.make * EVALUATION_FACTOR;
		}
	}
	```
	在以上代码中，我们使用 `switch` 和 `case` 运算符来实现类型守卫，从而确保在 `evaluatePrice` 方法中，我们可以安全地访问 `vehicle` 对象中的所包含的属性，来正确的计算该车辆类型所对应的价格。  

### 类型别名  
类型别名用来给一个类型起个新名字。  
```typescript
type Message = string | string[];

let greet = (message: Message) => {
  // ...
};
```

## 六、交叉类型  
在 TypeScript 中交叉类型是将多个类型合并为一个类型。通过 & 运算符可以将现有的多种类型叠加到一起成为一种类型，它包含了所需的所有类型的特性。  

```typescript
type PartialPointX = { x: number; };
type Point = PartialPointX & { y: number; };

let point: Point = {
  x: 1,
  y: 1
}
```

在上面代码中我们先定义了 PartialPointX 类型，接着使用 & 运算符创建一个新的 Point 类型，表示一个含有 x 和 y 坐标的点，然后定义了一个 Point 类型的变量并初始化。  

### 同名基础类型属性的合并  
那么现在问题来了，假设在合并多个类型的过程中，刚好出现某些类型存在相同的成员，但对应的类型又不一致，比如：

```typescript
interface X {
  c: string;
  d: string;
}

interface Y {
  c: number;
  e: string
}

type XY = X & Y;
type YX = Y & X;

let p: XY;
let q: YX;
```

在上面的代码中，接口 X 和接口 Y 都含有一个相同的成员 c，但它们的类型不一致。对于这种情况，此时 XY 类型或 YX 类型中成员 c 的类型是不是可以是 `string` 或 `number` 类型呢？比如下面的例子：

```typescript
p = { c: 6, d: "d", e: "e" }; 

// Type 'number' is not assignable to type 'never'. (2322)
// The expected type comes from property 'c' which

q = { c: "c", d: "d", e: "e" }; 

// Type 'string' is not assignable to type 'never'. (2322)
// input.ts(7，3): The expected type comes from property 'c' which
```

为什么接口 X 和接口 Y 混入后，成员 c 的类型会变成 `never` 呢？这是因为混入后成员 c 的类型为 `string & number`，即成员 c 的类型既可以是 `string` 类型又可以是 `number` 类型。很明显这种类型是不存在的，所以混入后成员 c 的类型为 `never`。

### 同名非基础类型属性的合并  
在上面示例中，刚好接口 X 和接口 Y 中内部成员 c 的类型都是基本数据类型，那么如果是非基本数据类型的话，又会是什么情形。我们来看个具体的例子：

```typescript
interface D { d: boolean; }
interface E { e: string; }
interface F { f: number; }

interface A { x: D; }
interface B { x: E; }
interface C { x: F; }

type ABC = A & B & C;

let abc: ABC = {
  x: {
    d: true,
    e: 'semlinker',
    f: 666
  }
};

console.log('abc:', abc);
```

以上代码成功运行后，控制台会输出以下结果：

```javascript
abc: {
	x: {
		d: true,
		e: 'semlinker',
		f: 666,
		__proto__: Object
	},
	__proto__: Object
}
```

可知，在混入多个类型时，若存在相同的成员，且成员类型为非基本数据类型，那么是可以成功合并。  

## 七、TypeScript 函数  

### TypeScript 函数与 JavaScript 函数的区别  
|   TypeScript   |     JavaScript     |
| :------------: | :----------------: |
|    含有类型    |       无类型       |
|    箭头函数    | 箭头函数 (ES2015)  |
|    函数类型    |     无函数类型     |
| 必填和可选参数 | 所有参数都是可选的 |
|    默认参数    |      默认参数      |
|    剩余参数    |      剩余参数      |
|    函数重载    |     无函数重载     |

### 箭头函数  
+ **1. 常见语法**  
  ```typescript
  myBooks.forEach(() => console.log('reading'));

  myBooks.forEach(title => console.log(title));

  myBooks.forEach((title, idx, arr) =>
    console.log(idx + '-' + title);
  );

  myBooks.forEach((title, idx, arr) => {
    console.log(idx + '-' + title);
  });
  ```  
+ **2. 使用示例**  
  ```typescript
  // 未使用箭头函数
  function Book() {
    let self = this;
    self.publishDate = 2016;
    setInterval(function () {
      console.log(self.publishDate);
    }, 1000);
  }

  // 使用箭头函数
  function Book() {
    this.publishDate = 2016;
    setInterval(() => {
      console.log(this.publishDate);
    }, 1000);
  }
  ```  

### 参数类型和返回类型  
```typescript
function createUserId(name: string, id: number): string {
  return name + id;
}
```  

### 函数类型  
```typescript
let IdGenerator: (chars: string, nums: number) => string;

function createUserId(name: string, id: number): string {
  return name + id;
}

IdGenerator = createUserId;
```  

### 可选参数及默认参数  
```typescript
// 可选参数
function createUserId(name: string, id: number, age?: number): string {
  return name + id;
}

// 默认参数
function createUserId(
  name: string = "semlinker",
  id: number,
  age?: number
): string {
  return name + id;
}
```  
在声明函数时，可以通过 `?` 来定义可选参数，比如 `age?: number` 这种形式。在实际使用时，需要注意的是可选参数要放在普通参数的后面，不然会导致编译错误。  

### 剩余参数  
```typescript
function push(array, ...items) {
  items.forEach(function (item) {
    array.push(item);
  });
}

let a = [];
push(a, 1, 2, 3);
```  

### 函数重载  
函数重载或方法重载是使用相同名称和不同参数数量或类型创建多个方法的一种能力。  
```typescript
function add(a: number, b: number): number;
function add(a: string, b: string): string;
function add(a: string, b: number): string;
function add(a: number, b: string): string;
function add(a: Combinable, b: Combinable) {
  // type Combinable = string | number;
  if (typeof a === 'string' || typeof b === 'string') {
    return a.toString() + b.toString();
  }
  return a + b;
}
```  
在以上代码中，我们为 add 函数提供了多个函数类型定义，从而实现函数的重载。在 TypeScript 中除了可以重载普通函数之外，我们还可以重载类中的成员方法。  
方法重载是指在同一个类中方法同名，参数不同（参数类型不同、参数个数不同或参数个数相同时参数的先后顺序不同），调用时根据实参的形式，选择与它匹配的方法执行操作的一种技术。所以类中成员方法满足重载的条件是：在同一个类中，方法名相同且参数列表不同。下面我们来举一个成员方法重载的例子：  
```typescript
class Calculator {
  add(a: number, b: number): number;
  add(a: string, b: string): string;
  add(a: string, b: number): string;
  add(a: number, b: string): string;
  add(a: Combinable, b: Combinable) {
  if (typeof a === 'string' || typeof b === 'string') {
    return a.toString() + b.toString();
  }
    return a + b;
  }
}

const calculator = new Calculator();
const result = calculator.add('Semlinker', ' Kakuqo');
```  
这里需要注意的是，当 TypeScript 编译器处理函数重载时，它会查找重载列表，尝试使用第一个重载定义。 如果匹配的话就使用这个。 因此，在定义重载的时候，一定要把最精确的定义放在最前面。另外在 Calculator 类中，add(a: Combinable, b: Combinable){ } 并不是重载列表的一部分，因此对于 add 成员方法来说，我们只定义了四个重载方法。  

## 八、TypeScript 数组  

### 数组解构  
```TypeScript
let x: number; let y: number; let z: number;
let five_array = [0,1,2,3,4];
[x,y,z] = five_array;
```  

### 数组展开运算符  
```TypeScript
let two_array = [0, 1];
let five_array = [...two_array, 2, 3, 4];
```  

### 数组遍历  
```TypeScript
let colors: string[] = ["red", "green", "blue"];
for (let i of colors) {
  console.log(i);
}
```  

## 九、TypeScript 对象  

### 对象解构  
```TypeScript
let person = {
  name: "Semlinker",
  gender: "Male",
};

let { name, gender } = person;
```  

### 对象展开运算符  
```TypeScript
let person = {
  name: "Semlinker",
  gender: "Male",
  address: "Xiamen",
};

// 组装对象
let personWithAge = { ...person, age: 33 };

// 获取除了某些项外的其它项
let { name, ...rest } = person;

console.log(rest);

// {
//   "gender": "Male",
//   "address": "Xiamen"
// } 
```  

## 十、TypeScript 接口  
接口是对行为的抽象，具体如何行动需要由类去实现。  
TypeScript 中的接口除了可用于对类的一部分行为进行抽象以外，也常用于对「对象的形状（Shape）」进行描述。

### 对象的形状  
```TypeScript
interface Person {
  name: string;
  age: number;
}

let semlinker: Person = {
  name: "semlinker",
  age: 33,
};
```  

### 可选 | 只读属性
```TypeScript
interface Person {
  readonly name: string;
  age?: number;
}
```  
只读属性用于限制只能在对象刚刚创建的时候修改其值。此外 TypeScript 还提供了 `ReadonlyArray<T>` 类型，它与 `Array<T>` 相似，只是把所有可变方法去掉了，因此可以确保数组创建后再也不能被修改。  
```TypeScript
let a: number[] = [1, 2, 3, 4];
let ro: ReadonlyArray<number> = a;
ro[0] = 12; // error!
ro.push(5); // error!
ro.length = 100; // error!
a = ro; // error!
```  

### 任意属性  
一个接口中除了包含必选和可选属性之外，还允许有其他的任意属性，可以使用索引签名来实现。
```TypeScript
interface Person {
  name: string;
  age?: number;
  [propName: string]: any;
}

const p1 = { name: "semlinker" };
const p2 = { name: "lolo", age: 5 };
const p3 = { name: "kakuqo", sex: 1 }
```  

### 接口与类型别名的区别
+ **1. Objects / Functions**  
  接口和类型别名都可以用来描述对象的形状或函数签名：  
  ```TypeScript
  // 接口
  interface Point {
    x: number;
    y: number;
  }

  interface SetPoint {
    (x: number, y: number): void;
  }

  // 类型别名
  type Point = {
    x: number;
    y: number;
  };

  type SetPoint = (x: number, y: number) => void;
  ```  

+ **2. Other Types**  
  与接口类型不一样，类型别名可以用于一些其他类型，比如原始类型、联合类型和元组：
  ```TypeScript
  // primitive
  type Name = string;

  // object
  type PartialPointX = { x: number; };
  type PartialPointY = { y: number; };

  // union
  type PartialPoint = PartialPointX | PartialPointY;

  // tuple
  type Data = [number, string];
  ```  

+ **3. Extend**  
  接口和类型别名都能够被扩展，但语法有所不同。此外，接口和类型别名不是互斥的。接口可以扩展类型别名，而反过来是不行的。  
  **Interface extends interface**  
  ```TypeScript
  interface PartialPointX { x: number; }
  interface Point extends PartialPointX { 
    y: number; 
  }
  ```  
  **Type alias extends type alias**  
  ```TypeScript
  type PartialPointX = { x: number; };
  type Point = PartialPointX & { y: number; };
  ```  
  **Interface extends type alias**  
  ```TypeScript
  type PartialPointX = { x: number; };
  interface Point extends PartialPointX {
    y: number;
  }
  ```  
  **Type alias extends interface**  
  ```TypeScript
  interface PartialPointX { x: number; }
  type Point = PartialPointX & { y: number; };
  ```  

+ **4. Implements**  
  类可以以相同的方式实现接口或类型别名，但类不能实现使用类型别名定义的联合类型：  
  ```TypeScript
  interface Point {
    x: number;
    y: number;
  }

  class SomePoint implements Point {
    x = 1;
    y = 2;
  }

  type Point2 = {
    x: number;
    y: number;
  };

  class SomePoint2 implements Point2 {
    x = 1;
    y = 2;
  }

  type PartialPoint = { x: number; } | { y: number; };

  // A class can only implement an object type or 
  // intersection of object types with statically known members.
  class SomePartialPoint implements PartialPoint { // Error
    x = 1;
    y = 2;
  }
  ```  

+ **5. Declaration merging**  
  与类型别名不同，接口可以定义多次，会被自动合并为单个接口。  
  ```TypeScript
  interface Point { x: number; }
  interface Point { y: number; }

  const point: Point = { x: 1, y: 2 };
  ```  

## 十一、TypeScript 类  

### 类的属性与方法  
通过 Class 关键字来定义一个类：
```TypeScript
class Greeter {
  // 静态属性
  static cname: string = "Greeter";
  // 成员属性
  greeting: string;

  // 构造函数 - 执行初始化操作
  constructor(message: string) {
    this.greeting = message;
  }

  // 静态方法
  static getClassName() {
    return "Class name is Greeter";
  }

  // 成员方法
  greet() {
    return "Hello, " + this.greeting;
  }
}

let greeter = new Greeter("world");
```  

### ECMAScript 私有字段  
```TypeScript
class Person {
  #name: string;

  constructor(name: string) {
    this.#name = name;
  }

  greet() {
    console.log(`Hello, my name is ${this.#name}!`);
  }
}

let semlinker = new Person("Semlinker");

semlinker.#name;
// Property '#name' is not accessible outside class 'Person'
// because it has a private identifier.
```  
与常规属性（甚至使用 private 修饰符声明的属性）不同，私有字段要牢记以下规则：  
+ 私有字段以 `#` 字符开头，有时我们称之为私有名称
+ 每个私有字段名称都唯一地限定于其包含的类
+ 不能在私有字段上使用 TypeScript 可访问性修饰符（如 public 或 private）
+ 私有字段不能在包含的类之外访问，甚至不能被检测到

### 访问器  
通过 getter 和 setter 方法来实现数据的封装和有效性校验，防止出现异常数据。  
```TypeScript
let passcode = "Hello TypeScript";

class Employee {
  private _fullName: string;

  get fullName(): string {
    return this._fullName;
  }

  set fullName(newName: string) {
    if (passcode && passcode == "Hello TypeScript") {
      this._fullName = newName;
    } else {
      console.log("Error: Unauthorized update of employee!");
    }
  }
}

let employee = new Employee();
employee.fullName = "Semlinker";
if (employee.fullName) {
  console.log(employee.fullName);
}
```  

### 类的继承  
通过 extends 关键字来实现继承。  
```TypeScript
class Animal {
  name: string;
  
  constructor(theName: string) {
    this.name = theName;
  }
  
  move(distanceInMeters: number = 0) {
    console.log(`${this.name} moved ${distanceInMeters}m.`);
  }
}

class Snake extends Animal {
  constructor(name: string) {
    super(name); // 调用父类的构造函数
  }
  
  move(distanceInMeters = 5) {
    console.log("Slithering...");
    super.move(distanceInMeters);
  }
}

let sam = new Snake("Sammy the Python");
sam.move();
```  

### 抽象类  
使用 `abstract` 关键字声明抽象类。抽象类不能被实例化，因为它里面包含一个或多个抽象方法。所谓的抽象方法，是指不包含具体实现的方法：  
```TypeScript
abstract class Person {
  constructor(public name: string){}

  abstract say(words: string) :void;
}

// Cannot create an instance of an abstract class.(2511)
const lolo = new Person(); // Error
```  
抽象类不能被直接实例化，我们只能实例化实现了所有抽象方法的子类：  
```TypeScript
abstract class Person {
  constructor(public name: string){}

  // 抽象方法
  abstract say(words: string) :void;
}

class Developer extends Person {
  constructor(name: string) {
    super(name);
  }
  
  say(words: string): void {
    console.log(`${this.name} says ${words}`);
  }
}

const lolo = new Developer("lolo");
lolo.say("I love ts!"); // lolo says I love ts!
```  

### 类方法重载  
```TypeScript
class ProductService {
    getProducts(): void;
    getProducts(id: number): void;
    getProducts(id?: number) {
      if(typeof id === 'number') {
          console.log(`获取id为 ${id} 的产品信息`);
      } else {
          console.log(`获取所有的产品信息`);
      }  
    }
}

const productService = new ProductService();
productService.getProducts(666); // 获取id为 666 的产品信息
productService.getProducts(); // 获取所有的产品信息 
```  

## 十二、TypeScript 泛型  

设计泛型的关键目的是在成员之间提供有意义的约束，这些成员可以是：类的实例成员、类的方法、函数参数和函数返回值。  
泛型（Generics）是允许同一个函数接受不同类型参数的一种模板。相比于使用 any 类型，使用泛型来创建可复用的组件要更好，因为泛型会保留参数类型。

### 泛型语法  
```TypeScript
function identity <T>(value: T) : T {
  return value;
}

console.log(identity<Number>(1));   // 1
```  
参考上面的代码，当我们调用 `identity<Number>(1)` ， `Number` 类型就像参数 `1` 一样，它将在出现 `T` 的任何位置填充该类型。  
代码中 `<T>` 内部的 `T` 被称为类型变量，它是我们希望传递给identity函数的类型占位符，同时它被分配给 `value`  参数用来代替它的类型：此时 `T` 充当的是类型，而不是特定的 Number 类型。  
其中 `T` 代表 **Type**，在定义泛型时通常用作第一个类型变量名称。但实际上 `T` 可以用任何有效名称代替。除了 `T` 之外，以下是常见泛型变量代表的意思：  
+ K（Key）：表示对象中的键类型;  
+ V（Value）：表示对象中的值类型;  
+ E（Element）：表示元素类型;  
其实并不是只能定义一个类型变量，我们可以引入希望定义的任何数量的类型变量。比如我们引入一个新的类型变量 `U` ，用于扩展我们定义的 `identity` 函数：  
```TypeScript
function identity <T, U>(value: T, message: U) : T {
  console.log(message);
  return value;
}

console.log(identity<Number, string>(68, "Semlinker"));
// Semlinker
// 68
```  
除了为类型变量显式设定值之外，一种更常见的做法是使编译器自动选择这些类型，从而使代码更简洁。我们可以完全省略尖括号，比如：  
```TypeScript
function identity <T, U>(value: T, message: U) : T {
  console.log(message);
  return value;
}

console.log(identity(68, "Semlinker"));
// Semlinker
// 68
```  

### 泛型接口  
```TypeScript
interface GenericIdentityFn<T> {
  (arg: T): T;
}
```  

### 泛型类  
```TypeScript
class GenericNumber<T> {
  zeroValue: T;
  add: (x: T, y: T) => T;
}

let myGenericNumber = new GenericNumber<number>();
myGenericNumber.zeroValue = 0;
myGenericNumber.add = function (x, y) {
  return x + y;
};
```  

### 泛型工具类型  
为了方便开发者，TypeScript内置了一些常用的工具类型。

+ **1. typeof**  
  在 TypeScript 中， `typeof` 操作符可以用来获取一个变量声明或对象的类型。  
  ```TypeScript
  interface Person {
    name: string;
    age: number;
  }

  const sem: Person = { name: 'semlinker', age: 33 };
  type Sem= typeof sem; // -> Person

  function toArray(x: number): Array<number> {
    return [x];
  }

  type Func = typeof toArray; // -> (x: number) => number[]
  ```  

+ **2. keyof**  
  `keyof` 操作符是在 TypeScript 2.1 版本引入的，该操作符可以用于获取某种类型的所有键，其返回类型是联合类型。  
  ```TypeScript
  interface Person {
    name: string;
    age: number;
  }

  type K1 = keyof Person; // "name" | "age"
  type K2 = keyof Person[]; // "length" | "toString" | "pop" | "push" | "concat" | "join" 
  type K3 = keyof { [x: string]: Person };  // string | number
  ```  
  在 TypeScript 中支持两种索引签名，数字索引和字符串索引：  
  ```TypeScript
  interface StringArray {
    // 字符串索引 -> keyof StringArray => string | number
    [index: string]: string; 
  }

  interface StringArray1 {
    // 数字索引 -> keyof StringArray1 => number
    [index: number]: string;
  }
  ```  
  为了同时支持两种索引类型，就得要求数字索引的返回值必须是字符串索引返回值的子类。**其中的原因就是当使用数值索引时，JavaScript 在执行索引操作时，会先把数值索引先转换为字符串索引。**所以 `keyof { [x: string]: Person }` 的结果会返回 `string | number`。  

+ **3. in**  
  `in` 用来遍历枚举类型：  
  ```TypeScript
  type Keys = "a" | "b" | "c"

  type Obj =  {
    [p in Keys]: any
  } // -> { a: any, b: any, c: any }
  ```  

+ **4. infer**  
  在条件类型语句中，可以用 `infer` 声明一个类型变量并且对它进行使用。  
  ```TypeScript
  type ReturnType<T> = T extends (
    ...args: any[]
  ) => infer R ? R : any;
  ```  
  以上代码中 `infer R` 就是声明一个变量来承载传入函数签名的返回值类型，简单说就是用它取到函数返回值的类型方便之后使用。  

+ **5. extends**  
  有时候我们定义的泛型不想过于灵活或者说想继承某些类等，可以通过 `extends` 关键字添加泛型约束。  

  ```TypeScript
  interface Lengthwise {
    length: number;
  }

  function loggingIdentity<T extends LengthWise>(arg: T): T {
    console.log(arg.length)l
    return arg;
  }
  ```  

  现在这个泛型函数被定义了约束，因此它不再是适用于任意类型：  

  ```TypeScript
  loggingIdentity(3);   // Error, number doesn't have a .length property
  ```  

  这时需要传入符合约束类型的值，必须包含必须的属性：  

  ```TypeScript
  loggingIdentity({length: 10, value: 3});
  ```   

+ **6. Partial**  
  `Partial<T>` 的作用就是将某个类型里的属性全部变为可选项 `?` 。  
  **定义：**  
  ```TypeScript
  type Partial<T> = {
    [P in keyof T]?: T[P];
  }
  ```  

  首先，通过 `keyof T` 拿到 `T` 的所有属性名，然后使用 `in` 进行遍历，将值赋给 `p`，最后通过 `T[P]` 去的相应的属性值。中间的 `?` 号，用于将所有属性变为可选。  

  ```TypeScript
  interface Todo {
    title: string;
    description: string;
  }

  function updateTodo(todo: Todo, fieldsToUpdate: Partial<Todo>) {
    return { ...todo, ...fieldsToUpdate };
  }

  const todo1 = {
    title: "Title 1",
    description: "Description 1",
  };

  const todo2 = updateTodo(todo1, {
    title: "Title 2",
    description: "Description",
  });
  ```  

  在上面的 `updateTodo` 方法中，我们利用了 `Partial<T>` 工具类型，定义 `fieldsToUpdate` 的类型为 `Partial<Todo>`，即：  

  ```TypeScript
  {
    title?: string | undefined;
    description?: string | undefined;
  }
  ```

## 十三、TypeScript 装饰器  

### 定义  

+ 它是一个表达式
+ 该表达式被执行后，返回一个函数
+ 函数的入参分别是 `target` 、 `name` 和 `descriptor`
+ 执行该函数后，可能返回 descriptor 对象，用于配置 target 对象

### 分类  

+ 类装饰器 ( Class decorators )
+ 属性装饰器 ( Property decorators )
+ 方法装饰器 ( Method decorators )
+ 参数装饰器 ( Parameter decorators )

若要启动实验性的装饰器特性，必须在命令行或 `tsconfig.json` 里启用 `experimentalDecorators` 编辑器选项：  

命令行:  
```TypeScript
tsc --target ES5 --experimentalDecorators
```  

tsconfig.json:  
```TypeScript
{
  "compilerOptions": {
    "target": "ES5",
    "experimentalDecorators": true,
  }
}
```  

### 类装饰器  

类装饰器声明：  
```TypeScript
declare type ClassDecorator = <TFunction extends Function>(
  target: TFunction
) => TFunction | void;
```  
类装饰器接收一个参数：
+ target: TFunction - 被装饰的类

类装饰器的使用：  
```TypeScript
function Greeter(target: Funtion): void {
  target.prototype.greet = function (): void {
    console.log("Hello world!");
  };
}

@Greeter
class Greeting {
  constructor() {
    //  内部实现
  }
}

let myGreeting = new Greeting();
(myGreeting as any).greet();  // output: 'Hello world!'

// ------------------------------------------------------------

function newGreeter(greeting: string) {
  return function (target: Function) {
    target.prototype.greet = function (): void {
      console.log(greeting);
    };
  };
}

@newGreeter("Hello!")
class newGreeting {
  constructor () {
    // 内部实现
  }
}

let myNewGreeting = new newGreeting();
(myGreeting as any).greet();  // output: 'Hello!'
```  
上面的例子中，定义了 `Greeter` 和 `newGreeter` 类装饰器，同时我们使用了 `@Greeter` 和 `@newGreeter` 语法糖，来使用装饰器。  

### 属性装饰器  

属性装饰器声明：  
```TypeScript
declare type PropertyDecorator = (target: Object, propertyKey: string | symbol) => void;
```

属性装饰器接收两个参数：
+ target: Object - 被装饰的类
+ propertyKey: string | symbol - 被装饰类的属性名

属性装饰器的使用：  
```TypeScript
function logProperty(target: any, key: string) {
  delete target[key];

  const backingField = "_" + key;

  Object.defineProperty(target, backingField, {
    writable: true,
    enumerable: true,
    configurable: true,
  });

  // property getter
  const getter = function (this: any) {
    const currVal = this[backingField];
    console.log(`Get: ${key} => ${currVal}`);
    return currVal;
  };

  // property setter
  const setter = function (this: any, newVal: any) {
    console.log(`Set: ${key} => ${newVal}`);
    this[backingField] = newVal;
  };

  // Create new property with getter and setter
  Object.defineProperty(target, key, {
    get: getter,
    set: setter,
    enumerable: true,
    configurable: true,
  });
}

class Person {
  @logProperty
  public name: string;

  constructor(name: string) {
    this.name = name;
  }
}

const p1 = new Person('aa');
p1.name = 'AA';
```  

以上定义了一个 `logProperty` 函数，来跟踪用户对属性的操作，当代码成功运行后，在控制台会输出以下结果：  

```TypeScript
Set: name => aa
Set: name => AA
```  

### 方法装饰器  

方法装饰器声明：  
```TypeScript
declare type MethodDecorator = <T>(target: Object, propertyKey: string | symbol, descriptor: TypePropertyDescript<T>) => TypedPropertyDescriptor<T> | void;
```  

方法装饰器接收三个参数： 
+ target: Object - 被装饰的类
+ propertyKey: string | symbol - 方法名
+ descriptor: TypePropertyDescript - 属性描述符

方法装饰器的使用：  

```TypeScript
function log(target: Object, propertyKey: string, descriptor: PorpertyDescriptor) {
  let originalMethod = descriptor.value;
  descriptor.value = function (...args: any[]) {
    console.log("wrapped function: before invoking " + propertyKey);
    let result = originalMethod.apply(this, args);
    console.log("wrapped function: after invoking " + propertyKey);
    return result;
  };
}

class Task {
  @log
  runTask(arg: any): any {
    console.log("runTask invoked, args: " + arg);
    return "finished";
  }
}

let task = new Task();
let result = task.runTask("Hello");
console.log("result: " + result);

// wrapped function: before invoking runTask
// runTask invoked, args: Hello
// wrapped function: after invoking runTask
// result: finished
```  

### 参数装饰器  

参数装饰器的声明：  
```TypeScript
declare type ParameterDecorator = (target: Object, propertyKey: string | symbol, parameterIndex: number) => void
```  

参数装饰器接收三个参数：  
+ target: Object - 被装饰的类
+ propertyKey: string | symbol - 方法名
+ paramerterIndex: number - 方法中参数的索引值

```TypeScript
function Log(target: Function, key: string, parameterIndex: number) {
  let functionLogged = key || target.prototype.constructor.name;
  console.log(`The parameter in position ${parameterIndex} at ${functionLogged} has been decorated`);
}

class Greeter {
  greeting: string;
  constructor(@Log phrase: string) {
    this.greeting = phrase;
  }
}

// The parameter in position 0 at Greeter has been decorated
```  

## 十四、TypeScript 4.0 新特性  

### 构造函数的类属性推断  

当 `noImplicitAny` 配置属性被启用后，TypeScript 4.0 就可以使用控制流分析来确认类中的属性类型：  

```TypeScript
class Person {
  // 在4.0版本之下的，编译器会提示错误信息： Member 'fullName' implicitly has an 'any' type.(7008)
  // 在使用过程中，如果无法保证对成员属性都进行赋值，该属性可能会被认为undefined
  fullName;   // (property) Person.fullName: string                 在4.0版本之下的，编译器会提示错误信息： Error
  firstName;  // (property) Person.firstName: string | undefined    在4.0版本之下的，编译器会提示错误信息： Error
  lastName;   // (property) Person.lastName: string | undefined     在4.0版本之下的，编译器会提示错误信息： Error

  constructor(fullName: string) {
    this.fullName = fullName;
    this.firstName = fullName.split(" ")[0];
    this.lastName = fullName.split(" ")[1];
  }
}
```  

### 标记的元组元素  

使用元组类型来声明剩余参数的类型：  

```TypeScript
function addPerson(...args: [string, number]): void {
  console.log(`Person info: name: ${args[0]}, age: ${args[1]}`)
}

addPerson("aa", 5);   // Person info: name: aa, age: 5

// 另一种方法实现

function addPerson(name: string, age: number) {
  ......
}
```  

对于第一种方式，无法设置第一个参数和第二个参数的名称。对类型检查没有影响，但在元组位置上缺少标签，会使得它们难于使用。为了提高开发者使用元组的体验， TS 4.0 支持为元组类型设置标签：  

```TypeScript
function addPerson(...args: [name: string, age: number]): void {
  ......
}

// 第一种方式的智能提示与这种不同
// 第一种未使用标签的智能提示: addPerson(args_0: string, args_1: number): void
// 第二种是使用标签的智能提示: addPerson(name: string, age: number): void
```  

## 十五、编译上下文  

### tsconfig.json的作用  

+ 用于标识 TypeScript 项目的根路径
+ 用于配置 TypeScript 编译器
+ 用于指定编译的文件

### tsconfig.json重要字段  

+ files - 设置要编译的文件的名称
+ include - 设置需要进行编译的文件，支持路径模式匹配
+ exclude - 设置无需进行编译的文件，支持路径模式匹配
+ compilerOptions - 设置与编译流程相关的选项

### compilerOptions选项  

```TypeScript
{
  "compilerOptions": {
    // 基本选项
    "target": "es5",
    "module": "commonjs",
    "lib": [],
    "allowJs": true,
    "checkJs": true,
    "jsx": "preserve",
    "declaration": true,
    "sourceMap": true,
    "outFile": "./",
    "outDir": "./",
    "rootDir": "./",
    "removeComments": true,
    "noEmit": true,
    "importHelpers": true,
    "isolatedModules": true,

    // 严格的类型检查选项
    "strict": true,
    "noImplicitAny": true,
    "strictNullChecks": true,
    "noImplicitThis": true,
    "alwaysStrict": true,

    // 额外的检查
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noImplicitReturns": true,
    "noFallthroughCasesInSwitch": true,

    // 模块解析选项
    "moduleResolution": "node",
    "baseUrl": "./",
    "paths": {},
    "rootDirs": [],
    "typeRoots": [],
    "types": [],
    "allowSyntheticDefaultImports": true,

    // Source Map Options
    "sourceRoot": "./",
    "mapRoot": "./",
    "inlineSourceMap": true,
    "inlineSources": true,

    // 其他选项
    "experimentalDecorators": true,
    "emitDecoratorMetadata": true,
  }
}
```