# JAVASCRIPT Interview Question

# 1. Core Concepts & Basics

## Q1. What are the different data types in JavaScript?

**Answer:** JavaScript has 8 data types: 7 primitive types `(string, number, bigint, boolean, undefined, symbol, null)` and 1 non-primitive type `(object)`. Primitives are immutable and stored by value; objects are mutable and stored by reference.

**Example:**

```javascript
console.log(typeof "Hello"); // "string"
console.log(typeof 42); // "number"
console.log(typeof true); // "boolean"
console.log(typeof undefined); // "undefined"
console.log(typeof null); // "object" (historical bug)
console.log(typeof {}); // "object"
```

## Q1 (বাংলা): জাভাস্ক্রিপ্টে ডেটা টাইপগুলো কী কী?

**উত্তর:** জাভাস্ক্রিপ্টে ৮টি ডেটা টাইপ আছে: ৭টি প্রিমিটিভ `(string, number, bigint, boolean, undefined, symbol, null)` এবং ১টি নন-প্রিমিটিভ `(object)`। প্রিমিটিভ ইমিউটেবল (পরিবর্তনযোগ্য নয়) এবং ভ্যালু দ্বারা সংরক্ষিত হয়; অবজেক্ট মিউটেবল এবং রেফারেন্স দ্বারা সংরক্ষিত হয়।

---

## Q2: Explain var, let, and const.

**Answer:** var is function-scoped, hoisted, and can be redeclared. let is block-scoped, hoisted but not initialized (Temporal Dead Zone), and cannot be redeclared in the same scope. const is block-scoped, must be initialized at declaration, and cannot be reassigned (though objects/arrays can be mutated).

**Example:**

```javascript
var a = 1; // function-scoped
let b = 2; // block-scoped
const c = 3; // block-scoped, cannot reassign
c = 4; // TypeError: Assignment to constant variable
const obj = { x: 1 };
obj.x = 2; // Allowed - object mutation
```

## Q2 (বাংলা): var, let এবং const ব্যাখ্যা কর।

**উত্তর:** var ফাংশন-স্কোপড, হোইস্টেড এবং পুনরায় ডিক্লেয়ার করা যায়। let ব্লক-স্কোপড, হোইস্টেড কিন্তু ইনিশিয়ালাইজ করা হয় না (Temporal Dead Zone), এবং একই স্কোপে পুনরায় ডিক্লেয়ার করা যায় না। const ব্লক-স্কোপড, ডিক্লেয়ারেশনে ইনিশিয়ালাইজ করতে হয়, এবং পুনরায় অ্যাসাইন করা যায় না (তবে অবজেক্ট/অ্যারে মিউটেট করা যায়)।

---

## Q3: What is hoisting in JavaScript?

**Answer:** Hoisting is JavaScript's default behavior of moving declarations to the top of their scope during compilation. var declarations and function declarations are hoisted. let and const are hoisted but remain in the Temporal Dead Zone until execution reaches their declaration.

**Example:**

```javascript
console.log(x); // undefined (var is hoisted)
var x = 5;

console.log(y); // ReferenceError: Cannot access 'y' before initialization
let y = 10;
```

## Q3 (বাংলা): জাভাস্ক্রিপ্টে হোইস্টিং কী?

**উত্তর:** হোইস্টিং হলো জাভাস্ক্রিপ্টের ডিফল্ট আচরণ যেখানে কম্পাইলেশনের সময় ডিক্লেয়ারেশনগুলো তাদের স্কোপের শীর্ষে নিয়ে যাওয়া হয়। var ডিক্লেয়ারেশন এবং ফাংশন ডিক্লেয়ারেশন হোইস্টেড হয়। let এবং const হোইস্টেড হয় কিন্তু তাদের ডিক্লেয়ারেশনে পৌঁছানোর আগ পর্যন্ত Temporal Dead Zone-এ থাকে।

---

## Q4. What is the difference between `==` and `===`?

**Answer:** `==` (abstract equality) performs type coercion before comparison. `===` (strict equality) checks both value and type without coercion. Always prefer === to avoid unexpected results.

**Example:**

```javascript
console.log(1 == "1"); // true (type coercion)
console.log(1 === "1"); // false (different types)
console.log(null == undefined); // true
console.log(null === undefined); // false
```

## Q4 (বাংলা): `==` এবং `===`-এর মধ্যে পার্থক্য কী?

**উত্তর:** `==` (abstract equality) তুলনার আগে টাইপ কোয়েরশন করে। `===` (strict equality) টাইপ কোয়েরশন ছাড়াই মান এবং টাইপ উভয়ই চেক করে। অপ্রত্যাশিত ফলাফল এড়াতে সবসময় `===` ব্যবহার করা ভালো।

