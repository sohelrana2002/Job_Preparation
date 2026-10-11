# JavaScript Advanced Concepts and Coding Problems (Q31 - Q50)

> Q1-Q10 are in `01-fundamentals.md`, Q11-Q30 in `02-core-concepts.md`.

---

## Q31. Callback hell

### English

**Question:** What is callback hell and how do you avoid it?

**Answer:** Callback hell is deeply nested callbacks, usually from sequential asynchronous operations, which makes code hard to read, debug and maintain (also called the "pyramid of doom"). Avoid it by using Promises with chaining, `async/await`, splitting logic into small named functions, and handling errors in one place.

### বাংলা

**প্রশ্ন:** Callback hell কী এবং কীভাবে এড়ানো যায়?

**উত্তর:** Callback hell হলো callback-এর ভেতরে callback, এভাবে গভীর nesting তৈরি হওয়া, সাধারণত একটার পর একটা asynchronous কাজ করতে গিয়ে। এতে code পড়া, debug করা ও রক্ষণাবেক্ষণ কঠিন হয় ("pyramid of doom")। এড়াতে Promise chaining, `async/await`, ছোট ছোট নামযুক্ত function-এ logic ভাগ করা এবং এক জায়গায় error সামলানো ব্যবহার করুন।

### Example

```js
// Callback hell
getUser(1, (user) => {
  getPosts(user.id, (posts) => {
    getComments(posts[0].id, (comments) => {
      console.log(comments);
    });
  });
});

// Fixed with async/await
async function load() {
  try {
    const user = await getUser(1);
    const posts = await getPosts(user.id);
    const comments = await getComments(posts[0].id);
    console.log(comments);
  } catch (err) {
    console.error(err);
  }
}
```

---

## Q32. Promise.all, allSettled, race and any

### English

**Question:** What is the difference between `Promise.all`, `allSettled`, `race` and `any`?

**Answer:**

- `Promise.all`: resolves with an array of all results; rejects immediately if any promise rejects.
- `Promise.allSettled`: waits for all to finish and returns each outcome (`fulfilled` or `rejected`); never rejects.
- `Promise.race`: settles with the first promise to settle, whether success or failure.
- `Promise.any`: resolves with the first fulfilled promise; rejects (AggregateError) only if all reject.

### বাংলা

**প্রশ্ন:** `Promise.all`, `allSettled`, `race` ও `any`-এর পার্থক্য কী?

**উত্তর:**

- `Promise.all`: সবগুলো সফল হলে সব ফলাফলের array দেয়; একটিও ব্যর্থ হলে সাথে সাথে reject হয়।
- `Promise.allSettled`: সবগুলো শেষ হওয়া পর্যন্ত অপেক্ষা করে এবং প্রতিটির ফলাফল (`fulfilled` বা `rejected`) দেয়; কখনো reject হয় না।
- `Promise.race`: যেটি সবার আগে শেষ হয় (সফল বা ব্যর্থ) সেটির ফলাফল দেয়।
- `Promise.any`: প্রথম সফল promise-এর ফলাফল দেয়; সবগুলো ব্যর্থ হলেই শুধু reject (AggregateError) হয়।

### Example

```js
const ok = (v, ms) => new Promise((r) => setTimeout(() => r(v), ms));
const fail = (ms) => new Promise((_, rej) => setTimeout(() => rej("err"), ms));

await Promise.all([ok("A", 100), ok("B", 200)]); // ["A", "B"]
await Promise.all([ok("A", 100), fail(50)]); // rejects "err"

await Promise.allSettled([ok("A", 100), fail(50)]);
// [{status:"fulfilled", value:"A"}, {status:"rejected", reason:"err"}]

await Promise.race([ok("slow", 300), ok("fast", 100)]); // "fast"
await Promise.any([fail(50), ok("B", 200)]); // "B"
```

---

## Q33. Debounce vs throttle

### English

**Question:** What are debounce and throttle? Implement debounce.

