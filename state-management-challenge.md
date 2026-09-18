# 🎯 Challenge : **Profile Manager & State Management**

> **Build a Profile Manager application to master React state management using useState, useReducer, useContext, and Zustand.**

---

## 🎯 Challenge Objectives

By completing this challenge, you'll build:

* ⚛️ React application using modern React hooks
* 👤 Profile management system
* ➕ Add profile functionality
* ✏️ Edit profile functionality
* 🗑️ Delete profile functionality
* 🔄 State management with useState
* 🧠 Complex state transitions with useReducer
* 🌍 Shared state with Context API
* 🏪 Global state with Zustand
* 🏗️ Proper React component architecture

---

## 🛠️ Prerequisites

Before starting, you should understand:

* Components
* Props
* State
* Event handling
* Array methods (map, filter)
* Basic React hooks

---

## 📋 Challenge Overview

You are building a small social platform dashboard.

Users can:

* View profiles
* Add new profiles
* Edit existing profiles
* Delete profiles
* Filter profiles
* Share profile data across multiple components

---

## 📁 Expected Project Structure

```text
src/
├── components/
│   ├── profiles/
│   │   ├── ProfileCard.jsx
│   │   ├── ProfileList.jsx
│   │   ├── AddProfileForm.jsx
│   │   └── EditProfileModal.jsx
│   │
│   └── layout/
│       └── Header.jsx
│
├── context/
│   └── ProfileContext.jsx
│
├── store/
│   └── profileStore.js
│
├── reducers/
│   └── profileReducer.js
│
├── data/
│   └── sampleProfiles.js
│
├── App.jsx
└── main.jsx
```

---

# 📝 Profile Data Structure

Use the following structure:

```js
{
  id: 1,
  name: "Abdelmalek",
  age: 19,
  role: "Frontend Developer",
  bio: "Computer Science Student",
  avatar: "https://...",
}
```

Create at least 5 profiles.

---

# ⚛️ Task 1: Profile Display

## Create ProfileCard

Display:

* Name
* Age
* Role
* Bio
* Avatar

### Requirements

* Card layout
* Responsive design
* Hover effect
* Professional UI

---

## Create ProfileList

Display all profiles.

Requirements:

* Use `.map()`
* Proper React keys
* Responsive grid

---

# 🔄 Task 2: State Management with useState

Store profiles using:

```jsx
const [profiles, setProfiles] = useState(...)
```

---

## Add Profile

Create a form that allows users to:

* Enter name
* Enter age
* Enter role
* Enter bio

When submitted:

* New profile appears instantly

---

## Delete Profile

Each card should contain:

```text
Delete Button
```

Clicking it removes the profile.

---

## Edit Profile

Each card should contain:

```text
Edit Button
```

Users can modify:

* Name
* Age
* Role
* Bio

Changes should update the UI.

---

## Concepts Practiced

* useState
* Immutability
* Array updates
* Controlled inputs
* Functional updates

---

# 🧠 Task 3: Refactor to useReducer

Replace multiple useState calls with:

```jsx
useReducer()
```

Create:

```js
const initialState = {
  profiles: [],
  selectedProfile: null,
  isEditing: false
};
```

---

## Actions

Implement:

```js
ADD_PROFILE
EDIT_PROFILE
DELETE_PROFILE
SELECT_PROFILE
CLOSE_MODAL
```

---

## Reducer Challenge

Create:

```js
function profileReducer(state, action) {
}
```

Handle all actions.

---

## Concepts Practiced

* Reducers
* Dispatching actions
* Complex state transitions
* Predictable state updates

---

# 🌍 Task 4: Context API

Imagine this structure:

```text
App
│
├── ProfileList
│
└── ProfileCard
     │
     └── EditButton
```

Profile data is needed deeply in the tree.

Without Context:

```text
App
 ↓
ProfileList
 ↓
ProfileCard
 ↓
EditButton
```

Props must be passed through every component.

This is called:

```text
Props Drilling
```

---

## Create Profile Context

Create:

```jsx
const ProfileContext = createContext();
```

---

## Provider

Wrap your application:

```jsx
<ProfileProvider>
  <App />
</ProfileProvider>
```

---

## Consume Context

Use:

```jsx
useContext(ProfileContext)
```

to access:

* profiles
* dispatch
* selectedProfile

from any component.

---

## Concepts Practiced

* createContext
* Provider
* useContext
* Props drilling
* Shared state

---

# 🏪 Task 5: Zustand

Create a Zustand store.

---

## Store State

Store:

```js
profiles
selectedProfile
```

---

## Actions

Create:

```js
addProfile()
editProfile()
deleteProfile()
selectProfile()
```

---

## Access Store

Use:

```jsx
const profiles = useProfileStore(
  state => state.profiles
);
```

and

```jsx
const addProfile = useProfileStore(
  state => state.addProfile
);
```

---

## Concepts Practiced

* Global state
* Zustand store
* Actions
* Selectors

---

# 🎯 Required Features

Your application must:

* Display profiles
* Add profiles
* Edit profiles
* Delete profiles
* Use immutable updates
* Use functional updates when needed
* Use useReducer actions
* Use Context API
* Use Zustand
* Have responsive UI

---

# 🧪 Testing Checklist

## useState Version

* Add profile works
* Edit profile works
* Delete profile works

---

## useReducer Version

* Actions update state correctly
* Reducer remains pure

---

## Context Version

* No prop drilling
* Components access shared state

---

## Zustand Version

* Components access global state
* Actions update store correctly

---

# 🚀 Bonus Challenges

If you finish early:

### Level 1

* Search profiles
* Sort profiles by age
* Sort profiles alphabetically

### Level 2

* Favorite profiles
* Profile statistics

Example:

```text
Total Profiles: 5
Average Age: 22
Developers: 3
Designers: 2
```

### Level 3

Implement:

* Dark Mode
* Theme Context
* Theme Zustand Store

---

# 🎓 Learning Goals

After completing this challenge, you should be able to answer:

* What problem does useState solve?
* When should I use useReducer?
* What is props drilling?
* Why does Context exist?
* What problem does Zustand solve?
* When should I choose useState vs useReducer vs Context vs Zustand?

---

## ✅ Success Criteria

A successful submission demonstrates:

* Correct React component structure
* Proper state management
* Immutable updates
* CRUD operations
* Context usage
* Zustand usage
* Clean UI
* Clean code organization

---

**🎉 Goal:** Build the same Profile Manager four times:

1. useState Version
2. useReducer Version
3. useReducer + Context Version
4. Zustand Version

If you can build all four, you understand the progression of React state management.
