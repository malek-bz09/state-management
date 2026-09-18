# Client-Side State Management in React

> Workshop Goal: Understand how React manages UI state and learn when to use `useState`, `useReducer`, `useContext`, and Zustand.
> 

---

# 0. The Big Picture

Before talking about state management, we need to understand why React exists.

```
Vanilla JS
   ↓
Imperative UI

React
   ↓
Declarative UI
   ↓
Components
   ↓
Props
   ↓
State
   ↓
State Management
   ├── useState
   ├── useReducer
   ├── useContext
   └── Zustand
```

The goal today is not to memorize APIs.

The goal is to understand:

> What problem do I have?
> 
> 
> What tool solves that problem?
> 

---

# 1. Vanilla JS vs React

## Vanilla JavaScript

In vanilla JS we manually update the DOM.

```jsx
const button = document.querySelector("#btn");
const counter = document.querySelector("#counter");

let count = 0;

button.addEventListener("click", () => {
  count++;
  counter.textContent = count;
});
```

Flow:

```
State changes
     ↓
Manually update DOM
     ↓
UI changes
```

### Question

What happens when 20 different parts of the UI depend on the same data?

Managing all those DOM updates becomes difficult.

---

# 2. Imperative vs Declarative

## Imperative

Tell the browser HOW.

```jsx
element.textContent = count;
```

## Declarative

Tell React WHAT the UI should look like.

```jsx
<h1>{count}</h1>
```

Flow:

```
State
  ↓
Desired UI
  ↓
React updates DOM
```

Important:

> React still manipulates the DOM.
> 
> 
> We just don't manually describe those DOM operations anymore.
> 

### Ask

Who is responsible for figuring out the DOM updates?

→ React.

---

# 3. Why React?

As applications grow:

```
User logs in
    ↓
Navbar updates
    ↓
Profile updates
    ↓
Sidebar updates
    ↓
Notifications update
```

React gives us:

- Components
- Props
- State
- Re-rendering
- Reconciliation

Core idea:

> UI is a representation of state.
> 

When state changes, React updates the UI.

---

# 4. Components

A component is a reusable piece of UI.

```jsx
function Button() {
  return <button>Click me</button>;
}
```

Applications are component trees:

```
App
├── Navbar
├── Sidebar
├── Main
└── Footer
```

Components help us:

- Reuse UI
- Organize code
- Isolate logic

### Ask

Is a React component just a function?

→ Usually yes.

---

# 5. Props

Props are data passed from parent to child.

```jsx
function A() {
  return 
  <Profile name="Malek" />
<Profile name="Sara" />
<Profile name="Ahmed" />;
}

function B({ name }) {
  return <C name={name} />;
}

function C({ name }) {
  return <h2>{name}</h2>;
}

function name () {
return <h1> MAlek <h1>
```

Flow:

```
Parent
  ↓
Props
  ↓
Child
```

Properties of props:

- Parent → Child
- Read only
- Configure components

### Ask

Can a child modify its props?

→ No.

---

What is the difference between Props and State?

---

# 7. UseState

---

### Why normal variables don't work

```
function Counter() {
  let count = 0;

  function increment() {
    count++;
  }

  return <button onClick={increment}>{count}</button>;
}
```

When `increment()` runs, `count` does change — but React was never told to
render again, so the UI doesn't update. And even if it did rerender, the
function runs from the top again:

```
let count = 0; // recreated every render — the old value is lost
```

Normal local variables don't persist between renders. **State does.**

### What is state?

State is data React stores *between* renders, which — when updated — tells
React to schedule a rerender.

```
State
 ↓ stored between renders
update state
 ↓
React schedules a rerender
 ↓
component runs again
 ↓
new UI
```

```
Props
 ↓
data Passed to component

State
 ↓
data Owned by component
```

### useState()

```
const [count, setCount] = useState(0);
```

- `count` — the current value
- `setCount` — the function that updates it and triggers a rerender
- `0` — the initial value

```
function Counter() {
  const [count, setCount] = useState(0);
  return <button onClick={() => setCount(count + 1)}>{count}</button>;
}
```

### setState doesn't change the *current* render

```
function increment() {
  setCount(1);
  console.log(count); // still logs the OLD value, e.g. 0
}
```

`setCount(1)` schedules the update — it doesn't retroactively change `count`
inside the function that's already running. The current execution finishes with
the old value; the *next* render sees the new one.

```
Current render: count = 0
     ↓
setCount(1)             ← scheduled, not immediate
     ↓
rest of current function still sees count = 0
     ↓
Next render: count = 1
```

---

# 10. Functional Updates

Bad:

```jsx
setCount(count + 1);
setCount(count + 1);
```

Both use the same snapshot.

Better:

```jsx
setCount(prev => prev + 1);
setCount(prev => prev + 1);
```

Flow:

```
0
↓
1
↓
2
```

Rule:

> If the next value depends on the previous value, use:
> 

```jsx
setState(prev => ...
```

---

# 11. Batching

React groups multiple state updates into a single render.

```jsx
setCount(prev => prev + 1);
setCount(prev => prev + 1);
setCount(prev => prev + 1);
```

Result:

```
0 → 1 → 2 → 3
```

React performs one render at the end.

---

# 12. State Ownership.

A common question is:

> "Which component should own the state?"
> 

Usually:

**Put state in the closest common parent that needs to control it.**

Example:

```
        App
       /   \
 ProfileList  SearchBar
```

If both `ProfileList` and `SearchBar` need `profiles`, the state can live in `App`.

```
function App() {
  const [profiles, setProfiles] = useState([]);

  return (
    <>
      <SearchBar profiles={profiles} />
      <ProfileList profiles={profiles} />
    </>
  );
}
```

