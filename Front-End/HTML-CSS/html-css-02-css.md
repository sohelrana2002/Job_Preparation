# CSS (Q13 - Q30)

> Place in: `02-frontend/html-css/02-css.md`
> HTML and accessibility questions (Q1-Q12) are in `01-html-and-accessibility.md`.

---

## Q13. Box model and `box-sizing`

### English

**Question:** Explain the CSS box model. What does `box-sizing: border-box` do?

**Answer:** Every element is a rectangular box made of four layers from inside out: content, padding, border and margin. With the default `box-sizing: content-box`, `width` and `height` apply only to the content, so padding and border are added on top of them, making the real size larger. With `box-sizing: border-box`, `width` and `height` include content, padding and border, which makes layouts much easier to calculate. Most projects reset it globally.

### বাংলা

**প্রশ্ন:** CSS box model ব্যাখ্যা করুন। `box-sizing: border-box` কী করে?

**উত্তর:** প্রতিটি element একটি আয়তাকার box, যা ভেতর থেকে বাইরে চার স্তরে তৈরি: content, padding, border ও margin। ডিফল্ট `box-sizing: content-box`-এ `width` ও `height` শুধু content-এর জন্য, তাই padding ও border তার উপর যোগ হয়ে আসল মাপ বড় হয়ে যায়। `box-sizing: border-box`-এ `width` ও `height`-এ content, padding ও border সবই ধরা হয়, ফলে layout-এর হিসাব অনেক সহজ হয়। বেশিরভাগ project-এ এটি global ভাবে reset করা হয়।

### Example

```css
.box-a {
  width: 200px;
  padding: 20px;
  border: 5px solid black;
  /* content-box: total width = 200 + 40 + 10 = 250px */
}

.box-b {
  box-sizing: border-box;
  width: 200px;
  padding: 20px;
  border: 5px solid black;
  /* border-box: total width = 200px */
}

*,
*::before,
*::after {
  box-sizing: border-box;
} /* common reset */
```

---

## Q14. Selectors and specificity

### English

**Question:** What is CSS specificity and how is it calculated?

**Answer:** Specificity decides which rule wins when several rules target the same element. Think of it as a score (inline, IDs, classes, elements): an inline style is the strongest (1,0,0,0); an ID selector counts (0,1,0,0); a class, attribute selector or pseudo-class counts (0,0,1,0); an element or pseudo-element counts (0,0,0,1); the universal selector `*` and combinators count nothing. Higher specificity wins; if equal, the later rule in the source wins. `:where()` has zero specificity and `:is()` takes the highest specificity inside it.

### বাংলা

**প্রশ্ন:** CSS specificity কী এবং কীভাবে হিসাব হয়?

**উত্তর:** একই element-কে একাধিক rule নির্দেশ করলে কোনটি জিতবে তা specificity ঠিক করে। এটিকে একটি স্কোর ভাবুন (inline, ID, class, element): inline style সবচেয়ে শক্তিশালী (1,0,0,0); ID selector (0,1,0,0); class, attribute selector বা pseudo-class (0,0,1,0); element বা pseudo-element (0,0,0,1); universal selector `*` ও combinator কিছুই যোগ করে না। বেশি specificity জেতে; সমান হলে source-এ পরের rule জেতে। `:where()`-এর specificity শূন্য, আর `:is()` তার ভেতরের সর্বোচ্চ specificity নেয়।

### Example

```css
p {
  color: black;
} /* (0,0,0,1) */
.text {
  color: blue;
} /* (0,0,1,0) */
p.text {
  color: green;
} /* (0,0,1,1) */
#intro {
  color: red;
} /* (0,1,0,0) */
div #intro.text {
  color: purple;
} /* (0,1,1,1) wins over #intro */
```

```html
<p id="intro" class="text" style="color: orange">Orange wins (inline)</p>
```

---

## Q15. Cascade, inheritance and `!important`

### English

**Question:** What is the cascade? How does inheritance work and when should you use `!important`?

**Answer:** The cascade is the algorithm that resolves conflicting declarations, in this order: importance (`!important`) and origin, then specificity, then source order (and `@layer` order). Inheritance means some properties pass from parent to child automatically (text-related ones such as `color`, `font-family`, `line-height`), while layout-related ones (`margin`, `padding`, `border`) do not. You can control it with `inherit`, `initial`, `unset` and `revert`. Avoid `!important` because it breaks the natural cascade and is hard to override; use it only for utility classes or to override third-party styles you cannot edit.

### বাংলা

**প্রশ্ন:** Cascade কী? Inheritance কীভাবে কাজ করে এবং `!important` কখন ব্যবহার করবেন?

