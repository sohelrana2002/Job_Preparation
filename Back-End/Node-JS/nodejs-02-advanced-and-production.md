# Node.js Advanced and Production (Q16 - Q30)

> Place in: `03-backend/nodejs/02-advanced-and-production.md`
> Core and event loop questions (Q1-Q15) are in `01-core-and-event-loop.md`.
> Version note: as of this writing, Node.js 24 is Active LTS and Node.js 26 is scheduled to enter LTS in October 2026 (dates can change; check nodejs.org/en/about/previous-releases).

---

## Q16. `cluster` vs `worker_threads` vs `child_process`

### English

**Question:** What is the difference between the `cluster`, `worker_threads` and `child_process` modules?

**Answer:**

- `child_process` (`spawn`, `exec`, `execFile`, `fork`) starts a separate OS process, to run shell commands, other programs or another Node.js script, communicating through pipes or IPC messages.
- `cluster` forks multiple Node.js processes of the same app that share one server port, so you can use all CPU cores for handling more requests; each worker has its own memory.
- `worker_threads` runs JavaScript in separate threads inside one process, ideal for CPU-heavy tasks; threads can share memory (`SharedArrayBuffer`) and exchange messages.

In production, scaling is often done with PM2 cluster mode or by running multiple containers behind a load balancer instead of writing cluster code manually.

### বাংলা

**প্রশ্ন:** `cluster`, `worker_threads` ও `child_process` module-এর পার্থক্য কী?

**উত্তর:**

- `child_process` (`spawn`, `exec`, `execFile`, `fork`) আলাদা একটি OS process চালু করে, shell command, অন্য program বা অন্য Node.js script চালাতে; pipe বা IPC message দিয়ে যোগাযোগ হয়।
- `cluster` একই app-এর একাধিক Node.js process fork করে যারা একটি server port শেয়ার করে, ফলে সব CPU core কাজে লাগিয়ে বেশি request সামলানো যায়; প্রতিটি worker-এর নিজস্ব memory।
- `worker_threads` একটি process-এর ভেতরে আলাদা thread-এ JavaScript চালায়, ভারী CPU কাজের জন্য আদর্শ; thread গুলো memory শেয়ার (`SharedArrayBuffer`) ও message আদান-প্রদান করতে পারে।

Production-এ নিজে cluster code না লিখে সাধারণত PM2 cluster mode বা load balancer-এর পেছনে একাধিক container চালিয়ে scale করা হয়।

### Example

```js
// cluster: one worker per CPU core
import cluster from "node:cluster";
import os from "node:os";
import http from "node:http";

if (cluster.isPrimary) {
  for (let i = 0; i < os.availableParallelism(); i++) cluster.fork();
  cluster.on("exit", (worker) => {
    console.log(`Worker ${worker.process.pid} died, restarting`);
    cluster.fork();
  });
} else {
  http
    .createServer((req, res) => res.end(`Handled by ${process.pid}`))
    .listen(3000);
}

// child_process: run a command
import { execFile } from "node:child_process";
execFile("node", ["--version"], (err, stdout) => console.log(stdout));
```

---

## Q17. CPU-intensive tasks

### English

**Question:** What happens if you run a CPU-intensive task in Node.js? How do you handle it?

**Answer:** Because JavaScript runs on one main thread, a long synchronous computation (large loops, image processing, heavy JSON parsing, password hashing with sync APIs) blocks the event loop, so every other request waits and the server appears frozen. Solutions: move the work to a `worker_threads` pool; use a separate process or service with a job queue (BullMQ, RabbitMQ) and process in the background; split work into small chunks yielding with `setImmediate`; use asynchronous library versions (which use the thread pool); use a native addon or another language for extreme cases; and improve the algorithm.

### বাংলা

**প্রশ্ন:** Node.js-এ CPU-নির্ভর ভারী কাজ চালালে কী হয়? কীভাবে সামলান?

**উত্তর:** JavaScript একটি মূল thread-এ চলে, তাই দীর্ঘ synchronous হিসাব (বড় loop, ছবি প্রক্রিয়া, ভারী JSON parsing, sync API-তে password hash) event loop আটকে দেয়; ফলে অন্য সব request অপেক্ষা করে এবং server জমে গেছে মনে হয়। সমাধান: কাজ `worker_threads` pool-এ সরান; আলাদা process বা সেবায় job queue (BullMQ, RabbitMQ) দিয়ে background-এ চালান; কাজ ছোট টুকরোয় ভেঙে `setImmediate` দিয়ে ছাড় দিন; library-র asynchronous সংস্করণ ব্যবহার করুন (যা thread pool ব্যবহার করে); চরম ক্ষেত্রে native addon বা অন্য ভাষা ব্যবহার করুন; এবং algorithm উন্নত করুন।

### Example

