---
layout: post
title: Using joblib.Memory to cache function results
date: 2023-10-22 08:56:00-0400
description: This example illustrates the usage of joblib.Memory with functions.
tags: Code
categories: PHD
giscus_comments: false
related_posts: false
toc:
  sidebar: left
---


If we need to call our function several time with the same input data, it is beneficial to avoid recomputing the same results over and over since it is expensive. `joblib.Memory` enables to cache results from a function into a specific location.

### Example

```python
from joblib import Memory
import time
import numpy as np

location = './cachedir'
memory = Memory(location, verbose=0)

rng = np.random.RandomState(42)
data = rng.randn(int(1e5), 10)


@memory.cache
def my_costly_compute_cached(data, column_index=0):
    """Simulate an expensive computation"""
    time.sleep(5)
    return data[column_index]



start = time.time()
data_trans = my_costly_compute_cached(data)
end = time.time()

print('\nThe function took {:.2f} s to compute.'.format(end - start))
print('\nThe transformed data are:\n {}'.format(data_trans))

start = time.time()
data_trans = my_costly_compute_cached(data)
end = time.time()

print('\nThe function took {:.2f} s to compute.'.format(end - start))
print('\nThe transformed data are:\n {}'.format(data_trans))
```

### Outputs

```text
The function took 5.08 s to compute.

The transformed data are:
 [ 0.49671415 -0.1382643   0.64768854  1.52302986 -0.23415337 -0.23413696
  1.57921282  0.76743473 -0.46947439  0.54256004]

The function took 0.03 s to compute.

The transformed data are:
 [ 0.49671415 -0.1382643   0.64768854  1.52302986 -0.23415337 -0.23413696
  1.57921282  0.76743473 -0.46947439  0.54256004]
```

### References

- [https://joblib.readthedocs.io/en/latest/auto_examples/memory_basic_usage.html](https://joblib.readthedocs.io/en/latest/auto_examples/memory_basic_usage.html)