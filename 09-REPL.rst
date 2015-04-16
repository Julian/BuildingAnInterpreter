======
A REPL
======

As a somewhat fun additional exercise, let's implement an interactive
interpreter which parses, compiles and executes your code in the same way the
Python interpreter does.

Most of the work here is straightforward.

You do not have access to the ``code`` (Python) stdlib module, so it will be up
to you to manually implement a simple execution loop.

Simply repeatedly read a line, and run it through the appropriate steps within
your interpreter.

Here specifically you might want to take a look at the ``streamio`` module if
you did not before.


Final Thoughts
--------------

To come full circle, verify that you can run the example that we set out to
interpret.

It should produce our expected output.
