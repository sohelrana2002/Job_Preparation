# JAVASCRIPT Interview Question

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
