# Node.js Core and Event Loop (Q1 - Q15)

> Place in: `03-backend/nodejs/01-core-and-event-loop.md`
> Advanced and production questions (Q16-Q30) are in `02-advanced-and-production.md`.
> Code uses ES Modules (`import`). Use `"type": "module"` in `package.json` or the `.mjs` extension. Express, REST API design, authentication and error-handling middleware will be in the next backend files.

## Top 30 Index

| #   | Topic                                                   | File |
| --- | ------------------------------------------------------- | ---- |
| 1   | What is Node.js and why use it                          | 01   |
| 2   | The event loop and its phases                           | 01   |
| 3   | `nextTick` vs `setImmediate` vs `setTimeout` vs Promise | 01   |
| 4   | Blocking vs non-blocking, error-first callbacks         | 01   |
| 5   | libuv and the thread pool                               | 01   |
| 6   | CommonJS vs ES Modules in Node.js                       | 01   |
| 7   | npm, `package.json`, semver, lock files                 | 01   |
| 8   | The `process` object and globals                        | 01   |
| 9   | EventEmitter                                            | 01   |
| 10  | Streams and backpressure                                | 01   |
| 11  | Buffers                                                 | 01   |
| 12  | `fs` and `path` modules                                 | 01   |
| 13  | Creating an HTTP server without Express                 | 01   |
| 14  | Error handling in Node.js                               | 01   |
| 15  | Promisify and async patterns                            | 01   |
| 16  | `cluster` vs `worker_threads` vs `child_process`        | 02   |
| 17  | CPU-intensive tasks                                     | 02   |
| 18  | Memory leaks and the heap                               | 02   |
| 19  | Environment configuration and `NODE_ENV`                | 02   |
| 20  | Security best practices                                 | 02   |
| 21  | The `crypto` module and password hashing                | 02   |
| 22  | Graceful shutdown                                       | 02   |
| 23  | Logging, monitoring, PM2 and health checks              | 02   |
| 24  | Performance optimization                                | 02   |
| 25  | Testing in Node.js                                      | 02   |
| 26  | WebSockets and real-time communication                  | 02   |
| 27  | When to use (and not use) Node.js                       | 02   |
| 28  | What is new in modern Node.js                           | 02   |
| 29  | Concurrency control (promise pool)                      | 02   |
| 30  | Practical: processing large files with streams          | 02   |

---

## Q1. What is Node.js and why use it?

### English

**Question:** What is Node.js? Why is it fast, and what is it good for?

**Answer:** Node.js is an open-source JavaScript runtime built on Chrome's V8 engine that lets you run JavaScript outside the browser, mostly on servers. It uses an event-driven, non-blocking I/O model: instead of creating one thread per request, a single main thread handles many connections by delegating I/O (network, files, database) to the system and running callbacks when results are ready. This makes it efficient and lightweight for I/O-heavy workloads such as REST APIs, real-time apps (chat, notifications), streaming and microservices. Other benefits: one language for frontend and backend, a huge npm ecosystem and fast startup.

### বাংলা

**প্রশ্ন:** Node.js কী? এটি কেন দ্রুত এবং কীসের জন্য ভালো?

**উত্তর:** Node.js হলো Chrome-এর V8 engine-এর উপর তৈরি একটি open-source JavaScript runtime, যা browser-এর বাইরে, বিশেষ করে server-এ, JavaScript চালাতে দেয়। এটি event-driven ও non-blocking I/O মডেল ব্যবহার করে: প্রতিটি request-এর জন্য আলাদা thread না বানিয়ে একটি মূল thread অনেক সংযোগ সামলায়, I/O (network, file, database) system-কে দিয়ে দেয় এবং ফলাফল এলে callback চালায়। তাই REST API, real-time app (chat, notification), streaming ও microservice-এর মতো I/O-নির্ভর কাজে এটি কার্যকর ও হালকা। অন্যান্য সুবিধা: frontend ও backend-এ একই ভাষা, বিশাল npm ecosystem এবং দ্রুত চালু হওয়া।

### Example

```js
// server.mjs
import http from "node:http";

const server = http.createServer((req, res) => {
  res.writeHead(200, { "Content-Type": "text/plain" });
  res.end("Hello from Node.js");
});

server.listen(3000, () => console.log("Listening on http://localhost:3000"));
```

---

## Q2. The event loop and its phases

### English

**Question:** Explain the Node.js event loop and its phases.

