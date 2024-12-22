---
title: "Why you can't use For Loops in JSX"
# summary: Take full control of your personal brand and privacy by migrating away from the big tech platforms!
date: 2024-07-04

# Featured image
# Place an image named `featured.jpg/png` in this page's folder and customize its options here.
image:
  caption: "post image"

authors:
  - admin

tags:
  - JSX
  - React
  - JavaScript
  - TypeScript
  - Programming
  - Web Development
---

JavaScript XML (**JSX**) is a syntax extension that allows for developers to write components and elements in a similar way to HTML inside of JavaScript, the elements are very similar to HTML and they can easily be converted to JavaScript. The example below shows how is converted to JavaScript using the **React.createElement()** function

```javascript
// JSX
const element = (
  <div className="container">
    <h1>Hello, JSX!</h1>
  </div>
);

// Converting JSX to javascript
const jsElement = React.createElement(
  "div",
  { className: "container" },
  React.createElement("h1", null, "Hello, JSX!")
);
```

**JSX** is adopted by multiple frameworks including React, Preact, Inferno, Vue and it can even be used with Angular. The widespread use is due to its flexibility and ease of use especially considering how there is little to no learning curve in transitioning from pure HTML. A **JSX** component necessarily has to be return a single element and this makes for a limitation that has to be adhered to strictly

```javascript
export default function ExampleComponent() {
  return (
    // The div serves as the required single parent element
    <div>
      <p>Element one</p>
      <p>Element two</p>
    </div>
  );
}
```

A major point of confusion in JSX for beginners is when they try to use the **_for_** loop syntax inside of the JSX body, that will always result in a Syntax error and the reason for that is because the JSX syntax always expects expressions and not statements i.e every element inside of the JSX must evaluate to and return a single value. The code snippets are examples of expressions and statements respectively

```javascript
/* EXPRESSIONS */

2 + 3; //evaluates to a number value

x + y > 5; //evaluates to a boolean value

function addNumbers(x, y) {
  return x / y; //evaluates to a value (depending on the types of x and y)
}
```

```javascript
/* STATEMENTS */

//This assigns a value to a variable but evaluates to nothing
x = 2 + 3;

//Conditional statement, it does not directly evaluate to a value
if (x > 5) {
  console.log("It's big!");
}

//This loop performs an action and does not return a value
while (y < 10) {
  y++;
}
```

Take note that expressions have one thing in common: you can be sure that they will produce a single output, such as a number, string, boolean, or even another expression. While statements mainly instruct the program to do something like calling functions, assigning values, controlling the program flow and so on, they do not directly return values (some may do so indirectly).

Another mistake that beginners make is failing to ensure that there is a single parent wrapper for the **JSX** syntax, this is important because just like said before, the JSX must evaluate to a single return value.

### Best Practices

- Instead of using for and while loops in JSX return statements, maps can be used instead. This works well because JavaScript _Array.map_ always have a return value

```javascript
/*Maps instead of loops*/
const words = ["hello", "world", "javascript"];
const uppercaseWords = words.map((word) => word.toUpperCase());
console.log(uppercaseWords); // Output: ['HELLO', 'WORLD', 'JAVASCRIPT']
```

- Statements can still be used in JSX components, but they cannot be used in the return statement. The example below shows a component that uses a for loop statement to create an list of JSX elements and then those elements are used in the return value.

```javascript
const ExampleComponent = () => {
  // Array of numbers
  const numbers = [1, 2, 3, 4, 5];

  // Using a for loop to create JSX elements
  const numberElements = [];
  for (let i = 0; i < numbers.length; i++) {
    numberElements.push(<li key={i}>{numbers[i]}</li>);
  }

  return (
    <div>
      <ul>{numberElements}</ul>
    </div>
  );
};
```

- We have established that JSX expressions return a single value but in cases where there is to be multiple elements without a single wrapper, JSX fragments can be used instead. The JSX fragment `<></>` allow us to group multiple elements together without introducing an extra parent in the DOM (Document Object Model), this can help us to achieve a cleaner solution when we want to avoid an unnecessary wrapper element.

```javascript
// Without JSX fragments (additional div wrapper)
const WithoutFragments = () => {
  return (
    <div>
      <p>Element one</p>
      <p>Element two</p>
    </div>
  );
};

// With JSX fragments
const WithFragments = () => {
  return (
    <>
      <p>Element one</p>
      <p>Element two</p>
    </>
  );
};
```

### Typescript JSX

Finally, it is also import to note that TSX (Typescript JSX) is just JSX with the added type-checking capabilities of the Typescript language. TSX is relatively verbose but type checking guarantees some safety and helps to catch potential issues early

```javascript
//JSX
import React from "react";

const JSXComponent = () => {
  const message = "Hello, JSX!";
  return <div>{message}</div>;
};

export default JSXComponent;
```

The equivalent TSX to achieve the same result code will be

```typescript
//TSX
import React from "react";

// Type definition for component props
type TSXComponentProps = {
  message: string;
};

const TSXComponent: React.FC<TSXComponentProps> = ({ message }) => {
  return <div>{message}</div>;
};

export default TSXComponent;
```
