# Haystack

## Overview
Haystack is an open-source framework for building production-ready LLM applications, retrieval-augmented generative pipelines and state-of-the-art search systems that work intelligently over large document collections.

## Core concepts

- **Components:** Haystack components are modular building blocks for AI pipelines. They're Python classes that perform specific tasks, often powered by LLMs. Components have defined inputs and outputs, making them easy to connect and validate in pipelines.

- **Generators:** Generators produce text responses based on prompts. They come in two types: chat (for conversational contexts) and non-chat (for simpler text generation tasks like translation or summarization). Each Generator is tailored to specific LLM technologies.

- **Retrievers:** Retrievers search through documents in a Document Store to find relevant information matching a user query. They're customized for specific Document Stores to handle unique database requirements efficiently.

- **Document Stores:** Document Stores are interfaces to databases that store and manage documents in Haystack. They provide methods for reading, writing, and deleting documents, allowing various components to interact with the stored data.

- **Data Classes:** Data classes in Haystack, such as Document and Answer, carry information through the pipeline. They encapsulate text, metadata, and other relevant data, facilitating organized data flow between components.

- **Pipelines:** Pipelines in Haystack combine various components, Document Stores, and integrations to create flexible and powerful AI systems. They can be designed for complex workflows, including preprocessing, indexing, and querying, and can be serialized for easy sharing and reuse.

## Secret Management

When a Haystack component that requires authentication (e.g. API key) gets initialised, it will detect the exported key and pass the corresponding value to the function parameter. Since pipelines are serializable, this method ensures there are no memory leaks and is recommended by Haystack.
``` py linenums="1"
# Export an environment variable name `OPENAI_API_KEY` with its value
dotenv.load_dotenv()
llm_generator = OpenAIGenerator() # Uses the default value from the env var for the component
```

## Example: Building Conversational AI with Function Calling
In this example, we’ll create a Haystack pipeline as a function-calling tool and implement applications using Azure OpenAI’s Chat Completion API through `AzureOpenAIChatGenerator` for agent-like behavior.

1. Setting Up Development Environment
    - `pip install haystack-ai`
    - `pip install dotenv`
    - `pip install gradio`
    - Create `.env` file and save your environment variables there: 
        - `AZURE_OPENAI_API_KEY` or `AZURE_OPENAI_AD_TOKEN`
        - `AZURE_OPENAI_ENDPOINT`
        - `AZURE_OPENAI_DEPLOYMENT_NAME`
2. Defining RAG pipeline
``` py title="rag_pipeline.py" linenums="1"
import os
from dotenv import load_dotenv
from haystack import Pipeline, Document
from haystack.utils import Secret
from haystack.document_stores.in_memory import InMemoryDocumentStore
from haystack.components.retrievers.in_memory import InMemoryBM25Retriever
from haystack.components.generators import AzureOpenAIGenerator
from haystack.components.builders.prompt_builder import PromptBuilder

load_dotenv()

# Store example documents used for RAG
document_store = InMemoryDocumentStore()
document_store.write_documents(
    [
        Document(content="My name is Jean and I live in Paris."),
        Document(content="My name is Mark and I live in Berlin."),
        Document(content="My name is Giorgio and I live in Rome."),
    ]
)

prompt_template = """
Given these documents, answer the question.
Documents:
{% for doc in documents %}
    {{ doc.content }}
{% endfor %}
Question: {{question}}
Answer:
"""

retriever = InMemoryBM25Retriever(document_store=document_store)

prompt_builder = PromptBuilder(template=prompt_template)

llm = AzureOpenAIGenerator(
    azure_deployment=os.getenv("AZURE_OPENAI_DEPLOYMENT_NAME")
)

# Initialise pipeline and add components.
rag_pipeline = Pipeline()
rag_pipeline.add_component("retriever", retriever)
rag_pipeline.add_component("prompt_builder", prompt_builder)
rag_pipeline.add_component("llm", llm)
rag_pipeline.connect("retriever", "prompt_builder.documents")
rag_pipeline.connect("prompt_builder", "llm")
rag_pipeline.draw(path="my_pipeline.png")


def rag_pipeline_func(query):
    result = rag_pipeline.run(
        {
            "retriever": {"query": query},
            "prompt_builder": {"question": query},
        }
    )
    return {"reply": result["llm"]["replies"][0]}
```

