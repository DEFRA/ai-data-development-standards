---
tags:
  - LangChain
  - LLM
  - Python
---

# Langchain Python Guide

## Setup

### Prerequisites

- Install langchain: `pip install langchain`
- Sign up for an [OpenAI account](https://platform.openai.com/signup) and obtain an API key
- Install OpenAI: `pip install -qU langchain-openai`
- Set your OpenAI key in an environment variable (optional)

### Additional packages

- langchain_community
- langchain_chroma
- langchain-core

## Prompting a model

``` py linenums="1"
from langchain_openai import ChatOpenAI

chat_model = ChatOpenAI()
```

Once instantiated, the model can be queried:

``` py linenums="1"
response = chat_model.invoke('Translate "Hello World" into German.')
print(response)
# Outputs:
content='Hallo Welt'
```

Alternatively, the output can be isolated:

``` py linenums="1"
print(response.content)
# Outputs:
Hallo Welt
```

Langchain also provides templates to allow system prompts and metadata:

``` py linenums="1"
# imports and code as above...
from langchain_core.prompts import ChatPromptTemplate

prompt = ChatPromptTemplate.from_messages([
    ("system", "You are a world class translator."),
    ("user", "{input}")
])

llm_chain = prompt | chat_model
response = llm_chain.invoke({
    "input": 'Translate "Hello World" into German.'
})
print(response)
# Outputs:
content='"Hello World" in German is "Hallo Welt".'
```

`pipe` creates a chain of commands - a concept that is central to Langchain.

Langchain provides a convenient parser for isolating the model’s output:

``` py linenums="1"
# imports and code as above...
from langchain_core.output_parsers import StrOutputParser

output_parser = StrOutputParser()
llm_chain = prompt | chat_model | output_parser
response = llm_chain.invoke({
    "input": 'Translate "Hello World" into German.'
})
print(response)
# Outputs:
"Hello World" in German is "Hallo Welt."
```