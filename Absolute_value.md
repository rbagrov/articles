# Absolute values with Python

Let's start with "What is an absolute value?". Absolute value for a real_number or "modulus" for a real_number is the non negative representation of that real_number. The sign of the real_number does not matter. Zero can have absolute value as well. In math absolute value is presented by |<real_number>| = <non-negative_real_number>. Your takeway for your tests from here is that any valid x you pass to your absolute_value function will always return non-negative response.

Why do we need to have such absolute value? Answer is simple for simple cases and complex for complex cases. 
+ Typical simple case is when you have for example a thermometer. You record values of this thermometer measurements. The numbers you record in your dataset can be positive, zero or negative. If you need to have the actual delta between values (each one of your measurements), you need to always present it as an absolute difference. These values going positive or negative doesn't matter. The amount of the difference is always an absolute value.
+ More complex case is when calculating distance between two points (Euclidean distance) in space (Euclidean space). To bit simplify you can think of Euclidean distance as straight-line distance between two points in two dimentional plane. After you make calculations not typically important for the purpose of this article you will endup with having value in absolute form, which will represent your distance
 

There are several different approaches of how absolute value can be calculated. It of course depends on the real_number itself.
Let's say we have a simple integer value for which we would like to have the absolute value. We can use conditional construction to check if our simple integer is less than zero. If so we have to return non-negative integer. This is our only condition. In any other case we return what we got as an argument.

Here is how it looks:
```python
def absolute_value(any_real_number):
    '''
    Algorithm for calculating the absolute value
    '''
    if any_real_number < 0:
        return any_real_number * -1
    return any_real_number
```

Here is you can test if you calculations are correct:
```python
In [15]: for i in range(-1, 2):
    ...:     print('|{}|={}'.format(i, absolute_value(i)))
    ...:     
|-1|=1
|0|=0
|1|=1

```

Let's check if there is build in function that will save us from writting our own one everytime we need it.
The two most popular ones are abs() which is a build in function and fabs() which is imorted from math.

People tend to compare performance and argue which is better, but the truth is that they are quite different by their purpose. Important note here is that both functions are available in Python2 and Python3.
+ abs(x), where x can be any numeric type: int, float, complex, binary/hex/octal number
Example:
```python
In [1]: abs(5) # integer
Out[1]: 5
In [2]: abs(-5.0) # float
Out[2]: 5.0
In [3]: abs(3+4j) # complex
Out[3]: 5.0
In [4]: abs(0b010) # binary
Out[4]: 2
In [5]: abs(0o40) # octal
Out[5]: 32
In [6]: abs(0x20) # hex
Out[6]: 32
```
+ fabs(x), where x can be only float. Integers are converted to float.

Performance wise, working with the same number there is no significant difference. 

Here is a simple comparison with a float number in contrast to our own modulus implementation:
```python
In [1]: %timeit absolute_value(-5.0)
10000000 loops, best of 3: 167 ns per loop
In [2]: %timeit abs(-5.0)
10000000 loops, best of 3: 44.4 ns per loop
In [3]: %timeit fabs(-5.0)
10000000 loops, best of 3: 53.7 ns per loop
```

So first glance conclusion performance wise is not to re-inplement build in functions as you will never get them running faster, despite how un accurate our test might be. It clearly shows a difference.

If fabs(x) vs. abs(x), where x is integer, fabs() is expected do give a little slower return as it converts the argument to float.

Here is what happens:
```python
In [1]: %timeit fabs(5)
10000000 loops, best of 3: 83.6 ns per loop
In [2]: %timeit abs(5)
10000000 loops, best of 3: 42.7 ns per loop
```

Whatever you choose between abs() and fabs() might be for what arguments you expect to feed and what datatypes work for you in the function returns.

The fun part of this article is where we ask: Can we calculate absolute values without branching (using if/else). Yes we can and here is how:

```python
In [1]: x = -5
In [2]: y = (x >> 31)
In [3]: (x ^ y) - y
Out[3]: 5
```
This is one of the two methods. It uses bit XOR and value 31 is taking the sign bit of the integer (asuming it is 32 bit one).
