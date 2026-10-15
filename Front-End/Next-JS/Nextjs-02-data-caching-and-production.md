# Next.js Data, Caching and Production (Q16 - Q30)

> Place in: `02-frontend/nextjs/02-data-caching-and-production.md`
> Fundamentals and rendering (Q1-Q15) are in `01-fundamentals-and-rendering.md`.
> Version notes: caching behavior changed between Next.js 14, 15 and 16. Check the docs for the exact version you use.

---

## Q16. Route Handlers

### English

**Question:** What are Route Handlers? How do they differ from API routes in the Pages Router?

**Answer:** Route Handlers are the App Router way to create API endpoints. You export functions named after HTTP methods (`GET`, `POST`, `PUT`, `DELETE`) from a `route.ts` file, using the standard Web `Request` and `Response` APIs (or `NextRequest`/`NextResponse`). They replace `pages/api` and do not receive `req`/`res` objects. A `route.ts` cannot live next to a `page.tsx` in the same segment. Use them for webhooks, third-party integrations and public APIs; for internal mutations, Server Actions are often simpler.

### বাংলা

**প্রশ্ন:** Route Handler কী? Pages Router-এর API route থেকে কীভাবে আলাদা?

**উত্তর:** Route Handler হলো App Router-এ API endpoint বানানোর উপায়। `route.ts` file থেকে HTTP method-এর নামে function (`GET`, `POST`, `PUT`, `DELETE`) export করতে হয়, যেখানে স্ট্যান্ডার্ড Web `Request` ও `Response` API (বা `NextRequest`/`NextResponse`) ব্যবহার হয়। এটি `pages/api`-এর জায়গা নেয় এবং `req`/`res` object পায় না। একই segment-এ `page.tsx`-এর পাশে `route.ts` থাকতে পারে না। Webhook, তৃতীয় পক্ষের integration ও public API-র জন্য ব্যবহার করুন; অভ্যন্তরীণ mutation-এর জন্য Server Action প্রায়ই সহজ।

### Example

```ts
// app/api/users/route.ts
import { NextRequest, NextResponse } from "next/server";

export async function GET(request: NextRequest) {
  const page = Number(request.nextUrl.searchParams.get("page") ?? 1);
  const users = await db.user.findMany({ skip: (page - 1) * 10, take: 10 });
  return NextResponse.json(users);
}

export async function POST(request: Request) {
  const body = await request.json();
  if (!body.email) {
    return NextResponse.json({ error: "Email required" }, { status: 400 });
  }
  const user = await db.user.create({ data: body });
  return NextResponse.json(user, { status: 201 });
}

// app/api/users/[id]/route.ts
export async function DELETE(
  _req: Request,
  { params }: { params: Promise<{ id: string }> },
) {
  const { id } = await params;
  await db.user.delete({ where: { id } });
  return new Response(null, { status: 204 });
}
```

---

## Q17. Server Actions

### English

**Question:** What are Server Actions and how do you use them securely?

**Answer:** Server Actions are async functions marked with `"use server"` that run on the server and can be called from forms (`<form action={fn}>`) or from Client Components. They remove the need to write a separate API route for mutations, work with progressive enhancement, and pair with `revalidatePath`/`revalidateTag` to refresh data. Security: every Server Action is a public HTTP endpoint, so always validate input (for example with Zod), check authentication and authorization inside the action, and never trust client-sent values such as user IDs.

### বাংলা

**প্রশ্ন:** Server Action কী এবং নিরাপদে কীভাবে ব্যবহার করবেন?

**উত্তর:** Server Action হলো `"use server"` দিয়ে চিহ্নিত async function, যা server-এ চলে এবং form (`<form action={fn}>`) বা Client Component থেকে call করা যায়। Mutation-এর জন্য আলাদা API route লেখার দরকার থাকে না, progressive enhancement সমর্থন করে এবং data নতুন করতে `revalidatePath`/`revalidateTag`-এর সাথে কাজ করে। নিরাপত্তা: প্রতিটি Server Action আসলে একটি public HTTP endpoint, তাই সবসময় input validate করুন (যেমন Zod দিয়ে), action-এর ভেতরে authentication ও authorization যাচাই করুন, এবং client থেকে আসা মান (যেমন user ID) কখনো বিশ্বাস করবেন না।

### Example

