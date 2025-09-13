# React & Nextjs notes

## React vs nextjs

React is NOT a JavaScript framework, it is a JavaScript **library** (thư viện) to build component-based web UI developed by Facebook in 2011.  
A library provides ready-made tools that you can use in your project without starting from scratch. On the other hand, a framework is like a ready-made structure that helps you build your project.

Team react recommend dùng framework nextjs khi build production-graded projects  
Next.js is a popular React-based framework used in building web applications with the use of React components. Next.js provides additional structure, features, and optimization for your web application.

React là single page application using client-side rendering technique.  
Unlike React, Next.js also supports server-side rendering (SSR). This enables a faster loading of the web page and also improves the SEO.

[blog](https://www.freecodecamp.org/news/nextjs-vs-react-differences/) compare react & nextjs very good

## Create a new project

Creating a new React project: You can use cra (create react app) or vite. Vite is not oppiniated like **cra**. Vite is also a build tool.

React **components** are JavaScript functions that return markup:

- `sfc`: stateless functional component
- `rsf`: react stateless function

[4:37](https://www.youtube.com/watch?v=NbTrGcz4DW8&list=PL4cUxeGkcC9gZD-Tvwfod2gaISzfRiP9d&index=6) giải thích the `{ {} }` of JSX very easy to understand

Event handlers receive an [event object](https://react.dev/learn/responding-to-events) as their only argument. By convention, it’s usually called `e`, which stands for “event”. You can use this object to read information about the event.

We use `useState()` hook instead of normal JS variables because normal JS variables does not trigger a re-render of the page when they are modified. States in React are variables that are reactive which means they triggered a component re-render when their value is changed.

React snippets
traditional react `rafce` => dùng cái này
typescript react `tsrafce`

## React components

You can think of components as custom HTML elements that encapsulate their own logic and UI structure. They can accept inputs called props (short for properties) and return React elements describing what should appear on the screen.

### Function Components vs Class Components

**Function components (stateless components)** are defined using `function` keyword (or arrow function syntax) just like a normal JS function. They takes props as input and return JSX elements. Mình chủ yếu dùng loại này nè.

Function components don't have **lifecycle methods**. However, with React Hooks, you can use the `useEffect` Hook to replicate lifecycle behavior (like `componentDidMount`, componentDidUpdate, componentWillUnmount, and so on).

**Class components** (stateful components) are ES6 classes that extend from `React.Component` or `React.PureComponent`. They have a render() method where you define the structure of your component's UI using JSX. They are defined using the `class and extends` keyword.

With the introduction of **React Hooks**, function components gained the ability to manage state and use lifecycle methods, blurring the distinction between function and class components.  
Functional components are generally more concise and easier to read, especially for simpler components.

### React Server Components (RSC)

There are 02 types of React components: Server components and Client components.

Server components:

- By **default**, Next.js treats all components as Server components
- These components can perform server-side tasks like reading files or fetching data directly from a database
- Wait for certain operations to complete **BEFORE** rendering content (`async/await`)
- The trade-off is that they can't use React hooks or handle user interactions

Client Components:

- To create a Client component, you'll need to add the `use client` directive **at the top** of your component file
- While Client components can't perform server-side tasks like reading files, they **can use hooks** and handle user interactions
- Client components are the traditional React components you're already familiar with from previous versions of React

## useEffect, useRef and useCallback

You pass a `setup` function into useEffect

`useEffect()` reactive dependency:

- If you pass **no dependency array** at all, your Effect runs after every single render (and re-render) of your component.
- Passing an empty dependency array: only run after the initial render.

useEffect runs after your component renders. Khi component first render thì có chạy code ngoài `useEffect` và `return` XML, sau đó `useEffect` chạy async task, sau khi async code của nó chạy xong thì code ngoài `useEffect` chạy lại 1 lần nữa

Usually you want specify the dependency array.

Your setup function may also optionally return a **cleanup function**. When your component is added to the DOM, React will run your setup function. After every re-render with changed dependencies, React will first run the cleanup function (if you provided it) with the old values, and then run your setup function with the new values. After your component is removed from the DOM, React will run your cleanup function.

`UseRef` stores element. `useState` stores values. useRef is used for DOM manipulation

useState hook is responsible for re-rendering the DOM elements. In the case of useRef, useRef doesn’t notify you when its content changes. Mutating the .current property doesn’t cause a re-render.

`useRef` return a mutable ref object which has a `.current` value that you can access.

References

[useRef video](https://www.youtube.com/watch?v=yviJikU4gwk) by Hitesh Choudhary

## JSX

`<>...</>` is called React Fragment

Trong JSX, tất cả thẻ đều phải close lại, kể cả  `<img/>` cũng cần phải đóng. Trong HTML thì không cần.

**JSX (JavaScript XML)** is a syntax extension to JavaScript, primarily used with the React library, that allows developers to write HTML-like code within JavaScript.  
While JSX became widely popular and is most commonly associated with React, it is not exclusively used with React.

Babel convert JSX into plain Javascript.

## State & props

State is different from normal variables declared with `let` & `const` in that they can be mutated interactively by the user.

If you render the same React component multiple times, each will get its own state.

States are variable so they can be mutated. So states are used to introduce interactivity.

Khi state của 1 component is changed (when the set function returned from useState() is called) thì nó sẽ auto re-render cái component đó. When you call a set function in a component, React will know that the state has changed and automatically updates the child components inside too. Calling the setSquares function lets React know the state of the component has changed. This will trigger a re-render of the components that use the squares state (Board) as well as its child components (the Square components that make up the board). When the Board’s state changes, both the Board component and every child Square re-renders automatically.

## Re-rendering mechanism in React

React determines what components to render through a process involving the Virtual DOM and reconciliation.

When the application first mounts, React renders all of our components and comes up with a sketch for what the DOM should look like (virtual DOM).

- **Every re-render in React starts with a state change**. When a component's internal state is updated using `useState` (for functional components) or `this.setState` (for class components). It's the only “trigger” in React for a component to re-render. In the past, there was a `forceUpdate()` method that also triggered a re-render, but that doesn't exist anymore.
- When a component re-renders, it also re-renders all of its descendants.
- Context changes: When a component consumes a context that has been updated (like redux).

When a re-render is triggered, React does not directly manipulate the actual DOM. Instead, it creates a new Virtual DOM tree representing the updated UI. It then efficiently compares this new Virtual DOM with the previous one, identifying the minimal set of changes needed to update the real DOM. This comparison process is known as **reconciliation**.

Only the identified changes are then "committed" to the actual browser DOM, leading to a visible update of the user interface. This selective updating is a key reason for React's performance.

Misconception 01: **The entire app does NOT re-renders whenever a state variable changes**. Re-renders only affect the component that owns the state + its descendants (if any).  
Data can't flow "up" in a React application. Chỉ có truyền props xuống thôi. Nên child component change their states and re-render would NOT affect the parent components. The point of a re-render is to figure out how a state change should affect the user interface. And so we need to re-render all **potentially-affected** components, to get an accurate snapshot.

Big Misconception 02: **A component will re-render because its props change**. In fact, props have nothing to do with re-renders.

When a component re-renders, because one of its state variables has been updated, that re-render will cascade all the way down the tree, in order for React to fill in the details of this new sketch, to capture a new snapshot.

## Thinking in React

When you build a user interface with React, you will first break it apart into pieces called components. Then, you will describe the different visual states for each of your components. Finally, you will connect your components together so that the data flows through them.

1. component hierarchy
2. Build the static version (only use props)
3. Find the minimal but complete representation of UI state
4. Identify where your state should live
Remember: React uses one-way data flow, passing data down the component hierarchy from parent to child component.
5. Add inverse data flow

## React forms handling

react-hook-form

## Authentication

[blog tutorial](https://dev.to/miracool/how-to-manage-user-authentication-with-react-js-3ic5) rất useful

## React router

React router just stimulate the url being changed. In reality, there is only one page.

## React architecture

[React Fiber Architecture](https://github.com/acdlite/react-fiber-architecture) by acdlite

## My bugs

[Vite 'global is not defined'](https://stackoverflow.com/questions/72114775/vite-global-is-not-defined)

## References

This [youtube series](https://www.youtube.com/playlist?list=PL4cUxeGkcC9gZD-Tvwfod2gaISzfRiP9d) by net Ninja is very helpful

[youtube series](https://www.youtube.com/watch?v=eCU7FfMl5WU&list=PLRAV69dS1uWQos1M1xP6LWN6C-lZvpkmq&index=2) by Hitesh Choudhary, deep dive
