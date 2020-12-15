
# profiling 

Setup a profiling cmd. Personally I use a format of

  pyprofile python script.py --args
  
so I can change the python src
  
google hints

- python -m cProfile -s tottime -o profile.cprof
- pyprof2calltree
- kcachegrind