```tsx
// app/actions.ts
"use server";
import { z } from "zod";
import { revalidatePath } from "next/cache";
import { auth } from "@/lib/auth";

const schema = z.object({ title: z.string().min(3) });

export async function createPost(formData: FormData) {
  const session = await auth();
  if (!session) throw new Error("Unauthorized"); // authorize on the server

  const parsed = schema.safeParse({ title: formData.get("title") });
  if (!parsed.success) return { error: "Title is too short" };

  await db.post.create({
    data: { title: parsed.data.title, authorId: session.user.id },
  });
  revalidatePath("/posts");
}

// app/posts/new/page.tsx
import { createPost } from "@/app/actions";
export default function NewPost() {
  return (
    <form action={createPost}>
      <input name="title" />
      <button type="submit">Create</button>
    </form>
  );
}
```

---

## Q18. Caching in Next.js

### English

**Question:** Explain how caching works in Next.js.

**Answer:** Historically (Next.js 13-15 App Router) there were four layers: Request Memoization (the same `fetch` in one render pass is deduplicated), the Data Cache (server-side `fetch` results), the Full Route Cache (static HTML and RSC payload) and the client-side Router Cache. Defaults changed over time: in Next.js 14 `fetch` was cached by default, in Next.js 15 it is not. Next.js 16 introduces Cache Components, a more explicit model: you enable `cacheComponents` in `next.config.ts` and opt in to caching with the `"use cache"` directive on a page, component or function, controlled with `cacheLife` (duration) and `cacheTag` (for invalidation). Anything not cached runs at request time.

### বাংলা

**প্রশ্ন:** Next.js-এ caching কীভাবে কাজ করে ব্যাখ্যা করুন।

**উত্তর:** ঐতিহাসিকভাবে (Next.js 13-15 App Router) চারটি স্তর ছিল: Request Memoization (একই render-এ একই `fetch` একবারই চলে), Data Cache (server-side `fetch`-এর ফলাফল), Full Route Cache (static HTML ও RSC payload) এবং client-side Router Cache। ডিফল্ট সময়ের সাথে বদলেছে: Next.js 14-এ `fetch` ডিফল্টভাবে cache হতো, Next.js 15-এ হয় না। Next.js 16 Cache Components আনে, যা আরও স্পষ্ট ব্যবস্থা: `next.config.ts`-এ `cacheComponents` চালু করে পেজ, component বা function-এ `"use cache"` directive দিয়ে caching বেছে নিতে হয়, `cacheLife` (সময়কাল) ও `cacheTag` (invalidate করার জন্য) দিয়ে নিয়ন্ত্রণ হয়। যা cache করা হয়নি তা request-এর সময় চলে।

### Example

```ts
// next.config.ts
import type { NextConfig } from "next";
const nextConfig: NextConfig = { cacheComponents: true };
export default nextConfig;
```

```tsx
// Next.js 16 Cache Components
import { cacheLife, cacheTag } from "next/cache";

export async function getProducts() {
  "use cache";
  cacheLife("hours"); // built-in profile
  cacheTag("products"); // for later invalidation
  return db.product.findMany();
}

// Next.js 15 style (fetch-level)
await fetch("https://api.example.com/products", {
  cache: "force-cache", // opt in to caching
  next: { revalidate: 3600, tags: ["products"] },
});
```

---

## Q19. `revalidatePath` vs `revalidateTag`

### English

**Question:** What is the difference between `revalidatePath` and `revalidateTag`?

**Answer:** Both invalidate cached data on demand, usually after a mutation in a Server Action or Route Handler. `revalidatePath("/posts")` invalidates the cache for a specific route, which is simple but coarse. `revalidateTag("posts")` invalidates every cached entry that was tagged with that name (through `fetch` `next.tags` or `cacheTag`), even across many pages, which is precise and scalable. In Next.js 16, there is also `updateTag` for Server Actions when the user must immediately see their own change (read-your-writes). Check the docs for the exact signature in your version.

### বাংলা

**প্রশ্ন:** `revalidatePath` আর `revalidateTag`-এর পার্থক্য কী?

