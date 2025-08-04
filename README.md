<!-- markdownlint-disable MD012 MD026 MD001 MD022 MD032 MD029 MD019 MD034 MD031 MD047 MD040 MD009 MD058 MD024  -->

# Redux

## 🔷 What is Redux?

Redux is a predictable state container for JavaScript applications. It helps you write applications that behave consistently across different environments (client, server, and native), are easy to test, and can be used with a wide range of libraries and frameworks.

Managing state in large applications can become complex, especially when:
- manage the state of multiple components.
- update the state based on user actions or other events.
- ensure that the state is consistent and predictable.

#### ⚙️ Installation core Redux

```bash
npm install redux
```

#### ⚙️ Installation core React-Redux

```bash
npm install react-redux
```

> Redux solves this by introducing a single source of truth and a strict set of rules for how state can be changed.

## 🔷 Redux Toolkit
Redux Toolkit is the official, recommended way to write Redux logic. It provides a set of tools and best practices to simplify the process of writing Redux applications.
It includes utilities for:
- **Store setup**: Simplifies the configuration of the Redux store.
- **Reducers**: Provides a way to create reducers using the `createSlice` function.
- **Actions**: Automatically generates action creators and action types.
- **Immutable updates**: Uses the Immer library to allow for simpler immutable state updates.

#### ⚙️ Installation

```bash
npm install @reduxjs/toolkit
```





