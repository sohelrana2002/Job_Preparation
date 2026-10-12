# React.js Core Concepts (Q1 - Q20)

> Place in: `02-frontend/react/01-core-concepts.md`
> Hooks and performance questions (Q21-Q40) are in `02-hooks-and-performance.md`.

## Top 40 Index

| #   | Topic                                         | File |
| --- | --------------------------------------------- | ---- |
| 1   | What is React? Library vs framework           | 01   |
| 2   | Virtual DOM and reconciliation                | 01   |
| 3   | JSX                                           | 01   |
| 4   | Functional vs class components                | 01   |
| 5   | Props vs state                                | 01   |
| 6   | Props drilling and solutions                  | 01   |
| 7   | Controlled vs uncontrolled components         | 01   |
| 8   | Keys in lists                                 | 01   |
| 9   | Lifting state up                              | 01   |
| 10  | Conditional rendering                         | 01   |
| 11  | Fragments                                     | 01   |
| 12  | Component lifecycle                           | 01   |
| 13  | Synthetic events                              | 01   |
| 14  | Refs                                          | 01   |
| 15  | `children` and composition                    | 01   |
| 16  | Higher-order components                       | 01   |
| 17  | Error boundaries                              | 01   |
| 18  | Portals                                       | 01   |
| 19  | Context API                                   | 01   |
| 20  | Strict Mode                                   | 01   |
| 21  | useState                                      | 02   |
| 22  | useEffect                                     | 02   |
| 23  | useEffect vs useLayoutEffect                  | 02   |
| 24  | useRef                                        | 02   |
| 25  | useMemo vs useCallback                        | 02   |
| 26  | React.memo                                    | 02   |
| 27  | useReducer vs useState                        | 02   |
| 28  | useContext re-render problem                  | 02   |
| 29  | Custom hooks                                  | 02   |
| 30  | Rules of hooks                                | 02   |
| 31  | What triggers a re-render                     | 02   |
| 32  | Performance optimization techniques           | 02   |
| 33  | Code splitting, lazy, Suspense                | 02   |
| 34  | State management: Context vs Redux vs Zustand | 02   |
| 35  | Data fetching: useEffect vs React Query       | 02   |
| 36  | Forms in React                                | 02   |
| 37  | React Router                                  | 02   |
| 38  | React 18 features                             | 02   |
| 39  | Server vs Client Components                   | 02   |
| 40  | Protected routes and authentication           | 02   |

---

## Q1. What is React? Library vs framework

### English

**Question:** What is React and why is it used? Is it a library or a framework?

**Answer:** React is a JavaScript library for building user interfaces using reusable components. It is a library, not a full framework, because it handles only the view layer; routing, data fetching and state management are chosen separately (React Router, React Query, Redux, etc.). Main benefits: component reusability, declarative UI, the virtual DOM for efficient updates, one-way data flow and a large ecosystem.

### বাংলা

**প্রশ্ন:** React কী এবং কেন ব্যবহার করা হয়? এটি library নাকি framework?

**উত্তর:** React হলো user interface বানানোর জন্য একটি JavaScript library, যেখানে পুনরায় ব্যবহারযোগ্য component দিয়ে UI তৈরি হয়। এটি পুরো framework নয়, library, কারণ এটি শুধু view অংশ সামলায়; routing, data fetching ও state management আলাদাভাবে বেছে নিতে হয় (React Router, React Query, Redux ইত্যাদি)। প্রধান সুবিধা: component পুনর্ব্যবহার, declarative UI, কার্যকর আপডেটের জন্য virtual DOM, একমুখী data flow এবং বিশাল ecosystem।

### Example

```jsx
function Greeting({ name }) {
  return <h1>Hello, {name}!</h1>;
}

export default function App() {
  return (
    <div>
      <Greeting name="Sohel" />
      <Greeting name="Rana" />
    </div>
  );
}
```

---

## Q2. Virtual DOM and reconciliation

### English

**Question:** What is the Virtual DOM and how does reconciliation work?