**উত্তর:** দুটিই প্রয়োজনে cache করা data অকার্যকর (invalidate) করে, সাধারণত Server Action বা Route Handler-এ mutation-এর পরে। `revalidatePath("/posts")` একটি নির্দিষ্ট route-এর cache অকার্যকর করে, যা সহজ কিন্তু মোটা দাগের। `revalidateTag("posts")` ওই নামে tag করা সব cache entry (`fetch`-এর `next.tags` বা `cacheTag` দিয়ে) অকার্যকর করে, এমনকি অনেক পেজ জুড়ে, যা নির্ভুল ও বড় app-এর জন্য ভালো। Next.js 16-এ Server Action-এর জন্য `updateTag`-ও আছে, যখন ব্যবহারকারীকে নিজের করা পরিবর্তন সাথে সাথে দেখাতে হবে (read-your-writes)। আপনার সংস্করণের সঠিক signature docs-এ দেখে নিন।

### Example

```ts
"use server";
import { revalidatePath, revalidateTag } from "next/cache";

export async function publishPost(id: string) {
  await db.post.update({ where: { id }, data: { published: true } });

  revalidatePath("/blog"); // one route
  revalidatePath(`/blog/${id}`); // its detail page
  // revalidateTag("posts");            // everything tagged "posts"
}
```

---

## Q20. Proxy (middleware)

### English

**Question:** What is Middleware (now Proxy) in Next.js and what is it used for?

**Answer:** It is code that runs on the server before a request is completed, so you can redirect, rewrite, set headers or cookies, and run lightweight checks for matched routes. Typical uses: redirecting unauthenticated users, locale detection, A/B testing, geo routing and adding security headers. Up to Next.js 15 it is `middleware.ts` with `export function middleware`; in Next.js 16 it was renamed to `proxy.ts` with `export function proxy`, and runs on the Node.js runtime. Limit its use to quick checks; do not do heavy data fetching there, and do real authorization close to the data (in Server Components or Server Actions).

### বাংলা

**প্রশ্ন:** Next.js-এ Middleware (এখন Proxy) কী এবং কীসের জন্য ব্যবহার হয়?

**উত্তর:** এটি এমন code যা request সম্পূর্ণ হওয়ার আগে server-এ চলে, ফলে মিলে যাওয়া route-এর জন্য redirect, rewrite, header বা cookie সেট করা এবং হালকা যাচাই করা যায়। সাধারণ ব্যবহার: লগইন না করা ব্যবহারকারীকে redirect, ভাষা শনাক্ত, A/B testing, geo routing এবং security header যোগ। Next.js 15 পর্যন্ত এটি `middleware.ts` ও `export function middleware`; Next.js 16-এ নাম বদলে হয়েছে `proxy.ts` ও `export function proxy`, এবং Node.js runtime-এ চলে। দ্রুত যাচাইয়ের জন্যই ব্যবহার করুন; এখানে ভারী data fetching করবেন না, আর আসল authorization data-র কাছাকাছি (Server Component বা Server Action-এ) করুন।

### Example

```ts
// proxy.ts  (Next.js 16)   |  in Next.js 15 and earlier: middleware.ts + "export function middleware"
import { NextResponse, type NextRequest } from "next/server";

export function proxy(request: NextRequest) {
  const session = request.cookies.get("session")?.value;

  if (!session && request.nextUrl.pathname.startsWith("/dashboard")) {
    return NextResponse.redirect(new URL("/login", request.url));
  }

  const response = NextResponse.next();
  response.headers.set("X-Frame-Options", "DENY");
  return response;
}

export const config = {
  matcher: ["/dashboard/:path*", "/settings/:path*"],
};
```

---

## Q21. Authentication in Next.js

### English

**Question:** How do you implement authentication in a Next.js app?

**Answer:** Options: a library such as Auth.js (NextAuth) or Better Auth, a hosted provider (Clerk, Auth0), or your own session/JWT logic. Core practices: store the session or token in an `HttpOnly`, `Secure`, `SameSite` cookie; verify the session on the server inside Server Components, Server Actions and Route Handlers (a "data access layer") because every action is a public endpoint; use `proxy.ts`/middleware only for optimistic redirects, not as the only protection; hash passwords with bcrypt or argon2; and add role-based checks for authorization. Never expose secrets to the client.

### বাংলা

**প্রশ্ন:** Next.js app-এ authentication কীভাবে বানাবেন?