Note: (Type Coercion) - টাইপ পরিবর্তন

---

## Q5. What is the difference between `null` and `undefined`?

**Answer:** `undefined` means a variable has been declared but not assigned a value. `null` is an assignment value that represents no value or empty object reference. `typeof null` returns `"object"` (historical bug), while `typeof undefined` returns `"undefined"`.

**Example:**

```javascript
let a;
console.log(a); // undefined
let b = null;
console.log(b); // null
console.log(typeof null); // "object"
console.log(typeof undefined); // "undefined"
```

## Q5 (বাংলা): `null` এবং `undefined`-এর মধ্যে পার্থক্য কী?

**উত্তর:** `undefined` মানে একটি ভেরিয়েবল ডিক্লেয়ার করা হয়েছে কিন্তু মান অ্যাসাইন করা হয়নি। `null` হলো একটি অ্যাসাইনমেন্ট ভ্যালু যা কোন মান নেই বা খালি অবজেক্ট রেফারেন্স বোঝায়। `typeof null` রিটার্ন করে `"object"` (ঐতিহাসিক বাগ), আর `typeof undefined` রিটার্ন করে `"undefined"`।

---

## Q6. Explain primitive vs reference types.

**Answer:** Primitive types `(string, number, boolean, etc.)` are stored directly in the variable location and copied by value. Reference types `(objects, arrays, functions)` store a reference `(memory address)` to the actual data; copying a reference type copies the reference, not the object itself.

**Example:**

```javascript
// Primitive (by value)
let a = 10;
let b = a;
b = 20;
console.log(a); // 10 (unchanged)

// Reference (by reference)
let obj1 = { x: 1 };
let obj2 = obj1;
obj2.x = 2;
console.log(obj1.x); // 2 (changed)
```

**Explaination: Primitive (Pass by Value)**

- কী ঘটছে: `JavaScript-এ Number, String, Boolean` ইত্যাদির মতো `Primitive` ডাটা টাইপগুলো সরাসরি মেমোরির নির্দিষ্ট জায়গায় মান `(Value)` স্টোর করে।

- ধাপ ১: a তৈরি হলে মেমোরিতে 10 জমা হয়।

- ধাপ ২: b = a করার সময় a-এর মানটির (10) একটি সম্পূর্ণ আলাদা কপি তৈরি হয়ে b-তে জমা হয়।

- ধাপ ৩: b = 20 করলে শুধু b-এর মান পরিবর্তন হয়। a এবং b মেমোরিতে দুটি ভিন্ন জায়গায় থাকায় a-এর মানে কোনো প্রভাব পড়ে না।

**Explanation: Reference (Pass by Reference)**

- কী ঘটছে: `Object, Array, Function` ইত্যাদি `Non-Primitive / Reference` টাইপগুলো সরাসরি মান স্টোর করে না; তারা মেমোরিতে অবজেক্টের ঠিকানা বা রেফারেন্স (Memory Address) স্টোর করে।

- ধাপ ১: obj1 তৈরি হলে মূল { x: 1 } অবজেক্টটি মেমোরির একটি নির্দিষ্ট জায়গায় জমা হয় এবং obj1-এর কাছে তার পয়েন্টার বা ঠিকানা থাকে।

- ধাপ ২: obj2 = obj1 করার সময় মূল অবজেক্টের কপি হয় না, বরং একই মেমোরি ঠিকানাটি obj2-এর সাথে শেয়ার করা হয়। অর্থাৎ obj1 এবং obj2 এখন একই ঘরকে নির্দেশ করছে।

- ধাপ ৩: obj2.x = 2 করার মানে হলো আপনি ঐ শেয়ার করা ঘরের ভেতরের মান পরিবর্তন করছেন।

- ধাপ ৪: ফলে obj1.x দিয়ে চেক করলেও পরিবর্তিত মান 2-ই দেখা যাবে, কারণ দুজনই একই মেমোরি এড্রেস দেখতে পাচ্ছে।

## Q6 (বাংলা): প্রিমিটিভ এবং রেফারেন্স টাইপের মধ্যে পার্থক্য ব্যাখ্যা কর।

**উত্তর:** প্রিমিটিভ টাইপ `(string, number, boolean ইত্যাদি)` সরাসরি ভেরিয়েবল লোকেশনে সংরক্ষিত হয় এবং ভ্যালু দ্বারা কপি হয়। রেফারেন্স টাইপ `(অবজেক্ট, অ্যারে, ফাংশন)` আসল ডেটার রেফারেন্স `(মেমরি অ্যাড্রেস)` সংরক্ষণ করে; রেফারেন্স টাইপ কপি করলে রেফারেন্স কপি হয়, অবজেক্ট নয়।