**Answer:** The event loop is what lets Node.js perform non-blocking I/O on a single thread. It runs in phases, each with a queue of callbacks: (1) timers (`setTimeout`, `setInterval`), (2) pending callbacks (some system-level callbacks), (3) idle/prepare (internal), (4) poll (retrieve new I/O events and run I/O callbacks; it may wait here), (5) check (`setImmediate`), and (6) close callbacks (`socket.on("close")`). Between every phase, Node runs the `process.nextTick` queue first and then the Promise microtask queue. A long synchronous task blocks the whole loop, so no other request is served meanwhile.

### বাংলা

**প্রশ্ন:** Node.js-এর event loop ও তার ধাপগুলো ব্যাখ্যা করুন।

**উত্তর:** Event loop-ই Node.js-কে একটি thread-এ non-blocking I/O করতে দেয়। এটি কয়েকটি ধাপে চলে, প্রতিটির নিজস্ব callback queue আছে: (১) timers (`setTimeout`, `setInterval`), (২) pending callbacks (কিছু system-স্তরের callback), (৩) idle/prepare (অভ্যন্তরীণ), (৪) poll (নতুন I/O event নেওয়া ও I/O callback চালানো; এখানে অপেক্ষা করতে পারে), (৫) check (`setImmediate`), এবং (৬) close callbacks (`socket.on("close")`)। প্রতিটি ধাপের মাঝে Node আগে `process.nextTick` queue, তারপর Promise microtask queue চালায়। দীর্ঘ synchronous কাজ পুরো loop আটকে দেয়, ফলে ততক্ষণ অন্য কোনো request সামলানো যায় না।

### Example

```js
import fs from "node:fs";

setTimeout(() => console.log("timeout"), 0);
setImmediate(() => console.log("immediate"));
process.nextTick(() => console.log("nextTick"));
Promise.resolve().then(() => console.log("promise"));
console.log("sync");
// sync, nextTick, promise, then timeout/immediate (order not guaranteed here)

fs.readFile(import.meta.filename, () => {
  setTimeout(() => console.log("timeout (in I/O)"), 0);
  setImmediate(() => console.log("immediate (in I/O)")); // always first inside an I/O callback
});
```

---

## Q3. `nextTick` vs `setImmediate` vs `setTimeout` vs Promise

### English

**Question:** What is the difference between `process.nextTick()`, `setImmediate()`, `setTimeout(fn, 0)` and Promise callbacks?

**Answer:** `process.nextTick` callbacks run right after the current operation finishes, before the event loop continues and even before Promise microtasks. Promise `.then`/`await` continuations run in the microtask queue right after the `nextTick` queue. `setTimeout(fn, 0)` runs in the timers phase of a later loop iteration (minimum about 1 ms). `setImmediate` runs in the check phase, right after the poll phase. Overusing `nextTick` recursively can starve the event loop because I/O never gets a chance to run.

### বাংলা

**প্রশ্ন:** `process.nextTick()`, `setImmediate()`, `setTimeout(fn, 0)` ও Promise callback-এর পার্থক্য কী?

**উত্তর:** `process.nextTick`-এর callback চলমান কাজ শেষ হওয়ার ঠিক পরেই চলে, event loop এগোনোর আগে এবং Promise microtask-এরও আগে। Promise-এর `.then`/`await` microtask queue-তে `nextTick` queue-র ঠিক পরে চলে। `setTimeout(fn, 0)` পরবর্তী loop-এর timers ধাপে চলে (সর্বনিম্ন প্রায় ১ ms)। `setImmediate` check ধাপে, অর্থাৎ poll ধাপের ঠিক পরে চলে। `nextTick` বারবার recursive ভাবে ব্যবহার করলে event loop অনাহারে থাকে (starvation), কারণ I/O চলার সুযোগই পায় না।

### Example

```js
console.log("1: sync");
setTimeout(() => console.log("5: setTimeout"), 0);
setImmediate(() => console.log("6: setImmediate"));
Promise.resolve().then(() => console.log("4: promise"));
process.nextTick(() => console.log("3: nextTick"));
console.log("2: sync end");
// Order: 1, 2, 3 (nextTick), 4 (promise), then 5/6
```

---

## Q4. Blocking vs non-blocking, error-first callbacks

### English

**Question:** What is the difference between blocking and non-blocking code? What are error-first callbacks?

**Answer:** Blocking code (for example `fs.readFileSync`) stops the thread until the operation finishes, so no other work runs. Non-blocking code (`fs.readFile`, `fs.promises.readFile`) starts the operation and continues; the result arrives later through a callback, Promise or `await`. In servers, always use non-blocking calls inside request handlers. Synchronous APIs are acceptable only at startup (such as reading config). The Node.js callback convention is error-first: the first argument is the error (or `null`) and the second is the result.

