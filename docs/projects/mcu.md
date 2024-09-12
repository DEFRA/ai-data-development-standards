---
tags:
  - LangChain
---

# Ministerial Correspondence

## Overview

The Ministerial Correspondence Unit is a sophisticated tool designed to streamline the management and response of documents and communications directed towards ministers, by leveraging advanced language models to automate initial response drafts, thus reducing the administrative burden on staff.

![image](../images/projects/mcu/home.png)

## Features

### Upload documents

This system allows users to upload emails and letters received from the public or other stakeholders that are then triaged by the system to:

- generate summary
- extract key points and facts 
- analyse its sentiment

Additionally, users can upload a variety of documents that serve as knowledge repositories for language models to be used in generating responses.

<div class="grid cards" markdown>

-   Upload knowledge:

    ![image](../images/projects/mcu/upload-knowledge.png)

-   Upload document:

    ![image](../images/projects/mcu/upload-document.png)

</div>

### Document queue

Uploaded documents with their respective status will be displayed here:

![image](../images/projects/mcu/document-queue.png)

### View triaged document

Uploaded document can then be viewed and selected to generate a response to it:

![image](../images/projects/mcu/generate-response.png)

### Generate response

When a user clicks `Generate response`, it will take them to the `Document Response` page, where the full document then can be viewed:

![image](../images/projects/mcu/document-response.png)

At the bottom of the page, a user can choose which model and personas should be used for generating a response, as well as select which knowledge should be accessed:

![image](../images/projects/mcu/select-models.png)

### Review and customise generated response

The system allows a user to view the generated response:

![image](../images/projects/mcu/agent-response.png)

The user can also view sources and citations used to generate the response:

![image](../images/projects/mcu/sources.png)

The user can then prompt the language model to adjust the response:

![image](../images/projects/mcu/customise-response.png)

Once a user is happy with the response, it can then be finalised:

![image](../images/projects/mcu/document-complete.png)

## Github Repos

- [DEFRA/coreai-mcu-core](https://github.com/DEFRA/coreai-mcu-core)