# Functions

Functions are the most basic constructs in your program. In short, functions contain code that
will run when they are called. When you call a function, a few things happen:

1. Arguments are collected and resolved
2. A local scope is created and populated with arguments
3. The code contained is executed

Let's look at each of these in detail.

## Arguments

Arguments seem relatively simple at first. You specify some arguments like:
```python
def add(x, y):
	return x + y
```
then you can call it with the same number of arguments:
```pycon
>>> add(2, 3)
5
```

So far, so good, right?

### Keyword arguments

Now, depending on how much python you've done, you might know about some more advanced forms,
such as optional arguments. Suppose we want to add an optional third argument to our add function:
```pycon
>>> def add(x, y, z=0):
... 	return x + y + z
... 
>>> add(2, 3)
5
>>> add(2, 3, 4)
9
>>> add(2, 3, z=4)
9
```

We've added an argument `z` which takes a default value of `0`.
Note that we can refer to z either by position (as the third positional arg) or by name (with `z=4`).

As a matter of fact, we can do this regardless of whether the argument is optional or not:
```pycon
>>> add(x=1, y=2)
3
>>> add(3, y=1, z=1)
5
```
Note that once we start giving keyword args, we can't give any further positional args:
```pycon
>>> add(x=3, 1, 1)
  File "<stdin>", line 1
SyntaxError: non-keyword arg after keyword arg
```
And we can't give the same argument in two ways:
```pycon
>>> add(2, 3, y=1)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: add() got multiple values for keyword argument 'y'
```

### Variadic arguments

Now, suppose we didn't care about specifying arguments by name (seriously, who names a variable `z`?),
but we do want to be able to do `add(1,2)`, `add(2,4,6,8)`, `add(1,2,3,4,5,...)`? We can do that!

If you give an argument name beginning with a `*`, this will capture all remaining positional arguments
and put them into a tuple under that name:
```python
def add(x, y, *numbers):
	total = x + y
	for number in numbers:
		total += number
	return total
```
You'll often see these called "positional args", "\*args", or "variadic args".
While it's a common convention to name the argument `*args`,
you should pick a more descriptive name if possible. It's the `*` which makes it special, not the exact name.

Note that in python2, it is a syntax error to put any further args (with one exception, see below)
after the `*args` in the argument list.
In python3, you may put arguments with defaults after the `*args`, which are "keyword-only arguments".

### More keyword arguments

The `*args` construct captures all positional arguments, but not keyword arguments.
There is an equivilent construct for keyword arguments, often called `**kwargs`. Though as with `*args`
you may use any argument name, it must be prefixed with `**` to indicate it should capture all remaining
keyword arguments. These arguments are put into a dict, which maps the keyword to the value, for example:
```pycon
>>> def thing_with_options(required_arg, optional_arg='not given', **extras):
... 	print "first arg:", required_arg
... 	print "second arg:", optional_arg
... 	print "extras:", extras
... 
>>> thing_with_options("foo", bar="baz")
first arg: foo
second arg: not given
extras: {'bar': 'baz'}
>>> thing_with_options("foo", optional_arg="bar", xyz=123, abc="baz")
first arg: foo
second arg: bar
extras: {'xyz': 123, 'abc': 'baz'}
```

### Putting it all together

So we now see there are two kinds of arguments: those specified by position and those specified by keyword.
Arguments may or may not have default values, though this has no bearing on whether they are specified by
keyword or position. Finally, any extra positional or keyword args may be captured by `*args` and `**kwargs`
respectively.

By combining `*args` and `**kwargs`, we can make a fully general function definition that will take **any**
combination of positional and keyword arguments. Note that a `**kwargs` argument **is** valid after a `*args`
(that's the exception mentioned above).

We can use this to create a fully generic "wrapper" function
that simply passes arguments on to the inner function:
```python
def wrapper(func, *args, **kwargs):
	print "wrapped"
	return func(*args, **kwargs)
```
You'll find this as a common idiom in many places where one function must call another without knowing anything
about its arguments.