**উত্তর:** Cascade হলো সেই algorithm যা পরস্পরবিরোধী declaration সমাধান করে, এই ক্রমে: গুরুত্ব (`!important`) ও origin, তারপর specificity, তারপর source-এর ক্রম (এবং `@layer`-এর ক্রম)। Inheritance মানে কিছু property parent থেকে child-এ নিজে থেকে যায় (লেখা-সংক্রান্ত যেমন `color`, `font-family`, `line-height`), আর layout-সংক্রান্ত property (`margin`, `padding`, `border`) যায় না। `inherit`, `initial`, `unset` ও `revert` দিয়ে এটি নিয়ন্ত্রণ করা যায়। `!important` এড়িয়ে চলুন, কারণ এটি স্বাভাবিক cascade ভেঙে দেয় এবং override করা কঠিন; শুধু utility class-এ বা সম্পাদনা করা যায় না এমন তৃতীয় পক্ষের style বদলাতে ব্যবহার করুন।

### Example

```css
body {
  color: #333;
  font-family: system-ui;
} /* inherited by children */
.card {
  border: 1px solid #ccc;
} /* NOT inherited */
.card-child {
  border: inherit;
} /* force inheritance */

.btn {
  background: blue;
}
.btn {
  background: green;
} /* later rule wins: green */

.hidden {
  display: none !important;
} /* acceptable utility use */
```

---

## Q16. `display: none` vs `visibility: hidden` vs `opacity: 0`

### English

**Question:** What is the difference between `display: none`, `visibility: hidden` and `opacity: 0`?

**Answer:** `display: none` removes the element from the layout completely (no space taken) and from the accessibility tree; it cannot be animated. `visibility: hidden` hides the element but keeps its space and removes it from the accessibility tree. `opacity: 0` makes it fully transparent but it still takes space, can still be clicked and focused, and can be animated smoothly. For hiding content only visually while keeping it for screen readers, use a "visually hidden" utility class.

### বাংলা

**প্রশ্ন:** `display: none`, `visibility: hidden` ও `opacity: 0`-এর পার্থক্য কী?

**উত্তর:** `display: none` element-কে layout থেকে পুরোপুরি সরিয়ে দেয় (কোনো জায়গা নেয় না) এবং accessibility tree থেকেও সরায়; এটি animate করা যায় না। `visibility: hidden` element লুকায় কিন্তু জায়গা ধরে রাখে এবং accessibility tree থেকে সরায়। `opacity: 0` পুরোপুরি স্বচ্ছ করে, কিন্তু জায়গা নেয়, ক্লিক ও focus করা যায়, এবং মসৃণভাবে animate করা যায়। শুধু চোখে লুকিয়ে screen reader-এর জন্য রাখতে "visually hidden" utility class ব্যবহার করুন।

### Example

```css
.a {
  display: none;
} /* gone, no space */
.b {
  visibility: hidden;
} /* invisible, space kept */
.c {
  opacity: 0;
} /* transparent, space kept, still clickable */

.sr-only {
  /* hidden visually, readable by screen readers */
  position: absolute;
  width: 1px;
  height: 1px;
  margin: -1px;
  padding: 0;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border: 0;
}
```

---

## Q17. The `position` property

### English

**Question:** Explain the values of the CSS `position` property.

**Answer:**

- `static` (default): normal flow; `top/left/z-index` have no effect.
- `relative`: stays in the flow and can be offset from its normal position; it also becomes the reference point for absolutely positioned children.
- `absolute`: removed from the flow and positioned relative to the nearest positioned ancestor (not `static`), or the page if none.
- `fixed`: removed from the flow and positioned relative to the viewport; it stays in place when scrolling.
- `sticky`: behaves like `relative` until a scroll threshold (for example `top: 0`) is reached, then sticks like `fixed` within its parent.

### বাংলা

**প্রশ্ন:** CSS `position` property-র মানগুলো ব্যাখ্যা করুন।

**উত্তর:**

- `static` (ডিফল্ট): স্বাভাবিক flow; `top/left/z-index` কাজ করে না।
- `relative`: flow-তেই থাকে এবং স্বাভাবিক অবস্থান থেকে সরানো যায়; এটি absolute child-এর রেফারেন্স বিন্দুও হয়।
- `absolute`: flow থেকে বেরিয়ে যায় এবং নিকটতম positioned ancestor-এর (যা `static` নয়) তুলনায় বসে, না থাকলে পেজের তুলনায়।
- `fixed`: flow থেকে বেরিয়ে যায় এবং viewport-এর তুলনায় বসে; scroll করলেও জায়গায় থাকে।
- `sticky`: scroll-এর নির্দিষ্ট সীমা (যেমন `top: 0`) পর্যন্ত `relative`-এর মতো চলে, তারপর parent-এর ভেতরে `fixed`-এর মতো আটকে যায়।

