# ECMAScript 6  
## 一、无序集合 Set  
没有排序概念的数组，并且具有元素不可重复的特性。  
### 1. Set 的代码示例  
Set 需要使用 `new Set()` 声明。
```javascript
let set = new Set("Hello!!!");
set.add(12);  // ['H', 'e', 'l', 'o', '!', 12]
console.log(set.has("!"));  // true
console.log(set.size);  // 6
set.add('l'); // ['H', 'e', 'l', 'o', '!', 12]
console.log(set.size);  // 6
set.add('Hello'); // ['H', 'e', 'l', 'o', '!', 12, 'Hello']
set.delete(12); //true  ['H', 'e', 'l', 'o', '!', 'Hello']
console.log(...set);  // H e l o ! Hello
set.clear();  // Set(0)
```  
