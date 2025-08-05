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






