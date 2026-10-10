# JavaScript Core Concepts (Q11 - Q30)

> Q1-Q10 are in `01-fundamentals.md`. This file continues the Top 50 list.

## Top 50 Index

| #     | Topic                                | File |
| ----- | ------------------------------------ | ---- |
| 1     | var, let, const                      | 01   |
| 2     | Hoisting                             | 01   |
| 3     | Closure                              | 01   |
| 4     | `this` keyword                       | 01   |
| 5     | `==` vs `===`                        | 01   |
| 6     | Event loop                           | 01   |
| 7     | Promises                             | 01   |
| 8     | async/await                          | 01   |
| 9     | Arrow vs regular function            | 01   |
| 10    | Prototype chain                      | 01   |
| 11    | Data types and `typeof null`         | 02   |
| 12    | Pass by value vs reference           | 02   |
| 13    | Shallow vs deep copy                 | 02   |
| 14    | Destructuring, spread, rest          | 02   |
| 15    | map, filter, reduce                  | 02   |
| 16    | forEach vs map                       | 02   |
| 17    | Truthy and falsy                     | 02   |
| 18    | null vs undefined                    | 02   |
| 19    | Scope and scope chain                | 02   |
| 20    | Execution context and call stack     | 02   |
| 21    | call, apply, bind                    | 02   |
| 22    | Higher-order functions and callbacks | 02   |
| 23    | IIFE                                 | 02   |
| 24    | `?.`, `??`, template literals        | 02   |
| 25    | const vs Object.freeze               | 02   |
| 26    | Currying                             | 02   |
| 27    | Classes                              | 02   |
| 28    | Modules (CommonJS vs ES)             | 02   |
| 29    | Set and Map                          | 02   |
| 30    | slice vs splice                      | 02   |
| 31-50 | Async, events, coding problems       | 03   |

---

## Q11. Data types and `typeof null`

### English

**Question:** What are the data types in JavaScript? Why does `typeof null` return `"object"`?

**Answer:** JavaScript has 7 primitive types: `string`, `number`, `bigint`, `boolean`, `undefined`, `null` and `symbol`. Everything else is an object (arrays, functions, dates, etc.). Primitives are immutable and compared by value. `typeof null` returns `"object"` because of a bug in the first version of JavaScript that was never fixed to avoid breaking existing code. To check for null, use `value === null`.

### বাংলা

**প্রশ্ন:** JavaScript-এ কী কী data type আছে? `typeof null` কেন `"object"` দেয়?

**উত্তর:** JavaScript-এ ৭টি primitive type আছে: `string`, `number`, `bigint`, `boolean`, `undefined`, `null` ও `symbol`। বাকি সবকিছু object (array, function, date ইত্যাদি)। Primitive মান পরিবর্তন করা যায় না এবং মান ধরে তুলনা হয়। `typeof null` `"object"` দেয় কারণ JavaScript-এর একদম প্রথম সংস্করণে একটি ভুল ছিল, পুরনো code ভেঙে যাবে বলে সেটা কখনো ঠিক করা হয়নি। null যাচাই করতে `value === null` ব্যবহার করুন।

### Example

```js
console.log(typeof "hi"); // "string"
console.log(typeof 42n); // "bigint"
console.log(typeof undefined); // "undefined"
console.log(typeof null); // "object" (historical bug)
console.log(typeof []); // "object"
console.log(Array.isArray([])); // true
console.log(typeof function () {}); // "function"
```

---

## Q12. Pass by value vs pass by reference

### English

**Question:** Does JavaScript pass by value or by reference?

**Answer:** JavaScript always passes arguments by value. For primitives, the value itself is copied. For objects, the value copied is the reference (memory address), so both variables point to the same object. Mutating the object through one variable is visible through the other, but reassigning the variable does not affect the original.

### বাংলা

**প্রশ্ন:** JavaScript কি pass by value নাকি pass by reference?

