# Python From the Ground Up

Hi there, and welcome to Python From the Ground Up,
my attempt at explaining python, its features and its ideosyncracies by talking
about how, conceptually, the underlying parts of it work.

## What this series is about
The thing that always struck me about python is that it has alot of behaviour
that seems weird or counter-intuitive, but makes perfect sense once you understand
what it actually translates to under the hood.

This series is my attempt to walk through python in a way where everything gets explained as you go,
so programmers can understand these concepts better and learn the language and its advanced features faster.

This series will assume the following:
* A basic knowledge of python (syntax, basic concepts like functions and classes)
* That you have experience with some other language
* A good understanding of generic programming concepts, such as object-oriented programming, data structures

## What this series is not about
While I may occasionally talk about specific examples involving CPython, this series will not
go into detail on the workings of the interpreter itself.
Rather, it will try to explain the processes underlying concepts
in terms of pseudocode or even just more python.

## A note on recursive dependencies
Many of the concepts explained here will recursively rely on each other.
For example, a dictionary is a class, but classes are implemented using dictionaries (more on that later).
In these cases, you should understand that the interpreter is providing certain things (eg. the dict class)
that breaks the recursive dependency. However I will still explain things in terms of pure python, as
even in these cases you can generally think about the concepts in terms of each other, even though
such a pure system would be impossible.