**Answer:** The Virtual DOM is a lightweight JavaScript representation of the real DOM. When state changes, React builds a new Virtual DOM tree, compares it with the previous one (diffing), finds the minimal set of changes and applies only those to the real DOM (reconciliation). The diffing algorithm assumes elements of different types produce different trees, and uses `key` props to match items in lists. It is faster than rewriting the whole DOM because real DOM updates are expensive.

### বাংলা

**প্রশ্ন:** Virtual DOM কী এবং reconciliation কীভাবে কাজ করে?

**উত্তর:** Virtual DOM হলো আসল DOM-এর একটি হালকা JavaScript প্রতিরূপ। State বদলালে React নতুন একটি Virtual DOM tree বানায়, আগেরটির সাথে তুলনা করে (diffing), ন্যূনতম কী কী বদলাতে হবে খুঁজে বের করে এবং শুধু সেটুকুই আসল DOM-এ প্রয়োগ করে (reconciliation)। Diffing algorithm ধরে নেয় ভিন্ন type-এর element ভিন্ন tree বানায়, আর list-এর উপাদান মেলাতে `key` ব্যবহার করে। আসল DOM আপডেট ব্যয়বহুল, তাই পুরো DOM নতুন করে লেখার চেয়ে এটি দ্রুত।

### Example

```jsx
function Counter() {
  const [count, setCount] = useState(0);
  // Only the text node inside <p> changes in the real DOM;
  // <button> and <h1> are untouched.
  return (
    <div>
      <h1>Counter</h1>
      <p>{count}</p>
      <button onClick={() => setCount(count + 1)}>Add</button>
    </div>
  );
}
```

---

## Q3. JSX

### English

**Question:** What is JSX? Can browsers read it?

**Answer:** JSX is a syntax extension that lets you write HTML-like markup inside JavaScript. Browsers cannot read it; tools like Babel or SWC compile it into `React.createElement` calls (or the newer `jsx()` runtime). Rules: return a single root element (or a Fragment), use `className` instead of `class`, use camelCase attributes (`onClick`), close all tags, and put JavaScript expressions inside `{}`.

### বাংলা

**প্রশ্ন:** JSX কী? Browser কি এটি সরাসরি পড়তে পারে?

**উত্তর:** JSX হলো একটি syntax extension, যা দিয়ে JavaScript-এর ভেতরে HTML-এর মতো markup লেখা যায়। Browser এটি পড়তে পারে না; Babel বা SWC-র মতো tool এটিকে `React.createElement` call-এ (বা নতুন `jsx()` runtime-এ) রূপান্তর করে। নিয়ম: একটিমাত্র root element (বা Fragment) return করতে হবে, `class`-এর বদলে `className`, camelCase attribute (`onClick`), সব tag বন্ধ করতে হবে এবং JavaScript expression `{}`-এর ভেতরে লিখতে হবে।

### Example

```jsx
const element = <h1 className="title">Hello, {user.name}</h1>;

// compiles to roughly:
const element2 = React.createElement(
  "h1",
  { className: "title" },
  "Hello, ",
  user.name,
);
```

---

## Q4. Functional vs class components

### English

**Question:** What is the difference between functional and class components?

**Answer:** Functional components are plain functions that return JSX and use Hooks (`useState`, `useEffect`) for state and lifecycle. Class components extend `React.Component`, use `this.state`, `this.setState` and lifecycle methods (`componentDidMount`, etc.). Functional components are the modern standard: shorter code, easier logic reuse through custom hooks, no `this` confusion. Class components are still supported, and error boundaries still require a class.

### বাংলা

**প্রশ্ন:** Functional component আর class component-এর পার্থক্য কী?

**উত্তর:** Functional component হলো সাধারণ function, যা JSX return করে এবং state ও lifecycle-এর জন্য Hook (`useState`, `useEffect`) ব্যবহার করে। Class component `React.Component` extend করে এবং `this.state`, `this.setState` ও lifecycle method (`componentDidMount` ইত্যাদি) ব্যবহার করে। Functional component এখনকার স্ট্যান্ডার্ড: কম code, custom hook দিয়ে logic পুনর্ব্যবহার সহজ, `this`-এর ঝামেলা নেই। Class component এখনো চলে, এবং error boundary এখনো class দিয়েই বানাতে হয়।