**Answer:** Debounce delays running a function until a certain time has passed since the last call; it is ideal for search inputs and window resize. Throttle ensures a function runs at most once in a given interval; it is ideal for scroll and mouse-move handlers. Both reduce the number of expensive calls and use closures to keep a timer or last-run timestamp.

### বাংলা

**প্রশ্ন:** Debounce ও throttle কী? Debounce implement করুন।

**উত্তর:** Debounce হলো শেষ call-এর পর নির্দিষ্ট সময় পার না হওয়া পর্যন্ত function চালানো বিলম্বিত করা; search input ও window resize-এ উপযোগী। Throttle নিশ্চিত করে যে নির্দিষ্ট সময়ের ব্যবধানে function সর্বোচ্চ একবার চলবে; scroll ও mouse-move-এ উপযোগী। দুটিই ব্যয়বহুল call কমায় এবং timer বা শেষ চলার সময় মনে রাখতে closure ব্যবহার করে।

### Example

```js
function debounce(fn, delay) {
  let timer;
  return function (...args) {
    clearTimeout(timer);
    timer = setTimeout(() => fn.apply(this, args), delay);
  };
}

function throttle(fn, limit) {
  let lastRun = 0;
  return function (...args) {
    const now = Date.now();
    if (now - lastRun >= limit) {
      lastRun = now;
      fn.apply(this, args);
    }
  };
}

const search = debounce((q) => console.log("Searching:", q), 500);
search("a");
search("ab");
search("abc"); // only "abc" runs after 500ms
```

---

## Q34. Event bubbling, capturing and delegation

### English

**Question:** Explain event bubbling, capturing and event delegation.

**Answer:** When an event fires, it travels in three phases: capturing (from `window` down to the target), target, and bubbling (from the target back up to `window`). By default, listeners run in the bubbling phase. `stopPropagation()` stops the event from travelling further. Event delegation attaches one listener to a parent and uses `event.target` to handle events from many children, which saves memory and works for dynamically added elements.

### বাংলা

**প্রশ্ন:** Event bubbling, capturing ও event delegation ব্যাখ্যা করুন।

**উত্তর:** কোনো event ঘটলে তা তিন ধাপে চলে: capturing (`window` থেকে নিচে target পর্যন্ত), target, এবং bubbling (target থেকে আবার উপরে `window` পর্যন্ত)। ডিফল্টভাবে listener bubbling ধাপে চলে। `stopPropagation()` event-কে আর এগোতে দেয় না। Event delegation-এ parent-এ একটিমাত্র listener বসিয়ে `event.target` দিয়ে অনেক child-এর event সামলানো হয়; এতে memory বাঁচে এবং পরে যোগ করা element-এও কাজ করে।

### Example

```html
<ul id="list">
  <li data-id="1">One</li>
  <li data-id="2">Two</li>
</ul>
```

```js
document.getElementById("list").addEventListener("click", (e) => {
  if (e.target.tagName === "LI") {
    console.log("Clicked item:", e.target.dataset.id);
  }
});

// capturing phase: pass `true` (or { capture: true })
parent.addEventListener("click", handler, true);
child.addEventListener("click", (e) => e.stopPropagation());
```

---

## Q35. localStorage, sessionStorage and cookies

### English

**Question:** What is the difference between `localStorage`, `sessionStorage` and cookies?

**Answer:**

- `localStorage`: about 5 to 10 MB, persists until manually cleared, shared across tabs of the same origin, never sent to the server.
- `sessionStorage`: same API, but cleared when the tab closes and not shared between tabs.
- Cookies: about 4 KB, can have an expiry date, are sent with every HTTP request to the server, and can be `HttpOnly` (not readable by JavaScript) and `Secure`.

For authentication tokens, `HttpOnly` cookies are safer than `localStorage` because they are not exposed to XSS scripts.

### বাংলা

**প্রশ্ন:** `localStorage`, `sessionStorage` ও cookie-র পার্থক্য কী?

**উত্তর:**

