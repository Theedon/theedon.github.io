---
title: "Beyond API Routes: Exploring the Potential of Server Actions in Next.js"
# summary: Take full control of your personal brand and privacy by migrating away from the big tech platforms!
date: 2024-04-01

# Featured image
# Place an image named `featured.jpg/png` in this page's folder and customize its options here.
image:
  caption: "post image"

authors:
  - admin

tags:
  - Next.JS
  - Actions
  - APIs
  - RPC
---

Some of the most common things to build in a web application are forms, and it is common practice to create API endpoints in order to be able to perform the data mutations after submission. The invention of Server Actions means we do not have to go through that extra step, but we can instead call a function that can handle it. Excited? Let me walk you through it. Server Actions are simply asynchronous functions in Next.js (a framework built on the React library) that let us perform some functions on the server directly from within our code without having to create API endpoints. It helps to be familiar with Remote Procedure Calls (RPC) when trying to understand Actions as they basically have the same concepts. RPCs are protocols that allow you to make a call on one machine, that call seems like a local call, but it is in fact executed on another machine on that network. This makes it easy to achieve seamless interaction between multiple components of a system that might be shared across multiple nodes, like in the case of distributed computing.

### Understanding Server and Client Components

Server components give you the ability to define components that do not get rendered on the client's device but instead completely render on the server. This comes with some caveats, such as the component not having any knowledge or information of the client-side properties such as the Browser document object or even hooks and effects that can be used to manipulate the event life cycle. We have established that server components are the next best thing since slice bread, you can still agree with me that there are uses for client components. The ability to make fetches directly from the server, as we could with server components, is a feature we lose with client components. Instead, we must now design API endpoints that we can hit to make CRUD requests, just as we did before server components were introduced (the Next.js Route handlers feature is a sweet way to achieve this).

### Benefits of Server Actions

Here come server actions. With this feature, you can make an RPC-like function (referred to as an action) that allows you to make a backend call (fetch data from your database) as if you were running it on the server. So basically, you have a client component hydrates (loads) on the browser, but can still make direct fetches from the server via server actions. How cool is that?. The way this works is by the client sending the request to the server securely, and then that call is made from the server before the response is now streamed back to the client. Server actions, in my opinion, have the most use cases in client components, but they can also be used in server components. We already know that server components run on the server, so they can make direct calls in the backend, but making post requests and mutations to the database is a completely different game. It is possible to do so from the server components, but there is some risk of cross-request site forgery (CRSF) which means attackers can manipulate form logic to gain unauthorized access. Server actions help to reduce the risk by properly encapsulating form logic and, most importantly, ensuring that the action always runs only on the server. A plus is that actions can be used in conjunction with server components in such a way that forms submitted by clients that do not have JavaScript enabled or that have bad Internet connections. This is possible because all of the JavaScript actually runs on the backend. The opportunities are limitless in that they give client components the ability to mimic server components and reduce some of the hassle that comes with having to create API endpoints.

### Implementation and Best Practices

Server actions are also really easy and straightforward to create compared to API routes. There are generally two methods to go about them.

1. **Inline-level**: You can create an async function in your server components directly and mark it with a "use server" directive.

```typescript
export default async function ServerComponent() {
  async function inlineAction() {
    "use server";

    //inline-level action

    return <>...</>;
  }
}
```

1. Module-level: you can also choose to create a module that houses the entire server action; this file must have "use server" as the very first line. Every export in this file is tagged to be a server action.

```typescript
"use server";
async function moduleAction() {
  //module-level action
}

export default moduleAction;
```

I personally prefer to have a directory in my src folder called actions, and inside of that, I keep my actions. The guidelines for creating them are straightforward: write your function keeping in mind that it will run on the server (database fetches, form submissions, data mutations), and then add a "use server" directive to the very beginning of the file. Server actions can be passed as props to client components, to the action prop of the form component, and even to any child of the form component; they can also be used in hooks and event handlers (useEffect, onClick). Technically, there is no limit to the number of actions that can be called on a page, but since every call consumes resources on the server and causes network overhead, they should be optimized and prioritized to ensure best practices.

```ts
function FormPage() {
  async function createUser(formData: FormData) {
    "use server";

    const rawFormData = {
      id: formData.get("userId"),
      firstName: formData.get("firstName"),
      email: formData.get("email"),
    };

    // mutate data
    // revalidate cache
  }

  return <form action={createUser}>{/*form UI*/}</form>;
}
export default FormPage;
```

The above piece of code is a server component that has an inline-level action defined which takes the form inputs as an argument and then performs some mutations with them. The action is passed as a prop to the form and javascript does not need to be enabled on the client.

### Limitations

While server actions provide a lot of advantages, there are still cases where good old APIs for client components are still the best way to go, such as in cases of:

- **Limited data transfer:** API Routes are can typically handle more data transfer as compared to server actions.
- There is intricate logic in the handling of data
- **Cache Management:** Actions allow basic cache management through revalidation strategies, but they don't offer the same level of granular control as API routes.
- **CORS Configuration:** Unlike API routes, Server Actions do not automatically configure Cross-Origin Resource Sharing (CORS) policies. However, configuration of CORS policies are usually a straightforward process and this should not present much of an obstacle.

Despite the limitations, actions are a very powerful feature and can be a real boost to a developer's productivity when used at the right time. They are also currently stable as of Nextjs 14, so feel free to use them in your projects. You can learn more about Server Actions and other ways to use them from the official [Nextjs documentation](https://nextjs.org/docs/app/building-your-application/data-fetching/server-actions-and-mutations).