3. Defining Weather Tool
``` py title="weather_tool.py" linenums="1"
WEATHER_INFO = {
    "Berlin": {"weather": "mostly sunny", "temperature": 7, "unit": "celsius"},
    "Paris": {"weather": "mostly cloudy", "temperature": 8, "unit": "celsius"},
    "Rome": {"weather": "sunny", "temperature": 14, "unit": "celsius"},
    "Madrid": {"weather": "sunny", "temperature": 10, "unit": "celsius"},
    "London": {"weather": "cloudy", "temperature": 9, "unit": "celsius"},
}


def get_current_weather(location: str):
    if location in WEATHER_INFO:
        return WEATHER_INFO[location]
    # If asked about a new city, return this knowledge.
    else:
        return {"weather": "sunny", "temperature": 21.8, "unit": "fahrenheit"}
```

4. Putting It All Together
``` py title="main.py" linenums="1"
import json
import os
import gradio as gr
from dotenv import load_dotenv
from haystack.dataclasses import ChatMessage
from haystack.components.generators.chat import AzureOpenAIChatGenerator

from tools import get_current_weather, rag_pipeline_func, tools

def main():
    load_dotenv()

    messages = [
        ChatMessage.from_system(
            "Don't make assumptions about what values to plug into functions. Ask for clarification if a user request is ambiguous."
        ),
        ChatMessage.from_user("WHat's the weather in Berlin?"),
    ]

    chat_generator = AzureOpenAIChatGenerator(
        azure_deployment=os.getenv("AZURE_OPENAI_DEPLOYMENT_NAME"),
        api_version=os.getenv("AZURE_OPENAI_API_VERSION"),
    )
    response = chat_generator.run(messages=messages, generation_kwargs={"tools": tools})

    function_call = json.loads(response["replies"][0].content)[0]
    function_name = function_call["function"]["name"]
    function_args = json.loads(function_call["function"]["arguments"])
    print("Function Name:", function_name)
    print("Function Arguments:", function_args)

    ## Find the correspoding function and call it with the given arguments
    available_functions = {
        "rag_pipeline_func": rag_pipeline_func,
        "get_current_weather": get_current_weather,
    }
    function_to_call = available_functions[function_name]
    function_response = function_to_call(**function_args)
    print("Function Response:", function_response)

    function_message = ChatMessage.from_function(
        content=json.dumps(function_response), name=function_name
    )
    messages.append(function_message)

    response = chat_generator.run(messages=messages, generation_kwargs={"tools": tools})

    print(response)

    def chatbot_with_fc(message, history):
        messages.append(ChatMessage.from_user(message))
        response = chat_generator.run(
            messages=messages, generation_kwargs={"tools": tools}
        )

        while True:
            if (
                response
                and response["replies"][0].meta["finish_reason"] == "tool_calls"
            ):
                function_calls = json.loads(response["replies"][0].content)
                print(response["replies"][0])
                for function_call in function_calls:
                    ## Parse function calling information
                    function_name = function_call["function"]["name"]
                    function_args = json.loads(function_call["function"]["arguments"])

                    ## Find the correspoding function and call it with the given arguments
                    function_to_call = available_functions[function_name]
                    function_response = function_to_call(**function_args)

                    ## Append function response to the messages list using `ChatMessage.from_function`
                    messages.append(
                        ChatMessage.from_function(
                            content=json.dumps(function_response), name=function_name
                        )
                    )
                    response = chat_generator.run(
                        messages=messages, generation_kwargs={"tools": tools}
                    )

            # Regular Conversation
            else:
                messages.append(response["replies"][0])
                break
        return response["replies"][0].content

    demo = gr.ChatInterface(
        fn=chatbot_with_fc,
        examples=[
            "Can you tell me where Giorgio lives?",
            "What's the weather like in Madrid?",
            "Who lives in London?",
            "What's the weather like where Mark lives?",
        ],
        title="Ask me about weather or where people live!",
    )

    demo.launch()


if __name__ == "__main__":
    main()
```