- `localStorage`: প্রায় ৫-১০ MB, নিজে মুছে না ফেললে থেকে যায়, একই origin-এর সব tab-এ শেয়ার হয়, server-এ যায় না।
- `sessionStorage`: একই API, কিন্তু tab বন্ধ করলে মুছে যায় এবং tab-এর মধ্যে শেয়ার হয় না।
- Cookie: প্রায় ৪ KB, মেয়াদ ঠিক করা যায়, প্রতিটি HTTP request-এর সাথে server-এ যায়, এবং `HttpOnly` (JavaScript পড়তে পারে না) ও `Secure` করা যায়।

Authentication token রাখার জন্য `HttpOnly` cookie, `localStorage`-এর চেয়ে নিরাপদ, কারণ XSS script সেটি পড়তে পারে না।

### Example

```js
localStorage.setItem("theme", "dark");
console.log(localStorage.getItem("theme"));
localStorage.removeItem("theme");

sessionStorage.setItem("step", "2");

localStorage.setItem("user", JSON.stringify({ name: "Sohel" }));
const user = JSON.parse(localStorage.getItem("user"));

document.cookie = "lang=bn; max-age=86400; path=/";
```

---

## Q36. Memoization

### English

**Question:** What is memoization? Implement a memoize function.

**Answer:** Memoization is an optimization technique that caches the result of a function for given arguments, so repeated calls with the same arguments return the cached result instead of recomputing. It works only for pure functions and uses closures to hold the cache. It is common for expensive calculations such as Fibonacci.

### বাংলা

**প্রশ্ন:** Memoization কী? একটি memoize function লিখুন।

**উত্তর:** Memoization হলো একটি optimization কৌশল, যেখানে কোনো argument-এর জন্য function-এর ফলাফল cache করে রাখা হয়, ফলে একই argument দিয়ে আবার call করলে নতুন করে হিসাব না করে cache থেকে উত্তর দেওয়া হয়। এটি শুধু pure function-এর জন্য কাজ করে এবং cache রাখতে closure ব্যবহার করে। Fibonacci-র মতো ভারী হিসাবে এটি খুব কাজের।

### Example

```js
function memoize(fn) {
  const cache = new Map();
  return function (...args) {
    const key = JSON.stringify(args);
    if (cache.has(key)) return cache.get(key);
    const result = fn.apply(this, args);
    cache.set(key, result);
    return result;
  };
}

const fib = memoize((n) => (n <= 1 ? n : fib(n - 1) + fib(n - 2)));
console.log(fib(40)); // fast
```

---

## Q37. Iterators and generators

### English

**Question:** What are iterators and generators?

**Answer:** An iterator is an object with a `next()` method that returns `{ value, done }`. Iterables (arrays, strings, Maps, Sets) implement `[Symbol.iterator]`, which is what `for...of` and spread use. A generator function (`function*`) is a special function that can pause with `yield` and resume later, producing values lazily. Generators are useful for lazy sequences and are the basis of older `async/await` implementations.

### বাংলা

**প্রশ্ন:** Iterator ও generator কী?

**উত্তর:** Iterator হলো এমন object যার `next()` method `{ value, done }` return করে। Iterable (array, string, Map, Set) গুলো `[Symbol.iterator]` implement করে, যেটি `for...of` ও spread ব্যবহার করে। Generator function (`function*`) এমন বিশেষ function যা `yield` দিয়ে মাঝপথে থামতে পারে এবং পরে আবার চালু হতে পারে, ফলে মান একটি একটি করে (lazy) তৈরি হয়। Lazy sequence বানাতে এটি কাজে লাগে এবং পুরনো `async/await` implementation-এর ভিত্তি ছিল।

### Example

```js
function* idGenerator() {
  let id = 1;
  while (true) yield id++;
}
const gen = idGenerator();
console.log(gen.next().value); // 1
console.log(gen.next().value); // 2

const iterator = [10, 20][Symbol.iterator]();
console.log(iterator.next()); // { value: 10, done: false }

for (const ch of "abc") console.log(ch);
```

---

## Q38. Error handling

### English

**Question:** How do you handle errors in JavaScript?

