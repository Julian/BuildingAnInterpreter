=============
A Snap Parser
=============

Our first goal is to be able to successfully parse our goal into an
`AST <http://en.wikipedia.org/wiki/Abstract_syntax_tree>`_. An AST is a
programmatic representation of the structure of our program (which you
may have encountered via the ``ast`` module in Python).

Create a test file named ``test_example.py`` in your Snap package's test
folder, and create a test case within it. The test case will guide us along the
way towards at least being able to parse our example program. We'll handle
interpreting it soon.

We wish to parse the example program we had earlier::

    foo = 2 + 7
    bar = "cat" + "dog"
    print(foo)
    print(bar)

into an AST. There are numerous ways to represent the above source
code as an AST. Feel free to be a bit creative, but as a sampling, we
will represent the first sample of this piece of code as the following
(RPython) AST::

    Assign("foo", BinaryOperation("+", Integer(2), Integer(7)))

See if you can write a (failing) test case that asserts that the result
of parsing the above source should produce that AST. You'll need to
create a few classes corresponding to the various node types in your
AST.

.. note::

    Hooray! Tests don't need to be valid RPython, they won't run within
    your interpreter, so you have freedom to write them however you'd
    like.

We'll follow roughly typical TDD (if you're unfamiliar, don't worry too
much about it), so create the function that will parse your AST and make
your example test pass by simply giving it a fake implementation that
simply returns the (overall) AST we want for our example program.

We'll now build out our parser in small chunks until we successfully can
parse the above program.


RPly and an Actual Start to Our Parser
--------------------------------------

We need a way to actually parse our source code into the AST we've
just designed. There are a number of paths forward. The most obvious
is to implement a parser ourselves "by hand", but there are a number
of tools that exist to make this easier. The RPython standard library
has the ``rpython.rlib.parsing`` module, but we'll use the `RPly
<https://rply.readthedocs.org/en/latest/index.html>`_ library, which has
a slightly nicer API.

There are only two pages of documentation, so have a quick read through
them to see how you will likely want to proceed. You'll likely find
consulting the CyCy parser helpful minutes as well but try and wait
until you're off the ground.

You may want to consult additional resources on EBNF notation if this is
your first exposure to it. For our purposes superficial knowledge should
suffice until you want to extend the parser (later) as well, so it can
also wait.

Break down the things you will need to parse into smaller chunks, write
unit tests for the (small AST) that will result from parsing them, and
then implement enough of a parser to make your tests pass, extending
your implementation at each step. For example, you might want to take
the following path:

    * Parse integer literals
    * Parse sums of integers
    * Parse assignment of an integer
    * Parse the assignment of a sum
    * ...

slowly building up your parser via your unit tests.


Translation
-----------

Let's not forget about translation! Generally speaking, you should
*not* necessarily attempt to translate your code not after change or
commit (it's tedious and long as you'll soon notice). Rely on your tests
to check that your code works, then simply periodically attempt to
translate, and fix any invalid RPython you've introduced since the last
time.

Let's set up enough to be able to translate our project and check if
it's valid RPython. Recall that translation only operates on code paths
that are actually reachable from your entry point, which we're about to
define, and only on objects *after* import-time, where you're able to
fully utilize Python.

.. note::

    If you forgot about the type unification requirement of RPython,
    you'll likely see your first translation error with your AST nodes!

    These errors are cryptic, but if you stare at them and trust them
    for long enough, they often are clear enough to point you in the
    right direction.

    See if you can solve your error.


More About non-RPython
----------------------

With the above note again in mind about translation operating only on
code paths your interpreter will traverse (and not ones used only during
tests or debugging), it's useful to set yourself up for easy debugging
of your parser.

Give your AST nodes a helpful ``__repr__`` and see what you can do in
regular Python to help yourself as you go along.

You've likely already implemented ``__eq__`` and ``__ne__`` methods on
your nodes to get your tests to pass. These too do not need to be valid
RPython since they likely will only be used in tests.


A Look Ahead
------------

In a coming exercise we'll begin writing a simple `REPL
<http://en.wikipedia.org/wiki/Read%E2%80%93eval%E2%80%93print_loop>`_,
similar to the Python REPL (or CyCy REPL).

You might want to look ahead and implement a simple REPL that simply can
display the AST for the inputted source code, which may be helpful as
you move forward.
