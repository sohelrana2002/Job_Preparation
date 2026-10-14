# Next.js Fundamentals and Rendering (Q1 - Q15)

> Place in: `02-frontend/nextjs/01-fundamentals-and-rendering.md`
> Data, caching and production questions (Q16-Q30) are in `02-data-caching-and-production.md`.
> Code uses the App Router with TypeScript. Version notes: Next.js 15 made `params`, `searchParams`, `cookies()` and `headers()` async; Next.js 16 added Cache Components (`"use cache"`) and replaced `middleware.ts` with `proxy.ts`.

## Top 30 Index

| #   | Topic                                        | File |
| --- | -------------------------------------------- | ---- |
| 1   | What is Next.js? Why use it over plain React | 01   |
| 2   | CSR, SSR, SSG and ISR                        | 01   |
| 3   | App Router vs Pages Router                   | 01   |
| 4   | File-based routing and special files         | 01   |
| 5   | Dynamic routes, catch-all and route groups   | 01   |
| 6   | Layouts and templates                        | 01   |
| 7   | Server vs Client Components in Next.js       | 01   |
| 8   | Data fetching in the App Router              | 01   |
| 9   | `generateStaticParams` and SSG               | 01   |
| 10  | ISR and `revalidate`                         | 01   |
| 11  | Streaming, `loading.tsx` and Suspense        | 01   |
| 12  | `Link`, `useRouter` and prefetching          | 01   |
| 13  | `next/image`                                 | 01   |
| 14  | Font and script optimization                 | 01   |
| 15  | Metadata and SEO                             | 01   |
| 16  | Route Handlers                               | 02   |
| 17  | Server Actions                               | 02   |
| 18  | Caching in Next.js                           | 02   |
| 19  | `revalidatePath` vs `revalidateTag`          | 02   |
| 20  | Proxy (middleware)                           | 02   |
| 21  | Authentication                               | 02   |
| 22  | Environment variables                        | 02   |
| 23  | Hydration and hydration errors               | 02   |
| 24  | Error handling                               | 02   |
| 25  | Performance optimization                     | 02   |
| 26  | Static vs dynamic rendering                  | 02   |
| 27  | Partial Prerendering and Cache Components    | 02   |
| 28  | Deployment                                   | 02   |
| 29  | Pages Router data fetching                   | 02   |
| 30  | What is new in Next.js 15 and 16             | 02   |

---

## Q1. What is Next.js? Why use it over plain React?

### English

**Question:** What is Next.js and why would you use it instead of plain React (for example Create React App or Vite)?

**Answer:** Next.js is a full-stack React framework. React is only a UI library, while Next.js adds file-based routing, multiple rendering modes (SSR, SSG, ISR), React Server Components, API endpoints (Route Handlers), Server Actions, image/font optimization, built-in SEO support, code splitting and a production build pipeline. Main benefits over a plain client-rendered React app: better SEO and faster first load because HTML is rendered on the server, less client JavaScript, and frontend and backend code in one project.

### বাংলা

**প্রশ্ন:** Next.js কী এবং সাধারণ React (যেমন Create React App বা Vite)-এর বদলে কেন ব্যবহার করবেন?

**উত্তর:** Next.js হলো একটি full-stack React framework। React শুধু UI library, আর Next.js যোগ করে file-based routing, একাধিক render পদ্ধতি (SSR, SSG, ISR), React Server Components, API endpoint (Route Handler), Server Actions, ছবি ও font optimization, বিল্ট-ইন SEO সমর্থন, code splitting এবং production build ব্যবস্থা। সাধারণ client-rendered React app-এর তুলনায় মূল সুবিধা: server-এ HTML তৈরি হয় বলে ভালো SEO ও দ্রুত প্রথম লোড, কম client JavaScript, এবং frontend ও backend code একই project-এ।

### Example

```bash
npx create-next-app@latest my-app
```

```tsx
// app/page.tsx  -> becomes the "/" route automatically
export default function HomePage() {
  return <h1>Hello from Next.js</h1>;
}
```