```js
// main.js
import { Worker } from "node:worker_threads";

function runInWorker(n) {
  return new Promise((resolve, reject) => {
    const worker = new Worker(new URL("./fib-worker.js", import.meta.url), {
      workerData: n,
    });
    worker.once("message", resolve);
    worker.once("error", reject);
  });
}

console.log(await runInWorker(42)); // main thread stays free

// fib-worker.js
import { parentPort, workerData } from "node:worker_threads";
const fib = (n) => (n < 2 ? n : fib(n - 1) + fib(n - 2));
parentPort.postMessage(fib(workerData));
```

---

## Q18. Memory leaks and the heap

### English

**Question:** What causes memory leaks in Node.js and how do you find them?

**Answer:** The V8 garbage collector frees objects that are no longer reachable. A leak happens when objects stay reachable but are no longer needed. Common causes: unbounded in-memory caches or global arrays/maps, event listeners that are never removed, timers (`setInterval`) never cleared, closures that retain large data, and unclosed resources (connections, streams). To find them: monitor `process.memoryUsage()` (a `heapUsed` that keeps growing), run with `node --inspect` and take heap snapshots in Chrome DevTools to compare, or use tools like `clinic.js`. You can raise the heap limit with `--max-old-space-size=4096`, but that only delays a leak. Fix with bounded caches (LRU), cleanup of listeners and timers, and `WeakMap`/`WeakRef` where suitable.

### বাংলা

**প্রশ্ন:** Node.js-এ memory leak কেন হয় এবং কীভাবে খুঁজে বের করেন?

**উত্তর:** V8 garbage collector যেসব object আর পৌঁছানো যায় না সেগুলো মুক্ত করে। Leak হয় যখন object আর দরকার নেই কিন্তু এখনো পৌঁছানো যাচ্ছে। সাধারণ কারণ: সীমাহীন in-memory cache বা global array/map, না সরানো event listener, না থামানো timer (`setInterval`), বড় data ধরে রাখা closure, এবং বন্ধ না করা resource (connection, stream)। খুঁজতে: `process.memoryUsage()` নজরে রাখুন (`heapUsed` ক্রমাগত বাড়ছে কিনা), `node --inspect` দিয়ে চালিয়ে Chrome DevTools-এ heap snapshot নিয়ে তুলনা করুন, বা `clinic.js`-এর মতো tool ব্যবহার করুন। `--max-old-space-size=4096` দিয়ে heap-এর সীমা বাড়ানো যায়, কিন্তু তাতে leak শুধু পিছিয়ে যায়। সমাধান: সীমিত cache (LRU), listener ও timer পরিষ্কার করা, এবং উপযুক্ত জায়গায় `WeakMap`/`WeakRef`।

### Example

```js
// Leak: cache grows forever
const cache = new Map();
app.get("/user/:id", async (req, res) => {
  if (!cache.has(req.params.id))
    cache.set(req.params.id, await loadUser(req.params.id));
  res.json(cache.get(req.params.id));
});

// Fix: bounded cache with a simple size limit
const MAX = 1000;
function setCache(key, value) {
  if (cache.size >= MAX) cache.delete(cache.keys().next().value); // evict oldest
  cache.set(key, value);
}

setInterval(() => {
  const mb = process.memoryUsage().heapUsed / 1024 / 1024;
  console.log(`heap: ${mb.toFixed(1)} MB`);
}, 10_000).unref();
```

```bash
node --inspect server.js          # then open chrome://inspect -> Memory -> heap snapshot
node --max-old-space-size=2048 server.js
```

---

## Q19. Environment configuration and `NODE_ENV`

### English

**Question:** How do you manage configuration and environment variables in Node.js? What does `NODE_ENV` do?

**Answer:** Follow the twelve-factor principle: keep configuration (database URLs, API keys, ports) in environment variables, not in code. Read them with `process.env` (always strings). Load a `.env` file with the `dotenv` package or, since Node.js 20.6, with the built-in `node --env-file=.env`. Never commit `.env`; commit a `.env.example`. Validate required variables at startup (for example with Zod) so the app fails fast. `NODE_ENV` (`development`, `test`, `production`) is a convention read by many libraries; for example, Express enables caching and fewer debug details in `production`. Use a secret manager for production secrets.

### বাংলা

**প্রশ্ন:** Node.js-এ configuration ও environment variable কীভাবে সামলান? `NODE_ENV` কী করে?

**উত্তর:** Twelve-factor নীতি মানুন: configuration (database URL, API key, port) code-এ নয়, environment variable-এ রাখুন। `process.env` দিয়ে পড়ুন (সবসময় string)। `.env` file `dotenv` package দিয়ে বা Node.js 20.6 থেকে বিল্ট-ইন `node --env-file=.env` দিয়ে লোড করুন। `.env` কখনো commit করবেন না; `.env.example` commit করুন। চালু হওয়ার সময়ই প্রয়োজনীয় variable যাচাই করুন (যেমন Zod দিয়ে), যাতে app দ্রুত ব্যর্থ হয়। `NODE_ENV` (`development`, `test`, `production`) একটি প্রচলিত নিয়ম যা অনেক library পড়ে; যেমন Express `production`-এ caching চালু করে এবং debug তথ্য কম দেয়। Production-এর secret-এর জন্য secret manager ব্যবহার করুন।

