---
tags:
  - LangChain
---

# Ministerial Correspondence

## Overview

The MCU AI project is a Proof of Concept (PoC) LLM system that showcases how large language models could be used to enhance the handling of ministerial communications. 

It aims to:

- Triage documents to extract key facts and generate summary
- Automate initial drafts of responses
- Reduce administrative workload for correspondence drafters



## Features

- Triages and summarises received correpondence using an LLM
- Retrieval Augmented Generation (RAG) to generate responses based on policy documents
- Built to accomodate a range of LLMs from various providers:
    - Azure OpenAI
    - Amazon Bedrock
    - Ollama (for local deployment)
- Uses custom personas to tailor response tone and language
- Supports full audit capabilities via response history page and IG Log

## Gallery

![image](../images/projects/mcu/document-queue.png)

<div class="grid cards" markdown>

-   LLM Selection

    ![image](../images/projects/mcu/select-model.png)

-   Persona Selection

    ![image](../images/projects/mcu/select-persona.png)

</div>

<div class="grid cards" markdown>

-   Knowledge Selection

    ![image](../images/projects/mcu/select-knowledge.png)

</div>

<div class="grid cards" markdown>

-   Sources

    ![image](../images/projects/mcu/sources.png)

</div>

<figure markdown="span">

  <figcaption>Response History</figcaption>

  ![image](../images/projects/mcu/agent-response.png){ width="600" }

</figure>



## Github Repos

- [DEFRA/coreai-mcu-core](https://github.com/DEFRA/coreai-mcu-core)