### Example

```jsx
// Class
class Counter extends React.Component {
  state = { count: 0 };
  render() {
    return (
      <button onClick={() => this.setState({ count: this.state.count + 1 })}>
        {this.state.count}
      </button>
    );
  }
}

// Functional
function Counter() {
  const [count, setCount] = useState(0);
  return <button onClick={() => setCount(count + 1)}>{count}</button>;
}
```

---

## Q5. Props vs state

### English

**Question:** What is the difference between props and state?

**Answer:** Props are inputs passed from a parent to a child; they are read-only and must not be modified by the child. State is data owned and managed inside a component that can change over time; updating it triggers a re-render. Data flows down through props (one-way), and a child communicates up by calling functions passed as props.

### বাংলা

**প্রশ্ন:** Props আর state-এর পার্থক্য কী?

**উত্তর:** Props হলো parent থেকে child-এ পাঠানো input; এগুলো read-only, child এদের বদলাতে পারে না। State হলো component-এর নিজস্ব data, যা সময়ের সাথে বদলাতে পারে এবং বদলালে re-render হয়। Data props দিয়ে নিচের দিকে যায় (একমুখী), আর child উপরে জানাতে চাইলে props হিসেবে পাওয়া function call করে।

### Example

```jsx
function Parent() {
  const [count, setCount] = useState(0); // state
  return <Child count={count} onAdd={() => setCount((c) => c + 1)} />;
}

function Child({ count, onAdd }) {
  // props (read-only)
  // count = 5;  // wrong: never modify props
  return <button onClick={onAdd}>Clicked {count}</button>;
}
```

---

## Q6. Props drilling

### English

**Question:** What is props drilling and how do you avoid it?

**Answer:** Props drilling is passing data through many intermediate components that do not need it, just to reach a deeply nested child. It makes code harder to maintain. Solutions: Context API for global-ish data (theme, user), component composition (passing `children` or elements as props), and state management libraries (Redux Toolkit, Zustand) for complex shared state.

### বাংলা

**প্রশ্ন:** Props drilling কী এবং কীভাবে এড়ানো যায়?

**উত্তর:** Props drilling হলো গভীরে থাকা কোনো child-এর কাছে data পৌঁছাতে মাঝের এমন অনেক component-এর মধ্য দিয়ে props পাঠানো, যাদের ওই data দরকারই নেই। এতে code রক্ষণাবেক্ষণ কঠিন হয়। সমাধান: global ধরনের data (theme, user) এর জন্য Context API, component composition (`children` বা element props হিসেবে পাঠানো), এবং জটিল shared state-এর জন্য state management library (Redux Toolkit, Zustand)।

### Example

```jsx
const UserContext = createContext(null);

function App() {
  const [user] = useState({ name: "Sohel" });
  return (
    <UserContext.Provider value={user}>
      <Layout />
    </UserContext.Provider>
  );
}

function Layout() {
  return <Sidebar />;
} // no user prop needed
function Sidebar() {
  return <Avatar />;
}
function Avatar() {
  const user = useContext(UserContext); // read directly
  return <span>{user.name}</span>;
}
```

---

## Q7. Controlled vs uncontrolled components

### English

**Question:** What is the difference between controlled and uncontrolled components?

**Answer:** In a controlled component, form data is handled by React state: the input's `value` comes from state and `onChange` updates it, so React is the single source of truth. In an uncontrolled component, the DOM keeps the data and you read it when needed using a `ref`. Controlled is better for validation and instant feedback; uncontrolled is simpler and slightly lighter for basic forms or file inputs.

### বাংলা

**প্রশ্ন:** Controlled আর uncontrolled component-এর পার্থক্য কী?

