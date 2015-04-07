=== Building blocks: Dicts, modules and scope ===

We start with the basics. It's easy enough to see what's going on with a block of code like this:
```python
print "Hello World"
```
and we think we understand that this does the same thing:
```python
greeting = "Hello World"
print greeting
```
But there's an important note here: what is this mysterious `greeting`, and how is it stored?

--- Global Scope ---
To find out, we run the `globals()` function, and look at what it returns.

You'll see a dict (that's a hash map for those of you new to python), with a bunch of keys.
We'll talk about most of these later. But what's important right now is that our "greeting"
variable is in the dict. You should see that it maps `"greeting": "Hello World"`.

You can also use `locals()` to see your local scope (more on that later).
But for now, note that when you run a simple piece of code directly,
the global scope **is** the local scope, and these two functions return the same thing.

Now try this:
```python
d = globals()
d["greeting"] = "Goodbye World"
print greeting
```
and sure enough, it will print "Goodbye World".
This globals() dict, in a very real way, is the entire collection of global variables
present in this module. Wait, module? Well, let me go off on a slight tangent.

--- Modules ---
If you've done some basic python, you'll have seen things like import statements.
You might have even written some of your own code in one file, and imported it for use in another.

These file-level constructs are known as **modules**.
Each module has a globals dict containing global variables,
along with some baked-in defaults like `__name__` and `__package__`.
These modules are, in fact, objects (more on that later) and when you access
attributes of them (eg. `sys.stdin`), you're actually accessing the module's global scope.

> But wait, what about all those functions and classes I define? Where do they go?
Good question. The trick to realise there is...

--- Everything is an assignment ---
Consider the following snippets:
```python
import sys
```
```python
def foo():
	return "bar"
```
```python
class Foobar(object):
	pass
```
They all have something in common - they add something to the local scope.
What I mean is, after you `import sys`, you can access the `sys` module.
And this works just like the `greeting` example above:
```python
>>> import sys
>>> globals()["sys"] is sys
True
```
`def` and `class` do the same thing - they first define a function or a class,
then they assign the defined function or class to the name you gave it. So something like:
```python
def add(x, y):
	return x + y
```
is equivilent to
```python
add = lambda x, y: x + y
```
(well actually there's some differences in the details of the function object, but more on that later)

