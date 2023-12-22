#  Side Effects

## Today's Learning Objectives

Today is the day we save our todos in `localStorage`, so we have them whenever we come back to our app.

## Your Mission

### Step 0. On Side Effects

Before we dive in today's task let's spend a minute explaining what "side effects" are.

A side effect is any code that modifies something outside the function this code is in. It's not a term specific to React (you can review the [chapter on functions](https://www.notion.so/mjsarfatti/Enough-JS-to-Be-Dangerous-d3be5beb31ad4778ada7161fb2cc1a7b#5bada0ad2ac84f56a84871dbfa28f714) from Manuele's guide), but it applies to React components since they are functions.

Things like performing a **network request** (for example getting data from an external API), manually **mutating the DOM** (for example via methods like `document.querySelector` - which is external to React), and **logging** are all examples of side effects.

If you are unsure if some of your code produces a side effect or not, ask your coach!

### Step 1. `useEffect`

Saving data to `localStorage` is a side effect because we are using a browser API (`window.localStorage`), and that is not part of React, nor - obviously - part of our component.

And as you may guess, there is a _hook_ for dealing with something that is a side effect: `useEffect`.

`useEffect` accepts two parameters: the first one is a function with the code that you want to execute. This function by default will run on every update. We'll talk about the second parameter later.

First, let's add to `App.js` a `useEffect` that saves our todos in `localStorage`:

```js
import React, { useState, useEffect } from "react";
import { v4 as uuidv4 } from "uuid";
import TodoList from "./TodoList";
import Form from "./Form";

const LSKEY = "MyTodoApp";

function App() {
  // Initialize the state
  const initialTodos = [];
  const [todos, setTodos] = useState(initialTodos);

  // Update the state
  function addTodo(todo) {
    setTodos([...todos, { id: uuidv4(), todo, completed: false }]);
  }

  // Save todos to localStorage
  useEffect(() => {
    window.localStorage.setItem(LSKEY + ".todos", JSON.stringify(todos));
  });

  return (
    <div className="App">
      <h1>My Todos</h1>
      <Form addTodo={addTodo} />
      <TodoList todos={todos} />
    </div>
  );
}

export default App;
```

As usual, let's unpack:

**1.**

At the beginning of the file, we add `useEffect` from React. And shortly after that, we set a unique key for our app (this is useful because you may work over time on different apps that all use `localhost`, which means they all share the same storage space)

```js
import React, { useRef, useEffect } from "react";
...
const LSKEY = "MyTodoApp";
```

**2.**

After our state management code we add a `useEffect` function. As you can see in this case it's pretty simple, we are basically saying: "Hey, React, every time you _update_ the `App` component make a copy of our todos and save it to localStorage! That's an order, not a question".

```js
useEffect(() => {
  window.localStorage.setItem(LSKEY + ".todos", JSON.stringify(todos));
});
```

Now, when does React _update_ `App`? It can get complicated, but for now, suffice to say `App` updates when we first load the page, and every time we add a todo or mark it complete. That is: on the first render, and every time the component's state changes.

### Step 2. The Second Parameter

In this case, it may seem to be working fine. But in real applications, you rarely want to run an effect every single time the component updates. Sometimes you want to do something only when it first renders (for example if you need to call an API). Other times only when _one specific_ state changes (remember that we can have multiple `useState` in a component).

How do we achieve that? By adding a second parameter to `useEffect`. This second parameter is an `array` and it can contain 0 or more items. For example, if we want to run our effect only when `todos` change, we simply add `todos` to the array, like so:

```js
useEffect(() => {
  window.localStorage.setItem(LSKEY + ".todos", JSON.stringify(todos));
}, [todos]); // <<- look here
```

### Step 3. Your Turn

Ok, so saving todos to `localStorage` was easy (right?). How do we retrieve them, though? For you to figure out 🙃

And here are a couple of hints:

- You can use multiple `useEffect`'s in one component _wink wink_
- How do you use the second parameter to make sure you only retrieve todos once when you load the application?

### Step 6.

Done?

HURRAY 🙌 🎉 🥳

Your introduction to React is completed.

And just like yesterday, push your changes to GitHub, build and deploy the app on Netlify, and ping your coach on Discord so that he/she can check the code you wrote.

## Good Luck!