**Answer:** Use `try/catch/finally` for synchronous code and for `await`. For promises use `.catch()`. Throw `Error` objects (not strings) so you get a stack trace, and create custom error classes for specific cases. `try/catch` cannot catch errors from a callback that runs later (like inside `setTimeout`), and an unhandled promise rejection should be handled with `.catch` or a global `unhandledrejection` listener.

### বাংলা

**প্রশ্ন:** JavaScript-এ error কীভাবে সামলাবেন?

**উত্তর:** Synchronous code ও `await`-এর জন্য `try/catch/finally` ব্যবহার করুন। Promise-এর জন্য `.catch()` ব্যবহার করুন। String নয়, `Error` object throw করুন, তাহলে stack trace পাওয়া যায়, আর নির্দিষ্ট ক্ষেত্রের জন্য custom error class বানান। পরে চলা callback-এর (যেমন `setTimeout`-এর ভেতরের) error `try/catch` ধরতে পারে না, আর unhandled promise rejection `.catch` বা global `unhandledrejection` listener দিয়ে সামলাতে হয়।

### Example

```js
class ValidationError extends Error {
  constructor(message, field) {
    super(message);
    this.name = "ValidationError";
    this.field = field;
  }
}

function validate(age) {
  if (age < 0) throw new ValidationError("Age cannot be negative", "age");
}

try {
  validate(-5);
} catch (err) {
  if (err instanceof ValidationError) console.log(err.field, err.message);
  else throw err;
} finally {
  console.log("always runs");
}
```

---

## Q39. `var` vs `let` in a loop with `setTimeout`

### English

**Question:** What is the output of this code and why? How do you fix it?

```js
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 1000);
}
```

**Answer:** It prints `3, 3, 3`. `var` is function-scoped, so there is only one `i` shared by all callbacks. By the time the timers fire, the loop has finished and `i` is 3. Using `let` fixes it because `let` creates a new binding for each loop iteration. An IIFE or passing the value as an argument also works, via closure.

### বাংলা

**প্রশ্ন:** এই code-এর output কী হবে এবং কেন? কীভাবে ঠিক করবেন?

**উত্তর:** Output হবে `3, 3, 3`। `var` function-scoped, তাই সব callback একই `i` শেয়ার করে। Timer চলার সময় loop শেষ হয়ে গেছে এবং `i`-এর মান ৩। `let` ব্যবহার করলে ঠিক হয় কারণ `let` প্রতিটি iteration-এর জন্য আলাদা নতুন binding তৈরি করে। IIFE বা মানটি argument হিসেবে পাঠিয়ে closure দিয়েও ঠিক করা যায়।

### Example

```js
for (let i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 1000); // 0, 1, 2
}

for (var j = 0; j < 3; j++) {
  ((n) => setTimeout(() => console.log(n), 1000))(j); // 0, 1, 2
}
```

---

## Q40. Garbage collection and memory leaks

### English

**Question:** How does garbage collection work? What causes memory leaks?

**Answer:** JavaScript automatically frees memory using mark-and-sweep: it starts from roots (global object, call stack), marks everything reachable, and removes the rest. A memory leak happens when objects are still reachable but no longer needed. Common causes: accidental global variables, forgotten timers (`setInterval`), event listeners that are never removed, closures holding large data, and detached DOM nodes. In React, always clean up in the `useEffect` return function.

### বাংলা

**প্রশ্ন:** Garbage collection কীভাবে কাজ করে? Memory leak কেন হয়?

**উত্তর:** JavaScript mark-and-sweep পদ্ধতিতে নিজে memory খালি করে: root (global object, call stack) থেকে শুরু করে যা যা পৌঁছানো যায় সেগুলো চিহ্নিত করে এবং বাকিগুলো মুছে দেয়। Memory leak হয় যখন কোনো object আর দরকার নেই কিন্তু এখনো পৌঁছানো যাচ্ছে। সাধারণ কারণ: অনিচ্ছাকৃত global variable, ভুলে যাওয়া timer (`setInterval`), না সরানো event listener, বড় data ধরে রাখা closure এবং বিচ্ছিন্ন DOM node। React-এ সবসময় `useEffect`-এর return function-এ cleanup করুন।

