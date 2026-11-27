# Junior Frontend Developer — Interview & Short Test Preparation

## ১. JavaScript গুরুত্বপূর্ণ প্রশ্ন ও উত্তর

### প্রশ্ন ১: var, let এবং const-এর মধ্যে পার্থক্য কী?

**উত্তর:**

- `var` function-scoped এবং একই scope-এ পুনরায় declare করা যায়।
- `let` block-scoped এবং পুনরায় declare করা যায় না।
- `const` block-scoped এবং পুনরায় assign করা যায় না।
- আধুনিক JavaScript-এ সাধারণত `const` এবং প্রয়োজনে `let` ব্যবহার করা হয়।

### প্রশ্ন ২: == এবং === এর মধ্যে পার্থক্য কী?

**উত্তর:**

- `==` শুধু value তুলনা করার আগে প্রয়োজনে type conversion করে।
- `===` value এবং data type দুটোই তুলনা করে।

```js
5 == "5"; // true
5 === "5"; // false
```

সাধারণত অনাকাঙ্ক্ষিত type conversion এড়াতে `===` ব্যবহার করা উচিত।

### প্রশ্ন ৩: Hoisting কী?

**উত্তর:**

JavaScript-এ Execution Context তৈরি হওয়ার সময় কিছু variable এবং function declaration তাদের scope অনুযায়ী আগে থেকেই মেমোরিতে নিবন্ধিত হয়। এই আচরণকে Hoisting বলে।

- `var` hoist হয় এবং প্রাথমিকভাবে `undefined` থাকে।
- `let` ও `const` hoist হয়, কিন্তু initialization-এর আগে Temporal Dead Zone-এ থাকে।
- Function Declaration সম্পূর্ণভাবে hoist হয়।

### প্রশ্ন ৪: Closure কী?

**উত্তর:**

কোনো function তার বাইরের function-এর variable-গুলোকে বাইরের function execution শেষ হওয়ার পরেও access করতে পারলে সেই বৈশিষ্ট্যকে Closure বলে।

```js
function outer() {
  let count = 0;

  return function inner() {
    count++;
    return count;
  };
}

const counter = outer();

counter(); // 1
counter(); // 2
```

ব্যবহার: Data Privacy, State Management এবং Function Factory।

### প্রশ্ন ৫: map(), filter() এবং reduce() কী কাজে ব্যবহৃত হয়?

**উত্তর:**

- `map()` প্রতিটি element পরিবর্তন করে নতুন array তৈরি করে।
- `filter()` শর্ত পূরণ করা element নিয়ে নতুন array তৈরি করে।
- `reduce()` array-এর element-গুলোকে একটি single value-তে রূপান্তর করে।

```js
const numbers = [1, 2, 3, 4, 5];

numbers.map((n) => n * 2);
// [2, 4, 6, 8, 10]

numbers.filter((n) => n > 2);
// [3, 4, 5]

numbers.reduce((sum, n) => sum + n, 0);
// 15
```

### প্রশ্ন ৬: Spread এবং Rest Operator কী?

**উত্তর:**

দুটির Syntax একই (`...`), কিন্তু ব্যবহার ভিন্ন।

- Spread Operator array বা object-এর element/property ছড়িয়ে দেয়।
- Rest Operator একাধিক argument বা element-কে একটি array-তে সংগ্রহ করে।

```js
const arr1 = [1, 2];
const arr2 = [...arr1, 3, 4];

function sum(...numbers) {
  return numbers.reduce((a, b) => a + b, 0);
}
```

### প্রশ্ন ৭: Promise কী?

**উত্তর:**

Promise হলো JavaScript-এর একটি object, যা asynchronous operation-এর ভবিষ্যৎ সফলতা বা ব্যর্থতার ফলাফল নির্দেশ করে।

এর তিনটি State:

- Pending
- Fulfilled
- Rejected

Promise-এর ফলাফল handle করতে `.then()`, `.catch()` এবং `.finally()` ব্যবহার করা হয়।

### প্রশ্ন ৮: async/await কী?

**উত্তর:**

`async/await` হলো Promise handle করার সহজ ও পরিষ্কার পদ্ধতি।

- `async` function সবসময় একটি Promise return করে।
- `await` Promise settle হওয়া পর্যন্ত সংশ্লিষ্ট async function-এর execution স্থগিত রাখে।
- Error handle করতে `try/catch` ব্যবহার করা হয়।

```js
async function getProducts() {
  try {
    const response = await fetch("/api/products");

    if (!response.ok) {
      throw new Error("Failed to fetch products");
    }

    const data = await response.json();
    return data;
  } catch (error) {
    console.error(error);
  }
}
```

### প্রশ্ন ৯: JavaScript-এর null এবং undefined-এর পার্থক্য কী?

**উত্তর:**

- `undefined` অর্থ variable declare করা হয়েছে কিন্তু কোনো value assign করা হয়নি।
- `null` অর্থ ইচ্ছাকৃতভাবে কোনো value নেই বোঝানো হয়েছে।

### প্রশ্ন ১০: Event Bubbling কী?

**উত্তর:**

কোনো HTML element-এ event ঘটলে সেটি প্রথমে target element-এ এবং পরে তার parent element-গুলোর দিকে propagate করে। একে Event Bubbling বলে।

Event bubbling বন্ধ করতে `event.stopPropagation()` ব্যবহার করা হয়।

---

## ২. HTML ও CSS গুরুত্বপূর্ণ প্রশ্ন ও উত্তর

### প্রশ্ন ১১: Semantic HTML কী?

**উত্তর:**

যেসব HTML element তাদের content-এর অর্থ বা উদ্দেশ্য স্পষ্টভাবে প্রকাশ করে, সেগুলোকে Semantic HTML বলে।

উদাহরণ:

- `<header>`
- `<nav>`
- `<main>`
- `<section>`
- `<article>`
- `<footer>`