### বাংলা

**প্রশ্ন:** Blocking ও non-blocking code-এর পার্থক্য কী? Error-first callback কী?

**উত্তর:** Blocking code (যেমন `fs.readFileSync`) কাজ শেষ না হওয়া পর্যন্ত thread থামিয়ে রাখে, ফলে আর কিছু চলে না। Non-blocking code (`fs.readFile`, `fs.promises.readFile`) কাজ শুরু করে এগিয়ে যায়; ফলাফল পরে callback, Promise বা `await`-এর মাধ্যমে আসে। Server-এ request handler-এর ভেতরে সবসময় non-blocking call ব্যবহার করুন। Synchronous API শুধু চালু হওয়ার সময় (যেমন config পড়া) গ্রহণযোগ্য। Node.js-এর callback নিয়ম হলো error-first: প্রথম argument error (বা `null`) এবং দ্বিতীয়টি ফলাফল।

### Example

```js
import fs from "node:fs";
import { readFile } from "node:fs/promises";

// Blocking: everything waits
const data = fs.readFileSync("big.txt", "utf8");

// Non-blocking, error-first callback
fs.readFile("big.txt", "utf8", (err, data) => {
  if (err) return console.error("Failed:", err.message);
  console.log(data.length);
});

// Non-blocking, async/await
try {
  const text = await readFile("big.txt", "utf8");
} catch (err) {
  console.error(err.message);
}
```

---

## Q5. libuv and the thread pool

### English

**Question:** What is libuv? Is Node.js really single-threaded?

**Answer:** libuv is a C library that provides the event loop and asynchronous I/O to Node.js across operating systems. Your JavaScript runs on a single main thread, but Node.js is not entirely single-threaded. Network I/O uses the operating system's async mechanisms (epoll, kqueue, IOCP), while some operations run in libuv's thread pool (default size 4, configurable with `UV_THREADPOOL_SIZE`): file system calls, `dns.lookup`, and CPU-heavy `crypto` (pbkdf2, scrypt) and `zlib` functions. V8 and the garbage collector also use extra threads.

### বাংলা

**প্রশ্ন:** libuv কী? Node.js কি সত্যিই single-threaded?

**উত্তর:** libuv হলো একটি C library, যা বিভিন্ন operating system-এ Node.js-কে event loop ও asynchronous I/O দেয়। আপনার JavaScript একটি মূল thread-এ চলে, কিন্তু Node.js পুরোপুরি single-threaded নয়। Network I/O operating system-এর async ব্যবস্থা (epoll, kqueue, IOCP) ব্যবহার করে, আর কিছু কাজ libuv-র thread pool-এ চলে (ডিফল্ট আকার ৪, `UV_THREADPOOL_SIZE` দিয়ে বদলানো যায়): file system call, `dns.lookup`, এবং ভারী `crypto` (pbkdf2, scrypt) ও `zlib` function। V8 ও garbage collector-ও আলাদা thread ব্যবহার করে।

### Example

```js
import crypto from "node:crypto";

const start = Date.now();
for (let i = 1; i <= 8; i++) {
  crypto.pbkdf2("password", "salt", 500_000, 64, "sha512", () => {
    console.log(`Hash ${i} done after ${Date.now() - start} ms`);
  });
}
// With the default pool of 4 threads, hashes finish in two batches of four.
```

```bash
UV_THREADPOOL_SIZE=8 node app.mjs   # set before the process starts
```

---

## Q6. CommonJS vs ES Modules in Node.js

### English

**Question:** What is the difference between CommonJS and ES Modules in Node.js?

**Answer:** CommonJS (`require`, `module.exports`) is the original system: modules load synchronously and are cached after the first `require`. ES Modules (`import`, `export`) are the standard: they are asynchronous, support top-level `await`, and are statically analyzable. Node.js treats `.mjs` files, and `.js` files with `"type": "module"` in `package.json`, as ES Modules. In ESM, relative imports need the file extension, and `__dirname`/`__filename` are not available (use `import.meta.dirname` and `import.meta.filename` in recent versions, or `import.meta.url`). `exports` is just a reference to `module.exports`; reassigning `exports` breaks it. Recent Node.js versions also support `require()` of ES modules, which eases mixing the two.

### বাংলা

**প্রশ্ন:** Node.js-এ CommonJS ও ES Modules-এর পার্থক্য কী?