---

## Q2. CSR, SSR, SSG and ISR

### English

**Question:** Explain CSR, SSR, SSG and ISR. When would you use each?

**Answer:**

- **CSR (Client-Side Rendering):** the browser downloads JavaScript and renders the page; good for private dashboards, poor for SEO and first load.
- **SSR (Server-Side Rendering):** HTML is generated on the server for every request; good for personalized or frequently changing data that needs SEO.
- **SSG (Static Site Generation):** HTML is generated at build time and served from a CDN; fastest, ideal for blogs, docs and marketing pages.
- **ISR (Incremental Static Regeneration):** static pages that are regenerated in the background after a set time or on demand; good for product pages or news that change occasionally.

### বাংলা

**প্রশ্ন:** CSR, SSR, SSG ও ISR ব্যাখ্যা করুন। কোনটি কখন ব্যবহার করবেন?

**উত্তর:**

- **CSR (Client-Side Rendering):** browser JavaScript নামিয়ে পেজ render করে; private dashboard-এর জন্য ভালো, SEO ও প্রথম লোডের জন্য দুর্বল।
- **SSR (Server-Side Rendering):** প্রতিটি request-এ server-এ HTML তৈরি হয়; ব্যক্তিগত বা ঘনঘন বদলানো data যেখানে SEO-ও দরকার সেখানে ভালো।
- **SSG (Static Site Generation):** build-এর সময়ই HTML তৈরি হয় এবং CDN থেকে দেওয়া হয়; সবচেয়ে দ্রুত, blog, docs ও marketing পেজের জন্য আদর্শ।
- **ISR (Incremental Static Regeneration):** static পেজ, যা নির্দিষ্ট সময় পরে বা প্রয়োজনে background-এ আবার তৈরি হয়; মাঝে মাঝে বদলায় এমন product পেজ বা সংবাদের জন্য ভালো।

### Example

```tsx
// SSG: no dynamic data, built once
export default function About() {
  return <h1>About us</h1>;
}

// SSR: dynamic API (cookies) forces rendering on every request
import { cookies } from "next/headers";
export default async function Profile() {
  const token = (await cookies()).get("token");
  return <p>Logged in: {token ? "yes" : "no"}</p>;
}

// ISR: static, refreshed at most every 60 seconds
export const revalidate = 60;
export default async function News() {
  const news = await fetch("https://api.example.com/news").then((r) =>
    r.json(),
  );
  return (
    <ul>
      {news.map((n: any) => (
        <li key={n.id}>{n.title}</li>
      ))}
    </ul>
  );
}
```

---

## Q3. App Router vs Pages Router

### English

**Question:** What is the difference between the App Router and the Pages Router?

**Answer:** The Pages Router (`pages/`) is the original system: every file is a route, components are Client Components by default, and data is fetched with `getStaticProps`, `getServerSideProps` and `getStaticPaths`. The App Router (`app/`, introduced in Next.js 13) is built on React Server Components: components are Server Components by default, data is fetched directly with `async/await` in components, and it adds nested layouts, `loading.tsx`/`error.tsx`, streaming, Server Actions and Route Handlers. The App Router is the recommended choice for new projects; both can coexist during migration.

### বাংলা

**প্রশ্ন:** App Router আর Pages Router-এর পার্থক্য কী?

**উত্তর:** Pages Router (`pages/`) হলো আদি ব্যবস্থা: প্রতিটি file একটি route, component ডিফল্টভাবে Client Component, এবং data আনা হয় `getStaticProps`, `getServerSideProps` ও `getStaticPaths` দিয়ে। App Router (`app/`, Next.js 13-এ এসেছে) React Server Components-এর উপর তৈরি: component ডিফল্টভাবে Server Component, component-এর ভেতরেই `async/await` দিয়ে সরাসরি data আনা যায়, এবং এতে আছে nested layout, `loading.tsx`/`error.tsx`, streaming, Server Actions ও Route Handler। নতুন project-এর জন্য App Router-ই সুপারিশ করা হয়; migration-এর সময় দুটো একসাথে থাকতে পারে।

