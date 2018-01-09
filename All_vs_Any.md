# All vs. Any

What all() and any() functions do and why I want to talk about their relation between each other.

Similarities:
+ Both are built_in python functions;
+ Both take one argument;
+ Both take argument that is iterable;
+ Both return boolean objects;

Differences:
+ all() acts as logical AND for objects in an iterable;
+ any() acts as logical OR for objects in an iterable;

all() implementation:
```python
def all(iterable):
    for element in iterable:
        if not element:
            return False
    return True
```

any() implementation:
```python
def any(iterable):
    for element in iterable:
        if element:
            return True
    return False
```

Comparison between feeded arguments and return values:

|   if the iterable contains              |  any() returns |  all() returns  |
|-----------------------------------------|---------|---------|
| only True values                       |  True   |  True   |
| only False values                        |  False  |  False  |
| one True value (rest are False) |  True   |  False  |
| one False value (rest are True) |  True   |  False  |
| is empty                          |  False  |  True   |
