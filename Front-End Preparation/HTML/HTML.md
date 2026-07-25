# HTML Interview Question (Front-End)

## Q1: What is HTML and what does it stand for? (Easy)

**Answer:** HTML stands for HyperText Markup Language. It is the standard markup language used to create the structure of web pages. It uses elements (tags) to define headings, paragraps, links, images, and other content.

**Example:**

```html
<!DOCTYPE html>
<html>
  <head>
    <title>My Page</title>
  </head>
  <body>
    <h1>Hello world!</h1>
    <p>This is a paragraph</p>
  </body>
</html>
```

## Q1 (বাংলা): HTML কী এবং এটি কীসের সংক্ষিপ্ত রূপ? (সহজ)

**উত্তর:** HTML হলো HyperText Markup Language-এর সংক্ষিপ্ত রূপ। এটি ওয়েব পেজের গঠন তৈরি করার জন্য ব্যবহৃত স্ট্যান্ডার্ড মার্কআপ ভাষা। এটি এলিমেন্ট (ট্যাগ) ব্যবহার করে হেডিং, প্যারাগ্রাফ, লিংক, ইমেজ এবং অন্যান্য কন্টেন্ট সংজ্ঞায়িত করে।

---

## Q2: What are the Semantic Elements in HTML? (Easy)

**Answer:** Semantic elements clearly describe their meaning to both the browser and the developer. Example include `<article>, <section>, <nav>, <header>, <footer>, and <aside>`. They improve SEO and accessibility.

**Example:**

```html
<article>
  <header>
    <h1>Blog Title</h1>
  </header>
  <p>Content goes here.</p>
  <footer>Published 2025</footer>
</article>
```

## Q2 (বাংলা): HTML-এ সিম্যান্টিক এলিমেন্ট কী? (সহজ)

**উত্তর:** সিম্যান্টিক এলিমেন্টগুলো তাদের অর্থ ব্রাউজার এবং ডেভেলপার উভয়ের কাছে পরিষ্কারভাবে বোঝায়। যেমন: `<article>, <section>, <nav>, <header>, <footer>, <aside>`। এগুলো SEO এবং অ্যাক্সেসিবিলিটি উন্নত করে।

---

## Q3: What is the difference between HTML and HTML 5? (Easy)

**Answer:** HTML is the older version of hypertext markup language, whereas HTML5 is the latest version. HTML5 introduced new features such as native audio and video support `(<audio>, <video>)`, semantic elements `(<article>, <header>, <footer>)`, vectors graphics support `(<svg>)`, and local storage capabilities `(localStorage, sessionStorage)` without relying on external plugins like Flash

**Example:**

```html
<!-- HTML5 Semantic & Media Structure -->
<header>
  <h1>My Website</h1>
</header>
<main>
  <video src="video.mp4" controls>Play</video>
</main>
```

## Q3 (বাংলা): HTML এবং HTML5 এর মধ্যে পার্থক্য কী? (সহজ)

**উত্তর:** HTML হলো হাইপারটেক্সট মার্কআপ ল্যাঙ্গুয়েজের পুরোনো সংস্করণ, আর HTML5 হলো এর সর্বশেষ এবং আধুনিক সংস্করণ। HTML5-এ বেশ কিছু নতুন ফিচার যুক্ত করা হয়েছে যেমন: নেটিভ অডিও ও ভিডিও সাপোর্ট `(<audio>, <video>)`, সেমান্টিক এলিমেন্টস `(<article>, <header>, <footer>)`, ভেক্টর গ্রাফিক্স সাপোর্ট `(<svg>)`, এবং ফ্ল্যাশ বা অন্য কোনো এক্সটার্নাল প্লাগইন ছাড়াই ব্রাউজারে ডেটা সেভ করার জন্য লোকাল স্টোরেজ সুবিধা

---

## Q4: What are semantic elements in HTML? Why are they importand? (Easy)

**Answer:** Semantic elements are elements that clearly describe their meaning to both the browser and the developer `(e.g., <form>, <table>, <article>)`. Non-semantic elements tell nothing about their content `(e.g., <div> and <span>)`.

They are important because:

1. **SEO(Search Engine Optimization):** Search engines understand the page structure better.
2. **Accessibility(a11y):** Screen readers use them to help visually impaired users navigate.
3. **Maintainability:** Makes the code much easier for developers to read.

**Example:**

```html
<!-- Non-Semantic (Bad Practice) -->
<div class="footer">© 2026 Company</div>

<!-- Semantic (Good Practice) -->
<footer>© 2026 Company</footer>
```

## Q4 (বাংলা): HTML-এ সেমান্টিক এলিমেন্ট (Semantic Elements) কী? এগুলো কেন গুরুত্বপূর্ণ? (সহজ)

