<!-- markdownlint-disable MD012 MD026 MD001 MD022 MD032 MD029 MD019 MD034 MD031 MD047 MD040 MD009 MD058 MD024  -->

<div>

 <img src="https://redux-toolkit.js.org/img/redux-logo-landscape.png" style="width:100%; height:auto;">

</div>

### 🔷 What is Redux?

Redux is a predictable state container for JavaScript applications. It helps you write applications that behave consistently across different environments (client, server, and native), are easy to test, and can be used with a wide range of libraries and frameworks.

Managing state in large applications can become complex, especially when:
- manage the state of multiple components.
- update the state based on user actions or other events.
- ensure that the state is consistent and predictable.

#### 🎯 Why Do We Need Redux?

In a React app, state is often managed using:

- `useState()`
- `useContext()`
- `useReducer()`

However, as an app grows:

- You get nested props (prop drilling).
- State needs to be shared between distant components.
- Debugging and maintaining become hard.

👉 Redux solves this by providing a single source of truth (the store) accessible by any component.

**🧩 Redux Core Principles**

1. Single Source of Truth
   - The entire app’s state is stored in one central store.
2. State is Read-only
   - You can’t directly modify the state; instead, you dispatch actions.
3. Changes via Pure Functions
   - State changes are handled by reducers, which are pure functions returning new state objects.


#### ⚙️ Installation core Redux

```bash
npm install redux
```

#### ⚙️ Installation React-Redux

```bash
npm install react-redux
```

### 🔷 Redux Toolkit
Redux Toolkit is the official, recommended way to write Redux logic. It provides a set of tools and best practicing to simplify the process of writing Redux applications.

#### 🎯 Why Redux Toolkit?

Traditional Redux required a lot of repetitive setup:

- Creating action types, action creators, and reducers manually.
- Handling immutable state updates.
- Configuring middlewares manually.

👉 Redux Toolkit automates and simplifies all of that with:

- Cleaner syntax
- Fewer files
- Built-in best practices
- Support for async actions


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

**⚙️ Project Setup**

Define Root State and Dispatch Types:

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


Handles asynchronous logic (like API requests).
Example: fetching users from an API.

`createAsyncThunk()`
```typescript
import { createAsyncThunk, createSlice } from '@reduxjs/toolkit';
import axios from 'axios';

export const fetchUsers = createAsyncThunk('users/fetch', async () => {
  const response = await axios.get('https://jsonplaceholder.typicode.com/users');
  return response.data;
});

interface UserState {
  users: any[];
  loading: boolean;
  error: string | null;
}

const initialState: UserState = {
  users: [],
  loading: false,
  error: null,
};

const userSlice = createSlice({
  name: 'users',
  initialState,
  reducers: {},
  extraReducers: (builder) => {
    builder
      .addCase(fetchUsers.pending, (state) => { state.loading = true; })
      .addCase(fetchUsers.fulfilled, (state, action) => {
        state.loading = false;
        state.users = action.payload;
      })
      .addCase(fetchUsers.rejected, (state, action) => {
        state.loading = false;
        state.error = action.error.message || 'Failed to fetch users';
      });
  },
});

export default userSlice.reducer;
```

✅ Automatically handles:

- Loading state (`pending`)
- Success (`fulfilled`)
- Error (`rejected`)


Connect Redux to the App:

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

Use Redux State and Dispatch in Components:

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

- **State Management**: It provides a centralized store for managing the state of the application, making it easier to track and update state changes it.
- **Asynchronous Actions**: Redux middleware like Redux Thunk or Redux Saga allows for handling asynchronous actions, making it easier to manage side effects such as API calls.
- **Predictability**: Redux enforces a unidirectional data flow, which makes the application state predictable and easier to debug.
- **Testing**: Redux's predictable state management makes it easier to write tests for your application.
- **Decoupling**: Redux promotes a decoupled architecture, where components can be easily connected to the Redux store without tightly coupling them together.
- **Scalability**: Redux's modular architecture allows for easy scalability and maintainability of the application.
- **Middleware Support**: Redux supports middleware, allowing for side effects like API calls and logging to be handled in a clean and organized way.
- **Time Travel**: Redux DevTools enables time travel debugging, allowing developers to inspect and revert state changes easily.