### Example

```js
// Leak: interval never cleared
function startPolling() {
  setInterval(() => fetchData(), 1000);
}

// Fix
const id = setInterval(() => fetchData(), 1000);
clearInterval(id);

// Leak: listener never removed
window.addEventListener("resize", onResize);
window.removeEventListener("resize", onResize);

// React cleanup
useEffect(() => {
  const id = setInterval(tick, 1000);
  return () => clearInterval(id);
}, []);
```

---

## Q41. `fetch` vs `axios`

### English

**Question:** What is the difference between `fetch` and `axios`?

**Answer:** `fetch` is built into browsers and Node 18+; it needs no installation but requires manual JSON parsing (`res.json()`), and it does not reject on HTTP error statuses like 404 or 500 (you must check `res.ok`). `axios` is a library that parses JSON automatically, rejects on error statuses, supports interceptors, request cancellation, timeouts and upload progress, at the cost of an extra dependency.

### বাংলা

**প্রশ্ন:** `fetch` আর `axios`-এর পার্থক্য কী?

**উত্তর:** `fetch` browser ও Node 18+-এ বিল্ট-ইন, আলাদা install লাগে না, কিন্তু JSON নিজে parse করতে হয় (`res.json()`) এবং 404 বা 500-এর মতো HTTP error status-এ reject হয় না (`res.ok` নিজে যাচাই করতে হয়)। `axios` একটি library, যা JSON নিজে parse করে, error status-এ reject করে, এবং interceptor, request cancel, timeout ও upload progress সমর্থন করে; বিনিময়ে একটি অতিরিক্ত dependency যোগ হয়।

### Example

```js
// fetch
const res = await fetch("/api/users");
if (!res.ok) throw new Error(`HTTP ${res.status}`);
const data = await res.json();

await fetch("/api/users", {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ name: "Sohel" }),
});

// axios
import axios from "axios";
const { data: users } = await axios.get("/api/users");
await axios.post("/api/users", { name: "Sohel" });
```

---

## Q42. Strict mode

### English

**Question:** What is `"use strict"` and what does it change?

**Answer:** Strict mode is an opt-in mode that makes JavaScript stricter: assigning to undeclared variables throws an error, duplicate parameter names are not allowed, `this` is `undefined` (not the global object) in plain function calls, writing to read-only properties throws, and some reserved words are blocked. ES modules and classes are strict by default.

### বাংলা

**প্রশ্ন:** `"use strict"` কী এবং কী পরিবর্তন করে?

**উত্তর:** Strict mode হলো ঐচ্ছিক একটি মোড, যা JavaScript-কে কঠোর করে: declare না করা variable-এ মান দিলে error আসে, parameter-এর নাম পুনরাবৃত্তি করা যায় না, সাধারণ function call-এ `this` হয় `undefined` (global object নয়), read-only property-তে লিখলে error আসে এবং কিছু reserved word নিষিদ্ধ। ES module ও class ডিফল্টভাবেই strict।

### Example

```js
"use strict";

// undeclaredVar = 10;      // ReferenceError

function show() {
  return this;
}
console.log(show()); // undefined (not window)

const obj = Object.freeze({ a: 1 });
// obj.a = 2;               // TypeError
```

---

## Q43. Polyfill: implement `Array.prototype.map`

### English

**Question:** Write your own implementation of `Array.prototype.map`.

**Answer:** A polyfill adds a missing feature using existing code. `map` must call the callback with `(element, index, array)`, support an optional `thisArg`, return a new array of the same length, and not mutate the original. Interviewers use this to check understanding of `this`, prototypes and callbacks.

### বাংলা

**প্রশ্ন:** `Array.prototype.map` নিজে লিখে দেখান।

**উত্তর:** Polyfill হলো বিদ্যমান code দিয়ে অনুপস্থিত feature যোগ করা। `map`-কে callback-এ `(element, index, array)` পাঠাতে হবে, ঐচ্ছিক `thisArg` সমর্থন করতে হবে, একই দৈর্ঘ্যের নতুন array return করতে হবে এবং মূল array বদলানো যাবে না। Interviewer এর মাধ্যমে `this`, prototype ও callback-এর ধারণা যাচাই করেন।

