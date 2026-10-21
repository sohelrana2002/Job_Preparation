# HTML and Accessibility (Q1 - Q12)

> Place in: `02-frontend/html-css/01-html-and-accessibility.md`
> CSS questions (Q13-Q30) are in `02-css.md`.

## Top 30 Index

| #   | Topic                                                   | File |
| --- | ------------------------------------------------------- | ---- |
| 1   | Semantic HTML                                           | 01   |
| 2   | Block, inline and inline-block                          | 01   |
| 3   | HTML5 new features                                      | 01   |
| 4   | Web accessibility basics and ARIA                       | 01   |
| 5   | `async` vs `defer` scripts                              | 01   |
| 6   | Meta tags and SEO                                       | 01   |
| 7   | Forms and validation                                    | 01   |
| 8   | `data-*` attributes                                     | 01   |
| 9   | Responsive images                                       | 01   |
| 10  | Critical rendering path                                 | 01   |
| 11  | Reflow vs repaint                                       | 01   |
| 12  | `<button>` vs `<a>`, `target="_blank"`, iframe security | 01   |
| 13  | Box model and `box-sizing`                              | 02   |
| 14  | Selectors and specificity                               | 02   |
| 15  | Cascade, inheritance and `!important`                   | 02   |
| 16  | `display: none` vs `visibility: hidden` vs `opacity: 0` | 02   |
| 17  | The `position` property                                 | 02   |
| 18  | `z-index` and stacking context                          | 02   |
| 19  | Flexbox                                                 | 02   |
| 20  | CSS Grid and Flexbox vs Grid                            | 02   |
| 21  | Responsive design and media queries                     | 02   |
| 22  | CSS units                                               | 02   |
| 23  | Pseudo-classes vs pseudo-elements                       | 02   |
| 24  | Centering an element                                    | 02   |
| 25  | CSS variables and dark mode                             | 02   |
| 26  | Transitions, animations and performance                 | 02   |
| 27  | Margin collapsing                                       | 02   |
| 28  | CSS approaches: Tailwind, Modules, BEM                  | 02   |
| 29  | Overflow and text truncation                            | 02   |
| 30  | Modern CSS features                                     | 02   |

---

## Q1. Semantic HTML

### English

**Question:** What is semantic HTML and why does it matter?

**Answer:** Semantic HTML uses elements that describe the meaning of the content, such as `<header>`, `<nav>`, `<main>`, `<article>`, `<section>`, `<aside>`, `<footer>`, `<figure>`, `<button>` and `<time>`, instead of generic `<div>` and `<span>` everywhere. Benefits: better accessibility (screen readers can navigate landmarks), better SEO (search engines understand the page structure), cleaner and more maintainable code, and built-in browser behavior (a `<button>` is focusable and works with the keyboard by default).

### বাংলা

**প্রশ্ন:** Semantic HTML কী এবং কেন গুরুত্বপূর্ণ?

**উত্তর:** Semantic HTML-এ সব জায়গায় সাধারণ `<div>` ও `<span>` না দিয়ে content-এর অর্থ বোঝায় এমন element ব্যবহার করা হয়, যেমন `<header>`, `<nav>`, `<main>`, `<article>`, `<section>`, `<aside>`, `<footer>`, `<figure>`, `<button>` ও `<time>`। সুবিধা: ভালো accessibility (screen reader landmark ধরে ঘুরতে পারে), ভালো SEO (search engine পেজের কাঠামো বোঝে), পরিষ্কার ও রক্ষণাবেক্ষণযোগ্য code, এবং বিল্ট-ইন browser আচরণ (`<button>` ডিফল্টভাবেই focus ও keyboard দিয়ে চালানো যায়)।

### Example