**উত্তর:** বিকল্প: Auth.js (NextAuth) বা Better Auth-এর মতো library, hosted সেবা (Clerk, Auth0), অথবা নিজের session/JWT logic। মূল অভ্যাস: session বা token `HttpOnly`, `Secure`, `SameSite` cookie-তে রাখুন; Server Component, Server Action ও Route Handler-এর ভেতরে server-এ session যাচাই করুন ("data access layer"), কারণ প্রতিটি action একটি public endpoint; `proxy.ts`/middleware শুধু দ্রুত redirect-এর জন্য ব্যবহার করুন, একমাত্র সুরক্ষা হিসেবে নয়; password bcrypt বা argon2 দিয়ে hash করুন; এবং authorization-এর জন্য role-ভিত্তিক যাচাই যোগ করুন। Secret কখনো client-এ পাঠাবেন না।

### Example

```ts
// lib/dal.ts  (data access layer)
import "server-only";
import { cookies } from "next/headers";
import { redirect } from "next/navigation";
import { verifySession } from "@/lib/session";

export async function requireUser() {
  const token = (await cookies()).get("session")?.value;
  const session = token ? await verifySession(token) : null;
  if (!session) redirect("/login");
  return session.user;
}

// app/dashboard/page.tsx
export default async function Dashboard() {
  const user = await requireUser();          // checked on the server every time
  return <h1>Welcome, {user.name}</h1>;
}

// setting the cookie after login
(await cookies()).set("session", token, {
  httpOnly: true, secure: true, sameSite: "lax", path: "/", maxAge: 60 * 60 * 24 * 7,
});
```

---

## Q22. Environment variables

### English

**Question:** How do environment variables work in Next.js? What does `NEXT_PUBLIC_` mean?

**Answer:** Next.js loads variables from `.env`, `.env.local`, `.env.development` and `.env.production`. By default, variables are available only on the server. Variables prefixed with `NEXT_PUBLIC_` are inlined into the browser bundle at build time, so they are public and must never hold secrets; changing them requires a rebuild. Keep `.env.local` out of Git, and use the `server-only` package to make sure server code is never imported into a Client Component.

### বাংলা

**প্রশ্ন:** Next.js-এ environment variable কীভাবে কাজ করে? `NEXT_PUBLIC_` মানে কী?

**উত্তর:** Next.js `.env`, `.env.local`, `.env.development` ও `.env.production` থেকে variable লোড করে। ডিফল্টভাবে variable শুধু server-এ পাওয়া যায়। `NEXT_PUBLIC_` দিয়ে শুরু হওয়া variable build-এর সময় browser bundle-এ বসে যায়, তাই এগুলো public এবং কখনো secret রাখা উচিত নয়; এদের বদলাতে হলে আবার build করতে হয়। `.env.local` Git-এ রাখবেন না, আর `server-only` package ব্যবহার করুন যাতে server code ভুলে Client Component-এ import না হয়।

### Example

```bash
# .env.local
DATABASE_URL="postgres://..."                # server only
STRIPE_SECRET_KEY="sk_live_..."              # server only
NEXT_PUBLIC_API_URL="https://api.example.com" # exposed to the browser
```

```ts
// server code
const db = process.env.DATABASE_URL;

// client code
const apiUrl = process.env.NEXT_PUBLIC_API_URL;

// lib/secret.ts
import "server-only"; // build error if imported by a Client Component
```

---

## Q23. Hydration and hydration errors

### English

**Question:** What is hydration? What causes hydration errors and how do you fix them?

**Answer:** Hydration is when React attaches event listeners and state to the server-rendered HTML in the browser, making it interactive. A hydration error occurs when the HTML rendered on the server differs from what the client renders first. Common causes: using `Date.now()`, `Math.random()` or timezone-dependent values during render; reading `window` or `localStorage` during render; invalid HTML nesting (for example, `<div>` inside `<p>`); and browser extensions that modify the DOM. Fixes: move browser-only logic into `useEffect`, render the same initial output on server and client, use `suppressHydrationWarning` for unavoidable differences like timestamps, or load a component client-only with `dynamic(..., { ssr: false })` inside a Client Component.

### বাংলা

**প্রশ্ন:** Hydration কী? Hydration error কেন হয় এবং কীভাবে ঠিক করবেন?

**উত্তর:** Hydration হলো browser-এ server-এ render করা HTML-এ React-এর event listener ও state জুড়ে দেওয়া, যাতে সেটি interactive হয়। Server-এ যে HTML তৈরি হয়েছে আর client প্রথমে যা render করে তা না মিললে hydration error হয়। সাধারণ কারণ: render-এর সময় `Date.now()`, `Math.random()` বা timezone-নির্ভর মান ব্যবহার; render-এ `window` বা `localStorage` পড়া; ভুল HTML nesting (যেমন `<p>`-এর ভেতরে `<div>`); এবং DOM বদলে দেওয়া browser extension। সমাধান: browser-নির্ভর logic `useEffect`-এ নিন, server ও client-এ প্রাথমিক output একই রাখুন, অনিবার্য পার্থক্যের (যেমন সময়) জন্য `suppressHydrationWarning` দিন, অথবা Client Component-এর ভেতরে `dynamic(..., { ssr: false })` দিয়ে শুধু client-এ লোড করুন।

