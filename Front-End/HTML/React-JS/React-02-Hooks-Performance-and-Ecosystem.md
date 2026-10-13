# React.js Hooks, Performance and Ecosystem (Q21 - Q40)

> Place in: `02-frontend/react/02-hooks-and-performance.md`
> Core concepts (Q1-Q20) are in `01-core-concepts.md`.

---

## Q21. `useState`

### English

**Question:** How does `useState` work? Why should you use the functional updater form?

**Answer:** `useState(initial)` returns the current state and a setter. Calling the setter schedules a re-render with the new value; the update is asynchronous and batched, so the state variable does not change immediately inside the same render. When the new state depends on the previous state, use `setState(prev => ...)` so you always work with the latest value. State must be treated as immutable: create new objects/arrays instead of mutating. Pass a function to `useState(() => expensive())` for lazy initialization.

### বাংলা

**প্রশ্ন:** `useState` কীভাবে কাজ করে? Functional updater কেন ব্যবহার করবেন?

**উত্তর:** `useState(initial)` বর্তমান state ও একটি setter return করে। Setter call করলে নতুন মান দিয়ে re-render নির্ধারিত হয়; আপডেট asynchronous ও batched, তাই একই render-এর ভেতরে state variable সাথে সাথে বদলায় না। নতুন state আগের state-এর উপর নির্ভর করলে `setState(prev => ...)` ব্যবহার করুন, যাতে সবসময় সর্বশেষ মান পাওয়া যায়। State অপরিবর্তনীয় হিসেবে দেখুন: mutate না করে নতুন object/array বানান। ভারী প্রাথমিক মানের জন্য `useState(() => expensive())` (lazy initialization) দিন।

### Example

```jsx
const [count, setCount] = useState(0);

function addThree() {
  setCount(count + 1);
  setCount(count + 1);
  setCount(count + 1); // result: +1 only (stale count)
}

function addThreeCorrect() {
  setCount((c) => c + 1);
  setCount((c) => c + 1);
  setCount((c) => c + 1); // result: +3
}

const [user, setUser] = useState({ name: "A", age: 20 });
setUser((prev) => ({ ...prev, age: 21 })); // never mutate user.age directly
```

---

## Q22. `useEffect`

### English

**Question:** How does `useEffect` work? Explain the dependency array and cleanup.

**Answer:** `useEffect` runs side effects (data fetching, subscriptions, timers, DOM updates) after React renders and paints. The dependency array controls when it re-runs: no array means after every render, `[]` means once after mount, `[a, b]` means when `a` or `b` change. The function you return is the cleanup, which runs before the effect re-runs and when the component unmounts. Missing or wrong dependencies cause stale values or infinite loops (for example, putting a new object/array/function created each render in the dependencies).

### বাংলা

**প্রশ্ন:** `useEffect` কীভাবে কাজ করে? Dependency array ও cleanup ব্যাখ্যা করুন।

**উত্তর:** `useEffect` React render ও paint করার পরে side effect (data fetch, subscription, timer, DOM আপডেট) চালায়। Dependency array ঠিক করে কখন আবার চলবে: array না দিলে প্রতিটি render-এর পরে, `[]` দিলে mount-এর পরে একবার, `[a, b]` দিলে `a` বা `b` বদলালে। যে function return করা হয় সেটিই cleanup, যা effect আবার চলার আগে এবং component unmount হওয়ার সময় চলে। Dependency ভুল বা বাদ গেলে পুরনো মান বা infinite loop হয় (যেমন প্রতি render-এ নতুন তৈরি object/array/function dependency-তে রাখা)।

### Example

```jsx
function UserProfile({ userId }) {
  const [user, setUser] = useState(null);

  useEffect(() => {
    let ignore = false;
    async function load() {
      const res = await fetch(`/api/users/${userId}`);
      const data = await res.json();
      if (!ignore) setUser(data); // avoid race condition
    }
    load();
    return () => {
      ignore = true;
    }; // cleanup
  }, [userId]); // re-run when userId changes

  return user ? <h1>{user.name}</h1> : <p>Loading...</p>;
}
```

---

## Q23. `useEffect` vs `useLayoutEffect`

### English

**Question:** What is the difference between `useEffect` and `useLayoutEffect`?

**Answer:** `useEffect` runs asynchronously after the browser has painted, so it does not block the screen. `useLayoutEffect` runs synchronously after DOM updates but before the browser paints, so it can read layout (size, position) and update the DOM without a visible flicker. Use `useEffect` by default; use `useLayoutEffect` only for measuring or adjusting layout, because it blocks painting.

### বাংলা

**প্রশ্ন:** `useEffect` আর `useLayoutEffect`-এর পার্থক্য কী?

