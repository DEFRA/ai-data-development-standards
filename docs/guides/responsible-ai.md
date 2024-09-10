---
tags:
  - Azure
  - Evaluation
---

# Responsible AI

## GOV.UK

Please refer to: [GovUK Responsible AI Toolkit](https://www.gov.uk/government/collections/responsible-ai-toolkit)

## Microsoft

Please refer to [Azure AI Responsible AI](../tech-radar/tools/azure-ai/responsible-ai.md)

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