### Example

```tsx
"use client";
import { useEffect, useState } from "react";
import dynamic from "next/dynamic";

// Bad: different on server and client -> hydration mismatch
function Bad() {
  return <p>{new Date().toLocaleTimeString()}</p>;
}

// Good: render after mount
function Good() {
  const [time, setTime] = useState<string | null>(null);
  useEffect(() => setTime(new Date().toLocaleTimeString()), []);
  return <p>{time ?? "..."}</p>;
}

// Client-only component
const Map = dynamic(() => import("./Map"), {
  ssr: false,
  loading: () => <p>Loading map...</p>,
});
```

---

## Q24. Error handling

### English

**Question:** How do you handle errors in the App Router?

**Answer:** `error.tsx` is an error boundary for a route segment; it must be a Client Component and receives `error` and a `reset` function to retry. `global-error.tsx` handles errors in the root layout and must define its own `<html>` and `<body>`. `not-found.tsx` renders when `notFound()` is called or a route does not match. Expected errors (such as validation failures) should be returned as values from Server Actions instead of thrown. Errors in event handlers are not caught by error boundaries; handle them with `try/catch`. Log errors to a service such as Sentry.

### বাংলা

**প্রশ্ন:** App Router-এ error কীভাবে সামলান?

**উত্তর:** `error.tsx` একটি route segment-এর error boundary; এটি Client Component হতে হবে এবং `error` ও আবার চেষ্টা করার `reset` function পায়। `global-error.tsx` root layout-এর error সামলায় এবং নিজের `<html>` ও `<body>` ঠিক করতে হয়। `notFound()` call হলে বা route না মিললে `not-found.tsx` দেখায়। প্রত্যাশিত error (যেমন validation ব্যর্থতা) throw না করে Server Action থেকে মান হিসেবে return করুন। Event handler-এর error boundary ধরে না; `try/catch` দিয়ে সামলান। Sentry-র মতো সেবায় error log করুন।

### Example

```tsx
// app/dashboard/error.tsx
"use client";
export default function Error({
  error,
  reset,
}: {
  error: Error;
  reset: () => void;
}) {
  return (
    <div>
      <h2>Something went wrong</h2>
      <p>{error.message}</p>
      <button onClick={() => reset()}>Try again</button>
    </div>
  );
}

// app/posts/[id]/page.tsx
import { notFound } from "next/navigation";
export default async function Post({
  params,
}: {
  params: Promise<{ id: string }>;
}) {
  const { id } = await params;
  const post = await getPost(id);
  if (!post) notFound(); // renders not-found.tsx
  return <h1>{post.title}</h1>;
}
```

---

## Q25. Performance optimization

### English

**Question:** How do you optimize the performance of a Next.js application?

**Answer:**

- Keep components as Server Components by default and push `"use client"` down to small interactive leaves.
- Use the right rendering strategy: static or ISR where possible, streaming with `Suspense` for slow parts.
- Optimize images with `next/image` (`priority` for LCP, `sizes`) and fonts with `next/font`.
- Fetch data in parallel to avoid waterfalls, and cache with `"use cache"` / revalidation.
- Reduce bundle size: dynamic imports (`next/dynamic`), analyze with `@next/bundle-analyzer`, avoid heavy libraries.
- Load third-party scripts with `next/script` and a lazy strategy.
- Use a CDN and measure Core Web Vitals (LCP, INP, CLS) with Lighthouse and real-user monitoring.

### বাংলা

**প্রশ্ন:** Next.js application-এর performance কীভাবে বাড়াবেন?

**উত্তর:**