### Example

```css
.card {
  position: relative;
}
.card .badge {
  position: absolute;
  top: 8px;
  right: 8px;
} /* relative to .card */

.navbar {
  position: sticky;
  top: 0;
  background: white;
} /* sticks on scroll */
.chat-button {
  position: fixed;
  bottom: 16px;
  right: 16px;
}
```

---

## Q18. `z-index` and stacking context

### English

**Question:** How does `z-index` work and what is a stacking context?

**Answer:** `z-index` controls the stacking order of overlapping elements, but it only works on positioned elements (anything other than `static`) and on flex/grid items. A stacking context is an isolated group: z-index values are compared only among siblings inside the same context, so a child with `z-index: 9999` can never appear above an element outside its parent's context that has a higher-ranked context. New stacking contexts are created by the root element, `position` with a `z-index` value, `opacity` below 1, `transform`, `filter`, `isolation: isolate`, and others. Most "z-index not working" bugs come from this.

### বাংলা

**প্রশ্ন:** `z-index` কীভাবে কাজ করে এবং stacking context কী?

**উত্তর:** `z-index` উপরে-নিচে ঢেকে থাকা element-এর ক্রম নিয়ন্ত্রণ করে, কিন্তু শুধু positioned element (`static` ছাড়া) ও flex/grid item-এ কাজ করে। Stacking context হলো একটি বিচ্ছিন্ন গোষ্ঠী: z-index-এর মান শুধু একই context-এর ভেতরের সহোদরদের মধ্যে তুলনা হয়, তাই `z-index: 9999` দেওয়া child-ও তার parent-এর context-এর বাইরের উচ্চতর ক্রমের element-এর উপরে যেতে পারে না। নতুন stacking context তৈরি করে root element, `z-index` মান সহ `position`, ১-এর কম `opacity`, `transform`, `filter`, `isolation: isolate` ইত্যাদি। "z-index কাজ করছে না" সমস্যার বেশিরভাগই এখান থেকে আসে।

### Example

```css
.parent-a {
  position: relative;
  z-index: 1;
} /* creates a stacking context */
.parent-a .child {
  position: absolute;
  z-index: 9999;
}

.parent-b {
  position: relative;
  z-index: 2;
} /* higher context */
/* .child stays BELOW .parent-b content because its whole context (1) < 2 */

.card {
  transform: translateZ(0);
} /* also creates a stacking context */
.modal-root {
  isolation: isolate;
} /* clean way to make one */
```

---

## Q19. Flexbox

### English

**Question:** Explain Flexbox: main axis, cross axis and the key properties.

**Answer:** Flexbox is a one-dimensional layout model (a row or a column). Set `display: flex` on the container. `flex-direction` sets the main axis (`row` by default, or `column`); the cross axis is perpendicular. `justify-content` aligns items along the main axis; `align-items` aligns them along the cross axis; `flex-wrap` allows wrapping; `gap` sets spacing. On items: `flex-grow` (share extra space), `flex-shrink` (shrink when tight), `flex-basis` (starting size) and the shorthand `flex: 1`; `align-self` overrides alignment; `order` changes visual order.

### বাংলা

**প্রশ্ন:** Flexbox ব্যাখ্যা করুন: main axis, cross axis ও মূল property গুলো।

**উত্তর:** Flexbox একমুখী (সারি বা কলাম) layout পদ্ধতি। Container-এ `display: flex` দিন। `flex-direction` main axis ঠিক করে (ডিফল্ট `row`, বা `column`); cross axis তার লম্ব। `justify-content` main axis বরাবর item সাজায়; `align-items` cross axis বরাবর সাজায়; `flex-wrap` ভাঁজ করতে দেয়; `gap` ফাঁক ঠিক করে। Item-এর ক্ষেত্রে: `flex-grow` (বাড়তি জায়গা ভাগ), `flex-shrink` (জায়গা কম হলে ছোট হওয়া), `flex-basis` (শুরুর মাপ) এবং সংক্ষিপ্ত রূপ `flex: 1`; `align-self` আলাদা করে সাজায়; `order` চোখে দেখা ক্রম বদলায়।

### Example

```css
.navbar {
  display: flex;
  justify-content: space-between; /* main axis */
  align-items: center; /* cross axis */
  gap: 16px;
}
.sidebar {
  flex: 0 0 250px;
} /* fixed width */
.content {
  flex: 1;
} /* takes remaining space */

.cards {
  display: flex;
  flex-wrap: wrap;
  gap: 16px;
}
.cards > * {
  flex: 1 1 250px;
} /* responsive cards */
```

---

## Q20. CSS Grid and Flexbox vs Grid

