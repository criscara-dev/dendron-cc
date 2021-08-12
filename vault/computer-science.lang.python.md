---
id: d0beeb2a-dfb0-4c99-a139-67d642d08c3e
title: Python
desc: ''
updated: 1628343295940
created: 1617975101150
---

- [[PROJECT-BLUEPRINT]]
- Magic methods in a class:

```python
class Person:
    def __init__:
        pass
    def __str__: # use it to turn an object into a string
        pass
    def __repr__: # use for debugging: to recreate an unambigous repr-esantation of the object
    pass
```

## Composition in Python:

Instead of inheritance, better to use:
Composition!
In the child class don't use `super().__init__(<parent-arg>)`
but instead use ` *args # as argument intead of the one you need`
ref. https://www.udemy.com/course/rest-api-flask-and-python/learn/lecture/15927826#overview


## serialization

Process of 'flatten out' complex data (Data Structures) in a 'stream' in order to send/store this data ( as a binary, json, xml, yaml, etc.).
De-serialize is the opposite process to open and read the complex data.


## testing Python
[piramid tests](https://martinfowler.com/articles/practical-test-pyramid.html)

## Pythonic ways

instead of:

```python
def count_negatives(nums):
    """Return the number of negative numbers in the given list.
    
    >>> count_negatives([5, -1, -2, 0, 3])
    2
    """
    # n_negative = 0
    # for num in nums:
    #     if num < 0:
    #         n_negative = n_negative + 1
    # return n_negative

    return len([num for num in nums if num < 0])

    # or even better, a sum of True values
    # return sum([num < 0 for num in nums])

print(count_negatives([5, -1, -2, 0, 3]))
```

[check here for more clues](https://www.kaggle.com/colinmorris/loops-and-list-comprehensions?utm_medium=email&utm_source=gamma&utm_campaign=thirty-days-of-ml&utm_content=day-5)


## [Zen of Python](https://en.wikipedia.org/wiki/Zen_of_Python)
> Readability counts.
Explicit is better than implicit.