**উত্তর:** JavaScript সবসময় value দিয়ে pass করে। Primitive-এর ক্ষেত্রে মানটিই copy হয়। Object-এর ক্ষেত্রে reference (memory address) copy হয়, তাই দুটি variable একই object-কে নির্দেশ করে। একটির মাধ্যমে object বদলালে অন্যটিতেও সেই পরিবর্তন দেখা যায়, কিন্তু variable-এ নতুন object বসালে মূল object-এর কিছু হয় না।

### Example

```js
let a = 10;
let b = a;
b = 20;
console.log(a); // 10

const obj1 = { name: "Sohel" };
const obj2 = obj1;
obj2.name = "Rana";
console.log(obj1.name); // "Rana" (same object)

function change(o) {
  o.age = 25; // mutates original
  o = { age: 99 }; // only changes local variable
}
change(obj1);
console.log(obj1.age); // 25
```

---

## Q13. Shallow copy vs deep copy

### English

**Question:** What is the difference between a shallow copy and a deep copy?

**Answer:** A shallow copy duplicates only the top level; nested objects are still shared by reference. Spread (`{...obj}`), `Object.assign` and `Array.from` make shallow copies. A deep copy duplicates everything, including nested objects. Use `structuredClone()` (modern, handles Date, Map, Set). `JSON.parse(JSON.stringify(obj))` also works but loses functions, `undefined`, and turns Dates into strings.

### বাংলা

**প্রশ্ন:** Shallow copy আর deep copy-র পার্থক্য কী?

**উত্তর:** Shallow copy শুধু উপরের স্তর copy করে, ভেতরের nested object গুলো আগের মতোই reference দিয়ে শেয়ার হয়। Spread (`{...obj}`), `Object.assign`, `Array.from` shallow copy বানায়। Deep copy সবকিছু, nested object সহ, নতুন করে copy করে। এর জন্য `structuredClone()` ব্যবহার করুন (Date, Map, Set সামলায়)। `JSON.parse(JSON.stringify(obj))`-ও কাজ করে, কিন্তু function ও `undefined` হারিয়ে যায় এবং Date string হয়ে যায়।

### Example

```js
const user = { name: "Sohel", address: { city: "Rajshahi" } };

const shallow = { ...user };
shallow.address.city = "Dhaka";
console.log(user.address.city); // "Dhaka" (affected!)

const deep = structuredClone(user);
deep.address.city = "Khulna";
console.log(user.address.city); // "Dhaka" (not affected)
```

---

## Q14. Destructuring, spread and rest

### English

**Question:** Explain destructuring, the spread operator and the rest parameter.

**Answer:** Destructuring extracts values from arrays or properties from objects into variables, with support for defaults and renaming. The spread operator (`...`) expands an iterable or object into individual elements, used for copying and merging. The rest parameter (`...`) does the opposite: it collects remaining elements or arguments into an array or object. Same syntax, different position and purpose.

### বাংলা

**প্রশ্ন:** Destructuring, spread operator ও rest parameter ব্যাখ্যা করুন।

**উত্তর:** Destructuring দিয়ে array-র মান বা object-এর property সরাসরি আলাদা variable-এ নেওয়া যায়, default মান ও নাম বদলানোও সম্ভব। Spread operator (`...`) কোনো array বা object-কে খুলে আলাদা আলাদা উপাদান বানায়, copy ও merge করতে কাজে লাগে। Rest parameter (`...`) উল্টো কাজ করে: বাকি উপাদান বা argument গুলোকে একটি array বা object-এ জড়ো করে। Syntax এক, কিন্তু বসার জায়গা ও উদ্দেশ্য আলাদা।

### Example

```js
const [first, , third = 0] = [1, 2]; // first=1, third=0
const { name: userName, age = 18 } = { name: "Rana" };

const merged = { ...{ a: 1 }, ...{ b: 2 } }; // spread
const nums = [...[1, 2], 3]; // [1, 2, 3]

function sum(...numbers) {
  // rest
  return numbers.reduce((t, n) => t + n, 0);
}
const [head, ...tail] = [1, 2, 3, 4]; // tail = [2, 3, 4]
```

