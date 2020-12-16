# Requirements 
`pip install isort autoflake black pytest pyprof2calltree`
if on mac also: 
`brew install qcachegrind`

# EVERY PROJECT EVERY PUSH 
- `python -m isort *.py`
- `python -m autoflake --in-place --remove-unused-variabes *.py`
- `python -m black *.py`

Assuming we have a folder "tests":
- `python -m pytest -vrs tests`

## Profiling 
- `python -m cProfile -o profile.cprof file_to_profile.py`
- `pyprof2calltree -k -i profile.cprof`
