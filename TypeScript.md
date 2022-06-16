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

let list: Array<number= [1, 2, 3]; // Array<number+泛型语法
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
在 TypeScript 中，任何类型都可以被归为 any 类型。这让 any 类型成为了类型系统的顶级类型（也被称作全局超级类型）。
```typescript
let notSure: any = 666;
notSure = "semlinker";
notSure = false;
```
`any`类型本质上是类型系统的一个逃逸舱。TypeScript允许我们对`any`类型的值执行任何操作，而无需事先执行任何形式的检查。比如：
```typescript
let value: any;

value.foo.bar; // OK
value.trim(); // OK
value(); // OK
new value(); // OK
value[0][1]; // OK
```
在许多场景下，这太宽松了。使用`any`类型，可以很容易地编写类型正确但在运行时有问题的代码。如果我们使用`any`类型，就无法使用TypeScript提供的大量的保护机制。为了解决`any`带来的问题，TypeScript 3.0引入了`unknown`类型。

### 8. Unknown 类型  
就像所有类型都可以赋值给`any`，所有类型也都可以赋值给`unknown`。这使得`unknown`成为TypeScript类型系统的另一种顶级类型（另一种是`any`）。
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
对`value`变量的所有赋值都被认为是类型正确的。但是，当我们尝试将类型为`unknown`的值赋值给其他类型的变量时会发生什么？
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
`unknown`类型只能被赋值给`any`类型和`unknown`类型本身。直观地说，这是有道理的：只有能够保证任意类型值的容器才能保存`unknown`类型的值。毕竟我们不知道变量`value`中存储了什么类型的值。

对类型为`unknown`的值执行操作时：
```typescript
let value: unknown;

value.foo.bar; // Error
value.trim(); // Error
value(); // Error
new value(); // Error
value[0][1]; // Error
```
将`value`变量类型设置为`unknown`后，这些操作都不再被认为是类型正确的。通过将`any`类型改变为`unknown`类型，我们已将允许所有更改的默认设置，更改为禁止任何更改。

### 9. Tuple 类型  
众所周知，数组一般由同种类型的值组成，但有时我们需要在单个变量中存储不同类型的值，这时候我们就可以使用元组。在JavaScript中是没有元组的，元组是TypeScript中特有的类型，其工作方式类似于数组。

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
某种程度上来说，`void`类型像是`any`类型相反，它表示没有任何类型。当一个函数没有返回值时，通常会见到其返回值类型是`void`：
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
需要注意的是，声明一个`void`类型的变量没有什么作用，因为在严格模式下，它的值只能为`undefined`：
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
  `object`类型是：TypeScript 2.2引入的新类型，它用于表示非原始类型。
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

## TypeScript 断言

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

## 类型守卫

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

## 联合类型和类型别名  

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

## 交叉类型  
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