### English

**Question:** What is CSS Grid? When do you choose Grid over Flexbox?

**Answer:** CSS Grid is a two-dimensional layout system (rows and columns together). You define tracks with `grid-template-columns` and `grid-template-rows`, use `fr` units for flexible space, `gap` for spacing, `grid-template-areas` for named layouts, and `repeat(auto-fit, minmax(...))` for responsive grids without media queries. Choose Flexbox for one-dimensional content-driven layouts (navbars, button groups, centering). Choose Grid for two-dimensional page or card layouts where rows and columns must align. They are often combined: Grid for the page structure, Flexbox inside components.

### বাংলা

**প্রশ্ন:** CSS Grid কী? Flexbox-এর বদলে কখন Grid বেছে নেবেন?

**উত্তর:** CSS Grid দ্বিমুখী (সারি ও কলাম একসাথে) layout ব্যবস্থা। `grid-template-columns` ও `grid-template-rows` দিয়ে track ঠিক করা হয়, নমনীয় জায়গার জন্য `fr` একক, ফাঁকের জন্য `gap`, নামযুক্ত layout-এর জন্য `grid-template-areas` এবং media query ছাড়াই responsive grid-এর জন্য `repeat(auto-fit, minmax(...))` ব্যবহার করা হয়। একমুখী ও content-চালিত layout-এ (navbar, button গোষ্ঠী, কেন্দ্রে আনা) Flexbox বেছে নিন। সারি ও কলাম মেলাতে হবে এমন দ্বিমুখী পেজ বা card layout-এ Grid বেছে নিন। প্রায়ই দুটো একসাথে ব্যবহার হয়: পেজের কাঠামোয় Grid, component-এর ভেতরে Flexbox।

### Example

```css
.layout {
  display: grid;
  grid-template-columns: 250px 1fr;
  grid-template-areas:
    "header header"
    "sidebar main"
    "footer footer";
  gap: 16px;
  min-height: 100vh;
}
.header {
  grid-area: header;
}
.sidebar {
  grid-area: sidebar;
}
.main {
  grid-area: main;
}
.footer {
  grid-area: footer;
}

.gallery {
  display: grid;
  grid-template-columns: repeat(
    auto-fit,
    minmax(220px, 1fr)
  ); /* responsive, no media query */
  gap: 12px;
}
```

---

## Q21. Responsive design and media queries

### English

**Question:** How do you build a responsive layout? What does mobile-first mean?

**Answer:** Start with the viewport meta tag, then combine fluid layouts (percentages, `fr`, Flexbox/Grid), flexible media (`max-width: 100%`), relative units and media queries to adapt at breakpoints. Mobile-first means writing base styles for small screens and enlarging with `min-width` queries, which keeps CSS lighter and prioritizes performance on phones. Choose breakpoints based on where the content breaks, not specific devices. Modern tools such as `clamp()`, container queries and `auto-fit` grids reduce the need for many breakpoints.

### বাংলা

**প্রশ্ন:** Responsive layout কীভাবে বানান? Mobile-first মানে কী?

**উত্তর:** Viewport meta tag দিয়ে শুরু করুন, তারপর fluid layout (শতাংশ, `fr`, Flexbox/Grid), নমনীয় media (`max-width: 100%`), আপেক্ষিক একক এবং breakpoint-এ খাপ খাওয়াতে media query মেলান। Mobile-first মানে ছোট পর্দার জন্য মূল style লেখা এবং `min-width` query দিয়ে বড় করা; এতে CSS হালকা থাকে এবং ফোনের performance অগ্রাধিকার পায়। নির্দিষ্ট device নয়, content যেখানে ভেঙে যায় সেই অনুযায়ী breakpoint বেছে নিন। `clamp()`, container query ও `auto-fit` grid-এর মতো আধুনিক উপায় অনেক breakpoint-এর প্রয়োজন কমায়।

### Example

```css
img {
  max-width: 100%;
  height: auto;
}

.container {
  padding: 16px;
} /* mobile base */
.grid {
  display: grid;
  gap: 16px;
  grid-template-columns: 1fr;
}

@media (min-width: 768px) {
  /* tablet and up */
  .grid {
    grid-template-columns: repeat(2, 1fr);
  }
}
@media (min-width: 1024px) {
  /* desktop */
  .container {
    max-width: 1100px;
    margin-inline: auto;
  }
  .grid {
    grid-template-columns: repeat(3, 1fr);
  }
}

h1 {
  font-size: clamp(1.5rem, 4vw, 3rem);
} /* fluid typography */
```

---

## Q22. CSS units

### English

**Question:** What is the difference between `px`, `em`, `rem`, `%`, `vh/vw` and `dvh`?

