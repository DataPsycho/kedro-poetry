# Reproduce the Project

Poetry version is used:
```
datapsycho@dataops:~/.../KedroPoetry$ poetry version
kedropoetry 0.1.0
```

Create a new project:
```
poetry new --src KedroPoetry
cd KedroPoetry
```

Here is the tree structure:
```
.
├── poetry.lock
├── pyproject.toml
├── README.rst
├── src
│   └── kedropoetry
│       ├── __init__.py
│       └── __pycache__
│           └── __init__.cpython-38.pyc
└── tests
    ├── __init__.py
    ├── __pycache__
    │   ├── __init__.cpython-38.pyc
    │   └── test_kedropoetry.cpython-38-pytest-7.1.2.pyc
    └── test_kedropoetry.py
```

Lets remove older pytest and install new pytest.
```
poetry remove pytest --dev
poetry add pytest --dev
```
That will install latest pytest in the repo. Now lets add the following lines into the `pyproject.toml` file and run the pytest command:

```
[tool.pytest.ini_options]
pythonpath = ["src"]
```

Run the Pytest:
```
poetry run pytest .
```
The sample test should pass.

Let's add some packages:

```
poetry add pandas
```

Lets open the `pyproject.toml` and add kedro as dependency as follows:

```
kedro = {version = "~0.18.2", python = ">=3.8,<3.11"}
```

The dependencies section should looks like as follows:
```
[tool.poetry.dependencies]
python = "^3.8"
pandas = "^1.4.3"
kedro = {version = "~0.18.2", python = ">=3.8,<3.11"}

```

The `poetry update` command can be run to install the package.

Now lets activate the Poetry Virtual environment as follows:
```
poetry shell
```

Now using kedro lest initiate a new project:
```
(kedropoetry-9Q6y5a-v-py3.8) datapsycho@dataops:~/.../KedroPoetry$ kedro new --starter=pandas-iris

Project Name
============
Please enter a human readable name for your new project.
Spaces, hyphens, and underscores are allowed.
 [Iris]: sample-project

The project name 'sample-project' has been applied to: 
- The project title in /home/datapsycho/PythonProject/KedroPoetry/sample-project/README.md 
- The folder created for your project in /home/datapsycho/PythonProject/KedroPoetry/sample-project 
- The project's python package in /home/datapsycho/PythonProject/KedroPoetry/sample-project/src/sample_project

A best-practice setup includes initialising git and creating a virtual environment before running 'pip install -r src/requirements.txt' to install project-specific dependencies. Refer to the Kedro documentation: https://kedro.readthedocs.io/
```

Let's see the tree structure:
```
(kedropoetry-9Q6y5a-v-py3.8) datapsycho@dataops:~/.../KedroPoetry$ tree . -L 1
.
├── poetry.lock
├── pyproject.toml
├── README.md
├── sample-project
├── src
└── tests
```
As usual kedro created its own folder structure in `sample-project` and do not take the advantage of already inplace `src` directory and `pyproject.toml` file.

Conclusion:
When there is alrady a `src` directory and a `pyproject.toml` file in place, `Kedro` does not take the advantage of it. So developers have to copy paste a lot of things to restructure the project again to make it compatible with Poetry. The expected result should be Kedro will recognize that there is a `src` folder and `toml` project setup file in the root and 
- Kedro will add conf, data, docs, logs, notebooks on the current root
- sample_projects inside of the `src` directory
- the tests are in the `tests` directory
- The `requirements.txt` and `setup.py` can be deleted later by the developer as it is not needed in the project using poetry

So may be there can be a --poetry flag which can be used while in the project poetry is going to be used. Or there can be --src-in-root (if exist) or --use-pyproject (if exist) flag to make it more general when there is already a src directory and a pyproject.toml file.





