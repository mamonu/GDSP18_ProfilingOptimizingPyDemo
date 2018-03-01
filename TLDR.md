# Profiling & the need for speed (the TLDR version)

Some suggestions for faster python code. Please feel free to add , just do a pull request when done! 


## what seems to be the problem ?

GIL:

http://wiki.python.org/moin/GlobalInterpreterLock

http://www.dabeaz.com/python/UnderstandingGIL.pdf


The GIL (Global Interpreter Lock) is how Python allows multiple threads to operate at the same time. 

Python’s memory management isn’t entirely [thread-safe](https://en.wikipedia.org/wiki/Thread_safety) , so the GIL is required to prevent multiple threads from running the same Python code at once.



dynamic typing: 

Higher level languages such as Python are optimized for ease of use and readability

This means that the programmer can leave many details to the runtime environment like:

      specifying variable types
      memory allocation/deallocation, etc.

The upside is that, compared to low-level languages, Python is typically faster to write, less error prone and easier to debug

The downside is that Python is harder to optimize — that is, turn into fast machine code — than languages like C or Fortran




## profiling 

Overview: Optimize what needs optimizing

You can only know what makes your program slow after first getting the program to give correct results, 


If found to be slow, profiling can show what parts of the program are consuming most of the time (finding the bottlenecks)


In short:

    Get it running.
    Test it's doing what its supposed to do.
    time/profile if slow.
    Optimise.
    Repeat from 2. 




## writing performant python 

dont use nested loops

use vectorized code and try not to iterate each row of a dataframe for example

lists are convenient but slow

use numpy / scipy as much as possible . DO NOT REINVENT THE WHEEL

if numpy / scipy does not have what you want roll your own functions.

if you have loops that run many times, 
(eg. at each word , at each row of a big dataframe)
be very careful of what you put in that loop. This is the hotspot that needs optimizing. Inside that inner loop have only things that are calculated and not other things that dont change and could be easily located outside the loop. 

instead of loops try to use 'map' 


## jit compilers 



Numba gives you the power to speed up your applications with high performance functions written directly in Python. With a few annotations, array-oriented and math-heavy Python code can be just-in-time compiled to native machine instructions, similar in performance to C, C++ and Fortran, without having to switch languages or Python interpreters.

Numba works by generating optimized machine code using the [LLVM compiler infrastructure](https://en.wikipedia.org/wiki/LLVM) at import time, runtime, or statically (using the included pycc tool). Numba supports compilation of Python to run on either CPU or GPU hardware, and is designed to integrate with the Python scientific software stack.

Numba is useful for the kind of functions involving calculations with arrays and loops that are not availiable in Numpy for example. Sometime Numba is not able to speed up a function and then falls back to normal python.

## memoizing
Memoization is a way of caching the results of a function call.

If a function is memoized, evaluating it is simply a matter of looking up the result you got the first time the function was called with those parameters. 

This is recorded in the memoization cache. If the lookup fails, that’s because the function has never been called with those parameters. 
Only then do you need to run the function itself.


## Cython 

[Cython](cython.org) implements a superset of the Python language with which you are able to write C and C++ modules for Python. Cython also allows you to call functions from compiled C libraries. Using Cython allows you to take advantage of Python’s strong typing of variables and operations.


## Useful references



