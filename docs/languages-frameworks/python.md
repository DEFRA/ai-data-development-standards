---
tags:
  - Python
---

# Python Development Standards

<div class="grid cards" markdown>

-   :material-radar:{ .lg .middle } __Techradar__

    ---

    Trial

-   :material-thumb-up:{ .lg .middle } __Recommendation__

    ---

    Please see notes below

</div>

> **NOTE**: Python has yet to be adopted by the tools authority as "Strategic Adopt". Python is **only** for the use in AI and Data and is not permitted outside of this remit. Please adhere to the following [Defra Software Development Standards](https://defra.github.io/software-development-standards/) for building digital services. For the use of Python in data science and data engineering, please refer to the [Data Analytics & Science Hub (DASH)](../data/dash.md) section. The DASH platform provides tools and software that enable users to carry out data analysis with powerful compute, accessible support and secure access to data.

## Overview

> **NOTE**:
> Python should **not** be used for frontend development. The GDS Service Manual has information on using client-side JavaScript. For server-side JavaScript we use Node.js. The principles of [Progressive Enhancement](https://www.gov.uk/service-manual/technology/using-progressive-enhancement) should be used for the design and build of frontend digital services. The building of backend services in Python is limited to the use in AI and Data. A javascript first approach should be taken and where there is adquate justification, Python can then be used.


This manual is designed to aid developers in writing clear and consistent Python code across projects.

Python is the tool of choice for developing artificial intelligence tools (e.g., traditional machine learning and large language models) and is adopted widely by the data analytics and science community. The Adoption of Python for software development in the AI and Data area will allow for easier deployment of artificial intelligence tools. Python’s prevalence in the artificial intelligence world is highlighted by the fact that OpenAI use Pytorch as their standard framework for training their large language models (e.g., GPT models).

## Code Formatting

We use `Black` to format our code. `Black` is an opinionated formatter that follows PEP 8. Where `Black` and PEP 8 don't express a view, we defer to the Google Python style guide. Use these as references unless something is explicitly mentioned here.

To add a new rule or exception, please create a pull request against this repo.

## Maximum Line Length

The maximum line length is 120 characters. This deviates from PEP 8's preferred 79 characters due to:

- Most developers now use IDEs that can handle longer lines
- GDS developers often work with nested JSON objects, ORM model definitions/queries, and long error/URL strings

## Linting

### Ruff

We recommend using the Ruff command-line checker as an all-in-one lint, code style, and complexity checker.

#### How to use Ruff

1. Add the `Ruff` module to your 'dev' or 'test' requirements/dependencies.
2. Run it alongside your unit tests.

#### Ruff Ignores

Ruff can ignore particular lines or files. You can specify rule exemptions per directory or file.

### Common Configuration

An example `pyproject.toml` configuration:

``` toml title="pyproject.toml" linenums="1"

[tool.black]
line-length = 120

[tool.ruff]
line-length = 120
target-version = "py311"
select = [
    "E",  # pycodestyle
    "W",  # pycodestyle
    "F",  # pyflakes
    "I",  # isort
    "B",  # flake8-bugbear
    "C90",  # mccabe cyclomatic complexity
    "G",  # flake8-logging-format
]
ignore = []
exclude = [
    "migrations/versions/"
]
```

## Environments

When a new Python project is created, a new virtual environment should be used to store the dependencies required to run the code within the project. Virtual environments are used to manage dependencies and ensure that the correct version of a dependency is available to the correct project. Additionally, they make the project reproducible as the virtual environment can be recreated on any machine. A text file ‘requirements.txt’ can be created with the command pip freeze > requirements.txt after the required dependencies are installed into the virtual environment. This file contains a list of the installed dependencies and their version number. This file should be included in the Git repository for the project. It should be updated if new dependencies are added to the project. After downloading a project, this file will be used to recreate the virtual environment on another machine, ensuring the code functions correctly. GDS Way suggest using pyenv-virtualenv to manage virtual environments.

- Use `Pyenv` to manage different Python versions
- Use `pyenv-virtualenv` to manage virtual Python environments
- Use `direnv` to manage environment variables

## Dependencies

### Applications

For applications or script bundles:

1. Pin specific versions of dependencies for reproducibility
2. Keep dependencies up-to-date for security
3. Document the upgrade process in your `README`
4. Use `requirements.txt` for pinned dependencies
5. Consider using `pip-tools` for larger projects
6. Use `requirements-dev.txt` for development/testing dependencies

### Libraries

For installable Python repositories:

1. Use `setup.py` to specify dependencies and version ranges
2. Update `setup.py` when testing new versions outside the existing range
3. Use PEP 440 git references for dependencies not available on PyPI
4. Specify testing dependencies in `tox.ini` (if using Tox) or `requirements-dev.txt`

#### Recommended Libraries (through a data science lens)

- **Data processing:** Pandas, NumPy, SciPy
- **Data visualisation / dashboarding:** Matplotlib, Seaborn, Plotly, Dash  
- **ML/AI/NLP:** Scikit-learn, LightGBM, XGBoost, CatBoost, PyTorch, TensorFlow, Keras, Langchain, LlamaIndex, Transformers, NLTK, spaCy
- **Statistics:** Statsmodels
- **HTTP:** requests, PyCrypto
- **Scraping and automation:** Selenium, BeautifulSoup
- **Unit testing:** pytest
- **Logging:** logging

## References

- [GDS Python Standards](https://gds-way.digital.cabinet-office.gov.uk/manuals/programming-languages/python/python.html#python-style-guide)
- [Style Guide](https://google.github.io/styleguide/pyguide.html)
- [ML Universal Guide](https://developers.google.com/machine-learning/guides)