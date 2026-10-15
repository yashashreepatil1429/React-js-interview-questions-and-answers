# React.js Interview Questions and Answers

A concise guide to React fundamentals, architecture, components, JSX, props,
state, events, conditional rendering, lists, keys, and component lifecycles.

## React fundamentals

### 1. What is React.js?

React is an open-source JavaScript library for building user interfaces. It
encourages developers to compose interfaces from reusable components and
describe what the UI should look like for a given state. React updates the
rendered interface when that state changes.

### 2. What are the major React.js versions?

React's major releases include React 15, 16, 17, 18, and 19. React 16
introduced the Fiber architecture, error boundaries, and portals; React 17
focused largely on making gradual upgrades easier; React 18 introduced
concurrent rendering capabilities and APIs such as `createRoot`; and React 19
added features including Actions, form actions, and improved support for
server-rendered applications. For a project, check its installed version in
`package.json` or with `npm list react`.

### 3. Who invented and implemented React?

React was created at Facebook (now Meta), with Jordan Walke widely credited as
its original author. It was developed to address the challenges of building
interactive user interfaces at scale. Facebook open-sourced React in 2013.

### 4. What technologies compete with React?

Common alternatives for building web user interfaces include Angular, Vue,
Svelte, and Solid. They solve similar problems but differ in their APIs,
rendering approaches, ecosystems, and conventions. The best choice depends on
the application, team experience, and project requirements.

### 5. What is the difference between the DOM and the virtual DOM?

The DOM (Document Object Model) is the browser's object representation of a
document. React elements are lightweight descriptions of UI; the term *virtual
DOM* is commonly used for these in-memory descriptions and the rendered tree
React manages. When a render produces a new description, React determines what
needs to change and commits the necessary updates to the host environment,
which is usually the browser DOM. It is not accurate to say React always
updates only one node or that every virtual-DOM update is automatically faster
than direct DOM code.

### 6. What is React's diffing algorithm?

Diffing is the process React uses to compare the previous rendered element tree
with the next one and determine which parts need updating. React uses
heuristics rather than comparing every possible tree shape exhaustively.
Element type, position, and keys help it identify whether to preserve, update,
or replace parts of the rendered tree.

### 7. What is reconciliation in React?

Reconciliation is React's process of rendering a new element tree, comparing it
with the previous tree, and deciding how to update the rendered output.
Diffing is part of this process; reconciliation is the broader process. React
then commits the required changes to the DOM or another rendering target.

### 8. What is the difference between props and state?

Props are inputs passed to a component by its parent. A component should treat
its props as read-only. State is data a component owns and updates over time,
usually with `useState` or `useReducer` in a function component. Updating state
requests a re-render. Both props and state can affect what the component
renders.

### 9. What are initialization, mounting, updating, and unmounting?

These terms describe stages in a component's rendered lifetime:

- **Initialization:** setting up initial values, such as initial state.
- **Mounting:** the component is rendered for the first time and added to the
  UI.
- **Updating:** the component renders again because its props, state, or
  relevant context changed.
- **Unmounting:** the component is removed from the UI; subscriptions,
  timers, or other external resources should be cleaned up.

Function components do not have class lifecycle methods. Effects can
synchronize with external systems and return cleanup functions; class
components have lifecycle methods such as `componentDidMount`,
`componentDidUpdate`, and `componentWillUnmount`.

## Components, JSX, and data

### 10. What is a React component?

A component is a reusable piece of UI. A function component receives props and
returns React elements, often written with JSX. Components can be composed to
build larger interfaces.

```jsx
function Greeting({ name }) {
  return <h1>Hello, {name}!</h1>;
}
```

### 11. What is the difference between function and class components?

Function components are JavaScript functions that return UI and can use Hooks
for state and other React features. Class components extend `React.Component`
and use methods such as `render` and lifecycle methods. Function components
are the standard choice for new React code, although class components remain
supported.

### 12. What is JSX?

JSX is a syntax extension that lets developers write element-like markup in
JavaScript. Build tools transform it into calls that create React elements.
JSX expressions use `{}` to embed JavaScript values.

```jsx
const title = <h1>{'React' + ' fundamentals'}</h1>;
```

### 13. Do browsers understand JSX directly?

No. Browsers do not natively parse JSX. A compiler or build tool such as Babel,
Vite, or a framework's compiler transforms JSX into JavaScript the browser can
run.

### 14. What are props, and how are they passed?

Props are values supplied by a parent through JSX attributes. They let a
component be configured without hard-coding its content.

