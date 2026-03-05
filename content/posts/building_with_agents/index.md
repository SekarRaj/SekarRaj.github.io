---
title: "Building Agents with Ollama"
date: 2026-03-05
draft: false
categories: ["ollama", "agents"]
tags: ["ollama", "agents","ai"]
---

# Building Local AI Agents with Ollama and Google ADK

As I started building agentic applications, I immediately stumbled upon two major obstacles. First—the obvious one—I kept running low on API funds and credits at an alarming rate. Second, I didn't have a fast way to prototype and test applications locally.

Fortunately, these hurdles led me to explore **Ollama**. For those of us who have used Docker in the past, Ollama feels instantly familiar. It uses similar CLI patterns to get models up and running in seconds.

## Why Ollama?

In "Docker parlance," the Ollama CLI is the application you install locally, while [ollama.com/library](https://ollama.com/library) is equivalent to Docker Hub. For the uninitiated: you use the CLI to run commands in your terminal, and the website hosts open-source models available for download.

To explore a model, you pull it to your machine, run it, and interact with it locally. Once a model is downloaded, it works entirely offline. The quality of the output depends on the specific model, its quantization level, and its training parameters.

Visit [ollama.com](https://ollama.com) to install the framework and fire it up on your machine.

### Essential Commands

The most frequent commands I use are:

* `ollama serve`: Starts the local server.
* `ollama pull`: Downloads a model from the library.
* `ollama run`: Pulls (if needed) and starts a model in one step.
* `ollama show`: Displays information about a specific model.
* `ollama create`: Used to tune models with custom system prompts or additional parameters.

---

## Agentic Development Kit (ADK)

Google's **ADK** is a framework designed for developing intelligent agents. While I also enjoy using LangGraph, I chose ADK for this example because of its structured approach to agent orchestration.

Today, we’re going to create a simple agent capable of tool-calling, with the underlying model served locally by Ollama.

### 1. Bringing Up the Model

While Ollama adds a tray icon upon installation for ease of access, I prefer starting it via the CLI to monitor logs. Use the `ollama serve` command:

<img src="assets/ollama_serve.png" alt="Ollama Serve" width="1200">

As you can see, the terminal is taken over by logs—we can ignore these for now. In a separate terminal, let's download the **Google Gemma 3** model.

`ollama pull gemma3`

<img src="assets/ollama_pull.png" alt="Ollama Pull" width="1000">

You can verify the details with `ollama show gemma3`:

<img src="assets/ollama_show.png" alt="Ollama Show" width="500">

Finally, you can test the model with `ollama run gemma3`. (Type `/bye` to exit the chat session).

### 2. Creating the Agent

We’ll use Python with the **uv** package manager for a fast, clean setup.

```bash
uv init matrix
cd matrix

```

The generated `pyproject.toml` file lists your dependencies. Add the ADK dependency and initialize the agent:

```bash
uv add google-adk
source .venv/bin/activate
adk create neo

```

<img src="assets/adk_create.png" alt="ADK Create" width="500">

Since ADK needs a way to talk to Ollama's API, we will use **LiteLLM**, which provides an OpenAI-compliant wrapper.

```bash
uv add litellm

```

### 3. Configuring the Agent

Open `neo/agent.py`. You'll see the default boilerplate. Replace it with the following code to point the agent to your local Ollama instance:

```python
from google.adk.agents.llm_agent import Agent
from google.adk.models.lite_llm import LiteLlm

root_agent = Agent(
    # Use the ollama_chat prefix for LiteLLM
    model=LiteLlm(model="ollama_chat/gemma3:latest"),
    name='root_agent',
    description='A helpful assistant for user questions.',
    instruction='Answer user questions to the best of your knowledge',
)

```

### 4. Running "Neo"

Now, let's fire up our agent:

```bash
adk run neo 

```

**Output:**

```text
Running agent root_agent, type exit to exit.
[user]: Is the matrix real? Give a short answer 

13:48:32 - LiteLLM:INFO: utils.py - LiteLLM completion() model= gemma3; provider = ollama_chat
[root_agent]: As a thought experiment, yes—the Matrix is real in the sense that a simulated reality is possible. However, in the literal, physical sense, it remains a fictional construct. 

```

## Conclusion

We’ve successfully created a powerful agent running entirely on your local machine. This setup gives you full control over your data and costs $0 in API credits.

In subsequent blogs, we’ll explore how to extend ADK with custom tools and multi-agent workflows.

---

### References

* [1] [Ollama Official Website](https://ollama.com)
* [2] [Google ADK Documentation](https://www.google.com/search?q=https://github.com/google/adk)
* [3] [LiteLLM Supported Providers](https://docs.litellm.ai/docs/providers/ollama)