- Component ডিফল্টভাবে Server Component রাখুন এবং `"use client"` ছোট interactive অংশে নামিয়ে দিন।
- সঠিক render কৌশল বেছে নিন: সম্ভব হলে static বা ISR, ধীর অংশে `Suspense` দিয়ে streaming।
- `next/image` দিয়ে ছবি (LCP-র জন্য `priority`, `sizes`) এবং `next/font` দিয়ে font optimize করুন।
- Data parallel ভাবে আনুন যাতে waterfall না হয়, এবং `"use cache"` / revalidation দিয়ে cache করুন।
- Bundle ছোট করুন: dynamic import (`next/dynamic`), `@next/bundle-analyzer` দিয়ে বিশ্লেষণ, ভারী library এড়ানো।
- তৃতীয় পক্ষের script `next/script` ও lazy strategy দিয়ে লোড করুন।
- CDN ব্যবহার করুন এবং Lighthouse ও real-user monitoring দিয়ে Core Web Vitals (LCP, INP, CLS) মাপুন।

### Example

```tsx
import dynamic from "next/dynamic";
import { Suspense } from "react";

const Chart = dynamic(() => import("./Chart"), {
  loading: () => <p>Loading chart...</p>, // loaded only when needed
});

export default function Page() {
  return (
    <>
      <Hero />
      <Suspense fallback={<p>Loading...</p>}>
        <Chart />
      </Suspense>
    </>
  );
}
```

---

## Q26. Static vs dynamic rendering

### English

**Question:** How does Next.js decide between static and dynamic rendering for a route?

**Answer:** A route is rendered statically (at build time, or revalidated later) when it uses no request-specific data. It becomes dynamic (rendered at request time) when it uses dynamic APIs such as `cookies()`, `headers()`, `searchParams`, or an uncached data request in a way that needs request-time data. In Next.js 15 and later, `cookies()`, `headers()`, `params` and `searchParams` are async and must be awaited. In older versions, you could force behavior with `export const dynamic = "force-dynamic"` or `"force-static"`; with Cache Components in Next.js 16, you instead choose what to cache with `"use cache"` and wrap request-time parts in `Suspense`.

### বাংলা

**প্রশ্ন:** Next.js কীভাবে ঠিক করে একটি route static নাকি dynamic ভাবে render হবে?

**উত্তর:** যে route কোনো request-নির্ভর data ব্যবহার করে না সেটি static ভাবে (build-এর সময়, বা পরে revalidate হয়ে) render হয়। `cookies()`, `headers()`, `searchParams`-এর মতো dynamic API অথবা request-সময়ের data দরকার এমন cache না করা data request ব্যবহার করলে route dynamic (request-এর সময় render) হয়ে যায়। Next.js 15 ও তার পরে `cookies()`, `headers()`, `params` ও `searchParams` async, তাই `await` করতে হয়। পুরনো সংস্করণে `export const dynamic = "force-dynamic"` বা `"force-static"` দিয়ে জোর করা যেত; Next.js 16-এর Cache Components-এ বরং `"use cache"` দিয়ে কী cache হবে বেছে নেওয়া হয় এবং request-সময়ের অংশ `Suspense`-এ মোড়ানো হয়।

### Example

```tsx
import { cookies, headers } from "next/headers";

// dynamic: reads request data
export default async function Page({
  searchParams,
}: {
  searchParams: Promise<{ q?: string }>;
}) {
  const { q } = await searchParams;
  const theme = (await cookies()).get("theme")?.value ?? "light";
  const ua = (await headers()).get("user-agent");
  return (
    <p>
      Search: {q} | theme: {theme}
    </p>
  );
}

// force behavior in older versions:
// export const dynamic = "force-dynamic";
```

---

## Q27. Partial Prerendering and Cache Components

### English

**Question:** What is Partial Prerendering (PPR) and how do Cache Components relate to it?

**Answer:** Partial Prerendering combines static and dynamic rendering in a single route: Next.js serves a pre-rendered static shell instantly from the CDN and streams in the dynamic parts (inside `Suspense` boundaries) at request time. Cache Components (Next.js 16) build on PPR: cached content (`"use cache"`) becomes part of the static shell, while request-time parts such as cookies-based content are wrapped in `Suspense` and streamed. The benefit is the speed of static pages with the freshness of dynamic ones, without choosing one mode for the whole page.

### বাংলা

**প্রশ্ন:** Partial Prerendering (PPR) কী এবং Cache Components-এর সাথে এর সম্পর্ক কী?