**উত্তর:** CommonJS (`require`, `module.exports`) আদি ব্যবস্থা: module synchronous ভাবে লোড হয় এবং প্রথমবার `require`-এর পর cache হয়। ES Modules (`import`, `export`) স্ট্যান্ডার্ড: এগুলো asynchronous, top-level `await` সমর্থন করে এবং আগে থেকেই বিশ্লেষণযোগ্য। Node.js `.mjs` file এবং `package.json`-এ `"type": "module"` থাকা `.js` file-কে ES Module হিসেবে ধরে। ESM-এ relative import-এ file-এর extension লাগে এবং `__dirname`/`__filename` থাকে না (সাম্প্রতিক সংস্করণে `import.meta.dirname` ও `import.meta.filename`, বা `import.meta.url` ব্যবহার করুন)। `exports` আসলে `module.exports`-এর একটি reference; `exports`-এ নতুন মান বসালে সেটি কাজ করে না। সাম্প্রতিক Node.js সংস্করণে ES module `require()` করাও যায়, ফলে দুটি মেশানো সহজ হয়েছে।

### Example

```js
// CommonJS
const path = require("path");
module.exports = { add: (a, b) => a + b };
// exports = { add }   // wrong: breaks the link to module.exports

// ES Modules
import path from "node:path";
import { add } from "./math.js"; // extension required
export const sub = (a, b) => a - b;
export default function main() {}

const data = await fetch("https://api.example.com"); // top-level await
console.log(import.meta.dirname);
```

```json
{ "type": "module" }
```

---

## Q7. npm, `package.json`, semver and lock files

### English

**Question:** Explain `package.json`, semantic versioning and the lock file. What are the types of dependencies?

**Answer:** `package.json` describes the project: name, version, scripts, entry point and dependencies. `dependencies` are needed at runtime, `devDependencies` only for development (tests, linters, build tools), and `peerDependencies` are packages the host project must provide. Semantic versioning is MAJOR.MINOR.PATCH: `^1.2.3` allows minor and patch updates (up to `<2.0.0`), `~1.2.3` allows only patch updates, and an exact version is pinned. The lock file (`package-lock.json`) records exact installed versions so installs are reproducible; commit it, and use `npm ci` in CI for clean, deterministic installs. Run `npm audit` to check vulnerabilities.

### বাংলা

**প্রশ্ন:** `package.json`, semantic versioning ও lock file ব্যাখ্যা করুন। Dependency-র ধরন কী কী?

**উত্তর:** `package.json` project-এর বর্ণনা দেয়: নাম, সংস্করণ, script, entry point ও dependency। `dependencies` runtime-এ লাগে, `devDependencies` শুধু development-এ (test, linter, build tool), আর `peerDependencies` এমন package যা host project-কে সরবরাহ করতে হবে। Semantic versioning হলো MAJOR.MINOR.PATCH: `^1.2.3` minor ও patch আপডেট অনুমোদন করে (`<2.0.0` পর্যন্ত), `~1.2.3` শুধু patch আপডেট, আর নির্দিষ্ট সংস্করণ লিখলে সেটিই স্থির থাকে। Lock file (`package-lock.json`) ঠিক কোন সংস্করণ install হয়েছে তা লিখে রাখে, ফলে install পুনরুৎপাদনযোগ্য হয়; এটি commit করুন, আর CI-তে পরিষ্কার ও নির্ধারিত install-এর জন্য `npm ci` ব্যবহার করুন। দুর্বলতা যাচাই করতে `npm audit` চালান।

### Example

```json
{
  "name": "my-api",
  "version": "1.0.0",
  "type": "module",
  "main": "src/index.js",
  "scripts": {
    "start": "node src/index.js",
    "dev": "node --watch src/index.js",
    "test": "node --test"
  },
  "dependencies": { "express": "^5.0.0" },
  "devDependencies": { "eslint": "~9.0.0" },
  "engines": { "node": ">=20.9" }
}
```

```bash
npm install express        # adds to dependencies
npm install -D eslint      # adds to devDependencies
npm ci                     # clean install from the lock file (CI)
npm audit
```

---

## Q8. The `process` object and globals

### English

**Question:** What is the `process` object? What other global objects does Node.js provide?

**Answer:** `process` is a global object with information and control over the current Node.js process: `process.env` (environment variables), `process.argv` (command-line arguments), `process.cwd()`, `process.exit(code)`, `process.memoryUsage()`, `process.uptime()`, `process.hrtime.bigint()` (high-resolution timer), and event hooks such as `process.on("SIGTERM")` and `process.on("uncaughtException")`. Other globals: `globalThis`, `Buffer`, `setTimeout`/`setInterval`/`setImmediate`, `console`, `URL`, `fetch`, `AbortController`, `structuredClone`, and in CommonJS `__dirname`, `__filename`, `require` and `module`. Variables declared with `var` in a module are local to the module, not global.

