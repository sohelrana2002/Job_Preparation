#### JavaScript-এ Callback Function হলো এমন একটি ফাংশন, যাকে আমরা আর্গুমেন্ট (Parameter) আকারে অন্য কোনো ফাংশনের ভেতরে পাঠাই। আর সেই বাইরের ফাংশনটি যখন প্রয়োজন হয় (অর্থাৎ, নির্দিষ্ট কোনো কাজ শেষ হলে) তখন এই ফাংশনটিকে "ডাকাডাকি" (Call back) করে।

##### এটাই JavaScript-এর সবচেয়ে শক্তিশালী এবং মৌলিক একটি ফিচার, কারণ JavaScript ফাংশনকে First-Class Citizen হিসেবে ব্যবহার করে—মানে আপনি ফাংশনকে ভেরিয়েবলের মতো store করতে পারেন, অথবা অন্য ফাংশনে পাস করতে পারেন।

### ১. রিয়েল-লাইফ Example:

ধরুন, আপনি একটি ফাস্টফুড রেস্টুরেন্টে গিয়ে অর্ডার দিলেন (যেমন: বার্গার)। অর্ডার দিতে ৫ মিনিট সময় লাগে। আপনি যদি কাউন্টারে দাঁড়িয়ে থাকেন, তাহলে আপনার সময় নষ্ট হবে।

তাই, আপনি রেস্টুরেন্টের কর্মীকে আপনার মোবাইল নম্বর (এটাই আপনার Callback Function) দিয়ে দিলেন, আর বললেন—"বার্গার ready হলে এই নম্বরে ফোন করে জানাবেন"।

এখন কর্মী আপনার নম্বরটি লিখে রাখলেন (Function Store করলেন), বার্গার বানানো শেষ হলে তিনি সেই নম্বরে ফোন করলেন (Callback Execute করলেন)। এখানে আপনি বাইরের কাজ করতে পারলেন, বার্গার তৈরি হয়ে গেলে আপনার ফাংশন (ফোন রিসিভ) কাজ করলো। এই পুরো প্রক্রিয়াটাই Callback।

### ২. টেকনিক্যাল Definition: (2 parts)

- Higher-Order Function: যে ফাংশনটি অন্য ফাংশনকে প্যারামিটার হিসেবে গ্রহণ করে।

- Callback Function: যে ফাংশনটিকে প্যারামিটার হিসেবে পাঠানো হয়, এবং পরবর্তীতে Higher-Order Function দ্বারা Execute করা হয়।

### ৩. 2 types of Callback

ক. Synchronous Callback (ব্লকিং)
এই ক্ষেত্রে Callback ফাংশনটি এখনই, অর্থাৎ যেই লাইনে লেখা আছে সেই লাইনেই Execute হয়। কোনো বিলম্ব বা অপেক্ষা নেই।
উদাহরণ: Array.map(), forEach(), filter() ইত্যাদি।

খ. Asynchronous Callback (নন-ব্লকিং)
এই ক্ষেত্রে Callback ফাংশনটি এখনই Execute হয় না, বরং নির্দিষ্ট কোনো ইভেন্ট (যেমন: ক্লিক করা), টাইমার শেষ হওয়া, অথবা সার্ভার থেকে ডেটা আসার পর Execute হয়। JavaScript এর Event Loop এই কাজটা করে।
উদাহরণ: setTimeout, addEventListener, fetch() (API কল)।

**Example:**

```javascript
// Synchronous Callback
// এটি একটি Higher-Order Function (কারণ এটি callback নিচ্ছে)
function processUserInput(name, callback) {
  console.log("User name: " + name);
  // callback ফাংশনটিকে এখানে ডাকা হচ্ছে (এখনই)
  callback(name);
}

// এটি হলো আমাদের Callback Function
function greetUser(username) {
  console.log("Hello, " + username + "! Best wishes for you.");
}

// processUserInput-এ greetUser-কে পাস করছি (এটাই callback)
processUserInput("SOHEL", greetUser);

// OUTPUT
// User name: SOHEL
// Hello, SOHEL! Best wishes for you.

// Asynchronous Callback
console.log("1. Start working...");

// এখানে setTimeout একটি Higher-Order Function, আর এর ভিতরের ফাংশনটি হলো Callback
setTimeout(function () {
  console.log("3. 2-sec done! (This line print later)");
}, 2000);

console.log("2. This line print before");

// OUTPUT ORDER:
// 1. Start working...
// 2. This line print before
// (২-sec later)
// 3. 2-sec done! (This line print later)
```

Explanation: এখানে কী হলো? JavaScript setTimeout-কে Browser-এর কাছে ছেড়ে দিয়ে (Web API-তে) তার নিজের কাজ (console.log("2")) করে ফেললো। ২ সেকেন্ড পর Browser যখন টাইমার শেষ করলো, তখন JavaScript-এর Event Loop আবার সেই Callback ফাংশনটিকে তুলে নিয়ে Execute করলো। এটাই Asynchronous Callback-এর ম্যাজিক।

### ৬. Callback-এর একটা বড় সমস্যা (এবং কেন আমরা Promise/Async-Await শিখি)

আপনি যদি অনেকগুলো Asynchronous কাজকে পরপর করতে চান (যেমন: ডেটা আনুন -> সেই ডেটা দিয়ে আরেকটা API কল করুন -> তারপর আবার আরেকটা), তাহলে Callback-এর ভেতরে Callback, তার ভেতরে আবার Callback লিখতে হয়। এটাকে বলে Callback Hell (বা Pyramid of Doom)। কোডটি পড়তে খুব খারাপ লাগে এবং ডিবাগ করা কঠিন হয়।

**Example: Callback Hell**

```javascript
getUser(function (user) {
  getPosts(user.id, function (posts) {
    getComments(posts[0].id, function (comments) {
      console.log(comments); // nesting!
    });
  });
});
```

##### Note: এই সমস্যা সমাধানের জন্যই JavaScript-এ Promise এবং Async/Await এসেছে। কিন্তু মনে রাখবেন, Promise এবং Async/Await-এর মূল ভিত্তি কিন্তু এই Callback কনসেপ্টই।
