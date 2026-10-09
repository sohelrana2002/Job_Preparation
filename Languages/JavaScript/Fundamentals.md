# JavaScript Fundamentals

---

## Q1. `var`, `let` and `const`

### English

**Question:** What is the difference between `var`, `let` and `const`?

**Answer:**

- `var` is function-scoped, can be redeclared and updated, and is hoisted with the value `undefined`.
- `let` is block-scoped, cannot be redeclared in the same scope, but can be updated. It is hoisted but stays in the Temporal Dead Zone (TDZ) until its declaration line.
- `const` is block-scoped, must be initialized at declaration, and cannot be reassigned. For objects and arrays, the reference is fixed but the contents can still be mutated.

### বাংলা

**প্রশ্ন:** `var`, `let` আর `const`-এর মধ্যে পার্থক্য কী?

**উত্তর:**

- `var` function-scoped। একই নাম আবার declare করা যায়, মান পরিবর্তন করা যায়। Hoisting-এর সময় এর মান `undefined` থাকে।
- `let` block-scoped। একই scope-এ আবার declare করা যায় না, কিন্তু মান পরিবর্তন করা যায়। Hoisting হয়, তবে declaration লাইনের আগে পর্যন্ত Temporal Dead Zone-এ থাকে, তাই আগে ব্যবহার করলে error আসে।
- `const` block-scoped। declare করার সময়ই মান দিতে হয় এবং পরে নতুন মান বসানো যায় না। তবে object বা array হলে ভেতরের মান বদলানো যায়, শুধু reference বদলানো যায় না।

### Example

```js
console.log(a); // undefined (var is hoisted)
var a = 10;

// console.log(b); // ReferenceError (TDZ)
let b = 20;

const user = { name: "Rana" };
user.name = "Sohel"; // allowed
// user = {};          // TypeError: Assignment to constant variable
```

---

## Q2. Hoisting

### English

**Question:** What is hoisting in JavaScript?

**Answer:** Hoisting is JavaScript's behavior of moving declarations to the top of their scope during the creation phase of the execution context, before the code runs. Function declarations are fully hoisted (name and body). `var` variables are hoisted and initialized with `undefined`. `let` and `const` are hoisted but not initialized, so accessing them early throws a `ReferenceError`. Function expressions follow the rule of the variable that holds them.

### বাংলা

**প্রশ্ন:** JavaScript-এ hoisting কী?

**উত্তর:** Code চালানোর আগে execution context তৈরির সময় JavaScript declaration গুলোকে তার scope-এর একদম উপরে নিয়ে যায়, একেই hoisting বলে। Function declaration পুরোপুরি (নাম ও body সহ) hoist হয়, তাই declare করার আগেই call করা যায়। `var` hoist হয় এবং মান হয় `undefined`। `let` ও `const` hoist হয় কিন্তু initialize হয় না, তাই আগে ব্যবহার করলে `ReferenceError` আসে। Function expression-এ যে variable-এ রাখা হয়েছে সেই variable-এর নিয়ম খাটে।

### Example

```js
sayHi(); // works: "Hi"
function sayHi() {
  console.log("Hi");
}

sayHello(); // TypeError: sayHello is not a function
var sayHello = function () {
  console.log("Hello");
};
```

---

## Q3. Closure

### English

**Question:** What is a closure? Give a practical use case.

**Answer:** A closure is a function that remembers the variables of the scope where it was created, even after that outer function has finished executing. Closures are used for data privacy (private variables), function factories, memoization, debounce/throttle, and keeping state in callbacks.

### বাংলা

**প্রশ্ন:** Closure কী? একটি বাস্তব ব্যবহার বলুন।

**উত্তর:** Closure হলো এমন একটি function, যে তার তৈরি হওয়ার জায়গার (outer scope) variable গুলো মনে রাখে, এমনকি outer function-এর কাজ শেষ হয়ে গেলেও। Closure দিয়ে private variable বানানো, function factory, memoization, debounce/throttle এবং callback-এ state ধরে রাখার কাজ করা হয়।

### Example

```js
function createCounter() {
  let count = 0; // private variable
  return {
    increment: () => ++count,
    getCount: () => count,
  };
}

const counter = createCounter();
counter.increment();
counter.increment();
console.log(counter.getCount()); // 2
console.log(counter.count); // undefined (cannot access directly)
```

---

## Q4. `this` keyword

### English

**Question:** How does the `this` keyword work in JavaScript?

**Answer:** The value of `this` is decided by how a function is called, not where it is written. In a method call, `this` is the object before the dot. In a plain function call, it is `undefined` in strict mode (or the global object otherwise). With `call`, `apply` or `bind`, it is set explicitly. With `new`, it is the newly created object. Arrow functions have no own `this`; they take it from the surrounding scope.

### বাংলা

**প্রশ্ন:** JavaScript-এ `this` কীভাবে কাজ করে?

