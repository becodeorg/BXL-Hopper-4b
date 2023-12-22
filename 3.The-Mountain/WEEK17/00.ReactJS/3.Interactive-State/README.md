# Making it Interactive

## Today's Learning Objectives

We have our component set up with _state_, and now we will make it interactive.

## Your Mission

### Step 1. Components Structure

You should have at least 3 components in your code (if you have more, that's also good!) with the following structure:

```
- App
  └ Form
  └ TodoList
```

`Form` and `TodoList` are "siblings", and they are both contained in the parent component `App`.

It's important to have one `Form` component that contains both the input field and the button because they have to interact with each other.

If that is not your case, you need to adapt your code to this structure (again, it's not a problem if you just have _extra_ components).

### Step 2. The Form

Dealing with forms in React is pleasure and pain. There are two ways of dealing with forms: with _controlled_ components, and with _uncontrolled_ components.

The first technique gives you greater control, but it can visibly slow your application down if you have _a lot_ of fields, and/or _a lot_ of nested components.

The second technique is as fast as a browser can be and it's usually fine for a large part of use cases.

We are going to use _uncontrolled_ components because we like simplicity, but you can read more about the two techniques in the official documentation, or in other material you find online.

Your `Form` component should look something like this:

```js
import React from "react";

export default function Form() {
  return (
    <div>
      <input type="text" placeholder="Write a new todo" />
      <br />
      <button>Add todo</button>
    </div>
  );
}
```

### Step 3. Click

When we click the button, we want a todo added to the list. So let's do the first thing: let's add a click handler function (empty for the moment), and an `onClick` property to the button.

_Side note: they are called "attributes" in HTML, but "properties" (most often abbreviated in "prop") in React. The reason will be clear later._

```js
import React from "react";

export default function Form() {
  function clickHandler() {
    // Something...
  }

  return (
    <div>
      <input type="text" placeholder="Write a new todo" />
      <br />
      <button onClick={clickHandler}>Add todo</button>
    </div>
  );
}
```

Now, we know that we have to, somehow, get the value of the input inside the `clickHandler` function. You might be tempted to add a _class_ or an _ID_ to the input field and retrieve it's value with `document.getElementById()`, `document.querySelector()` or something like that...

NEVER DO THAT IN REACT.

If you do that you break out of the React update cycle, really bad things will happen to your application, and you will have bad luck for 7 years.

_Note: I shouldn't say "never", there are in fact extreme circumstances in which you have no alternative, but I encountered such circumstances at most 3 times in 6 years of using React._

The "React" way of referring to a DOM element is by using a thing called "ref" (short for _reference_). And, of course, there is a hook for that.

### Step 4. `useRef`

Let's update our component with `userRef`:

```js
import React, { useRef } from "react";

export default function Form() {
  const inputRef = useRef();

  function clickHandler() {
    const inputElement = inputRef.current;

    // Do something with inputElement...
    console.log(inputElement.value);
  }

  return (
    <div>
      <input ref={inputRef} type="text" placeholder="Write a new todo" />
      <br />
      <button onClick={clickHandler}>Add todo</button>
    </div>
  );
}
```

And now let's unpack this!

**1.**

At the beginning of the file, we pull in `useRef` from React.

```js
import React, { useRef } from "react";
```

**2.**

In line 4 we call the hook and assign it to a variable:

```js
const inputRef = useRef();
```

**3.**

Let's jump ahead to our `<input>` element. Here we assign the _ref_ we created to the `ref` property (property, not attribute 😉) of the element. It will not work if you use just `inputRef` here.

```js
<input ref={inputRef} type="text" placeholder="Write a new todo" />
```

**4.**

Finally, in the `clickHandler` function we see an example of how to use it. It's important to note that the current value of a _ref_ is accessed from its `current` property. In our example, `inputRef.current` contains our DOM element.

```js
const inputElement = inputRef.current;
```

_Note: `useRef` is useful for more than referring to DOM elements. It’s handy for keeping any mutable (fancy word? look it up!) value around across React re-renders. Feel free to read more about it in the official documentation._

### Step 5. Lifting State Up (Your Turn)

So now we have a click handler that can access whatever we write in the input field. And in our `TodoList` component – remember? – we have a state as an array with our todos.

How do we make the two components talk to each other?

The React way is called "lifting state up".

Instead of connecting the two components directly, we have to take the state in `TodoList` and the handler in `Form` and move them to the parent component `App`.

`App` will take the input value from the form and use it to update the state that is now under its control.

How to do that? Your job to find out (the YouTube video I suggested on the first day is very, very relevant 😉). All I can say is that it will involve `App` passing variables and functions down to its children `TodoList` and `Form`. The way you pass variables down is by using "props". Do _not_ move HTML elements around. They are fine where they are.

#### IMPORTANT: Never modify the state directly!

What does it mean?

Imagine we have to deal with fruits in a basket. At the beginning our basket only has apples and pears, and we want to add bananas. You may be tempted to do something like this:

```js
// at the beginning of our component:
const [fruits, setFruits] = useState(["apples", "pears"]);

// later:
fruits.push("bananas"); // 🙅‍♂️  we add the item directly to the state array
setFruits(fruits); // 🙅‍♂️  we use the state array
```

This will cause all kind of really weird and impossible to debug problems, for reasons that are too complex to treat here (but feel free to read: https://reactjs.org/docs/state-and-lifecycle.html#using-state-correctly).

See, up there we modified the `fruits` array directly by pushing a new item (`'bananas'`), and then we passed that same array to the update function. As said, this will break your app in weird ways.

What then? We have to "clone" the state, before we modify and update it:

```js
// at the beginning of our component:
const [fruits, setFruits] = useState(["apples", "pears"]);

// later:
const newFruits = [...fruits]; // ✅  we "shallow clone" the array
newFruits.push("bananas"); // ✅  we add the item to the cloned array
setFruits(newFruits); // ✅  we use the cloned array
```

And one more thing: because of how JavaScript treats objects, a safer way to clone objects and arrays (which, remember, are just special objects) is to "deep clone" them. For example doing so:

```js
const newFruits = JSON.parse(JSON.stringify(fruits)); // ✅  we "deep clone" the array
```

What is the difference between "shallow" and "deep" clone? Up to you to study the subject :)

### Step 6.

Done?

Just like yesterday, push your changes to GitHub, build and deploy the app on Netlify, and ping your coach on Discord so that he/she can check the code you wrote.

## Good Luck!