**উত্তর:** `useEffect` browser পর্দায় paint করার পরে asynchronous ভাবে চলে, তাই পর্দা আটকায় না। `useLayoutEffect` DOM আপডেটের পরে কিন্তু browser paint করার আগেই synchronous ভাবে চলে, তাই layout (মাপ, অবস্থান) পড়ে ও DOM বদলে ঝিলিক (flicker) ছাড়াই দেখাতে পারে। ডিফল্টভাবে `useEffect` ব্যবহার করুন; শুধু layout মাপা বা ঠিক করার দরকার হলে `useLayoutEffect` ব্যবহার করুন, কারণ এটি paint আটকে রাখে।

### Example

```jsx
function Tooltip({ children }) {
  const ref = useRef(null);
  const [height, setHeight] = useState(0);

  useLayoutEffect(() => {
    setHeight(ref.current.getBoundingClientRect().height); // measured before paint
  }, []);

  return (
    <div ref={ref} style={{ marginTop: -height }}>
      {children}
    </div>
  );
}
```

---

## Q24. `useRef`

### English

**Question:** What is `useRef` used for?

**Answer:** `useRef(initial)` returns a mutable object `{ current }` that persists for the whole life of the component. Changing `.current` does not trigger a re-render. Uses: accessing DOM nodes, storing values that should survive renders without causing updates (timer IDs, previous value, a mounted flag, a render counter). Do not read or write `ref.current` during rendering for data that should drive the UI.

### বাংলা

**প্রশ্ন:** `useRef` কীসের জন্য ব্যবহার হয়?

**উত্তর:** `useRef(initial)` একটি পরিবর্তনযোগ্য object `{ current }` return করে, যা component-এর পুরো জীবনকাল টিকে থাকে। `.current` বদলালে re-render হয় না। ব্যবহার: DOM node-এ পৌঁছানো, এবং এমন মান রাখা যা render-এর মধ্যে টিকে থাকবে কিন্তু UI আপডেট ঘটাবে না (timer ID, আগের মান, mounted flag, render গণনা)। যে data UI চালাবে তার জন্য render চলাকালে `ref.current` পড়া বা লেখা উচিত নয়।

### Example

```jsx
function usePrevious(value) {
  const ref = useRef();
  useEffect(() => {
    ref.current = value;
  });
  return ref.current; // value from the previous render
}

function Stopwatch() {
  const [time, setTime] = useState(0);
  const intervalRef = useRef(null);

  const start = () => {
    intervalRef.current = setInterval(() => setTime((t) => t + 1), 1000);
  };
  const stop = () => clearInterval(intervalRef.current);

  return (
    <>
      <p>{time}</p>
      <button onClick={start}>Start</button>
      <button onClick={stop}>Stop</button>
    </>
  );
}
```

---

## Q25. `useMemo` vs `useCallback`

### English

**Question:** What is the difference between `useMemo` and `useCallback`? When should you use them?

**Answer:** `useMemo(fn, deps)` caches the result of a computation; `useCallback(fn, deps)` caches the function itself (it is equal to `useMemo(() => fn, deps)`). Use `useMemo` for expensive calculations or to keep a stable object/array reference; use `useCallback` to keep a stable function reference, mainly when passing callbacks to memoized children (`React.memo`) or when the function is a dependency of another hook. Do not use them everywhere: they have their own cost and only help when they prevent real expensive work or unnecessary re-renders.

### বাংলা

**প্রশ্ন:** `useMemo` আর `useCallback`-এর পার্থক্য কী? কখন ব্যবহার করবেন?

**উত্তর:** `useMemo(fn, deps)` একটি হিসাবের ফলাফল cache করে; `useCallback(fn, deps)` function-টিকেই cache করে (এটি `useMemo(() => fn, deps)`-এর সমান)। ভারী হিসাব বা স্থির object/array reference রাখতে `useMemo` ব্যবহার করুন; স্থির function reference রাখতে `useCallback` ব্যবহার করুন, বিশেষ করে memoized child-এ (`React.memo`) callback পাঠানোর সময় বা যখন function অন্য hook-এর dependency। সব জায়গায় ব্যবহার করবেন না: এদের নিজেদেরও খরচ আছে এবং বাস্তবে ভারী কাজ বা অপ্রয়োজনীয় re-render আটকালেই শুধু লাভ হয়।

### Example

```jsx
function ProductList({ products, onSelect }) {
  const [query, setQuery] = useState("");

  const filtered = useMemo(
    () =>
      products.filter((p) =>
        p.name.toLowerCase().includes(query.toLowerCase()),
      ),
    [products, query],
  );

  return <List items={filtered} onSelect={onSelect} />;
}

function Parent() {
  const [count, setCount] = useState(0);
  const handleSelect = useCallback((id) => console.log("selected", id), []);
  return (
    <>
      <button onClick={() => setCount(count + 1)}>{count}</button>
      <ProductList products={data} onSelect={handleSelect} />
    </>
  );
}
```

