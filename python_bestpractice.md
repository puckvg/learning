
# Notes on best practice when writing Python code other people should read or use. 

- Use auto-formatting (black, isort, flake8), and stop worrying about Python format.

- Use pathlib for files and directories, instead of strings.

- Use logging module for print statements, with different levels of logging (debug, info, error etc)

- Use unit tests for every file, and test every function. Test for code coverage. 

- Avoid deep nested sub-module structures. Harder for others to use and find.

- Never have global dependencies in functions. If a function dependent on a variable, it should be set in the definition. 