### বাংলা

**প্রশ্ন:** `process` object কী? Node.js আর কী কী global object দেয়?

**উত্তর:** `process` একটি global object, যা বর্তমান Node.js process সম্পর্কে তথ্য ও নিয়ন্ত্রণ দেয়: `process.env` (environment variable), `process.argv` (command-line argument), `process.cwd()`, `process.exit(code)`, `process.memoryUsage()`, `process.uptime()`, `process.hrtime.bigint()` (সূক্ষ্ম timer), এবং `process.on("SIGTERM")` ও `process.on("uncaughtException")`-এর মতো event hook। অন্যান্য global: `globalThis`, `Buffer`, `setTimeout`/`setInterval`/`setImmediate`, `console`, `URL`, `fetch`, `AbortController`, `structuredClone`, এবং CommonJS-এ `__dirname`, `__filename`, `require` ও `module`। Module-এর ভেতরে `var` দিয়ে declare করা variable ওই module-এর নিজস্ব, global নয়।

### Example

```js
console.log(process.argv); // [nodePath, scriptPath, ...args]
console.log(process.env.PORT ?? 3000);
console.log(process.cwd(), process.pid, process.platform);
console.log(process.memoryUsage().heapUsed / 1024 / 1024, "MB");

const t0 = process.hrtime.bigint();
await doWork();
console.log(`Took ${Number(process.hrtime.bigint() - t0) / 1e6} ms`);

process.on("SIGINT", () => {
  console.log("Bye");
  process.exit(0);
});
```

---

## Q9. EventEmitter

### English

**Question:** What is `EventEmitter` and how does it work?

**Answer:** `EventEmitter` (from the `events` module) is the core of Node.js's event-driven design; many built-ins such as streams, HTTP servers and sockets extend it. You register listeners with `on`/`once`/`addListener`, trigger them with `emit(name, ...args)` and remove them with `off`/`removeListener`. Listeners run synchronously in the order they were registered. Emitting an `"error"` event with no listener throws and can crash the process, so always attach an error listener. Node.js warns when more than 10 listeners are added to one event, which usually signals a memory leak.

### বাংলা

**প্রশ্ন:** `EventEmitter` কী এবং কীভাবে কাজ করে?

**উত্তর:** `EventEmitter` (`events` module থেকে) Node.js-এর event-driven নকশার মূল অংশ; stream, HTTP server ও socket-এর মতো অনেক বিল্ট-ইন এটি extend করে। `on`/`once`/`addListener` দিয়ে listener নিবন্ধন করা হয়, `emit(name, ...args)` দিয়ে চালানো হয় এবং `off`/`removeListener` দিয়ে সরানো হয়। Listener গুলো নিবন্ধনের ক্রম অনুযায়ী synchronous ভাবে চলে। Listener ছাড়া `"error"` event emit করলে error throw হয় এবং process ভেঙে যেতে পারে, তাই সবসময় error listener বসান। একটি event-এ ১০টির বেশি listener যোগ করলে Node.js সতর্ক করে, যা সাধারণত memory leak-এর ইঙ্গিত।

### Example

```js
import { EventEmitter } from "node:events";

class OrderService extends EventEmitter {
  place(order) {
    // ...save order
    this.emit("orderPlaced", order);
  }
}

const orders = new OrderService();
orders.on("orderPlaced", (o) => console.log("Send email for", o.id));
orders.once("orderPlaced", (o) => console.log("First order only:", o.id));
orders.on("error", (err) => console.error("Order error:", err));

orders.place({ id: 1 });
orders.place({ id: 2 });
```

---

## Q10. Streams and backpressure

### English

**Question:** What are streams in Node.js? What is backpressure?

**Answer:** Streams process data piece by piece instead of loading everything into memory, making them memory-efficient and fast for large files, uploads, downloads and network data. Four types: Readable (source), Writable (destination), Duplex (both, such as a socket) and Transform (modifies data, such as gzip). Backpressure happens when the writer is slower than the reader: `writable.write()` returns `false`, and the producer should pause until the `"drain"` event. Using `pipe` or, better, `stream.pipeline` (which also handles errors and cleanup) manages backpressure automatically.

### বাংলা

**প্রশ্ন:** Node.js-এ stream কী? Backpressure কী?