---

## Q26. `React.memo`

### English

**Question:** What does `React.memo` do?

**Answer:** `React.memo(Component)` is a higher-order component that skips re-rendering when the props are shallowly equal to the previous props. It helps when a parent re-renders often but the child's props rarely change. It is useless if you pass new object, array or function references every render, so combine it with `useMemo` and `useCallback`. A custom comparison function can be passed as the second argument.

### বাংলা

**প্রশ্ন:** `React.memo` কী করে?

**উত্তর:** `React.memo(Component)` একটি higher-order component, যা props আগের props-এর সাথে shallow ভাবে সমান হলে re-render বাদ দেয়। Parent ঘনঘন re-render হয় কিন্তু child-এর props কমই বদলায় এমন ক্ষেত্রে কাজে লাগে। প্রতি render-এ নতুন object, array বা function reference পাঠালে এটি অকেজো, তাই `useMemo` ও `useCallback`-এর সাথে ব্যবহার করুন। দ্বিতীয় argument হিসেবে নিজের তুলনা function-ও দেওয়া যায়।

### Example

```jsx
const ExpensiveChild = React.memo(function ExpensiveChild({ data, onClick }) {
  console.log("ExpensiveChild rendered");
  return <button onClick={onClick}>{data.label}</button>;
});

function Parent() {
  const [count, setCount] = useState(0);
  const data = useMemo(() => ({ label: "Click me" }), []);
  const onClick = useCallback(() => console.log("clicked"), []);

  return (
    <>
      <button onClick={() => setCount(count + 1)}>Parent: {count}</button>
      <ExpensiveChild data={data} onClick={onClick} />{" "}
      {/* does not re-render */}
    </>
  );
}
```

---

## Q27. `useReducer` vs `useState`

### English

**Question:** When would you use `useReducer` instead of `useState`?

**Answer:** `useReducer(reducer, initialState)` manages state through a pure `reducer(state, action)` function and `dispatch(action)`. Prefer it when state has several related fields, the next state depends on complex logic, or many different actions update the same state (for example, forms, shopping carts, multi-step flows). It centralizes update logic, makes it easier to test and pairs well with Context for a Redux-like pattern. Use `useState` for simple, independent values.

### বাংলা

**প্রশ্ন:** কখন `useState`-এর বদলে `useReducer` ব্যবহার করবেন?

**উত্তর:** `useReducer(reducer, initialState)` একটি pure `reducer(state, action)` function ও `dispatch(action)` দিয়ে state সামলায়। State-এ একাধিক সম্পর্কিত field থাকলে, পরবর্তী state জটিল logic-এর উপর নির্ভর করলে, বা অনেক ধরনের action একই state বদলালে (যেমন form, shopping cart, ধাপে ধাপে flow) এটি ভালো। এটি আপডেটের logic এক জায়গায় রাখে, test করা সহজ করে এবং Context-এর সাথে Redux-এর মতো ধরন বানাতে সাহায্য করে। সহজ ও স্বাধীন মানের জন্য `useState`-ই যথেষ্ট।

### Example

```jsx
function cartReducer(state, action) {
  switch (action.type) {
    case "add":
      return [...state, { ...action.item, qty: 1 }];
    case "remove":
      return state.filter((i) => i.id !== action.id);
    case "clear":
      return [];
    default:
      throw new Error(`Unknown action: ${action.type}`);
  }
}

function Cart() {
  const [cart, dispatch] = useReducer(cartReducer, []);
  return (
    <>
      <button
        onClick={() => dispatch({ type: "add", item: { id: 1, name: "Book" } })}
      >
        Add
      </button>
      <button onClick={() => dispatch({ type: "clear" })}>Clear</button>
      <p>Items: {cart.length}</p>
    </>
  );
}
```

---

## Q28. `useContext` re-render problem

### English

**Question:** Why can Context cause unnecessary re-renders and how do you reduce them?

**Answer:** Every component that calls `useContext(SomeContext)` re-renders whenever the Provider's `value` changes, even if it only uses a small part of that value. Also, `value={{ a, b }}` creates a new object each render of the Provider, triggering consumers every time. Fixes: memoize the value with `useMemo`; split into several smaller contexts (for example, separate state and dispatch contexts); keep the Provider's state low in the tree; or use a state library with selectors (Zustand, Redux Toolkit).

### বাংলা

**প্রশ্ন:** Context কেন অপ্রয়োজনীয় re-render ঘটায় এবং কীভাবে কমাবেন?