**উত্তর:** Controlled component-এ form-এর data React state সামলায়: input-এর `value` state থেকে আসে এবং `onChange` সেটি আপডেট করে, ফলে React-ই একমাত্র সত্যের উৎস। Uncontrolled component-এ data DOM নিজেই রাখে এবং দরকারের সময় `ref` দিয়ে পড়া হয়। Validation ও তাৎক্ষণিক feedback-এর জন্য controlled ভালো; সাধারণ form বা file input-এর জন্য uncontrolled সহজ ও হালকা।

### Example

```jsx
function Controlled() {
  const [email, setEmail] = useState("");
  return <input value={email} onChange={(e) => setEmail(e.target.value)} />;
}

function Uncontrolled() {
  const inputRef = useRef(null);
  const submit = () => console.log(inputRef.current.value);
  return (
    <>
      <input ref={inputRef} defaultValue="" />
      <button onClick={submit}>Submit</button>
    </>
  );
}
```

---

## Q8. Keys in lists

### English

**Question:** Why are keys important in lists? Why is using the array index as a key a bad idea?

**Answer:** A `key` is a stable, unique identifier that helps React match list items between renders, so it can update, add, remove or reorder items correctly and efficiently. Using the index as a key is risky when the list can be reordered, filtered or have items inserted/removed, because the same index will point to different data, causing wrong UI state (for example, input values jumping to the wrong row). Use a unique id from your data.

### বাংলা

**প্রশ্ন:** List-এ key কেন গুরুত্বপূর্ণ? Array index-কে key বানানো কেন খারাপ?

**উত্তর:** `key` হলো একটি স্থায়ী ও অনন্য পরিচয়, যা React-কে এক render থেকে আরেক render-এ list-এর উপাদান মেলাতে সাহায্য করে, ফলে সে সঠিকভাবে ও কার্যকরভাবে উপাদান আপডেট, যোগ, মুছে ফেলা বা ক্রম বদল করতে পারে। List-এর ক্রম বদলালে, filter হলে বা মাঝে কিছু যোগ/বাদ গেলে index key ঝুঁকিপূর্ণ, কারণ একই index তখন ভিন্ন data-কে নির্দেশ করে এবং UI-তে ভুল state দেখা যায় (যেমন input-এর মান ভুল সারিতে চলে যায়)। Data-র নিজস্ব unique id ব্যবহার করুন।

### Example

```jsx
const todos = [
  { id: "a1", text: "Learn React" },
  { id: "b2", text: "Build project" },
];

// Good
<ul>
  {todos.map((todo) => (
    <li key={todo.id}>{todo.text}</li>
  ))}
</ul>;

// Risky when list order can change
{
  todos.map((todo, index) => <li key={index}>{todo.text}</li>);
}
```

---

## Q9. Lifting state up

### English

**Question:** What does "lifting state up" mean?

**Answer:** When two or more sibling components need the same data, you move that state to their closest common parent and pass it down via props, along with setter functions. This keeps a single source of truth and keeps the components in sync.

### বাংলা

**প্রশ্ন:** "Lifting state up" মানে কী?

**উত্তর:** দুই বা তার বেশি sibling component-এর একই data দরকার হলে, সেই state-টি তাদের নিকটতম common parent-এ নিয়ে যাওয়া হয় এবং props ও setter function দিয়ে নিচে পাঠানো হয়। এতে একটিমাত্র সত্যের উৎস থাকে এবং component গুলো সবসময় সমন্বিত থাকে।

### Example

```jsx
function Converter() {
  const [celsius, setCelsius] = useState(0);
  return (
    <>
      <CelsiusInput value={celsius} onChange={setCelsius} />
      <FahrenheitDisplay value={(celsius * 9) / 5 + 32} />
    </>
  );
}

const CelsiusInput = ({ value, onChange }) => (
  <input
    type="number"
    value={value}
    onChange={(e) => onChange(+e.target.value)}
  />
);
const FahrenheitDisplay = ({ value }) => <p>{value} °F</p>;
```

---

## Q10. Conditional rendering

### English

**Question:** What are the ways to render conditionally in React?

