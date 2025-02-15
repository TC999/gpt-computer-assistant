<img src="https://github.com/user-attachments/assets/10a3a9ca-1f39-410c-ac48-a7365de589d9" >
<br>
<br>
<a name="readme-top"></a>

<div align="center">


</div>


  <p>
    <a href="https://discord.gg/dNKGm4dfnR">
    <img src="https://img.shields.io/badge/Discord-Join-7289DA?logo=discord&logoColor=white">
    </a>
    <a href="https://twitter.com/upsonicai">
    <img src="https://img.shields.io/twitter/follow/upsonicai?style=social">
    </a>
    <a href="https://trendshift.io/repositories/10584" target="_blank"><img src="https://trendshift.io/api/badge/repositories/10584" alt="unclecode%2Fcrawl4ai | Trendshift" style="width: 100px; height: 20px;"     
    <a href="https://www.python.org/">
    <img src="https://img.shields.io/badge/Made%20with-Python-1f425f.svg" alt="Made_with_python">
    </a>
    <img src="https://static.pepy.tech/personalized-badge/upsonic?period=total&units=international_system&left_color=grey&right_color=blue&left_text=PyPI%20Downloads" alt="pypi_downloads">
  </p>


# Introduction
Upsonic is a reliability-focused framework designed for real-world applications. It enables trusted agent workflows in your organization through advanced reliability features, including verification layers, triangular architecture, validator agents, and output evaluation systems.

# Why Choose Upsonic?
Upsonic is a next-generation framework that makes agents production-ready by solving three critical challenges:

1- **Reliability**: While other frameworks require expertise and complex coding for reliability features, Upsonic offers easy-to-activate reliability layers without disrupting functionality.

2- **Model Context Protocol**: The MCP allows you to leverage tools with various functionalities developed both officially and by third parties without requiring you to build custom tools from scratch.

3- **Secure Runtime**: Isolated environment to run agents

![sdk-server](https://github.com/user-attachments/assets/1b276199-ae60-4221-b8e6-b266443a3641)

<br>

**Key features:**

- **Production-Ready Scalability**: Deploy seamlessly on AWS, GCP, or locally using Docker.
- **Task-Centric Design**: Focus on practical task execution, with options for:
    - Basic tasks via LLM calls.
    - Advanced tasks with V1 agents.
    - Complex automation using V2 agents with MCP integration.
- **MCP Server Support**: Utilize multi-client processing for high-performance tasks.
- **Tool-Calling Server**: Exception-secure tool management with robust server API interactions.
- **Computer Use Integration**: Execute human-like tasks using Anthropic’s ‘Computer Use’ capabilities.
- **Easily adding tools:** You can add your custom tools and MCP tools with a single line of code.
<br>

# 📙 Documentation

You can access our documentation at [docs.upsonic.ai](https://docs.upsonic.ai/). All concepts and examples are available there.

<br>

# 🛠️ Getting Started

### Prerequisites

- Python 3.10 or higher
- Access to OpenAI or Anthropic API keys (Azure and Bedrock Supported)

## Installation

```bash
pip install upsonic

```



# Basic Example

Set your OPENAI_API_KEY

```console
export OPENAI_API_KEY=sk-***
```

Start the agent 

```python
from upsonic import Task, Agent

task = Task("Who developed you?")

agent = Agent("Coder")

agent.print_do(task)
```

<br>
<br>


## Reliability Layer

LLM output reliability is critical, particularly for numerical operations and action execution. Upsonic addresses this through a multi-layered reliability system, enabling control agents and verification rounds to ensure output accuracy.

**Verifier Agent**: Validates outputs, tasks, and formats - detecting inconsistencies, numerical errors, and hallucinations
**Editor Agent**: Works with verifier feedback to revise and refine outputs until they meet quality standards
**Rounds**: Implements iterative quality improvement through scored verification cycles
**Loops**: Ensures accuracy through controlled feedback loops at critical reliability checkpoints


```python
class ReliabilityLayer:
  prevent_hallucination = 10

agent = Agent("Coder", reliability_layer=ReliabilityLayer)
```


## Multi Agent

Distribute tasks effectively across agents with our automated task distribution mechanism. This tool matches tasks based on the relationship between agent and task, ensuring collaborative problem-solving across agents and tasks. 
```python
from upsonic import MultiAgent

MultiAgent.do([agent2, agent1], [task1, task2])
```

##  Response Format

The output is essential for deploying an AI agent across apps or as a service. In Upsonic, we use Pydantic BaseClass as input for the task system. This allows you to configure the output exactly how you want it, such as a list of news with title, body, and URL. You can create a flexible yet robust output mechanism that improves interoperability between the agent and your app.

```python
from upsonic import ObjectResponse

# Example ObjectResponse usage
class News(ObjectResponse):
    title: str
    body: str
    url: str
    tags: list[str]

class ResponseFormat(ObjectResponse):
    news_list: list[News]

```


## Tool Integration via MCP

Upsonic officially supports [Model Context Protocol (MCP)](https://www.claudemcp.com/) and custom tools. You can use hundreds of MCP servers at https://glama.ai/mcp/servers or https://smithery.ai/ We also support Python functions inside a class as a tool. You can easily generate your integrations with that.

```python
from upsonic import Agent, Task, ObjectResponse

# Define Fetch MCP configuration
class FetchMCP:
    command = "uvx"
    args = ["mcp-server-fetch"]

# Create response format for web content
class WebContent(ObjectResponse):
    title: str
    content: str
    summary: str
    word_count: int

# Initialize agent
web_agent = Agent(
    "Web Content Analyzer",
    model="openai/gpt-4o",  # You can use other models
)

# Create a task to analyze a web page
task = Task(
    description="Fetch and analyze the content from url. Extract the main content, title, and create a brief summary.",
    context=["https://upsonic.ai"],
    tools=[FetchMCP],
    response_format=WebContent
)
    
# Usage
web_agent.print_do(task)
print(result.title)
print(result.summary)

```



## Direct LLM Call

Direct LLM calls offer faster, cheaper solutions for simple tasks. In Upsonic, you can make calls to model providers without any abstraction level and organize structured outputs. You can also use tools with LLM calls.

```python
from upsonic import Direct

Direct.do(task1)

```

## Reliable Computer Use
Computer use can able to human task like humans, mouse move, mouse click, typing and scrolling and etc. So you can build tasks over non-API systems. It can help your linkedin cases, internal tools. Computer use is supported by only Claude for now.

```python

from upsonic.client.tools import ComputerUse

...

tools = [ComputerUse]
...

```

<br>
<br>

## Telemetry

We use anonymous telemetry to collect usage data. We do this to focus our developments on more accurate points. You can disable it by setting the UPSONIC_TELEMETRY environment variable to false.

```python
import os
os.environ["UPSONIC_TELEMETRY"] = "False"
```
<br>
<br>



### Coming Soon

- **Dockerized Server Deploy**
- **Verifiers For Computer Use**