**উত্তর:** যে component `useContext(SomeContext)` call করে, Provider-এর `value` বদলালেই সেটি আবার render হয়, সেই value-র ছোট একটি অংশ ব্যবহার করলেও। তাছাড়া `value={{ a, b }}` লিখলে Provider-এর প্রতিটি render-এ নতুন object তৈরি হয়, ফলে consumer গুলো প্রতিবার ট্রিগার হয়। সমাধান: `useMemo` দিয়ে value memoize করুন; একাধিক ছোট context-এ ভাগ করুন (যেমন state ও dispatch আলাদা context); Provider-এর state tree-র নিচের দিকে রাখুন; অথবা selector সমর্থন করা state library (Zustand, Redux Toolkit) ব্যবহার করুন।

### Example

```jsx
const StateContext = createContext(null);
const DispatchContext = createContext(null);

function AppProvider({ children }) {
  const [state, dispatch] = useReducer(reducer, initialState);
  return (
    <DispatchContext.Provider value={dispatch}>
      {" "}
      {/* stable, never re-renders consumers */}
      <StateContext.Provider value={state}>{children}</StateContext.Provider>
    </DispatchContext.Provider>
  );
}
```

---

## Q29. Custom hooks

### English

**Question:** What are custom hooks? Write one.

**Answer:** A custom hook is a function whose name starts with `use` and that calls other hooks to extract and reuse stateful logic between components. Each component that calls it gets its own independent state. They replace patterns like HOCs and render props in most cases. Common examples: `useFetch`, `useDebounce`, `useLocalStorage`, `useToggle`, `useWindowSize`.

### বাংলা

**প্রশ্ন:** Custom hook কী? একটি লিখে দেখান।

**উত্তর:** Custom hook হলো এমন function যার নাম `use` দিয়ে শুরু হয় এবং যা অন্য hook call করে, component-গুলোর মধ্যে state-সম্পর্কিত logic আলাদা করে পুনর্ব্যবহার করতে। যে component এটি call করে সে নিজস্ব স্বাধীন state পায়। বেশিরভাগ ক্ষেত্রে এটি HOC ও render props-এর জায়গা নেয়। সাধারণ উদাহরণ: `useFetch`, `useDebounce`, `useLocalStorage`, `useToggle`, `useWindowSize`।

### Example

```jsx
function useDebounce(value, delay = 500) {
  const [debounced, setDebounced] = useState(value);
  useEffect(() => {
    const id = setTimeout(() => setDebounced(value), delay);
    return () => clearTimeout(id);
  }, [value, delay]);
  return debounced;
}

function useLocalStorage(key, initial) {
  const [value, setValue] = useState(() => {
    try {
      const saved = localStorage.getItem(key);
      return saved ? JSON.parse(saved) : initial;
    } catch {
      return initial;
    }
  });
  useEffect(
    () => localStorage.setItem(key, JSON.stringify(value)),
    [key, value],
  );
  return [value, setValue];
}

// usage
const search = useDebounce(query, 400);
const [theme, setTheme] = useLocalStorage("theme", "light");
```

---

## Q30. Rules of hooks

### English

**Question:** What are the rules of hooks and why do they exist?

**Answer:** (1) Call hooks only at the top level of a component or custom hook; never inside loops, conditions or nested functions. (2) Call hooks only from React function components or other custom hooks, not regular functions. React tracks hooks by their call order on each render, so the order must be identical every time; breaking this mixes up state. The `eslint-plugin-react-hooks` rules enforce this automatically.

### বাংলা

**প্রশ্ন:** Hook-এর নিয়মগুলো কী এবং কেন আছে?

**উত্তর:** (১) Hook শুধু component বা custom hook-এর একদম উপরের স্তরে call করুন; loop, শর্ত বা nested function-এর ভেতরে নয়। (২) Hook শুধু React function component বা অন্য custom hook থেকে call করুন, সাধারণ function থেকে নয়। React প্রতিটি render-এ hook-গুলো call-এর ক্রম ধরে ট্র্যাক করে, তাই ক্রম প্রতিবার একই থাকতে হবে; ভাঙলে state এলোমেলো হয়ে যায়। `eslint-plugin-react-hooks` এই নিয়ম স্বয়ংক্রিয়ভাবে যাচাই করে।

### Example

```jsx
function Bad({ show }) {
  if (show) {
    const [count, setCount] = useState(0); // wrong: conditional hook
  }
}

function Good({ show }) {
  const [count, setCount] = useState(0); // always at the top
  if (!show) return null;
  return <p>{count}</p>;
}
```

---

## Q31. What triggers a re-render

### English

**Question:** When does a React component re-render?