```jsx
function Welcome({ name }) {
  return <p>Welcome, {name}</p>;
}

<Welcome name="Ava" />;
```

Props are read-only from the receiving component's perspective.

### 15. What is state, and how do you update it?

State is component-owned data that can change over time. In a function
component, `useState` returns the current value and a setter. Call the setter
instead of mutating the value directly.

```jsx
const [count, setCount] = useState(0);
setCount(currentCount => currentCount + 1);
```

The functional updater is useful when the next value depends on the previous
value.

### 16. What is one-way data flow?

In React, data typically flows from parent components to child components
through props. A child can notify a parent about an event by calling a
callback passed as a prop. This makes the direction of data and updates easier
to understand.

### 17. What are controlled and uncontrolled form elements?

A controlled form element gets its value from React state and reports changes
through an event handler. An uncontrolled element stores its current value in
the DOM; a ref can be used to read it when needed. Controlled elements are
useful when the UI must respond to or validate each change.

## Events and rendering

### 18. How does event handling work in React?

Pass a function to a JSX event prop such as `onClick` or `onChange`. React
invokes it when the event occurs. Use camelCase event prop names and pass the
function itself rather than calling it during rendering.

```jsx
<button onClick={handleClick}>Save</button>
```

### 19. How do you pass an argument to an event handler?

Use an arrow function to call the handler with the desired argument when the
event occurs:

```jsx
<button onClick={() => deleteItem(item.id)}>Delete</button>
```

Passing `deleteItem(item.id)` directly would call it during rendering.

### 20. What is conditional rendering?

Conditional rendering means choosing which elements to render based on a
condition. JavaScript conditional expressions such as `if`, the ternary
operator, and `&&` can be used in component logic or JSX.

```jsx
return isLoggedIn ? <Dashboard /> : <Login />;
```

### 21. How do you render a list in React?

Use JavaScript array methods such as `map` to transform data into elements.
Give each sibling element a stable `key` so React can track it across renders.

```jsx
items.map(item => <li key={item.id}>{item.name}</li>);
```

### 22. Why are keys important in lists?

Keys give React a stable identity for each item among its siblings. They help
React match items when a list is reordered, inserted into, or deleted from, so
it can update the correct elements and preserve component state where
appropriate. Keys should be stable and unique among siblings.

### 23. Why should array indexes usually not be used as keys?

An index identifies an item's position, not the item itself. If items are
inserted, removed, or reordered, index keys can cause React to associate
existing component state or DOM with the wrong item. An index can be acceptable
for a static list that will never be reordered or changed.

### 24. Can a component return multiple elements?

Yes. A component can return one parent element, or use a Fragment to group
multiple elements without adding an extra DOM node.

```jsx
return (
  <>
    <h2>Profile</h2>
    <p>Details</p>
  </>
);
```

## Architecture and lifecycle

### 25. What is React Fiber?

Fiber is the internal reconciliation architecture introduced in React 16. It
represents rendering work in units that React can schedule and prioritize.
Fiber is an implementation detail; application code should use React's public
APIs rather than depend on its internals.

### 26. What happens after a state update?

A state setter schedules an update. React renders the affected component tree
to calculate the next UI, reconciles it with the previous output, and commits
the necessary changes. React may batch updates, so a setter does not mean the
DOM changes synchronously on that exact line.

### 27. How do Effects relate to mounting, updating, and unmounting?

`useEffect` runs after React commits a render when its dependencies require the
Effect to run. If it returns a cleanup function, React runs that cleanup
before the Effect runs again and when the component is removed. Effects are
intended for synchronizing with external systems, not for every calculation
that can be performed during rendering.

### 28. What is the purpose of the `useEffect` dependency array?

The dependency array tells React which reactive values used by the Effect
should cause it to re-run:

- No array: run after every committed render.
- `[]`: no changing dependencies; run on mount, with cleanup on unmount (in
  development Strict Mode, React may perform an extra setup-and-cleanup cycle).
- `[value]`: re-run when `value` changes.

Include the reactive values used by the Effect to avoid stale values and keep
the dependency list aligned with the code.

### 29. What are Hooks, and what are the Rules of Hooks?

Hooks are functions such as `useState`, `useEffect`, and `useContext` that let
function components use React features. Call Hooks only at the top level of a
function component or a custom Hook—not inside loops, conditions, or nested
functions. This keeps Hook calls in the same order on every render.

### 30. What is the difference between rendering and committing?

During **render**, React calls components to determine what the next UI should
be. During **commit**, React applies the required changes to the DOM or other
host environment and runs relevant commit-phase work. A render does not
necessarily mean every DOM node changes.