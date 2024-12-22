---
title: "Using URLs as a State Manager"
# summary: Take full control of your personal brand and privacy by migrating away from the big tech platforms!
date: 2024-04-01

# Featured image
# Place an image named `featured.jpg/png` in this page's folder and customize its options here.
image:
  caption: "post image"

authors:
  - admin

tags:
  - TypeScript
  - JavaScript
  - Programming
  - Type Safety
---

### Introduction

Typescript, which is a superset of the JavaScript programming language, is a language that has become a favourite among developers due to its special features. It is helpful to understand that TypeScript is not a replacement, it is really just JavaScript with enhancements, and that comes in the form of an explicit type system.

### JavaScript: Weakly Typed

JavaScript is a dynamic language that allows changing the types of variables, adding new methods to objects, and even changing the prototypes of objects at runtime. The advantage of this is that you can write flexible code without worrying about strict rules that can make you feel hindered.

```javascript
let firstName = "toyin"; //string assigned

firstName = 43; //number assigned

firstName = () => {
  return null;
}; // function assigned

firstName = true; //bolean assigned
```

Just like everything else, if there are advantages, then there will be disadvantages. The issue with being a flexible language is that errors are not easily caught, and this can end up being a challenge, such as having runtime errors, which mean that errors that occur between type mismatches like the example I gave above will not manifest until runtime, and developers agree that being able to deal with errors during compile time is a far greater developer experience than during runtime. Code is also considerably more difficult to understand since there are no explicit types, and that can make it a nightmare to work on a large, complex project with a huge team. This is where TypeScript shines.

### Typescript: Strongly Typed

Typescript brings with it an explicit type system that adds type annotations to define the behaviour and shape of variables and other entities within the codebase. It adds static typing to JavaScript (TypeScript has to be transpiled to plain JavaScript before execution), catches errors early, helps IDEs give better refactoring and autocompletion support, and thereby helps developers build more robust and easier-to-debug applications.

```typescript
let firstName: string = "toyin"; //string assigned

firstName = 43;
//Error: Type 'number' is not assignable to type 'string'

//Function takes in a boolean and returns a boolean or null
const func: (a: boolean) => boolean | null = (a: boolean) => {
  if (a) {
    return a;
  } else {
    return null;
  }
}; // function assigned
```

### Any vs Never vs Unknown

Apart from the popular basic types in TypeScript, such as string, boolean, number, etc. TypeScript also has other types that can be used to define variables and some of them are Any, Never and Unknown

### The Any Type

**Any** is a type that I like to refer to as "the lazy type". It is a type used when you do not want to go through the process of typing the entity and you just want any type of value to be allowed. It disables type checking and basically tells Typescript to stand down. It can be a very convenient type to use, but it becomes easy to abuse as we can end up bringing back all of those JavaScript challenges that we were trying to get rid of in the first place. It is a trade-off between flexibility and type safety and should only be used sparingly and in cases where there is no other option.

```typescript
let user: any; //type === "any"

user = 4;
//type allowed to change since user is of "any type"

user = false;
//type allowed to change since user is of "any type"
```

```typescript
let user: any = {}; //type === "any"

user.getEmail();
/* Typescript throws no error but results in runtime error
 *  since the getEmail() method is not present
 */

console.log(user.lastName);
```

### The Unknown Type

The **unknown** type is a type used to represent values that are not known at compile-time and can only be decided at runtime. The **unknown** type is a complete set of all of the possible typescript types, which means that it allows any possible values, but it also provides type safety by ensuring that type narrowing or type assertions must be performed before operations are performed on values.

```typescript
//Type assertions
let userName: unknown;
let firstName: string;

userName = "toyin";

firstName = userName;
console.log(firstName.toUpperCase()); //Error: Type 'unknown' is not assignable to type 'string'

firstName = userName as string;
console.log(firstName.toUpperCase()); //Output: TOYIN
```

```typescript
//Type guards
function getUserAge(user: unknown) {
  if (
    typeof user === "object" &&
    user &&
    "age" in user &&
    typeof user.age === "number"
  ) {
    return user.age;
  }
  return 0;
}

getUserAge({ age: 10 }); //returns 10

getUserAge({ age: true }); //returns 0

/* The unknown type ensures that guards are used before
 *  any property can be accessed thereby ensuring type safety
 */
```

### The Never Type

The **never** type, which is usually more challenging to understand compared to the previous two, is just a type that specifies that a value will never occur. Functions that never return a value and variables that are never assigned a value are of the **never** type. It is a relatively less common type but is used in situations where a function should never be completed (such as infinite loops) or when something should never happen.

A function that has a **never** return type never returns a value and this usually occurs in case of thrown errors and infinite loops

```typescript
//function throws error and never returns a value
function errorThrower(errorMessage: string): never {
  throw new Error(errorMessage);
}

errorThrower("this is an error");

//function runs forever and returns never
const infiniteLoop: never = () => {
  while (true) {
    console.log("infinite loop");
  }
};
```

Also, if a piece of code will never be reached, then it is of the **never** type.

```typescript
function checkType(value: string | boolean) {
  if (typeof value === "string") {
    console.log(value);
  } else if (typeof value === "boolean") {
    console.log(!value);
  } else {
    const varUnreachable: never = value;
    //varUnreachable is of type never since it will never be reached
  }
}

checkType("toyin");
checkType("true");
checkType(4); //Argument of type '4' is not assignable to parameter of type 'string | boolean'
```

### Conclusion

**Any** provides maximum flexibility by allowing values of any type. **Unknown** is a safer alternative by requiring type narrowing and guards. **Never** helps in denoting values that should never occur. While each type has its place and use cases, it's crucial for developers to strike a balance between flexibility and type safety.
