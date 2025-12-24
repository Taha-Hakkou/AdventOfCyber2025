# day 8

AI agent exploitation (prompt injection)
 
- **LLMs (large language models)** have restrictions that prevent them from going beyond their built-in abilities, which limits them. They cannot act outside their text box, and their training only lasts up to a certain point in time. Because of this, they may invent facts, miss recent events, or fail at tasks that require real-world actions.

- Common risks include **prompt injection**, **jailbreaking**, and **data poisoning**, where attackers shape prompts or data to force the model to produce unsafe or unintended results.

- These gaps in control explain why the next step was to move towards **agentic AI**, where LLMs are given the ability to plan, act, and interact with the outside world.

- AI agent will try to:
1. Plan multi-step plans to accomplish goals.
2. Act on things (run tools, call APIs, copy files).
3. Watch & adapt, adapting strategy when things fail or new knowledge is discovered.

- agentic AI uses **chain-of-thought (CoT)** reasoning to improve its ability to perform complex, multi-step tasks autonomously.

- CoT has a critical limitation: because it operates in isolation, without access to external knowledge or tools, it often suffers from fact hallucination, outdated knowledge, and error propagation.

- **ReAct (Reason + Act)** addresses this limitation by unifying reasoning and acting within the same framework.

- Nowadays, almost any LLM natively supports function calling, which enables the model to call external tools or APIs. Developers register tools with the model, describing them in JSON schemas.