### Example

```bash
# .env.example
PORT=3000
DATABASE_URL=postgres://user:pass@localhost:5432/app
JWT_SECRET=change-me
```

```bash
node --env-file=.env src/index.js
NODE_ENV=production node src/index.js
```

```js
import { z } from "zod";

const env = z
  .object({
    PORT: z.coerce.number().default(3000),
    DATABASE_URL: z.string().url(),
    JWT_SECRET: z.string().min(16),
    NODE_ENV: z
      .enum(["development", "test", "production"])
      .default("development"),
  })
  .parse(process.env); // throws immediately if something is missing

export default env;
```

---

## Q20. Security best practices

### English

**Question:** What are the key security best practices for a Node.js backend?

**Answer:**

- Validate and sanitize all input (Zod, Joi) and limit body size.
- Prevent injection: use parameterized queries or an ORM; guard against NoSQL operator injection (`{ "$ne": null }`); never pass user input to `eval`, `exec` or `child_process` shells (use `execFile` with arguments).
- Hash passwords with bcrypt, scrypt or argon2; never store plain text.
- Use HTTPS, secure headers (`helmet`), a strict CORS policy, rate limiting and `HttpOnly`/`Secure`/`SameSite` cookies.
- Prevent path traversal and avoid exposing stack traces to clients.
- Keep secrets out of code and Git, run `npm audit`, keep dependencies and Node.js updated, and use a lock file.
- Apply least privilege to database users and run containers as non-root.

### বাংলা

**প্রশ্ন:** Node.js backend-এর প্রধান নিরাপত্তা অভ্যাসগুলো কী?

**উত্তর:**

- সব input যাচাই ও পরিষ্কার করুন (Zod, Joi) এবং body-র আকার সীমিত করুন।
- Injection ঠেকান: parameterized query বা ORM ব্যবহার করুন; NoSQL operator injection (`{ "$ne": null }`) থেকে সাবধান থাকুন; ব্যবহারকারীর input কখনো `eval`, `exec` বা `child_process` shell-এ দেবেন না (argument সহ `execFile` ব্যবহার করুন)।
- Password bcrypt, scrypt বা argon2 দিয়ে hash করুন; কখনো plain text রাখবেন না।
- HTTPS, নিরাপদ header (`helmet`), কঠোর CORS নীতি, rate limiting এবং `HttpOnly`/`Secure`/`SameSite` cookie ব্যবহার করুন।
- Path traversal ঠেকান এবং client-কে stack trace দেখাবেন না।
- Secret code ও Git-এর বাইরে রাখুন, `npm audit` চালান, dependency ও Node.js হালনাগাদ রাখুন এবং lock file ব্যবহার করুন।
- Database user-কে ন্যূনতম অনুমতি দিন এবং container non-root হিসেবে চালান।

### Example

```js
import { execFile } from "node:child_process";
import { exec } from "node:child_process";

// BAD: command injection ( filename = "a.txt; rm -rf /" )
exec(`cat ${filename}`);
// GOOD: arguments are not interpreted by a shell
execFile("cat", [filename]);

// BAD: NoSQL injection ( body.password = { "$ne": null } )
await User.findOne({ email: body.email, password: body.password });
// GOOD: validate the type first
const { email, password } = z
  .object({ email: z.string().email(), password: z.string() })
  .parse(body);

// Express hardening
// app.use(helmet());
// app.use(express.json({ limit: "10kb" }));
// app.use(rateLimit({ windowMs: 15 * 60 * 1000, limit: 100 }));
```

---

## Q21. The `crypto` module and password hashing

### English

**Question:** How do you hash passwords and generate secure random values in Node.js?

**Answer:** The built-in `crypto` module provides hashing (`createHash`), HMAC (`createHmac`), secure random values (`randomBytes`, `randomUUID`, `randomInt`) and key-derivation functions (`scrypt`, `pbkdf2`). A fast hash such as SHA-256 is not suitable for passwords because it is easy to brute force; use a slow, salted algorithm: `crypto.scrypt`, or the `bcrypt`/`argon2` libraries, with a unique random salt per password. Compare secrets with `crypto.timingSafeEqual` to avoid timing attacks. Use HMAC to verify webhook signatures. Never use `Math.random()` for tokens or IDs that must be unguessable.

### বাংলা

**প্রশ্ন:** Node.js-এ password hash ও নিরাপদ random মান কীভাবে বানান?

