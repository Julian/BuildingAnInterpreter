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