**উত্তর:** Partial Prerendering একই route-এ static ও dynamic render মিলিয়ে দেয়: Next.js আগে থেকে তৈরি একটি static shell CDN থেকে সাথে সাথে পাঠায় এবং dynamic অংশগুলো (`Suspense` boundary-র ভেতরে) request-এর সময় stream করে আনে। Cache Components (Next.js 16) PPR-এর উপর তৈরি: cache করা content (`"use cache"`) static shell-এর অংশ হয়, আর cookie-ভিত্তিক content-এর মতো request-সময়ের অংশ `Suspense`-এ মুড়ে stream হয়। সুবিধা হলো পুরো পেজের জন্য একটি মোড বেছে না নিয়ে static পেজের গতি ও dynamic পেজের নতুনত্ব একসাথে পাওয়া।

### Example

```tsx
// next.config.ts -> cacheComponents: true
import { Suspense } from "react";
import { cookies } from "next/headers";
import { cacheLife } from "next/cache";

async function ProductInfo() {
  "use cache"; // part of the static shell
  cacheLife("hours");
  const product = await db.product.findFirst();
  return <h1>{product.name}</h1>;
}

async function UserGreeting() {
  const name = (await cookies()).get("name")?.value; // request-time data
  return <p>Hello, {name ?? "guest"}</p>;
}

export default function Page() {
  return (
    <>
      <ProductInfo /> {/* instant */}
      <Suspense fallback={<p>...</p>}>
        <UserGreeting /> {/* streamed in */}
      </Suspense>
    </>
  );
}
```

---

## Q28. Deployment

### English

**Question:** How do you deploy a Next.js application?

**Answer:** The easiest option is Vercel (zero config, supports all features, preview deployments). You can also self-host: run `next build` then `next start` on a Node.js server (Next.js 16 requires Node.js 20.9 or later), or use `output: "standalone"` to produce a minimal folder for Docker. Behind a reverse proxy like Nginx, configure caching and compression. Static-only sites can use `output: "export"` and be hosted on any static host, but this disables server features (Server Actions, dynamic rendering, ISR). Set environment variables in the hosting platform and use a CDN for static assets.

### বাংলা

**প্রশ্ন:** Next.js application কীভাবে deploy করবেন?

**উত্তর:** সবচেয়ে সহজ উপায় Vercel (কোনো কনফিগ লাগে না, সব feature সমর্থন করে, preview deployment আছে)। নিজের server-এও চালানো যায়: Node.js server-এ `next build` তারপর `next start` (Next.js 16-এ Node.js 20.9 বা তার পরের সংস্করণ লাগে), অথবা Docker-এর জন্য `output: "standalone"` দিয়ে ছোট একটি folder বানানো যায়। Nginx-এর মতো reverse proxy-র পেছনে caching ও compression ঠিক করুন। শুধু static সাইট হলে `output: "export"` দিয়ে যেকোনো static host-এ রাখা যায়, তবে তাতে server feature (Server Action, dynamic rendering, ISR) বন্ধ হয়ে যায়। Hosting platform-এ environment variable সেট করুন এবং static asset-এর জন্য CDN ব্যবহার করুন।

### Example

```ts
// next.config.ts
import type { NextConfig } from "next";
const nextConfig: NextConfig = {
  output: "standalone", // minimal server for Docker
};
export default nextConfig;
```

```dockerfile
FROM node:20-alpine AS builder
WORKDIR /app
COPY . .
RUN npm ci && npm run build

FROM node:20-alpine
WORKDIR /app
ENV NODE_ENV=production
COPY --from=builder /app/.next/standalone ./
COPY --from=builder /app/.next/static ./.next/static
COPY --from=builder /app/public ./public
EXPOSE 3000
CMD ["node", "server.js"]
```

---

## Q29. Pages Router data fetching

### English

**Question:** How did data fetching work in the Pages Router (`getStaticProps`, `getServerSideProps`, `getStaticPaths`)?

**Answer:**

- `getStaticProps`: runs at build time (and on revalidation with `revalidate`) to fetch data for a statically generated page (SSG/ISR).
- `getStaticPaths`: used with dynamic routes to list which paths to pre-render, with `fallback` controlling unknown paths.
- `getServerSideProps`: runs on every request on the server (SSR) and passes data as props.

These functions run only on the server and work only in page files, not in regular components. In the App Router they are replaced by `async` Server Components, `generateStaticParams` and cache/revalidate settings. This question still appears often because many existing projects use the Pages Router.

### বাংলা

**প্রশ্ন:** Pages Router-এ data fetching কীভাবে কাজ করত (`getStaticProps`, `getServerSideProps`, `getStaticPaths`)?