**Answer:** Use `if/else` before the return, the ternary operator `cond ? A : B`, logical AND `cond && <A />`, or return `null` to render nothing. Careful with `&&` and numbers: `count && <A />` renders `0` when `count` is `0`; use `count > 0 && <A />` instead.

### বাংলা

**প্রশ্ন:** React-এ শর্ত অনুযায়ী render করার উপায়গুলো কী কী?

**উত্তর:** return-এর আগে `if/else`, ternary operator `cond ? A : B`, logical AND `cond && <A />`, অথবা কিছুই দেখাতে না চাইলে `null` return করা যায়। `&&` আর সংখ্যার ক্ষেত্রে সতর্ক থাকুন: `count` ০ হলে `count && <A />` পর্দায় `0` দেখায়; তাই `count > 0 && <A />` লিখুন।

### Example

```jsx
function Notifications({ count, isLoggedIn }) {
  if (!isLoggedIn) return <p>Please log in</p>; // early return

  return (
    <div>
      {count > 0 ? <span>{count} new</span> : <span>No new messages</span>}
      {count > 0 && <button>Mark all read</button>}
    </div>
  );
}
```

---

## Q11. Fragments

### English

**Question:** What is a React Fragment and why use it?

**Answer:** A Fragment lets you group multiple elements without adding an extra DOM node. This keeps the HTML clean and avoids breaking layouts (for example inside tables or flex/grid). Short syntax is `<>...</>`; use `<React.Fragment key={id}>` when a key is needed.

### বাংলা

**প্রশ্ন:** React Fragment কী এবং কেন ব্যবহার করব?

**উত্তর:** Fragment দিয়ে অতিরিক্ত DOM node না বানিয়ে একাধিক element-কে একসাথে group করা যায়। এতে HTML পরিষ্কার থাকে এবং layout ভাঙে না (যেমন table বা flex/grid-এর ভেতরে)। সংক্ষিপ্ত syntax `<>...</>`; key দরকার হলে `<React.Fragment key={id}>` ব্যবহার করুন।

### Example

```jsx
function Row({ item }) {
  return (
    <>
      <td>{item.name}</td>
      <td>{item.price}</td>
    </>
  );
}

items.map((item) => (
  <React.Fragment key={item.id}>
    <dt>{item.term}</dt>
    <dd>{item.description}</dd>
  </React.Fragment>
));
```

---

## Q12. Component lifecycle

### English

**Question:** Explain the component lifecycle and how it maps to hooks.

**Answer:** A component goes through three phases: mounting (created and inserted into the DOM), updating (re-rendered when props or state change) and unmounting (removed from the DOM). In class components these are `componentDidMount`, `componentDidUpdate` and `componentWillUnmount`. With hooks, `useEffect(fn, [])` runs after mount, `useEffect(fn, [dep])` runs after updates to `dep`, and the cleanup function returned from `useEffect` runs before the next effect and on unmount.

### বাংলা

**প্রশ্ন:** Component lifecycle ব্যাখ্যা করুন এবং hook-এর সাথে কীভাবে মিলে তা বলুন।

**উত্তর:** একটি component তিন ধাপের মধ্য দিয়ে যায়: mounting (তৈরি হয়ে DOM-এ বসে), updating (props বা state বদলালে আবার render হয়) এবং unmounting (DOM থেকে সরে যায়)। Class component-এ এগুলো `componentDidMount`, `componentDidUpdate` ও `componentWillUnmount`। Hook-এ `useEffect(fn, [])` mount-এর পরে চলে, `useEffect(fn, [dep])` `dep` বদলালে চলে, আর `useEffect` থেকে return করা cleanup function পরের effect-এর আগে এবং unmount-এর সময় চলে।

### Example

```jsx
function Timer() {
  const [seconds, setSeconds] = useState(0);

  useEffect(() => {
    const id = setInterval(() => setSeconds((s) => s + 1), 1000); // mount
    return () => clearInterval(id); // unmount cleanup
  }, []);

  useEffect(() => {
    document.title = `Seconds: ${seconds}`; // on update
  }, [seconds]);

  return <p>{seconds}</p>;
}
```