---

## Q15. map, filter and reduce

### English

**Question:** What do `map`, `filter` and `reduce` do?

**Answer:** `map` transforms every element and returns a new array of the same length. `filter` returns a new array containing only the elements that pass a test. `reduce` folds an array into a single value (number, object, array) using an accumulator. None of them mutate the original array.

### বাংলা

**প্রশ্ন:** `map`, `filter` ও `reduce` কী কাজ করে?

**উত্তর:** `map` প্রতিটি উপাদানকে রূপান্তর করে একই দৈর্ঘ্যের নতুন array দেয়। `filter` শর্ত পূরণ করা উপাদানগুলো নিয়ে নতুন array দেয়। `reduce` একটি accumulator ব্যবহার করে পুরো array-কে একটিমাত্র মানে (সংখ্যা, object বা array) পরিণত করে। এদের কেউই মূল array বদলায় না।

### Example

```js
const products = [
  { name: "A", price: 100, inStock: true },
  { name: "B", price: 200, inStock: false },
  { name: "C", price: 300, inStock: true },
];

const names = products.map((p) => p.name); // ["A", "B", "C"]
const available = products.filter((p) => p.inStock); // A and C
const total = available.reduce((sum, p) => sum + p.price, 0); // 400

const countByStock = products.reduce((acc, p) => {
  acc[p.inStock] = (acc[p.inStock] || 0) + 1;
  return acc;
}, {});
```

---

## Q16. forEach vs map

### English

**Question:** What is the difference between `forEach` and `map`?

**Answer:** `forEach` runs a function for each element and always returns `undefined`; it is used for side effects. `map` returns a new array built from the callback's return values, so it is used for transformation and can be chained. Neither can be stopped early with `break`; use a `for...of` loop, `some` or `every` for that.

### বাংলা

**প্রশ্ন:** `forEach` আর `map`-এর পার্থক্য কী?

**উত্তর:** `forEach` প্রতিটি উপাদানের জন্য function চালায় এবং সবসময় `undefined` return করে, তাই শুধু side effect-এর কাজে লাগে (যেমন log করা)। `map` callback-এর return মান দিয়ে নতুন array বানায়, তাই রূপান্তরের কাজে লাগে এবং chain করা যায়। কোনোটিতেই `break` দিয়ে মাঝপথে থামানো যায় না; সেজন্য `for...of`, `some` বা `every` ব্যবহার করতে হয়।

### Example

```js
const nums = [1, 2, 3];

const r1 = nums.forEach((n) => n * 2);
console.log(r1); // undefined

const r2 = nums.map((n) => n * 2);
console.log(r2); // [2, 4, 6]

nums.map((n) => n * 2).filter((n) => n > 2); // chaining works with map
```

---

## Q17. Truthy and falsy values

### English

**Question:** What are truthy and falsy values?

**Answer:** In a boolean context, every value is either truthy or falsy. There are exactly 8 falsy values: `false`, `0`, `-0`, `0n`, `""`, `null`, `undefined` and `NaN`. Everything else is truthy, including `"0"`, `"false"`, `[]` and `{}`.

### বাংলা

**প্রশ্ন:** Truthy ও falsy মান কী?

**উত্তর:** Boolean প্রসঙ্গে (যেমন `if` শর্তে) প্রতিটি মান হয় truthy, নয় falsy। Falsy মান মোট ৮টি: `false`, `0`, `-0`, `0n`, `""`, `null`, `undefined` ও `NaN`। বাকি সব truthy, এমনকি `"0"`, `"false"`, `[]` ও `{}`-ও truthy।

### Example

