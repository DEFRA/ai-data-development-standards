# Crew AI Project Setup

## Walkthrough

1. Create a new Conda virtual environment

    - `conda create --name flooding_alerts python=3.11`

2. Activate the Conda virtual environment

    - `conda activate flooding_alerts`

3. Create a new CrewAI project

    - `crewai create flooding_alerts`

    This creates a ready made crewai project structure, containing agents, tasks, config etc.

4. Open the new CrewAI project in VSCode

    - `cd flooding_alerts`
    - `code .`

5. Enter the poetry shell

    - `poetry shell`

6. Update the poetry config so that the virtual environment resides within the current project directory

    - `poetry config virtualenvs.path ./.venv`

7. Add additional dependencies

    - `poetry add crewai_tools`
    - `poetry add langchain_community`

8. Lock the poetry dependency versions

    - `poetry lock`

9. Install dependencies

    - `poetry install`

10. Run the CrewAI project

    - `poetry run flooding_alerts`