---

## Q13. Synthetic events

### English

**Question:** What are synthetic events in React?

**Answer:** A `SyntheticEvent` is React's cross-browser wrapper around the native browser event, with the same interface (`preventDefault`, `stopPropagation`, `target`). React attaches handlers at the root rather than on every element (event delegation), which is efficient. Handlers are written in camelCase and receive a function, not a string. Access the native event with `e.nativeEvent`.

### বাংলা

**প্রশ্ন:** React-এ synthetic event কী?

**উত্তর:** `SyntheticEvent` হলো browser-এর আসল event-এর চারপাশে React-এর তৈরি cross-browser wrapper, যার interface একই (`preventDefault`, `stopPropagation`, `target`)। React প্রতিটি element-এ আলাদা handler না বসিয়ে root-এ বসায় (event delegation), যা কার্যকর। Handler camelCase-এ লেখা হয় এবং string নয়, function পাঠাতে হয়। আসল event পেতে `e.nativeEvent` ব্যবহার করুন।

### Example

```jsx
function Form() {
  const handleSubmit = (e) => {
    e.preventDefault(); // works like native
    console.log(e.nativeEvent); // original browser event
  };
  return (
    <form onSubmit={handleSubmit}>
      <button type="submit" onClick={(e) => e.stopPropagation()}>
        Save
      </button>
    </form>
  );
}
```

---

## Q14. Refs

### English

**Question:** What are refs and when should you use them?

**Answer:** A ref gives direct access to a DOM element or holds a mutable value that persists across renders without causing a re-render. Use refs for focusing inputs, scrolling, measuring elements, managing media playback, or integrating third-party libraries. Avoid using refs for data that should update the UI; use state for that. To pass a ref through a component to a child's DOM node, use `forwardRef` (in React 19, `ref` can be passed as a normal prop).

### বাংলা

**প্রশ্ন:** Ref কী এবং কখন ব্যবহার করবেন?

**উত্তর:** Ref দিয়ে সরাসরি কোনো DOM element-এ পৌঁছানো যায়, অথবা এমন একটি পরিবর্তনযোগ্য মান ধরে রাখা যায় যা render-এর মধ্যেও টিকে থাকে এবং বদলালে re-render হয় না। Input-এ focus দেওয়া, scroll করা, element-এর মাপ নেওয়া, media চালানো বা তৃতীয় পক্ষের library যুক্ত করতে ref ব্যবহার করুন। যে data বদলালে UI আপডেট হওয়া দরকার তার জন্য ref নয়, state ব্যবহার করুন। Child-এর DOM node-এ ref পাঠাতে `forwardRef` লাগে (React 19-এ `ref` সাধারণ prop হিসেবেই পাঠানো যায়)।

### Example

```jsx
function SearchBox() {
  const inputRef = useRef(null);

  useEffect(() => {
    inputRef.current.focus(); // focus on mount
  }, []);

  return <input ref={inputRef} placeholder="Search..." />;
}

const FancyInput = forwardRef((props, ref) => <input ref={ref} {...props} />);
```

---

## Q15. `children` prop and composition

### English

**Question:** What is the `children` prop and what is component composition?

**Answer:** `children` is a special prop containing whatever is placed between a component's opening and closing tags. Composition means building complex UIs by combining smaller components, passing content as `children` or other props, instead of using inheritance. It makes components flexible and reusable (layouts, cards, modals) and helps avoid props drilling.

### বাংলা

**প্রশ্ন:** `children` prop কী এবং component composition কী?

**উত্তর:** `children` হলো একটি বিশেষ prop, যাতে component-এর খোলা ও বন্ধ tag-এর মাঝে যা কিছু রাখা হয় তা থাকে। Composition মানে inheritance-এর বদলে ছোট ছোট component জুড়ে এবং `children` বা অন্য prop দিয়ে content পাঠিয়ে জটিল UI বানানো। এতে component নমনীয় ও পুনর্ব্যবহারযোগ্য হয় (layout, card, modal) এবং props drilling কমে।