এগুলো Accessibility, SEO এবং Code Readability উন্নত করতে সাহায্য করে।

### প্রশ্ন ১২: Flexbox এবং CSS Grid-এর মধ্যে পার্থক্য কী?

**উত্তর:**

- Flexbox মূলত এক-মাত্রিক layout system, অর্থাৎ row অথবা column বরাবর layout তৈরি করে।
- CSS Grid দ্বি-মাত্রিক layout system, অর্থাৎ row এবং column উভয় দিক নিয়ন্ত্রণ করে।

Navigation এবং ছোট component layout-এ Flexbox এবং Product Grid-এর মতো layout-এ CSS Grid ব্যবহার করা যায়।

### প্রশ্ন ১৩: CSS Box Model কী?

**উত্তর:**

প্রতিটি HTML element-এর layout চারটি অংশ নিয়ে গঠিত:

1. Content
2. Padding
3. Border
4. Margin

`box-sizing: border-box` ব্যবহার করলে element-এর নির্ধারিত width এবং height-এর মধ্যে padding ও border অন্তর্ভুক্ত থাকে।

### প্রশ্ন ১৪: CSS Specificity কী?

**উত্তর:**

একই element-এর ওপর একাধিক CSS rule প্রয়োগ হলে কোন rule বেশি প্রাধান্য পাবে, তা নির্ধারণের পদ্ধতিকে Specificity বলে।

সাধারণ অগ্রাধিকার:

- Inline Style
- ID Selector
- Class, Attribute, Pseudo-class
- Element Selector

### প্রশ্ন ১৫: Responsive Web Design কী?

**উত্তর:**

যে পদ্ধতিতে একটি Website বিভিন্ন Screen Size ও Device অনুযায়ী নিজের Layout এবং Content মানিয়ে নিতে পারে, তাকে Responsive Web Design বলে।

ব্যবহৃত পদ্ধতি:

- CSS Media Queries
- Flexbox এবং Grid
- Relative Units
- Responsive Images
- Mobile-first Design

### প্রশ্ন ১৬: Media Query কী?

**উত্তর:**

Media Query ব্যবহার করে নির্দিষ্ট Screen Size বা Device Condition অনুযায়ী CSS Style পরিবর্তন করা যায়।

```css
.container {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
}

@media (max-width: 768px) {
  .container {
    grid-template-columns: 1fr;
  }
}
```

### প্রশ্ন ১৭: Tailwind CSS এবং Bootstrap-এর মধ্যে পার্থক্য কী?

**উত্তর:**

- Tailwind CSS একটি Utility-first CSS Framework। ছোট ছোট utility class ব্যবহার করে custom design তৈরি করা যায়।
- Bootstrap একটি Component-based CSS Framework। এতে আগে থেকে তৈরি component এবং grid system রয়েছে।

---

## ৩. React.js গুরুত্বপূর্ণ প্রশ্ন ও উত্তর

### প্রশ্ন ১৮: React.js কী?

**উত্তর:**

React.js হলো একটি JavaScript Library, যা Component-based পদ্ধতিতে User Interface তৈরি করতে ব্যবহৃত হয়।

এর গুরুত্বপূর্ণ বৈশিষ্ট্য:

- Component-based Architecture
- Declarative UI
- JSX
- One-way Data Flow
- Hooks

### প্রশ্ন ১৯: Component এবং Props কী?

**উত্তর:**

- Component হলো UI-এর reusable অংশ।
- Props হলো Parent Component থেকে Child Component-এ Data পাঠানোর পদ্ধতি।

Props read-only; Child Component সরাসরি Props পরিবর্তন করতে পারে না।

### প্রশ্ন ২০: State কী?

**উত্তর:**

State হলো Component-এর নিজস্ব পরিবর্তনশীল Data।

State পরিবর্তন হলে React প্রয়োজন অনুযায়ী Component পুনরায় render করে।

### প্রশ্ন ২১: Props এবং State-এর পার্থক্য কী?

**উত্তর:**

| Props                            | State                          |
| -------------------------------- | ------------------------------ |
| Parent থেকে আসে                  | Component নিজে পরিচালনা করে    |
| Read-only                        | Setter দিয়ে পরিবর্তন করা যায় |
| Component-এর বাইরে থেকে Data আসে | Component-এর নিজস্ব Data       |

### প্রশ্ন ২২: useState কী?

**উত্তর:**

`useState` হলো React Hook, যা Functional Component-এ State পরিচালনা করতে ব্যবহৃত হয়।

```jsx
const [count, setCount] = useState(0);

const increment = () => {
  setCount((prev) => prev + 1);
};
```

### প্রশ্ন ২৩: useEffect কী?

**উত্তর:**

`useEffect` হলো React Hook, যা Component-এর সঙ্গে সম্পর্কিত Side Effect পরিচালনা করতে ব্যবহৃত হয়।

উদাহরণ:

- API Data Fetching
- Event Listener
- Timer
- External System-এর সঙ্গে Synchronization

```jsx
useEffect(() => {
  console.log("Component mounted");
}, []);
```

### প্রশ্ন ২৪: useEffect-এর Dependency Array কীভাবে কাজ করে?

**উত্তর:**

- Dependency Array না দিলে প্রতিটি Commit-এর পর Effect চলে।
- `[]` দিলে Mount-এর পর Effect চলে।
- `[value]` দিলে Mount এবং `value` পরিবর্তিত হলে Effect চলে।

প্রয়োজন অনুযায়ী Cleanup Function ব্যবহার করে Event Listener বা Timer পরিষ্কার করতে হয়।

### প্রশ্ন ২৫: React-এ Key কেন ব্যবহার করা হয়?

**উত্তর:**

