---
tags:
  - AWS
---

# AWS AI Services

<div class="grid cards" markdown>

-   :material-radar:{ .lg .middle } __Techradar__

    ---

    Assess

-   :material-thumb-up:{ .lg .middle } __Recommendation__

    ---

    Continue with assessment

</div>

## Overview

AWS offers a range of pre-trained AI services designed for integration into applications. These services address common business needs, including:

- Delivering personalized recommendations
- Modernizing contact centers
- Enhancing safety and security measures
- Boosting customer engagement

Powered by the same advanced deep learning technology behind Amazon.com and AWS machine learning services, these AI solutions provide high-quality and accurate results. The APIs continually learn and improve, ensuring the applications benefit from the latest advancements in AI technology.

## Main Services

### Amazon Q Business

Amazon Q Business is a generative AI assistant that is fully managed and hosted by AWS, it allows a ChatGPT-like experience where users can prompt an AI to perform tasks, such as “generate a sales script for our new company product”. Furthermore, it can also create an app from the generated response to make repeat similar responses quicker.

It can leverage business specific data (knowledge retrieval) - information can be uploaded in PDF, Word, HTML format which the system can then refer to.

The system is available via an API, with SDK’s available in most popular languages such as JavaScript, Java and Python.

### Amazon Q Developer

Amazon Q Developer is an AI coding assistant that can perform a number of tasks aimed at improving developer productivity, such as:

- code suggestion and generation
- code summarisation
- upgrading code or translating from one language to another
- writing tests
- analysing errors and logs

Q developer is available through the AWS console, or as plugins for common IDE’s such as Visual Studio Code.

### Bedrock

Whilst Amazon Q is a hosted end-to-end hosted service, AWS Bedrock provides access to the underlying FM’s (Foundation Models) and LLM’s, with models from leading providers, such as Meta (Lama2 family), Anthropic (Claude family), AI21, Cohere and AWS’s own models, allowing software developers the opportunity to develop their own custom applications.

Access to Bedrock models is either through the AWS console, or via SDK’s such as Python’s Boto3 of JavaScript libraries, a comprehensive range of supporting services such as vector stores are available through the AWS ecosystem.

Bedrock also offers a number of unique features, such as Guardrails ([AI Safety - Guardrails for Amazon Bedrock - AWS](https://aws.amazon.com/bedrock/guardrails/)) for adding filters and safeguards to help build responsible solutions.

## Advantages of using AWS

Amazon Q offers end-to-end solutions for teams that do not have technical capabilities, Q apps offer the ability to streamline AI tasks; AWS offers exclusive access to certain models such as AWS Titan, and Anthropic Claude. Solutions built on AWS can take advantage of the wider AWS framework, such as IAM, VPC, firewalls etc.

Data is protected by AWS policies. AWS also offer a number of unique value adds such as a framework that can redact PII.

## Disadvantages of using AWS

Amazon Q offers no choice of LLM for language tasks, so models that perform better in specific scenarios may not be available, affecting system performance; currently Bedrock and Q are not available in the UK (May 2024), so GDPR and DPA issues may be a problem.

AWS offers common functionality that other model providers offer, such as API access, fine tuning, embeddings generation.

## Accessing Bedrock through Langchain

AWS Bedrock models are available in Langchain