```js
if ([]) console.log("empty array is truthy"); // runs
if ("0") console.log("'0' is truthy"); // runs
if (!"") console.log("empty string is falsy"); // runs

console.log(Boolean(NaN)); // false
console.log(!!{}); // true
```

---

## Q18. null vs undefined

### English

**Question:** What is the difference between `null` and `undefined`?

**Answer:** `undefined` means a variable has been declared but not assigned a value, or a property/function result is missing; JavaScript sets it automatically. `null` is an intentional "no value" that a developer assigns explicitly. `typeof undefined` is `"undefined"`, while `typeof null` is `"object"`. They are loosely equal (`==`) but not strictly equal (`===`).

### বাংলা

**প্রশ্ন:** `null` আর `undefined`-এর পার্থক্য কী?

**উত্তর:** `undefined` মানে variable declare করা হয়েছে কিন্তু মান দেওয়া হয়নি, অথবা property বা function-এর ফলাফল নেই; এটি JavaScript নিজে বসায়। `null` হলো ইচ্ছাকৃতভাবে "কোনো মান নেই" বোঝানো, যা developer নিজে বসায়। `typeof undefined` হলো `"undefined"`, আর `typeof null` হলো `"object"`। `==` দিয়ে দুটি সমান, কিন্তু `===` দিয়ে সমান নয়।

### Example

```js
let a;
console.log(a); // undefined

const user = { name: "Rana", address: null };
console.log(user.age); // undefined (no such property)
console.log(user.address); // null (intentionally empty)

console.log(null == undefined); // true
console.log(null === undefined); // false
```

---

## Q19. Scope and scope chain

### English

**Question:** What is scope, lexical scope and the scope chain?

**Answer:** Scope decides where a variable can be accessed: global, function or block scope. Lexical scope means a function's accessible variables are determined by where it is written in the source code. The scope chain is the lookup path: JavaScript searches the current scope first, then each outer scope in turn, up to the global scope. If not found, it throws a `ReferenceError`.

### বাংলা

**প্রশ্ন:** Scope, lexical scope ও scope chain কী?

**উত্তর:** Scope ঠিক করে কোনো variable কোথা থেকে ব্যবহার করা যাবে: global, function বা block scope। Lexical scope মানে কোনো function কোন variable দেখতে পাবে তা নির্ভর করে code-এ সেটি কোথায় লেখা হয়েছে তার উপর। Scope chain হলো খোঁজার পথ: JavaScript আগে বর্তমান scope-এ খোঁজে, না পেলে একটি একটি করে বাইরের scope-এ, শেষে global scope পর্যন্ত যায়। কোথাও না পেলে `ReferenceError` আসে।

### Example

```js
const global = "global";

function outer() {
  const outerVar = "outer";
  function inner() {
    const innerVar = "inner";
    console.log(innerVar, outerVar, global); // all accessible
  }
  inner();
  // console.log(innerVar); // ReferenceError
}
outer();

if (true) {
  let blockScoped = 1;
  var functionScoped = 2;
}
// console.log(blockScoped);   // ReferenceError
console.log(functionScoped); // 2
```

---

## Q20. Execution context and call stack

### English

**Question:** What are the execution context and the call stack?

**Answer:** An execution context is the environment where code runs. It has a creation phase (memory is allocated, variables hoisted, `this` determined) and an execution phase (code runs line by line). The global context is created first, and each function call creates a new one. The call stack is a LIFO structure that tracks these contexts; the top one is currently running. Too many nested calls cause a "Maximum call stack size exceeded" (stack overflow) error.

### বাংলা

**প্রশ্ন:** Execution context ও call stack কী?

**উত্তর:** Execution context হলো সেই পরিবেশ যেখানে code চলে। এর দুটি ধাপ: creation phase (memory বরাদ্দ, variable hoist, `this` নির্ধারণ) এবং execution phase (এক লাইন করে code চলা)। প্রথমে global context তৈরি হয়, তারপর প্রতিটি function call-এ নতুন context তৈরি হয়। Call stack হলো LIFO (শেষে ঢুকলে আগে বের হয়) কাঠামো, যা এই context গুলো ট্র্যাক করে; সবার উপরেরটি বর্তমানে চলছে। অতিরিক্ত nested call হলে "Maximum call stack size exceeded" (stack overflow) error আসে।

