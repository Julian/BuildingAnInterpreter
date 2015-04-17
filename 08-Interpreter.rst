==============
An Interpreter
==============

Finally! It's time to actually execute some code.

We have bytecode, which essentially is a tape-like sequence of instructions
that we will interpret. We've casted our (potentially) complicated language
into a sequence of simple stack manipulation operations.

Some of you will find interpretation to be the most "fun" part of the process.
We need to implement the appropriate stack manipulation for each bytecode we
wish to interpret.


The Main Loop
-------------

Interpreting bytecode will take place within a main loop similar to the
`CPython VM's main loop <https://github.com/python/cpython/blob/2.7/Python/ceval.c#L1102>`_
or `PyPy's main loop <https://bitbucket.org/pypy/pypy/src/a69d4a5a96389cf0d5478ff9fad898e52153fa92/pypy/interpreter/pyopcode.py?at=default#cl-177>`_.
A giant loop that simply performs whatever bytecode instruction is at the
current position we're processing.

A program counter should keep track of that position within our
bytecode. Our interpreter will read sequentially through the bytecode
sequence, possibly moving the program counter to some other position (if
you implement a jump or conditional expression in a later exercise).

Along with a stack (an RPython list), we'll implement our plan.

For each bytecode instruction that your compiler produces, implement the
appropriate stack manipulation.

.. note::

    Depending on the particular bytecode instruction, you may find it
    difficult at this stage to write tests without simply making tests
    that assert about the internal state of your stack.

    Try this out.

    You might find it more reasonable as you progress to write tests that use
    the print bytecode mentioned below instead once you have a working entry
    point.


Print
-----

Until you implement full support for function calls, it will likely be useful
to special-case the print() instruction by giving it its own bytecode.

Note that you cannot use the ``sys`` module in RPython for the most part, nor
do you have access to ``open``. You may write to stdout directly via
``os.write`` by passing in an fd of ``1`` for stdout.

You may also want to check out the ``streamio`` module from the RPython
standard library which can provide some provisional file-like support for
RPython.

Once you have the ability to print values, you can begin print-debugging your
own interpreter!


Wrapper Objects
---------------

Much like Python, our toy Snap language allows you to print objects that aren't
necessarily strings.

It becomes useful to start applying OOP techniques to objects at your *language
level* and not just at the RPython level for your interpreter. For example, you
may have an integer object which represents a Snap integer within your runtime.

The convention is to call objects like these ``W_Integer``, for example, where
the ``W_`` prefix indicates that this object is a wrapper object.

Once you have wrapper objects, you can begin to encapsulate Snap
features on each wrapper object. A ``W_Integer`` for example may have
a ``.to_string`` method in RPython, which returns a ``W_String`` Snap
string. Your ``PRINT`` bytecode might then be implemented by simply
delegating to this method in order to produce a string, which you can
implement polymorphically on each Snap type you might have.

If you begin to implement method calls in Snap, your ``to_string`` method might
further become a Snap-accessible method that you can call on your Snap objects
directly if so chosen.