**উত্তর:** `this`-এর মান নির্ভর করে function কীভাবে call করা হয়েছে তার উপর, কোথায় লেখা হয়েছে তার উপর নয়। Method call-এ `this` হলো dot-এর আগের object। সাধারণ function call-এ strict mode-এ `undefined`, না হলে global object। `call`, `apply`, `bind` দিয়ে `this` নিজে ঠিক করে দেওয়া যায়। `new` দিয়ে call করলে `this` হলো নতুন তৈরি object। Arrow function-এর নিজস্ব `this` নেই, সে বাইরের scope থেকে `this` নেয়।

### Example

```js
const user = {
  name: "Sohel",
  regular() {
    return this.name;
  },
  arrow: () => typeof this,
};

console.log(user.regular()); // "Sohel"

const detached = user.regular;
// detached();               // this is lost (undefined in strict mode)

const bound = user.regular.bind({ name: "Rana" });
console.log(bound()); // "Rana"
```

---

## Q5. `==` vs `===`

### English

**Question:** What is the difference between `==` and `===`?

**Answer:** `==` is loose equality: it converts (coerces) the operands to the same type before comparing. `===` is strict equality: it compares both value and type with no conversion. Prefer `===` to avoid surprising results. `Object.is` is a third option that also distinguishes `NaN` and `-0`.

### বাংলা

**প্রশ্ন:** `==` আর `===`-এর মধ্যে পার্থক্য কী?

**উত্তর:** `==` হলো loose equality, তুলনার আগে দুই দিকের মানকে একই type-এ রূপান্তর (type coercion) করে। `===` হলো strict equality, এখানে মান ও type দুটোই মিলতে হয় এবং কোনো রূপান্তর হয় না। অপ্রত্যাশিত ফলাফল এড়াতে সাধারণত `===` ব্যবহার করা উচিত।

### Example

```js
console.log(5 == "5"); // true  (string converted to number)
console.log(5 === "5"); // false (different types)
console.log(null == undefined); // true
console.log(null === undefined); // false
console.log(NaN === NaN); // false
console.log(Object.is(NaN, NaN)); // true
```

---

## Q6. Event Loop

### English

**Question:** Explain the JavaScript event loop.

**Answer:** JavaScript is single-threaded and has one call stack. Asynchronous work (timers, network requests) is handled by the browser or Node.js APIs. When it finishes, its callback is placed in a queue. The event loop continuously checks: if the call stack is empty, it first runs all tasks in the microtask queue (promise callbacks, `queueMicrotask`), and then takes one task from the macrotask queue (`setTimeout`, `setInterval`, I/O). Microtasks always have higher priority than macrotasks.

### বাংলা

**প্রশ্ন:** JavaScript-এর event loop ব্যাখ্যা করুন।

**উত্তর:** JavaScript single-threaded, অর্থাৎ একটাই call stack। Asynchronous কাজ (timer, network request) browser বা Node.js-এর API সামলায়। কাজ শেষ হলে তার callback একটি queue-তে জমা হয়। Event loop বারবার দেখে call stack খালি কি না। খালি হলে আগে microtask queue-র সব কাজ (promise callback, `queueMicrotask`) চালায়, তারপর macrotask queue (`setTimeout`, `setInterval`, I/O) থেকে একটি কাজ নেয়। Microtask-এর অগ্রাধিকার সবসময় macrotask-এর চেয়ে বেশি।

### Example

```js
console.log("1");
setTimeout(() => console.log("2 (macrotask)"), 0);
Promise.resolve().then(() => console.log("3 (microtask)"));
console.log("4");

// Output: 1, 4, 3 (microtask), 2 (macrotask)
```

---

## Q7. Promises

### English

**Question:** What is a Promise and what are its states?

**Answer:** A Promise is an object representing the eventual completion or failure of an asynchronous operation and its resulting value. It has three states: `pending` (initial), `fulfilled` (completed successfully) and `rejected` (failed). Once settled, a promise cannot change state. You handle results with `.then()`, `.catch()` and `.finally()`. Useful combinators are `Promise.all`, `Promise.allSettled`, `Promise.race` and `Promise.any`.

### বাংলা

**প্রশ্ন:** Promise কী এবং এর অবস্থাগুলো কী কী?

**উত্তর:** Promise হলো একটি object, যা কোনো asynchronous কাজের ভবিষ্যৎ সফলতা বা ব্যর্থতা এবং তার ফলাফলকে প্রতিনিধিত্ব করে। এর তিনটি অবস্থা: `pending` (শুরুর অবস্থা), `fulfilled` (সফলভাবে শেষ) এবং `rejected` (ব্যর্থ)। একবার settled হয়ে গেলে অবস্থা আর বদলায় না। ফলাফল সামলাতে `.then()`, `.catch()`, `.finally()` ব্যবহার করা হয়। একসাথে অনেক promise সামলাতে `Promise.all`, `Promise.allSettled`, `Promise.race`, `Promise.any` আছে।

### Example