```html
<!-- Non-semantic -->
<div class="header"><div class="nav">...</div></div>
<div class="click" onclick="save()">Save</div>

<!-- Semantic -->
<header>
  <nav aria-label="Main">
    <ul>
      <li><a href="/">Home</a></li>
    </ul>
  </nav>
</header>
<main>
  <article>
    <h1>Article title</h1>
    <p>Published <time datetime="2026-10-21">21 Oct 2026</time></p>
  </article>
  <aside>Related links</aside>
</main>
<footer>&copy; 2026</footer>
<button type="button" onclick="save()">Save</button>
```

---

## Q2. Block, inline and inline-block

### English

**Question:** What is the difference between block, inline and inline-block elements?

**Answer:** Block elements (`div`, `p`, `h1`, `section`) start on a new line, take the full available width and respect width, height, margin and padding on all sides. Inline elements (`span`, `a`, `strong`) flow within a line, take only the space they need, and ignore `width`/`height` and vertical margins. `inline-block` flows inline like text but accepts width, height and vertical margin/padding like a block. You can change this with the CSS `display` property.

### বাংলা

**প্রশ্ন:** Block, inline ও inline-block element-এর পার্থক্য কী?

**উত্তর:** Block element (`div`, `p`, `h1`, `section`) নতুন লাইনে শুরু হয়, পুরো প্রস্থ নেয় এবং width, height, চারপাশের margin ও padding মানে। Inline element (`span`, `a`, `strong`) লাইনের মধ্যে বসে, যতটুকু দরকার ততটুকু জায়গা নেয়, এবং `width`/`height` ও উপর-নিচের margin উপেক্ষা করে। `inline-block` লেখার মতো লাইনের মধ্যে বসে কিন্তু block-এর মতো width, height ও উপর-নিচের margin/padding মানে। CSS-এর `display` property দিয়ে এটি বদলানো যায়।

### Example

```html
<div>Block 1</div>
<div>Block 2</div>
<!-- each on its own line -->
<span>Inline 1</span><span>Inline 2</span>
<!-- same line -->
```

```css
span.badge {
  display: inline-block; /* now width/height/padding work */
  width: 80px;
  height: 30px;
  padding: 4px 8px;
}
```

---

## Q3. HTML5 new features

### English

**Question:** What are the main new features introduced in HTML5?

**Answer:** Semantic elements (`header`, `nav`, `main`, `article`, `section`, `footer`); native multimedia with `<audio>` and `<video>` (no Flash); `<canvas>` and inline SVG for graphics; new form input types (`email`, `date`, `number`, `range`, `tel`, `url`) with built-in validation attributes (`required`, `pattern`, `min`, `max`); Web Storage (`localStorage`, `sessionStorage`); Geolocation, Drag and Drop, Web Workers and the History API; and a simplified `<!DOCTYPE html>` and `<meta charset>`.

### বাংলা

**প্রশ্ন:** HTML5-এ প্রধান নতুন feature গুলো কী?

**উত্তর:** Semantic element (`header`, `nav`, `main`, `article`, `section`, `footer`); `<audio>` ও `<video>` দিয়ে native multimedia (Flash লাগে না); গ্রাফিক্সের জন্য `<canvas>` ও inline SVG; বিল্ট-ইন validation attribute (`required`, `pattern`, `min`, `max`) সহ নতুন form input type (`email`, `date`, `number`, `range`, `tel`, `url`); Web Storage (`localStorage`, `sessionStorage`); Geolocation, Drag and Drop, Web Worker ও History API; এবং সহজ `<!DOCTYPE html>` ও `<meta charset>`।

### Example

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>HTML5 demo</title>
  </head>
  <body>
    <video controls width="320" poster="cover.jpg">
      <source src="clip.mp4" type="video/mp4" />
      Your browser does not support video.
    </video>
    <input type="email" required placeholder="you@example.com" />
    <input type="date" min="2026-01-01" />
    <canvas id="board" width="300" height="150"></canvas>
  </body>