**উত্তর:** Stream সবকিছু memory-তে লোড না করে টুকরো টুকরো করে data প্রক্রিয়া করে, ফলে বড় file, upload, download ও network data-র ক্ষেত্রে memory সাশ্রয়ী ও দ্রুত। চার ধরন: Readable (উৎস), Writable (গন্তব্য), Duplex (দুটোই, যেমন socket) এবং Transform (data বদলায়, যেমন gzip)। Backpressure হয় যখন লেখক পাঠকের চেয়ে ধীর: `writable.write()` `false` return করে এবং `"drain"` event না আসা পর্যন্ত উৎপাদককে থামতে হয়। `pipe` বা আরও ভালো `stream.pipeline` (যা error ও cleanup-ও সামলায়) ব্যবহার করলে backpressure স্বয়ংক্রিয়ভাবে সামলানো হয়।

### Example

```js
import { createReadStream, createWriteStream } from "node:fs";
import { createGzip } from "node:zlib";
import { pipeline } from "node:stream/promises";

// Compress a large file without loading it into memory
await pipeline(
  createReadStream("big.log"),
  createGzip(),
  createWriteStream("big.log.gz"),
);

// Manual backpressure
function writeMany(writer, items) {
  for (const item of items) {
    if (!writer.write(item)) {
      writer.once("drain", () =>
        writeMany(writer, items.slice(items.indexOf(item) + 1)),
      );
      return;
    }
  }
}
```

---

## Q11. Buffers

### English

**Question:** What is a Buffer in Node.js?

**Answer:** A `Buffer` is a fixed-size chunk of raw binary memory allocated outside the V8 heap. It is used to handle binary data such as files, TCP streams, images and encryption output, where JavaScript strings are not suitable. Create buffers with `Buffer.from(...)`, `Buffer.alloc(size)` (zero-filled, safe) or `Buffer.allocUnsafe(size)` (faster but may contain old data, so overwrite it fully). Buffers convert to strings with `toString(encoding)` (`utf8`, `hex`, `base64`) and can be joined with `Buffer.concat`. `Buffer` is a subclass of `Uint8Array`.

### বাংলা

**প্রশ্ন:** Node.js-এ Buffer কী?

**উত্তর:** `Buffer` হলো V8 heap-এর বাইরে বরাদ্দ করা নির্দিষ্ট মাপের কাঁচা binary memory-র টুকরো। File, TCP stream, ছবি ও encryption-এর ফলাফলের মতো binary data সামলাতে এটি ব্যবহার হয়, যেখানে JavaScript string উপযুক্ত নয়। `Buffer.from(...)`, `Buffer.alloc(size)` (শূন্য দিয়ে ভরা, নিরাপদ) বা `Buffer.allocUnsafe(size)` (দ্রুত কিন্তু পুরনো data থাকতে পারে, তাই পুরোটা নিজে লিখে দিন) দিয়ে buffer বানানো হয়। `toString(encoding)` (`utf8`, `hex`, `base64`) দিয়ে string-এ রূপান্তর হয় এবং `Buffer.concat` দিয়ে জোড়া লাগানো যায়। `Buffer` হলো `Uint8Array`-এর subclass।

### Example

```js
const buf = Buffer.from("Hello, Node", "utf8");
console.log(buf); // <Buffer 48 65 6c 6c 6f 2c 20 4e 6f 64 65>
console.log(buf.toString("hex"));
console.log(buf.toString("base64")); // SGVsbG8sIE5vZGU=
console.log(Buffer.from("SGVsbG8sIE5vZGU=", "base64").toString());

const zeros = Buffer.alloc(4); // <Buffer 00 00 00 00>
const joined = Buffer.concat([Buffer.from("A"), Buffer.from("B")]);
console.log(joined.length); // 2
```

---

## Q12. `fs` and `path` modules

### English

**Question:** How do you work with files and paths in Node.js? What is the difference between the `fs` APIs?

**Answer:** The `fs` module has three styles: synchronous (`readFileSync`, blocks the event loop), callback-based (`readFile`) and promise-based (`fs/promises`, preferred with `async/await`). `readFile` loads the whole file into memory, so use streams for large files. The `path` module builds cross-platform paths safely: `path.join`, `path.resolve`, `path.basename`, `path.extname`, `path.dirname`. Never concatenate user input into file paths directly, because it allows path traversal (`../../etc/passwd`); resolve the path and verify it stays inside the allowed folder.

### বাংলা

**প্রশ্ন:** Node.js-এ file ও path নিয়ে কীভাবে কাজ করেন? `fs` API-র ধরনগুলোর পার্থক্য কী?