### Example

```jsx
function Card({ title, children }) {
  return (
    <div className="card">
      <h2>{title}</h2>
      <div className="card-body">{children}</div>
    </div>
  );
}

<Card title="Profile">
  <Avatar />
  <p>Full-stack developer</p>
</Card>;
```

---

## Q16. Higher-Order Components (HOC)

### English

**Question:** What is a Higher-Order Component? Is it still used?

**Answer:** A HOC is a function that takes a component and returns a new, enhanced component, used to reuse logic such as authentication checks, logging or data injection (for example `connect` in old Redux, `withRouter`). Today, custom hooks cover most of these cases more simply, so HOCs are less common in new code, but they still appear in older codebases and are a common interview question.

### বাংলা

**প্রশ্ন:** Higher-Order Component কী? এখনো কি ব্যবহার হয়?

**উত্তর:** HOC হলো এমন function যা একটি component নেয় এবং একটি উন্নত নতুন component return করে; authentication যাচাই, logging বা data inject করার মতো logic পুনর্ব্যবহারে কাজে লাগে (যেমন পুরনো Redux-এর `connect`, `withRouter`)। এখন custom hook বেশিরভাগ ক্ষেত্রে এটি আরও সহজে করে, তাই নতুন code-এ HOC কম দেখা যায়, তবে পুরনো codebase-এ আছে এবং interview-তে প্রায়ই জিজ্ঞাসা করা হয়।

### Example

```jsx
function withAuth(WrappedComponent) {
  return function AuthenticatedComponent(props) {
    const { user } = useAuth();
    if (!user) return <Navigate to="/login" />;
    return <WrappedComponent {...props} user={user} />;
  };
}

const Dashboard = ({ user }) => <h1>Welcome, {user.name}</h1>;
export default withAuth(Dashboard);
```

---

## Q17. Error boundaries

### English

**Question:** What are error boundaries?

**Answer:** An error boundary is a class component that catches JavaScript errors in its child component tree during rendering, in lifecycle methods and in constructors, logs them and shows a fallback UI instead of crashing the whole app. It uses `static getDerivedStateFromError` and `componentDidCatch`. It does not catch errors in event handlers, asynchronous code (setTimeout, promises) or server-side rendering. The `react-error-boundary` library offers a ready-made functional wrapper.

### বাংলা

**প্রশ্ন:** Error boundary কী?

**উত্তর:** Error boundary হলো এমন একটি class component, যা তার child tree-র render, lifecycle method ও constructor-এর JavaScript error ধরে, log করে এবং পুরো app ভেঙে না দিয়ে বিকল্প (fallback) UI দেখায়। এটি `static getDerivedStateFromError` ও `componentDidCatch` ব্যবহার করে। Event handler, asynchronous code (setTimeout, promise) এবং server-side rendering-এর error এটি ধরে না। `react-error-boundary` library থেকে তৈরি functional wrapper পাওয়া যায়।

### Example

```jsx
class ErrorBoundary extends React.Component {
  state = { hasError: false };

  static getDerivedStateFromError() {
    return { hasError: true };
  }
  componentDidCatch(error, info) {
    console.error(error, info.componentStack);
  }
  render() {
    return this.state.hasError ? (
      <h2>Something went wrong.</h2>
    ) : (
      this.props.children
    );
  }
}

<ErrorBoundary>
  <UserProfile />
</ErrorBoundary>;
```

---

## Q18. Portals

### English

**Question:** What are portals and when are they useful?

**Answer:** `ReactDOM.createPortal(children, domNode)` renders children into a DOM node outside the parent component's DOM hierarchy, while keeping them in the same React tree (so context and event bubbling still work). It is used for modals, tooltips, dropdowns and toasts that must escape parent `overflow: hidden` or `z-index` problems.

### বাংলা

**প্রশ্ন:** Portal কী এবং কখন কাজে লাগে?