</html>
```

---

## Q4. Web accessibility basics and ARIA

### English

**Question:** What is web accessibility (a11y)? What are the key practices and when do you use ARIA?

**Answer:** Accessibility means people with disabilities (visual, motor, hearing, cognitive) can use your site, usually with assistive tools like screen readers or a keyboard only. Key practices: use semantic HTML; give images meaningful `alt` text (empty `alt=""` for decorative ones); associate every form input with a `<label>`; make everything keyboard-operable with a visible focus style; keep color contrast at least 4.5:1 for normal text (WCAG AA); set the `lang` attribute; use a logical heading order; and provide a "skip to content" link. ARIA (`role`, `aria-label`, `aria-expanded`, `aria-live`) adds meaning when native HTML cannot. The first rule of ARIA: prefer a native element over ARIA, and wrong ARIA is worse than none.

### বাংলা

**প্রশ্ন:** Web accessibility (a11y) কী? মূল অভ্যাসগুলো কী এবং ARIA কখন ব্যবহার করেন?

**উত্তর:** Accessibility মানে প্রতিবন্ধী মানুষ (দৃষ্টি, চলাচল, শ্রবণ, মনোযোগ বা বুঝতে সমস্যা) যেন আপনার সাইট ব্যবহার করতে পারেন, সাধারণত screen reader বা শুধু keyboard-এর মতো সহায়ক উপায়ে। মূল অভ্যাস: semantic HTML ব্যবহার করুন; ছবিতে অর্থপূর্ণ `alt` দিন (সাজসজ্জার ছবিতে ফাঁকা `alt=""`); প্রতিটি form input-কে `<label>`-এর সাথে যুক্ত করুন; সবকিছু keyboard দিয়ে চালানো যায় এমন করুন এবং দৃশ্যমান focus style রাখুন; সাধারণ লেখায় color contrast অন্তত ৪.৫:১ রাখুন (WCAG AA); `lang` attribute দিন; heading-এর ক্রম যুক্তিসঙ্গত রাখুন; এবং "skip to content" link দিন। ARIA (`role`, `aria-label`, `aria-expanded`, `aria-live`) তখনই ব্যবহার করুন যখন native HTML দিয়ে অর্থ বোঝানো যায় না। ARIA-র প্রথম নিয়ম: ARIA-র চেয়ে native element ভালো, আর ভুল ARIA না দেওয়ার চেয়ে খারাপ।

### Example

```html
<a href="#main" class="skip-link">Skip to content</a>

<label for="email">Email</label>
<input id="email" type="email" required aria-describedby="email-help" />
<small id="email-help">We never share your email.</small>

<img src="chart.png" alt="Sales grew 20% in 2026" />
<img src="divider.png" alt="" />
<!-- decorative -->

<button aria-expanded="false" aria-controls="menu">Menu</button>
<ul id="menu" hidden>
  ...
</ul>

