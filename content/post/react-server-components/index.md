---
title: "Why React Server Components Embrace Async, But Client Components Don't"
# summary: Take full control of your personal brand and privacy by migrating away from the big tech platforms!
date: 2024-04-01

# Featured image
# Place an image named `featured.jpg/png` in this page's folder and customize its options here.
image:
  caption: "post image"

authors:
  - admin

tags:
  - programming
---

### What are Components

Components are the fundamental building blocks in any React project and, by extension, any framework built on top of the React library. They can be thought of as how LEGO bricks make up a LEGO set. The developer has all of the control, and you can combine and arrange your bricks (components) in any way that you want. A single project is really just a combination of well-defined components.

### Synchronous Components

In programming, synchronous and asynchronous are ways to define the flow of execution of a program. Synchronous code executes line by line; this means that there is an order of operation, such as the second operation waiting for the first operation to be completed before it runs, and so on. A synchronous component also follows the same logic; all of the code inside of it is left to be completed sequentially, which means that the program flow is blocked or halted until the current operation is finished. The following code example shows a component with a function to add two numbers; the function is then called to be stored in the result variable. Only when the computation of that function is completed can the program move on and the value be stored in the results variable.

```typescript
// Synchronous component
export default function SynchronousComponent() {
  // Synchronous function
  const addNumbers = (a: number, b: number) => {
    return a + b;
  };

  // Using the synchronous function in the component
  const result = addNumbers(5, 7);

  return (
    <>
      <p className="pOne">
        The result of adding 5 and 7 synchronously is: {result}
      </p>
    </>
  );
}
```

### Asynchronous Components

Asynchronous components do not follow the same blocking pattern. Asynchronous code allows pending operations to be put to the side while other subsequent operations continue their operations. A common example is how a fetch call can be made to retrieve data from an API, and while the data is being expected, other operations can continue and the application can continue its rendering. Once that data is successfully returned from the API, any part of the code that is required can continue with its operations or renders. Callbacks used to be the traditional way of performing asynchronous operations in Javascript, but promises and the async/await syntax were more recently introduced, and they make writing asynchronous code more readable and maintainable. The code below shows a server component marked as asynchronous that fetches data from an API and displays the result.

```typescript
export default async function MyServerComponent() {
  const response = await fetch("https://api.example.com/data");
  const data = await response.json();

  return (
    <div>
      <p>{data}</p>
    </div>
  );
}
```

It is also helpful to understand that the rendering of JSX in a React component is always synchronous regardless of the component being synchronous or asynchronous, this is because the virtual DOM (Document Object Model) that React uses operates synchronously, this ensures that the state of the application is reflected by the UI (User Interface) in a predictable order.

### Why Client Components cannot be Asynchronous

Client components run on the client side (on the browser) as opposed to server components that run completely on the server. Some of the reasons they cannot be marked as asynchronous are:

1. **Execution Threads:** Servers have the capability of multiple threads, which means that there are multiple threads that operations can be performed on, but there is just a single (main) thread on the browser. Using asynchronous operations directly on client components would block the main thread, leading to an unresponsive UI and thereby hindering user interaction.
2. **Hydration Issues:** When a request for a React page is made from a browser, initial server-rendered content is sent to the client, and that is expected to be hydrated with the client-side data. That initial server-rendered HTML is expected to match the client-side state for hydration to occur. However, asynchronous client components would mean that some of the data is not available, thereby creating empty spaces or outdated information. These make hydration a challenge and can lead to errors.
3. **Data Consistency and User Experience:** It can become a serious challenge to manage the user experience if there are multiple asynchronous operations in a client component. If data from different async operations returns in an order that the programmer does not (which is usually the case because the response time of data fetches is unpredictable), then the user might get unexpected changes or errors. It can also be challenging to handle loading states with asynchronous client components, thereby making it confusing for the user to know which section is loading and which is not.

While we currently cannot have asynchronous client components, there are alternatives to handling asynchronous operations.

- **useEffect:** This is a React hook that enables you to perform side effects in React functional components. The side effect means that you can perform operations and actions outside the render function of a component. This allows you to perform operations (including asynchronous ones) like fetching data from an API without affecting the initial rendering of the component. useEffect runs immediately after the render completes and before the next render occurs. This ensures that we can avoid blocking the render thread in the browser and improve the performance of the application. An optional dependency array can also be passed to it in order to specify when to re-run the effects.
- ```typescript
  "use client";
  import { useState, useEffect } from "react";

  export default function AsyncComponent() {
    const [message, setMessage] = useState<string>("Loading...");

    useEffect(() => {
      // Simulating an asynchronous operation with a delay
      const fetchDataAsync = async () => {
        try {
          // Simulating a delay of 5 seconds
          await new Promise((resolve) => setTimeout(resolve, 5000));

          // After the delay, update the state with a message
          setMessage("Async data fetched!");
        } catch (error) {
          console.error("Error fetching data:", error);
          setMessage("Error fetching data");
        }
      };

      fetchDataAsync();
    }, []); // Empty dependency array ensures the effect runs only once on component mount

    return (
      <div>
        <p>This is the first div</p>
        <div>
          <p>{message}</p>
        </div>
        <div>
          <p>This is the third div</p>
        </div>
      </div>
    );
  }
  ```
- **Server Components:** The ability to use server components asynchronously means that they should usually be used to perform such operations that require asynchronous capabilities whenever possible. An example is fetching data from an API inside an asynchronous server component and then passing that data as a prop to a client component that is nested inside the server component. This also reduces the load on the client.

```typescript
//Server Component
import { Suspense } from "react";

export default async function ServerComponent() {
  const response = await fetch("https://api.example.com/data");
  const data = await response.json();

  return (
    //Suspense boundary for loading state
    <Suspense fallback={<div>Loading...</div>}>
      <ClientComponent data={data} />
    </Suspense>
  );
}
```

```typescript
//Client Component
"use client";
export default function ClientComponent({ data }) {
  return (
    <div>
      <pre>Data: {data}</pre>
    </div>
  );
}
```

It is also helpful to understand that the rendering of JSX in a React component is always synchronous regardless of the component being synchronous or asynchronous, this is because the virtual DOM that react uses operates synchronously, this ensures that the state of the application is reflected by the UI in a predictable order.

### Conclusion

Client components cannot be marked as asynchronous, but they certainly have their own use cases as compared to server components. Server components in React are best for data fetching and aggregation, SEO, authentication, and authorization, while client components are suitable for interactivity and user responsiveness, dynamic UI updates, and integration with browser APIs. It is important to use them according to their strengths and hand in hand as a part of the server architecture in order to build performant and optimal web applications.
