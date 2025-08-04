<!-- markdownlint-disable MD012 MD026 MD001 MD022 MD032 MD029 MD019 MD034 MD031 MD047 MD040 MD009 MD058 MD024  -->

# Redux

## 🔷 What is Redux?

Redux is a predictable state container for JavaScript applications. It helps you write applications that behave consistently across different environments (client, server, and native), are easy to test, and can be used with a wide range of libraries and frameworks.

Managing state in large applications can become complex, especially when:
- manage the state of multiple components.
- update the state based on user actions or other events.
- ensure that the state is consistent and predictable.

> Redux solves this by introducing a single source of truth and a strict set of rules for how state can be changed.

#### 🧠 Core Concepts of Redux
Redux revolves around three main principles:

1. **Single Source of Truth**: The state of your application is stored in a single object tree within a single store. This makes it easier to track changes and debug your application.

```javascript
const initialState = {
  user: null,
  posts: [],
};
```

2. **State is Read-Only**: The only way to change the state is by dispatching actions. Actions are plain JavaScript objects that describe what happened in the application.

```javascript
const addPostAction = {
  type: 'ADD_POST',
  payload: {
    id: 1,
    title: 'My First Post',
    content: 'This is the content of my first post.',
  },
};
```