<div role="status" aria-live="polite">3 items added to cart</div>
```

```css
:focus-visible {
  outline: 3px solid #1a73e8;
  outline-offset: 2px;
}
```

---

## Q5. `async` vs `defer` scripts

### English

**Question:** What is the difference between a normal `<script>`, `async` and `defer`?

**Answer:** A normal `<script>` blocks HTML parsing while it downloads and runs. `async` downloads in parallel and executes as soon as it is ready, pausing the parser at that moment; execution order is not guaranteed, so it suits independent scripts like analytics. `defer` downloads in parallel but executes only after the HTML is fully parsed, in the order written, before `DOMContentLoaded`; it suits scripts that depend on the DOM or on each other. `type="module"` scripts are deferred by default. Placing a script at the end of `<body>` is the older alternative.

### বাংলা

**প্রশ্ন:** সাধারণ `<script>`, `async` ও `defer`-এর পার্থক্য কী?

**উত্তর:** সাধারণ `<script>` নামানো ও চালানোর সময় HTML parsing আটকে রাখে। `async` parallel ভাবে নামে এবং প্রস্তুত হওয়া মাত্র চলে (সেই মুহূর্তে parser থামে); চলার ক্রম নিশ্চিত নয়, তাই analytics-এর মতো স্বাধীন script-এর জন্য উপযুক্ত। `defer` parallel ভাবে নামে কিন্তু HTML পুরোপুরি parse হওয়ার পরে, লেখার ক্রম অনুযায়ী, `DOMContentLoaded`-এর আগে চলে; DOM বা একে অপরের উপর নির্ভরশীল script-এর জন্য উপযুক্ত। `type="module"` script ডিফল্টভাবেই deferred। `<body>`-এর শেষে script রাখা এর পুরনো বিকল্প।

### Example

```html
<script src="blocking.js"></script>
<!-- blocks parsing -->
<script src="analytics.js" async></script>
<!-- independent, any order -->
<script src="vendor.js" defer></script>
<!-- runs 1st after parsing -->
<script src="app.js" defer></script>
<!-- runs 2nd, order kept -->
<script type="module" src="main.js"></script>
<!-- deferred by default -->
```

---

## Q6. Meta tags and SEO

### English

**Question:** Which `<head>` meta tags are important for SEO and responsive design?

**Answer:** Essential tags: `<meta charset="UTF-8">`; `<meta name="viewport" content="width=device-width, initial-scale=1">` for responsive layouts on mobile; a unique `<title>` (about 50-60 characters); `<meta name="description">` (about 150-160 characters, shown in search results); `<link rel="canonical">` to avoid duplicate-content issues; `<meta name="robots">` to control indexing; and Open Graph/Twitter tags (`og:title`, `og:image`) for social previews. Beyond meta tags, good SEO needs semantic headings (one `h1`), fast loading, mobile friendliness and a sitemap.

### বাংলা

**প্রশ্ন:** SEO ও responsive design-এর জন্য `<head>`-এর কোন meta tag গুলো গুরুত্বপূর্ণ?

**উত্তর:** জরুরি tag: `<meta charset="UTF-8">`; মোবাইলে responsive layout-এর জন্য `<meta name="viewport" content="width=device-width, initial-scale=1">`; অনন্য `<title>` (প্রায় ৫০-৬০ অক্ষর); `<meta name="description">` (প্রায় ১৫০-১৬০ অক্ষর, search ফলাফলে দেখায়); duplicate content সমস্যা এড়াতে `<link rel="canonical">`; indexing নিয়ন্ত্রণে `<meta name="robots">`; এবং social preview-র জন্য Open Graph/Twitter tag (`og:title`, `og:image`)। Meta tag ছাড়াও ভালো SEO-র জন্য semantic heading (একটি `h1`), দ্রুত লোড, mobile-friendly design ও sitemap দরকার।

### Example

```html
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Learn React Hooks | MyBlog</title>
  <meta
    name="description"
    content="A practical guide to React hooks with examples."
  />
  <link rel="canonical" href="https://example.com/react-hooks" />
  <meta name="robots" content="index, follow" />
  <meta property="og:title" content="Learn React Hooks" />
  <meta property="og:image" content="https://example.com/cover.jpg" />
  <meta name="twitter:card" content="summary_large_image" />