---

## Q7. What are template literals?

**Answer:** Template literals are string literals using backticks (`) that allow embedded expressions (${expression}), multi-line strings, and tagged templates.

**Example:**

```javascript
const name = "John";
const age = 30;
// Multi-line and expression interpolation
const message = `Hello ${name},
You are ${age} years old.`;
console.log(message);
```

## Q7 (বাংলা): টেমপ্লেট লিটারেল কী?

**উত্তর:** টেমপ্লেট লিটারেল হলো ব্যাকটিক (`) ব্যবহার করা স্ট্রিং লিটারেল যা এমবেডেড এক্সপ্রেশন (${expression}), মাল্টি-লাইন স্ট্রিং এবং ট্যাগড টেমপ্লেট সমর্থন করে।

---

## Q8. What is type coercion?

**Answer:** Type coercion is the automatic or implicit conversion of values from one data type to another `(e.g., string to number)` in JavaScript, often occurring with operators like `+ or ==`.

**Example:**

```javascript
console.log("5" + 3); // '53' (number coerced to string)
console.log("5" - 3); // 2 (string coerced to number)
console.log(true + 1); // 2 (true coerced to 1)
```

## Q8 (বাংলা): টাইপ কোয়েরশন কী?

**উত্তর:** টাইপ কোয়েরশন হলো জাভাস্ক্রিপ্টে মানসমূহের এক ডেটা টাইপ থেকে অন্য ডেটা টাইপে স্বয়ংক্রিয় বা ইম্প্লিসিট রূপান্তর `(যেমন স্ট্রিং থেকে নাম্বার)`, যা সাধারণত `+ বা ==` অপারেটরের সাথে ঘটে।

---

## Q9. Difference between `typeof` and `instanceof`.

**Answer:** `typeof` returns a string indicating the primitive type of a variable. `instanceof` checks if an object is an instance of a specific constructor or class (prototype chain check).

**Example:**

```javascript
console.log(typeof []); // "object"
console.log([] instanceof Array); // true
console.log([] instanceof Object); // true
console.log(typeof null); // "object" (bug)
```

## Q9 (বাংলা): `typeof` এবং `instanceof`-এর মধ্যে পার্থক্য কী?

**উত্তর:** `typeof` একটি ভেরিয়েবলের প্রিমিটিভ টাইপ নির্দেশক স্ট্রিং রিটার্ন করে। `instanceof` চেক করে একটি অবজেক্ট নির্দিষ্ট কনস্ট্রাক্টর বা ক্লাসের ইনস্ট্যান্স কিনা (প্রোটোটাইপ চেইন চেক)।

---

## Q10. What is `NaN` and how to check if a value is `NaN`?

**Answer:** NaN (Not-a-Number) is a numeric value that represents an invalid number. It is the only value in JavaScript that is not equal to itself. Use `Number.isNaN()` (ES6) or `isNaN()` (with caution) to check.

**Example:**

```javascript
console.log(0 / 0); // NaN
console.log(parseInt("abc")); // NaN
console.log(NaN === NaN); // false
console.log(Number.isNaN(NaN)); // true
console.log(isNaN("abc")); // true (coerces to number first)
```

## Q10 (বাংলা): NaN কী এবং কীভাবে একটি মান NaN কিনা চেক করবে?

**উত্তর:** NaN (Not-a-Number) একটি সংখ্যাসূচক মান যা একটি অবৈধ সংখ্যা নির্দেশ করে। জাভাস্ক্রিপ্টে এটি একমাত্র মান যা নিজের সমান নয়। চেক করতে `Number.isNaN()` (ES6) বা `isNaN()` (সতর্কতার সাথে) ব্যবহার কর।

---

## Q11. What are `arrow functions` and how are they different from `regular functions`?

**Answer:** Arrow functions (`() => {}`) are a shorter syntax. They don't have their own `this`, `arguments`, `super`, or `new.target`. They cannot be used as constructors. `this` is lexically bound (inherits from the surrounding scope).

**Example:**

```javascript
const regular = function () {
  console.log(this);
};
const arrow = () => {
  console.log(this);
};

const obj = { name: "test" };
regular.call(obj); // { name: 'test' }
arrow.call(obj); // window/global (lexical this)
```

# 2. Functions

## Q11 (বাংলা): `অ্যারো ফাংশন` কী এবং `রেগুলার ফাংশন` থেকে কীভাবে আলাদা?