**উত্তর:**

- `getStaticProps`: build-এর সময় (এবং `revalidate` দিলে revalidation-এর সময়) static ভাবে তৈরি পেজের (SSG/ISR) data আনে।
- `getStaticPaths`: dynamic route-এর সাথে ব্যবহার হয়, কোন path গুলো আগে থেকে তৈরি হবে তা বলে, আর `fallback` অজানা path কীভাবে চলবে ঠিক করে।
- `getServerSideProps`: প্রতিটি request-এ server-এ চলে (SSR) এবং data props হিসেবে পাঠায়।

এই function গুলো শুধু server-এ চলে এবং শুধু page file-এ কাজ করে, সাধারণ component-এ নয়। App Router-এ এদের জায়গা নিয়েছে `async` Server Component, `generateStaticParams` এবং cache/revalidate সেটিং। অনেক চালু project এখনো Pages Router ব্যবহার করে, তাই এই প্রশ্ন এখনো প্রায়ই আসে।

### Example

```tsx
// pages/posts/[id].tsx
import type { GetStaticPaths, GetStaticProps } from "next";

export const getStaticPaths: GetStaticPaths = async () => {
  const posts = await fetchPosts();
  return {
    paths: posts.map((p) => ({ params: { id: String(p.id) } })),
    fallback: "blocking",
  };
};

export const getStaticProps: GetStaticProps = async ({ params }) => {
  const post = await fetchPost(params!.id as string);
  if (!post) return { notFound: true };
  return { props: { post }, revalidate: 60 }; // ISR
};

export default function Post({ post }: { post: { title: string } }) {
  return <h1>{post.title}</h1>;
}

// SSR version
// export const getServerSideProps: GetServerSideProps = async (ctx) => ({ props: { ... } });
```

---

## Q30. What is new in Next.js 15 and 16

### English

**Question:** What are the major changes in Next.js 15 and 16?

**Answer:**

- **Next.js 15:** React 19 support; `fetch` requests, `GET` Route Handlers and the client Router Cache are no longer cached by default; `cookies()`, `headers()`, `params` and `searchParams` became async; Turbopack for development became stable; `next/form` and improved Server Actions security.
- **Next.js 16:** Turbopack is the default bundler; Cache Components with the `"use cache"` directive and Partial Prerendering; `middleware.ts` renamed to `proxy.ts` (Node.js runtime); stable React Compiler support; new caching APIs such as `updateTag`; incremental prefetching and improved routing; Node.js 20.9+ and TypeScript 5.1+ required; React 19.2 features; DevTools MCP for AI-assisted debugging.

Always check the official upgrade guide, and use the codemod `npx @next/codemod@canary upgrade latest` to migrate.

### বাংলা

**প্রশ্ন:** Next.js 15 ও 16-তে প্রধান পরিবর্তনগুলো কী?

**উত্তর:**

- **Next.js 15:** React 19 সমর্থন; `fetch` request, `GET` Route Handler এবং client Router Cache আর ডিফল্টভাবে cache হয় না; `cookies()`, `headers()`, `params` ও `searchParams` async হয়েছে; development-এর জন্য Turbopack stable; `next/form` এবং উন্নত Server Actions নিরাপত্তা।
- **Next.js 16:** Turbopack ডিফল্ট bundler; `"use cache"` directive সহ Cache Components ও Partial Prerendering; `middleware.ts`-এর নাম বদলে `proxy.ts` (Node.js runtime); stable React Compiler সমর্থন; `updateTag`-এর মতো নতুন caching API; incremental prefetching ও উন্নত routing; Node.js 20.9+ ও TypeScript 5.1+ প্রয়োজন; React 19.2-এর feature; AI-সহায়তায় debug করার জন্য DevTools MCP।

সবসময় অফিসিয়াল upgrade guide দেখুন, এবং migrate করতে `npx @next/codemod@canary upgrade latest` codemod ব্যবহার করুন।

### Example

```ts
// Next.js 14: this was cached by default
const res = await fetch("https://api.example.com/data");

// Next.js 15+: not cached by default, opt in explicitly
const cached = await fetch("https://api.example.com/data", {
  cache: "force-cache",
});

// Next.js 15+: async request APIs
const { id } = await params;
const cookieStore = await cookies();

// Next.js 16: rename middleware.ts -> proxy.ts, and the export to `proxy`
// export function proxy(request: NextRequest) { ... }
```