### Example

```js
function first() {
  second();
  console.log("first done");
}
function second() {
  third();
  console.log("second done");
}
function third() {
  console.log("third done");
}

first();
// Output: third done, second done, first done

function infinite() {
  infinite();
}
// infinite(); // RangeError: Maximum call stack size exceeded
```

---

## Q21. call, apply and bind

### English

**Question:** What is the difference between `call`, `apply` and `bind`?

**Answer:** All three set the value of `this` for a function. `call(thisArg, a, b)` invokes the function immediately with arguments passed one by one. `apply(thisArg, [a, b])` invokes immediately with arguments passed as an array. `bind(thisArg, a, b)` does not invoke; it returns a new function with `this` (and optionally some arguments) permanently fixed.

### বাংলা

**প্রশ্ন:** `call`, `apply` ও `bind`-এর পার্থক্য কী?

**উত্তর:** তিনটিই function-এর `this` ঠিক করে দেয়। `call(thisArg, a, b)` সাথে সাথে function চালায়, argument গুলো আলাদা আলাদা দিতে হয়। `apply(thisArg, [a, b])` সাথে সাথে চালায়, তবে argument গুলো array আকারে দিতে হয়। `bind(thisArg, a, b)` function চালায় না, বরং `this` (এবং চাইলে কিছু argument) স্থায়ীভাবে ঠিক করা একটি নতুন function return করে।

### Example

```js
function intro(city, country) {
  return `${this.name} from ${city}, ${country}`;
}
const person = { name: "Sohel" };

console.log(intro.call(person, "Rajshahi", "BD"));
console.log(intro.apply(person, ["Rajshahi", "BD"]));

const bound = intro.bind(person, "Rajshahi");
console.log(bound("BD"));
```

---

## Q22. Higher-order functions and callbacks

### English

**Question:** What is a higher-order function? What is a callback?

**Answer:** A higher-order function is a function that takes another function as an argument, returns a function, or both. A callback is a function passed as an argument to be called later, either synchronously (like in `map`) or asynchronously (like in `setTimeout`). This works because functions are first-class citizens in JavaScript.

### বাংলা

**প্রশ্ন:** Higher-order function কী? Callback কী?

**উত্তর:** Higher-order function হলো এমন function যা অন্য function-কে argument হিসেবে নেয়, অথবা function return করে, অথবা দুটোই করে। Callback হলো এমন function যা argument হিসেবে পাঠানো হয় এবং পরে call করা হয়, synchronous ভাবে (যেমন `map`) বা asynchronous ভাবে (যেমন `setTimeout`)। এটা সম্ভব কারণ JavaScript-এ function হলো first-class citizen, অর্থাৎ function-কে অন্য যেকোনো মানের মতো ব্যবহার করা যায়।

### Example

```js
function multiplier(factor) {
  // returns a function
  return (n) => n * factor;
}
const double = multiplier(2);
console.log(double(5)); // 10

[1, 2, 3].map(double); // map is higher-order, double is callback

setTimeout(() => console.log("later"), 1000); // async callback
```

---

## Q23. IIFE

### English

**Question:** What is an IIFE and why is it used?

**Answer:** An Immediately Invoked Function Expression is a function that is defined and executed right away. It creates a private scope, so variables inside do not pollute the global scope. It was widely used before ES6 modules and `let/const` for data privacy and for the module pattern. It is still useful for running `async` code at the top level.

### বাংলা

**প্রশ্ন:** IIFE কী এবং কেন ব্যবহার হয়?