### Example

```
pages/                      app/
  index.tsx                   page.tsx
  about.tsx                   about/page.tsx
  posts/[id].tsx              posts/[id]/page.tsx
  api/users.ts                api/users/route.ts
  _app.tsx / _document.tsx    layout.tsx
```

---

## Q4. File-based routing and special files

### English

**Question:** How does routing work in the App Router? What are the special files?

**Answer:** Folders inside `app/` define URL segments, and a route is publicly accessible only when it has a `page.tsx` (or a `route.ts`). Special files: `layout.tsx` (shared UI that persists across navigation), `page.tsx` (route UI), `loading.tsx` (loading UI via Suspense), `error.tsx` (error boundary, must be a Client Component), `not-found.tsx` (404 UI), `template.tsx` (like layout but re-mounts on navigation) and `route.ts` (API endpoint).

### বাংলা

**প্রশ্ন:** App Router-এ routing কীভাবে কাজ করে? বিশেষ file গুলো কী কী?

**উত্তর:** `app/`-এর ভেতরের folder গুলো URL-এর অংশ ঠিক করে, আর `page.tsx` (বা `route.ts`) থাকলেই কেবল route সবার জন্য খোলা হয়। বিশেষ file: `layout.tsx` (navigation-এর মধ্যেও থাকা শেয়ার্ড UI), `page.tsx` (route-এর UI), `loading.tsx` (Suspense দিয়ে loading UI), `error.tsx` (error boundary, Client Component হতে হবে), `not-found.tsx` (404 UI), `template.tsx` (layout-এর মতো কিন্তু navigation-এ আবার mount হয়) এবং `route.ts` (API endpoint)।

### Example

```
app/
  layout.tsx            -> root layout (required, has <html> and <body>)
  page.tsx              -> /
  loading.tsx           -> loading UI for this segment
  not-found.tsx         -> 404
  dashboard/
    layout.tsx          -> nested layout
    page.tsx            -> /dashboard
    error.tsx           -> error UI for /dashboard
    settings/page.tsx   -> /dashboard/settings
```

---

## Q5. Dynamic routes, catch-all and route groups

### English

**Question:** How do dynamic routes, catch-all segments and route groups work?

**Answer:** `[id]` creates a dynamic segment (`/posts/42`). `[...slug]` is a catch-all that matches any number of segments (`/docs/a/b/c`), and `[[...slug]]` is optional catch-all that also matches the base path. A folder in parentheses like `(marketing)` is a route group: it organizes code or lets you share a layout without affecting the URL. A folder starting with `_` is private and not routable. Since Next.js 15, `params` is a Promise and must be awaited in Server Components.

### বাংলা

**প্রশ্ন:** Dynamic route, catch-all segment ও route group কীভাবে কাজ করে?

**উত্তর:** `[id]` একটি dynamic segment বানায় (`/posts/42`)। `[...slug]` catch-all, যেকোনো সংখ্যক segment মেলায় (`/docs/a/b/c`), আর `[[...slug]]` ঐচ্ছিক catch-all, যা মূল পথও মেলায়। `(marketing)`-এর মতো বন্ধনীর ভেতরের folder হলো route group: URL না বদলে code সাজাতে বা layout শেয়ার করতে দেয়। `_` দিয়ে শুরু হওয়া folder private, route হয় না। Next.js 15 থেকে `params` একটি Promise, তাই Server Component-এ `await` করতে হয়।

### Example

```tsx
// app/posts/[id]/page.tsx
export default async function PostPage({
  params,
}: {
  params: Promise<{ id: string }>;
}) {
  const { id } = await params;
  return <h1>Post {id}</h1>;
}

// app/docs/[...slug]/page.tsx  -> /docs/a/b  => slug = ["a", "b"]
// app/(shop)/cart/page.tsx     -> URL is /cart, "(shop)" is not in the URL
```

---

## Q6. Layouts and templates

### English