**উত্তর:** বিল্ট-ইন `crypto` module hashing (`createHash`), HMAC (`createHmac`), নিরাপদ random মান (`randomBytes`, `randomUUID`, `randomInt`) এবং key-derivation function (`scrypt`, `pbkdf2`) দেয়। SHA-256-এর মতো দ্রুত hash password-এর জন্য উপযুক্ত নয়, কারণ brute force করা সহজ; ধীর ও salt-যুক্ত algorithm ব্যবহার করুন: `crypto.scrypt`, অথবা `bcrypt`/`argon2` library, প্রতিটি password-এর জন্য আলাদা random salt সহ। Timing attack এড়াতে secret তুলনায় `crypto.timingSafeEqual` ব্যবহার করুন। Webhook signature যাচাইয়ে HMAC ব্যবহার করুন। যে token বা ID অনুমান-অযোগ্য হতে হবে তার জন্য কখনো `Math.random()` ব্যবহার করবেন না।

### Example

```js
import crypto from "node:crypto";
import { promisify } from "node:util";

const scrypt = promisify(crypto.scrypt);

export async function hashPassword(password) {
  const salt = crypto.randomBytes(16);
  const key = await scrypt(password, salt, 64);
  return `${salt.toString("hex")}:${key.toString("hex")}`;
}

export async function verifyPassword(password, stored) {
  const [saltHex, keyHex] = stored.split(":");
  const key = await scrypt(password, Buffer.from(saltHex, "hex"), 64);
  return crypto.timingSafeEqual(key, Buffer.from(keyHex, "hex"));
}

const token = crypto.randomBytes(32).toString("hex");
const id = crypto.randomUUID();

const signature = crypto
  .createHmac("sha256", process.env.WEBHOOK_SECRET)
  .update(rawBody)
  .digest("hex");
```

---

## Q22. Graceful shutdown

### English

**Question:** What is a graceful shutdown and how do you implement it?

**Answer:** A graceful shutdown lets the app finish in-flight work before exiting, instead of dropping requests when it is stopped or redeployed. Listen for `SIGTERM` (sent by Docker, Kubernetes and PM2) and `SIGINT` (Ctrl+C); stop accepting new connections with `server.close()`; let current requests finish; close database and Redis connections and flush logs; then call `process.exit(0)`. Add a timeout as a safety net in case something hangs, and make the health check fail during shutdown so the load balancer stops sending traffic.

### বাংলা

**প্রশ্ন:** Graceful shutdown কী এবং কীভাবে বানান?

**উত্তর:** Graceful shutdown মানে app বন্ধ বা নতুন করে deploy হওয়ার সময় request ফেলে না দিয়ে চলমান কাজ শেষ করে তারপর বের হওয়া। `SIGTERM` (Docker, Kubernetes ও PM2 পাঠায়) এবং `SIGINT` (Ctrl+C) শুনুন; `server.close()` দিয়ে নতুন connection নেওয়া বন্ধ করুন; চলমান request শেষ হতে দিন; database ও Redis connection বন্ধ করুন ও log flush করুন; তারপর `process.exit(0)` call করুন। কিছু আটকে গেলে নিরাপত্তার জন্য একটি timeout রাখুন, এবং shutdown চলাকালে health check ব্যর্থ করুন যাতে load balancer নতুন traffic পাঠানো বন্ধ করে।

### Example

```js
import http from "node:http";

const server = http.createServer(app);
server.listen(3000);

let shuttingDown = false;

async function shutdown(signal) {
  if (shuttingDown) return;
  shuttingDown = true;
  console.log(`${signal} received, shutting down...`);

  const forceExit = setTimeout(() => process.exit(1), 10_000); // safety net
  forceExit.unref();

  server.close(async (err) => {
    // stops new connections
    try {
      await db.$disconnect();
      await redis.quit();
    } finally {
      process.exit(err ? 1 : 0);
    }
  });
}

process.on("SIGTERM", () => shutdown("SIGTERM"));
process.on("SIGINT", () => shutdown("SIGINT"));
```

---

## Q23. Logging, monitoring, PM2 and health checks

### English

**Question:** How do you handle logging and monitoring in production? What is PM2?

**Answer:** Use a structured JSON logger (`pino` is very fast; `winston` is flexible) with levels (`error`, `warn`, `info`, `debug`), a request ID for tracing, and never log secrets or passwords. Send logs to a central system (ELK, Loki, CloudWatch). Add a `/health` endpoint for load balancers and uptime checks, track metrics (latency, error rate, memory, event loop lag) with Prometheus or an APM tool, and report errors with Sentry. PM2 is a production process manager: it restarts crashed apps, supports cluster mode, manages logs, starts apps on server boot, and gives zero-downtime reloads (`pm2 reload`). In containers, the orchestrator usually does this job instead.

### বাংলা

**প্রশ্ন:** Production-এ logging ও monitoring কীভাবে সামলান? PM2 কী?