**উত্তর:** `fs` module-এ তিন ধরনের পদ্ধতি আছে: synchronous (`readFileSync`, event loop আটকে দেয়), callback-ভিত্তিক (`readFile`) এবং promise-ভিত্তিক (`fs/promises`, `async/await`-এর সাথে পছন্দের)। `readFile` পুরো file memory-তে লোড করে, তাই বড় file-এর জন্য stream ব্যবহার করুন। `path` module নিরাপদে cross-platform path বানায়: `path.join`, `path.resolve`, `path.basename`, `path.extname`, `path.dirname`। ব্যবহারকারীর input সরাসরি file path-এ জুড়বেন না, কারণ এতে path traversal (`../../etc/passwd`) সম্ভব হয়; path resolve করে যাচাই করুন যে সেটি অনুমোদিত folder-এর ভেতরেই আছে।

### Example

```js
import { readFile, writeFile, mkdir, readdir, stat } from "node:fs/promises";
import path from "node:path";

const dir = path.join(import.meta.dirname, "data");
await mkdir(dir, { recursive: true });
await writeFile(path.join(dir, "note.txt"), "Hello", "utf8");
console.log(await readFile(path.join(dir, "note.txt"), "utf8"));
console.log(await readdir(dir), (await stat(dir)).isDirectory());

// Path traversal protection
const uploadRoot = path.resolve("uploads");
function safePath(userInput) {
  const full = path.resolve(uploadRoot, userInput);
  if (!full.startsWith(uploadRoot + path.sep)) throw new Error("Invalid path");
  return full;
}
```

---

## Q13. Creating an HTTP server without Express

### English

**Question:** How do you create an HTTP server using only the built-in `http` module? Why do people use Express?

**Answer:** `http.createServer((req, res) => {...})` creates a server; `req` is a readable stream with `method`, `url` and `headers`, and `res` is a writable stream used to set the status code and headers (`res.writeHead`) and send the body (`res.end`). You must parse the URL, route manually, read the body from the stream, and set `Content-Type` yourself. Express and similar frameworks (Fastify, Hono, Koa) wrap this with routing, middleware, body parsing, cleaner error handling and a large ecosystem, so you write less repetitive code.

### বাংলা

**প্রশ্ন:** শুধু বিল্ট-ইন `http` module দিয়ে HTTP server কীভাবে বানান? মানুষ কেন Express ব্যবহার করে?

**উত্তর:** `http.createServer((req, res) => {...})` server বানায়; `req` একটি readable stream যাতে `method`, `url` ও `headers` আছে, আর `res` একটি writable stream যা দিয়ে status code ও header ঠিক (`res.writeHead`) এবং body পাঠানো (`res.end`) হয়। URL parse করা, নিজে route করা, stream থেকে body পড়া এবং `Content-Type` নিজে ঠিক করা আপনার কাজ। Express ও অনুরূপ framework (Fastify, Hono, Koa) এগুলোকে routing, middleware, body parsing, পরিষ্কার error handling ও বড় ecosystem দিয়ে মুড়ে দেয়, ফলে বারবার একই code লিখতে হয় না।

### Example

```js
import http from "node:http";

const json = (res, status, data) => {
  res.writeHead(status, { "Content-Type": "application/json" });
  res.end(JSON.stringify(data));
};

const server = http.createServer(async (req, res) => {
  const url = new URL(req.url, `http://${req.headers.host}`);

  if (req.method === "GET" && url.pathname === "/health") {
    return json(res, 200, { status: "ok" });
  }

  if (req.method === "POST" && url.pathname === "/echo") {
    let body = "";
    for await (const chunk of req) body += chunk; // read the body stream
    try {
      return json(res, 200, { received: JSON.parse(body) });
    } catch {
      return json(res, 400, { error: "Invalid JSON" });
    }
  }

  json(res, 404, { error: "Not found" });
});