**Answer:** A component re-renders when: its own state changes (including via `useReducer`), its parent re-renders (even if its props are the same, unless wrapped in `React.memo`), or a context it consumes changes. Calling a setter with the identical value (compared with `Object.is`) is skipped. Changing a ref or a regular variable does not trigger a re-render. A re-render means React calls the component function again; it does not necessarily mean the real DOM changes.

### বাংলা

**প্রশ্ন:** React component কখন re-render হয়?

**উত্তর:** Component re-render হয় যখন: তার নিজের state বদলায় (`useReducer` সহ), তার parent re-render হয় (props একই থাকলেও, যদি `React.memo` দিয়ে মোড়ানো না থাকে), অথবা সে যে context ব্যবহার করে তা বদলায়। Setter-এ একই মান (`Object.is` দিয়ে তুলনা) দিলে re-render বাদ যায়। Ref বা সাধারণ variable বদলালে re-render হয় না। Re-render মানে React component function আবার call করে; এর মানে এই নয় যে আসল DOM বদলাবেই।

### Example

```jsx
function Parent() {
  const [count, setCount] = useState(0);
  return (
    <>
      <button onClick={() => setCount(count + 1)}>{count}</button>
      <Child /> {/* re-renders every time Parent renders */}
    </>
  );
}

const Child = React.memo(function Child() {
  return <p>I only render once</p>;
});
```

---

## Q32. Performance optimization techniques

### English

**Question:** How do you optimize a React application's performance?

**Answer:**

- Avoid unnecessary re-renders: `React.memo`, `useMemo`, `useCallback`, and keep state close to where it is used.
- Code splitting and lazy loading with `React.lazy` and `Suspense`.
- Virtualize long lists (`react-window`, `@tanstack/react-virtual`).
- Use stable, unique `key` props.
- Debounce/throttle expensive handlers.
- Optimize images (lazy loading, modern formats) and bundle size (analyze, tree shake).
- Use the React DevTools Profiler to measure first; do not optimize blindly.

### বাংলা

**প্রশ্ন:** React application-এর performance কীভাবে বাড়াবেন?

**উত্তর:**

- অপ্রয়োজনীয় re-render আটকান: `React.memo`, `useMemo`, `useCallback`, এবং state যেখানে ব্যবহার হয় তার কাছে রাখুন।
- `React.lazy` ও `Suspense` দিয়ে code splitting ও lazy loading।
- লম্বা list virtualize করুন (`react-window`, `@tanstack/react-virtual`)।
- স্থির ও অনন্য `key` ব্যবহার করুন।
- ভারী handler-এ debounce/throttle দিন।
- ছবি (lazy loading, আধুনিক format) ও bundle size (বিশ্লেষণ, tree shaking) ঠিক করুন।
- আগে React DevTools Profiler দিয়ে মাপুন; অন্ধভাবে optimize করবেন না।

### Example

```jsx
import { FixedSizeList } from "react-window";

function BigList({ items }) {
  return (
    <FixedSizeList
      height={400}
      itemCount={items.length}
      itemSize={40}
      width="100%"
    >
      {({ index, style }) => <div style={style}>{items[index].name}</div>}
    </FixedSizeList>
  );
}

<img src="photo.webp" loading="lazy" alt="Product" />;
```

---

## Q33. Code splitting, `lazy` and `Suspense`

### English

**Question:** What are code splitting, `React.lazy` and `Suspense`?

**Answer:** Code splitting breaks the bundle into smaller chunks that load on demand, so users download less JavaScript up front and the first page loads faster. `React.lazy(() => import("./Page"))` loads a component only when it is first rendered, and `<Suspense fallback={...}>` shows a fallback UI while it loads. A common place to split is per route.

### বাংলা

**প্রশ্ন:** Code splitting, `React.lazy` ও `Suspense` কী?

**উত্তর:** Code splitting হলো bundle-কে ছোট ছোট অংশে ভাগ করা, যেগুলো দরকার হলে লোড হয়, ফলে ব্যবহারকারীকে শুরুতে কম JavaScript নামাতে হয় এবং প্রথম পেজ দ্রুত খোলে। `React.lazy(() => import("./Page"))` কোনো component প্রথমবার render হওয়ার সময়ই লোড করে, আর `<Suspense fallback={...}>` লোড হওয়ার সময় বিকল্প UI দেখায়। সাধারণত প্রতিটি route-এ আলাদা করা হয়।

### Example

```jsx
import { lazy, Suspense } from "react";
import { Routes, Route } from "react-router-dom";

const Dashboard = lazy(() => import("./pages/Dashboard"));
const Settings = lazy(() => import("./pages/Settings"));

function App() {
  return (
    <Suspense fallback={<p>Loading...</p>}>
      <Routes>
        <Route path="/" element={<Dashboard />} />
        <Route path="/settings" element={<Settings />} />
      </Routes>
    </Suspense>
  );
}
```

