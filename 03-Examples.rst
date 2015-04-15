Some Example Interpreters
-------------------------

As we move forward with our own interpreter, it will be helpful to have other
interpreters that we can reference if and when we are stuck, either with design
or implementation.

To start with, besides CyCy which we have already cloned, let's retrieve two
more interpreters written with RPython.

First, let's clone the "official" RPython example interpreter. It lives `on
BitBucket <https://bitbucket.org/pypy/example-interpreter/>`_.

.. note::

    To clone it, you'll need the mercurial VCS. If you do not already
    have mercurial, you can install it as any other Python package via

        $ pip install --user mercurial

    In this instance personal recommendation is to install mercurial
    globally, but feel free to install it into a virtualenv as well if
    you wish.

Execute::

    $ hg clone https://bitbucket.org/pypy/example-interpreter

to grab a copy of it. Take the same quick flip through the source code as you
did through CyCy. You'll hopefully notice some surface-level similarities
because CyCy was written via the same strategy of occasional glancing at the
example interpreter for inspiration.

As we progress through writing our own interpreter, you now have at least a
pair of interpreters to reference. In order of complexity, we have:

    * the example interpreter implementing a toy language similar to our own

    * CyCy implementing (the small subset of) C

While we're looking at interpreters, clone the Topaz and PyPy interpreters as
well. These interpreters are full-blown (real-world) projects with all of the
considerations that brings, so they come with huge additional levels of
complexity. Try not to be flustered by it, we clone them now for two reasons:

    * familiarity with Python might in some way provide additional guidance if
      you can manage to find the associated implementation in PyPy

    * both repos are large -- we might want to take quick looks at PyPy later
      in the day, so we'll clone them up front. Feel free to leave the clone
      running in the background while we proceed.
