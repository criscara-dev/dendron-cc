---
id: d0beeb2a-dfb0-4c99-a139-67d642d08c3e
title: Python
desc: ''
updated: 1624285649512
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