**Answer:** `px` is an absolute unit. `em` is relative to the element's own font size (for font-size, relative to the parent's), so nested values compound. `rem` is relative to the root (`html`) font size, which is predictable and good for accessibility and spacing. `%` is relative to a parent's value (the property depends on the case). `vw`/`vh` are 1% of the viewport width/height. On mobile, `100vh` can be taller than the visible area because of the browser toolbar; `dvh` (dynamic viewport height) fixes this. `ch` is the width of the "0" character, handy for text width limits.

### বাংলা

**প্রশ্ন:** `px`, `em`, `rem`, `%`, `vh/vw` ও `dvh`-এর পার্থক্য কী?

**উত্তর:** `px` একটি পরম একক। `em` element-এর নিজের font size-এর তুলনায় (font-size-এর ক্ষেত্রে parent-এর তুলনায়), তাই nested হলে মান গুণ হতে থাকে। `rem` root (`html`) font size-এর তুলনায়, যা অনুমানযোগ্য এবং accessibility ও spacing-এর জন্য ভালো। `%` parent-এর মানের তুলনায় (কোন property-র মান তা ক্ষেত্রভেদে বদলায়)। `vw`/`vh` viewport-এর প্রস্থ/উচ্চতার ১%। মোবাইলে browser toolbar-এর কারণে `100vh` দৃশ্যমান অংশের চেয়ে বড় হতে পারে; `dvh` (dynamic viewport height) এটি ঠিক করে। `ch` হলো "0" অক্ষরের প্রস্থ, লেখার প্রস্থ সীমিত করতে কাজে লাগে।

### Example

```css
html {
  font-size: 100%;
} /* respects the user's browser setting */
body {
  font-size: 1rem;
} /* 16px by default */
h1 {
  font-size: 2rem;
  margin-bottom: 1.5rem;
}

.parent {
  font-size: 20px;
}
.parent .child {
  padding: 1em;
} /* 20px, relative to its own font size */

.hero {
  min-height: 100dvh;
} /* full screen on mobile too */
.article {
  max-width: 65ch;
} /* comfortable line length */
```

---

## Q23. Pseudo-classes vs pseudo-elements

### English

**Question:** What is the difference between pseudo-classes and pseudo-elements?

**Answer:** A pseudo-class (single colon) selects an element in a particular state or position: `:hover`, `:focus`, `:focus-visible`, `:checked`, `:disabled`, `:first-child`, `:nth-child(n)`, `:not()`, `:has()`. A pseudo-element (double colon) styles a specific part of an element or inserts generated content: `::before`, `::after`, `::first-line`, `::placeholder`, `::selection`. `::before` and `::after` require the `content` property and are inline by default.

### বাংলা

**প্রশ্ন:** Pseudo-class আর pseudo-element-এর পার্থক্য কী?

**উত্তর:** Pseudo-class (একটি কোলন) element-কে কোনো নির্দিষ্ট অবস্থা বা অবস্থানে ধরে: `:hover`, `:focus`, `:focus-visible`, `:checked`, `:disabled`, `:first-child`, `:nth-child(n)`, `:not()`, `:has()`। Pseudo-element (দুটি কোলন) element-এর একটি নির্দিষ্ট অংশে style দেয় বা তৈরি করা content বসায়: `::before`, `::after`, `::first-line`, `::placeholder`, `::selection`। `::before` ও `::after`-এ `content` property লাগে এবং ডিফল্টভাবে inline।

### Example

```css
a:hover {
  color: crimson;
}
input:focus-visible {
  outline: 2px solid dodgerblue;
}
li:nth-child(odd) {
  background: #f5f5f5;
}
button:not(:disabled):hover {
  transform: translateY(-1px);
}
.card:has(img) {
  padding: 0;
} /* parent selector */

.required::after {
  content: " *";
  color: red;
}
input::placeholder {
  color: #999;
}
::selection {
  background: #ffe58f;
}
```

---

## Q24. Centering an element

### English

**Question:** How do you center an element horizontally and vertically in CSS?

**Answer:** The modern ways: with Flexbox, `display: flex; justify-content: center; align-items: center` on the parent; with Grid, `display: grid; place-items: center` on the parent. For horizontal-only centering of a block with a set width, use `margin-inline: auto`; for inline content, `text-align: center`. The older absolute method is `position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%)`.

### বাংলা

**প্রশ্ন:** CSS-এ কোনো element-কে আনুভূমিক ও উল্লম্বভাবে মাঝখানে কীভাবে আনবেন?

