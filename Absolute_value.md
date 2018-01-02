# Absolute values with Python

Let's start with "What is an absolute value?". Absolute value for a real_number or "modulus" for a real_number is the non negative representation of that real_number. The sign of the real_number does not matter. Zero can have absolute value as well. In math absolute value is presented by |<real_number>| = <non-negative_real_number>. Your takeway for your tests from here is that any valid x you pass to your absolute_value function will always return non-negative response.

Why do we need to have such absolute value? Answer is simple for simple cases and complex for complex cases. For example if you have a thermometer and you record values of its measurements, the numbers in your data can be positive, zero or negative. If you need to have the actual delta between values, you need to always present it as an absolute difference, going positive/negative doesn't matter. The amount of the difference is always an absolute value.

There are several different approaches of how absolute value can be calculated. It of course depends on the real_number itself.
Let's say we have a simple integer value for which we would like to have the asbolute value. We can use conditional construction to check if our simple integer is less than zero. If so we have to return non-negative integer. If our simple integer value is zero, we return zero, because |0|=0 . If the integer argument is more than zero we just return it.
