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

# 3. Scope & Execution Context

## Q21. What is the difference between global and local scope?

**Answer:** Global scope refers to variables declared outside any function, accessible everywhere. Local scope refers to variables declared inside a function, accessible only within that function.

**Example:**

```javascript
const globalVar = "I am global";
function test() {
  const localVar = "I am local";
  console.log(globalVar); // accessible
}
test();
console.log(localVar); // ReferenceError
```

## Q21 (বাংলা): গ্লোবাল এবং লোকাল স্কোপের মধ্যে পার্থক্য কী?

**উত্তর:** গ্লোবাল স্কোপ বলতে কোনো ফাংশনের বাইরে ডিক্লেয়ার করা ভেরিয়েবল বোঝায়, যা সর্বত্র অ্যাক্সেসযোগ্য। লোকাল স্কোপ বলতে ফাংশনের ভিতরে ডিক্লেয়ার করা ভেরিয়েবল বোঝায়, যা শুধু ঐ ফাংশনের ভিতরে অ্যাক্সেসযোগ্য।

---

## Q22. Explain block scope.

**Answer:** Block scope restricts variable access to the block (`{}`) where it is declared. `let` and `const` are block-scoped. `var` is not block-scoped.

**Example:**

```javascript
if (true) {
  let x = 10;
  var y = 20;
}
console.log(y); // 20
console.log(x); // ReferenceError
```

## Q22 (বাংলা): ব্লক স্কোপ ব্যাখ্যা কর।

**উত্তর:** ব্লক স্কোপ ভেরিয়েবল অ্যাক্সেসকে সেই ব্লক (`{}`) এর মধ্যে সীমাবদ্ধ করে যেখানে এটি ডিক্লেয়ার করা হয়। `let` এবং `const` ব্লক-স্কোপড। `var` ব্লক-স্কোপড নয়।

---

## Q23. What is the difference between lexical scope and dynamic scope?

**Answer:** Lexical scope (static scope) is determined by where variables are defined in the source code. Dynamic scope is determined by the call stack at runtime. JavaScript uses lexical scope.

**Example:**

```javascript
let x = "global";
function outer() {
  let x = "outer";
  function inner() {
    console.log(x); // 'outer' (lexical - looks at definition)
  }
  inner();
}
outer();

console.log(x); // global
```

## Q23 (বাংলা): লেক্সিক্যাল স্কোপ এবং ডায়নামিক স্কোপের মধ্যে পার্থক্য কী?

**উত্তর:** লেক্সিক্যাল স্কোপ (স্ট্যাটিক স্কোপ) নির্ধারিত হয় সোর্স কোডে ভেরিয়েবল কোথায় ডিফাইন করা হয়েছে তার দ্বারা। ডায়নামিক স্কোপ নির্ধারিত হয় রানটাইমে কল স্ট্যাক দ্বারা। জাভাস্ক্রিপ্ট লেক্সিক্যাল স্কোপ ব্যবহার করে।

---

## Q24. What is an execution context?

**Answer:** An execution context is an abstract environment containing information about the code being executed, including the variable environment, scope chain, and this value. There are three types: global, function, and eval.

**Example:** When a function is called, a new execution context is created and pushed onto the call stack.

```javascript
function example() {
  // Each call creates a new execution context
  console.log("Executing");
}
example();
```

## Q24 (বাংলা): এক্সিকিউশন কনটেক্সট কী?

**উত্তর:** এক্সিকিউশন কনটেক্সট হলো একটি অ্যাবস্ট্রাক্ট এনভায়রনমেন্ট যা এক্সিকিউট করা কোড সম্পর্কিত তথ্য ধারণ করে, যেমন ভেরিয়েবল এনভায়রনমেন্ট, স্কোপ চেইন এবং this মান। তিন ধরনের: গ্লোবাল, ফাংশন এবং eval।

**উদাহরণ**: যখন একটি ফাংশন কল করা হয়, একটি নতুন এক্সিকিউশন কনটেক্সট তৈরি হয় এবং কল স্ট্যাকে পুশ করা হয়।

---

## Q25. Explain the call stack in JavaScript.

