# State

## Today's Learning Objectives

Today you will learn about the single most important feature of React: `state`.

## Your Mission

### Step 1. UI as a Function of State

> \- _UI as a what of what!?_  
> \- Ok, ok. Put it on hold for a second, it will make more sense at the end of the exercise.

If you've been following the instructions, you probably have somewhere a component similar - at least in structure - to this:

```js
import React from "react";

export default function TodoList() {
  return (
    <ul>
      <li>
        <input type="checkbox" /> My first todo
      </li>
      <li>
        <input type="checkbox" /> My second todo
      </li>
    </ul>
  );
}
```

Now, this is all good, but we know that in the end we want that list of todos to be dynamic. What's even the point of using JavaScript if my list is hardcoded?

Before we can make it fully dynamic, we have to fix this component's wiring.

### Step 2. `.map`

Our todos are a list, and what is the data structure that can best represent a list?

You got it:

```js
// An array 🙌
const todos = ["My first todo", "My second todo"];
```

Let's add this array to our component. We will then use a particular array method which will become your best friend whenever you use arrays in React:

```js
import React from "react";

export default function TodoList() {
  const todos = ["My first todo", "My second todo"];
  return (
    <ul>
      {todos.map((todo) => (
        <li>
          <input type="checkbox" /> {todo}
        </li>
      ))}
    </ul>
  );
}
```

Before going to the next step, make sure you understand what the `.map` method does, and you understand why we used curly brackets where we used them.

### Step 3. State

Every component in React can manage its own state. Some components have a very simple state, for example "active: true/false". other components have complex state objects, or even multiple states.

If all this sounds theoretical and confusing, just let it sit in the back of your head, and let's add something practical to our code.

In React the way we manage state is by using the _hook_ `useState` (I'll let you read more yourself about hooks - in the following days we will see two more hooks, but there are many more). A _hook_ is kind of a special function provided by React.

```js
import React, { useState } from "react";

export default function TodoList() {
  const initialTodos = ["My first todo", "My second todo"];
  const [todos, setTodos] = useState(initialTodos);
  return (
    <ul>
      {todos.map((todo) => (
        <li>
          <input type="checkbox" /> {todo}
        </li>
      ))}
    </ul>
  );
}
```

There are 3 things to unpack here, 1 of which will be... interesting to do 😏  
So let's start from the easy 2.

**1.**  
In the first line we have added `useState` to the list of dependencies that we pull in from React:

```js
import React, { useState } from "react";
```

Nothing fancy so far.

**2.**  
In line 4 we renamed our array from `todos` to `initialTodos` (we will remove it altogether tomorrow, but we need it right now to understand `useState`):

```js
const initialTodos = ["My first todo", "My second todo"];
```

**3.**  
In line 5 the fun begins. Let's see what happens one part at a time.

```js
const [todos, setTodos] = useState(...);
```

What is that sourcery, I hear you asking? It's called `destructuring assignment`. It's simpler than it seems. It's just a shorthand for:

```js
// useState returns two values inside an array
const myState = useState(...);

// The first value is always the current state - In our case, the current list of todos
const todos = myState[0];

// The second value is always a function that let's me update the current state
const setTodos = myState[1];
```

Note that you can name those two variables whatever you want, but it's a convention to name them "thing", and "setThing".

Now for the second half of that line:

```js
... = useState(initialTodos);
```

`useState` accepts a parameter (`initialTodos` in our case) that will be the state when the app is first loaded. The parameter is _optional_, but in this stage we need to set it to our initial list of todos, or we wouldn't see anything (go ahead, try to remove it and see what happens).

We still need to add user interaction (we'll do it tomorrow), but our component is now ready for the magic of React:

_Every time a state is updated (in our case it would be by using the `setTodos` function) React updates the connected UI elements 🥳_

### Step 4. Your Turn

I've shown enough, now it's your turn.

We have two problems here:

1. If you use that code and open the console, you will see an error that says `Warning: Each child in a list should have a unique "key" prop.`
2. We still need a way of setting a todo (its checkbox at least) to done/not-done (checked/unchecked)

It's up to you to fix them 🙃

Note on the second problem: right now you can of course just click on the checkboxes you have, and it will seem to work. But if you leave it like this React will never know the checkbox is checked (the browser itself knows, but the React code does not). Remember, the UI must depend on the state. Which means that the visible state of the checkboxes must also depend on the _state_ of our component. You basically need to find a way of making the checked state of checkboxes come from the _state_ we have set up in the component (hint: maybe the shape of the todos array needs to be modified/augmented a bit).

### Step 5.

Done?

Just like yesterday, push your changes to GitHub, build and deploy the app on Netlify, and ping your coach on Discord so that he/she can check the code you wrote.

### (Step 1. Oh, wait)

> UI as a Function of State

So, what does it really mean?

It means that the UI, which is the interface that I see, _depends_ directly on some "state". This state can be an array, an object, an array of objects, etc. etc.

And not only it depends on the state, but it is also _determined_ by it. This means that if I have a certain state, I will always end up with a certain UI. If I have a different state, I will have a different UI.

In our case, the UI of our list depends on the "todos" state. The todos that are shown (the `<li>` elements printed in the HTML) are directly dependent on whatever is in the `todos` variable.

Add one item to the array, and you will now see 3 todos. Remove that item from the array, and you will go back to the previous 2 todos.

Does it make some sense? If not, let's chat about it on Discord!

## Good Luck!