### Example

```js
Array.prototype.myMap = function (callback, thisArg) {
  if (typeof callback !== "function") {
    throw new TypeError(`${callback} is not a function`);
  }
  const result = [];
  for (let i = 0; i < this.length; i++) {
    if (i in this) {
      result[i] = callback.call(thisArg, this[i], i, this);
    }
  }
  return result;
};

console.log([1, 2, 3].myMap((n) => n * 2)); // [2, 4, 6]
```

---

## Q44. `Object.keys`, `values`, `entries` and `assign`

### English

**Question:** What are `Object.keys`, `Object.values`, `Object.entries` and `Object.assign`?

**Answer:** `Object.keys(obj)` returns an array of own enumerable property names, `Object.values(obj)` returns the values, and `Object.entries(obj)` returns `[key, value]` pairs. `Object.fromEntries` converts pairs back to an object. `Object.assign(target, ...sources)` copies own enumerable properties into the target (shallow) and mutates the target.

### বাংলা

**প্রশ্ন:** `Object.keys`, `values`, `entries` ও `assign` কী?

**উত্তর:** `Object.keys(obj)` object-এর নিজস্ব enumerable property-র নামের array দেয়, `Object.values(obj)` মানগুলোর array দেয়, আর `Object.entries(obj)` `[key, value]` জোড়ার array দেয়। `Object.fromEntries` জোড়া থেকে আবার object বানায়। `Object.assign(target, ...sources)` source-এর নিজস্ব enumerable property গুলো target-এ copy করে (shallow) এবং target-কেই বদলায়।

### Example

```js
const user = { name: "Sohel", age: 24 };

Object.keys(user); // ["name", "age"]
Object.values(user); // ["Sohel", 24]
Object.entries(user); // [["name","Sohel"], ["age",24]]

const doubled = Object.fromEntries(
  Object.entries({ a: 1, b: 2 }).map(([k, v]) => [k, v * 2]),
); // { a: 2, b: 4 }

const merged = Object.assign({}, user, { city: "Rajshahi" });
```

---

## Q45. Implement deep clone

### English

**Question:** Write a function to deep clone an object.

**Answer:** In real projects use `structuredClone()`. In an interview, implement it recursively: return primitives as they are, handle `Date`, arrays and objects, and copy each own property recursively. A robust version also tracks circular references with a `WeakMap`.

### বাংলা

**প্রশ্ন:** একটি object deep clone করার function লিখুন।

**উত্তর:** বাস্তব project-এ `structuredClone()` ব্যবহার করুন। Interview-তে recursive ভাবে লিখতে হয়: primitive যেমন আছে তেমন return করুন, `Date`, array ও object আলাদা করে সামলান এবং প্রতিটি নিজস্ব property recursive ভাবে copy করুন। উন্নত সংস্করণে circular reference ধরতে `WeakMap` ব্যবহার করা হয়।

### Example

```js
function deepClone(value, seen = new WeakMap()) {
  if (value === null || typeof value !== "object") return value;
  if (value instanceof Date) return new Date(value);
  if (seen.has(value)) return seen.get(value); // circular reference

  const copy = Array.isArray(value) ? [] : {};
  seen.set(value, copy);

  for (const key of Object.keys(value)) {
    copy[key] = deepClone(value[key], seen);
  }
  return copy;
}

const original = { a: 1, nested: { b: [1, 2, { c: 3 }] } };
const clone = deepClone(original);
clone.nested.b[2].c = 99;
console.log(original.nested.b[2].c); // 3
```

---

## Q46. Flatten a nested array

### English

**Question:** Write a function to flatten a nested array.

**Answer:** The built-in way is `arr.flat(Infinity)`. For interviews, implement it recursively (or with `reduce`): for each element, if it is an array, flatten it and concatenate, otherwise push it. An iterative version using a stack avoids deep recursion.

### বাংলা