**Answer:** The call stack is a data structure that tracks function calls. When a function is called, it's pushed onto the stack. When it returns, it's popped off. JavaScript is single-threaded, so it has one call stack.

**Example:**

```javascript
function first() {
  second();
}
function second() {
  third();
}
function third() {
  console.log("Done");
}
first(); // Stack: first -> second -> third
```

## Q25 (বাংলা): জাভাস্ক্রিপ্টে কল স্ট্যাক ব্যাখ্যা কর।

**উত্তর:** কল স্ট্যাক একটি ডেটা স্ট্রাকচার যা ফাংশন কল ট্র্যাক করে। যখন একটি ফাংশন কল হয়, এটি স্ট্যাকে পুশ হয়। যখন এটি রিটার্ন করে, এটি পপ হয়। জাভাস্ক্রিপ্ট সিঙ্গেল-থ্রেডেড, তাই এটির একটি কল স্ট্যাক আছে।

---

## Q26. How does JavaScript handle variable shadowing?

**Answer:** Variable shadowing occurs when a variable declared in an inner scope has the same name as a variable in an outer scope. The inner variable "shadows" the outer one, making the outer variable inaccessible in the inner scope.

**Example:**

```javascript
let x = "outer";
function test() {
  let x = "inner";
  console.log(x); // 'inner' (shadows outer)
}
test();
console.log(x); // 'outer'
```

## Q26 (বাংলা): জাভাস্ক্রিপ্ট কীভাবে ভেরিয়েবল শ্যাডোয়িং হ্যান্ডেল করে?

**উত্তর:** ভেরিয়েবল শ্যাডোয়িং ঘটে যখন একটি ইনার স্কোপে ডিক্লেয়ার করা ভেরিয়েবলের নাম বাইরের স্কোপের ভেরিয়েবলের মতো হয়। ইনার ভেরিয়েবল বাইরেরটিকে "শ্যাডো" করে, ফলে বাইরের ভেরিয়েবল ইনার স্কোপে অ্যাক্সেসযোগ্য হয় না।

---

## Q27. What is the Temporal Dead Zone (TDZ)?

**Answer:** The `TDZ` is the period between entering a scope and the actual declaration of a `let` or `const` variable. During this period, accessing the variable throws a `ReferenceError`. This prevents accessing variables before they are initialized.

**Example:**

```javascript
console.log(a); // ReferenceError (TDZ)
let a = 5;
```

## Q27 (বাংলা): টেম্পোরাল ডেড জোন (TDZ) কী?

**উত্তর:** `TDZ` হলো একটি স্কোপে প্রবেশ করা এবং `let` বা `const` ভেরিয়েবলের প্রকৃত ডিক্লেয়ারেশনের মধ্যে সময়কাল। এই সময়কালে ভেরিয়েবল অ্যাক্সেস করলে `ReferenceError` হয়। এটি ভেরিয়েবল ইনিশিয়ালাইজ করার আগে অ্যাক্সেস করা প্রতিরোধ করে।

---

## Q28. What is the scope chain?

**Answer:** The scope chain is the hierarchical chain of scopes that JavaScript uses to resolve variable names. When a variable is accessed, JavaScript looks in the current scope, then the outer scope, and so on, up to the global scope.

**Example:**

```javascript
let global = "global";
function outer() {
  let outerVar = "outer";
  function inner() {
    let innerVar = "inner";
    console.log(global); // finds in global
    console.log(outerVar); // finds in outer
  }
  inner();
}
outer();
```

## Q28 (বাংলা): স্কোপ চেইন কী?

**উত্তর:** স্কোপ চেইন হলো স্কোপগুলির শ্রেণিবিন্যাসিক চেইন যা জাভাস্ক্রিপ্ট ভেরিয়েবল নাম সমাধান করতে ব্যবহার করে। যখন একটি ভেরিয়েবল অ্যাক্সেস করা হয়, জাভাস্ক্রিপ্ট বর্তমান স্কোপে, তারপর বাইরের স্কোপে এবং এভাবে গ্লোবাল স্কোপ পর্যন্ত দেখে।

---

## Q29. How does the `this` keyword work in different contexts?