**উত্তর:** আধুনিক উপায়: Flexbox-এ parent-এ `display: flex; justify-content: center; align-items: center`; Grid-এ parent-এ `display: grid; place-items: center`। নির্দিষ্ট width-এর block শুধু আনুভূমিকভাবে মাঝে আনতে `margin-inline: auto`; inline content-এর জন্য `text-align: center`। পুরনো absolute পদ্ধতি হলো `position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%)`।

### Example

```css
.flex-center {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
}
.grid-center {
  display: grid;
  place-items: center;
  min-height: 100vh;
}

.box {
  width: 600px;
  margin-inline: auto;
} /* horizontal only */

.modal {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
```

---

## Q25. CSS variables and dark mode

### English

**Question:** What are CSS custom properties (variables)? How would you implement dark mode?

**Answer:** CSS variables are declared with `--name` and used with `var(--name, fallback)`. Unlike Sass variables they are live in the browser: they follow the cascade and inheritance, can be changed at runtime with JavaScript, and can be scoped to any element. For dark mode, define color tokens on `:root`, override them under `@media (prefers-color-scheme: dark)` and/or a `[data-theme="dark"]` attribute toggled by a button (saving the choice in `localStorage`), and use the variables everywhere. The `color-scheme` property also adjusts built-in form controls and scrollbars.

### বাংলা

**প্রশ্ন:** CSS custom property (variable) কী? Dark mode কীভাবে বানাবেন?

**উত্তর:** CSS variable `--name` দিয়ে declare করা হয় এবং `var(--name, fallback)` দিয়ে ব্যবহার হয়। Sass variable-এর মতো নয়, এগুলো browser-এ জীবন্ত: cascade ও inheritance মানে, JavaScript দিয়ে runtime-এ বদলানো যায় এবং যেকোনো element-এ সীমাবদ্ধ করা যায়। Dark mode-এর জন্য `:root`-এ রঙের token ঠিক করুন, `@media (prefers-color-scheme: dark)` এবং/অথবা button দিয়ে বদলানো `[data-theme="dark"]` attribute-এ সেগুলো override করুন (পছন্দ `localStorage`-এ সংরক্ষণ করে), এবং সব জায়গায় variable ব্যবহার করুন। `color-scheme` property বিল্ট-ইন form control ও scrollbar-ও সামঞ্জস্য করে।

### Example

```css
:root {
  --bg: #ffffff;
  --text: #1a1a1a;
  --primary: #2563eb;
  color-scheme: light dark;
}
@media (prefers-color-scheme: dark) {
  :root {
    --bg: #0f172a;
    --text: #e2e8f0;
  }
}
[data-theme="dark"] {
  --bg: #0f172a;
  --text: #e2e8f0;
}

body {
  background: var(--bg);
  color: var(--text);
}
.btn {
  background: var(--primary, blue);
}
```

```js
document.documentElement.dataset.theme = "dark";
document.documentElement.style.setProperty("--primary", "#16a34a");
```

---

## Q26. Transitions, animations and performance

### English

**Question:** What is the difference between CSS transitions and animations? How do you keep them smooth?

**Answer:** A transition animates between two states when a property changes (for example on hover), defined with `transition: property duration easing delay`. An animation uses `@keyframes` for multi-step sequences that can run automatically, loop and reverse, with `animation-*` properties. For smooth 60fps animation, animate only `transform` and `opacity`, which are handled by the GPU compositor without triggering layout or paint; avoid animating `width`, `height`, `top` or `margin`. Use `will-change` sparingly, and respect users who prefer less motion with `@media (prefers-reduced-motion: reduce)`.

### বাংলা

**প্রশ্ন:** CSS transition আর animation-এর পার্থক্য কী? কীভাবে মসৃণ রাখবেন?

**উত্তর:** Transition কোনো property বদলালে (যেমন hover-এ) দুই অবস্থার মধ্যে animate করে, `transition: property duration easing delay` দিয়ে ঠিক করা হয়। Animation `@keyframes` দিয়ে বহু ধাপের ক্রম বানায়, যা নিজে থেকে চলতে, বারবার ঘুরতে ও উল্টাতে পারে, `animation-*` property দিয়ে নিয়ন্ত্রিত। ৬০fps মসৃণ animation-এর জন্য শুধু `transform` ও `opacity` animate করুন, কারণ এগুলো GPU compositor layout বা paint ছাড়াই সামলায়; `width`, `height`, `top` বা `margin` animate করা এড়ান। `will-change` অল্প ব্যবহার করুন, এবং `@media (prefers-reduced-motion: reduce)` দিয়ে কম নড়াচড়া পছন্দ করা ব্যবহারকারীকে সম্মান করুন।

### Example