**উত্তর:** IIFE (Immediately Invoked Function Expression) হলো এমন function যা লেখার সাথে সাথেই চলে যায়। এটি একটি private scope তৈরি করে, তাই ভেতরের variable গুলো global scope নষ্ট করে না। ES6 module ও `let/const` আসার আগে data privacy ও module pattern-এর জন্য এটি খুব ব্যবহৃত হতো। এখনো top level-এ `async` code চালাতে কাজে লাগে।

### Example

```js
(function () {
  const secret = "hidden";
  console.log("runs immediately", secret);
})();
// console.log(secret); // ReferenceError

(async () => {
  const res = await fetch("/api/data");
  console.log(await res.json());
})();
```

---

## Q24. Optional chaining, nullish coalescing and template literals

### English

**Question:** What are `?.`, `??` and template literals? How is `??` different from `||`?

**Answer:** Optional chaining (`?.`) safely accesses nested properties and returns `undefined` instead of throwing if something is `null` or `undefined`. Nullish coalescing (`??`) returns the right side only when the left side is `null` or `undefined`. `||` returns the right side for any falsy value (`0`, `""`, `false`), which can be a bug. Template literals use backticks for multi-line strings and `${}` interpolation.

### বাংলা

**প্রশ্ন:** `?.`, `??` ও template literal কী? `??` আর `||`-এর পার্থক্য কী?

**উত্তর:** Optional chaining (`?.`) nested property নিরাপদে access করে; মাঝে `null` বা `undefined` থাকলে error না দিয়ে `undefined` return করে। Nullish coalescing (`??`) শুধু তখনই ডান পাশের মান দেয় যখন বাম পাশ `null` বা `undefined`। `||` যেকোনো falsy মানের (`0`, `""`, `false`) জন্য ডান পাশ দেয়, যা অনেক সময় bug তৈরি করে। Template literal backtick দিয়ে লেখা হয়, এতে multi-line string ও `${}` দিয়ে মান বসানো যায়।

### Example

```js
const user = { profile: null, score: 0 };

console.log(user.profile?.name); // undefined (no error)
console.log(user.score || 100); // 100 (wrong! 0 is valid)
console.log(user.score ?? 100); // 0 (correct)

const name = "Sohel";
console.log(`Hello ${name},
total: ${2 + 3}`);
```

---

## Q25. `const` vs `Object.freeze`

### English

**Question:** Does `const` make an object immutable? What does `Object.freeze` do?

**Answer:** No. `const` only prevents reassigning the variable; the object's properties can still change. `Object.freeze(obj)` prevents adding, removing or changing properties of that object. However, freeze is shallow: nested objects can still be modified unless they are frozen too. In strict mode, writing to a frozen object throws an error; otherwise it fails silently.

### বাংলা

**প্রশ্ন:** `const` কি object-কে immutable করে? `Object.freeze` কী করে?

**উত্তর:** না। `const` শুধু variable-এ নতুন মান বসানো আটকায়, object-এর property বদলানো আটকায় না। `Object.freeze(obj)` ওই object-এ property যোগ, মুছে ফেলা বা পরিবর্তন করা আটকায়। তবে freeze shallow, অর্থাৎ ভেতরের nested object আলাদাভাবে freeze না করলে সেগুলো বদলানো যায়। Strict mode-এ frozen object-এ লিখতে গেলে error আসে, নইলে কিছু না বলে ব্যর্থ হয়।

### Example

```js
const user = { name: "Sohel", address: { city: "Rajshahi" } };
user.name = "Rana"; // allowed with const

Object.freeze(user);
user.name = "Changed"; // ignored (or TypeError in strict mode)
user.address.city = "Dhaka"; // still works (shallow freeze)

console.log(user.name); // "Rana"
console.log(user.address.city); // "Dhaka"
```

---

## Q26. Currying

### English

**Question:** What is currying?