**উত্তর:** Structured JSON logger ব্যবহার করুন (`pino` খুব দ্রুত; `winston` নমনীয়), level সহ (`error`, `warn`, `info`, `debug`), trace করার জন্য request ID সহ, এবং secret বা password কখনো log করবেন না। Log কেন্দ্রীয় ব্যবস্থায় পাঠান (ELK, Loki, CloudWatch)। Load balancer ও uptime যাচাইয়ের জন্য `/health` endpoint রাখুন, Prometheus বা APM tool দিয়ে metric (latency, error rate, memory, event loop lag) ট্র্যাক করুন এবং Sentry দিয়ে error রিপোর্ট করুন। PM2 একটি production process manager: crash করা app আবার চালু করে, cluster mode সমর্থন করে, log সামলায়, server boot-এ app চালু করে এবং zero-downtime reload (`pm2 reload`) দেয়। Container-এ সাধারণত orchestrator এই কাজ করে।

### Example

```js
import pino from "pino";
import { randomUUID } from "node:crypto";

export const logger = pino({
  level: process.env.LOG_LEVEL ?? "info",
  redact: ["req.headers.authorization", "password"], // hide secrets
});

// request logging with an id
app.use((req, res, next) => {
  req.log = logger.child({ reqId: randomUUID() });
  req.log.info({ method: req.method, url: req.url }, "incoming request");
  next();
});

app.get("/health", (req, res) =>
  res.json({ status: "ok", uptime: process.uptime() }),
);
```

```js
// ecosystem.config.cjs
module.exports = {
  apps: [
    {
      name: "api",
      script: "src/index.js",
      instances: "max",
      exec_mode: "cluster",
      env_production: { NODE_ENV: "production" },
    },
  ],
};
// pm2 start ecosystem.config.cjs --env production
```

---

## Q24. Performance optimization

### English

**Question:** How do you improve the performance of a Node.js application?

**Answer:**

- Never block the event loop: avoid sync APIs and heavy CPU work in request handlers.
- Run independent async tasks in parallel with `Promise.all`.
- Cache expensive results (Redis or in-memory with a TTL) and use HTTP caching headers.
- Optimize the database: indexes, pagination, selecting only needed fields, avoiding N+1 queries, and connection pooling.
- Use streams for large payloads, enable gzip/brotli compression, and keep connections alive.
- Scale horizontally with cluster mode or multiple instances behind a load balancer or reverse proxy (Nginx).
- Measure first with profilers (`node --inspect`, `--cpu-prof`, `clinic.js`) and load testing tools (autocannon, k6) instead of guessing.

### বাংলা

**প্রশ্ন:** Node.js application-এর performance কীভাবে বাড়ান?

**উত্তর:**

- Event loop কখনো আটকাবেন না: request handler-এ sync API ও ভারী CPU কাজ এড়ান।
- স্বাধীন async কাজ `Promise.all` দিয়ে parallel চালান।
- ব্যয়বহুল ফলাফল cache করুন (Redis বা TTL সহ in-memory) এবং HTTP caching header ব্যবহার করুন।
- Database optimize করুন: index, pagination, শুধু দরকারি field আনা, N+1 query এড়ানো এবং connection pooling।
- বড় payload-এ stream ব্যবহার করুন, gzip/brotli compression চালু করুন এবং connection চালু (keep-alive) রাখুন।
- Cluster mode বা load balancer/reverse proxy (Nginx)-এর পেছনে একাধিক instance দিয়ে অনুভূমিকভাবে scale করুন।
- অনুমান না করে আগে profiler (`node --inspect`, `--cpu-prof`, `clinic.js`) ও load test tool (autocannon, k6) দিয়ে মাপুন।

### Example

```js
// Parallel instead of sequential
const [user, orders, stats] = await Promise.all([
  getUser(id),
  getOrders(id),
  getStats(id),
]);

// Simple in-memory cache with TTL
const cache = new Map();
async function cached(key, ttlMs, loader) {
  const hit = cache.get(key);
  if (hit && hit.expires > Date.now()) return hit.value;
  const value = await loader();
  cache.set(key, { value, expires: Date.now() + ttlMs });
  return value;
}
const products = await cached("products", 60_000, () => db.product.findMany());
```

```bash
npx autocannon -c 100 -d 20 http://localhost:3000/api/products
node --cpu-prof server.js
```

---

## Q25. Testing in Node.js

### English

**Question:** How do you test a Node.js application? Which tools are common?

**Answer:** Tests are usually split into unit tests (a single function or module in isolation, with dependencies mocked), integration tests (several parts together, such as an API endpoint with a test database) and end-to-end tests. Common tools: the built-in `node:test` runner with `node:assert` (no extra dependency, run with `node --test`), Jest or Vitest (richer mocking, snapshots, coverage), and `supertest` to test HTTP endpoints without opening a real port. Good practice: keep tests independent, use a separate test database, mock external services, aim for meaningful coverage of business logic, and run tests in CI on every push.

### বাংলা

**প্রশ্ন:** Node.js application কীভাবে test করেন? কোন tool গুলো প্রচলিত?