List-এর প্রতিটি Element-কে আলাদাভাবে শনাক্ত করতে Key ব্যবহার করা হয়। এটি React-কে কোন Element পরিবর্তিত, যোগ বা বাদ হয়েছে তা বুঝতে সাহায্য করে।

```jsx
{
  products.map((product) => <ProductCard key={product.id} product={product} />);
}
```

সম্ভব হলে Unique এবং Stable ID ব্যবহার করা উচিত। Index-কে Key হিসেবে ব্যবহার করা সব ক্ষেত্রে নিরাপদ নয়।

### প্রশ্ন ২৬: Controlled Component কী?

**উত্তর:**

যে Form Element-এর Value React State দ্বারা নিয়ন্ত্রিত হয়, তাকে Controlled Component বলে।

```jsx
const [name, setName] = useState("");

<input value={name} onChange={(e) => setName(e.target.value)} />;
```

### প্রশ্ন ২৭: Conditional Rendering কী?

**উত্তর:**

নির্দিষ্ট Condition অনুযায়ী UI-এর বিভিন্ন অংশ দেখানো বা লুকানোকে Conditional Rendering বলে।

```jsx
{
  isLoggedIn ? <Dashboard /> : <Login />;
}
```

### প্রশ্ন ২৮: React-এ API কীভাবে Fetch করবে?

**উত্তর:**

`fetch()` অথবা Axios ব্যবহার করে API থেকে Data আনা যায়। সাধারণত Loading, Error এবং Success State পরিচালনা করতে হয়।

```jsx
const [products, setProducts] = useState([]);
const [loading, setLoading] = useState(true);

useEffect(() => {
  const fetchProducts = async () => {
    try {
      const response = await fetch("/api/products");

      if (!response.ok) {
        throw new Error("Failed to fetch");
      }

      const data = await response.json();
      setProducts(data);
    } catch (error) {
      console.error(error);
    } finally {
      setLoading(false);
    }
  };

  fetchProducts();
}, []);
```

---

## ৪. Next.js গুরুত্বপূর্ণ প্রশ্ন ও উত্তর

### প্রশ্ন ২৯: Next.js কী?

**উত্তর:**

Next.js হলো React-এর ওপর তৈরি একটি Framework, যা Full-stack Web Application তৈরিতে ব্যবহৃত হয়।

এর সুবিধা:

- File-based Routing
- Server-side Rendering
- Static Site Generation
- Server Components
- Metadata ও SEO Support
- API এবং Backend Capabilities

### প্রশ্ন ৩০: React এবং Next.js-এর মধ্যে পার্থক্য কী?

**উত্তর:**

React একটি JavaScript Library, যা UI তৈরিতে ব্যবহৃত হয়।

Next.js হলো React-based Framework, যা Routing, Rendering, Data Fetching এবং SEO-এর মতো অতিরিক্ত সুবিধা প্রদান করে।

### প্রশ্ন ৩১: SSR, SSG এবং CSR কী?

**উত্তর:**

- CSR (Client-Side Rendering): Browser-এ JavaScript ব্যবহার করে UI তৈরি হয়।
- SSR (Server-Side Rendering): প্রতিটি Request অনুযায়ী Server-এ HTML তৈরি হয়।
- SSG (Static Site Generation): Build Time-এ HTML তৈরি হয়।

### প্রশ্ন ৩২: Server Component এবং Client Component কী?

**উত্তর:**

- Server Component Server-এ Render হয় এবং Server-side Data Access-এর জন্য উপযোগী।
- Client Component Browser-এ Interactive UI তৈরি করতে ব্যবহৃত হয়।

Client Component-এ `useState`, `useEffect` এবং Event Handler ব্যবহার করা যায়।

Client Component ঘোষণা করতে `"use client"` ব্যবহার করা হয়।

### প্রশ্ন ৩৩: Next.js-এ Dynamic Routing কী?

**উত্তর:**

URL-এর পরিবর্তনশীল অংশ অনুযায়ী Page তৈরি করাকে Dynamic Routing বলে।

উদাহরণ:

```text
app/products/[id]/page.tsx
```

এখানে `/products/101` এবং `/products/102` একই Dynamic Route ব্যবহার করতে পারে।

---

## ৫. E-Commerce Frontend গুরুত্বপূর্ণ প্রশ্ন ও উত্তর

### প্রশ্ন ৩৪: E-Commerce Website-এর Product Listing Page কীভাবে তৈরি করবে?

**উত্তর:**

প্রথমে API থেকে Product Data Fetch করব। এরপর `map()` ব্যবহার করে প্রতিটি Product-এর জন্য Reusable Product Card তৈরি করব।

প্রয়োজন অনুযায়ী Search, Category Filter, Sorting এবং Pagination যুক্ত করব। পাশাপাশি Loading, Error ও Empty State পরিচালনা করব।

### প্রশ্ন ৩৫: Shopping Cart কীভাবে পরিচালনা করবে?

**উত্তর:**

React State বা Context API ব্যবহার করে Cart Data পরিচালনা করা যায়।

প্রতিটি Cart Item-এর জন্য Product ID, Quantity এবং Price সংরক্ষণ করব।

- Add to Cart
- Remove from Cart
- Update Quantity
- Calculate Total Price

Page Refresh-এর পর Cart সংরক্ষণ করতে প্রয়োজনে LocalStorage ব্যবহার করা যায়।

### প্রশ্ন ৩৬: Product Search এবং Filtering কীভাবে করবে?

**উত্তর:**

Product Data-এর ওপর `filter()` ব্যবহার করে Search এবং Category অনুযায়ী Product নির্বাচন করা যায়।

```js
const filteredProducts = products.filter((product) =>
  product.name.toLowerCase().includes(search.toLowerCase()),
);
```

বড় Dataset হলে Server-side Search এবং Filtering ব্যবহার করা অধিক কার্যকর।