</head>
```

---

## Q7. Forms and validation

### English

**Question:** How do you build an accessible form with built-in HTML validation?

**Answer:** Wrap controls in a `<form>`, give each input a `name`, an `id` and a linked `<label for>`, choose the right `type` (`email`, `tel`, `number`, `password`), and use validation attributes: `required`, `minlength`, `maxlength`, `min`, `max`, `pattern`. Use `<fieldset>` and `<legend>` to group radio buttons, `autocomplete` to help users, and a `<button type="submit">`. Browser validation improves user experience, but always validate again on the server because client-side checks can be bypassed.

### বাংলা

**প্রশ্ন:** HTML-এর বিল্ট-ইন validation সহ accessible form কীভাবে বানাবেন?

**উত্তর:** Control গুলো `<form>`-এর ভেতরে রাখুন, প্রতিটি input-কে `name`, `id` ও যুক্ত `<label for>` দিন, সঠিক `type` বেছে নিন (`email`, `tel`, `number`, `password`), এবং validation attribute ব্যবহার করুন: `required`, `minlength`, `maxlength`, `min`, `max`, `pattern`। Radio button গোষ্ঠীবদ্ধ করতে `<fieldset>` ও `<legend>`, ব্যবহারকারীকে সাহায্য করতে `autocomplete`, এবং `<button type="submit">` ব্যবহার করুন। Browser validation ব্যবহারকারীর অভিজ্ঞতা ভালো করে, কিন্তু server-এ আবার validate করতেই হবে, কারণ client-side যাচাই এড়ানো যায়।

### Example

```html
<form action="/register" method="post">
  <label for="name">Name</label>
  <input
    id="name"
    name="name"
    type="text"
    required
    minlength="3"
    autocomplete="name"
  />

  <label for="phone">Phone</label>
  <input
    id="phone"
    name="phone"
    type="tel"
    pattern="01[3-9][0-9]{8}"
    title="e.g. 01712345678"
  />

  <fieldset>
    <legend>Gender</legend>
    <label><input type="radio" name="gender" value="male" /> Male</label>
    <label><input type="radio" name="gender" value="female" /> Female</label>
  </fieldset>

  <button type="submit">Register</button>
</form>
```

---

## Q8. `data-*` attributes

### English

**Question:** What are `data-*` attributes and how do you use them?

**Answer:** Custom data attributes let you store extra information on an HTML element without affecting its appearance or breaking validity. Name them `data-` followed by a lowercase name (for example `data-user-id`). JavaScript reads them with `element.dataset.userId` (kebab-case becomes camelCase), and CSS can target them with attribute selectors (`[data-state="open"]`). Do not store sensitive information in them, since everything is visible in the page source.

### বাংলা

**প্রশ্ন:** `data-*` attribute কী এবং কীভাবে ব্যবহার করেন?

**উত্তর:** Custom data attribute দিয়ে চেহারা না বদলে বা HTML-এর বৈধতা না ভেঙে element-এ বাড়তি তথ্য রাখা যায়। নাম `data-` দিয়ে শুরু করে ছোট হাতের নাম দিন (যেমন `data-user-id`)। JavaScript `element.dataset.userId` দিয়ে এটি পড়ে (kebab-case হয়ে যায় camelCase), আর CSS attribute selector (`[data-state="open"]`) দিয়ে এটি ধরতে পারে। গোপন তথ্য এতে রাখবেন না, কারণ পেজের source-এ সবকিছু দেখা যায়।

### Example

```html
<button data-product-id="42" data-state="closed" id="buy">Buy</button>
```

```js
const btn = document.getElementById("buy");
console.log(btn.dataset.productId); // "42"
btn.dataset.state = "open"; // updates the attribute
```

```css
button[data-state="open"] {
  background: green;
}
```

---

## Q9. Responsive images

### English

**Question:** How do you serve responsive and optimized images in HTML?

**Answer:** Use `srcset` with `sizes` so the browser picks the best file for the screen width and pixel density; use `<picture>` with `<source>` for art direction or modern formats (AVIF/WebP) with a fallback; add `loading="lazy"` for below-the-fold images; set `width` and `height` (or CSS `aspect-ratio`) to prevent layout shift (CLS); and always provide `alt`. For the hero (LCP) image, do not lazy load; consider `fetchpriority="high"`.

### বাংলা

**প্রশ্ন:** HTML-এ responsive ও optimized ছবি কীভাবে দেবেন?

**উত্তর:** `srcset` ও `sizes` ব্যবহার করুন, যাতে browser পর্দার প্রস্থ ও pixel density অনুযায়ী সেরা file বেছে নেয়; art direction বা আধুনিক format (AVIF/WebP) সহ fallback-এর জন্য `<picture>` ও `<source>` ব্যবহার করুন; পেজের নিচের অংশের ছবিতে `loading="lazy"` দিন; layout shift (CLS) আটকাতে `width` ও `height` (বা CSS `aspect-ratio`) দিন; এবং সবসময় `alt` দিন। Hero (LCP) ছবিতে lazy loading দেবেন না; `fetchpriority="high"` বিবেচনা করুন।

### Example

```html
<img
  src="photo-800.jpg"
  srcset="photo-400.jpg 400w, photo-800.jpg 800w, photo-1600.jpg 1600w"
  sizes="(max-width: 600px) 100vw, 50vw"
  width="800"
  height="600"
  alt="Mountain view"
  loading="lazy"
