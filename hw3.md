

# Homework 3

- I, a Python developer, sometimes need to be able to speed-up calculation
- I need a function that will utilize all the cpu's on my node
- I also would like to know the progress of this task, when it will finish
- I want to re-use this function, for many tasks, such as KRR HP search


```Python
def square_and_add(x, y=5):
    return x*x + y

xs = [1, 2, 3, 4, 5, 6, 7]

results = parallel(square_and_add, xs, n_cores=5)
```

- advanced: What if you need to change y?
- advanced: What if the func has many positional and keyword arguments, but you only want to parallelize one argument
- advanced: For multi arg: what if I wanted to first have a x_list and run in through parallel() and then a y_list and then a z_list?
- advanced: Use it to do hyper-parameter optimization of a sklearn model


## Resources

multiprocessing, tqdm, rich, functools,