---

## Q34. State management: Context vs Redux vs Zustand

### English

**Question:** How do you choose between Context, Redux Toolkit and Zustand?

**Answer:** First separate server state (API data, best handled by React Query/SWR) from client state (UI state). Use local `useState` when possible. Context suits rarely changing global values (theme, auth user). Redux Toolkit suits large apps with complex, shared state, strict structure, middleware and excellent DevTools; it reduces Redux boilerplate with `createSlice`. Zustand is a small, minimal store with selectors and little boilerplate, a good middle ground. Choose the simplest tool that fits the problem.

### বাংলা

**প্রশ্ন:** Context, Redux Toolkit ও Zustand-এর মধ্যে কীভাবে বেছে নেবেন?

**উত্তর:** আগে server state (API data, যা React Query/SWR ভালো সামলায়) আর client state (UI state) আলাদা করুন। সম্ভব হলে local `useState` ব্যবহার করুন। কম বদলায় এমন global মানের (theme, লগইন করা user) জন্য Context উপযুক্ত। বড় app-এ জটিল shared state, কঠোর কাঠামো, middleware ও চমৎকার DevTools দরকার হলে Redux Toolkit ভালো; `createSlice` দিয়ে এটি Redux-এর boilerplate কমায়। Zustand ছোট ও সহজ store, selector আছে এবং boilerplate কম, মাঝামাঝি ভালো বিকল্প। সমস্যার জন্য সবচেয়ে সহজ যে tool যথেষ্ট সেটিই বেছে নিন।

### Example

```jsx
// Redux Toolkit slice
import { createSlice, configureStore } from "@reduxjs/toolkit";

const counterSlice = createSlice({
  name: "counter",
  initialState: { value: 0 },
  reducers: {
    increment: (state) => {
      state.value += 1;
    }, // Immer allows "mutation"
  },
});
export const { increment } = counterSlice.actions;
export const store = configureStore({
  reducer: { counter: counterSlice.reducer },
});

// Zustand store
import { create } from "zustand";
const useCounter = create((set) => ({
  count: 0,
  increment: () => set((s) => ({ count: s.count + 1 })),
}));

const count = useCounter((s) => s.count); // selector: only re-renders on change
```

---

## Q35. Data fetching: `useEffect` vs React Query

### English

**Question:** How do you fetch data in React? Why use React Query (TanStack Query)?

**Answer:** The basic way is `fetch` inside `useEffect` with loading and error state, plus handling race conditions and cleanup manually. React Query manages server state for you: caching, background refetching, request deduplication, retries, pagination, `loading/error` states and mutations with cache invalidation, with much less code. In frameworks like Next.js, data can also be fetched on the server (Server Components).

### বাংলা

**প্রশ্ন:** React-এ data কীভাবে আনেন? React Query (TanStack Query) কেন ব্যবহার করবেন?

**উত্তর:** মৌলিক উপায় হলো `useEffect`-এর ভেতরে `fetch` করা, সাথে loading ও error state এবং race condition ও cleanup নিজে সামলানো। React Query আপনার হয়ে server state সামলায়: caching, background-এ আবার আনা, একই request এর পুনরাবৃত্তি রোধ, retry, pagination, `loading/error` অবস্থা এবং cache invalidate করে mutation, অনেক কম code-এ। Next.js-এর মতো framework-এ server-এও data আনা যায় (Server Components)।

### Example

```jsx
import { useQuery, useMutation, useQueryClient } from "@tanstack/react-query";

function Users() {
  const { data, isLoading, error } = useQuery({
    queryKey: ["users"],
    queryFn: () => fetch("/api/users").then((r) => r.json()),
    staleTime: 60_000,
  });

  const queryClient = useQueryClient();
  const addUser = useMutation({
    mutationFn: (user) =>
      fetch("/api/users", { method: "POST", body: JSON.stringify(user) }),
    onSuccess: () => queryClient.invalidateQueries({ queryKey: ["users"] }),
  });

  if (isLoading) return <p>Loading...</p>;
  if (error) return <p>Error: {error.message}</p>;
  return (
    <ul>
      {data.map((u) => (
        <li key={u.id}>{u.name}</li>
      ))}
    </ul>
  );
}
```

---

## Q36. Forms in React

### English

**Question:** How do you handle forms and validation in React?

**Answer:** For simple forms, use controlled inputs with `useState` and validate on change or submit. For larger forms, use React Hook Form, which uses uncontrolled inputs with refs, so it re-renders much less and has built-in validation, error handling and performance. Combine it with a schema library like Zod or Yup for type-safe validation. Always validate again on the server; client validation is only for user experience.