/>

<picture>
  <source srcset="hero.avif" type="image/avif" />
  <source srcset="hero.webp" type="image/webp" />
  <img
    src="hero.jpg"
    alt="Hero banner"
    width="1200"
    height="600"
    fetchpriority="high"
  />
</picture>
```

---

## Q10. Critical rendering path

### English

**Question:** What happens when you type a URL and the browser renders the page? What is the critical rendering path?

**Answer:** The browser resolves DNS, opens a TCP/TLS connection, sends an HTTP request and receives the HTML. Then the critical rendering path runs: the HTML is parsed into the DOM; CSS is parsed into the CSSOM; DOM and CSSOM combine into the render tree (only visible elements); layout computes sizes and positions; paint draws pixels; and compositing assembles layers. CSS is render-blocking, and synchronous JavaScript is parser-blocking. To speed this up: minimize and inline critical CSS, use `defer`/`async` for scripts, compress assets, use a CDN and preload key resources.

### বাংলা

**প্রশ্ন:** URL টাইপ করলে browser কীভাবে পেজ render করে? Critical rendering path কী?

**উত্তর:** Browser DNS খোঁজে, TCP/TLS সংযোগ খোলে, HTTP request পাঠায় এবং HTML পায়। তারপর critical rendering path চলে: HTML parse হয়ে DOM বানায়; CSS parse হয়ে CSSOM বানায়; DOM ও CSSOM মিলে render tree তৈরি করে (শুধু দৃশ্যমান element); layout মাপ ও অবস্থান হিসাব করে; paint pixel আঁকে; এবং compositing স্তরগুলো জোড়া লাগায়। CSS render আটকে রাখে, আর synchronous JavaScript parsing আটকে রাখে। গতি বাড়াতে: critical CSS ছোট করে inline করুন, script-এ `defer`/`async` দিন, asset compress করুন, CDN ব্যবহার করুন এবং গুরুত্বপূর্ণ resource preload করুন।

### Example

```html
<head>
  <style>
    /* critical above-the-fold CSS inline */
  </style>
  <link
    rel="preload"
    href="/fonts/inter.woff2"
    as="font"
    type="font/woff2"
    crossorigin
  />
  <link rel="stylesheet" href="/styles.css" />
  <script src="/app.js" defer></script>
</head>
```

---

## Q11. Reflow vs repaint

### English

**Question:** What is the difference between reflow and repaint? How do you avoid expensive ones?

**Answer:** Reflow (layout) recalculates the size and position of elements; it is triggered by changes such as width, height, margin, font size, adding/removing elements or reading layout values like `offsetHeight` after a write. Repaint redraws pixels when only appearance changes (color, background, visibility) without layout changes. Reflow is the most expensive and always causes a repaint. Changes to `transform` and `opacity` can be handled on the compositor (GPU) without reflow or repaint. To avoid layout thrashing, batch DOM reads and writes, use CSS classes instead of many inline style changes, animate with `transform`/`opacity`, and use `DocumentFragment` for bulk inserts.

### বাংলা

**প্রশ্ন:** Reflow আর repaint-এর পার্থক্য কী? ব্যয়বহুলগুলো কীভাবে এড়াবেন?

**উত্তর:** Reflow (layout) element-এর মাপ ও অবস্থান আবার হিসাব করে; width, height, margin, font size বদলালে, element যোগ/বাদ দিলে বা লেখার পর `offsetHeight`-এর মতো layout-এর মান পড়লে এটি ঘটে। Repaint শুধু চেহারা (color, background, visibility) বদলালে layout না বদলে pixel আবার আঁকে। Reflow সবচেয়ে ব্যয়বহুল এবং সবসময় repaint-ও ঘটায়। `transform` ও `opacity`-এর পরিবর্তন compositor-এ (GPU) reflow বা repaint ছাড়াই হতে পারে। Layout thrashing এড়াতে DOM পড়া ও লেখা আলাদা গুচ্ছে করুন, অনেক inline style না বদলে CSS class বদলান, `transform`/`opacity` দিয়ে animate করুন এবং একসাথে অনেক element যোগ করতে `DocumentFragment` ব্যবহার করুন।

### Example

```js
// Bad: read-write-read-write forces layout every time
items.forEach((el) => {
  el.style.width = el.parentNode.offsetWidth + "px";
});