**উত্তর:** Test সাধারণত ভাগ করা হয় unit test (একটি function বা module আলাদাভাবে, dependency mock করে), integration test (একাধিক অংশ একসাথে, যেমন test database সহ API endpoint) এবং end-to-end test-এ। প্রচলিত tool: `node:assert` সহ বিল্ট-ইন `node:test` runner (বাড়তি dependency লাগে না, `node --test` দিয়ে চলে), Jest বা Vitest (উন্নত mocking, snapshot, coverage), এবং আসল port না খুলে HTTP endpoint test করতে `supertest`। ভালো অভ্যাস: test গুলো স্বাধীন রাখুন, আলাদা test database ব্যবহার করুন, বাইরের সেবা mock করুন, business logic-এর অর্থপূর্ণ coverage রাখুন এবং প্রতিটি push-এ CI-তে test চালান।

### Example

```js
// math.test.js  ->  run with: node --test
import { describe, it, mock } from "node:test";
import assert from "node:assert/strict";
import { add, getUserName } from "./math.js";

describe("add", () => {
  it("adds two numbers", () => {
    assert.equal(add(2, 3), 5);
  });
  it("throws on invalid input", () => {
    assert.throws(() => add("a", 1), /Invalid/);
  });
});

it("returns the user name (mocked repo)", async () => {
  const repo = { find: mock.fn(async () => ({ name: "Sohel" })) };
  assert.equal(await getUserName(repo, 1), "Sohel");
  assert.equal(repo.find.mock.callCount(), 1);
});
```

```js
// API test with supertest (Express/other app exported without .listen)
import request from "supertest";
import app from "./app.js";

it("GET /health returns 200", async () => {
  const res = await request(app).get("/health");
  assert.equal(res.status, 200);
});
```

---

## Q26. WebSockets and real-time communication

### English

**Question:** How do you build real-time features in Node.js? What is the difference between HTTP, WebSockets and Server-Sent Events?

**Answer:** Plain HTTP is request/response: the client always starts the exchange. WebSockets keep one persistent, full-duplex connection so both client and server can send messages at any time with low overhead, good for chat, live notifications, collaborative editing and games. Server-Sent Events (SSE) are a simpler one-way stream from server to client over HTTP, good for live feeds. In Node.js, use the `ws` library for raw WebSockets or Socket.IO, which adds rooms, automatic reconnection, acknowledgements and fallbacks. To scale to multiple server instances, you need a pub/sub backend (the Socket.IO Redis adapter) and sticky sessions or a shared message bus. Authenticate the connection during the handshake.

### বাংলা

**প্রশ্ন:** Node.js-এ real-time feature কীভাবে বানান? HTTP, WebSocket ও Server-Sent Events-এর পার্থক্য কী?

**উত্তর:** সাধারণ HTTP হলো request/response: আদান-প্রদান সবসময় client শুরু করে। WebSocket একটি স্থায়ী full-duplex সংযোগ রাখে, ফলে client ও server যেকোনো সময় কম খরচে message পাঠাতে পারে; chat, live notification, একসাথে সম্পাদনা ও game-এর জন্য ভালো। Server-Sent Events (SSE) HTTP-র উপর server থেকে client-এ সহজ একমুখী stream, live feed-এর জন্য ভালো। Node.js-এ কাঁচা WebSocket-এর জন্য `ws` library বা Socket.IO ব্যবহার করুন, যা room, স্বয়ংক্রিয় reconnect, acknowledgement ও fallback যোগ করে। একাধিক server instance-এ scale করতে pub/sub backend (Socket.IO-র Redis adapter) এবং sticky session বা shared message bus লাগে। Handshake-এর সময়ই সংযোগ authenticate করুন।

### Example

```js
// Socket.IO server
import { createServer } from "node:http";
import { Server } from "socket.io";

const httpServer = createServer();
const io = new Server(httpServer, { cors: { origin: "https://myapp.com" } });

io.use((socket, next) => {
  // authenticate on handshake
  const user = verifyToken(socket.handshake.auth.token);
  if (!user) return next(new Error("Unauthorized"));
  socket.data.user = user;
  next();
});

io.on("connection", (socket) => {
  socket.join("room-1");
  socket.on("chat:message", (text) => {
    io.to("room-1").emit("chat:message", { user: socket.data.user.name, text });
  });
});

httpServer.listen(3000);
```

---

## Q27. When to use (and not use) Node.js

### English

**Question:** When is Node.js a good choice for a backend, and when is it not?

**Answer:** Node.js is a good fit for I/O-bound applications with many concurrent connections: REST/GraphQL APIs, real-time apps, streaming, API gateways and backends-for-frontends, microservices, and tools where sharing JavaScript/TypeScript code with the frontend saves time. It is less suitable for heavy CPU-bound work (video encoding, large-scale number crunching, machine learning training) unless you offload that work to worker threads, queues or services in another language. Compared with Java/Spring or .NET, Node.js has a lighter runtime and faster iteration but fewer built-in enterprise conventions; compared with Python, it has stronger concurrency for I/O but a weaker data-science ecosystem. Choose based on the workload and the team's skills.