**Answer:** `this` refers to the object that is executing the current function. Its value depends on how the function is called: global context → global object (or `undefined` in strict mode), method call → the object, arrow functions → lexical `this`, constructor → new instance, `call/apply/bind` → explicitly set.

**Example:**

```javascript
const obj = {
  name: "John",
  greet: function () {
    console.log(this.name);
  },
};
obj.greet(); // 'John' (method call)
const greet = obj.greet;
greet(); // undefined or window (global call)
```

## Q29 (বাংলা): বিভিন্ন কনটেক্সটে `this` কীওয়ার্ড কীভাবে কাজ করে?

**উত্তর:** `this` বর্তমান ফাংশনটি এক্সিকিউট করছে এমন অবজেক্টকে নির্দেশ করে। এর মান নির্ভর করে ফাংশনটি কীভাবে কল করা হয় তার উপর: গ্লোবাল কনটেক্সট → গ্লোবাল অবজেক্ট (অথবা স্ট্রিক্ট মোডে `undefined`), মেথড কল → অবজেক্ট, অ্যারো ফাংশন → লেক্সিক্যাল `this`, কনস্ট্রাক্টর → নতুন ইনস্ট্যান্স, `call/apply/bind` → স্পষ্টভাবে সেট করা।

---

## Q30. What is the difference between implicit and explicit binding of `this`?

**Answer:** Implicit binding occurs when `this` is determined by the calling context (e.g., `obj.method()`). Explicit binding occurs when `this` is explicitly set using `call, apply, or bind`.

**Example:**

```javascript
function greet() {
  console.log(this.name);
}

const obj1 = { name: "Sohel" };
const obj2 = { name: "Rolex" };

// Implicit
obj1.greet = greet;
obj1.greet(); //Sohel

// Explicit
greet.call(obj2); //Rolex
```

## Q30 (বাংলা): `this`-এর ইম্প্লিসিট এবং এক্সপ্লিসিট বাইন্ডিংয়ের মধ্যে পার্থক্য কী?

**উত্তর:** ইম্প্লিসিট বাইন্ডিং ঘটে যখন `this` কলিং কনটেক্সট দ্বারা নির্ধারিত হয় (যেমন `obj.method()`)। এক্সপ্লিসিট বাইন্ডিং ঘটে যখন `this` স্পষ্টভাবে `call, apply, বা bind` ব্যবহার করে সেট করা হয়।

---

# 4. Objects & Arrays

## Q31. How do you create objects in JavaScript?

**Answer:** Objects can be created using object literals (`{}`), the `new` keyword with a constructor, `Object.create()`, or ES6 classes.

**Example:**

```javascript
// Object literal
const obj1 = {
  name: "Sohel",
};

// Constructor
const obj2 = new Object({
  name: "Rolex",
});

// Object.create
const proto = {
  greet() {
    console.log("Hello");
  },
};
const obj3 = Object.create(proto);

// ES6 class
class Person {
  constructor(name) {
    this.name = name;
  }
}
const obj4 = new Person("Sohel");
```

## Q31 (বাংলা): জাভাস্ক্রিপ্টে কীভাবে অবজেক্ট তৈরি করবে?

**উত্তর:** অবজেক্ট লিটারেল (`{}`), কনস্ট্রাক্টরের সাথে `new` কীওয়ার্ড, `Object.create()`, অথবা ES6 ক্লাস ব্যবহার করে অবজেক্ট তৈরি করা যায়।

---

## Q32. Difference between dot notation and bracket notation.

**Answer:** Dot notation (`obj.property`) is simpler and faster but requires a valid identifier. Bracket notation (`obj['property']`) allows dynamic property names and characters not valid in identifiers.

**Example:**

```javascript
// Dot notation
const obj1 = {
  firstName: "Sohel",
  lastName: "Rana",
};
console.log(obj1.firstName); // Sohel

// Bracket notation
console.log(obj1["lastName"]); // Rana
```

## Q32 (বাংলা): ডট নোটেশন এবং ব্র্যাকেট নোটেশনের মধ্যে পার্থক্য কী?

