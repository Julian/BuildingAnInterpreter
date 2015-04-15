A Start
=======

The "language" we'll implement is exceedingly simple. We'll aim for a
small initial goal:

    * variable assignment
    * string and integer literals
    * simple output to stdout

We'll call our interpreter (and language) Snap. Don't bother Googling
it.

The simple syntax for our language will lead us to aim directly for the
following small program::

    foo = 2 + 7
    bar = "cat" + "dog"
    print(foo)
    print(bar)

where we will want::

    $ snap example.snap

containing the above program to put the appropriate output on stdout.

We'll tackle our interpreter in 3 phases, a small parser, a small
corresponding bytecode compiler, and then a small corresponding bytecode
interpreter.

After that we'll embellish!


Initial Setup
-------------

Fork the repo containing these instructions, which you can use to house
your interpreter (feel free to just use the local clone if you prefer).
You can use the CyCy repo as a guide (or any of the other interpreters)
for all of the basic steps below. We will assume a similar layout for
the remainder of this session.

It lives at https://github.com/Julian/BuildingAnInterpreter.

We'll be creating a fairly typical Python package, despite its purpose,
so create the package layout you are comfortable with, along with a
virtualenv for your project (be sure to use ``-p pypy`` if you do).
Install the ``rpython`` toolchain which we'll shortly need from PyPI.

We'll be writing tests! Install your favorite test runner as well, and follow
along with the next step, where we finally start writing our parser.