### বাংলা

**প্রশ্ন:** Backend-এর জন্য Node.js কখন ভালো পছন্দ এবং কখন নয়?

**উত্তর:** অনেক একযোগে সংযোগ থাকা I/O-নির্ভর application-এ Node.js ভালো মানায়: REST/GraphQL API, real-time app, streaming, API gateway ও backend-for-frontend, microservice, এবং যেখানে frontend-এর সাথে JavaScript/TypeScript code শেয়ার করে সময় বাঁচে। ভারী CPU-নির্ভর কাজে (video encoding, বড় মাপের সংখ্যা হিসাব, machine learning training) এটি কম উপযুক্ত, যদি না কাজটি worker thread, queue বা অন্য ভাষার সেবায় সরানো হয়। Java/Spring বা .NET-এর তুলনায় Node.js-এর runtime হালকা ও দ্রুত iteration সম্ভব, কিন্তু বিল্ট-ইন enterprise নিয়ম কম; Python-এর তুলনায় I/O concurrency-তে শক্তিশালী কিন্তু data-science ecosystem দুর্বল। কাজের ধরন ও দলের দক্ষতা অনুযায়ী বেছে নিন।

### Example

```
Good fit                           Poor fit (without offloading)
-----------------------------      ------------------------------------
REST / GraphQL API                 Video / image transcoding
Chat, notifications, live feeds    ML model training
API gateway / BFF                  Heavy scientific computing
Streaming, file upload/proxy       Long-running CPU loops per request
```

---

## Q28. What is new in modern Node.js

### English

**Question:** What modern features does recent Node.js include that used to require third-party packages?

**Answer:** Notable built-ins in recent LTS versions: global `fetch`, `AbortController`, `WebSocket` client and `structuredClone`; the `node:test` test runner with mocking and coverage; `node --watch` for auto-restart (replacing nodemon in many cases); `node --env-file` for `.env` files (replacing dotenv); the `util.parseArgs` CLI parser; ES module improvements such as `import.meta.dirname`/`import.meta.filename` and `require()` of ES modules; an experimental permission model (`--permission`) to restrict file/network access; an experimental built-in `node:sqlite`; and running TypeScript files directly through type stripping in newer versions (check the docs for stability in your version). Release policy: even-numbered majors become LTS; from Node.js 27, there is one major per year and every release becomes LTS.

### বাংলা

**প্রশ্ন:** সাম্প্রতিক Node.js-এ কোন আধুনিক feature আছে যার জন্য আগে তৃতীয় পক্ষের package লাগত?

**উত্তর:** সাম্প্রতিক LTS সংস্করণের উল্লেখযোগ্য বিল্ট-ইন: global `fetch`, `AbortController`, `WebSocket` client ও `structuredClone`; mocking ও coverage সহ `node:test` test runner; auto-restart-এর জন্য `node --watch` (অনেক ক্ষেত্রে nodemon-এর বদলে); `.env` file-এর জন্য `node --env-file` (dotenv-এর বদলে); `util.parseArgs` CLI parser; ES module-এর উন্নতি যেমন `import.meta.dirname`/`import.meta.filename` ও ES module `require()` করা; file/network access সীমিত করার পরীক্ষামূলক permission model (`--permission`); পরীক্ষামূলক বিল্ট-ইন `node:sqlite`; এবং নতুন সংস্করণে type stripping দিয়ে সরাসরি TypeScript file চালানো (আপনার সংস্করণে স্থিতিশীলতা docs-এ দেখে নিন)। Release নীতি: জোড় সংখ্যার major LTS হয়; Node.js 27 থেকে বছরে একটি major এবং প্রতিটি release LTS হবে।

### Example

```bash
node --watch src/index.js                 # restarts on file changes
node --env-file=.env src/index.js         # loads .env without dotenv
node --test                               # built-in test runner
node --permission --allow-fs-read=./data app.js   # restrict file access (experimental)
```

```js
const res = await fetch("https://api.github.com/repos/nodejs/node"); // no axios needed
const data = await res.json();

import { parseArgs } from "node:util";
const { values } = parseArgs({
  options: { port: { type: "string", short: "p" } },
});
```

---

## Q29. Concurrency control (promise pool)

### English

**Question:** How do you run many async tasks with a limited concurrency (for example, 1000 API calls, 5 at a time)?

**Answer:** Running `Promise.all` on thousands of tasks starts them all at once, which can exhaust sockets, memory or hit rate limits. Instead use a promise pool: keep at most N tasks running and start the next one as soon as one finishes. You can use the `p-limit` library or write a small worker-pool function where N workers pull items from a shared queue. For failures, decide between failing fast or collecting results with `Promise.allSettled`, and add retry with backoff for transient errors.

### বাংলা

**প্রশ্ন:** অনেক async কাজ সীমিত concurrency-তে (যেমন ১০০০টি API call, একসাথে ৫টি) কীভাবে চালান?