server.listen(3000);
```

---

## Q14. Error handling in Node.js

### English

**Question:** How do you handle errors in Node.js? What are `uncaughtException` and `unhandledRejection`?

**Answer:** Distinguish operational errors (expected runtime problems such as a failed network call, invalid input or a missing file, which you handle and recover from) from programmer errors (bugs such as reading a property of `undefined`, which you fix). Use `try/catch` with `async/await`, `.catch()` with Promises, and the error-first argument with callbacks; throw `Error` objects (or custom subclasses) with a message and status code. Since Node.js 15, an unhandled promise rejection crashes the process by default. `process.on("uncaughtException")` and `process.on("unhandledRejection")` should be used only to log and shut down gracefully, because the process may be in an unknown state; let a process manager restart it. Never swallow errors silently.

### বাংলা

**প্রশ্ন:** Node.js-এ error কীভাবে সামলান? `uncaughtException` ও `unhandledRejection` কী?

**উত্তর:** Operational error (প্রত্যাশিত runtime সমস্যা, যেমন network call ব্যর্থ, ভুল input বা নেই এমন file, যা আপনি সামলে নেন) আর programmer error (bug, যেমন `undefined`-এর property পড়া, যা ঠিক করতে হয়) আলাদা করুন। `async/await`-এর সাথে `try/catch`, Promise-এর সাথে `.catch()`, এবং callback-এ error-first argument ব্যবহার করুন; message ও status code সহ `Error` object (বা custom subclass) throw করুন। Node.js 15 থেকে unhandled promise rejection ডিফল্টভাবে process ভেঙে দেয়। `process.on("uncaughtException")` ও `process.on("unhandledRejection")` শুধু log করে সুন্দরভাবে বন্ধ করতে ব্যবহার করুন, কারণ process অজানা অবস্থায় থাকতে পারে; process manager দিয়ে আবার চালু করান। কখনো error চুপচাপ গিলে ফেলবেন না।

### Example

```js
class AppError extends Error {
  constructor(message, statusCode = 500, isOperational = true) {
    super(message);
    this.name = "AppError";
    this.statusCode = statusCode;
    this.isOperational = isOperational;
  }
}

async function getUser(id) {
  const user = await db.user.findUnique({ where: { id } });
  if (!user) throw new AppError("User not found", 404);
  return user;
}

try {
  await getUser(99);
} catch (err) {
  if (err instanceof AppError && err.isOperational) console.warn(err.message);
  else throw err; // programmer error: let it surface
}

process.on("unhandledRejection", (reason) => {
  console.error("Unhandled rejection:", reason);
  process.exit(1); // restart via PM2 / container
});
process.on("uncaughtException", (err) => {
  console.error("Uncaught exception:", err);
  process.exit(1);
});
```

---

## Q15. Promisify and async patterns

### English

**Question:** How do you convert callback-based APIs to Promises? What async helpers does Node.js provide?

**Answer:** Use `util.promisify(fn)` for functions that follow the error-first callback convention, or wrap manually with `new Promise((resolve, reject) => ...)`. Many modules already ship promise versions: `fs/promises`, `timers/promises`, `stream/promises`, `dns/promises`. Helpers: `events.once(emitter, "event")` awaits an event, `for await...of` iterates over async iterables (such as streams), and `AbortController` cancels operations (fetch, timers, streams). For parallel work use `Promise.all`; avoid `await` inside a loop when tasks are independent.

### বাংলা

**প্রশ্ন:** Callback-ভিত্তিক API-কে Promise-এ কীভাবে রূপান্তর করেন? Node.js কী কী async সহায়ক দেয়?

**উত্তর:** Error-first callback নিয়ম মানা function-এর জন্য `util.promisify(fn)` ব্যবহার করুন, অথবা নিজে `new Promise((resolve, reject) => ...)` দিয়ে মুড়ুন। অনেক module-এ আগে থেকেই promise সংস্করণ আছে: `fs/promises`, `timers/promises`, `stream/promises`, `dns/promises`। সহায়ক: `events.once(emitter, "event")` কোনো event-এর জন্য অপেক্ষা করে, `for await...of` async iterable (যেমন stream) ঘোরায়, এবং `AbortController` কাজ বাতিল করে (fetch, timer, stream)। Parallel কাজে `Promise.all` ব্যবহার করুন; কাজগুলো স্বাধীন হলে loop-এর ভেতরে `await` এড়িয়ে চলুন।

### Example

```js
import { promisify } from "node:util";
import { setTimeout as sleep } from "node:timers/promises";
import { once, EventEmitter } from "node:events";
import fs from "node:fs";

const readFileAsync = promisify(fs.readFile);
const text = await readFileAsync("a.txt", "utf8");

// Manual wrapper
const legacyApi = (id) =>
  new Promise((resolve, reject) =>
    oldCallbackApi(id, (err, result) => (err ? reject(err) : resolve(result))),
  );

await sleep(1000); // promise-based timer

const emitter = new EventEmitter();
setTimeout(() => emitter.emit("ready", 42), 100);
const [value] = await once(emitter, "ready");

// Parallel vs sequential
const [a, b] = await Promise.all([fetchUser(1), fetchPosts(1)]);

const controller = new AbortController();
setTimeout(() => controller.abort(), 3000);
const res = await fetch("https://api.example.com", {
  signal: controller.signal,
});
```