// Good: read first, then write
const width = container.offsetWidth;
items.forEach((el) => {
  el.style.width = width + "px";
});

// Bulk insert
const frag = document.createDocumentFragment();
data.forEach((d) => {
  const li = document.createElement("li");
  li.textContent = d;
  frag.append(li);
});
list.append(frag);
```

---

## Q12. `<button>` vs `<a>`, `target="_blank"` and iframe security

### English

**Question:** When do you use `<button>` vs `<a>`? What are the security concerns with `target="_blank"` and `<iframe>`?

**Answer:** Use `<a href>` for navigation (going to another page or location), and `<button>` for actions on the current page (submit, open a modal, toggle). Buttons are keyboard and screen reader friendly by default; a clickable `<div>` is not. For `target="_blank"`, add `rel="noopener noreferrer"`: it stops the new page from accessing `window.opener` (tabnabbing) and hides the referrer; modern browsers apply `noopener` by default but being explicit is good practice. For `<iframe>`, use the `sandbox` attribute to restrict capabilities, set `referrerpolicy`, `loading="lazy"`, and a `title` for accessibility; servers can block framing with `X-Frame-Options` or CSP `frame-ancestors` to prevent clickjacking.

### বাংলা

**প্রশ্ন:** `<button>` আর `<a>` কখন ব্যবহার করবেন? `target="_blank"` ও `<iframe>`-এর নিরাপত্তা ঝুঁকি কী?

**উত্তর:** Navigation-এর (অন্য পেজ বা জায়গায় যাওয়া) জন্য `<a href>`, আর বর্তমান পেজের কাজের (submit, modal খোলা, toggle) জন্য `<button>` ব্যবহার করুন। Button ডিফল্টভাবেই keyboard ও screen reader-বান্ধব; ক্লিকযোগ্য `<div>` তা নয়। `target="_blank"`-এর সাথে `rel="noopener noreferrer"` দিন: এটি নতুন পেজকে `window.opener` ধরতে দেয় না (tabnabbing) এবং referrer লুকায়; আধুনিক browser ডিফল্টে `noopener` প্রয়োগ করে, তবুও স্পষ্ট লেখা ভালো অভ্যাস। `<iframe>`-এ ক্ষমতা সীমিত করতে `sandbox` attribute, `referrerpolicy`, `loading="lazy"` এবং accessibility-র জন্য `title` দিন; clickjacking ঠেকাতে server `X-Frame-Options` বা CSP-র `frame-ancestors` দিয়ে frame করা আটকাতে পারে।

### Example

```html
<a href="/pricing">See pricing</a>
<button type="button" id="open-modal">Open modal</button>

<a href="https://external.com" target="_blank" rel="noopener noreferrer"
  >External site</a
>

<iframe
  src="https://www.youtube.com/embed/xyz"
  title="Product demo video"
  sandbox="allow-scripts allow-same-origin"
  loading="lazy"
  referrerpolicy="no-referrer"
  allowfullscreen
></iframe>
```
