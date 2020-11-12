# Homework one : 04-11-2020

## Python Continous Integration 

Assignment: Use the following tools to make a CI pipeline for your Python GitHub project

Many CI DevOps tools exists, since you are working on GitHub, use
- GitHub Actions

The purpose of the following python tools
### isort
`pip install isort`
`isort file.py`
automatically sorts imports 

### flake8
`pip install flake8`
`flake8 path/to/repo/ --statistics`
or see `flake8 --help`
automatically "lints" (i.e. checks syntax and provides instructions on how to clean) code
here specifically for pep8

### autoflake 
`pip install autoflake`
`autoflake --in-place example.py`
removes unused imports. 
WARNING: will only work for simple imports, won't work for e.g. `from os import path, sep`

### black 
`pip install black`
`black /path/to/repo/`
enforces formatting that conforms to pep8. note isort will overwrite black8.

### mypy
`pip install mypy`
`mypy program.py`
optional static (lint-like) type checker
####dynamic vs static typing:
a function without type annotations is considering to be <i>dynamically typed</i>: 
```
def greeting(name):
    return "hello " + name 
```
these mypy will <b>not</b> check - if you want it to, you need to add <i>type hints</i>:
```
def greeting(name: str) -> str: 
    return "hello " + name
```
which is now <i>statically</i> typed - mypy can use the hints to detect incorrect usages of the function 
such as `greeting(3)` (which would not be noticed using the dynamically typed version).

### autolint
couldn't really find this one?
https://github.com/magnars/autolint ? 

### pytest 
`pip install pytest`
`pytest /path/to/repo/`
pytest runs all files of the form `test__*.py` or `*__test.py` in the current directory and sub-directories

## Example project
See example project at : 
https://github.com/puckvg/ActionsCalculatorLibrary

### Comments: 
- Didn't vibe with black because it wouldn't tell you a line number which didn't match formatting 
- Haven't yet gotten into static typing so didn't use mypy
- Couldn't find autolint 
- What about autopep8?

## TODO Future projects
- Always test code on commit to GitHub
- Always test lint on commit to GitHub
- Always in-force format on commit

Never accept code in your codebase that is not formatted in a defined style and fails unittests.
This becomes especially important when working with others, and rules like these makes it easier to collaborate.

## Useful references 
- https://github.com/charnley/rmsd
- https://docs.pytest.org/en/stable/getting-started.html
- https://pre-commit.com/ 