### প্রশ্ন ৩৭: E-Commerce Website-এ SEO কেন গুরুত্বপূর্ণ?

**উত্তর:**

SEO Search Engine-এ Website-এর Visibility বাড়াতে সাহায্য করে।

গুরুত্বপূর্ণ বিষয়:

- Semantic HTML
- Unique Page Title
- Meta Description
- Descriptive URLs
- Image Alt Text
- Proper Heading Structure
- SSR বা Static Rendering-এর উপযুক্ত ব্যবহার

### প্রশ্ন ৩৮: Website Performance কীভাবে উন্নত করবে?

**উত্তর:**

- Image Optimization
- Code Splitting
- Lazy Loading
- Minimize Unnecessary Re-renders
- Reduce Unused JavaScript
- Proper Caching
- Optimize API Requests
- Reduce Render-blocking Resources

---

## ৬. Git ও AI Tools

### প্রশ্ন ৩৯: Git এবং GitHub-এর মধ্যে পার্থক্য কী?

**উত্তর:**

Git হলো একটি Distributed Version Control System, যা Code-এর পরিবর্তন Track করে।

GitHub হলো Git Repository Host করার একটি Platform, যেখানে Team Collaboration, Pull Request এবং Code Review করা যায়।

### প্রশ্ন ৪০: Git-এর কিছু গুরুত্বপূর্ণ Command কী?

**উত্তর:**

```bash
git init
git clone <repository-url>
git status
git add .
git commit -m "message"
git pull
git push
git checkout -b feature-branch
git merge development
```

### প্রশ্ন ৪১: AI Coding Tools কীভাবে ব্যবহার করো?

**উত্তর:**

আমি GitHub Copilot এবং Claude Code-এর মতো AI Tools ব্যবহার করি Code Suggestions, Debugging, Boilerplate Generation এবং Problem Solving-এর জন্য।

তবে AI-এর দেওয়া Code সরাসরি গ্রহণ করি না। নিজে বুঝে, Review করে এবং প্রয়োজন অনুযায়ী Test করে ব্যবহার করি।

---

## ৭. Resume ও Project-Based Viva

### প্রশ্ন ৪২: নিজের সম্পর্কে বলো।

**উত্তর:**

আমি Md. Sohel Rana। আমি Computer Science and Engineering-এ B.Sc. সম্পন্ন করেছি। আমি মূলত Frontend Development-এ কাজ করি এবং React.js, Next.js, TypeScript ও Tailwind CSS ব্যবহার করে Responsive এবং SEO-friendly Web Application তৈরি করতে স্বাচ্ছন্দ্যবোধ করি।

আমার কিছু Full-stack Project রয়েছে, যার মধ্যে E-Book Application এবং Mini Kanban Board উল্লেখযোগ্য। আমি নতুন Technology শিখতে এবং একটি Professional Development Team-এর সঙ্গে কাজ করে নিজের দক্ষতা বাড়াতে আগ্রহী।

### প্রশ্ন ৪৩: কেন Frontend Development পছন্দ করো?

**উত্তর:**

আমি UI Development এবং User Experience নিয়ে কাজ করতে পছন্দ করি। একটি Design-কে Functional, Responsive এবং User-friendly Interface-এ রূপান্তর করার বিষয়টি আমাকে আগ্রহী করে। তাই Frontend Development-এ নিজের ক্যারিয়ার গড়তে চাই।

### প্রশ্ন ৪৪: AI Tech Craft-এ তোমার কাজ কী ছিল?

**উত্তর:**

আমি Junior Front-End Developer হিসেবে Remote-এ কাজ করেছি। সেখানে React.js, HTML, CSS এবং JavaScript ব্যবহার করে Responsive Interface তৈরি করেছি। একটি Real Estate Website-এর Frontend Development-এ কাজ করেছি এবং Interactive Image Gallery ও Firebase-backed Lead Capture যুক্ত করেছি।

এছাড়া Render-blocking Script এবং Asset Delivery Optimize করে Client Applications-এর Page Load Performance উন্নত করতে কাজ করেছি।

### প্রশ্ন ৪৫: তোমার E-Book Application সম্পর্কে বলো।

**উত্তর:**

এটি একটি Full-stack E-Book Platform, যেখানে User-রা Book Browse, Search, Purchase এবং Access করতে পারে।

Frontend-এ Next.js, React Query, Axios এবং Context API ব্যবহার করেছি। Backend-এ Node.js, Express.js এবং MongoDB ব্যবহার করেছি।

এছাড়া JWT Authentication, Role-based Access Control, SSLCommerz Payment Integration, Socket.IO Notifications এবং AI Book Assistant যুক্ত করেছি।

### প্রশ্ন ৪৬: তোমার সবচেয়ে বড় Strength কী?

**উত্তর:**

আমার প্রধান Strength হলো নতুন Technology দ্রুত শেখার সক্ষমতা, Problem-solving এবং নিজের শেখা বিষয়গুলো বাস্তব Project-এ প্রয়োগ করার আগ্রহ। আমি নিজের Code বুঝে লিখতে এবং Maintainable Solution তৈরি করতে গুরুত্ব দিই।

### প্রশ্ন ৪৭: কেন আমাদের প্রতিষ্ঠানে কাজ করতে চাও?

**উত্তর:**

আমি এমন একটি Professional Environment-এ কাজ করতে চাই, যেখানে বাস্তব Project-এ অবদান রাখার পাশাপাশি Senior Developers-এর কাছ থেকে শিখতে পারব। আপনাদের Frontend এবং E-Commerce Development-সংক্রান্ত কাজের মাধ্যমে নিজের React.js ও Next.js দক্ষতা আরও উন্নত করতে চাই।

### প্রশ্ন ৪৮: তোমাকে কেন নিয়োগ দেওয়া উচিত?

**উত্তর:**

