# zip() function
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

# super() in python
The reason we use super is so that child classes that may be using cooperative multiple inheritance will call the correct next parent class function in the Method Resolution Order (MRO).

```python
class ChildB(Base):
    def __init__(self):
        super().__init__() 
```

### What difference is there actually in this code?

```python
class ChildA(Base):
    def __init__(self):
        Base.__init__(self)

class ChildB(Base):
    def __init__(self):
        super(ChildB, self).__init__()
        # super().__init__() # you can call super like this in Python 3!
```

The primary difference in this code is that you get a layer of indirection in the `__init__` with `super`, which uses the current class to determine the next class's `__init__` to look up in the MRO.

### If Python didn't have `super`

Here's code that's actually closely equivalent to `super` (how it's implemented in C, minus some checking and fallback behavior, and translated to Python):

```python
class ChildB(Base):
    def __init__(self):
        mro = type(self).mro()             # Get the Method Resolution Order.
        check_next = mro.index(ChildB) + 1 # Start looking after *this* class.
        while check_next < len(mro):
            next_class = mro[check_next]
            if '__init__' in next_class.__dict__:
                next_class.__init__(self)
                break
            check_next += 1
```

Written a little more like native Python:

```python
class ChildB(Base):
    def __init__(self):
        mro = type(self).mro()
        for next_class in mro[mro.index(ChildB) + 1:]: # slice to end
            if hasattr(next_class, '__init__'):
                next_class.__init__(self)
                break
```

### `super()` and MRO
For example, we have next hierarchy structure:

```
    A
   / \
  B   C
   \ /
    D
```

In this case MRO of D will be (only for Python 3):

```python
In [26]: D.__mro__
Out[26]: (__main__.D, __main__.B, __main__.C, __main__.A, object)
```

Let's create a class where `super()` calls after method execution.

```python
In [23]: class A(object): #  or with Python 3 can define class A:
...:     def __init__(self):
...:         print("I'm from A")
...:  
...: class B(A):
...:      def __init__(self):
...:          print("I'm from B")
...:          super().__init__()
...:
...: class C(A):
...:      def __init__(self):
...:          print("I'm from C")
...:          super().__init__()
...:  
...: class D(B, C):
...:      def __init__(self):
...:          print("I'm from D")
...:          super().__init__()
...: d = D()
...:
I'm from D
I'm from B
I'm from C
I'm from A

    A
   / ⇖
  B ⇒ C
   ⇖ /
    D
```

So we can see that resolution order is same as in MRO. But when we call `super()` in the beginning of the method:

```python
In [21]: class A(object):  # or class A:
...:     def __init__(self):
...:         print("I'm from A")
...:  
...: class B(A):
...:      def __init__(self):
...:          super().__init__()  # or super(B, self).__init_()
...:          print("I'm from B")
...:
...: class C(A):
...:      def __init__(self):
...:          super().__init__()
...:          print("I'm from C")
...:  
...: class D(B, C):
...:      def __init__(self):
...:          super().__init__()
...:          print("I'm from D")
...: d = D()
...:
I'm from A
I'm from C
I'm from B
I'm from D
```

We have a different order it is reversed a order of the MRO tuple.

```
    A
   / ⇘
  B ⇐ C
   ⇘ /
    D

# Algorithm:

# 1. Build an inheritance graph such that the children point at the parents (you'll have to imagine the arrows are there) and
#    keeping the correct left to right order. (I've marked methods that call super with ^)

#          A^
#       /  |  \
#     /    |    \
#   B^     C^    D^  I^
#        / | \  /   /
#       /  |  X    /
#      /   |/  \  /
#    E^    F^   G^
#     \    |    /
#       \  |  /
#          H
# (In this example, A is a child of B, so imagine an edge going FROM A TO B)

# 2. Remove all classes that aren't eventually inherited by A

#          A^
#       /  |  \
#     /    |    \
#   B^     C^    D^
#        / | \  /  
#       /  |  X
#      /   |/  \
#    E^    F^   G^
#     \    |    /
#       \  |  /
#          H

# 3. For each level of the graph from bottom to top
#       For each node in the level from right to left
#           Remove all of the edges coming into the node except for the right-most one
#           Remove all of the edges going out of the node except for the left-most one

# Level {H}
#
#          A^
#       /  |  \
#     /    |    \
#   B^     C^    D^
#        / | \  /  
#       /  |  X
#      /   |/  \
#    E^    F^   G^
#               |
#               |
#               H

# Level {G F E}
#
#         A^
#       / |  \
#     /   |    \
#   B^    C^   D^
#         | \ /  
#         |  X    
#         | | \
#         E^F^ G^
#              |
#              |
#              H

# Level {D C B}
#
#      A^
#     /| \
#    / |  \
#   B^ C^ D^
#      |  |  
#      |  |
#      |  |  
#      E^ F^ G^
#            |
#            |
#            H

# Level {A}
#
#   A^
#   |
#   |
#   B^  C^  D^
#       |   |
#       |   |
#       |   |
#       E^  F^  G^
#               |
#               |
#               H

# The resolution order can now be determined by reading from top to bottom, left to right.  A B C E D F G H
```