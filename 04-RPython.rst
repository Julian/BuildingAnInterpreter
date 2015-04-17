RPython
=======

There is an introduction to RPython in the `RPython language documentation
<http://rpython.readthedocs.org/en/latest/rpython.html>`_,
which explains what subset of Python constitutes valid RPython.

It is *not* critical that you read that page from top to bottom (yet, probably
not even at all at least for today).

The most important things to remember from the start are:

* You cannot mix objects of "un-unifyable" types in the same collection.
  Lists must contain all strs, or all instances of a class. It does not
  have to be the same exact class, the classes just have to be unifyable,
  which means that there is some common base class between them (at the
  RPython level this cannot be ``object``, so you can't mix ints with
  instances of your RPython class). Relatedly, you can mix ``None`` with some
  types, but not others. See the documentation page above for details.

* Most of the (Python) standard library (and similarly external modules) are
  not valid RPython. There is a separate, smaller `RPython standard library
  <http://rpython.readthedocs.org/en/latest/rlib.html>`_. With a few
  exceptions, it comprises the external RPython code you have available.

* Write simple code and it is likely to be easily converted to RPython even if
  it is invalid. If your code is complex, it will likely need untangling once
  you try and translate it.

Unfortunately the most important rule though is the one at the top of the
documentation, that if it translates, it's valid, and if it ain't, it isn't.


Two Specific Notes
------------------

Besides the above, there are two specific things which you might find
surprising initially that are worth learning upfront.

Firstly, the RPython toolchain will expect an entry point similar to
the one expected when writing C code. By default it will expect a
function called ``main`` in the file you pass to the ``rpython`` binary
(explained shortly). The function should return an ``int`` as ``main``
would in C. This is where the translation process which we'll discuss
(as well as your interpreter) will begin.

Secondly, the ``assert`` statement in RPython has special semantics. Whereas in
Python it is generally used to describe invariants that should never be false
in a piece of code, in RPython it *additionally* makes an assertion of that
invariant *to the toolchain itself* so that the toolchain can make use of that
information.

As a quick example, you'll likely soon notice once we begin writing our parser
that the toolchain will complain during translation if for instance you have::


    class Parent(object):
       ...

    class ChildA(Parent):
       attr_only_on_this_child = 12

    class ChildB(Parent):
       ...

and you try and access ``attr_only_on_this_child`` even if you only pass in
instances of ``ChildA``. You will need to tell the toolchain that you are
*guaranteeing* that only instances of ``ChildA`` will be there, and not
``ChildB``, and the way to do so is by saying ``assert isinstance(myinstance,
ChildA)``, which signals that exact fact.
