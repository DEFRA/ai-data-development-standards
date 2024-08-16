# Retrieval Augmented Generation

## Overview

Retrieval Augmented Generation (RAG) is a process that allows LLM’s to answer domain-specific queries using data it has not seen during it’s training.

LLM’s such as ChatGPT are trained on a huge volume of text and are capable of answering general queries with a high degree of accuracy, however, they don’t respond well when answering queries against data it has not previously seen. in order to improve the LLM, 2 approaches can be taken:

- Fine-tuning
- RAG

Fine-tuning involves taking a copy of an LLM and performing additional training steps with new data. Training LLM’s is compute expensive and needs to be done on a regular basis as new data becomes available, given the time and cost of this approach it is often seen as impractical.

RAG is the preferred approach and involves a number of steps:

## Building Data Repository

The first step is to build a repository of content that will be used for answering domain-specific queries, this would typically include Word documents, PDF documents and web pages, this information is typically stored in a database.

### Create vector embeddings

During the RAG process, the system takes a users query and tries to find related documents from the document store. In order to provide fast retrieval, vector embeddings are created from the data in the datastore as vector representations to capture the semantic meaning of the text; these can then be matched against a users query to find the best matching documents to use for the RAG’s context.

Consider the 2 phrases:

- `the cat sat on the mat`
- `a cat was sitting on a mat`

The phrases are sufficiently different to make pattern matching difficult, however, vector embeddings capture the meaning of the phrases, meaning that these 2 phrases will have very similar vector embeddings, so a users query such as where was the cat sitting? will allow the system to find an answer.

## Building System Prompt

A prompt is an instruction sent to an LLM to tell it how to respond to a users query, it will typically contain metadata that contains the format the LLM should use to respond and details of the information being sent to the LLM to be answered.

It can typically include a “persona” which is a brief description of the type of person the LLM should impersonate when responding e.g. “You are an IT specialist who answers technical queries”.

Prompts should be verbose with clear instructions regarding the format of the input that the LLM will receive and the output format you require. The prompt should be broken down into clear sections, with each section separated by a delimiter - in this case `[]`. Delimiters can be be square brackets, angle brackets dashes and so on.

A typical RAG prompt can look like:
```
[INST]
You are an expert in writing responses to correspondence from the general public. 
You are assisting a Defra employee in writing a response to the document contained in [DOCUMENT] below.

You should only provide a response based on the knowledge contained in [CONTEXT] below. 
You should not infer any additional information.
[/INST]

[DOCUMENT]
{document}
[/DOCUMENT]

[CONTEXT]
{context}
[/CONTEXT]
```
Where `[INST]` are the instructions for the LLM, `[DOCUMENT]` is the users query and `[CONTEXT]` is the related information retrieved from the document store.

Each different LLM will take prompts in different formats, and will require careful tweaking to ensure the best possible response from the LLM.

## Fetching Relevant Documents

Documents are retrieved from the data store by comparing vector embeddings in the document store with the users query, the best matching documents are combined into the prompt as the prompts context.

Algorithms such as cosine similarity are used to find matching texts.

The RAG system can be extended further by including a history element, where a log of the users chat and the LLM’s responses are sent in the prompt.