```css
.btn {
  transition:
    transform 0.2s ease,
    background-color 0.2s ease;
}
.btn:hover {
  transform: translateY(-2px) scale(1.02);
}

@keyframes fade-in-up {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
.card {
  animation: fade-in-up 0.5s ease-out both;
}

@media (prefers-reduced-motion: reduce) {
  * {
    animation: none !important;
    transition: none !important;
  }
}
```

---

## Q27. Margin collapsing

### English

**Question:** What is margin collapsing?

**Answer:** Margin collapsing is when the vertical margins of adjacent block elements combine into a single margin equal to the larger of the two, instead of adding up. It happens between adjacent siblings, between a parent and its first or last child when there is no padding, border or content separating them, and for empty blocks. It does not happen with horizontal margins, in flex or grid containers, with floated or absolutely positioned elements, or when a new block formatting context is created (for example `overflow: hidden` or `display: flow-root` on the parent).

### বাংলা

**প্রশ্ন:** Margin collapsing কী?

**উত্তর:** Margin collapsing হলো পাশাপাশি block element-এর উপর-নিচের margin যোগ না হয়ে মিলে গিয়ে দুটির মধ্যে বড়টির সমান একটিমাত্র margin হওয়া। এটি ঘটে পাশাপাশি সহোদর element-এর মধ্যে, parent ও তার প্রথম বা শেষ child-এর মধ্যে (যখন মাঝে padding, border বা content নেই), এবং ফাঁকা block-এ। আনুভূমিক margin-এ, flex বা grid container-এ, float বা absolute positioned element-এ, এবং নতুন block formatting context তৈরি হলে (যেমন parent-এ `overflow: hidden` বা `display: flow-root`) এটি ঘটে না।

### Example

```css
.a {
  margin-bottom: 40px;
}
.b {
  margin-top: 20px;
}
/* gap between .a and .b is 40px, not 60px */

.parent {
  display: flow-root;
} /* stops child margin leaking out of the parent */
.stack {
  display: flex;
  flex-direction: column;
  gap: 20px;
} /* no collapsing, predictable */
```

---

## Q28. CSS approaches: Tailwind, CSS Modules, BEM

### English

**Question:** What are the common ways to organize and write CSS? Compare Tailwind, CSS Modules, BEM and CSS-in-JS.

**Answer:**

- **BEM** (Block\_\_Element--Modifier): a naming convention for plain CSS that keeps specificity flat and class names predictable.
- **CSS Modules**: regular CSS files whose class names are locally scoped at build time, avoiding global conflicts; no runtime cost.
- **Tailwind CSS**: utility-first classes directly in markup, fast to build, consistent design tokens and small production CSS through purging; trade-offs are long class lists and a learning curve (solve with component abstraction).
- **CSS-in-JS** (styled-components, Emotion): styles in JavaScript with dynamic props; adds runtime cost and works less well with Server Components (zero-runtime options like vanilla-extract exist).
- **Sass/SCSS**: preprocessor with variables, nesting and mixins; native CSS now covers much of it (variables, nesting).

### বাংলা

**প্রশ্ন:** CSS সাজানো ও লেখার প্রচলিত উপায়গুলো কী? Tailwind, CSS Modules, BEM ও CSS-in-JS তুলনা করুন।

**উত্তর:**

- **BEM** (Block\_\_Element--Modifier): সাধারণ CSS-এর নামকরণ নিয়ম, যা specificity সমতল ও class-এর নাম অনুমানযোগ্য রাখে।
- **CSS Modules**: সাধারণ CSS file, যার class-এর নাম build-এর সময় local scope পায়, ফলে global সংঘর্ষ হয় না; runtime খরচ নেই।
- **Tailwind CSS**: markup-এই utility-ভিত্তিক class, দ্রুত বানানো যায়, design token সামঞ্জস্যপূর্ণ থাকে এবং purge হয়ে production CSS ছোট হয়; ত্রুটি হলো লম্বা class তালিকা ও শেখার সময় (component বানিয়ে সমাধান করা যায়)।
- **CSS-in-JS** (styled-components, Emotion): JavaScript-এ style লেখা, dynamic props সহ; runtime খরচ যোগ হয় এবং Server Component-এর সাথে কম মানায় (vanilla-extract-এর মতো zero-runtime বিকল্প আছে)।
- **Sass/SCSS**: variable, nesting ও mixin সহ preprocessor; native CSS এখন এর অনেকটাই দেয় (variable, nesting)।

### Example

```html
<!-- BEM -->
<button class="card__button card__button--primary">Buy</button>

<!-- Tailwind -->
<button class="rounded-lg bg-blue-600 px-4 py-2 text-white hover:bg-blue-700">
  Buy
</button>
```

```css
/* Button.module.css  (CSS Modules) */
.primary {
  background: royalblue;
  color: white;
}
```

```jsx
import styles from "./Button.module.css";
<button className={styles.primary}>Buy</button>;
```

