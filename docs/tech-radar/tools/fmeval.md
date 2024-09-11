---
tags:
  - AWS
  - Evaluation
---

# FMEval

<div class="grid cards" markdown>

-   :material-radar:{ .lg .middle } __Techradar__

    ---

    Hold

-   :material-thumb-up:{ .lg .middle } __Recommendation__

    ---

    Refrain from investing; monitor developments

</div>

> **NOTE**: FMEval currently supports **only** Python 3.10, which is now in the "security fixes only" stage of its life cycle and binary installers are no longer provided for it.

## Overview

The `fmeval` library is designed to evaluate Large Language Models (LLMs) to help select the best one for specific tasks. It supports tasks like open-ended generation, text summarization, question answering, and classification. The library includes algorithms to evaluate models based on accuracy, toxicity, semantic robustness, and prompt stereotyping.

## Key Features

**1. ModelRunner Interface**

This encapsulates the logic for invoking different types of LLMs, simplifying interactions within the evaluation algorithms. It supports Amazon SageMaker Endpoints, JumpStart models, and can be extended for other models.

**2. Evaluation Algorithms**

The library provides built-in algorithms for various tasks, allowing users to evaluate models based on specific criteria.

**3. Custom Datasets**

Users can evaluate models using custom datasets by configuring a DataConfig object.

## Installation

> **NOTE**: Currently `fmeval` supports only Python 3.10, due to its dependancy on ray 2.23.0 used for graph generation.

To install `fmeval`, use Python 3.10 and run:

```
pip install fmeval
```

## Usage

- **ModelRunner:** Create a `ModelRunner` to perform LLM invocations.

- **Evaluation:** Use the provided evaluation algorithms to assess your model's performance.

```py title="Example" linenums="1"
from fmeval.eval_algorithms.toxicity import Toxicity, ToxicityConfig

eval_algo = Toxicity(ToxicityConfig())
eval_output = eval_algo.evaluate(model=model_runner)
```

## Telemetry

Tracks usage of AWS-hosted LLMs, which can be disabled by setting DISABLE_FMEVAL_TELEMETRY to true.

## Development and Extensions:

- **Custom Algorithms:** New evaluation algorithms and metrics can be added using `Transform` and `TransformPipeline` classes.

- **Transform Class:** Encapsulates logic for transforming individual records in a dataset.

- **TransformPipeline Class:** Applies a sequence of `Transform` objects to a dataset.

## Troubleshooting:

- **Windows:** Fix errors related to `ray.init()` by installing Python from the official website.

- **Mac:** Resolve Rust compiler errors by installing specific Rust versions.

- **Memory Issues:** Address out-of-memory errors by adjusting the `PARALLELIZATION_FACTOR` environment variable.

## References

- [FMEval](https://github.com/aws/fmeval/tree/main)
- [Python 3.10](https://www.python.org/downloads/release/python-31014/)