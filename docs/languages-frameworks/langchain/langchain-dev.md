---
tags:
  - LLM
---

# Langchain Development

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

Langchain is an open-source framework designed to simplify the development of applications powered by large language models (LLMs). It provides a suite of tools and products that enable developers to create context-aware, reasoning applications by leveraging an organization's data and APIs. Langchain offers flexibility in model selection, supports retrieval-augmented generation (RAG), and includes features for debugging, testing, deploying, and monitoring LLM-based systems.

## Setup

### Prerequisites

- Install langchain: `npm install -S langchain`
- Sign up for an OpenAI account and obtain an API key
- Install OpenAI: `npm install @langchain/openai`
- Set your OpenAI key in an environment variable (optional)

### Additional packages

- @langchain/community
- @langchain/core
- @langchain/nomic
- @langchain/openai
- @qdrant/js-client-rest
- cheerio

## Prompting a Model

``` js linenums="1"
import { ChatOpenAI } from "@langchain/openai"

const chatModel = new ChatOpenAI({
    openAIApiKey: "..."
})

const response = await chatModel.invoke('Translate "Hello World" into German.')
console.log(response.content)
```

Once instantiated, the model can be queried:
``` js linenums="1"
const response = await chatModel.invoke('Translate "Hello World" into German.')
console.log(response)
// Outputs:
AIMessage {
  lc_serializable: true,
  lc_kwargs: {
    content: '"Hallo Welt"',
    additional_kwargs: { function_call: undefined, tool_calls: undefined }
  },
  lc_namespace: [ 'langchain_core', 'messages' ],
  content: '"Hallo Welt"',
  name: undefined,
  additional_kwargs: { function_call: undefined, tool_calls: undefined }
}
```

Alternatively, the output can be isolated:
``` js linenums="1"
console.log(response.content)
// Outputs:
"Hello World" in German is "Hallo Welt."
```

Langchain also provides templates to allow system prompts and metadata:
``` js linenums="1"
// imports and code as above...
import { ChatPromptTemplate } from "@langchain/core/prompts"
const prompt = ChatPromptTemplate.fromMessages([
    ["system", "You are a world class translator."],
    ["user", "{input}"]
])
const llmChain = prompt.pipe(chatModel)
const response = await llmChain.invoke({
    input: 'Translate "Hello World" into German.'
})
console.log(response)
// Outputs:
AIMessage {
  lc_serializable: true,
  lc_kwargs: {
    content: '"Hello World" in German is "Hallo Welt".',
    additional_kwargs: { function_call: undefined, tool_calls: undefined }
  },
  lc_namespace: [ 'langchain_core', 'messages' ],
  content: '"Hello World" in German is "Hallo Welt".',
  name: undefined,
  additional_kwargs: { function_call: undefined, tool_calls: undefined }
}
```

`pipe` creates a chain of commands - a concept that is central to Langchain
Langchain provides a convenient parser for isolating the model’s output:
``` js linenums="1"
// imports and code as above...
import { StringOutputParser } from "@langchain/core/output_parsers"
const outputParser = new StringOutputParser()
const llmChain = prompt.pipe(chatModel).pipe(outputParser)
response = await llmChain.invoke({
    input: 'Translate "Hello World" into German.'
})
console.log(response)
// Outputs:
"Hello World" in German is "Hallo Welt."
```