**উত্তর:** ডট নোটেশন (`obj.property`) সহজ এবং দ্রুত কিন্তু বৈধ আইডেন্টিফায়ার প্রয়োজন। ব্র্যাকেট নোটেশন (`obj['property']`) ডায়নামিক প্রপার্টি নাম এবং আইডেন্টিফায়ারে অবৈধ অক্ষর অনুমোদন করে।

---

## Q33. How to merge two objects?

**Answer:** Use `Object.assign()` or the spread operator (`...`). Both perform shallow merging; nested objects are not deeply merged.

**Example:**

```javascript
const obj1 = {
  a: 1,
  b: 2,
};

const obj2 = {
  b: 3,
  c: 4,
};

const merged1 = Object.assign({}, obj1, obj2);
console.log("merged1: ", merged1);
// { a: 1, b: 3, c: 4 }

const merged2 = {
  ...obj1,
  ...obj2,
};
console.log("merged2: ", merged2);
// { a: 1, b: 3, c: 4 }
```

## Q33 (বাংলা): কীভাবে দুটি অবজেক্ট মার্জ করবে?

**উত্তর:** `Object.assign()` বা স্প্রেড অপারেটর (`...`) ব্যবহার কর। উভয়ই শ্যালো মার্জ করে; নেস্টেড অবজেক্ট গভীরভাবে মার্জ হয় না।

---

## Q34. What are object destructuring and array destructuring?

**Answer:** Destructuring is a syntax that extracts values from objects or arrays into distinct variables.

**Example:**

```javascript
// Object destructuring
const person = { name: "Sohel", age: 26 };
const { name, age } = person;
console.log(name, age); // 'Sohel' 26

// Array destructuring
const colors = ["red", "green", "blue"];
const [first, second] = colors;
console.log(first, second); // 'red' 'green'
```

## Q34 (বাংলা): অবজেক্ট ডিস্ট্রাকচারিং এবং অ্যারে ডিস্ট্রাকচারিং কী?

**উত্তর:** ডিস্ট্রাকচারিং হলো একটি সিনট্যাক্স যা অবজেক্ট বা অ্যারে থেকে মান বের করে পৃথক ভেরিয়েবলে নেয়।

---

## Q35. Explain shallow copy vs deep copy.

**Answer:** Shallow copy copies only the top-level properties; nested objects are copied by reference. Deep copy creates a completely independent copy of all nested objects.

**Example:**

```javascript
const original = { a: 1, b: { c: 2 } };

// Shallow copy
const shallow = { ...original };
shallow.b.c = 3;
console.log(original.b.c); // 3 (changed)

// Deep copy (using JSON)
const deep = JSON.parse(JSON.stringify(original));
deep.b.c = 4;
console.log(original.b.c); // 3 (unchanged)
```

## Q35 (বাংলা): শ্যালো কপি এবং ডিপ কপির মধ্যে পার্থক্য ব্যাখ্যা কর।

**উত্তর:** শ্যালো কপি শুধুমাত্র শীর্ষ-স্তরের প্রপার্টি কপি করে; নেস্টেড অবজেক্ট রেফারেন্স দ্বারা কপি হয়। ডিপ কপি সমস্ত নেস্টেড অবজেক্টের সম্পূর্ণ স্বাধীন কপি তৈরি করে।

---

## Q36. How to clone an object in JavaScript?

**Answer:** Use the spread operator (`{...obj}`), `Object.assign({}, obj)`, `JSON.parse(JSON.stringify(obj))` (for deep clone, with limitations), or `structuredClone(obj)` (modern).

**Example:**

```javascript
const obj = { a: 1, b: { c: 2 } };

const clone1 = { ...obj }; // shallow
const clone2 = Object.assign({}, obj); // shallow
const clone3 = JSON.parse(JSON.stringify(obj)); // deep
const clone4 = structuredClone(obj); // deep (modern)
```

## Q36 (বাংলা): জাভাস্ক্রিপ্টে কীভাবে একটি অবজেক্ট ক্লোন করবে?

**উত্তর:** স্প্রেড অপারেটর (`{...obj}`), `Object.assign({}, obj)`, `JSON.parse(JSON.stringify(obj))` (ডিপ ক্লোনের জন্য, সীমাবদ্ধতা সহ), অথবা `structuredClone(obj)` (আধুনিক) ব্যবহার কর।