❓Difference between Redux and Context API
|  | Feature | Redux | Context API |
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


### RTK Query

RTK Query is a powerful data fetching and caching tool built into Redux Toolkit.
It simplifies API calls, caching, loading states, and data synchronization between your frontend and backend — all automatically.

#### 🎯 Why Use RTK Query?

Traditional Redux async workflows require:

- Writing createAsyncThunk() for every API request
- Managing loading, error, and success states manually
- Writing reducers to update the store

👉 RTK Query automates all of this. It:

- Fetches and caches data automatically
- Refetches on demand
- Invalidates stale data
- Handles loading/error states out of the box

#### 🏗️ Setup Example

**✅ Install Dependencies**

```bash
npm install @reduxjs/toolkit react-redux axios
```

**✅ Set Up RTK Query API Slice (`services/api.ts`)**

```typescript
import { createApi, fetchBaseQuery } from '@reduxjs/toolkit/query/react';

// Define your API service
export const userApi = createApi({
  reducerPath: 'userApi', // unique key for store
  baseQuery: fetchBaseQuery({ baseUrl: 'https://jsonplaceholder.typicode.com' }),
  tagTypes: ['Users'], // used for cache invalidation
  endpoints: (builder) => ({
    // GET users
    getUsers: builder.query<any[], void>({
      query: () => 'users',
      providesTags: ['Users'],
    }),

    // GET single user
    getUserById: builder.query<any, number>({
      query: (id) => `users/${id}`,
      providesTags: ['Users'],
    }),

    // POST new user
    addUser: builder.mutation<any, Partial<any>>({
      query: (newUser) => ({
        url: 'users',
        method: 'POST',
        body: newUser,
      }),
      invalidatesTags: ['Users'],
    }),

    // DELETE user
    deleteUser: builder.mutation<void, number>({
      query: (id) => ({
        url: `users/${id}`,
        method: 'DELETE',
      }),
      invalidatesTags: ['Users'],
    }),
  }),
});

// Export hooks generated by RTK Query
export const {
  useGetUsersQuery,
  useGetUserByIdQuery,
  useAddUserMutation,
  useDeleteUserMutation,
} = userApi;
```

**✅ Configure Store (`app/store.ts`)**

```typescript
import { configureStore } from '@reduxjs/toolkit';
import { userApi } from '../services/api';

export const store = configureStore({
  reducer: {
    [userApi.reducerPath]: userApi.reducer,
  },
  middleware: (getDefaultMiddleware) =>
    getDefaultMiddleware().concat(userApi.middleware),
});

export type RootState = ReturnType<typeof store.getState>;
export type AppDispatch = typeof store.dispatch;
```

**✅ Wrap App with Provider (`index.tsx`)**

```typescript
import React from 'react';
import ReactDOM from 'react-dom';
import { Provider } from 'react-redux';
import { store } from './app/store';
import App from './App';

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
);
```

**✅ Create Component Using RTK Query (`components/UserList.tsx`)**

```typescript
import React, { useState } from 'react';
import {
  useGetUsersQuery,
  useAddUserMutation,
  useDeleteUserMutation,
} from '../services/api';

const UserList = () => {
  const { data: users, error, isLoading } = useGetUsersQuery();
  const [addUser] = useAddUserMutation();
  const [deleteUser] = useDeleteUserMutation();
  const [name, setName] = useState('');

  const handleAddUser = async () => {
    if (name.trim() === '') return;
    await addUser({ name, email: `${name}@example.com` });
    setName('');
  };

  if (isLoading) return <p>Loading users...</p>;
  if (error) return <p>Error fetching users!</p>;

  return (
    <div style={{ padding: '20px' }}>
      <h2>User List</h2>

      <div style={{ marginBottom: '20px' }}>
        <input
          type="text"
          placeholder="Add user name"
          value={name}
          onChange={(e) => setName(e.target.value)}
        />
        <button onClick={handleAddUser}>Add User</button>
      </div>

      <ul>
        {users?.map((user: any) => (
          <li key={user.id}>
            {user.name} - {user.email}{' '}
            <button onClick={() => deleteUser(user.id)}>❌ Delete</button>
          </li>
        ))}
      </ul>
    </div>
  );
};

export default UserList;
```








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