**Answer:** Currying transforms a function that takes multiple arguments into a chain of functions that each take one argument: `f(a, b, c)` becomes `f(a)(b)(c)`. It enables partial application and reusable, specialized functions. It relies on closures to remember earlier arguments.

### বাংলা

**প্রশ্ন:** Currying কী?

**উত্তর:** Currying হলো একাধিক argument নেওয়া function-কে এমন function-এর শৃঙ্খলে রূপান্তর করা যেখানে প্রতিটি function একটি করে argument নেয়: `f(a, b, c)` হয়ে যায় `f(a)(b)(c)`। এর মাধ্যমে partial application ও পুনরায় ব্যবহারযোগ্য বিশেষ function বানানো যায়। আগের argument মনে রাখতে এটি closure-এর উপর নির্ভর করে।

### Example

```js
const add = (a) => (b) => (c) => a + b + c;
console.log(add(1)(2)(3)); // 6

const addTen = add(10);
console.log(addTen(5)(5)); // 20

// generic curry helper
function curry(fn) {
  return function curried(...args) {
    return args.length >= fn.length
      ? fn(...args)
      : (...next) => curried(...args, ...next);
  };
}
const sum3 = curry((a, b, c) => a + b + c);
console.log(sum3(1)(2)(3), sum3(1, 2)(3)); // 6 6
```

---

## Q27. Classes in JavaScript

### English

**Question:** How do ES6 classes work?

**Answer:** A `class` is syntax sugar over prototype-based inheritance. The `constructor` initializes new instances, `extends` sets up inheritance, `super()` calls the parent constructor (required before using `this` in a child), `static` members belong to the class itself, and `#field` declares truly private fields. Classes are not hoisted like functions (they stay in the TDZ).

### বাংলা

**প্রশ্ন:** ES6 class কীভাবে কাজ করে?

**উত্তর:** `class` আসলে prototype-ভিত্তিক inheritance-এর সহজ syntax। `constructor` নতুন instance তৈরির সময় মান বসায়, `extends` দিয়ে inheritance হয়, `super()` parent constructor call করে (child class-এ `this` ব্যবহারের আগে এটি বাধ্যতামূলক), `static` member class-এর নিজের, আর `#field` দিয়ে সত্যিকারের private field বানানো যায়। Function-এর মতো class hoist হয় না (TDZ-তে থাকে)।

### Example

```js
class Account {
  #balance = 0; // private
  static bank = "ABC Bank";

  constructor(owner) {
    this.owner = owner;
  }
  deposit(amount) {
    this.#balance += amount;
    return this;
  }
  get balance() {
    return this.#balance;
  }
}

class Savings extends Account {
  constructor(owner, rate) {
    super(owner); // must come first
    this.rate = rate;
  }
}

const acc = new Savings("Sohel", 5);
acc.deposit(500).deposit(200);
console.log(acc.balance); // 700
// console.log(acc.#balance);   // SyntaxError
```

---

## Q28. Modules: CommonJS vs ES Modules

### English

**Question:** What is the difference between CommonJS and ES Modules?

**Answer:** CommonJS (`require` / `module.exports`) is the original Node.js system: it loads modules synchronously at runtime and can be called conditionally. ES Modules (`import` / `export`) are the official standard: they are statically analyzed (enabling tree shaking), support top-level `await`, and work in both browsers and Node.js (with `"type": "module"` or `.mjs`). ES Modules also use live bindings, while CommonJS exports a copy of the value.

### বাংলা

**প্রশ্ন:** CommonJS আর ES Modules-এর পার্থক্য কী?

**উত্তর:** CommonJS (`require` / `module.exports`) হলো Node.js-এর আদি ব্যবস্থা: runtime-এ synchronous ভাবে module লোড করে এবং শর্ত দিয়ে call করা যায়। ES Modules (`import` / `export`) হলো অফিসিয়াল স্ট্যান্ডার্ড: এগুলো আগে থেকেই বিশ্লেষণ করা যায় (তাই tree shaking সম্ভব), top-level `await` সমর্থন করে, এবং browser ও Node.js (`"type": "module"` বা `.mjs` দিয়ে) দুই জায়গাতেই চলে। ES Modules-এ live binding থাকে, আর CommonJS মানের একটি copy export করে।