---

## Q37. What is the difference between `Object.freeze()` and `Object.seal()`?

**Answer:** `Object.freeze()` prevents adding, deleting, or modifying properties. `Object.seal()` prevents adding or deleting properties but allows modifying existing properties.

**Example:**

```javascript
const obj1 = { a: 1 };
Object.freeze(obj1);
obj1.a = 2; // fails silently (or TypeError in strict mode)

const obj2 = { a: 1 };
Object.seal(obj2);
obj2.a = 2; // allowed
delete obj2.a; // fails
```

## Q37 (বাংলা): `Object.freeze()` এবং `Object.seal()`-এর মধ্যে পার্থক্য কী?

**উত্তর:** `Object.freeze()` প্রপার্টি যোগ, মুছে ফেলা বা পরিবর্তন করা প্রতিরোধ করে। `Object.seal()` প্রপার্টি যোগ বা মুছে ফেলা প্রতিরোধ করে কিন্তু বিদ্যমান প্রপার্টি পরিবর্তন করার অনুমতি দেয়।

---

## Q38. Explain object prototypes.

**Answer:** Every JavaScript object has an internal `[[Prototype]]` property (accessed via `__proto__` or `Object.getPrototypeOf()`) that points to another object. This forms a prototype chain, allowing inheritance of properties and methods.

**Example:**

```javascript
const parent = {
  greet() {
    console.log("Hello");
  },
};
const child = Object.create(parent);
child.greet(); // 'Hello' (inherited)
console.log(Object.getPrototypeOf(child) === parent); // true
```

## Q38 (বাংলা): অবজেক্ট প্রোটোটাইপ ব্যাখ্যা কর।

**উত্তর:** প্রতিটি জাভাস্ক্রিপ্ট অবজেক্টের একটি অভ্যন্তরীণ `[[Prototype]]` প্রপার্টি আছে (`__proto__` বা `Object.getPrototypeOf()` এর মাধ্যমে অ্যাক্সেসযোগ্য) যা অন্য একটি অবজেক্টের দিকে নির্দেশ করে। এটি একটি প্রোটোটাইপ চেইন গঠন করে, যা প্রপার্টি এবং মেথডের ইনহেরিটেন্স অনুমোদন করে।

---

## Q39. What is prototypal inheritance?

**Answer:** Prototypal inheritance is a mechanism where an object can inherit properties and methods from another object through the prototype chain. JavaScript uses prototypal inheritance instead of classical inheritance.

**Example:**

```javascript
const animal = {
  speak() {
    console.log("Animal speaks");
  },
};
const dog = Object.create(animal);
dog.bark = function () {
  console.log("Woof");
};
dog.speak(); // 'Animal speaks' (inherited)
dog.bark(); // 'Woof'
```

## Q39 (বাংলা): প্রোটোটাইপাল ইনহেরিটেন্স কী?

**উত্তর:** প্রোটোটাইপাল ইনহেরিটেন্স হলো একটি প্রক্রিয়া যেখানে একটি অবজেক্ট প্রোটোটাইপ চেইনের মাধ্যমে অন্য অবজেক্ট থেকে প্রপার্টি এবং মেথড ইনহেরিট করতে পারে। জাভাস্ক্রিপ্ট ক্লাসিক্যাল ইনহেরিটেন্সের পরিবর্তে প্রোটোটাইপাল ইনহেরিটেন্স ব্যবহার করে।

---

## Q40. How to check if an object has a property?

**Answer:** Use `'property' in object` (checks own and inherited properties), `object.hasOwnProperty('property')` (checks own properties only), or `Object.hasOwn(object, 'property')` (ES2022).

**Example:**

```javascript
const obj = { name: "John" };
console.log("name" in obj); // true
console.log(obj.hasOwnProperty("name")); // true
console.log(Object.hasOwn(obj, "name")); // true
```

## Q40 (বাংলা): কীভাবে চেক করবে একটি অবজেক্টের একটি প্রপার্টি আছে?

**উত্তর:** `'property' in object` (নিজের এবং ইনহেরিটেড উভয় প্রপার্টি চেক করে), `object.hasOwnProperty('property')` (শুধু নিজের প্রপার্টি চেক করে), অথবা `Object.hasOwn(object, 'property')` (ES2022) ব্যবহার কর।