**Question:** How do layouts work? What is the difference between a layout and a template?

**Answer:** A layout wraps its child pages and nested layouts. It keeps its state and does not re-render when you navigate between its children, so it is perfect for navbars and sidebars. The root layout (`app/layout.tsx`) is required and must contain `<html>` and `<body>`. A layout receives `children` as a prop. A template looks the same but creates a new instance on every navigation, resetting state and re-running effects; use it for enter/exit animations or per-page logging.

### বাংলা

**প্রশ্ন:** Layout কীভাবে কাজ করে? Layout আর template-এর পার্থক্য কী?

**উত্তর:** Layout তার child পেজ ও nested layout-কে মুড়ে রাখে। Child-দের মধ্যে navigate করলে এর state থেকে যায় এবং আবার render হয় না, তাই navbar ও sidebar-এর জন্য উপযুক্ত। Root layout (`app/layout.tsx`) বাধ্যতামূলক এবং তাতে `<html>` ও `<body>` থাকতে হয়। Layout `children` prop পায়। Template দেখতে একই, কিন্তু প্রতিটি navigation-এ নতুন instance তৈরি করে, ফলে state reset হয় ও effect আবার চলে; enter/exit animation বা প্রতি পেজের logging-এ ব্যবহার করুন।

### Example

```tsx
// app/layout.tsx
export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en">
      <body>
        <Navbar />
        <main>{children}</main>
        <Footer />
      </body>
    </html>
  );
}

// app/dashboard/layout.tsx  (nested)
export default function DashboardLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <div className="flex">
      <Sidebar />
      <section>{children}</section>
    </div>
  );
}
```

---

## Q7. Server vs Client Components in Next.js

### English

**Question:** What is the difference between Server and Client Components in Next.js? When do you need `"use client"`?

**Answer:** In the App Router, every component is a Server Component by default: it renders on the server, sends no component JavaScript to the browser, and can access databases, secrets and the file system. It cannot use `useState`, `useEffect`, event handlers or browser APIs. Add `"use client"` at the top of a file when you need state, effects, event handlers, browser APIs (`window`, `localStorage`) or context providers. The directive marks a boundary: that file and everything it imports becomes client code. Keep Client Components small and as deep in the tree as possible, and pass Server Components to them as `children`.

### বাংলা

**প্রশ্ন:** Next.js-এ Server Component আর Client Component-এর পার্থক্য কী? কখন `"use client"` লাগে?

**উত্তর:** App Router-এ প্রতিটি component ডিফল্টভাবে Server Component: এটি server-এ render হয়, browser-এ component-এর JavaScript পাঠায় না, এবং database, secret ও file system ব্যবহার করতে পারে। এতে `useState`, `useEffect`, event handler বা browser API ব্যবহার করা যায় না। State, effect, event handler, browser API (`window`, `localStorage`) বা context provider দরকার হলে file-এর একদম উপরে `"use client"` দিন। এই directive একটি সীমানা চিহ্নিত করে: ওই file এবং সে যা import করে সবই client code হয়ে যায়। Client Component ছোট রাখুন এবং tree-র যতটা সম্ভব নিচে রাখুন, আর Server Component গুলো `children` হিসেবে পাঠান।

### Example

```tsx
// app/products/page.tsx  (Server Component)
import AddToCart from "./AddToCart";

export default async function ProductsPage() {
  const products = await db.product.findMany(); // runs only on the server
  return (
    <ul>
      {products.map((p) => (
        <li key={p.id}>
          {p.name} <AddToCart productId={p.id} />
        </li>
      ))}
    </ul>
  );
}

// app/products/AddToCart.tsx  (Client Component)
("use client");
import { useState } from "react";

export default function AddToCart({ productId }: { productId: string }) {
  const [added, setAdded] = useState(false);
  return (
    <button onClick={() => setAdded(true)}>{added ? "Added" : "Add"}</button>
  );
}
```

---

## Q8. Data fetching in the App Router

### English

**Question:** How do you fetch data in the App Router?

