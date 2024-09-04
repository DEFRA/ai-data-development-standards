# Promptfoo

<div class="grid cards" markdown>

-   :material-radar:{ .lg .middle } __Techradar__

    ---

    Assess

-   :material-thumb-up:{ .lg .middle } __Recommendation__

    ---

    Continue with assessment

</div>

> **NOTE**: The use of promptfoo is **under review**.

## Overview

Promptfoo is a Node.js tool for testing and evaluating LLM outputs, it provides command line tooling as well as JavaScript and Python libraries. Prompts are entered into a test script where they are passed through chosen LLM models and the outputs evaluated for accuracy, data type or sentimentality.

## Installation

1. Promptfoo requires Node.js 16.x (available [here](https://nodejs.org/en/download)). 

  - To use the command line option, it must be installed globally:

        npm install -g promptfoo
    
  - To install for a specific project, a local copy can be installed:

        npm install promptfoo
  
  Once installed, run `promptfoo init` to create a promptfooconfig.yaml config file

## Command line usage

- From the folder where the promptfooconfig.yaml file is located, enter:

    `promptfoo eval`

Results are displayed on screen.

- To view more information in a browser, enter: 

    `promptfoo view`

### YAML file

The yaml config file contains numerous options to configure tests. The file contains a number of required sections:

- **prompts**: a list of prompts to test, can include prompt text such as “Write a tweet about {{topic}}” or a (list) of text file(s) containing prompts. variables are contained in curly braces

- **providers**: an array of models to evaluate, e.g. [openai:gpt-3.5-turbo-0613, openai:gpt-4]

- **tests**: a list of tests to carry out

- **defaultTests** (optional): a set of tests to run with every test in the script

``` yaml linenums="1"
prompts: [prompt1.txt, prompt2.txt]
providers: [openai:gpt-3.5-turbo, vertex:gemini-pro]
defaultTest:
  assert:
    - type: llm-rubric
      value: does not describe self as an AI, model, or chatbot
tests:
  - vars:
      language: French
      input: Hello world
    assert:
      - type: contains-json
      - type: javascript
        value: output.toLowerCase().includes('bonjour')
  - vars:
      language: German
      input: How's it going?
    assert:
      - type: similar
        value: was geht
        threshold: 0.6 # cosine similarity
```
Full options are available from [Configuration | promptfoo](https://www.promptfoo.dev/docs/configuration/guide).

### RAG testing

Retrieval-augmented generation can be tested by adding context documents at testing time. Context documents are stored and retrieved from a vector store, typically by creating a Python script to fetch documents and use this as a provider in the promptfooconfig.yaml file. Assertions can then be made against the retrieved documents. 

## Node (JavaScript) usage

Promptfoo can be run as a Node package:

``` js linenums="1"
import promptfoo from 'promptfoo'

const results = await promptfoo.evaluate(testSuite, options)
```

where testSuite is a JavaScript equivalent of the promptfooconfig.yaml file, e.g.

``` js linenums="1"
const testSuite = {
  providers: ['openai:gpt-3.5-turbo', 'vertex:gemini-pro'],
  tests: [
    {
      vars: {
        language: 'French',
        input: 'Hello world'
      },
      assert: [
        {
          type: 'contains-json',
          type: 'javascript',
          value: output.toLowerCase().includes('bonjour')
        }
      ]
    },
    {
      vars: {
        language: 'German',
        input: 'How\'s it going?'
      },
      assert: [
        {
          type: 'similar',
          value: 'was geht',
          threshold: 0.6
        }
      ]
    }
  ]
}
```

## CI/CD integration

Promptfoo can be integrated into a CI/CD pipeline, typically GitHub actions or Jenkins.

### Jenkins

Steps to use promptfoo with Jenkins include:

1. Installing promptfoo:

    `npm install -g promptfoo`

2. Setting any environment variables, e.g. `OPENAI_API_KEY`

3. Running a test script:

    `promptfoo eval -c path/to/config.yaml --prompts path/to/prompts/**/*.json -o output.json`

The output file output.json can be parsed and further actions such as failing the build can be taken.

### GitHub actions

A pre-configured GitHub action is available to create automatic tests.

Promptfoo can also be integrated into Jest and Mocha/Chai testing environments.

## Recommended usage

Promptfoo can be used on a local development machine to ensure prompts are producing the required output.

Promptfoo is ideally integrated into a CI/CD/MLOps testing pipeline, where models are fine-tuned and re-trained on a periodic basis.

## Supported models

Promptfoo currently supports the following models:

- OpenAI: requires the environmental variable `OPENAI_API_KEY` to be set with a valid API key
- Anthropic: requires the environmental variable `ANTHROPIC_API_KEY` to be set with a valid API key
- Azure: requires the environmental variable `AZURE_OPENAI_API_KEY` to be set with a valid API key
- Llama
- Ollama
- Google Vertex
- Google AI Studio
- HuggingFace
- Replicate
- AWS Bedrock
- Cohere
- Mistral
- OpenLLM
- Perplexity
- Together AI
- vllm

Python and JavaScript scripts can also be used.

More information is available from [Providers | promptfoo](https://www.promptfoo.dev/docs/providers/).