**উত্তর:** সেমান্টিক এলিমেন্ট হলো এমন কিছু ট্যাগ যা ব্রাউজার এবং ডেভেলপার উভয়ের কাছেই তার ভেতরের কনটেন্ট বা অর্থ স্পষ্টভাবে প্রকাশ করে `(যেমন: <form>, <table>, <article>)`। অন্যদিকে নন-সেমান্টিক এলিমেন্ট তার ভেতরের কনটেন্ট সম্পর্কে কোনো ধারণা দেয় না `(যেমন: <div> এবং <span>)`।

এগুলোর গুরুত্ব হলো:

1. **SEO(Search Engine Optimization):** সার্চ ইঞ্জিনগুলো পেজের স্ট্রাকচার সহজে বুঝতে পারে।
2. **Accessibility(a11y):** স্ক্রিন রিডার ব্যবহারকারী (দৃষ্টিপ্রতিবন্ধী) ব্যক্তিরা সহজে ওয়েবসাইট নেভিগেট করতে পারেন।
3. **Maintainability:** কোড সহজে পড়া এবং মেইনটেইন করা যায়।

---

## Q5: What are the difference between Block-level and Inline elements? (Easy)

**Answer:**

- **Block-level elements:** Always start on a new line and take up the full width available (stretches out to the left and right as far as it can). Example: `<div>, <p>, <h1>-<h6>, <ul>, <li>`.

- **Inline elements:** Do not start on a new line and only take up as much width as necessary. Example: `<span>, <a>, <strong>, <em>, <img>`.

**Example:**

```html
<!-- Block element takes whole row -->
<p style="background-color: yellow">This is a paragraph.</p>
<!-- Inline element takes only content width -->
<span style="background-color: cyan"> This is a span. </span>
```

## Q5 (বাংলা): Block-level এবং Inline এলিমেন্টের মধ্যে পার্থক্য কী? (সহজ)

**উত্তর:**

- **Block-level elements:** এগুলো সবসময় নতুন লাইন থেকে শুরু হয় এবং স্ক্রিনের পুরো অ্যাভেইলেবল উইডথ (ডানে-বামে যতটুকু জায়গা আছে) দখল করে নেয়। উদাহরণ: `<div>, <p>, <h1>-<h6>, <ul>, <li>`।

- **Inline elements:** এগুলো নতুন লাইন থেকে শুরু হয় না এবং এদের কনটেন্টের জন্য যতটুকু জায়গা দরকার ঠিক ততটুকুই দখল করে। উদাহরণ: `<span>, <a>, <strong>, <em>, <img>`।

---

## Q6: What is an HTML Boilerplate? (Easy)

**Answer:** An HTML Boilerplate is the basic template or skeleton of and HTML document.

It contains the minimum required elements needed to create a valid HTML page.

**Example:**

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>My Website</title>
  </head>
  <body>
    <h1>Hello World</h1>
  </body>
</html>
```

Interview Notes:

- Every HTML document starts with a boilerplate.
- It provides the browser with the page structure.

## Q6 (বাংলা): HTML Boilerplate কী? (সহজ)

**উত্তর:**
HTML Boilerplate হলো একটি HTML Document-এর Basic Structure বা Skeleton।

এটি একটি Valid Web Page তৈরির জন্য প্রয়োজনীয় Minimum Structure প্রদান করে।

---

## Q7: What is the purpose of a Boilerplate? (Easy)

**Answer:**
A Boilerplate:

- Provides a standard structure.
- Helps browsers understand the document.
- Prevents common HTML issues.
- Improves maintainability.

## Q7 (বাংলা): Boilerplate কেন ব্যবহার করা হয়? (সহজ)

**উত্তর:**
Boilerplate ব্যবহার করা হয়:

- Standard Structure তৈরির জন্য।
- Browser-কে Document বুঝতে সাহায্য করার জন্য।
- HTML Error কমানোর জন্য।
- Code Maintainable রাখার জন্য।

---

## Q8: What is `<!DOCTYPE html>`? (Easy)

**Answer:**
`<!DOCTYPE html>` is a declaration that tells the browser the document is written using HTML5.

It is not an HTML tag.

Why Important?

- Without it, browsers may enter Quirks Mode.

Interview Notes:

- DOCTYPE is a declaration.
- Not an HTML element.
- Must be the first line.

## Q8 (বাংলা): `<!DOCTYPE html>` কী? (সহজ)

**উত্তর:**
এটি Browser-কে জানায় যে Document টি HTML5 ব্যবহার করছে।

এটি কোনো HTML Tag নয়।

---

## Q9: What is Quirks Mode? (Easy)

**Answer:**
Quirks Mode is a browser compatibility mode that mimics old browser behavior.

If DOCTYPE is missing, browsers may render pages differently.

## Q9 (বাংলা): Quirks Mode কী? (সহজ)

**উত্তর:**
Quirks Mode হলো Browser-এর একটি Compatibility Mode যেখানে Browser পুরোনো নিয়ম অনুসরণ করে Page Render করে।

---

## Q10: What is the `<head>` tag? (Easy)

**Answer:**
The `<head>` element contains metadata about the document.

It is not visible on the webpage.

**Example:**

```html
<head>
  <title>My Website</title>
