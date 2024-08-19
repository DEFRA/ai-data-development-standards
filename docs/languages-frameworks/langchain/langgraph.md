# LangGraph

> **NOTE**: The use of LangGraph is **under review** for the use in production.

## Overview

LangGraph is a LangChain library designed to build stateful, multi-agent llm applications. It extends the LangChain Expression Language by enabling the coordination of multiple actors across various computational steps in a cyclic manner.

## Possible Use Cases

- Redirecting customer queries and feedback to specialized teams better equipped to provide accurate and detailed responses
- Calling specialised tools to resolve custom queries.

## Example Use Case

1. Overview
The bot acts as a customer support representative and answers customer queries. If a query targets billing or technical issues, the bot then redirects it to a more specialised agent to asnwer it.

2. Installation
```
npm install @langchain/core @langchain/openai @langchain/langgraph
```

3. Code snippet
``` js title="customer_support.js" linenums="1"
const { ChatOpenAI } = require("@langchain/openai");
const { END, MessageGraph } = require("@langchain/langgraph")
const { ChatPromptTemplate, MessagesPlaceholder } = require("@langchain/core/prompts");
const { AIMessage } = require("@langchain/core/messages");
const { HumanMessage } = require("@langchain/core/messages");
const { StringOutputParser } = require("@langchain/core/output_parsers");

const model = new ChatOpenAI({
  temperature: 0
})

const graph = new MessageGraph();

graph.addNode("initial_support", async (state) => {
  const SYSTEM_TEMPLATE = `
    You are frontline support staff for LangCorp, a company that sells computers.
    Be concise in your responses.
    You can chat with customers and help them with basic questions, but if the customer is having a billing or technical problem,
    do not try to answer the question directly or gather information.
    Instead, immediately transfer them to the billing or technical team by asking the user to hold for a moment.
    Otherwise, just respond conversationally.`;

  const prompt = ChatPromptTemplate.fromMessages([
    ["system", SYSTEM_TEMPLATE],
    new MessagesPlaceholder("messages"),
  ]);

  return prompt.pipe(model).invoke({ messages: state });
});

graph.addNode("billing_support", async (state) => {
  const SYSTEM_TEMPLATE = `
    You are an expert billing support specialist for LangCorp, a company that sells computers.
    Help the user to the best of your ability, but be concise in your responses.
    You have the ability to authorize refunds, which you can do by transferring the user to another agent who will collect the required information.
    If you do, assume the other agent has all necessary information about the customer and their order.
    You do not need to ask the user for more information.`;

  let messages = state;
  if (messages[messages.length - 1]._getType() === "ai") {
    messages = state.slice(0, -1);
  }

  const prompt = ChatPromptTemplate.fromMessages([
    ["system", SYSTEM_TEMPLATE],
    new MessagesPlaceholder("messages"),
  ]);
  return prompt.pipe(model).invoke({ messages });
});

graph.addNode("technical_support", async (state) => {
  const SYSTEM_TEMPLATE = `
    You are an expert at diagnosing technical computer issues. You work for a company called LangCorp that sells computers.
    Help the user to the best of your ability, but be concise in your responses.`;

  let messages = state;
  if (messages[messages.length - 1]._getType() === "ai") {
    messages = state.slice(0, -1);
  }

  const prompt = ChatPromptTemplate.fromMessages([
    ["system", SYSTEM_TEMPLATE],
    new MessagesPlaceholder("messages"),
  ]);
  return prompt.pipe(model).invoke({ messages });
});

graph.addNode("refund_tool", async () => {
  return new AIMessage("Refund processed!");
});

graph.addConditionalEdges("initial_support", async (state) => {
  const SYSTEM_TEMPLATE = `
    You are an expert customer support routing system.
    Your job is to detect whether a customer support representative is routing a user to a billing team or a technical team, or if they are just responding conversationally.`;
  const HUMAN_TEMPLATE = `
    The previous conversation is an interaction between a customer support representative and a user.
    Extract whether the representative is routing the user to a billing or technical team, or whether they are just responding conversationally.

    If they want to route the user to the billing team, respond only with the word "BILLING".
    If they want to route the user to the technical team, respond only with the word "TECHNICAL".
    Otherwise, respond only with the word "RESPOND".

    Remember, only respond with one of the above words.`;
  const prompt = ChatPromptTemplate.fromMessages([
    ["system", SYSTEM_TEMPLATE],
    new MessagesPlaceholder("messages"),
    ["human", HUMAN_TEMPLATE],
  ]);
  const chain = prompt
    .pipe(model)
    .pipe(new StringOutputParser());
  const rawCategorization = await chain.invoke({ messages: state });
  if (rawCategorization.includes("BILLING")) {
    return "billing";
  } else if (rawCategorization.includes("TECHNICAL")) {
    return "technical";
  } else {
    return "conversational";
  }
}, {
  billing: "billing_support",
  technical: "technical_support",
  conversational: END
});

graph.addEdge("technical_support", END);

graph.addConditionalEdges("billing_support", async (state) => {
  const mostRecentMessage = state[state.length - 1];
  const SYSTEM_TEMPLATE = `
    Your job is to detect whether a billing support representative wants to refund the user.`;
  const HUMAN_TEMPLATE = `
    The following text is a response from a customer support representative.
    Extract whether they want to refund the user or not.
    If they want to refund the user, respond only with the word "REFUND".
    Otherwise, respond only with the word "RESPOND".

    Here is the text:

    <text>
    {text}
    </text>

    Remember, only respond with one word.`;
  const prompt = ChatPromptTemplate.fromMessages([
    ["system", SYSTEM_TEMPLATE],
    ["human", HUMAN_TEMPLATE],
  ]);
  const chain = prompt
    .pipe(model)
    .pipe(new StringOutputParser());
  const response = await chain.invoke({ text: mostRecentMessage.content });
  if (response.includes("REFUND")) {
    return "refund";
  } else {
    return "end";
  }
}, {
  refund: "refund_tool",
  end: END
});

graph.addEdge("refund_tool", END);

graph.setEntryPoint("initial_support");

const runnable = graph.compile();

myfunc = async(myGraph, humanMessage) => {
  const stream = await myGraph.stream(humanMessage);
  for await (const value of stream) {
    const [nodeName, output] = Object.entries(value)[0];
    if (nodeName !== END) {
      console.log("---STEP---")
      console.log(nodeName, output.content);
      console.log("---END STEP---")
    }
  }
}

const main = async () => {
  const humanMessage = new HumanMessage("I've changed my mind and I want a refund for order #182818!")
  await myfunc(runnable, humanMessage)
}

main()
```