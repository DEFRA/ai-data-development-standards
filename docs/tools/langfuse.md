# LangFuse

## Overview

## Traces
A `trace` typically represents a single request or operation. It contains the overall input and output of the function, as well as metadata about the request, such as the user, the session, and tags.

Each trace can contain multiple `observations` to log the individual steps of the execution.

`Observations` are of different types:

- Events are the basic building blocks. They are used to track discrete events in a trace.
- Spans represent durations of units of work in a trace.
- Generations are spans used to log generations of AI models. They contain additional attributes about the model, the prompt, and the completion. For generations, [token usage and costs](https://langfuse.com/docs/model-usage-and-cost) are automatically calculated.