আমার React.js, Next.js এবং TypeScript-এর ওপর বাস্তব Project Experience রয়েছে। আমি Responsive UI তৈরি, API Integration এবং Frontend Problem Solving করতে পারি। পাশাপাশি নতুন Technology শেখার আগ্রহ এবং Team-এর সঙ্গে কাজ করার মানসিকতা রয়েছে। আমি দ্রুত শিখে Team-এর কাজে অবদান রাখতে পারব বলে বিশ্বাস করি।

---

## ৮. Short Test-এর জন্য Practical Coding

### প্রশ্ন ৪৯: একটি Product Card তৈরি করো।

```jsx
function ProductCard({ product }) {
  return (
    <div className="rounded-lg border p-4">
      <img
        src={product.image}
        alt={product.name}
        className="h-48 w-full object-cover"
      />

      <h2 className="mt-3 font-semibold">{product.name}</h2>

      <p className="mt-2 text-lg font-bold">৳{product.price}</p>

      <button className="mt-3 w-full rounded bg-black p-2 text-white">
        Add to Cart
      </button>
    </div>
  );
}

export default ProductCard;
```

### প্রশ্ন ৫০: একটি Responsive Product Grid তৈরি করো।

```jsx
<div className="grid grid-cols-1 gap-4 sm:grid-cols-2 lg:grid-cols-4">
  {products.map((product) => (
    <ProductCard key={product.id} product={product} />
  ))}
</div>
```

### প্রশ্ন ৫১: JavaScript দিয়ে Array থেকে সর্বোচ্চ Price-এর Product বের করো।

```js
const highestPriceProduct = products.reduce((highest, product) =>
  product.price > highest.price ? product : highest,
);
```

### প্রশ্ন ৫২: API থেকে Product Data Fetch করার সময় কী কী বিষয় বিবেচনা করবে?

**উত্তর:**

1. Loading State
2. Error Handling
3. HTTP Response Status
4. Data Validation
5. Empty State
6. Component Cleanup বা Request Cancellation যেখানে প্রয়োজন

---

# শেষ মুহূর্তের প্রস্তুতি

অবশ্যই ভালোভাবে প্রস্তুত করবে:

1. JavaScript: প্রশ্ন ১–১০
2. React: প্রশ্ন ১৮–২৮
3. CSS: প্রশ্ন ১২–১৭
4. Next.js: প্রশ্ন ২৯–৩৩
5. E-Commerce: প্রশ্ন ৩৪–৩৮
6. নিজের Resume ও Project: প্রশ্ন ৪২–৪৮

**বিশেষ পরামর্শ:** উত্তর মুখস্থ না করে প্রতিটি বিষয়ের Concept বোঝার চেষ্টা করবে। Interviewer কোনো প্রশ্নের Follow-up করলে যেন নিজের ভাষায় ব্যাখ্যা করতে পারো।

## খ. প্রজেক্ট ভিত্তিক প্রশ্ন

### ByteSpace

**৬. ByteSpace প্রজেক্টটি সম্পর্কে বলুন।**

এটি একটি Figma ডিজাইন থেকে বানানো responsive landing page। আমি Next.js, TypeScript আর Tailwind CSS দিয়ে বানিয়েছি, কিছু অংশে Bootstrap-ও ব্যবহার করেছি। লক্ষ্য ছিল ডিজাইনের সাথে pixel-accurate মিল রাখা, আর desktop, tablet ও mobile তিন ধরনের screen-এই layout ঠিক রাখা। কম্পোনেন্টগুলো reusable করে বানিয়েছি।

**৭. Figma থেকে কোডে নেওয়ার সময় কীভাবে এগোন?**

প্রথমে পুরো ডিজাইন দেখে section ভাগ করি, তারপর repeated অংশ চিহ্নিত করে (button, card, heading) সেগুলো আলাদা component করি। Figma থেকে spacing, font size, color আর breakpoint দেখে নিই। প্রথমে desktop বা mobile যেকোনো একটি ঠিক করে, তারপর অন্যগুলোতে responsive করি। শেষে ডিজাইনের পাশে রেখে মিলিয়ে দেখি।

**৮. responsive করতে গিয়ে কোন সমস্যায় পড়েছিলেন?**

`[তোমার আসল অভিজ্ঞতা বলো, যেমন: navbar mobile-এ কীভাবে হবে, grid-এর column বদলানো, image overflow]`। সমাধান হিসেবে Tailwind-এর breakpoint prefix (`sm:`, `md:`, `lg:`) ব্যবহার করেছি এবং Flexbox/Grid দিয়ে layout ঠিক করেছি।

### E-Book Application

**৯. E-Book Application প্রজেক্টটি সম্পর্কে বলুন।**

এটি একটি full-stack প্ল্যাটফর্ম। ফ্রন্টএন্ডে Next.js (App Router), আর ব্যাকএন্ডে Node.js, Express ও MongoDB। ব্যবহারকারী লগইন করে বই ব্রাউজ, সার্চ, cart ও favorites ব্যবহার করতে পারে। আলাদা একটি admin dashboard আছে। দুই ধরনের role আছে এবং JWT দিয়ে role-based access নিয়ন্ত্রণ করেছি। এতে SSLCommerz পেমেন্ট (bKash, Nagad, card), Socket.IO দিয়ে real-time notification, আর Grok API দিয়ে একটি AI book assistant আছে।

**১০. SSR আর SSG কোথায় ব্যবহার করেছেন এবং কেন?**

`[কোন পেজে কোনটি ব্যবহার করেছ তা নিজে মিলিয়ে বলো]`। সাধারণ ধারণা হলো, যেসব পেজের content কম বদলায় এবং SEO দরকার (যেমন বইয়ের তালিকা বা বিস্তারিত পেজ), সেখানে SSG ভালো, কারণ build time-এ তৈরি হয়ে দ্রুত লোড হয়। যেখানে প্রতিবার request-এ ডেটা নতুন লাগে, সেখানে SSR ব্যবহার করা হয়।

