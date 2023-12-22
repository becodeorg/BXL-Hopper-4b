# Day 0: The Basics

## Today's Learning Objectives

Today's goal is to familiarize yourself with a few basic React concepts: _components_, and _JSX_.

We will also go over some JavaScript features that you will use over, and over, and over again in your React applications.

### What is a "component"?

You will hear this word a lot. _Components_ are the building blocks of every React application. Let's start with an analogy: _If we were writing HTML, components would be the blocks that build the page_.

For example, `<h1>Hello World</h1>` can be thought of as a component.

Components can contain other components! How many components can you count in the following block of HTML?

```html
<div class="header">
  <h1>Hello World</h1>
  <div class="menu">
    <div class="menu-item">About</div>
    <div class="menu-item">Portfolio</div>
    <div class="menu-item">Contacts</div>
  </div>
</div>
```

`<h1>Hello World</h1>` is a component. And so is every `<div class="menu-item">...</div>`. And components can contain other components, which means that `<div class="menu">...</div>` is a component, and `<div class="header">...</div>` as well.

In React things are a little different, but the analogy with HTML can help you wrap your heads around components.

From a technical point of view, a React component is a function that in the end returns some JSX (more about it later). For example:

```js
const Title = () => <h1>Hello World</h1>;
```

Or in its longer form:

```js
const Title = function () {
  return <h1>Hello World</h1>;
};
```

But, wait a second. Is that HTML? Is that JavaScript? Can I just write HTML in my JavaScript file and you never told me??

Well... yes and no. Keep reading!

### What is "JSX"?

JSX is a syntax that mixes HTML in JavaScript code. It's both, and it's neither. JSX allows you to write HTML-like stuff directly in your JavaScript files, but JSX is _not_ official JavaScript, and browsers don't know how to deal with it.

(Not to worry: our build system will transform JSX into regular browser-friendly JavaScript for us)

This is a simple example of JSX (only the part that looks like HTML is a _JSX expression_, the rest is regular JavaScript):

```js
const title = <h1>Hello World</h1>;
//            ⬆️ JSX expressions  ⬆️
```

This is _not_ JSX, it's just a string that represents some HTML:

```js
const title = "<h1>Hello World</h1>";
//            ⬆️ just a string      ⬆️
```

Let's look at the first example now, the one with the JSX expression. Something peculiar happens: «Hello World» is in fact a `string`, even if it doesn't immediately look like one. If you want to use JavaScript (for example a variable, or a ternary operator) inside a _JSX expression_ you need to use `{}`.

A simple example:

```js
const name = "Peter";
const title1 = <h1>Hello, {name}</h1>; // ✅ prints "Hello, Peter"
const title2 = <h1>Hello, name</h1>; // 🙅‍♂️ prints "Hello, name"
```

A more complex example:

```js
const name = "Peter";
const morning = false;
const title = (
  <h1>
    {morning ? "Hello" : "Good evening"}, {name}
  </h1>
);
// prints "Good evening, Peter"
```

### Components and JSX, components vs. JSX

I bet you missed a detail 🙂  
Let's see once again two basic examples.

A _component_:

```js
const Title = () => <h1>Hello World</h1>;
```

Just _JSX_:

```js
const title = <h1>Hello World</h1>;
```

How are they different? Pay close attention: there are _two things_ that make a component a component:

1. The variable name starts with a capital letter (`Title`, and not `title`)
2. A component is a function that returns JSX

`const title = () => <h1>Hello World</h1>` is _not_ a component, the variable name starts with a lower case letter.  
`const Title = <h1>Hello World</h1>` is _not_ a component, it is not a function.

Without these two characteristics, your code may be valid JavaScript, but will not work as a React component.

### Components inside components inside components...

There is a very practical reason why components have to start with a capital letter: a JSX expression can not only contain HTML, but also other React components! React needs to know which JSX is an HTML element, and which JSX is a custom component. It does so by looking at the first letter. Is it a capital letter? Then it's a custom component. Otherwise it's regular HTML.

Look at this example mixing components and HTML:

```js
const Title = () => {
  const name = "Peter";
  return <h1>Hello, {name}</h1>;
};

const Menu = () => {
  return (
    <div className="menu">
      <div className="menu-item">About</div>
      <div className="menu-item">Portfolio</div>
      <div className="menu-item">Contacts</div>
    </div>
  );
};

const Header = () => {
  return (
    <div className="header">
      <Title />
      <Menu />
    </div>
  );
};
```

_Oh, by the way, have I mentioned a small but important difference between JSX and HTML? In JSX we have to use `className` instead of `class`. At least for the time being, let's hope they fix this._

And this is how you compose components in a React application ✌️

Note that you don't _have_ to make every single HTML element of your app a component. It's perfectly fine to have one React component return multiple levels of HTML elements, like the `Menu` component above. Sometimes it's overkill to subdivide every single little piece of HTML into its own component.

### Some JavaScript, back to basics

I will not go into details here, but make sure you have a good understanding of the following concepts (you can go back to the relevant exercises that you completed a couple of months ago, or look for references online).

In this list each concept is linked to the relevant MDN article.

- [Arrow functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)
- [Template literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals)
- [Array methods (and especially `.map`)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map)

For a more complete list (we won't necessarily use everything in that list, but it can come in handy in the future) see [this article](https://kentcdodds.com/blog/javascript-to-know-for-react).

## Alright!

Enough for the introduction, let go write some code.  
Go to the [Getting Started](../1.Getting-Started) chapter to start up your first React application.