---

## Q29. Overflow and text truncation

### English

**Question:** How do you handle overflowing content and truncate text with an ellipsis?

**Answer:** The `overflow` property controls content that exceeds its box: `visible` (default), `hidden` (clip), `scroll`, and `auto` (scrollbar only when needed); `overflow-x` and `overflow-y` control each axis. For single-line truncation, combine `white-space: nowrap`, `overflow: hidden` and `text-overflow: ellipsis` (the element needs a constrained width). For multi-line truncation, use `-webkit-line-clamp` (supported in all modern browsers) or the newer `line-clamp`. In flex children, add `min-width: 0` so the text can actually shrink. Use `overflow-wrap: anywhere` for long unbroken strings such as URLs.

### বাংলা

**প্রশ্ন:** উপচে পড়া content কীভাবে সামলান এবং লেখা ellipsis দিয়ে কীভাবে ছোট করেন?

**উত্তর:** `overflow` property box-এর বাইরে চলে যাওয়া content নিয়ন্ত্রণ করে: `visible` (ডিফল্ট), `hidden` (কেটে ফেলা), `scroll` এবং `auto` (দরকার হলেই scrollbar); `overflow-x` ও `overflow-y` প্রতিটি অক্ষ আলাদা নিয়ন্ত্রণ করে। এক লাইনে ছোট করতে `white-space: nowrap`, `overflow: hidden` ও `text-overflow: ellipsis` একসাথে ব্যবহার করুন (element-এর একটি সীমিত width থাকতে হবে)। একাধিক লাইনে ছোট করতে `-webkit-line-clamp` (সব আধুনিক browser সমর্থন করে) বা নতুন `line-clamp` ব্যবহার করুন। Flex child-এ `min-width: 0` দিন, নইলে লেখা আসলে ছোট হতে পারে না। URL-এর মতো ভাঙা যায় না এমন লম্বা string-এ `overflow-wrap: anywhere` ব্যবহার করুন।

### Example

```css
.truncate {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  max-width: 250px;
}

.clamp-3 {
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: 3;
  overflow: hidden;
}

.flex-row > .title {
  min-width: 0;
} /* lets truncation work inside flex */
.url {
  overflow-wrap: anywhere;
}
.table-wrapper {
  overflow-x: auto;
} /* horizontal scroll for wide tables */
```

---

## Q30. Modern CSS features

### English

**Question:** What modern CSS features should a developer know today?

**Answer:** Notable features: CSS variables; `clamp()`, `min()`, `max()` for fluid sizing; `aspect-ratio`; `gap` in Flexbox/Grid; container queries (`@container`) so components respond to their container size instead of the viewport; the `:has()` parent selector; native CSS nesting; cascade layers (`@layer`) to manage specificity; logical properties (`margin-inline`, `padding-block`) for right-to-left support; `dvh/svh/lvh` viewport units; `color-mix()` and `oklch()` colors; subgrid; scroll snap; and `view transitions`. Check browser support (caniuse.com) before relying on newer features, and use `@supports` for fallbacks.

### বাংলা

**প্রশ্ন:** আজকের দিনে একজন developer-এর কোন আধুনিক CSS feature জানা উচিত?

**উত্তর:** উল্লেখযোগ্য feature: CSS variable; নমনীয় মাপের জন্য `clamp()`, `min()`, `max()`; `aspect-ratio`; Flexbox/Grid-এ `gap`; container query (`@container`), যাতে component viewport-এর বদলে নিজের container-এর মাপ অনুযায়ী সাড়া দেয়; `:has()` parent selector; native CSS nesting; specificity সামলাতে cascade layer (`@layer`); ডান-থেকে-বাম ভাষা সমর্থনের জন্য logical property (`margin-inline`, `padding-block`); `dvh/svh/lvh` viewport একক; `color-mix()` ও `oklch()` রং; subgrid; scroll snap; এবং view transition। নতুন feature-এর উপর নির্ভর করার আগে browser সমর্থন (caniuse.com) দেখে নিন, আর fallback-এর জন্য `@supports` ব্যবহার করুন।

### Example

```css
.card-wrapper {
  container-type: inline-size;
}
@container (min-width: 400px) {
  .card {
    display: grid;
    grid-template-columns: 150px 1fr;
  }
}

.video {
  aspect-ratio: 16 / 9;
  width: 100%;
}
.section {
  padding-block: clamp(2rem, 6vw, 5rem);
  margin-inline: auto;
}

.btn {
  background: blue;
  &:hover {
    background: darkblue;
  } /* native nesting */
}

@layer reset, base, components, utilities;

@supports (container-type: inline-size) {
  /* use container queries */
}
```
