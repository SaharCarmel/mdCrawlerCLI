Skip to main content
**Join us at Interrupt: The Agent AI Conference by LangChain on May 13 & 14 in San Francisco!**
![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)![Open on GitHub](https://img.shields.io/badge/Open%20on%20GitHub-grey?logo=github&logoColor=white)
Prerequisites
This guide assumes familiarity with the following concepts:
  * Chat models
  * LangChain Tools
  * How to use a model to call tools


In order to force our LLM to select a specific tool, we can use the `tool_choice` parameter to ensure certain behavior. First, let's define our model and tools:
```
from langchain_core.tools import tool@tooldefadd(a:int, b:int)->int:"""Adds a and b."""return a + b@tooldefmultiply(a:int, b:int)->int:"""Multiplies a and b."""return a * btools =[add, multiply]
```

**API Reference:**tool
For example, we can force our tool to call the multiply tool by using the following code:
```
llm_forced_to_multiply = llm.bind_tools(tools, tool_choice="multiply")llm_forced_to_multiply.invoke("what is 2 + 4")
```

```
AIMessage(content='', additional_kwargs={'tool_calls': [{'id': 'call_9cViskmLvPnHjXk9tbVla5HA', 'function': {'arguments': '{"a":2,"b":4}', 'name': 'Multiply'}, 'type': 'function'}]}, response_metadata={'token_usage': {'completion_tokens': 9, 'prompt_tokens': 103, 'total_tokens': 112}, 'model_name': 'gpt-4o-mini', 'system_fingerprint': None, 'finish_reason': 'stop', 'logprobs': None}, id='run-095b827e-2bdd-43bb-8897-c843f4504883-0', tool_calls=[{'name': 'Multiply', 'args': {'a': 2, 'b': 4}, 'id': 'call_9cViskmLvPnHjXk9tbVla5HA'}], usage_metadata={'input_tokens': 103, 'output_tokens': 9, 'total_tokens': 112})
```

Even if we pass it something that doesn't require multiplcation - it will still call the tool!
We can also just force our tool to select at least one of our tools by passing in the "any" (or "required" which is OpenAI specific) keyword to the `tool_choice` parameter.
```
llm_forced_to_use_tool = llm.bind_tools(tools, tool_choice="any")llm_forced_to_use_tool.invoke("What day is today?")
```

```
AIMessage(content='', additional_kwargs={'tool_calls': [{'id': 'call_mCSiJntCwHJUBfaHZVUB2D8W', 'function': {'arguments': '{"a":1,"b":2}', 'name': 'Add'}, 'type': 'function'}]}, response_metadata={'token_usage': {'completion_tokens': 15, 'prompt_tokens': 94, 'total_tokens': 109}, 'model_name': 'gpt-4o-mini', 'system_fingerprint': None, 'finish_reason': 'stop', 'logprobs': None}, id='run-28f75260-9900-4bed-8cd3-f1579abb65e5-0', tool_calls=[{'name': 'Add', 'args': {'a': 1, 'b': 2}, 'id': 'call_mCSiJntCwHJUBfaHZVUB2D8W'}], usage_metadata={'input_tokens': 94, 'output_tokens': 15, 'total_tokens': 109})
```

#### Was this page helpful?
