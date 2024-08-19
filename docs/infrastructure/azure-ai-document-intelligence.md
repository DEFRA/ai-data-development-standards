# Azure AI Document Intelligence

> **NOTE**: The use of Azure AI Document Intelligence is **under review** for the use in production.

## Overview

Azure AI Document Intelligence (previously known as Azure Form Recognizer) is a Azure service that uses machine learning models to enable data extraction and classification of business documents. The service is able to extract data from both structured and unstructured documents such as:

- Forms
- Receipts
- Identity documents

## Possible use cases

- Extraction of invoice forms into structured data to be stored in database
- Classification of document for triage / routing purposes

## In depth

Document Intelligence offers two ways of interacting with the service:

1. REST API - Also offers SDKs for:
    - .NET
    - JavaScript
    - Java
    - Python
2. Studio: This is a web-based tool that allows you to upload documents, view the extracted data, and train custom models.

All responses from are returned in JSON format and also include a confidence score for each extracted field. This means we could use the confidence score to determine whether human review is required for the extracted data where the use case allows.

### Data Extraction

#### Prebuilt Models

Azure AI Document Intelligence provides prebuilt models for extracting information from common document types such as invoices, receipts, and business cards.

As these are prebuilt models, they don't require any training data or custom configuration. However, their accuracy on our specific documents may vary and will need evaluated on a case-by-case basis. The structure of the extracted data could also fluctuate / unpredictable depending on the document.

Also a external risk if Microsoft changes the model and the output structure changes.

#### Custom Models

There is also the facility to train custom models using our own forms and documents. This may allow us to achieve higher accuracy over the prebuilt models and allow complete control over the structure the extracted data is returned in.

Models can be trained using a small number of sample documents (as few as 5) and can be retrained as more data becomes available. This could be useful for extracting data from forms that are unique to Defra.

### Document Classification

Document intelligence can also be used to classify documents based on format / content. Classification always requires a custom model to be trained but only a small number of sample documents are required.

The minimum number of sample documents required to train a classification model is 5 across 2 different classes (document types).

## Demo

## Current Recommendation

Continue with assessment - See [**Next Steps**](#next-steps)

## Next steps

- Find simple forms to test the prebuilt models
- Assess whether we need to train a custom for complex forms
- Find a suitable complex form to test the custom model training capabilities