#### ❓key components of Redux architecture

The key components of Redux architecture revolve around a unidirectional data flow and a predictable state container.
Here are the core pieces:

#### 🔑 1. Store

The Redux store is a centralized object that holds the state of the application. It allows components to access and update the state using dispatch and selectors. The state inside the store can only be modified by dispatching actions.

- Holds the application state.
- There is a single store in a Redux app (centralized state).
- Provides methods:

  - `getState()` – gets the current state.
  - `dispatch(action)` – sends an action to the reducer.
  - `subscribe(listener)` – registers a listener that runs when the state changes.

```javascript
// store/index.js
import { createStore, combineReducers } from 'redux';
import { taskReducer } from '../reducers/taskReducer';

const rootReducer = combineReducers({
  taskState: taskReducer,
});

const store = createStore(rootReducer);

export default store;
```


#### 🔑 2. Actions

Actions in Redux are plain JavaScript objects that contain information about an event that occurred in the application. They are the only way to send data to the Redux store and trigger state updates.

Each action must have a type property that describes the event, and it can optionally carry a payload with additional data.

- Plain JavaScript objects that describe what happened.
- Must have a `type` field (string) and can include additional data (payload).

```javascript
// actions/taskActions.js
export const ADD_TASK = 'ADD_TASK';

export const addTask = (task) => ({
  type: ADD_TASK,
  payload: task,
});
```

#### 🔑 3. Reducers

In Redux, reducers are pure functions that handle state logic, accepting the initial state and action type to update and return to the state, facilitating changes in React view components.

- Pure functions that take the current `state` and an `action`, and return a new state.
- Reducers do not mutate the state; they return a new copy.


```javascript
// reducers/taskReducer.js
import { ADD_TASK } from '../actions/taskActions';

const initialState = {
  tasks: [],
};

export const taskReducer = (state = initialState, action) => {
  switch (action.type) {
    case ADD_TASK:
      return {
        ...state,
        tasks: [...state.tasks, action.payload],
      };
    default:
      return state;
  }
};
```

#### 🔑 4. Dispatch

- The role of the dispatch function in Redux is to send actions to the store.
- It's a method provided by the Redux store that accepts an action object as its argument and initiates the process of updating the application's state based on that action.

```javascript
// components/AddTask.js
import { useDispatch } from 'react-redux';
import { addTask } from '../actions/taskActions';

const AddTask = () => {
  const dispatch = useDispatch();

  const handleAdd = () => {
    const newTask = {
      id: Date.now(),
      title: 'Design login page',
      assignedTo: 'John',
      status: 'Pending',
    };
    dispatch(addTask(newTask));
  };

  return <button onClick={handleAdd}>Add Task</button>;
};
```

#### 🔑 5. State

- The entire app’s state is stored in a single JavaScript object within the store.
- It's read-only and can only be updated through dispatching actions and reducers.

```javascript
// components/TaskList.js
import { useSelector } from 'react-redux';

const TaskList = () => {
  const tasks = useSelector((state) => state.taskState.tasks);

  return (
    <ul>
      {tasks.map((task) => (
        <li key={task.id}>
          {task.title} - Assigned to {task.assignedTo}
        </li>
      ))}
    </ul>
  );
};
```

#### 🔑 6. Middleware (Optional but common)

Middleware in Redux is like a gatekeeper that stands between the actions you dispatch in your app and the part of Redux responsible for updating the state. It's is used to catch these actions before they go to the state updates and do some extra stuff with them. For example, it can check if a user is authenticated or not before giving the permissions.

- Used to extend Redux with custom functionality.
- Intercepts actions before they reach the reducer.
- Examples: redux-thunk, redux-saga, logging, error tracking.

```javascript
// With redux-thunk
export const saveTask = (task) => async (dispatch) => {
  const response = await fetch('/api/tasks', {
    method: 'POST',
    body: JSON.stringify(task),
  });
  const data = await response.json();
  dispatch(addTask(data));
};
```

#### 🔁 Unidirectional Flow in Practice:

- User clicks "Add Task" ➡️
- `dispatch(addTask(...))` is called ➡️
- Redux sends action to `taskReducer` ➡️
- Reducer returns a new state ➡️
- Store updates state ➡️
- `TaskList` re-renders with new task


