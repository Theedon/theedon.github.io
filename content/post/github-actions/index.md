---
title: "Effortless Efficiency: Automating Workflows with GitHub Actions"
# summary: Take full control of your personal brand and privacy by migrating away from the big tech platforms!
date: 2024-04-01

# Featured image
# Place an image named `featured.jpg/png` in this page's folder and customize its options here.
image:
  caption: "post image"

authors:
  - admin

tags:
  - GitHub Actions
  - Automation
  - Continuous Integration
  - Continuous Deployment
---

Every software project either small, medium sized or large need to have some development workflows. Those workflows are the steps that a developer takes to plan, test, build and deploy software. The advantages of workflows really come into light when developers work in teams, they can be thought of as a framework that promotes efficiency and boosts collaboration in a project.

A very popular phrase that I like to use in explaining this is "too many cooks spoil the broth", it is usually the case where two or more people are working on a project but they have different ideas of doing things, it could be something as little as the folder structure in the project. such as choosing to have a dedicated folder for CSS files or whether to use a src (source) folder or not. It could even be something like choosing to end statements with semi-colons which is optional but acceptable in languages like JavaScript and python.

The below code snippets shows a sample of code written without any code formatter and the same formatted with a code formatter (prettier)

```typescript
//Typescript Jsx code written without code formatting
import React, { useState } from "react";
export default function MyComponent() {
  const [count, setCount] = useState(0);
  const handleClick = () => {
    setCount(count + 1);
  };
  return <div>Hello World</div>;
}
```

```typescript
//Code formatted with Prettier
import { useState } from "react";
export default function MyComponent() {
  const [count, setCount] = useState(0);
  const handleClick = () => {
    setCount(count + 1);
  };
  return <div>Hello World</div>;
}
```

Git is a powerful VCS (Version Control System) and the most popular hub for hosting git projects remotely is GitHub. GitHub offers multiple features but the particular one of interest is GitHub Actions. GitHub actions are simply automated workflows that you can setup in your repository. Think of it as having a fully independent computer system that performs operations of your choosing wherever you want. You can trigger operations to be run when events occur such as run some tests on a branch immediately someone makes a pull request from it. You can also decide to schedule jobs to happen i.e deploy the new changes in the project every midnight by 12:30am.

### Defining a Workflow

To define a workflow, all you need to do is to create a `.github` folder, then create a `workflows` folder inside of it and then you can have yaml files inside of the workflows folder.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1705491738653/2aef2fb6-9797-4530-a036-8122daaf71aa.png align="center")

Each file carries a specification of a workflow to be run and you decide the events that trigger it and the jobs that it runs. GitHub spins up an isolated container (which are faster than Virtual Machines), assigns it to your workflow and runs your jobs.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1705487306980/ef2fdc83-7700-4b73-89e4-9e922d3b074f.png align="center")

### **Common Use Cases**

GitHub Actions are useful for so many purposes but some of the ones that I often use them for are

- Code formatting: I usually have a workflow for using prettier to format certain files in the project so that a consistent code format can be maintained across the project and for all developers. This workflow only runs on a successful merged pull request to the main branch, this helps to ensure that the author can retain his own preferred code format up until when it is merged in.
  ```yaml
  name: Format code with Prettier
  on:
    push:
      branches: [main] # Trigger on pushes to the main branch (adjust as needed)
    pull_request:
      branches: [main] # Optional: Run on pull requests to main
  jobs:
    format:
      runs-on: ubuntu-latest
      steps:
        - uses: actions/checkout@v3
        - uses: actions/setup-node@v3
          with:
            node-version: 16 # Adjust as needed
        - run: npm install # Or yarn install
        - run: npx prettier --write . # Format all files in the repository
        - if: failure() # Only run this step if formatting failed
          run: git diff --exit-code # Check for uncommitted changes
  ```
- Linting: Running lints can be extremely helpful and there are lots of linters that are often used with programming languages to ensure code quality and prevent common errors. An example is how I setup a workflow to run ESLint on my Typescript projects on push to any branch in GitHub. I also have a setting on that ensures that a pull request cannot be merged unless the status check from that linting workflow is passed.
- Deployment: Workflows can also be setup for continuous development which automates the process and makes it very easy. An example is how my team uses a workflow to deploy our Next.js project to AWS each time a pull request is merged in successfully.

### Pre-Built Actions

There are also pre-built actions that are available on the GitHub marketplace, examples are:

- Slack Notify Action: This action helps you send notifications to your slack channel based on your workflow results.
- Setup Node.js Action: This pre-built action helps you setup your desired version of nodejs in your container environment.
- The GitHub pages action helps you deploy your project to GitHub pages with a pre-built action.

Actions are really a supercharged approached to development workflows and while there are many more features that were not covered here, there are lots of resources that i think you might find helpful in learning more about the topic.

GitHub offers a [certification course](https://learn.microsoft.com/en-us/users/githubtraining/collections/n5p4a5z7keznp5) on GitHub Actions that teaches all you need to know about actions.[  
](https://learn.microsoft.com/en-us/users/githubtraining/collections/n5p4a5z7keznp5)You can find a [list of pre-built](https://github.com/sdras/awesome-actions) actions that you can use in your workflows here.[  
](https://learn.microsoft.com/en-us/users/githubtraining/collections/n5p4a5z7keznp5)You can also check out the [GitHub documentations](https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows) for the multiple events that you can use to trigger your workflows.

### Caveats and Limitations

Every tool has its limitations, and GitHub Actions is no exception. While it offers many advantages, there are a few potential challenges to be aware of such as:

- Resource constraints: GitHub has to provision resources for multiple users, so you may occasionally encounter limitations in terms of available processing power or storage. However, this is rare. If you do face such constraints, you can explore using self-hosted runners, which allow you to leverage local machines or cloud services that you already pay for.
- Operating system support: GitHub Actions may not support all operating systems. To address this, you can use self-hosted runners, giving you complete control over the operating environment.  
   Despite these potential challenges, GitHub Actions is a powerful tool that continues to improve. In my opinion, it's worth exploring for any developer seeking to automate their workflows.
