---
title: "OlymPick: How to Build a Tool for Curating Must-Watch Olympic Events with AI"
# summary: Take full control of your personal brand and privacy by migrating away from the big tech platforms!
date: 2024-04-01

# Featured image
# Place an image named `featured.jpg/png` in this page's folder and customize its options here.
image:
  caption: "post image"

authors:
  - admin

tags:
  - AI
  - Olympics
  - FastAPI
  - Langchain
  - LLaMA
---

### Introduction

The Olympics are arguably the most-watched sporting event in history, featuring a vast array of competitions across nearly every sport. With so many games happening simultaneously, it often becomes a daunting task for a sports enthusiast like me to decide which event to watch. As a fan of multiple sports, I frequently find myself struggling to choose the best game to view at any given moment.

This challenge inspired me to develop a simple AI tool that leverages artificial intelligence to select the "best" game to watch at a specific time, simplifying the decision-making process and enhancing the viewing experience.

## Technology Stack

The focus of this project is to build a back-end using FastAPI, which employs an agent built with Langchain to retrieve the current Olympic games. The agent will then determine the best game to watch using a large language model (LLM). We will make calls to the model through the Groq API; you can obtain an API key and check the list of available models here. For this project, I have chosen the open-source [LLaMA 3.1 70B Versatile model](https://huggingface.co/meta-llama/Meta-Llama-3.1-70B).

- **FastAPI**: Back-end framework.
- **Langchain**: Framework for building agents.
- **GroqCloud**: API for accessing the LLM.
- **LLaMA 3.1 70B Versatile**: Large language model used for decision-making.

### Setting up the Environment

First, we create a project folder. We'll call ours **_Olympick_**, as that is the name of the project. I have Python 3.10 installed on my machine, but any recent Python version should work fine.

```bash
mkdir olympick # create folder

cd olympick # change directory into olympick folder
```

Next, we create and activate a virtual environment to ensure we have an isolated space to install all our dependencies without worrying about conflicts with other Python installations or dependency versions.

```bash

python3 -m venv .venv # create virtual env

source .venv/bin/activate # activate virtual env
```

Now, we install our project dependencies:

```bash
pip install -U fastapi uvicorn langchain langchain-groq langchainhub langchain-community
```

### Building the Tools

For this agent, we will have two tools. Our LLM does not have information on the current time or the ongoing Olympics since it was trained on past data.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1722121596194/2272f30d-cd29-4d4d-a9c3-8a729ecb09a6.png align="center")

One tool will retrieve the current date and time, and the other will search the internet for information on Olympic events happening in the next 24 hours.

1. Get the Current Date: This tool simply retrieves the current date. Our LLM does not have access to real-time data, so we use the `datetime` function from the `datetime` package. We use the`@tool` decorator from the Langchain community because it helps us register our function as a tool, which we can then use in our agent.

   ```python
   from datetime import datetime
   from langchain_community.tools import tool

   @tool
   def get_date_time() -> str:
       """Returns the current date and time"""

       current_date = datetime.now().strftime("%A, %B %d, %Y %I:%M:%S %p")
       return f"current date and time is {current_date}"
   ```

2. Internet Search: Since our LLM does not have access to live data, we need a tool to search the internet for information. We will use [**Tavil**](https://app.tavily.com/)**y**, an AI search engine optimized for LLMs. It offers a generous free tier with 1,000 queries. I created an account and obtained an API key, which I added to my environment variables.

   ```python
   from langchain_community.tools import tool
   from langchain_community.tools.tavily_search import TavilySearchResults

   @tool
   def internet_search(question: str) -> str:
       """
       Performs an internet search using the TavilySearchResults tool
       and returns the search results.
       """

       search_tool = TavilySearchResults()
       result = search_tool.invoke(question)
       return result
   ```

### Creating the agent

Now that we have created our tools, we need to create an agent and bind these tools to it so the agent can use them. For this, we use the`create_tool_calling_agent` function from Langchain, which creates an agent that can use specified tools. To keep things modular, we create a function that encapsulates this process, allowing us to create an agent by specifying just the LLM and tools as parameters. We also use a pre-made object from the Langchain Hub community as our prompt template when creating the agent.

```python
from langchain import hub
from langchain.agents import AgentExecutor, create_tool_calling_agent

def build_agent(llm, tools):
    prompt = hub.pull("hwchase17/openai-tools-agent")
    agent = create_tool_calling_agent(llm, tools, prompt)
    return AgentExecutor(agent=agent, tools=tools, verbose=True)
```

We then use the function to create an agent from the tools, as shown below:

```python
tools = [get_date_time, internet_search]
agent_executor = build_agent(model, tools)
```

### Building the endpoint

Now that our agent is ready, we just need to build the endpoint to access the data. It will be a POST request with no parameters, as we do not expect any input from the user.

```python
from fastapi import FastAPI
app = FastAPI()

@app.get("/")
async def get_olympick_info():
    prompt = "Determine the current time and date. Identify the most anticipated Olympic event scheduled to occur within the next 24 hours of that time, considering factors such as popularity of the sport, athlete profiles, and historical viewership data. Give the exact time it will start and provide details on how to view. Ensure your final result is accurate and concise"
    response = agent_executor.invoke({"input": prompt})
    return response["output"]
```

Prompt engineering is a crucial part of using LLMs, so it's important to create a detailed prompt for the model. The agent will refine this prompt during the iterative process of responding to the request.

### Deploying the application

We can run our application locally using Uvicorn as our ASGI server. The default URL is [http://127.0.0.1:8000](http://127.0.0.1:8000), but you can change it using the `--host` flag.

```python
uvicorn src.main --reload
```

Making a request to the endpoint gives us the result:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1722174381420/9cfa1382-0325-474c-9d5d-244e1a9c30bf.png align="center")

When we look at the server logs, you can see the steps the agent takes with the tools. First, it checks the time and then uses **Tavily** to search for upcoming games. The LLM fine-tunes the outputs to provide a final response. The key point is how the model calls the tools repeatedly, as many times as needed, to get the best results.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1722175989622/23c766aa-ded4-4ecb-90ee-dc39bfd45b40.png align="center")

### Enhancements

This application was designed to be simple, but there are several ways to improve the overall quality and user experience. Here are some to consider:

- **Integrate a Sports API Tool**: By incorporating a dedicated sports API, we can directly access real-time game data, bypassing the need to rely on general internet searches, blogs, and news websites. This will ensure more accurate and timely information about ongoing games.
- **Improve Model Quality**: Leveraging a higher-quality language model will result in more precise prompts and better overall outcomes. Superior models can generate more accurate and relevant results, enhancing the agent's performance.
- **Develop a Front-end Interface**: Building a front-end application will provide a user-friendly interface for users to interact with the agent. This will improve accessibility and usability, making the application more practical and engaging for end-users.

### Conclusion

In conclusion, OlymPick is a powerful tool designed to enhance the Olympic viewing experience by leveraging advanced technologies like FastAPI, Langchain, and the LLaMA 3.1 70B Versatile model.

By automating the process of selecting the best Olympic event to watch, it simplifies decision-making for sports enthusiasts. While the current implementation provides a solid foundation, this project exemplifies how AI can be used to create personalized and efficient solutions for real-world challenges.

You can check out and fork the source code for this project on [github](https://github.com/Theedon/Olympick). Happy Building!