---

## Q41. Difference between forEach and map.

**Answer:** `forEach` iterates over an array, executing a function for each element; it returns `undefined`. `map` iterates and returns a new array with the results of calling a function on each element.

**Example:**

```javascript
const arr = [1, 2, 3];

// forEach (returns undefined)
const result1 = arr.forEach((x) => x * 2);
console.log(result1); // undefined

// map (returns new array)
const result2 = arr.map((x) => x * 2);
console.log(result2); // [2, 4, 6]
```

## Q41 (বাংলা): `forEach` এবং `map`-এর মধ্যে পার্থক্য কী?

**উত্তর:** `forEach` একটি অ্যারের উপর পুনরাবৃত্তি করে, প্রতিটি উপাদানের জন্য একটি ফাংশন এক্সিকিউট করে; এটি `undefined` রিটার্ন করে। `map` পুনরাবৃত্তি করে এবং প্রতিটি উপাদানে ফাংশন কলের ফলাফল সহ একটি নতুন অ্যারে রিটার্ন করে।

---

## Q42. How does `reduce` work?

**Answer:** `reduce` applies a reducer function to each element of an array, accumulating a single result. It takes a callback (accumulator, currentValue) and an optional initial value.

**Example:**

```javascript
const arr = [1, 2, 3, 4];

const sum = arr.reduce((acc, currVal) => {
  return acc + curVal;
}, 0);
console.log("Sum: ", sum); //10

// Without initial value (uses first element as accumulator)
const product = arr.reduce((acc, currVal) => {
  return acc * curVal;
});
console.log("Product: ", product); //24
```

## Q42 (বাংলা): `reduce` কীভাবে কাজ করে?

**উত্তর:** `reduce` একটি অ্যারের প্রতিটি উপাদানে একটি রিডিউসার ফাংশন প্রয়োগ করে, একটি একক ফলাফল সংগ্রহ করে। এটি একটি কলব্যাক (accumulator, currentValue) এবং একটি ঐচ্ছিক প্রাথমিক মান নেয়।

---

## Q43. Difference between `slice` and `splice`.

**Answer:** `slice(start, end)` returns a new array (shallow copy) of selected elements without modifying the original. `splice(start, deleteCount, ...items)` modifies the original array by removing, replacing, or adding elements.

**Example:**

```javascript
const arr = [1, 2, 3, 4, 5];
const sliced = arr.slice(1, 3);
console.log(sliced); // [2, 3]
console.log(arr); // [1, 2, 3, 4, 5] (unchanged)

const spliced = arr.splice(1, 2, "a", "b");
console.log(spliced); // [2, 3]
console.log(arr); // [1, 'a', 'b', 4, 5] (modified)
```

## Q43 (বাংলা): `slice` এবং `splice`-এর মধ্যে পার্থক্য কী?

**উত্তর:** `slice(start, end)` মূল অ্যারে পরিবর্তন না করেই নির্বাচিত উপাদানের একটি নতুন অ্যারে (শ্যালো কপি) রিটার্ন করে। `splice(start, deleteCount, ...items)` উপাদান সরিয়ে, প্রতিস্থাপন করে বা যোগ করে মূল অ্যারে পরিবর্তন করে।

---

## Q44. What is the difference between `find` and `filter`?

**Answer:** `find` returns the first element that satisfies the condition (or `undefined`). `filter` returns a new array with all elements that satisfy the condition.

**Example:**

```javascript
const arr = [1, 2, 3, 4, 5];

const found = arr.find((x) => x > 3);
console.log(found); //4

const filtered = arr.filter((x) => x > 3);
console.log(filtered); // [4, 5]
```

## Q44 (বাংলা): `find` এবং `filter`-এর মধ্যে পার্থক্য কী?

**উত্তর:** `find` শর্ত পূরণ করে এমন প্রথম উপাদান রিটার্ন করে (অথবা `undefined`)। `filter` শর্ত পূরণ করে এমন সমস্ত উপাদান সহ একটি নতুন অ্যারে রিটার্ন করে।

---
