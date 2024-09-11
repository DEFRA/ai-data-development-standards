# CrewAI

<div class="grid cards" markdown>

-   :material-radar:{ .lg .middle } __Techradar__

    ---

    Assess

-   :material-thumb-up:{ .lg .middle } __Recommendation__

    ---

    Continue with assessment

</div>

> **NOTE**: The use of CrewAI is **under review**.

## Overview

CrewAI is a framework designed to coordinate role-playing, autonomous AI agents. It facilitates collaborative intelligence, enabling agents to work together efficiently on complex tasks.

## Core Concepts

### Agents

An agent is an autonomous unit programmed to:

- Perform tasks
- Make decisions
- Communicate with other agents

Think of an agent as a member of a team, with specific skills and a particular job to do. Agents can have different roles like 'Researcher', 'Writer', or 'Customer Support', each contributing to the overall goal of the crew.

### Tools

- CrewAI comes with a number of tools, details of these can be found in the
docs
- In addition to these tools, CrewAI supports all Langchain tools

``` py linenums="1"
from crewai_tools import SerperDevTool, \
                         ScrapeWebsiteTool, \
                         WebsiteSearchTool
```

#### SerperDevTool

Integrates with Serper, an external service that allows you to search Google.

#### ScrapeWebsiteTool

Allows your agent to access a given URL and extract its contents

*Scrape any website:*

``` py linenums="1"
scrape_tool = ScrapeWebsiteTool()
```

*Using the website_url property, restricts the scrape to a specific website:*

``` py linenums="1"
check_for_flooding_scrape_tool = ScrapeWebsiteTool(
  website_url="https://check-for-flooding.service.gov.uk/
)
```

#### WebsiteSearchTool

- RAG over a website
- Semantic search over the contents of a website

#### Custom tools
- Load customer data
- Tap into previous conversations etc…

#### Assigning tools

- Tools can be assigned to either an Agent or a Task
- If you give the tool to an agent, that agent can use the tool for any task
- If you give the tool to a task, the agent will only use that specific tool when performing that task
- Task tools override agent tools.
    - For example, if an agent has 10 tools and a task has 3 tools, the agent will only use the 3 tools defined in the task


### Tasks

Tasks can specify additional rules including:

- requiring human input
- output formats (JSON, File etc)
- set a callback
- set a context
- …

``` py linenums="1" title="output as a JSON file"
venue_task = Task(
  description="..."
  human_input=True,
  output_json=VenueDetails,
  output_file="venue_details.json",
  # Outputs the venue details as a JSON file
  agent=venue_coordinator
)
```

``` py linenums="1" title="human input required and asyncronous"
some_task = Task(
  description="..."
  human_input=True,
  async_execution=True,
  agent=logistics_manager
)
```

#### Context

To enable a task to use the output from other tasks use the context property:

``` py linenums="1"
research_task = Task(
.....
)

profile_task = Task(
.....
)

resume_strategy_task = Task(
  description=(
    "...."
  ),
  expected_output=(
    "...."
  ),
  output_file="tailored_resume.md",
  context=[research_task, profile_task],
  agent=resume_strategist
)
```

### Memory

The ability for the agent to remember what it has done, learn from it and apply that knowledge to future executions.

#### Memory System Components

1. Long term memory

    - Exists even after the crew finishes
    - This memory is stored in a local DB
    - Can be used in future tasks
    - Leads to self-improving agents

2. Short term memory

    - Only exists during a crew execution
    - Every time you kickoff a crew this memory is blank
    - Memory - knowledge, activities and learnings are shared between all agents in the crew
    - This is shared during the crew execution, before providing ‘task completion’

3. Entity memory

    - Only exists during the crew execution
    - Stores the subjects that are being discussed
        - for example a specific company, the entity could store what it knows about that company in the entity memory

4. Contextual Memory
    - Maintains the context of interactions by combining ShortTermMemory, LongTermMemory, and EntityMemory, aiding in the coherence and relevance of agent responses over a sequence of tasks or a conversation.

#### Enabling memory for the crew

- Enabling memory for the crew is simple. Just set memory=True when defining the crew:

``` py linenums="1"
crew = Crew(
  agents=[support_agent, support_quality_assurance_agent],
  tasks=[inquiry_resolution, quality_assurance_review],
  verbose=2,
  Crew AI R&D 3
  memory=True
)
```

## Approaches / Tips

### Role playing

Roles, goals and back story should be focused. Use specific keywords to refine output. 

### Focus

Use multiple agents each with a specific focused role.

### Tools

Using too many tools can overload the LLM. Only provide the tools absolutely required for the task.

### Collaboration

Allow the agents to talk to each other. This produces a better outcome.

### Guardrails

CrewAI puts guardrails in place at the framework level (Further research is required around this).

### QA Agent

- It is recommended to add a quality assurance agent at the end of the chain.
- This can improve the output significantly

For example:

``` py linenums="1"
support_quality_assurance_agent = Agent(
  role="Support Quality Assurance Specialist",
  goal="Get recognition for providing the "
  "best support quality assurance in your team",
  backstory=(
    "You work at crewAI (https://crewai.com) and "
    "are now working with your team "
    "on a request from {customer} ensuring that "
    "the support representative is "
    "providing the best support possible.\n"
    "You need to make sure that the support representativ
    "is providing full"
    "complete answers, and make no assumptions."
  ),
  verbose=True
)
```

## References

- [Agents](https://docs.crewai.com/core-concepts/Agents/)
- [Tasks](https://docs.crewai.com/core-concepts/Tasks/)
- [Tools](https://docs.crewai.com/core-concepts/Tools/)
- [Memory](https://docs.crewai.com/core-concepts/Memory/)