**উত্তর:** হাজার হাজার কাজে `Promise.all` চালালে সব একসাথে শুরু হয়, ফলে socket বা memory শেষ হতে পারে অথবা rate limit লেগে যায়। এর বদলে promise pool ব্যবহার করুন: একসাথে সর্বোচ্চ N টি কাজ চলবে এবং একটি শেষ হওয়া মাত্র পরেরটি শুরু হবে। `p-limit` library ব্যবহার করতে পারেন, অথবা নিজে ছোট worker-pool function লিখতে পারেন, যেখানে N টি worker একটি শেয়ার্ড queue থেকে কাজ নেয়। ব্যর্থতার ক্ষেত্রে দ্রুত থামা না `Promise.allSettled` দিয়ে সব ফলাফল সংগ্রহ করা ঠিক করুন, এবং সাময়িক error-এর জন্য backoff সহ retry যোগ করুন।

### Example

```js
async function promisePool(items, limit, worker) {
  const results = new Array(items.length);
  let next = 0;

  async function run() {
    while (next < items.length) {
      const index = next++; // safe: single-threaded
      try {
        results[index] = {
          status: "fulfilled",
          value: await worker(items[index], index),
        };
      } catch (reason) {
        results[index] = { status: "rejected", reason };
      }
    }
  }

  await Promise.all(Array.from({ length: Math.min(limit, items.length) }, run));
  return results;
}

const urls = Array.from(
  { length: 100 },
  (_, i) => `https://api.example.com/items/${i}`,
);
const results = await promisePool(urls, 5, async (url) =>
  (await fetch(url)).json(),
);
console.log(results.filter((r) => r.status === "rejected").length, "failed");

// with p-limit
// import pLimit from "p-limit";
// const limit = pLimit(5);
// await Promise.all(urls.map((u) => limit(() => fetch(u))));
```

---

## Q30. Practical: processing large files with streams

### English

**Question:** How would you process a 5 GB log or CSV file in Node.js without running out of memory? How do you serve a large file over HTTP?

**Answer:** Never use `readFile` for huge files because it loads everything into memory. Create a read stream and process it line by line with the `readline` module (or a Transform stream), and write results to another stream using `stream.pipeline` so errors are handled and backpressure is respected. To serve a large file over HTTP, pipe `fs.createReadStream` into the response and set `Content-Type`/`Content-Length`; for downloads, support `Range` requests for resumable downloads and use `pipeline` so the file handle is closed if the client disconnects.

### বাংলা

**প্রশ্ন:** Memory শেষ না করে Node.js-এ ৫ GB-র log বা CSV file কীভাবে প্রক্রিয়া করবেন? বড় file HTTP-তে কীভাবে পরিবেশন করবেন?

**উত্তর:** বিশাল file-এর জন্য কখনো `readFile` ব্যবহার করবেন না, কারণ এটি সব memory-তে লোড করে। Read stream বানিয়ে `readline` module (বা Transform stream) দিয়ে লাইন ধরে ধরে প্রক্রিয়া করুন, এবং ফলাফল `stream.pipeline` দিয়ে অন্য stream-এ লিখুন, যাতে error সামলানো হয় ও backpressure মানা হয়। HTTP-তে বড় file পরিবেশন করতে `fs.createReadStream`-কে response-এ pipe করুন এবং `Content-Type`/`Content-Length` ঠিক করুন; download-এ পুনরায় শুরু করার জন্য `Range` request সমর্থন করুন এবং `pipeline` ব্যবহার করুন, যাতে client সংযোগ ছিন্ন করলে file handle বন্ধ হয়ে যায়।

### Example

```js
import { createReadStream, createWriteStream } from "node:fs";
import { stat } from "node:fs/promises";
import { pipeline } from "node:stream/promises";
import readline from "node:readline";
import http from "node:http";

// 1) Count ERROR lines in a huge log without loading it
async function countErrors(file) {
  const rl = readline.createInterface({
    input: createReadStream(file),
    crlfDelay: Infinity,
  });
  let count = 0;
  for await (const line of rl) if (line.includes("ERROR")) count++;
  return count;
}
console.log(await countErrors("app.log"));

// 2) Filter one large file into another (streaming)
import { Transform } from "node:stream";
const onlyErrors = new Transform({
  transform(chunk, enc, cb) {
    const lines = chunk
      .toString()
      .split("\n")
      .filter((l) => l.includes("ERROR"));
    cb(null, lines.length ? lines.join("\n") + "\n" : "");
  },
});
await pipeline(
  createReadStream("app.log"),
  onlyErrors,
  createWriteStream("errors.log"),
);

// 3) Serve a large file over HTTP
http
  .createServer(async (req, res) => {
    const file = "video.mp4";
    const { size } = await stat(file);
    res.writeHead(200, { "Content-Type": "video/mp4", "Content-Length": size });
    pipeline(createReadStream(file), res).catch(() => res.destroy());
  })
  .listen(3000);
```
