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

#### ⚙️ Installation React-Redux

```bash
npm install react-redux
```

## 🔷 Redux Toolkit
Redux Toolkit is the official, recommended way to write Redux logic. It provides a set of tools and best practices to simplify the process of writing Redux applications.
It includes utilities for:
- **Store setup**: Simplifies the configuration of the Redux store.
- **Reducers**: Provides a way to create reducers using the `createSlice` function.
- **Actions**: Automatically generates action creators and action types.
- **Immutable updates**: Uses the Immer library to allow for simpler immutable state updates.

#### ⚙️ Installation Redux-Toolkit

```bash
npm install @reduxjs/toolkit
```

Redux Toolkit TypeScript Quick Start:

⚙️ **Project Setup**

Define Root State and Dispatch Types

`app/store.ts`
```typescript
import { configureStore } from '@reduxjs/toolkit';
import counterReducer from '../features/counter/counterSlice';

export const store = configureStore({
  reducer: {
    counter: counterReducer,
  },
});

// Infer types for dispatch and state
export type RootState = ReturnType<typeof store.getState>;
export type AppDispatch = typeof store.dispatch;
```

Define Typed Hooks

`app/hooks.ts`
```typescript
import { TypedUseSelectorHook, useDispatch, useSelector } from 'react-redux'
import { RootState, AppDispatch } from './store'

export const useAppDispatch: () => AppDispatch = useDispatch
export const useAppSelector: TypedUseSelectorHook<RootState> = useSelector
```

Create a Slice

`src/features/counter/counterSlice.ts`
```typescript
import { createSlice, PayloadAction } from '@reduxjs/toolkit';

interface CounterState {
  value: number;
}

const initialState: CounterState = {
  value: 0,
};

export const counterSlice = createSlice({
  name: 'counter',
  initialState,
  reducers: {
    increment: (state) => {
      state.value += 1;
    },
    decrement: (state) => {
      state.value -= 1;
    },
    incrementByAmount: (state, action: PayloadAction<number>) => {
      state.value += action.payload;
    },
  },
});

export const { increment, decrement, incrementByAmount } = counterSlice.actions;

export default counterSlice.reducer;
```

Connect Redux to the App

`src/index.tsx`
```typescript
import React from 'react';
import ReactDOM from 'react-dom/client';
import { Provider } from 'react-redux';
import { store } from './app/store';
import App from './App';

ReactDOM.createRoot(document.getElementById('root')!).render(
  <React.StrictMode>
    <Provider store={store}>
      <App />
    </Provider>
  </React.StrictMode>
);
```

Use Redux State and Dispatch in Components

`src/features/counter/Counter.tsx`
```typescript
import React from 'react';
import { increment, decrement, incrementByAmount } from './counterSlice';
import { useAppDispatch, useAppSelector } from '../../app/hooks';

const Counter: React.FC = () => {
  const count = useAppSelector((state) => state.counter.value);
  const dispatch = useAppDispatch();

  return (
    <div>
      <h1>Count: {count}</h1>
      <button onClick={() => dispatch(increment())}>+</button>
      <button onClick={() => dispatch(decrement())}>-</button>
      <button onClick={() => dispatch(incrementByAmount(5))}>+5</button>
    </div>
  );
};

export default Counter;
```


❓ Redux solves several challenges in complex front-end applications:

- **State Management**: It provides a centralized store for managing the state of the application, making it easier to track and update state changes.
- **Asynchronous Actions**: Redux middleware like Redux Thunk or Redux Saga allows for handling asynchronous actions, making it easier to manage side effects such as API calls.
- **Predictability**: Redux enforces a unidirectional data flow, which makes the application state predictable and easier to debug.
- **Testing**: Redux's predictable state management makes it easier to write tests for your application.
- **Decoupling**: Redux promotes a decoupled architecture, where components can be easily connected to the Redux store without tightly coupling them together.
- **Scalability**: Redux's modular architecture allows for easy scalability and maintainability of the application.
- **Middleware Support**: Redux supports middleware, allowing for side effects like API calls and logging to be handled in a clean and organized way.
- **Time Travel**: Redux DevTools enables time travel debugging, allowing developers to inspect and revert state changes easily.


❓Difference between Redux and Context API
|--| Feature | Redux | Context API |
|--|---------|-------|-------------|
| 1 | **Purpose** | State management for complex applications | Lightweight state management for simpler use cases |
| 2 | **Middleware** | Supports middleware for side effects (e.g., Redux Thunk, Redux Saga) | No built-in middleware support |
| 3 | **DevTools** | Provides powerful DevTools for debugging and time travel | Limited DevTools support |
| 4 | **Boilerplate** | More boilerplate code required for setup and configuration | Less boilerplate, easier to set up |
| 5 | **Learning Curve** | Steeper learning curve due to concepts like actions, reducers, and middleware | Easier to learn, especially for React developers |
| 6 | **Use Cases** | Best suited for large, complex applications with extensive state management needs | Ideal for small to medium-sized applications or specific components |
| 7 | **Community and Ecosystem** | Large community with extensive ecosystem of libraries and tools | Smaller community, fewer third-party libraries |
| 8 | **Data Flow** | Unidirectional data flow, making it easier to understand and debug | Bidirectional data flow, which can lead to more complex interactions |
| 9 | **API** | Well-defined API with actions, reducers, and middleware | Simpler API, primarily using React's built-in context |


❓Pure functions in the context of Redux

A pure function is a function that:

1. Returns the same output given the same input (no randomness or time-based logic).

2. Does not cause side effects, such as:

- Modifying external variables or state
- Making API calls
- Logging to the console
- Writing to disk or localStorage



In Redux:

Reducers in Redux must be pure functions. That means:

- They take two inputs: the current state and an action
- They return a new state based solely on those inputs
- They do not modify the original state (they are immutable)
- They do not perform side effects

#### 🏗️ Example of a Pure Reducer:
```javascript
function counterReducer(state = 0, action) {
  switch (action.type) {
    case 'INCREMENT':
      return state + 1;
    case 'DECREMENT':
      return state - 1;
    default:
      return state;
  }
}
```

Why Redux Uses Pure Functions:

- **Predictability**: Given the same state and action, the same result is always returned.

- **Testability**: Easier to write unit tests because there's no external dependency.

- **Debuggability**: Easier to track state changes and use features like time-travel debugging.


#### 🏗️ Impure Example (What NOT to do in a Reducer):

```javascript
function badReducer(state = [], action) {
  switch (action.type) {
    case 'ADD_ITEM':
      state.push(action.payload); // ❌ mutating state
      return state;
    case 'FETCH_DATA':
      fetch('/api/data'); // ❌ side effect
      return state;
    default:
      return state;
  }
}
```


