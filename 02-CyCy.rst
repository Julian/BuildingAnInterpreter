====
CyCy
====


Installing CyCy
---------------

Clone the CyCy repo which can be found at https://github.com/Magnetic/CyCy::

    $ git clone https://github.com/Magnetic/cycy
    $ cd cycy


Optional: Create a Virtualenv
#############################

Create a virtualenv which you will install CyCy into. If you have no other
preference on location, create one within the CyCy repo checkout::

    $ python -m pip install -U virtualenv
    $ virtualenv -p pypy venv

which will create a virtualenv (a directory) named ``venv`` in the checkout,
and will use the PyPy interpreter you've installed inside it.


Install CyCy as you would a normal Python package::

    $ venv/bin/pip install -e .

which should fetch all of the necessary dependencies.

Run ``venv/bin/rpython --help`` to confirm that the toolchain was successfully
installed, and you're on your way.


Alternate: Without a Virtualenv
###############################

If you do not wish to use a virtualenv, you can install CyCy and its
dependencies globally::

    $ pip install --user -e .

Follow along with the rest of the document by removing ``venv/bin/`` from the
beginning of commands, since your binaries will be available globally.

.. note::

    If after running the above command you cannot run ``rpython --help``,
    you likely do not have ``~/.local/bin`` on your shell's ``$PATH``.
    Follow the instructions of your shell on adding it (typically either
    to ``.bash_profile`` or ``.zshenv`` if you're using bash or zsh
    respectively).


Running & Translating CyCy
--------------------------

Take a quick look at the contents of the directories in front of you. We will
cover the types of components in them as we develop our own interpreter, but it
will be useful (and perhaps somewhat irresistible) to peek at what (generally
small amount of) code is there.

At a high level, the translation process will take the code you have and
translate it into an executable you can run standalone.

Before we do so, let's prove that the interpreter can simply be run as a normal
program *on top* of Python::

    $ venv/bin/pypy -m cycy

should present you with a CyCy REPL. Here you certainly *will* notice things
amiss (bonus points if you simply crash the interpreter).

But! If you run something simple, like::

    CC-> int main (void) { return 2 + 23; }

you should see::

    25

outputted below.

.. note::

    For both CyCy and the interpreter we will attempt to build, you may find it
    helpful to install the ``rlwrap`` \*nix utility and to use it as a wrapper
    around the REPL::

        $ rlwrap venv/bin/pypy -m cycy

    will give you a REPL with readline support provided via rlwrap.

    You can find it in homebrew or via your Linux package manager.

Now that we can run CyCy on top of Python, let's use the RPython toolchain to
create an executable that is *independent* of the Python runtime.

From the same directory (the root of the checkout):::

    $ venv/bin/rpython --output=cycy-nojit cycy/target.py

will produce a long stream of output that you might find interesting to stare
out.

After around 2 minutes of churning, out should pop an executable (in the
current directory) which you can run via::

    $ ./cycy-nojit

which should produce a REPL with (*roughly*) the same behavior as before.

This executable depends neither on Python nor the RPython toolchain::

    $ file ./cycy-nojit
    $ otool -L ./cycy-nojit

which should show you some basic information on the executable (and
specifically that it does not in fact try to link against Python). On Linux,
use ``ldd`` in place of ``otool -L``.
