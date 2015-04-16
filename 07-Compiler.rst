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
