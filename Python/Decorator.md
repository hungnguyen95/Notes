# Function in python
In Python, everything is object. Functions in Python are first-class objects which means that they can be referenced by a variable, added in the lists, passed as arguments to another function etc.

## Functions can be referred by variables

Basic function in python:

```python
def say_hello(): 
  print("Hello")
  
# say_hello() Output: Hello
```

We can make another variable say_hello2 to refer to say_hello function as shown below:

```python
say_hello2 = say_hello

print(say_hello2()) # Output: Hello 
```

With the above statement say_hello2 and say_hello are pointing to the same function definition, and execution of both will produce same result

## Functions can be passed as arguments to another function

```python
def say_hello(say_hi_func):
  print("Hello")
  
  say_hi_func()

def say_hi():
  print("Hi")
  
say_hello(say_hi)
#Output:   Hello
#          Hi
```

In the above example, we have passed say_hi function as an argument to say_hello function. say_hi is referenced by say_hi_func variable. Inside say_hello, say_hi_func is called which references say_hi and it prints “Hi”.

## Functions can be defined inside another function

```python
def say_hello():
  print("Hello")
  
  def say_hi():
    print("Hi")
    
  say_hi()  
    
say_hello() # Output: Hello
            #         Hi 
  
say_hi() # Gives error  
```

In the above example, say_hi function has been defined inside say_hello function. It is valid in python. When we call function say_hello, say_hi function gets defined inside say_hello function and it is called inside say_hello() function.

If we try to call say_hi outside say_hello function, Python will give error because say_hi does not exists outside of say_hello function.

## Functions can return references to another function

```python
def say_hello():
  print("Hello")
  
  def say_hi():
    print("Hi")
    
  return say_hi

say_hi_func = say_hello() # Prints Hello and returns say_hi function which gets stored in variable say_hi_func

say_hi_func() # By say_hi function is refered by say_hi_func variable so calling say_hi_func will call say_hi. 
              # It will print Hi
```

In the above example, say_hello function returns a reference for say_hi function. Returned function reference is assigned to say_hi_func. Thus say_hi_func also starts pointing to say_hi function.

Another example with function arguments

```python
def say_hello(hello_var):
  print(hello_var)
  
  def say_hi(hi_var):
    print(hello_var + " " + hi_var)
    
  return say_hi
  
say_hi_func = say_hello("Hello") # Print Hello and returns say_hi function which gets stored in say_hi_func variable

say_hi_func("Hi") # Call say_hi function and print "Hello Hi"
```

Variable hello_var is accessed even outside the say_hello function because say_hi function was defined inside say_hello function so it can access all the variables of say_hello function. This is called as closure.

With the understanding of functions, now let’s understand decorators.

# Decorators

**Decorators are callable objects which are used to modify functions or classes.**

Callable objects are objects which accepts some arguments and returns some objects. functions and classes are examples of callable objects in Python.

Function decorators are functions which accepts function references as arguments and adds a wrapper around them and returns the function with the wrapper as a new function.

Let’s see this with an example:

```python
def decorator_func(some_func):
  # define another wrapper function which modifies some_func
  def wrapper_func():
    print("Wrapper function started")
    
    some_func()
    
    print("Wrapper function ended")
    
  return wrapper_func # Wrapper function add something to the passed function and decorator returns the wrapper function
    
def say_hello():
  print ("Hello")
  
say_hello = decorator_func(say_hello)

say_hello()

# Output:
#  Wrapper function started
#  Hello
#  Wrapper function started
```

In the above example, decorator_func is the decorator function which accepts some_func, a function object, as an argument. It defines a wrapper_func which calls some_func but it also executes some code of it’s own.

This wrapper_func is returned by our decorator function and it’s stored in say_hello variable. Thus now say_hello refers to wrapper_func which has some extra code apart from calling the function which was passed as argument. Thus in another way we can say that, decorator function modified our say_hello function and added some extra lines of code in it. This is what decorator are. The output is the modified say_hello function with extra print statments

## Python syntax for decorator

```python
@decorator_func
def say_hell():
    print 'Hello'
```

```python
def say_hello():
    print 'Hello'
say_hello = deocrator_func(say_hello)
```

Here, decorator_func will added some code on top of the say_hello function and will return the modified function or the wrapper function.

**Functions with arguments and decorators**

Consider the below example for function with parameters and decorators:

```python
def decorator_func(say_hello_func):
  def wrapper_func(hello_var, world_var):
    hello = "Hello, "
    world = "World"
    
    if not hello_var:
      hello_var = hello
    
    if not world_var:
      world_var = world
      
    return say_hello_func(hello_var, world_var)
  
  return wrapper_func

@decorator_func
def say_hello(hello_var, world_var):
  print hello_var + " " + world_var
  
say_hello("Hello", "")
```

Here, we have defined say_hello function with two arguments and @decorator_func. Inner function of decorator_func i.e. wrapper_func must take exactly the same number of argument as say_hello function.

Here. @decorators_func validates if the passed parameters are not empty and none, if they are, it calls say_hello function with default arguments.