This leads to an important React pattern:

# Lifting State Up

Move state from a child to a parent so multiple components can share it.

```
Before:

ProfileList
 └── profiles

After:

        App
       /   \
ProfileList SearchBar
       ↑       ↑
       shared state
```

---

# 13. Derived State

Bad:

```jsx
const [firstName, setFirstName] = useState("");
const [lastName, setLastName] = useState("");
const [fullName, setFullName] = useState("");
```

Good:

```jsx
const fullName = `${firstName} ${lastName}`;
```

Rule:

> If a value can be calculated, don't store it.
> 

### Ask

Should every value be stored in state?

→ No.

---

# 14. Immutability

Bad:

```jsx
profiles.push(profile);
user.name = "Ahmed";
```

Good:

```jsx
[...profiles, profile]

{ ...user, name: "Ahmed" }
```

Rule:

> Never mutate state directly.
> 

Always create a new object or array.

---

# 15. When useState Gets Complicated

Imagine our Profile Manager has:

```
profiles
selectedProfile
isEditing
```

And we need:

```
ADD_PROFILE
EDIT_PROFILE
DELETE_PROFILE
SELECT_PROFILE
CLOSE_EDITOR
```

With many `setState` calls, the logic can become difficult to follow.

This is where `useReducer` becomes useful.

---

# 16. useReducer Mental Model

```
Current State
      +
    Action
      ↓
   Reducer
      ↓
   New State
```

Reducer:

```jsx
function reducer(state, action) {
  switch (action.type) {
    case "increment":
      return {
        ...state,
        count: state.count + 1
      };

    default:
      return state;
  }
}
```

---

# 17. Actions & Dispatch

Action:

```jsx
{
  type: "increment"
}
```

Dispatch:

```jsx
dispatch({
  type: "increment"
});
```

Flow:

```
User click
     ↓
dispatch(action)
     ↓
Reducer
     ↓
New state
     ↓
Render
```

Key idea:

> Action describes WHAT happened.
> 
> 
> Reducer decides HOW state changes.
> 

### Ask

What is the difference between an action and state?

---

# 18. useState vs useReducer

### useState

Good for:

```
count
theme
isOpen
selectedId
```

### useReducer

Good for:

```
Many related values
+
Many transitions
+
Complex logic
```

Rule:

> useReducer is not better.
> 
> 
> It's useful when state transitions become complex.
> 

---

# 19. Prop Drilling

Imagine:

```
App
 ↓ user
Layout
 ↓ user
Sidebar
 ↓ user
Profile
 ↓ user
Avatar
```

Only Avatar needs the user.

The rest are passing data through.

This is:

> Prop Drilling
> 

### Ask

What's wrong with prop drilling?

Nothing.

It becomes annoying when the tree becomes deep.

---

# 20. useContext

Context solves prop drilling.

Create context:

```jsx
const UserContext = createContext();
```

Provide value:

```jsx
<UserContext.Provider value={user}>
  <home />
</UserContext.Provider>
```

Consume value:

```jsx
const user = useContext(UserContext);
```

Flow:

```
Provider
    ↓
Context value
    ↓
Any descendant component
```

Important:

> Context is not state.
> 

Context shares state.

It does not create state.

---

# 21. useReducer + Context

Often we combine them.

```
useReducer
    ↓
Manages state

Context
    ↓
Shares state
```

```jsx
<ProfileContext.Provider
  value={{ state, dispatch }}
>
  {children}
</ProfileContext.Provider>
```

Any descendant can dispatch actions.

---

# 22. Zustand

As applications grow:

```
Navbar
Profile
Notifications
Cart
Dashboard
```

Many unrelated components need shared state.

Context can do it.

But eventually:

- More Providers
- More boilerplate
- More nesting

This is where Zustand helps.

---

# 23. Zustand Mental Model

```
           Store
         /   |   \
        /    |    \
     Nav  Cart  Profile
```

Create a store:

```jsx
const useCounterStore = create((set) => ({
  count: 0,

  increment: () =>
    set(state => ({
      count: state.count + 1
    }))
}));
```

Use it:

```jsx
const count = useCounterStore(
  state => state.count
);
```

Important:

> Zustand is for genuinely shared application state.
> 

Not every state belongs in Zustand.

---

# 24. Decision Tree

```
Need state?
    ↓
Simple local state?
    ↓
useState

Complex transitions?
    ↓
useReducer

Prop drilling problem?
    ↓
useContext

Shared state across unrelated parts?
    ↓
Zustand
```

---

# Final Mental Model

```
State
 │
 ├── Simple Local State
 │       ↓
 │    useState
 │
 ├── Complex Local State
 │       ↓
 │    useReducer
 │
 └── Shared State
         │
         ├── Through a subtree
         │       ↓
         │    Context
         │
         └── Across the app
                 ↓
              Zustand
```

# Questions For The Audience

### React

- Why use React instead of manually manipulating the DOM?
- What does declarative mean?
- What is a component?
- What is the difference between props and state?

### useState

- Why doesn't changing a normal variable update the UI?
- Does `setState` immediately change state?
- What is a render snapshot?
- When should we use `prev => ...`?

### useReducer

- What problem does useReducer solve?
- What is an action?
- What is dispatch?
- Why not just use useState everywhere?

### Context

- What is prop drilling?
- Does Context create state?
- Why would we use Context?

### Zustand

- Why not use Context everywhere?
- What is a store?
- Should all state go into Zustand?

---

# Final Principle

```
What problem do I have?
         ↓
What is the simplest solution?
         ↓
Choose the tool
```

> State management is not about using the most powerful tool.
> It's about managing state at the right level of complexity.
> 
> 
>
