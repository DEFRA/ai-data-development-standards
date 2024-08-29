---
tags:
  - Azure
  - Evaluation
---

# Responsible AI

## GOV.UK

Please refer to: [GovUK Responsible AI Toolkit](https://www.gov.uk/government/collections/responsible-ai-toolkit)

## Microsoft

### Azure Responsible AI tools

- **Prompt Shields**: detects and blocks prompt injection attacks, including a new model for identifying indirect prompt attacks before they impact your model

- **Groundedness Detection** (coming soon): Detects “hallucinations” in model outputs

- **Safety system messages** (coming soon): Steers your model’s behavior toward safe, responsible outputs

- **Safety evaluations** (available in preview): Assesses an application’s vulnerability to jailbreak attacks and to generating content risks

- **Risks and safety monitoring** (available in preview in Azure OpenAI Service): Helps understand what model inputs, outputs, and end users are triggering content filters to inform mitigations
    
### Responsible AI Toolbox

Responsible AI Toolkit is a suite of tools providing a collection of model and data exploration and assessment user interfaces and libraries that enable a better understanding of AI systems. These interfaces and libraries empower developers and stakeholders of AI systems to develop and monitor AI more responsibly, and take better data-driven actions.

The tools are available via open source on [GitHub](https://github.com/microsoft/responsible-ai-toolbox?culture=en-us&country=us) or can be accessed through the Azure Machine Learning platform.

- [Repo: Responsible AI Toolbox](https://github.com/microsoft/responsible-ai-toolbox?culture=en-us&country=us)
- [azureml-examples/sdk/python/responsible-ai at main · Azure/azureml-examples](https://github.com/Azure/azureml-examples/tree/main/sdk/python/responsible-ai)
- [8 part blog post - Getting started with Azure Machine Learning Responsible AI components](https://techcommunity.microsoft.com/t5/ai-machine-learning-blog/getting-started-with-azure-machine-learning-responsible-ai/ba-p/3746948?WT.mc_id=aiml-114127-cxa)

### Responsible AI Dashboard

Enables you to easily flow through different stages of model debugging and decision-making. This customizable experience can be taken in a multitude of directions, from analyzing the model or data holistically, to conducting a deep dive or comparison on cohorts of interest, to explaining and perturbing model predictions for individual instances, and to informing users on business decisions and actions.

![image](../images/responsible-ai-dashboard.png)

### Responsible AI Scorecard

An Azure Machine Learning Responsible AI scorecard is a PDF report that's generated based on Responsible AI dashboard insights and customizations to accompany your machine learning models. You can easily configure, download, and share your PDF scorecard with your technical and non-technical stakeholders to educate them about your data and model health and compliance, and to help build trust. You can also use the scorecard in audit reviews to inform the stakeholders about the characteristics of your model.

## General

- Resources and information: [Evaluating LLM systems: Metrics, challenges, and best practices](https://medium.com/data-science-at-microsoft/evaluating-llm-systems-metrics-challenges-and-best-practices-664ac25be7e5#feb9)

- Checklist for AI deployment: [USAID Artificial Intelligence Ethics Checklist.pdf](https://www.usaid.gov/sites/default/files/2023-12/USAID%20Artificial%20Intelligence%20Ethics%20Checklist.pdf)

In addition to examining the model from various perspectives, such as data source, model design, and production environment, the best practice is to evaluate the LLM application using pre-designed questions in different RAI categories, including:

| Potential harm categories | Harm description with sample evaluation datasets |
| :------------------------ | :---------------------------------------------- |
| Harmful content           | :material-circle-medium: Self-harm<br>:material-circle-medium: Hate<br>:material-circle-medium: Sexual<br>:material-circle-medium: Violence<br>:material-circle-medium: Fairness<br>:material-circle-medium: Attacks<br>:material-circle-medium: Jailbreaks: System breaks out of instruction, leading to harmful content|
| Regulation                | :material-circle-medium: Copyright<br>:material-circle-medium: Privacy and security<br>:material-circle-medium: Third-party content regulation<br>:material-circle-medium: Advice related to highly regulated domains, such as medical, financial and legal<br>:material-circle-medium: Generation of malware<br>:material-circle-medium: Jeopardizing the security system |
| Hallucination             | :material-circle-medium: Ungrounded content: non-factual<br>:material-circle-medium: Ungrounded content: conflicts<br>:material-circle-medium: Hallucination based on common world knowledge|
| Other categories          | :material-circle-medium: Transparency<br>:material-circle-medium: Accountability: Lack of provenance for generated content (origin and changes of generated content may not be traceable)<br>:material-circle-medium: Quality of Service (QoS) disparities<br>:material-circle-medium: Inclusiveness: Stereotyping, demeaning, or over- and underrepresenting social groups<br>:material-circle-medium: Reliability and safety |

## References

- [GovUK Responsible AI Toolkit](https://www.gov.uk/government/collections/responsible-ai-toolkit)
- [Empowering responsible AI practices | Microsoft AI](https://www.microsoft.com/en-us/ai/responsible-ai)
- [Prompt Shields](https://techcommunity.microsoft.com/t5/ai-azure-ai-services-blog/azure-ai-announces-prompt-shields-for-jailbreak-and-indirect/ba-p/4099140)
- [Groundedness Detection](https://techcommunity.microsoft.com/t5/ai-azure-ai-services-blog/detect-and-mitigate-ungrounded-model-outputs/ba-p/4099261)
- [Safety system messages](https://learn.microsoft.com/en-us/azure/ai-services/openai/concepts/system-message)
- [Safety evaluations](https://techcommunity.microsoft.com/t5/ai-ai-platform-blog/introducing-ai-assisted-safety-evaluations-in-azure-ai-studio/ba-p/4098595)
- [Risks and safety monitoring](https://techcommunity.microsoft.com/t5/ai-azure-ai-services-blog/introducing-risks-amp-safety-monitoring-feature-in-azure-openai/ba-p/4099218)
- [Responsible AI Scorecard](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-responsible-ai-scorecard?view=azureml-api-2)