**Answer:** Server Components can be `async` and fetch data directly with `fetch` or by calling a database/ORM, with no `useEffect`. Independent requests should be started in parallel with `Promise.all` to avoid waterfalls. Note on caching: before Next.js 15, `fetch` results were cached by default; from Next.js 15, `fetch` is not cached by default, and you opt in with `cache: "force-cache"` or `next: { revalidate: n }`. In Next.js 16 with Cache Components, caching is controlled with the `"use cache"` directive. For client-side, interactive data (polling, infinite scroll, mutations), use React Query or SWR inside Client Components.

### বাংলা

**প্রশ্ন:** App Router-এ data কীভাবে আনেন?

**উত্তর:** Server Component `async` হতে পারে এবং `useEffect` ছাড়াই সরাসরি `fetch` দিয়ে বা database/ORM call করে data আনতে পারে। স্বাধীন request গুলো `Promise.all` দিয়ে একসাথে শুরু করতে হয়, যাতে একটার পর একটা অপেক্ষা (waterfall) না হয়। Caching নিয়ে মনে রাখুন: Next.js 15-এর আগে `fetch`-এর ফলাফল ডিফল্টভাবে cache হতো; Next.js 15 থেকে ডিফল্টভাবে cache হয় না, চাইলে `cache: "force-cache"` বা `next: { revalidate: n }` দিয়ে চালু করতে হয়। Cache Components সহ Next.js 16-এ caching নিয়ন্ত্রণ হয় `"use cache"` directive দিয়ে। Client-side ও interactive data (polling, infinite scroll, mutation)-এর জন্য Client Component-এ React Query বা SWR ব্যবহার করুন।

### Example

```tsx
// app/dashboard/page.tsx
async function getUser() {
  return fetch("https://api.example.com/user").then((r) => r.json());
}
async function getPosts() {
  return fetch("https://api.example.com/posts", {
    next: { revalidate: 60 },
  }).then((r) => r.json());
}

export default async function Dashboard() {
  const [user, posts] = await Promise.all([getUser(), getPosts()]); // parallel
  return (
    <>
      <h1>{user.name}</h1>
      <ul>
        {posts.map((p: any) => (
          <li key={p.id}>{p.title}</li>
        ))}
      </ul>
    </>
  );
}
```

---

## Q9. `generateStaticParams` and SSG

### English

**Question:** How do you statically generate dynamic routes in the App Router?

**Answer:** Export `generateStaticParams` from a dynamic route. It returns the list of param values to pre-render at build time (the replacement for `getStaticPaths`). Pages not returned are rendered on demand when first requested and then cached, unless you set `dynamicParams = false`, which makes unknown paths return 404. This gives you the speed of SSG for pages like blog posts and product pages.

### বাংলা

**প্রশ্ন:** App Router-এ dynamic route কীভাবে static ভাবে তৈরি করবেন?

**উত্তর:** Dynamic route থেকে `generateStaticParams` export করুন। এটি build-এর সময় আগে থেকে তৈরি করার param মানের তালিকা return করে (`getStaticPaths`-এর বিকল্প)। তালিকার বাইরের পেজ প্রথম request-এ চাহিদা অনুযায়ী তৈরি হয়ে cache হয়, তবে `dynamicParams = false` দিলে অজানা পথে 404 আসে। এর ফলে blog post ও product পেজের মতো পেজে SSG-র গতি পাওয়া যায়।

### Example