#### ❓`<Provider>` Component in React Redux

The `<Provider>` component in React Redux serves a critical role:
> 👉 It makes the Redux store available to the entire React component tree.

When you use Redux in a React app, you need a way to allow React components (especially deeply nested ones) to:

- Access the state (`useSelector`)
- Dispatch actions (`useDispatch`)

Instead of manually passing the Redux store down via props, React Redux uses React Context internally. The `<Provider>` wraps your app and injects the Redux store into the context, so any child component can connect to it.

🔧 How It Works:

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

- store is the Redux store you created.
- Every component inside `<App />` (and nested below) now has access to the Redux store.

🧩 Without `<Provider>`, Redux won't work properly in React:

If you try to use `useSelector()` or `useDispatch()` outside of a `<Provider>`, you'll get an error like:

> ❌ "Could not find react-redux context value; please ensure the component is wrapped in a `<Provider>`"


#### ❓`connect` function in React Redux

It allows you to:

- Read data from the Redux store and pass it as props to your component.
- Dispatch actions from the component as props.

🔧 Syntax

```javascript
connect(mapStateToProps, mapDispatchToProps)(YourComponent)
```

1. `mapStateToProps(state)` function:

- In React Redux, the `mapStateToProps` function is used to connect Redux state to React component props.
- It's a function that maps parts of the Redux state to the props of a React component, and allows the component to access and use that state.

```typescript
const mapStateToProps = (state: RootState) => {
  return {
    // Map the Redux state to component props
    // For example, map the 'count' state to a prop called 'count'
    count: state.count,
  };
};
```

2. `mapDispatchToProps(dispatch)` function:

- In React Redux, the `mapDispatchToProps` function is used to connect Redux actions to React component props.
- It's a function that maps Redux actions to props of a React component, and allows the component to dispatch those actions.

```typescript
const mapDispatchToProps = (dispatch: Dispatch) => {
  return {
    // Map Redux actions to component props
    // For example, map the 'increment' action to a prop called 'increment'
    increment: () => dispatch(increment()),
  };
};
```

✅ Full Example:

```typescript
import { connect } from 'react-redux';
import { RootState } from './store';
import { increment } from './actions';
interface Props {
  count: number;
  increment: () => void;
}
const mapStateToProps = (state: RootState) => {
  return {
    count: state.count,
  };
};
const mapDispatchToProps = (dispatch: Dispatch) => {
  return {
    increment: () => dispatch(increment()),
  };
};
const YourComponent = connect(mapStateToProps, mapDispatchToProps)(YourComponent);
```



#### ❓Typical flow of data like in a React with Redux app

In a React + Redux application, the typical flow of data follows a unidirectional pattern, which helps maintain predictability and ease of debugging. Here's a high-level overview of the flow:

#### 🔁 Typical Data Flow in React with Redux

1. User Interaction (View Layer - React)

- A user interacts with the UI (e.g. clicks a button, submits a form).
- This triggers an event handler in a React component.

2. Dispatching an Action

- The component dispatches an action using the `dispatch()` function from Redux.

```javascript
const dispatch = useDispatch();
dispatch(someAction());
```
> An action is a plain JavaScript object with a `type` field and optionally a `payload`.

3. Middleware (Optional)

- Middleware (like `redux-thunk`, `redux-saga`) can intercept actions before they reach the reducer.
- Use case: perform asynchronous operations (e.g., API calls).

```javascript
dispatch(fetchUsers()); // Thunk action creator
```

4. Reducers

- The action is sent to the reducers, which are pure functions.
- A reducer determines how the state should change based on the action.

```javascript
function todoReducer(state = [], action) {
  switch (action.type) {
    case 'ADD_TODO':
      return [...state, action.payload];
    default:
      return state;
  }
}
```
5. Store Update

- The reducer returns a new state, which is stored in the Redux store.
- Redux does not mutate the existing state — it returns a new copy.

6. React Components Re-render

- Components connected to the Redux store (via `connect()` or `useSelector()`) automatically receive the updated state.

```javascript
function TodoList() {
  const todos = useSelector(state => state.todos);
}
```
> When the relevant slice of state changes, the component re-renders with new props/data.