**১১. React Query আর Axios কেন ব্যবহার করেছেন?**

Axios দিয়ে API call করেছি, আর React Query দিয়ে সেই ডেটা fetch, cache, loading ও error state সামলেছি। এতে নিজে `useEffect` আর `useState` দিয়ে বারবার একই কোড লিখতে হয় না, আর একই ডেটা বারবার আনতে হয় না কারণ cache থাকে।

**১২. JWT authentication কীভাবে কাজ করে? আপনি কীভাবে ব্যবহার করেছেন?**

ব্যবহারকারী লগইন করলে সার্ভার একটি signed token দেয়। পরের request-এ ক্লায়েন্ট সেই token পাঠায়, সার্ভার যাচাই করে ব্যবহারকারীকে চেনে। আমি token-এ ব্যবহারকারীর role রেখে middleware দিয়ে যাচাই করেছি, যাতে সাধারণ ব্যবহারকারী admin route-এ ঢুকতে না পারে। `[token কোথায় রেখেছ (cookie/localStorage) এবং কেন, তা নিজে মিলিয়ে বলো]`।

**১৩. SSLCommerz পেমেন্ট ইন্টিগ্রেশনের flow বুঝিয়ে বলুন।**

সংক্ষেপে: ব্যবহারকারী checkout করলে সার্ভার SSLCommerz-এ পেমেন্ট সেশন শুরু করে এবং একটি gateway URL পায়, ব্যবহারকারী সেখানে গিয়ে পেমেন্ট করে। পেমেন্টের পর success, fail বা cancel URL-এ ফিরে আসে, আর সার্ভার সেই ফলাফল যাচাই করে অর্ডারের অবস্থা আপডেট করে। `[তোমার প্রজেক্টে ঠিক কীভাবে করেছ, বিশেষত যাচাইয়ের ধাপ, তা মিলিয়ে বলো]`।

**১৪. Socket.IO কেন ব্যবহার করেছেন?**

Real-time notification দেওয়ার জন্য। সাধারণ HTTP request-এ ক্লায়েন্টকে বারবার সার্ভারকে জিজ্ঞেস করতে হয়, কিন্তু Socket.IO-তে সংযোগ খোলা থাকে, তাই সার্ভার নিজে থেকে ক্লায়েন্টকে ঘটনা জানাতে পারে।

### Mini Kanban Board

**১৫. Mini Kanban Board প্রজেক্টটি সম্পর্কে বলুন।**

এটি একটি task management অ্যাপ, যেখানে drag-and-drop করে task এক column থেকে অন্য column-এ নেওয়া যায় এবং state ডেটাবেসে সংরক্ষিত থাকে। ফ্রন্টএন্ডে Next.js, TypeScript, React Hooks ও Context API, ব্যাকএন্ডে Node.js, Express, PostgreSQL ও Prisma, আর Docker ব্যবহার করেছি। কোড reusable component ও custom hook দিয়ে সাজিয়েছি, আর semantic HTML, accessibility ও SEO-র মৌলিক বিষয় মেনেছি।

**১৬. Drag-and-drop কীভাবে করেছেন?**

`[কোন লাইব্রেরি (যেমন dnd-kit, react-beautiful-dnd) বা কীভাবে করেছ তা নিজে বলো]`। drop করার পর নতুন অবস্থান API-এর মাধ্যমে ডেটাবেসে সেভ করেছি, যাতে রিফ্রেশ করলেও অবস্থা থাকে।

**১৭. Prisma কী? কেন ব্যবহার করেছেন?**

Prisma একটি ORM। এতে schema ফাইলে মডেল লিখে type-safe উপায়ে ডেটাবেস query করা যায়, আর migration দিয়ে ডেটাবেসের কাঠামো বদলানো যায়। আমি PostgreSQL-এর সাথে ব্যবহার করেছি এবং Prisma migration চালিয়েছি।

### কাজের অভিজ্ঞতা

**১৮. AI Tech Craft-এ কী কাজ করেছেন?**

আমি জুনিয়র ফ্রন্টএন্ড ডেভেলপার হিসেবে টিমের সাথে একটি real estate ওয়েবসাইটের responsive ফ্রন্টএন্ড শুরু থেকে বানাতে অংশ নিয়েছি। এতে একটি interactive image gallery এবং Firebase-ভিত্তিক lead capture ছিল।

**১৯. Page load time প্রায় ৩০% কমিয়েছেন বলেছেন। কীভাবে মেপেছেন ও কী করেছেন?**

`[এটি তোমার আসল কাজ থেকে বলো। CV-তে লেখা: browser performance audit দিয়ে মাপা, render-blocking script ঠিক করা, asset delivery উন্নত করা]`। সাধারণত এর অর্থ হলো script `defer` বা `async` করা, ছবি optimize করা, অপ্রয়োজনীয় ফাইল কমানো। কোন টুলে মেপেছ (Lighthouse/DevTools) তা নির্দিষ্ট করে বলো। যা করোনি, তা বলো না।

**২০. Fiverr-এ কী ধরনের কাজ করেছেন? কোনো কঠিন ক্লায়েন্টের অভিজ্ঞতা আছে?**

৩৩টি অর্ডার করেছি, যার প্রায় ১৫টি web development প্রজেক্ট (frontend, backend, full-stack)। কাজের মধ্যে ছিল REST API integration, bug fix, page speed ও cross-browser compatibility উন্নত করা। ক্লায়েন্টের চাহিদা পরিষ্কার না হলে আগে প্রশ্ন করে বুঝে নিতাম এবং ছোট ছোট আপডেট দিয়ে নিশ্চিত হতাম। `[চাইলে একটি নির্দিষ্ট ঘটনা যোগ করো]`।