### বাংলা

**প্রশ্ন:** React-এ form ও validation কীভাবে সামলান?

**উত্তর:** সহজ form-এ `useState` দিয়ে controlled input ব্যবহার করুন এবং change বা submit-এর সময় validate করুন। বড় form-এ React Hook Form ব্যবহার করুন; এটি ref-সহ uncontrolled input ব্যবহার করে, তাই re-render অনেক কম হয়, এবং validation, error সামলানো ও performance ভেতরেই আছে। Type-নিরাপদ validation-এর জন্য Zod বা Yup-এর মতো schema library-র সাথে মেলান। Server-এও সবসময় আবার validate করুন; client-এর validation শুধু ব্যবহারকারীর সুবিধার জন্য।

### Example

```jsx
import { useForm } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import { z } from "zod";

const schema = z.object({
  email: z.string().email("Invalid email"),
  password: z.string().min(8, "At least 8 characters"),
});

function LoginForm() {
  const {
    register,
    handleSubmit,
    formState: { errors },
  } = useForm({
    resolver: zodResolver(schema),
  });

  return (
    <form onSubmit={handleSubmit((data) => console.log(data))}>
      <input {...register("email")} placeholder="Email" />
      {errors.email && <span>{errors.email.message}</span>}
      <input type="password" {...register("password")} />
      {errors.password && <span>{errors.password.message}</span>}
      <button type="submit">Login</button>
    </form>
  );
}
```

---

## Q37. React Router

### English

**Question:** How does client-side routing work with React Router? Name the key hooks.

**Answer:** React Router changes the URL and renders the matching component without a full page reload, using the browser History API. Key pieces: `BrowserRouter`, `Routes`/`Route`, `Link`/`NavLink` (no reload, unlike `<a>`), nested routes with `Outlet`, dynamic params (`/users/:id`), and hooks `useNavigate`, `useParams`, `useLocation` and `useSearchParams`. Configure a catch-all route for 404 pages.

### বাংলা

**প্রশ্ন:** React Router-এ client-side routing কীভাবে কাজ করে? প্রধান hook গুলো বলুন।

**উত্তর:** React Router browser-এর History API ব্যবহার করে পুরো পেজ reload না করে URL বদলায় এবং মিলে যাওয়া component render করে। প্রধান অংশ: `BrowserRouter`, `Routes`/`Route`, `Link`/`NavLink` (`<a>`-এর মতো reload করে না), `Outlet` দিয়ে nested route, dynamic param (`/users/:id`), এবং hook: `useNavigate`, `useParams`, `useLocation`, `useSearchParams`। 404 পেজের জন্য একটি catch-all route রাখুন।

### Example

```jsx
import {
  BrowserRouter,
  Routes,
  Route,
  Link,
  useParams,
  useNavigate,
} from "react-router-dom";

function UserPage() {
  const { id } = useParams();
  const navigate = useNavigate();
  return (
    <>
      <h1>User {id}</h1>
      <button onClick={() => navigate(-1)}>Back</button>
    </>
  );
}

function App() {
  return (
    <BrowserRouter>
      <nav>
        <Link to="/">Home</Link> <Link to="/users/5">User 5</Link>
      </nav>
      <Routes>
        <Route path="/" element={<h1>Home</h1>} />
        <Route path="/users/:id" element={<UserPage />} />
        <Route path="*" element={<h1>404 Not Found</h1>} />
      </Routes>
    </BrowserRouter>
  );
}
```

---

## Q38. React 18 features

### English

**Question:** What is new in React 18?

**Answer:**

- Automatic batching: multiple state updates are batched even inside promises, timeouts and native events.
- Concurrent rendering: React can interrupt and prioritize rendering work.
- `useTransition` / `startTransition`: mark updates as non-urgent so typing stays responsive.
- `useDeferredValue`: defer updating a value used for heavy rendering.
- `createRoot` replaces `ReactDOM.render`.
- Suspense improvements and streaming server rendering.

### বাংলা

**প্রশ্ন:** React 18-এ নতুন কী আছে?

**উত্তর:**

- Automatic batching: promise, timeout ও native event-এর ভেতরেও একাধিক state আপডেট একসাথে batch হয়।
- Concurrent rendering: React render-এর কাজ থামাতে ও অগ্রাধিকার দিতে পারে।
- `useTransition` / `startTransition`: কিছু আপডেটকে কম জরুরি চিহ্নিত করে, ফলে টাইপ করার সময় UI সাড়া দেয়।
- `useDeferredValue`: ভারী render-এ ব্যবহৃত মানের আপডেট পিছিয়ে দেয়।
- `ReactDOM.render`-এর জায়গায় `createRoot`।
- Suspense-এর উন্নতি ও streaming server rendering।