### Example

```js
// CommonJS
const fs = require("fs");
module.exports = { add: (a, b) => a + b };

// ES Modules
import fs from "fs";
export const add = (a, b) => a + b;
export default function greet() {
  return "hi";
}

// import { add } from "./math.js";
// import greet from "./greet.js";
```

---

## Q29. Set and Map

### English

**Question:** What are `Set` and `Map`? How are they different from arrays and objects?

**Answer:** A `Set` stores unique values of any type, so duplicates are removed automatically, and `has()` lookup is fast. A `Map` stores key-value pairs where keys can be any type (objects, functions), keeps insertion order, and has a `size` property. A plain object only allows string/symbol keys and has inherited prototype properties. `WeakMap`/`WeakSet` hold keys weakly so they can be garbage collected.

### বাংলা

**প্রশ্ন:** `Set` ও `Map` কী? Array ও object থেকে এরা কীভাবে আলাদা?

**উত্তর:** `Set` যেকোনো type-এর unique মান রাখে, তাই duplicate নিজে থেকেই বাদ পড়ে এবং `has()` দিয়ে দ্রুত খোঁজা যায়। `Map` key-value জোড়া রাখে যেখানে key যেকোনো type-এর (object, function সহ) হতে পারে, insertion-এর ক্রম মনে রাখে এবং `size` property আছে। সাধারণ object-এ key শুধু string বা symbol হতে পারে এবং prototype থেকে আসা property থাকে। `WeakMap`/`WeakSet` key-কে দুর্বলভাবে ধরে রাখে, তাই garbage collect হতে পারে।

### Example

```js
const unique = new Set([1, 2, 2, 3, 3]);
console.log([...unique]); // [1, 2, 3]
console.log(unique.has(2)); // true

const map = new Map();
const keyObj = { id: 1 };
map.set(keyObj, "object as key");
map.set("name", "Sohel");
console.log(map.get(keyObj)); // "object as key"
console.log(map.size); // 2

for (const [key, value] of map) console.log(key, value);
```

---

## Q30. `slice` vs `splice`

### English

**Question:** What is the difference between `slice` and `splice`?

**Answer:** `slice(start, end)` returns a shallow copy of a portion of the array and does not change the original. `splice(start, deleteCount, ...items)` changes the original array by removing and/or inserting elements, and returns the removed elements. Other mutating methods include `push`, `pop`, `shift`, `unshift`, `sort` and `reverse`; non-mutating alternatives are `slice`, `concat`, `map`, `filter`, `toSorted` and `toReversed`.

### বাংলা

**প্রশ্ন:** `slice` আর `splice`-এর পার্থক্য কী?

**উত্তর:** `slice(start, end)` array-র একটি অংশের shallow copy return করে, মূল array বদলায় না। `splice(start, deleteCount, ...items)` মূল array বদলায়: উপাদান মুছে ফেলে এবং/অথবা নতুন উপাদান বসায়, আর যেগুলো মুছেছে সেগুলো return করে। অন্যান্য যেসব method মূল array বদলায় সেগুলো হলো `push`, `pop`, `shift`, `unshift`, `sort`, `reverse`; না বদলানো বিকল্প হলো `slice`, `concat`, `map`, `filter`, `toSorted`, `toReversed`।

### Example

```js
const arr = [1, 2, 3, 4, 5];

console.log(arr.slice(1, 3)); // [2, 3]
console.log(arr); // [1, 2, 3, 4, 5] (unchanged)

const removed = arr.splice(1, 2, "a", "b", "c");
console.log(removed); // [2, 3]
console.log(arr); // [1, "a", "b", "c", 4, 5] (changed)
```