</head>
```

Common Content:

- title
- meta
- style
- script
- link

## Q10 (বাংলা): `<head>` Tag কী? (সহজ)

**উত্তর:**
`<head>` Element-এর ভিতরে Document সম্পর্কিত তথ্য থাকে।

এগুলো Browser ব্যবহার করে কিন্তু User সরাসরি দেখতে পায় না।

---

## Q11: What is metadata? (Easy)

**Answer:**
Metadata is data about data.

It provides information about the web page.

**Example:**

```html
<meta charset="UTF-8" />
```

## Q11 (বাংলা): Metadata কী? (সহজ)

**উত্তর:**
Metadata হলো Data সম্পর্কে তথ্য।

এটি Browser ও Search Engine-কে Page সম্পর্কে তথ্য দেয়।

---

## Q12: What is the `<title>` tag? (Easy)

**Answer:**
The `<title>` tag defines the title of the webpage.

**Example:**

```html
<title>My Portfolio</title>
```

Displayed In:

- Browser Tab
- Search Engine Results
- Bookmarks
- Very important for SEO.

## Q12 (বাংলা): `<title>` Tag কী? (সহজ)

**উত্তর:**
Web Page-এর Title নির্ধারণ করে।

এটি Browser Tab এবং Google Search Result-এ দেখা যায়।

---

## Q13: Can a page have multiple title tags? (Easy)

**Answer:**
No.

Only one title tag should exist.

**Example:**

```html
<!-- Wrong -->
<title>One</title> <title>Two</title>
```

## Q13 (বাংলা): একাধিক Title Tag ব্যবহার করা যায়? (সহজ)

**উত্তর:**
না।

একটি Page-এ একটি Title Tag ব্যবহার করা উচিত।

---

## Q14: What is UTF-8? (Easy)

**Answer:**
UTF-8 is a character encoding standard.

It supports almost all languages.

- UTF stands for Unicode Transformation Format.

**Example:**

```html
<meta charset="UTF-8" />
```

## Q14 (বাংলা): UTF-8 কী? (সহজ)

**উত্তর:**
UTF-8 হলো Character Encoding Standard।

এটি প্রায় সব ভাষা Support করে।

---

## Q15: Why should we use UTF-8? (Easy)

**Answer:**
Without UTF-8:

- Characters may break.
- Symbols may appear incorrectly.

**Example:**

- Bangla, Arabic, Chinese characters may not display correctly.

## Q15 (বাংলা): UTF-8 কেন ব্যবহার করা হয়? (সহজ)

**উত্তর:**
সঠিকভাবে বিভিন্ন ভাষার Character দেখানোর জন্য।

---

## Q16: What is the viewport meta tag? (Easy)

**Answer:**
It controls how a webpage is displayed on mobile devices.

**Example:**

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
```

Benefits:

- Responsive Design
- Mobile Friendly Layout

## Q16 (বাংলা): Viewport Meta Tag কী? (সহজ)

**উত্তর:**
Mobile Device-এ Website সঠিকভাবে দেখানোর জন্য Viewport Meta Tag ব্যবহার করা হয়।

---

## Q17: What is DOM? (Easy)

**Answer:**
DOM stands for Document Object Model.

The browser converts HTML into a tree-like structure.

**Example:**

```html
<body>
  <h1>Hello</h1>
</body>
```

DOM:

Document -> html -> body ->h1

## Q17 (বাংলা): DOM কী? (সহজ)

**উত্তর:**
DOM হলো Browser দ্বারা তৈরি একটি Tree Structure যেখানে HTML Elements Node হিসেবে থাকে।

---

## Q18: How does a browser read HTML? (Easy)

**Answer:**
Steps:

1. Download HTML
2. Parse HTML
3. Build DOM
4. Load CSS
5. Build Render Tree
6. Paint Screen

## Q18 (বাংলা): Browser কীভাবে HTML পড়ে? (সহজ)

**উত্তর:**
ধাপগুলো:

1. HTML Download করে
2. Parse করে
3. DOM তৈরি করে
4. CSS Load করে
5. Render Tree তৈরি করে
6. Screen-এ দেখায়

---

## Q19: What is HTML Parsing? (Easy)

**Answer:**
Parsing is the process of reading and interpreting HTML code.

The browser converts HTML into DOM nodes.

## Q19 (বাংলা): HTML Parsing কী? (সহজ)

**উত্তর:**
Browser যখন HTML Code পড়ে DOM তৈরি করে সেই Process-কে Parsing বলে।

---