**উত্তর:** অ্যারো ফাংশন (`() => {}`) একটি ছোট সিনট্যাক্স। তাদের নিজস্ব `this`, `arguments`, `super`, বা `new.target` নেই। এগুলি কনস্ট্রাক্টর হিসেবে ব্যবহার করা যায় না। `this` লেক্সিক্যালি বাউন্ড হয় (পার্শ্ববর্তী স্কোপ থেকে উত্তরাধিকারসূত্রে প্রাপ্ত)।

---

## Q12. Explain function declaration vs function expression.

**Answer:** Function declarations are hoisted (can be called before declaration). Function expressions are not hoisted (only the variable declaration is hoisted, not the assignment).

**Example:**

```javascript
// Function Declaration (hoisted)
sayHello(); // "Hello"
function sayHello() {
  console.log("Hello");
}

// Function Expression (not hoisted)
sayHi(); // TypeError: sayHi is not a function
var sayHi = function () {
  console.log("Hi");
};
```

## Q12 (বাংলা): ফাংশন ডিক্লেয়ারেশন এবং ফাংশন এক্সপ্রেশনের মধ্যে পার্থক্য ব্যাখ্যা কর।

**উত্তর:** ফাংশন ডিক্লেয়ারেশন হোইস্টেড হয় (ডিক্লেয়ারেশনের আগে কল করা যায়)। ফাংশন এক্সপ্রেশন হোইস্টেড হয় না (শুধু ভেরিয়েবল ডিক্লেয়ারেশন হোস্টেড হয়, অ্যাসাইনমেন্ট নয়)।

---

## Q13. What is the difference between `call`, `apply`, and `bind`?

**Answer:** All three change the `this` context. `call` invokes the function with arguments passed individually. `apply` invokes with arguments as an `array`. `bind` returns a new function with a bound `this` (does not invoke immediately).

**Example:**

```javascript
function greet(greeting, punctuation) {
  console.log(greeting + ", " + this.name + punctuation);
}
const person = { name: "John" };

greet.call(person, "Hello", "!"); // Hello, John!
greet.apply(person, ["Hi", "?"]); // Hi, John?
const bound = greet.bind(person, "Hey");
bound("..."); // Hey, John...
```

## Q13 (বাংলা): `call`, `apply` এবং `bind`-এর মধ্যে পার্থক্য কী?

**উত্তর:** তিনটিই `this` কনটেক্সট পরিবর্তন করে। `call` ফাংশনকে পৃথক আর্গুমেন্ট সহ ইনভোক করে। `apply` আর্গুমেন্টগুলি অ্যারে হিসেবে ইনভোক করে। `bind` বাউন্ড `this` সহ একটি নতুন ফাংশন রিটার্ন করে (তাৎক্ষণিকভাবে ইনভোক করে না)

---

## Q14. What is a `higher-order` function?

**Answer:** A higher-order function is a function that takes one or more functions as arguments, returns a function, or both. Examples: `map`, `filter`, `reduce`.

**Example:**

```javascript
function higherOrder(fn) {
  return function (x) {
    return fn(x) * 2;
  };
}
const double = higherOrder((x) => x + 1);
console.log(double(5)); // 12
```

## Q14 (বাংলা): হাইয়ার-অর্ডার ফাংশন কী?

**উত্তর:** হাইয়ার-অর্ডার ফাংশন হলো এমন একটি ফাংশন যা এক বা একাধিক ফাংশনকে আর্গুমেন্ট হিসেবে নেয়, একটি ফাংশন রিটার্ন করে, অথবা উভয়ই করে। উদাহরণ: `map`, `filter`, `reduce`।

---

## Q15. What is a pure function?

**Answer:** A pure function always returns the same output for the same input, has no side effects (does not modify external state), and does not depend on external state.

**Example:**

```javascript
// Pure function
function add(a, b) {
  return a + b;
}

// Impure function (side effect)
let counter = 0;
function increment() {
  counter++;

  return counter;
}
```

## Q15 (বাংলা): পিওর ফাংশন কী?

**উত্তর:** পিওর ফাংশন সবসময় একই ইনপুটের জন্য একই আউটপুট রিটার্ন করে, কোন সাইড ইফেক্ট নেই (বাহ্যিক স্টেট পরিবর্তন করে না), এবং বাহ্যিক স্টেটের উপর নির্ভর করে না।

---

## Q16. Explain closures in JavaScript.

**Answer:** A closure is a function that remembers and accesses variables from its outer (lexical) scope even after the outer function has finished execution. It is created when a function is defined inside another function and references the outer function's variables.

**Example:**

