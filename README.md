# financial dashboard
  - public home page, 
  - login page, 
  - dashboard pages that are protected by authentication, 
    - ability for users to add, edit & delete invocies
(by the end you'll have the tools to starts buding full-stack nextJs applications)

## Outline: (pre-Req :React+JS (props,state,hooks & Server-Client&Suspense))
- Styling
- Opimizations
- Routing
- Data Fetching
- Search & Pagination
- Mutating Data
- Error Handling
- Form Validation and Accessiblity
- Authentications
- Metadata

# Getting Started
## Folder Structure
- app
  - lib (function store): reusable utility functions | data fetching functions,...
  - ui (ui components): cards, tables, forms,...
- public (images and stuff)


- seeding: populating Database with Initial data 
- prisma/drizzle auto-generate types based on your database schema (TypeScript stuff)


## Run your first NextJS app: npx create-next-app@latest dash --usepnpm/  pnpm i/ pnpm dev

# CSS Styling
## Global CSS: app/ui/global.css :box-sizing,m0,p0 stuff to apply everywhere or by tags
## Tailwind & CSS modules- Base Tailwind directives in global.css | X.module.css then import styles.xyz class
## conditionally add classNames with the `clsx` utility package:
```tsx
import clsx from 'clsx';
 
export default function InvoiceStatus({ status }: { status: string }) {
  return (
    <span
      className={clsx(
        'inline-flex items-center rounded-full px-2 py-1 text-sm',
        {
          'bg-gray-100 text-gray-500': status === 'pending',
          'bg-green-500 text-white': status === 'paid',
        },
      )}
    />
    // ...
)}
```
- other ways to style. Sass, CSS-in-JS and so on but they are becoming obselete so don't bother...
- clsx with twMerge : cn for shadcn/ui (ahaaa...)

# Optimizing fonts & Images (Cumulative Layout Shift)
## Custom fonts 
- (next/font): build time loaded, Server side stored with assets: so no dynamic get (Very fast)
## Add Image 
- (next/image): import from next/image IMAGE and fill all props: restricts enoneousity & dynamic swap
## more Optimize fonts and images in Next
Image Optimization Docs, Font Optimization Docs, Improving Web Performance with Images (MDN), Web Fonts (MDN), How Core Web Vitals Affect SEO [READINGS]

# Creating Layouts and Pages
- Create /dashboard routes using `file system routing`
- folder & files for routings: new route segments
- create nested-layouts that can be shared between multiple dashboard pages
- Understand `Colocation`,`Partial rendering` and `Root Layout`

## Nested Routing:(page.tsx, layout.tsx) in folder-hierarchy to make a UI
- page.tsx in /app/X => /X route 
  - colocation is when routes and components can be in the same folder but not affect each other
- layout.tsx (for partial rendering cappabilities):
  - only sections of the page are treated a block for re-rendering
- the RootLayout (Entry point to web-app: back for all-place components mostly for metadata)

# Navigation Between Pages
- next/link (Client-Side Navigation for withing page Cross-Component navigation) prod: on entry to viewport code prefetch, click near instant load
- show active link (usePathname()) : with clsx we can highlight current active dir
- navigation <a>

# PostgreSQL setup
-push to github
-setup a vercel account and link git repo for preview & deployment
-create and like project to postgres db
-send db init data: seed






































































































































































































































#### Other React Concepts
# You will learn
- How to create and nest components: functions & export default main component from file
- How to add markup and styles: className
- How to display data: JSX {soandso.soandso}
- How to render conditions and lists: ?-:- / && / if-else, also map with key-attrib
- How to respond to events and update the screen: onClick
    - useState: make component/page have state - remember things
    - funcs with use- Hooks (API reference Check)
- How to share data between components: lifting-state-up (parent state to children pass)
    - props: count={something}, ({count})/(props.count)

# Typescript (TypeScript's type system):
## Type System: Types by Inference & Defined Types - Objects and Interfaces
```typescript
// Object without Type Declarations but by Inference
const user = { name: "Hayes", id: 0,};

// an Interface that Defines the structure of Child objects
interface User { name: string; id: number; }
// Now the Type can be directly Understood, by Definition
const user: User = { name: "Hayes", id: 0, };

//// You can Even Create your Own Object Creation Method!
interface User { name: string; id: number; }
class UserAccount { name: string; id: number;
  constructor(name: string, id: number) {
    this.name = name;  this.id = id;  }
}
const user: User = new UserAccount("Murphy", 1);
```


# Usage & Datatypes
- We use interfaces to annotate parameters and return values in functions :for Validation
```typescript
function deleteUser(user: User) {  /*... */ } // recieves Object of Type User
function getAdminUser(): User { /*... */ } // returns Object of Type User
```
- There is already a small set of primitive types available in JavaScript: `boolean`, `bigint`, `null`, `number`, `string`, `symbol`, and `undefined`, which you can use in an interface. 
- TypeScript extends this list with a few more, such as 
  - `any` (allow anything), 
  - `unknown` (ensure someone using this type declares what the type is), 
  - `never` (it’s not possible that this type could happen), and 
  - `void` (a function which returns undefined or has no return value).
- We use `Interfaces` & `Types` for building data types from these primitives
  - You should prefer Interfaces over Types unless you need specific features
  
# Composing Types: Unions & Generics & `typeof` keyword
- Union (One of many values): `type MyBool = true | false;` :it would generally be boolean (the structural type system)
  - Mostly used when limited values are allowed: `type LockStates = "locked" | "unlocked"; type PositiveOddNumbersUnderTen = 1 | 3 | 5 | 7 | 9;`
  - In function Usave: `function getLength(obj: string | string[]) {return obj.length;}`
- Generics `<>`: provide type to elements of function or array: 
  - `type NumberArray = Array<number>;type ObjectWithNameArray = Array<{ name: string }>;`
  - `interface Backpack<Type> { add: (obj: Type) => void; get: () => Type;}` 
    - Describes an object with functions that must addhere to passed types: add must recieve, and get returns same type of the specific instance
    - `declare const backpack: Backpack<string>;const object = backpack.get(); backpack.add(23);` object is string & add(23) will cause an error
- typeof: identifies type of vars: `typeof X === "string"` or `Array.isArray(X)` it is unique to check if an Item is an array

# The Structural Type System: duck/structural typing
- a subset of the object’s fields to match for Type to be the Same and thus code work
```typescript
// Basic View of Passing
interface Point { x: number; y: number; }
function logPoint(p: Point) { console.log(`${p.x}, ${p.y}`); }
const point = { x: 12, y: 26 };
logPoint(point); // // logs "12, 26" :Just like Inference Typing

// Subset Only Law
const rect = { x: 33, y: 3, z: 30, height: 80 };
logPoint(rect); // logs "33, 3" :B/c it will find all nessesary properties
 const color = { hex: "#187ABF" };
logPoint(color); // this will not work--

// Advanced but Property Finding still satisfied
class VirtualPoint { x: number; y: number;
  constructor(x: number, y: number) {
    this.x = x;  this.y = y;  }
}
const newVPoint = new VirtualPoint(13, 56);
logPoint(newVPoint); // logs "13, 56" It will still find nesseary properties so it is still of type point
```




NextJs

# React Foundations
## About React & Next.Js
- Building Blocks of a web application: (full correct decomposition)
  - User Interface: `React` Is Here, Other things Agnostic 
  - Routing
  - Data Fetching
  - Rendering
  - Integrations
  - Infrastructure
  - Performance
  - Scalability
  - Developer Experience
- We had to use your own code or attach libraries like: most commonly `React-Router-Dom` to help with routing & `Axios` to help with data fetching...

NextJs - is a react framework that gives you building blocks to create web applications
![The Place of NextJS](image.png)
- NextJs adds: Rendering, Routing, DataFetching, ... for efficient connection with the Backend DB

# Rendering User Interfaces(UI)
- The DOM can be manupilated by base DOM methods(body.getElementby,doc.createElement,preventDefault,...) & JavaScript

# Updating UI with JavaScript
  - With these you can do anything, but its Verbose (repetive), We want Just tell it what you want, and CPU figure how to do it under the hood
## Imperative Vs. Declarative programming (TINKER WITH HOW THE DOM WORKS TO DO WHAT YOU WANT |||| SAY WHAT WE WANT TO SHOW)
- tell a step by step instruction to make pizza, or order pizza (without the under the hood): I mean you just want the pizza right?
- React make web-dev Declarative
## React figured out how to make computers a chef that undersand orders and people just had to order Views instead of giving computer step-by-step guide


# Getting Started with React
- `react-package: ReactBase & DOMTools` => document.getElementById(WHERETODELIVER) => `RouterDOM`.createRoot(WHERETODELIVER).render(THEORDER)
## JSX: Three rules: returnSingleElement, CloseAllTags, CamelCased-HTML-attribs: and this is the language in which you write the Order
- JavaScript compilers like Babel convert JSX code to regular JavaScript & thus `babel` for converision
- Once Set You just give orders rather than explain how the web-app should look logically

# Building UI with Components
## React Core Concepts: `Components`, `Props`, `State`
- ReactComponents: Building Blocks of UI (CapitalizedJSFunctions) & These are Nestable
# Diplaying Data With Props: In React Data flows down the component tree: One Way
- Prop Passing, Prop destructuring or props.X  , JSX {} to display in html, & Key for Array.map elements
# Adding Interactiviy with State: OnClick, handleClick, useState(count,setCount)

# From React to NextJS :Client:=>Server Side
## Creating your first page (folders & files manage routing):app folder app/layout.js for RootLayout with main-nav,footer & so on
## Server & Client Components: Environments(Server:->app-containing-pc-in-data-center & Client:->browser) & Network boundary(The Separation)
- Components separated by Net Boundary first Server Comps are rendered on server and Sent as RSC(React Server Component Payload)
  - RSC contains
    - Rendered Result of Server Components
    - Placeholders (holes) to Where Client Components should be rendered and js files for them as a reference
  - React Uses RSC to consolidate Server-Client and Update the DOM

# Using Client Components:
- create component 'use-client'(here you can useState & useEffect): then Import and use in an Server component

# useCallback & memo: cache func def between rerenderes:
- if your function depends on changing variables: 
```jsx
export default function ProductPage({ productId, referrer, theme }) {
  const handleSubmit = useCallback((orderDetails) => {
    post('/product/' + productId + '/buy', {
      referrer,
      orderDetails,
    });
  }, [productId, referrer]);

``` 
Because it depends on the productId & Referrer which might change
- here we are sure to send the right request all the time (optimizes editing of function only if dependancies change, and not on every re-render)
- UseCallback for Components is memo & together usage
```jsx
// In JavaScript, a function () {} or () => {} always creates a different function, similar to how the {} object literal always creates a new object. 

import { memo } from 'react';
const ShippingForm = memo(function ShippingForm({ onSubmit }) {
  // Component ShippingForm only re-renders if Props change
});


function ProductPage({ productId, referrer, theme }) {
  // Every time the theme changes, this will be a different function...
  function handleSubmit(orderDetails) {
    post('/product/' + productId + '/buy', {
      referrer,
      orderDetails,
    });
  }
    // Tell React to cache your function between re-renders...
  const handleSubmit = useCallback((orderDetails) => {
    post('/product/' + productId + '/buy', {
      referrer,
      orderDetails,
    });
  }, [productId, referrer]); // ...so as long as these dependencies don't change...
 
 {/* IF second...ShippingForm will receive the same props and can skip re-rendering */}
  return (
    <div className={theme}>
      {/* ... so ShippingForm's props will never be the same, and it will re-render every time */}
      <ShippingForm onSubmit={handleSubmit} />
    </div>
  );
}
```



# useContext:=> ContextProvider:createContext, component-call
- all contextUsingChildrenComponents re-render if context-changes
- to make Changes provide useState Controlled context-data
  - Nested Providers for a smaller set of children
```jsx
<ThemeContext.Provider value="dark">
  ...
  <ThemeContext.Provider value="light">
    <Footer />
  </ThemeContext.Provider>
  ...
</ThemeContext.Provider>
```

# useContext & useCallback-memo POWER
```jsx
function MyApp() {
  const [currentUser, setCurrentUser] = useState(null);

  function login(response) {
    storeCredentials(response.credentials);
    setCurrentUser(response.user);
  }

  return (
    <AuthContext.Provider value={{ currentUser, login }}>
      <Page />
    </AuthContext.Provider>
  );
}
/* EVERYTIME AYTHING CHANGES, login & currentUser,setCurrentUser
are newly created so all elements inside context will re-render
*/

import { useCallback, useMemo } from 'react';

function MyApp() {
  const [currentUser, setCurrentUser] = useState(null);

  const login = useCallback((response) => {
    storeCredentials(response.credentials);
    setCurrentUser(response.user);
  }, []);

  const contextValue = useMemo(() => ({
    currentUser,
    login
  }), [currentUser, login]);

  return (
    <AuthContext.Provider value={contextValue}> // value name
      <Page />
    </AuthContext.Provider>
  );
}
/* Here if response have not changed, then by useCallback we know login function will stay the same, which will not change currentUser
and by useMemo we maintain a re-render agnostic contextValue that is sustained through re-renders as long as login & currentUser stay the same */
```


# useDeferredValue: 
## Showing stale content while fresh content is loading
```jsx
export default function App() {
  const [query, setQuery] = useState('');
  const deferredQuery = useDeferredValue(query);
  return (
    <>
      <label>
        Search albums:
        <input value={query} onChange={e => setQuery(e.target.value)} />
      </label>
      <Suspense fallback={<h2>Loading...</h2>}>
        <SearchResults query={deferredQuery} />
      </Suspense>
    </>
  );
}
// Don't directly load queue for next re-render for searchResult to update, this prevents suspense loading over and over
```

## Indicating content is stale & selective re-render 
```jsx
<div style="{{ opacity: query !== deferredQuery ? 0.5 : 1, }}" >
  <SearchResults query={deferredQuery} />
</div>
// styling by staleness

import { useState, useDeferredValue } from 'react';
import SlowList from './SlowList.js';

export default function App() {
  const [text, setText] = useState('');
  const deferredText = useDeferredValue(text);
  return (
    <>
      <input value={text} onChange="{e => setText(e.target.value)}" />
      <SlowList text={deferredText} />
    </>
  );
}
// this will not block keystrocks
```





# useEffect: useEffect(setup, dependencies?): sync with external sys
```jsx
import { useEffect } from 'react';
import { createConnection } from './chat.js';

function ChatRoom({ roomId }) {
  const [serverUrl, setServerUrl] = useState('https://localhost:1234');

  useEffect(() => { 
    // SETUP CODE (BODY OF USE-EFFECT)
    const connection = createConnection(serverUrl, roomId);
    connection.connect();
    connection.disconnect(); 
    // CLEAN-UP CODE (RETURN OF USE-EFFECT)
    return () => {
    };
  }, [serverUrl, roomId]);
  // ...
}
/* PERFECT CHATROOM EXAMPLE
- On every rerender: cleanup code runs with past-props and state
- and setup code runs with new props and state before load
- on intial load only setup code runs as there is no prev state
- and on component removal: cleanup code runs one more time
*/
```
## rather than writting effects everytime repetitively: create CustomHooks for similar patterns declaratively
```jsx
function useChatRoom({ serverUrl, roomId }) {
  useEffect(() => {
    const options = {
      serverUrl: serverUrl,
      roomId: roomId
    };
    const connection = createConnection(options);
    connection.connect();
    return () => connection.disconnect();
  }, [roomId, serverUrl]);
}

function ChatRoom({ roomId }) {
  const [serverUrl, setServerUrl] = useState('https://localhost:1234');

  useChatRoom({
    roomId: roomId,
    serverUrl: serverUrl
  });
  // ... no need for specifying detailed useEffect repeatedly
}
```

# useId, for those js-### names of tag-id's why I cant scrape em
```jsx
export default function Form() {
  const id = useId();
  return (
    <form>
      <label htmlFor={id + '-firstName'}>First Name:</label>
      <input id={id + '-firstName'} type="text" />
      <hr />
      <label htmlFor={id + '-lastName'}>Last Name:</label>
      <input id={id + '-lastName'} type="text" />
    </form>
  );
}
/* labels will tag elements correctly b/c same Id per render
but not constant enough for scraping path detection
*/
```

# forwardRef & useImperativeHandle: expose only set of methods:
- its like java Encapsulation, just allow the user to do a limited set of things on data
```jsx
import { forwardRef, useRef, useImperativeHandle } from 'react';

const MyInput = forwardRef(function MyInput(props, ref) {
  const inputRef = useRef(null);

  useImperativeHandle(ref, () => {
    return {
      focus() {
        inputRef.current.focus();
      },
      scrollIntoView() {
        inputRef.current.scrollIntoView();
      },
    };
  }, []);

  return <input {...props} ref={inputRef} />;
});

// this only allows focus & scrollIntoView for input Elements
```

# useInsertionEffect for CSS-in-JS library

# useLayoutEffect: Measuring layout before the browser repaints the screen (place modals rightly on click position and size of modal)
: all this re-renders happen before screen is painted with layout
```jsx
function Tooltip() {
  const ref = useRef(null);
  const [tooltipHeight, setTooltipHeight] = useState(0); // You don't know real height yet

  useLayoutEffect(() => {
    const { height } = ref.current.getBoundingClientRect();
    setTooltipHeight(height); // Re-render now that you know the real height
  }, []);

  // ...use tooltipHeight in the rendering logic below...
}
```

# useMemo:runs a argumentless function if nested internal props change
```jsx
export default function TodoList({ todos, tab, theme }) {
  // Tell React to cache your calculation between re-renders...
  const visibleTodos = useMemo(
    () => filterTodos(todos, tab),
    [todos, tab] // ...so as long as these dependencies don't change...
  );
  return (
    <div className={theme}>
      {/* ...List will receive the same props and can skip re-rendering */}
      <List items={visibleTodos} />
    </div>
  );
}

/* Difference with useCallback (function outcome check)
But since allmost always if props same function outcome same its same
Just have to wrap in another layer of argumentless function to use useMemo instead of useCallback, maybe clutteredness
ALL ISSUES WITH memo(components) and objects are required to keep performance benefits by maintaing sameness of object values feed to these functions
```

# useOptimistic :useState but withOptimisticUpdateFn
- used with useState that overrides its value, useOptimistic provides an optimistic value to render until useStates updates with real value\
  

# useReducer :managing state with a reducer (multi-update function)
[state, setState] => [state, dispatch]
useState(initstate) => useReducer(reducerFunc, initstate)
reducerFunc(state,action) {
    if action.type === 'X': do x
    else if action.type === 'Y': do y
    // or
    switch (action.type) {
        case 'X': {return ...}
        case 'Y': {return ...}
    }
    throw Error('Unknown action: ' action.type)
}
dispatch({type:"X"}) :depending on a single state, and type of action
dispatch({type: "Y"}) :we can now do multiple different things 

# useRef : mutable value whose change don;t cause re-rendering, but change should strictly be in EventHandlerFunctions or useEffect
# useState: Immutable ref and its value cause a re-render

# useSyncExternalStore: snapshot, subscribe, getSnapshot:
- pinia base implementation: have listeners and return data
- which will be snapshot triggering a re-render

# useTransition: non-UI blocking StateUpdate (python: thread)
- mark Heavy functions by wrapping them in startTransition
- detect if finished by boolean isPending false to render
- whilst you can continue using UI, 
  - Heavy tabs to load, etc..
```jsx
function TabContainer() {
  const [isPending, startTransition] = useTransition();
  const [tab, setTab] = useState('about');

  function selectTab(nextTab) {
    startTransition(() => {
      setTab(nextTab);
    });
  }
  // ...
}
```

# Tips
## React Components
### Fragment (<> | <Fragment />)
### Profiler (<Profiler id='root',onRender={onRender} />)
- Takes the elements and calls your onRender function with data about performance for the given element: id, phase, actualDuration, baseDuration, startTime, commitTime
### StrictMode: to fix common bugs in dev: extra render & effect run, also usage of Deprecated api check (comon weird renders caught)
### Suspense: fallback until children finish loading
```jsx
<Suspense fallback={<Loading />}>
  <SomeComponent />
</Suspense>
// or even 
<Suspense fallback={<Loading />}>
  <Biography />
  <Panel>
    <Albums />
  </Panel>
</Suspense>
// the together at Once or not at all logic
<Suspense fallback={<BigSpinner />}>
    <Biography artistId={artist.id} />
    <Suspense fallback={<AlbumsGlimmer />}>
        <Panel>
            <Albums artistId={artist.id} />
        </Panel>
    </Suspense>
</Suspense>
// nested suspense, when Bio loads another suspense until album
```
- with useDeferredValue you can check stale & show by style too
- with useTransition you can make existing UI not hide while Suspense