```tsx
// app/blog/[slug]/page.tsx
export async function generateStaticParams() {
  const posts = await fetch("https://api.example.com/posts").then((r) =>
    r.json(),
  );
  return posts.map((post: { slug: string }) => ({ slug: post.slug }));
}

// export const dynamicParams = false;   // unknown slugs -> 404

export default async function BlogPost({
  params,
}: {
  params: Promise<{ slug: string }>;
}) {
  const { slug } = await params;
  const post = await fetch(`https://api.example.com/posts/${slug}`).then((r) =>
    r.json(),
  );
  return (
    <article>
      <h1>{post.title}</h1>
    </article>
  );
}
```

---

## Q10. ISR and `revalidate`

### English

**Question:** How does Incremental Static Regeneration work?

**Answer:** ISR serves a pre-rendered static page and regenerates it in the background after a time window, using a stale-while-revalidate strategy: the first visitor after the window expires still gets the old page while Next.js rebuilds it, and later visitors get the fresh one. Set the time with `export const revalidate = 60` or `next: { revalidate: 60 }` in `fetch`. On-demand revalidation with `revalidatePath` or `revalidateTag` refreshes content immediately after a change (for example, after a CMS update). With Cache Components in Next.js 16, the equivalent control is `cacheLife` in a `"use cache"` scope.

### বাংলা

**প্রশ্ন:** Incremental Static Regeneration কীভাবে কাজ করে?

**উত্তর:** ISR আগে থেকে তৈরি static পেজ পরিবেশন করে এবং নির্দিষ্ট সময় পরে background-এ সেটি আবার তৈরি করে, stale-while-revalidate কৌশলে: সময় পার হওয়ার পর প্রথম দর্শক পুরনো পেজই পান, আর Next.js নতুন করে বানায়; এরপরের দর্শকরা নতুনটি পান। সময় ঠিক করতে `export const revalidate = 60` বা `fetch`-এ `next: { revalidate: 60 }` দিন। `revalidatePath` বা `revalidateTag` দিয়ে on-demand revalidation করলে পরিবর্তনের সাথে সাথে (যেমন CMS আপডেটের পর) content নতুন হয়। Next.js 16-এ Cache Components থাকলে সমতুল্য নিয়ন্ত্রণ হলো `"use cache"` scope-এ `cacheLife`।

### Example

```tsx
// time-based
export const revalidate = 3600; // regenerate at most once per hour

// on-demand: app/api/revalidate/route.ts
import { revalidatePath } from "next/cache";
import { NextRequest, NextResponse } from "next/server";

export async function POST(request: NextRequest) {
  const { secret, path } = await request.json();
  if (secret !== process.env.REVALIDATE_SECRET) {
    return NextResponse.json({ error: "Unauthorized" }, { status: 401 });
  }
  revalidatePath(path);
  return NextResponse.json({ revalidated: true });
}
```

---

## Q11. Streaming, `loading.tsx` and Suspense

### English

**Question:** What is streaming in Next.js? How do `loading.tsx` and `Suspense` work?

**Answer:** Streaming sends the page's HTML to the browser in chunks as each part becomes ready, instead of waiting for all data. Users see the layout and fast parts immediately while slow parts show a fallback. `loading.tsx` automatically wraps the page in a `<Suspense>` boundary with that fallback UI. For finer control, wrap individual slow components in `<Suspense fallback={...}>` yourself. This improves perceived performance (Time to First Byte and First Contentful Paint).

### বাংলা

**প্রশ্ন:** Next.js-এ streaming কী? `loading.tsx` ও `Suspense` কীভাবে কাজ করে?

**উত্তর:** Streaming মানে সব data আসার জন্য অপেক্ষা না করে পেজের HTML-এর অংশগুলো প্রস্তুত হওয়া মাত্র টুকরো টুকরো করে browser-এ পাঠানো। ব্যবহারকারী layout ও দ্রুত অংশগুলো সাথে সাথে দেখেন, ধীর অংশে বিকল্প UI দেখা যায়। `loading.tsx` পেজকে স্বয়ংক্রিয়ভাবে `<Suspense>` boundary-তে মুড়ে ওই fallback দেখায়। আরও সূক্ষ্ম নিয়ন্ত্রণের জন্য ধীর component গুলোকে নিজেই `<Suspense fallback={...}>`-এ মুড়ুন। এতে অনুভূত গতি (TTFB ও First Contentful Paint) বাড়ে।

### Example

```tsx
// app/dashboard/loading.tsx
export default function Loading() {
  return <p>Loading dashboard...</p>;
}

// app/dashboard/page.tsx
import { Suspense } from "react";

