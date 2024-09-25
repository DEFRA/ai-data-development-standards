---
tags:
  - LLM
  - Agents
---
# Agents

## Overview

This page provides an overview of different agent frameworks and types for Large Language Models (LLMs), as well as common agent design patterns.

## Agent Frameworks

- LangGraph (Python/JavaScript)
- Haystack (Python)
- CrewAI (Python)
- AutoGen (Python)

## Agent Types

- **Tool-Using Agents** - Use external tools and functions (e.g., databases, APIs, search engines, or calculators) to retrieve information or perform actions

- **Reflex Agents** - Make decisions based on current inputs. These agents don’t store previous states or contexts

- **Memory-Augmented Agents** - Store past interactions and use them to improve further responses

- **Persona-Based Agents** - Adopt a specific persona or role, influencing their language, tone, and behavior based on that persona

- **Self-Improving Agents** - Iterate and improve on their own outputs by evaluating their performance and learning from mistakes

## Agent Design Patterns

### Monolithic Design

A single agent that handles many tasks, such as chatbots with a wide range of abilities.

### Modular Design

Breaks agents into modules, each handling specific tasks (e.g., one for retrieval, one for reasoning).

### Orchestrated Design

Agents are coordinated by a meta-agent that delegates tasks to specialized agents.

- Can handle more complex tasks by combining the strengths of multiple agents
- Can be more difficult to implement and maintain than monolithic or modular designs

## References

- [RagaAI Agentic LLM Design Patterns](https://raga.ai/blogs/agentic-llm-design-patterns)