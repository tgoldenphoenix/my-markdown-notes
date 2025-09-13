# Redux notes

Redux is a _JS library_. You can use Redux together with React, or with any other view library. Redux là front-end library.

Redux is much more powerful and provides a large number of features that the React **Context API** doesn't provide.

Chỉ nên dùng redux nếu người dùng lớn, real-time (web bảng giá chứng khoáng). Vài ngàn người dùng thì không cần.\
Khác như là change theme, change language.

Phải dùng **redux-toolkit** để viết ứng dụng redux. Viết riêng khó hơn.

The core of Redux is reducers, actions, and the store.

## Functional programming

Redux is based on function programming. JS is a multi-paradigm language. It can support OOP, functional programming, procedural programming.

Do not mutate the data.

A **Higher order function** is a function that takes a function as an argument, or returns it, or both.  
Example in JS: `map(), setTimeout()`

**Currying** is a technique that allows us to take a function that has N arguments and _convert_ it into a function that has a single argument.

```javascript
add(1)(5); // currying
add(1,5); // normal
```

In redux, reducer are pure functions.

## Pure functions

Concurrency

Reducer are pure functions.

## Store

Store is the single source of truth. Component sẽ đến store "mua hàng" về. A store can have many **slices**.

`getState()`

The function called `createSlice` from Redux Toolkit auto create actions.

Redux Toolkit's `createSlice` uses a library called `Immer` inside. Immer tracks all the changes you've tried to make, and then uses that list of changes to return a safely immutably updated value, as if you'd written all the immutable update logic by hand.

React components need to use the `useSelector` and `useDispatch` hooks to talk to the Redux store.

## Reducer & Actions

The **store** is immutable, we use a **reducer function** to update values. Reducers take in two arguments: the store and an **action**.

**Slice** là khái niệm của redux toolkit, không có trong core redux. Each slice have its own corresponding reducer.  
Function `createSlice()` của nó automatically use `Immer` internally to let you write simpler immutable update logic using "mutating" syntax.

An **action** is a plain JavaScript object that has a `type` field.

A **thunk** is a specific kind of Redux function that can contain asynchronous logic.

## createAsyncThunk

A function that accepts a Redux action type string and a callback function that should return a promise.

## Terms

store: store the global states that we want to access

Trong Store có nhiều slices. A **slice** is a collection of Redux reducer logic and actions for a single feature in your app, typically defined together in a single file. The name comes from splitting up the root Redux state object into multiple "slices" of state.

Redux allows store setup to be customized with different kinds of plugins ("**middleware**" and "enhancers").

**reducers** take the action and apply change on the states

The Redux store has a method called **dispatch**. The only way to update the state is to call store.dispatch() and pass in an action object.

## Persistence and Rehydration

In the context of Redux and Redux Persist, rehydration is the process of restoring the Redux store's state from a persisted storage (like `localStorage` or AsyncStorage) after the application has been refreshed or closed and reopened.

Mặc định redux nó lưu trên RAM (memory), khi refresh trang or close browser sẽ mất dữ liệu. Mình phải dùng redux persist để lưu trong local storage.

## References

[why & when to use redux](https://www.youtube.com/watch?v=ayl5Olj50yQ)

