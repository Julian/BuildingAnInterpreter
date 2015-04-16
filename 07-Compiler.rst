===============
A Snap Compiler
===============

We have the foundation of a parser, which means we now can convert
source code into programmatic representations of an AST.

Our objective now is to be able to compile an AST into bytecode. Our bytecode,
roughly speaking, is similar to bytecode you likely will have encountered at
least in passing with Python.

It is a sequence of bytes which program (what will become) our virtual machine,
in an analogous way to how machine code is a sequence of bytes that programs
our CPU.

Our compiler will take an AST and walk it to produce a single stream of
bytecode that encapsulates the instructions we need to execute.

Designing bytecode languages is an art unto itself, but our fundamental goal
during compilation is to create a simple language that manipulates a `stack
<http://en.wikipedia.org/wiki/Stack_machine>`_ which we will maintain as our
VM.

For example, here is a human-readable rendering of what adding two numbers
might look like for our VM::

    LOAD_CONST 0
    LOAD_CONST 1
    BINARY_ADD

The interpretation of these three instructions are:

    * load a particular constant, whose ID is 0 (we'll maintain constants in an
      array, so this will be an index specifying the constant within the
      array), then push its value onto our stack

    * do the same for another constant with ID 1

    * pop the top two entries off the stack, perform addition, and push the
      result back onto the stack

The non-human readable version should use simple bytes to represent the
actual instructions ultimately, which the VM will then dispatch on.


First Steps
-----------

Your compiler should operate on an AST, and ultimately will produce bytecode: a
simple sequence of bytes which our upcoming VM will execute.

In fairly typical OO style, generally it will simply delegate to each of
your AST nodes for compilation, in which case each node will know how to
compile itself.

We'll need to store state during our compilation process. We need at
least a place to store variables and one to store constants that the
bytecode we generate will reference.

For example, a simple constant node like the one we'll need for integers
or strings should compile itself by simply registering itself as a
constant on an object that is keeping context for the compilation.

In the bytecode we will emit, loading this constant for use on the stack
will consist of accessing an appropriate element within the registered
constants list.

Write your first compiler test! There are (at least) two ways to test
your compiler. One is in integration with your parser, by asserting that
some inputted source code parses and then compiles into an expected
piece of bytecode. The other is to test direct compilation of an AST.

Try out both methods and see which you find easier to read. You may also
find it convenient to have a way to produce human-readable bytecode
dumping for your bytecode, which should produce output similar to
our first example above, or to what you'd get out of the `dis module
<https://docs.python.org/2/library/dis.html>`_.