## প্রশ্ন: কীভাবে Website-এর Page Load Time প্রায় ৩০% কমিয়েছিলেন?

**উত্তর:**

প্রথমে Chrome DevTools-এর Lighthouse ব্যবহার করে Website-এর Performance Audit করেছিলাম। সেখানে Render-blocking JavaScript এবং বড় Asset-এর কারণে Loading Delay হচ্ছে কি না, তা পরীক্ষা করেছিলাম।

এরপর:

- Non-critical JavaScript-এ `defer` ব্যবহার করেছি।
- অপ্রয়োজনীয় JavaScript কমিয়েছি।
- Image Compress করে WebP Format ব্যবহার করেছি।
- প্রয়োজন অনুযায়ী Lazy Loading ও Browser Caching ব্যবহার করেছি।

সবশেষে Optimization-এর আগে ও পরে Performance Test করে উন্নতি যাচাই করেছি।

### গুরুত্বপূর্ণ বিষয়

- **Render-blocking:** যে Script HTML Parsing বা Rendering-এ বাধা সৃষ্টি করে।
- **Asset Optimization:** Image, CSS ও JavaScript-এর File Size এবং Delivery উন্নত করা।
- **Lighthouse:** Website-এর Performance Audit করার Tool।

**৩০% কীভাবে হিসাব করা হয়?**

উদাহরণস্বরূপ, আগে Loading Time ৫ সেকেন্ড এবং পরে ৩.৫ সেকেন্ড হলে:

Improvement = ((5 - 3.5) / 5) × 100 = 30%

**মনে রাখবে:** এগুলো Performance Optimization-এর সাধারণ পদ্ধতি। তোমার বাস্তব অভিজ্ঞতায় যেগুলো করেছিলে, শুধু সেগুলোই নিজের কাজ হিসেবে বলবে।


# মক শর্ট টেস্ট – নমুনা সমাধান

> আগে নিজে চেষ্টা করো, তারপর এই ফাইল দেখো। এটি একটি সম্ভাব্য সমাধান, একমাত্র নয়।

---

## পার্ট গ: JS লজিক ফাংশন

```js
// ১. স্ট্রিং উল্টানো
export function reverseString(str) {
  return str.split("").reverse().join("");
}

// ২. ডুপ্লিকেট বাদ
export function removeDuplicates(arr) {
  return [...new Set(arr)];
}

// ৩. cart-এর মোট দাম
export function cartTotal(cart) {
  const total = cart.reduce((sum, item) => sum + item.price * item.qty, 0);
  return Number(total.toFixed(2));
}

// ৪. ক্যাটাগরি অনুযায়ী গণনা
export function countByCategory(products) {
  return products.reduce((acc, p) => {
    acc[p.category] = (acc[p.category] || 0) + 1;
    return acc;
  }, {});
}

// ৫. দ্বিতীয় সর্বোচ্চ ভিন্ন মান
export function secondHighest(arr) {
  const unique = [...new Set(arr)].sort((a, b) => b - a);
  return unique.length >= 2 ? unique[1] : null;
}
```

**ব্যাখ্যা (ভাইভার জন্য):**
- `Set` শুধু অনন্য মান রাখে, তাই ডুপ্লিকেট বাদ দিতে সুবিধা।
- `reduce` দিয়ে একটি অ্যারে থেকে একটি মান (যোগফল বা অবজেক্ট) বানানো যায়।
- `toFixed(2)` স্ট্রিং দেয়, তাই `Number()` দিয়ে সংখ্যায় ফিরিয়েছি। ভাসমান সংখ্যার ছোট ভুল (যেমন `0.1 + 0.2`) এড়াতে এটি কাজে লাগে।

---

## পার্ট ক, খ, গ: UI + API + Cart (একটি কম্পোনেন্ট)

Next.js (App Router) হলে ফাইলের উপরে `"use client"` লাগবে। React (Vite) হলে লাগবে না।

