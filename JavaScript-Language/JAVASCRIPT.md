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