#### 🧠 Summary Diagram

```scss
User Event
   ↓
dispatch(action)
   ↓
Middleware (optional)
   ↓
Reducer(s)
   ↓
New State → Redux Store
   ↓
React Component (via useSelector or connect)
   ↓
Updated UI
```


#### ❓Immutability in Redux

- In Redux, immutability refers to the principle of not mutating state directly.
- Instead, when you need to update the state, you create a new copy of the state object with the desired changes applied.
- This ensures that the original state remains unchanged, and provides predictable state management and efficient change detection.

#### ✅ Immutability Example

```javascript
function addTodo(state, action) {
  const newTodo = { id: state.length + 1, text: action.payload };
  return [...state, newTodo];
}
// Before
const initialState = [{ id: 1, text: 'Learn Redux' }];
const newState = addTodo(initialState, { type: 'ADD_TODO', payload: 'Learn React' });
// After
const newState = addTodo(initialState, { type: 'ADD_TODO', payload: 'Learn React' });
```


#### ❓Strategies for handling side-effects in Redux applications

In Redux, side effects are anything that affects the outside world or depends on it — like API calls, timers, logging, or interacting with localStorage.
Reducers must stay pure, so side effects are handled outside of them.

#### 1️⃣ Redux Thunk – Most Common & Simple

- Lets you write action creators that return a function instead of an action object.
- That function receives `dispatch` and `getState`, so it can:
  - Perform async work (like `fetch()` calls).
  - Dispatch multiple actions at different stages.

```javascript
export const fetchUser = (id) => async (dispatch) => {
  dispatch({ type: 'USER_FETCH_REQUEST' });

  try {
    const res = await fetch(`/api/users/${id}`);
    const data = await res.json();
    dispatch({ type: 'USER_FETCH_SUCCESS', payload: data });
  } catch (err) {
    dispatch({ type: 'USER_FETCH_FAILURE', error: err.message });
  }
};
```

> ✅ Pros: Simple, minimal setup.
> ⚠️ Cons: Can get messy with complex async flows.


#### 2️⃣ Redux Saga – For Complex Flows

- Uses generator functions (`function`*) to describe side effects as declarative "effects.
- Good for:
  - Complex async logic
  - Cancelling tasks
  - Listening to multiple actions over time

```javascript
function* fetchUser(action) {
  try {
    const data = yield call(api.getUser, action.payload);
    yield put({ type: 'USER_FETCH_SUCCESS', payload: data });
  } catch (err) {
    yield put({ type: 'USER_FETCH_FAILURE', error: err.message });
  }
}

function* watchFetchUser() {
  yield takeEvery('USER_FETCH_REQUEST', fetchUser);
}
```

> ✅ Pros: Great for orchestration, cancellations, retries.
> ⚠️ Cons: Steeper learning curve.



#### ❓Selectors in Redux

In Redux, selectors are functions used to extract and derive specific pieces of data from the store’s state.
They’re basically the “query layer” between your Redux store and your UI components.

#### 🧩 Why Use Selectors?

1. **Encapsulation** – Components don’t need to know the exact shape of the state.
2. **Reusability** – You can use the same selector in multiple places.
3. **Testability** – Selectors are pure functions, so they’re easy to unit test.
4. **Performance** – With memoization (e.g., reselect), selectors can avoid recomputing unless relevant state changes.


🔹 Basic Example

Without a selector:
```javascript
const todos = useSelector(state => state.todos);
```

With a selector:
```javascript
// selectors.js
export const selectTodos = (state) => state.todos;

// Component
const todos = useSelector(selectTodos);
```

🔹 Derived Data Example
Selectors can compute values from state without storing them in state:
```javascript
export const selectCompletedTodos = (state) =>
  state.todos.filter(todo => todo.completed);

const completed = useSelector(selectCompletedTodos);
```

🔹 Memoized Selector with Reselect
Memoization avoids recalculating if inputs haven’t changed:
```javascript
import { createSelector } from 'reselect';

export const selectTodos = state => state.todos;

export const selectCompletedTodos = createSelector(
  [selectTodos],
  todos => todos.filter(todo => todo.completed)
);
```