### Example

```jsx
function SearchPage() {
  const [query, setQuery] = useState("");
  const [results, setResults] = useState([]);
  const [isPending, startTransition] = useTransition();

  function handleChange(e) {
    setQuery(e.target.value); // urgent: update input now
    startTransition(() => {
      setResults(filterHugeList(e.target.value)); // non-urgent
    });
  }

  return (
    <>
      <input value={query} onChange={handleChange} />
      {isPending ? <p>Updating...</p> : <ResultList items={results} />}
    </>
  );
}
```

---

## Q39. Server Components vs Client Components

### English

**Question:** What are React Server Components and how do they differ from Client Components?

**Answer:** Server Components run only on the server (for example in the Next.js App Router) and send rendered output to the browser; they add no JavaScript to the client bundle and can access databases, files and secrets directly. They cannot use state, effects or browser APIs. Client Components (marked with `"use client"`) are hydrated in the browser and support state, effects and event handlers. Best practice: keep components as Server Components by default and use Client Components only for interactivity.

### বাংলা

**প্রশ্ন:** React Server Component কী এবং Client Component থেকে কীভাবে আলাদা?

**উত্তর:** Server Component শুধু server-এ চলে (যেমন Next.js App Router-এ) এবং render করা ফলাফল browser-এ পাঠায়; client bundle-এ কোনো JavaScript যোগ করে না এবং সরাসরি database, file ও secret ব্যবহার করতে পারে। এতে state, effect বা browser API ব্যবহার করা যায় না। Client Component (`"use client"` দিয়ে চিহ্নিত) browser-এ hydrate হয় এবং state, effect ও event handler সমর্থন করে। ভালো অভ্যাস: ডিফল্টভাবে Server Component রাখুন, শুধু interactivity-র জন্য Client Component ব্যবহার করুন।

### Example

```jsx
// app/users/page.jsx  (Server Component by default)
import LikeButton from "./LikeButton";

export default async function UsersPage() {
  const users = await db.user.findMany();       // direct DB access on the server
  return (
    <ul>
      {users.map((u) => (
        <li key={u.id}>{u.name} <LikeButton id={u.id} /></li>
      ))}
    </ul>
  );
}

// app/users/LikeButton.jsx
"use client";
import { useState } from "react";
export default function LikeButton() {
  const [liked, setLiked] = useState(false);
  return <button onClick={() => setLiked(!liked)}>{liked ? "Liked" : "Like"}</button>;
}
```

---

## Q40. Protected routes and authentication

### English

**Question:** How do you implement protected routes and authentication in a React app?

**Answer:** Keep the auth state (user and token status) in a context or store. Create a `ProtectedRoute` wrapper that checks it: if the user is not authenticated, redirect to `/login`; otherwise render the child routes with `<Outlet />`. Prefer `HttpOnly` cookies for tokens over `localStorage` (safer against XSS), refresh tokens on expiry (axios interceptor) and show a loading state while the session is being verified. Remember that client-side protection is only for UX; real authorization must be enforced on the server.

### বাংলা

**প্রশ্ন:** React app-এ protected route ও authentication কীভাবে বানাবেন?

**উত্তর:** Auth state (user ও token-এর অবস্থা) context বা store-এ রাখুন। একটি `ProtectedRoute` wrapper বানান যা এটি যাচাই করে: user লগইন করা না থাকলে `/login`-এ পাঠায়, নইলে `<Outlet />` দিয়ে child route গুলো দেখায়। Token রাখতে `localStorage`-এর চেয়ে `HttpOnly` cookie ভালো (XSS-এর বিরুদ্ধে নিরাপদ), মেয়াদ শেষে token refresh করুন (axios interceptor) এবং session যাচাইয়ের সময় loading অবস্থা দেখান। মনে রাখবেন client-side সুরক্ষা শুধু ব্যবহারকারীর সুবিধার জন্য; আসল authorization অবশ্যই server-এ প্রয়োগ করতে হবে।

### Example

```jsx
import { Navigate, Outlet, useLocation } from "react-router-dom";

function ProtectedRoute() {
  const { user, loading } = useAuth();
  const location = useLocation();

  if (loading) return <p>Checking session...</p>;
  if (!user) return <Navigate to="/login" state={{ from: location }} replace />;
  return <Outlet />;
}

<Routes>
  <Route path="/login" element={<Login />} />
  <Route element={<ProtectedRoute />}>
    <Route path="/dashboard" element={<Dashboard />} />
    <Route path="/profile" element={<Profile />} />
  </Route>
</Routes>;
```