**প্রশ্ন:** Nested array-কে flatten করার function লিখুন।

**উত্তর:** বিল্ট-ইন উপায় হলো `arr.flat(Infinity)`। Interview-তে recursive ভাবে (বা `reduce` দিয়ে) লিখতে হয়: প্রতিটি উপাদান array হলে সেটিকে flatten করে জুড়ুন, নইলে সরাসরি যোগ করুন। Stack ব্যবহার করে iterative সংস্করণ লিখলে গভীর recursion এড়ানো যায়।

### Example

```js
const nested = [1, [2, [3, [4]], 5]];

console.log(nested.flat(Infinity)); // [1, 2, 3, 4, 5]

function flatten(arr) {
  return arr.reduce(
    (acc, item) => acc.concat(Array.isArray(item) ? flatten(item) : item),
    [],
  );
}
console.log(flatten(nested)); // [1, 2, 3, 4, 5]
```

---

## Q47. Remove duplicates from an array

### English

**Question:** How do you remove duplicates from an array?

**Answer:** The shortest way is `[...new Set(arr)]`. Alternatives are `filter` with `indexOf` (O(n²)) and `reduce` with an object or Map as a lookup (O(n)). For arrays of objects, deduplicate by a key such as `id` using a `Map`.

### বাংলা

**প্রশ্ন:** Array থেকে duplicate কীভাবে সরাবেন?

**উত্তর:** সবচেয়ে ছোট উপায় `[...new Set(arr)]`। বিকল্প হলো `filter` ও `indexOf` (O(n²)) অথবা `reduce` দিয়ে object বা Map ব্যবহার করে খোঁজা (O(n))। Object-এর array হলে `id`-এর মতো কোনো key ধরে `Map` দিয়ে duplicate সরানো হয়।

### Example

```js
const nums = [1, 2, 2, 3, 3, 3];

console.log([...new Set(nums)]); // [1, 2, 3]
console.log(nums.filter((n, i) => nums.indexOf(n) === i)); // [1, 2, 3]

const users = [
  { id: 1, n: "A" },
  { id: 1, n: "A" },
  { id: 2, n: "B" },
];
const uniqueUsers = [...new Map(users.map((u) => [u.id, u])).values()];
console.log(uniqueUsers); // [{id:1,...}, {id:2,...}]
```

---

## Q48. Reverse a string and check for a palindrome

### English

**Question:** Reverse a string, and check if a string is a palindrome.

**Answer:** Strings are immutable, so split into characters, reverse the array and join back: `str.split("").reverse().join("")`. A palindrome reads the same forwards and backwards. Normalize the string first (lowercase, remove non-alphanumeric characters) and compare it with its reverse. A two-pointer approach checks from both ends in O(n) without building a new string.

### বাংলা

**প্রশ্ন:** একটি string উল্টান এবং palindrome কিনা যাচাই করুন।

**উত্তর:** String অপরিবর্তনীয়, তাই অক্ষরে ভেঙে array বানিয়ে উল্টে আবার জোড়া লাগাতে হয়: `str.split("").reverse().join("")`। Palindrome হলো যা সামনে থেকে ও পিছন থেকে একই পড়া যায়। আগে string-কে স্বাভাবিক করুন (ছোট হাতের অক্ষর, অক্ষর-সংখ্যা ছাড়া বাকি বাদ), তারপর উল্টানো রূপের সাথে তুলনা করুন। Two-pointer পদ্ধতিতে দুই প্রান্ত থেকে O(n) সময়ে নতুন string না বানিয়েই যাচাই করা যায়।

### Example

```js
const reverse = (s) => s.split("").reverse().join("");
console.log(reverse("hello")); // "olleh"

function isPalindrome(str) {
  const clean = str.toLowerCase().replace(/[^a-z0-9]/g, "");
  let left = 0,
    right = clean.length - 1;
  while (left < right) {
    if (clean[left++] !== clean[right--]) return false;
  }
  return true;
}
console.log(isPalindrome("A man, a plan, a canal: Panama")); // true
console.log(isPalindrome("hello")); // false
```

