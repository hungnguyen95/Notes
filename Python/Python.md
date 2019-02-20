# Python notes

## zip() function
The `zip()` function takes:

- **iterables** - can be built-in iterables (like: list, string, dict), or user-defined iterables (object that has __iter__ method).

The `zip()` function returns an iterator of tuples based on the iterable object.

- If no parameters are passed, zip() returns an empty iterator
- If a single iterable is passed, zip() returns an iterator of 1-tuples. Meaning, the number of elements in each tuple is 1.
- If multiple iterables are passed, ith tuple contains ith Suppose, two iterables are passed; one iterable containing 3 and other containing 5 elements. Then, the returned iterator has 3 tuples. It's because iterator stops when shortest iterable is exhaused.

```python
numberList = [1, 2, 3]
strList = ['one', 'two', 'three']

# Two iterables are passed
result = zip(numberList, strList)

# Converting itertor to set
resultSet = set(result)
print(resultSet)
#Print: {(2, 'two'), (3, 'three'), (1, 'one')}
```