```javascript
function outer() {
  let count = 0;

  return function inner() {
    count++;
    return count;
  };
}

const counter = outer();
console.log(counter()); // 1
console.log(counter()); // 2 (count persists)
```

## Q16 (বাংলা): জাভাস্ক্রিপ্টে ক্লোজার ব্যাখ্যা কর।

**উত্তর:** ক্লোজার হলো একটি ফাংশন যা তার বাইরের (লেক্সিক্যাল) স্কোপ থেকে ভেরিয়েবল মনে রাখে এবং অ্যাক্সেস করে, এমনকি বাইরের ফাংশনের এক্সিকিউশন শেষ হওয়ার পরেও। এটি তৈরি হয় যখন একটি ফাংশন অন্য ফাংশনের ভিতরে ডিফাইন করা হয় এবং বাইরের ফাংশনের ভেরিয়েবল রেফারেন্স করে।

---

## Q17. What is a callback function?

**Answer:** A callback function is a function passed as an argument to another function, to be executed later (often after an asynchronous operation completes).

**Example:**

```javascript
function fetchData(callback) {
  setTimeout(() => {
    callback("Data received");
  }, 1000);
}

fetchData((data) => {
  console.log(data); //"Data received" after 1s
});
```

## Q17 (বাংলা): কলব্যাক ফাংশন কী?

**উত্তর:** কলব্যাক ফাংশন হলো একটি ফাংশন যা অন্য ফাংশনে আর্গুমেন্ট হিসেবে পাস করা হয়, পরে এক্সিকিউট করার জন্য (প্রায়শই অ্যাসিঙ্ক্রোনাস অপারেশন শেষ হওয়ার পর)।

---

## Q18. Explain currying in JavaScript.

**Answer:** Currying is transforming a function that takes multiple arguments into a sequence of functions, each taking a single argument.

**Example:**

```javascript
// Normal function
function add(a, b, c) {
  return a + b + c;
}

// Curried function
function curriedAdd(a) {
  return function (b) {
    return function (c) {
      return a + b + c;
    };
  };
}

console.log(curriedAdd(1)(2)(3)); // 6
```

## Q18 (বাংলা): জাভাস্ক্রিপ্টে কারিিং ব্যাখ্যা কর।

**উত্তর:** কারিিং হলো এমন একটি প্রক্রিয়া যা একাধিক আর্গুমেন্ট নেওয়া একটি ফাংশনকে এমন ফাংশনের ক্রমে রূপান্তরিত করে, যেখানে প্রতিটি ফাংশন একটি করে আর্গুমেন্ট নেয়।

---

## Q19. What is recursion? Give an example.

**Answer:** Recursion is a technique where a function calls itself to solve a problem by breaking it down into smaller sub-problems. It requires a base case to stop the recursion.

**Example:**

```javascript
function factorial(n) {
  if (n <= 1) {
    return 1; // Base case
  }

  return n * factorial(n - 1); //Recursive call
}

console.log(factorial(5)); // 120
```

## Q19 (বাংলা): রিকারশন কী? একটি উদাহরণ দাও।

**উত্তর:** রিকারশন হলো একটি কৌশল যেখানে একটি ফাংশন নিজেকে কল করে একটি সমস্যা সমাধান করে, সমস্যাটিকে ছোট উপ-সমস্যায় ভেঙে। এটি থামার জন্য একটি বেস কেস প্রয়োজন।

---

## Q20. Explain the difference between synchronous and asynchronous functions.

**Answer:** Synchronous functions execute sequentially; each operation blocks until completion. Asynchronous functions allow other operations to run while waiting for tasks (like I/O) to complete, using callbacks, Promises, or async/await.

**Example:**

```javascript
// Synchronous
console.log("Start");
console.log("End"); // Start, End

// Asynchronous
console.log("Start");
setTimeout(() => console.log("Async"), 0);
console.log("End"); // Start, End, Async
```

## Q20 (বাংলা): সিঙ্ক্রোনাস এবং অ্যাসিঙ্ক্রোনাস ফাংশনের মধ্যে পার্থক্য ব্যাখ্যা কর।

**উত্তর:** সিঙ্ক্রোনাস ফাংশন ক্রমান্বয়ে এক্সিকিউট করে; প্রতিটি অপারেশন সম্পূর্ণ না হওয়া পর্যন্ত ব্লক করে। অ্যাসিঙ্ক্রোনাস ফাংশন অন্যান্য অপারেশন চলতে দেয় যখন কাজ (যেমন I/O) সম্পূর্ণ হওয়ার জন্য অপেক্ষা করে, কলব্যাক, প্রমিজ বা async/await ব্যবহার করে।

---
