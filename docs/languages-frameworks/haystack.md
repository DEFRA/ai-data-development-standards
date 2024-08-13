# Haystack
Haystack is an open-source framework for building production-ready LLM applications, retrieval-augmented generative pipelines and state-of-the-art search systems that work intelligently over large document collections.

## Table of Contents

1. [Core Concepts](#core-concepts)
2. [Secret Management]()
3. [Example Use Case]()

## Core concepts

1. Components: Haystack components are modular building blocks for AI pipelines. They're Python classes that perform specific tasks, often powered by LLMs. Components have defined inputs and outputs, making them easy to connect and validate in pipelines.

2. Generators: Generators produce text responses based on prompts. They come in two types: chat (for conversational contexts) and non-chat (for simpler text generation tasks like translation or summarization). Each Generator is tailored to specific LLM technologies.

3. Retrievers: Retrievers search through documents in a Document Store to find relevant information matching a user query. They're customized for specific Document Stores to handle unique database requirements efficiently.

4. Document Stores: Document Stores are interfaces to databases that store and manage documents in Haystack. They provide methods for reading, writing, and deleting documents, allowing various components to interact with the stored data.

5. Data Classes: Data classes in Haystack, such as Document and Answer, carry information through the pipeline. They encapsulate text, metadata, and other relevant data, facilitating organized data flow between components.

6. Pipelines: Pipelines in Haystack combine various components, Document Stores, and integrations to create flexible and powerful AI systems. They can be designed for complex workflows, including preprocessing, indexing, and querying, and can be serialized for easy sharing and reuse.

## Secret Management

When a Haystack component that requires authentication (e.g. API key) gets initialised, it will detect the exported key and pass the corresponding value to the function parameter. Since pipelines are serializable, this method ensures there are no memory leaks and is recommended by Haystack.
``` py linenums="1"
# Export an environment variable name `OPENAI_API_KEY` with its value
dotenv.load_dotenv()
llm_generator = OpenAIGenerator() # Uses the default value from the env var for the component
```