**উত্তর:** `ReactDOM.createPortal(children, domNode)` children-কে parent component-এর DOM কাঠামোর বাইরের একটি DOM node-এ render করে, কিন্তু React tree-তে একই জায়গায় রাখে (তাই context ও event bubbling আগের মতোই কাজ করে)। Modal, tooltip, dropdown ও toast-এ ব্যবহার হয়, যেগুলোকে parent-এর `overflow: hidden` বা `z-index` সমস্যা থেকে বের করে আনতে হয়।

### Example

```jsx
import { createPortal } from "react-dom";

function Modal({ isOpen, onClose, children }) {
  if (!isOpen) return null;
  return createPortal(
    <div className="overlay" onClick={onClose}>
      <div className="modal" onClick={(e) => e.stopPropagation()}>
        {children}
      </div>
    </div>,
    document.body,
  );
}
```

---

## Q19. Context API

### English

**Question:** What is the Context API and when should you use it?

**Answer:** Context lets you share data (theme, authenticated user, language) with any component in a tree without passing props manually. You create it with `createContext`, provide a value with `Provider`, and read it with `useContext`. Use it for low-frequency, app-wide data. Caution: every consumer re-renders when the provided value changes, so it is not ideal for frequently changing, complex state; consider splitting contexts, memoizing the value or using a state library.

### বাংলা

**প্রশ্ন:** Context API কী এবং কখন ব্যবহার করবেন?

**উত্তর:** Context দিয়ে হাতে হাতে props না পাঠিয়ে tree-র যেকোনো component-এর সাথে data (theme, লগইন করা user, ভাষা) শেয়ার করা যায়। `createContext` দিয়ে বানাতে হয়, `Provider` দিয়ে মান দিতে হয় এবং `useContext` দিয়ে পড়তে হয়। কম পরিবর্তন হয় এমন app-ব্যাপী data-র জন্য ব্যবহার করুন। সতর্কতা: Provider-এর মান বদলালে সব consumer আবার render হয়, তাই ঘনঘন বদলায় এমন জটিল state-এর জন্য উপযুক্ত নয়; context ভাগ করুন, মান memoize করুন বা state library ব্যবহার করুন।

### Example

```jsx
const ThemeContext = createContext("light");

function ThemeProvider({ children }) {
  const [theme, setTheme] = useState("light");
  const value = useMemo(() => ({ theme, setTheme }), [theme]);
  return (
    <ThemeContext.Provider value={value}>{children}</ThemeContext.Provider>
  );
}

function ThemeButton() {
  const { theme, setTheme } = useContext(ThemeContext);
  return (
    <button onClick={() => setTheme(theme === "light" ? "dark" : "light")}>
      Current: {theme}
    </button>
  );
}
```

---

## Q20. Strict Mode

### English

**Question:** What does `<React.StrictMode>` do? Why do effects run twice in development?

**Answer:** Strict Mode is a development-only tool that highlights potential problems. It intentionally double-invokes component bodies, state updaters and effects (mount, unmount, mount again) to reveal impure rendering and missing cleanup. It does not affect the production build. If an effect behaves wrongly when run twice, it usually needs a proper cleanup function.

### বাংলা

**প্রশ্ন:** `<React.StrictMode>` কী করে? Development-এ effect কেন দুইবার চলে?

**উত্তর:** Strict Mode শুধু development-এর জন্য একটি tool, যা সম্ভাব্য সমস্যা ধরিয়ে দেয়। এটি ইচ্ছা করেই component-এর body, state updater ও effect দুইবার চালায় (mount, unmount, আবার mount), যাতে impure render ও cleanup না থাকার সমস্যা ধরা পড়ে। Production build-এ এর কোনো প্রভাব নেই। কোনো effect দুইবার চললে ভুল আচরণ করলে সাধারণত তাতে সঠিক cleanup function যোগ করা দরকার।

### Example

```jsx
root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
);

useEffect(() => {
  const controller = new AbortController();
  fetch("/api/data", { signal: controller.signal })
    .then((r) => r.json())
    .then(setData)
    .catch((e) => e.name !== "AbortError" && console.error(e));
  return () => controller.abort(); // cleanup makes double-run safe
}, []);
```