export default function Dashboard() {
  return (
    <>
      <h1>Dashboard</h1> {/* shows immediately */}
      <Suspense fallback={<p>Loading stats...</p>}>
        <SlowStats /> {/* async Server Component */}
      </Suspense>
      <Suspense fallback={<p>Loading feed...</p>}>
        <SlowFeed />
      </Suspense>
    </>
  );
}
```

---

## Q12. `Link`, `useRouter` and prefetching

### English

**Question:** How does navigation work in Next.js? What is prefetching?

**Answer:** Use the `<Link>` component for client-side navigation without a full page reload. In production, `Link` automatically prefetches the route when it enters the viewport, so navigation feels instant. For programmatic navigation in Client Components, use `useRouter()` from `next/navigation` (`push`, `replace`, `back`, `refresh`). Other hooks: `usePathname`, `useSearchParams`, `useParams`. On the server, use `redirect()` and `notFound()`.

### বাংলা

**প্রশ্ন:** Next.js-এ navigation কীভাবে কাজ করে? Prefetching কী?

**উত্তর:** পুরো পেজ reload ছাড়া client-side navigation-এর জন্য `<Link>` component ব্যবহার করুন। Production-এ `Link` viewport-এ আসা মাত্র route স্বয়ংক্রিয়ভাবে prefetch করে, তাই navigation তাৎক্ষণিক মনে হয়। Client Component-এ code দিয়ে navigate করতে `next/navigation` থেকে `useRouter()` ব্যবহার করুন (`push`, `replace`, `back`, `refresh`)। অন্যান্য hook: `usePathname`, `useSearchParams`, `useParams`। Server-এ `redirect()` ও `notFound()` ব্যবহার হয়।

### Example

```tsx
import Link from "next/link";
import { redirect } from "next/navigation";

export default function Nav() {
  return (
    <nav>
      <Link href="/">Home</Link>
      <Link href="/blog/hello" prefetch={false}>
        Post
      </Link>
    </nav>
  );
}

// Client Component
("use client");
import { useRouter, usePathname } from "next/navigation";

function LoginButton() {
  const router = useRouter();
  const pathname = usePathname();
  return (
    <button onClick={() => router.push(`/login?from=${pathname}`)}>
      Login
    </button>
  );
}

// Server Component / Server Action
// if (!user) redirect("/login");
```

---

## Q13. `next/image`

### English

**Question:** What does the `next/image` component do and why use it?

**Answer:** `<Image>` optimizes images automatically: it serves modern formats (WebP/AVIF), resizes for the device using `sizes`/`srcset`, lazy loads by default, and prevents layout shift (CLS) by reserving space through `width` and `height` (or `fill`). Use `priority` for above-the-fold images such as the hero (LCP). Remote images must be allowed in `next.config` using `images.remotePatterns` for security.

### বাংলা

**প্রশ্ন:** `next/image` component কী করে এবং কেন ব্যবহার করবেন?

**উত্তর:** `<Image>` স্বয়ংক্রিয়ভাবে ছবি optimize করে: আধুনিক format (WebP/AVIF) দেয়, `sizes`/`srcset` দিয়ে device অনুযায়ী মাপ বদলায়, ডিফল্টভাবে lazy load করে, এবং `width` ও `height` (বা `fill`) দিয়ে জায়গা আগে থেকে ধরে রেখে layout shift (CLS) আটকায়। পেজের উপরের অংশের ছবির (যেমন hero, LCP) জন্য `priority` দিন। নিরাপত্তার জন্য বাইরের ছবির উৎস `next.config`-এ `images.remotePatterns` দিয়ে অনুমোদন করতে হয়।

### Example

```tsx
import Image from "next/image";
import hero from "@/public/hero.jpg"; // static import gives width/height automatically

export default function Hero() {
  return (
    <>
      <Image src={hero} alt="Hero banner" priority placeholder="blur" />
      <Image
        src="https://cdn.example.com/product.jpg"
        alt="Product"
        width={400}
        height={300}
        sizes="(max-width: 768px) 100vw, 400px"
      />
    </>
  );
}