```jsx
"use client";
import { useEffect, useMemo, useState } from "react";

const API = "https://fakestoreapi.com";

export default function ProductPage() {
  const [products, setProducts] = useState([]);
  const [categories, setCategories] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  const [search, setSearch] = useState("");
  const [category, setCategory] = useState("all");
  const [cart, setCart] = useState([]);
  const [reloadKey, setReloadKey] = useState(0);

  // ডেটা আনা
  useEffect(() => {
    async function load() {
      try {
        setLoading(true);
        setError(null);
        const [pRes, cRes] = await Promise.all([
          fetch(`${API}/products`),
          fetch(`${API}/products/categories`),
        ]);
        if (!pRes.ok || !cRes.ok) throw new Error("ডেটা আনা যায়নি");
        setProducts(await pRes.json());
        setCategories(await cRes.json());
      } catch (err) {
        setError(err.message || "কিছু একটা ভুল হয়েছে");
      } finally {
        setLoading(false);
      }
    }
    load();
  }, [reloadKey]);

  // ফিল্টার: সার্চ + ক্যাটাগরি একসাথে
  const filtered = useMemo(() => {
    return products.filter((p) => {
      const matchCategory = category === "all" || p.category === category;
      const matchSearch = p.title.toLowerCase().includes(search.trim().toLowerCase());
      return matchCategory && matchSearch;
    });
  }, [products, search, category]);

  // cart ফাংশন
  const addToCart = (product) => {
    setCart((prev) => {
      const found = prev.find((i) => i.id === product.id);
      if (found) {
        return prev.map((i) => (i.id === product.id ? { ...i, qty: i.qty + 1 } : i));
      }
      return [...prev, { id: product.id, title: product.title, price: product.price, qty: 1 }];
    });
  };

  const changeQty = (id, delta) => {
    setCart((prev) =>
      prev
        .map((i) => (i.id === id ? { ...i, qty: i.qty + delta } : i))
        .filter((i) => i.qty > 0)
    );
  };

  const removeItem = (id) => setCart((prev) => prev.filter((i) => i.id !== id));

  const itemCount = cart.reduce((sum, i) => sum + i.qty, 0);
  const total = Number(cart.reduce((sum, i) => sum + i.price * i.qty, 0).toFixed(2));

  return (
    <div className="min-h-screen bg-gray-50">
      {/* Navbar */}
      <nav className="sticky top-0 z-10 flex items-center justify-between bg-white px-4 py-3 shadow">
        <h1 className="text-xl font-bold">MyShop</h1>
        <div className="rounded-full bg-gray-900 px-3 py-1 text-sm text-white">
          Cart: {itemCount}
        </div>
      </nav>

      <main className="mx-auto grid max-w-7xl gap-6 p-4 lg:grid-cols-[1fr_320px]">
        <section>
          {/* সার্চ ও ফিল্টার */}
          <div className="mb-4 flex flex-col gap-3 sm:flex-row">
            <input
              type="text"
              value={search}
              onChange={(e) => setSearch(e.target.value)}
              placeholder="প্রোডাক্ট খুঁজুন..."
              className="w-full rounded border px-3 py-2"
            />
            <select
              value={category}
              onChange={(e) => setCategory(e.target.value)}
              className="rounded border px-3 py-2 sm:w-56"
            >
              <option value="all">সব ক্যাটাগরি</option>
              {categories.map((c) => (
                <option key={c} value={c}>{c}</option>
              ))}
            </select>
          </div>

          {/* loading / error / empty / list */}
          {loading && <p className="py-10 text-center">লোড হচ্ছে...</p>}

          {error && !loading && (
            <div className="py-10 text-center">
              <p className="mb-3 text-red-600">{error}</p>
              <button
                onClick={() => setReloadKey((k) => k + 1)}
                className="rounded bg-gray-900 px-4 py-2 text-white"
              >
                আবার চেষ্টা করুন
              </button>
            </div>
          )}

          {!loading && !error && filtered.length === 0 && (
            <p className="py-10 text-center">কোনো প্রোডাক্ট পাওয়া যায়নি</p>
          )}

          {!loading && !error && filtered.length > 0 && (
            <div className="grid grid-cols-1 gap-4 sm:grid-cols-2 xl:grid-cols-3">
              {filtered.map((p) => (
                <div
                  key={p.id}
                  className="flex flex-col rounded-lg bg-white p-4 shadow transition hover:-translate-y-1 hover:shadow-lg"
                >
                  <img
                    src={p.image}
                    alt={p.title}
                    className="mb-3 h-48 w-full object-contain"
                  />
                  <h2 className="line-clamp-2 font-semibold">{p.title}</h2>
                  <p className="text-sm capitalize text-gray-500">{p.category}</p>
                  <div className="mt-2 flex items-center justify-between">
                    <span className="font-bold">${p.price}</span>
                    <span className="text-sm">★ {p.rating?.rate}</span>
                  </div>
                  <button
                    onClick={() => addToCart(p)}
                    className="mt-auto rounded bg-gray-900 py-2 text-white"
                  >
                    Add to Cart
                  </button>
                </div>
              ))}
            </div>
          )}
        </section>

        {/* Cart */}
        <aside className="h-fit rounded-lg bg-white p-4 shadow">
          <h2 className="mb-3 text-lg font-bold">Cart</h2>
          {cart.length === 0 && <p className="text-sm text-gray-500">Cart খালি</p>}
          {cart.map((i) => (
            <div key={i.id} className="mb-3 border-b pb-3">
              <p className="line-clamp-1 text-sm font-medium">{i.title}</p>
              <p className="text-sm">${i.price}</p>
              <div className="mt-1 flex items-center gap-2">
                <button onClick={() => changeQty(i.id, -1)} className="rounded border px-2">−</button>
                <span>{i.qty}</span>
                <button onClick={() => changeQty(i.id, 1)} className="rounded border px-2">+</button>
                <button onClick={() => removeItem(i.id)} className="ml-auto text-sm text-red-600">
                  মুছুন
                </button>
              </div>
            </div>
          ))}
          <p className="mt-3 text-right font-bold">মোট: ${total}</p>
        </aside>
      </main>
    </div>
  );
}
```

**মূল বিষয় (ভাইভার জন্য):**
- `useMemo` দিয়ে ফিল্টার করা তালিকা হিসাব করেছি, যাতে সার্চ ও ক্যাটাগরি একসাথে কাজ করে এবং অপ্রয়োজনীয় হিসাব না হয়।
- `reloadKey` বদলে `useEffect` আবার চালিয়ে "আবার চেষ্টা করুন" করেছি।
- cart-এ একই প্রোডাক্ট থাকলে `qty` বাড়াই, না থাকলে নতুন লাইন যোগ করি। `qty` ০ হলে `filter` দিয়ে বাদ দিই।
- state সরাসরি বদলাইনি, নতুন অ্যারে বা অবজেক্ট বানিয়েছি (immutability)।
- `key` হিসেবে `id` ব্যবহার করেছি, index নয়।

**বোনাসের ইঙ্গিত:**
- `localStorage`: cart পড়ার জন্য প্রথম render-এ `useEffect`-এ `JSON.parse(localStorage.getItem("cart"))`, আর cart বদলালে আরেকটি `useEffect`-এ `JSON.stringify` করে লেখো। Next.js-এ সার্ভারে `localStorage` নেই, তাই এটি অবশ্যই `useEffect`-এর ভেতরে করতে হবে।
- সাজানো: `[...filtered].sort((a, b) => a.price - b.price)`, মূল অ্যারে না বদলে কপি করে।
- Debounce: `useEffect`-এ `setTimeout` দিয়ে ৩০০ms পরে সার্চ মান বসাও, আর cleanup-এ `clearTimeout` করো।