```js
const wait = (ms, ok = true) =>
  new Promise((resolve, reject) =>
    setTimeout(() => (ok ? resolve("done") : reject(new Error("failed"))), ms),
  );

wait(500)
  .then((value) => console.log(value)) // "done"
  .catch((err) => console.error(err))
  .finally(() => console.log("finished"));

Promise.all([wait(100), wait(200)]).then(console.log); // ["done", "done"]
```

---

## Q8. async / await

### English

**Question:** How do `async/await` work, and how do you handle errors?

**Answer:** `async/await` is syntax sugar over promises. An `async` function always returns a promise. `await` pauses the execution of that async function (not the whole program) until the promise settles, then returns its value or throws its error. Errors are handled with `try/catch`. To run independent tasks in parallel, start them together and use `Promise.all` instead of awaiting one by one.

### বাংলা

**প্রশ্ন:** `async/await` কীভাবে কাজ করে এবং error কীভাবে সামলাবেন?

**উত্তর:** `async/await` হলো promise-এর উপর তৈরি সহজ syntax। `async` function সবসময় একটি promise return করে। `await` ওই async function-এর execution সাময়িক থামিয়ে রাখে (পুরো program নয়) যতক্ষণ না promise settled হয়, তারপর তার মান দেয় বা error throw করে। Error সামলাতে `try/catch` ব্যবহার করা হয়। একে অপরের উপর নির্ভরশীল না হলে কাজগুলো একসাথে শুরু করে `Promise.all` দিয়ে parallel চালানো ভালো, একটার পর একটা `await` করলে সময় বেশি লাগে।

### Example

```js
async function loadData() {
  try {
    const [users, posts] = await Promise.all([
      fetch("/api/users").then((r) => r.json()),
      fetch("/api/posts").then((r) => r.json()),
    ]);
    return { users, posts };
  } catch (error) {
    console.error("Request failed:", error.message);
    throw error;
  }
}
```

---

## Q9. Arrow functions vs regular functions

### English

**Question:** How are arrow functions different from regular functions?

**Answer:**

- Arrow functions do not have their own `this`, they inherit it lexically.
- They have no `arguments` object (use rest parameters instead).
- They cannot be used as constructors with `new`.
- They have no `prototype` property.
- They allow a shorter syntax with implicit return.

### বাংলা

**প্রশ্ন:** Arrow function আর সাধারণ function-এর পার্থক্য কী?

**উত্তর:**

- Arrow function-এর নিজস্ব `this` নেই, বাইরের scope থেকে নেয়।
- `arguments` object নেই (এর বদলে rest parameter ব্যবহার করতে হয়)।
- `new` দিয়ে constructor হিসেবে ব্যবহার করা যায় না।
- `prototype` property নেই।
- ছোট syntax এবং implicit return সুবিধা আছে।

### Example

```js
function Timer() {
  this.seconds = 0;
  setInterval(() => {
    this.seconds++; // arrow keeps `this` of Timer
  }, 1000);
}

const add = (a, b) => a + b; // implicit return
const sum = (...nums) => nums.reduce((t, n) => t + n, 0);

// const Person = () => {};
// new Person();                    // TypeError: Person is not a constructor
```

---

## Q10. Prototype and prototypal inheritance

### English

**Question:** What is the prototype chain?

**Answer:** Every JavaScript object has an internal link to another object called its prototype. When you access a property, JavaScript looks at the object itself first, then follows the prototype chain upward until it finds the property or reaches `null`. This is how inheritance works in JavaScript. ES6 `class` syntax is just a cleaner way to write the same prototype-based mechanism.

### বাংলা

**প্রশ্ন:** Prototype chain কী?

**উত্তর:** JavaScript-এর প্রতিটি object-এর ভেতরে আরেকটি object-এর সাথে একটি লুকানো সংযোগ থাকে, তাকে prototype বলে। কোনো property খুঁজতে গেলে JavaScript আগে object-টির নিজের মধ্যে দেখে, না পেলে prototype chain ধরে উপরের দিকে খুঁজতে থাকে, যতক্ষণ না পায় অথবা `null`-এ পৌঁছায়। JavaScript-এ inheritance এভাবেই কাজ করে। ES6-এর `class` আসলে একই prototype-ভিত্তিক ব্যবস্থার সহজ লেখার ধরন।

### Example

```js
function Animal(name) {
  this.name = name;
}
Animal.prototype.speak = function () {
  return `${this.name} makes a sound`;
};

const dog = new Animal("Tommy");
console.log(dog.speak()); // found on prototype
console.log(dog.hasOwnProperty("speak")); // false
console.log(Object.getPrototypeOf(dog) === Animal.prototype); // true

class Cat extends Animal {
  speak() {
    return `${this.name} meows`;
  }
}
console.log(new Cat("Kitty").speak()); // "Kitty meows"
```

---

## Next Questions To Add (5 more for this file)

- Q11. Shallow copy vs deep copy
- Q12. Destructuring, spread and rest
- Q13. `map`, `filter`, `reduce`
- Q14. Falsy and truthy values
- Q15. Template literals, optional chaining, nullish coalescing