// next.config.ts
// images: { remotePatterns: [{ protocol: "https", hostname: "cdn.example.com" }] }
```

---

## Q14. Font and script optimization

### English

**Question:** How does Next.js optimize fonts and third-party scripts?

**Answer:** `next/font` downloads Google Fonts (or local fonts) at build time and self-hosts them with the rest of your assets, so there are no external font requests, no layout shift and better privacy. `next/script` lets you control when third-party scripts load via the `strategy` prop: `beforeInteractive`, `afterInteractive` (default), `lazyOnload` and `worker` (experimental). This avoids blocking the main thread with analytics or chat widgets.

### বাংলা

**প্রশ্ন:** Next.js কীভাবে font ও তৃতীয় পক্ষের script optimize করে?

**উত্তর:** `next/font` build-এর সময় Google Fonts (বা local font) নামিয়ে বাকি asset-এর সাথে নিজের server থেকে দেয়, ফলে বাইরের font request লাগে না, layout shift হয় না এবং গোপনীয়তাও ভালো থাকে। `next/script` দিয়ে `strategy` prop-এর মাধ্যমে তৃতীয় পক্ষের script কখন লোড হবে নিয়ন্ত্রণ করা যায়: `beforeInteractive`, `afterInteractive` (ডিফল্ট), `lazyOnload` ও `worker` (পরীক্ষামূলক)। এতে analytics বা chat widget main thread আটকে রাখে না।

### Example

```tsx
// app/layout.tsx
import { Inter } from "next/font/google";
import Script from "next/script";

const inter = Inter({ subsets: ["latin"], display: "swap" });

export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en" className={inter.className}>
      <body>
        {children}
        <Script
          src="https://www.googletagmanager.com/gtag/js?id=G-XXXX"
          strategy="afterInteractive"
        />
      </body>
    </html>
  );
}
```

---

## Q15. Metadata and SEO

### English

**Question:** How do you handle SEO and metadata in Next.js?

**Answer:** In the App Router, export a static `metadata` object or an async `generateMetadata` function from a `layout.tsx` or `page.tsx` for title, description, Open Graph and Twitter tags, canonical URLs and robots rules. Metadata merges from parent layouts to child pages. Special files `sitemap.ts`, `robots.ts` and `opengraph-image.tsx` generate those resources. Because pages are rendered on the server, crawlers get full HTML. Also use semantic HTML, `next/image` with alt text, and structured data (JSON-LD).

### বাংলা

**প্রশ্ন:** Next.js-এ SEO ও metadata কীভাবে সামলান?

**উত্তর:** App Router-এ `layout.tsx` বা `page.tsx` থেকে একটি static `metadata` object অথবা async `generateMetadata` function export করুন; এতে title, description, Open Graph ও Twitter tag, canonical URL এবং robots নিয়ম দেওয়া যায়। Parent layout থেকে child পেজে metadata মিলে যায় (merge হয়)। বিশেষ file `sitemap.ts`, `robots.ts` ও `opengraph-image.tsx` ওই resource গুলো তৈরি করে। পেজ server-এ render হয় বলে crawler পুরো HTML পায়। এর সাথে semantic HTML, alt সহ `next/image` এবং structured data (JSON-LD) ব্যবহার করুন।

### Example

```tsx
// app/blog/[slug]/page.tsx
import type { Metadata } from "next";

export async function generateMetadata({
  params,
}: {
  params: Promise<{ slug: string }>;
}): Promise<Metadata> {
  const { slug } = await params;
  const post = await getPost(slug);
  return {
    title: post.title,
    description: post.excerpt,
    alternates: { canonical: `https://example.com/blog/${slug}` },
    openGraph: { title: post.title, images: [post.coverImage] },
  };
}

// app/sitemap.ts
import type { MetadataRoute } from "next";
export default function sitemap(): MetadataRoute.Sitemap {
  return [{ url: "https://example.com", lastModified: new Date() }];
}
```
