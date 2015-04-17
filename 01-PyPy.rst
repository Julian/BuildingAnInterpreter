====
PyPy
====

Besides being the most prominent example of an interpreter written in
RPython, having been the interpreter for which RPython was developed, we
will also use PyPy in order to actually translate our own interpreter,
which we will discuss later.

Below are brief instructions for installing and running PyPy.


Installing PyPy
---------------

PyPy can be installed by following the instructions at:

http://pypy.org/download.html

or:


OS X
####

    $ brew install pypy


Windows
#######

Windows binaries are available at the above link.


Linux
#####

PyPy should be available in your Linux distro's package manager.

Alternatively, tarballs are available at:

https://github.com/squeaky-pl/portable-pypy

which should work across distributions.


Running PyPy
------------

As with CPython, the REPL is accessible via ``pypy`` at a terminal.

Feel free to play around a bit if you've never done so already, whereby
you'll (hopefully!) notice nothing amiss.
