---
tags:
  - LLM
  - LangChain
---

# Langchain

<div class="grid cards" markdown>

-   :material-radar:{ .lg .middle } __Techradar__

    ---

    Hold

-   :material-thumb-up:{ .lg .middle } __Recommendation__

    ---

    Refrain from investing; monitor developments

</div>

> **NOTE**: The use of LangChain is **under review** for the use in production.

## Overview

Langchain is a framework for developing applications using Large Language Models (LLM’s), it provides common interfaces for common Natural Language Processing (NLP) tasks to numerous vendors, including:

- **Chat**: interact with various models to create chatbots, question-answer systems, queries can be chained together to provide a chat history, and context documents can be loaded and used to answer RAG (Retrieval Augmented Generation) queries.

- **Templates**: Langchain provides a number of pre-written prompt templates to use to query LLM’s.

- **Document and webpage loading**: external documents can be loaded from multiple sources - e.g. text files, web pages, databases to provide context for chats.

- **Embeddings**: LLM’s work with word embeddings rather than words, many LLM’s provide API access to generate embeddings and Langchain provides a common interface for generating embeddings.

- **Vector storage and calculation**: Langchain provides easy access to a number of common vector storage systems, including databases and math systems to provide vector searching. Vector searching uses mathematical techniques such as cosine similarity to find similarities between different bodies of text, this allows an intelligent system to find the most appropriate context data for a user query by searching a vector store for similar text to a users query.

By offering a common interface, it is quick and easy to switch out back end providers.

## Recommendation

**July 2024**

As of July 2024, our recommendation is that LangChain is only used at the prototyping / PoC stage particularly when LLM evaluations are taking place for the project.

Although we have seen the following benefits from using LangChain:

- Ability to rapidly prototype LLM applications
- The ease of switching between different LLMs for evaluation
- Easy integration with LLM observability tools such as LangSmith and LangFuse

We have found the following issues outweigh these benefits:

- LangChain is not yet production-ready
- Uses a non-industry standard versioning. It is unclear whether breaking changes will be introduced in minor versions
- Large level of abstraction, which can make it difficult to debug and understand the framework
- Limited and outdated documentation
- Rare occasions of manual LLM config which defeats the purpose of using LangChain to switch between LLMs easily

The largest challenge listed here is dealing with the complex abstractions that LangChain provides and breaking changes on package updates. Although we have delivered multiple PoCs using LangChain, the downsides / risks of using it in production currently outweigh the benefits it brings.

The benefits of using LangChain are also hampered by the fact that everything it does can be done with vendor API calls. This means especially for production use cases, LangChain just adds an unnecessary dependency and abstraction layer.

If a project is in the early stages and the team is looking to quickly prototype and evaluate different LLMs, LangChain can be a good tool to use. However, if the project has the intention of moving to production, teams should consider using the vendor APIs directly.

## Getting started

Langchain provides JavaScript (Node) and Python libraries, instructions for installation can be found at:

- Python - PIP
- JavaScript (Node) - npm

Langchain modules are imported using the standard `import` command.

## Local vs API

Langchain supports the use of online LLM’s accessed via an API, as well as locally hosted LLM’s. Online API’s provide quick and easy access to commercially available systems such as OpenAI. To use these systems, an API key will be required, which can be obtained by signing up to the providers website. Usage will be charged as per the providers pricing structure.

Conversely, there are a number of excellent local LLM’s that can be downloaded and run free of charge, including Meta’s Llama models. One of the easiest ways of running local models is to use [Ollama](https://ollama.com/), a free framework for downloading and running models. Once a model has been downloaded and is running, it can be accessed in the same way as any other mode provider.

## Choosing an LLM provider

There are many considerations to choosing an LLM, including accuracy, speed and price. Huggingface provide a useful leader board of accuracy, individual providers provide pricing models for the various components of their systems.

Langchain allows the use of different providers for different tasks, e.g. using OpenAI to generate embeddings, and Mistral for chat. Using word embeddings from one provider (e.g. OpenAI) with a different LLM (e.g. Mistral) is not inherently a bad idea, but there are important considerations to keep in mind:

1. **Semantic Consistency**: Word embeddings capture semantic information about words and their relationships. If you use embeddings from one provider and a different language model, ensure that they are consistent in their understanding of word meanings. Mismatched semantics could lead to unexpected results.

2. **Compatibility**: Different language models and embeddings might have varying dimensions, scales, and representations. It’s crucial to verify that the embeddings align with the language model’s expectations. If they don’t match, you might need to perform dimensionality reduction or other adjustments.

3. **Fine-Tuning**: If you plan to fine-tune your language model (e.g., Mistral) on specific tasks, using embeddings from the same provider can be advantageous. Fine-tuning with consistent embeddings helps the model learn better task-specific representations.

4. **Domain Adaptation**: Consider the domain of your data. If your embeddings come from a different domain (e.g., news articles) than your target task (e.g., medical text), they might not transfer well. Domain-specific embeddings might be more suitable.

5. **Evaluation**: Evaluate the performance of your combined system. Use appropriate benchmarks and metrics to assess how well the embeddings integrate with the language model. If the results are satisfactory, you’re on the right track.

6. **Bias and Fairness**: Be cautious of any biases present in the embeddings. Some embeddings may inadvertently carry biases from their training data. Ensure that these biases do not negatively impact your downstream tasks.

7. **Resource Constraints**: Using embeddings from different providers might increase computational overhead. Consider the additional memory and processing requirements when combining them.

Whilst it is easy to swap providers using Langchain, once all models have been evaluated and a decision made, it should not be swapped out, e.g. using one model for local development and a different model for production.

## Test and experiment

Different LLM providers and models will produce different results it is important to test the results from your application on a regular basis. Consider using promptfoo to automate testing.

## References

- [LangChain in Python](https://python.langchain.com/v0.2/docs/introduction/)
- [LangChain in JavaScript](https://js.langchain.com/v0.2/docs/introduction/)