---

## Q49. FizzBuzz and Two Sum

### English

**Question:** Solve FizzBuzz and the Two Sum problem.

**Answer:** FizzBuzz: for numbers 1 to n, print "FizzBuzz" if divisible by 3 and 5, "Fizz" if divisible by 3, "Buzz" if divisible by 5, otherwise the number. Check the combined condition first. Two Sum: given an array and a target, return the indices of two numbers that add up to the target. The brute force is O(n²); using a Map to store seen numbers gives O(n).

### বাংলা

**প্রশ্ন:** FizzBuzz ও Two Sum সমাধান করুন।

**উত্তর:** FizzBuzz: ১ থেকে n পর্যন্ত সংখ্যায় ৩ ও ৫ দুটো দিয়ে ভাগ গেলে "FizzBuzz", শুধু ৩ দিয়ে গেলে "Fizz", শুধু ৫ দিয়ে গেলে "Buzz", নইলে সংখ্যাটিই প্রিন্ট করুন। দুটোর মিলিত শর্ত আগে যাচাই করতে হবে। Two Sum: একটি array ও একটি target দেওয়া থাকলে, যে দুটি সংখ্যার যোগফল target-এর সমান তাদের index বের করুন। Brute force O(n²); Map-এ দেখা সংখ্যা রেখে করলে O(n)।

### Example

```js
for (let i = 1; i <= 15; i++) {
  if (i % 15 === 0) console.log("FizzBuzz");
  else if (i % 3 === 0) console.log("Fizz");
  else if (i % 5 === 0) console.log("Buzz");
  else console.log(i);
}

function twoSum(nums, target) {
  const seen = new Map(); // value -> index
  for (let i = 0; i < nums.length; i++) {
    const need = target - nums[i];
    if (seen.has(need)) return [seen.get(need), i];
    seen.set(nums[i], i);
  }
  return [];
}
console.log(twoSum([2, 7, 11, 15], 9)); // [0, 1]
```

---

## Q50. `sort()` pitfalls

### English

**Question:** Why does `[10, 9, 1].sort()` give `[1, 10, 9]`? How do you sort correctly?

**Answer:** By default, `sort()` converts elements to strings and compares them by UTF-16 code units, so `"10"` comes before `"9"`. To sort numbers, pass a compare function: `(a, b) => a - b` for ascending and `(a, b) => b - a` for descending. `sort()` also mutates the original array; use `toSorted()` or copy first (`[...arr].sort()`) to avoid that. To sort strings with locale rules, use `localeCompare`.

### বাংলা

**প্রশ্ন:** `[10, 9, 1].sort()` কেন `[1, 10, 9]` দেয়? সঠিকভাবে sort কীভাবে করবেন?

**উত্তর:** ডিফল্টভাবে `sort()` উপাদানগুলোকে string-এ রূপান্তর করে UTF-16 কোড অনুযায়ী তুলনা করে, তাই `"10"`, `"9"`-এর আগে আসে। সংখ্যা sort করতে compare function দিতে হয়: ছোট থেকে বড় করতে `(a, b) => a - b`, বড় থেকে ছোট করতে `(a, b) => b - a`। `sort()` মূল array-ও বদলে দেয়; এড়াতে `toSorted()` বা আগে copy (`[...arr].sort()`) করুন। ভাষা-নিয়ম মেনে string sort করতে `localeCompare` ব্যবহার করুন।

### Example

```js
console.log([10, 9, 1].sort()); // [1, 10, 9] (wrong for numbers)
console.log([10, 9, 1].sort((a, b) => a - b)); // [1, 9, 10]
console.log([10, 9, 1].sort((a, b) => b - a)); // [10, 9, 1]

const people = [
  { name: "Rana", age: 30 },
  { name: "Sohel", age: 24 },
];
people.sort((a, b) => a.age - b.age); // sort objects by age
["b", "a", "c"].sort((a, b) => a.localeCompare(b));

const original = [3, 1, 2];
const sorted = original.toSorted((a, b) => a - b); // original unchanged
```
