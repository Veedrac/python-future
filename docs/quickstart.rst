.. _quickstart-guide:

Quick-start guide
=================

You can use ``future`` to help to port your code from Python 2 to Python 3
today -- and still have it run on Python 2.

If you already have Python 3 code, you can instead use ``future`` to
offer Python 2 compatibility with almost no extra work.

Installation
------------

To install the latest stable version, type::

    pip install future

If you would prefer the latest development version, it is available `here <https://github.com/PythonCharmers/python-future>`_.


If you are writing code from scratch
------------------------------------

The easiest way is to start each new module with these lines::

    from __future__ import (absolute_import, division,
                            print_function, unicode_literals)
    from future.builtins import *

Then write standard Python 3 code. The :mod:`future` package will
provide support for running your code on Python 2.6 and 2.7 mostly unchanged.

- For more examples, see :ref:`overview`.
- For explicit import forms, see :ref:`imports`.
- For more details, see :ref:`what-else`.


To convert existing Python 3 code
---------------------------------

To offer backward compatibility with Python 2 from your Python 3 code,
you can use the ``pasteurize`` script. This adds these lines at the top of each
module::

    from __future__ import absolute_import
    from __future__ import division
    from __future__ import print_function
    from __future__ import unicode_literals

    from future.builtins import open
    from future.builtins import str
    # etc., as needed

    from future import standard_library
    standard_library.install_hooks()
    
and converts several Python 3-only constructs (like keyword-only arguments) to a
form compatible with both Py3 and Py2. Most remaining Python 3 code should
simply work on Python 2.

See :ref:`backwards-conversion` for more details.


To convert existing Python 2 code
---------------------------------

Start with the :ref:`automatic-conversion` page.

.. If you already know Python 3, start with the :ref:`automatic-conversion` page.
.. If you don't know Python 3 yet, start with :ref:`python3-essentials`.


.. _standard-library:

Standard library reorganization
-------------------------------

:mod:`future` supports the standard library reorganization (PEP 3108)
via import hooks, allowing almost all moved standard library modules to
be accessed under their Python 3 names and locations in Python 2::
    
    from future import standard_library
    with standard_library.hooks():
        import socketserver
        import queue
        import configparser
        from collections import UserList
        from itertools import filterfalse, zip_longest

        import html
        import html.entities
        import html.parser

        import http
        import http.client
        import http.server
        import http.cookies
        import http.cookiejar

        import xmlrpc.client
        import xmlrpc.server

``urllib`` currently requires an explicit import because the name clashes with
that on Python 2 and because Python's syntax does not allow imports of this
form with a dotted module name after ``as``::

    import future.moves.urllib.parse as urllib.parse

For submodules of ``urllib`` and other packages (like ``http``), this
alternative form is available::

    from future.standard_library import import_

    urllib = import_('urllib')
    import_('urllib.parse')
    import_('urllib.request')
    import_('urllib.error')

    response = urllib.request.urlopen('http://mywebsite.com')
    # etc.

For an explanation of these and other forms of imports from the standard
library, see :ref:`standard-library-imports`.


.. _py2-dependencies:

Python 2-only dependencies
--------------------------

If you have dependencies that support only Python 2, you may be able to use the
``past`` module to automatically translate these Python 2 modules to Python 3
upon import. First, install the Python 2-only package into your Python 3
environment::

    $ pip3 install mypackagename --no-compile   # to ignore SyntaxErrors
    
(or use ``pip`` if this points to your Py3 environment.)

Then add the following code at the top of your (Py3 or Py2/3-compatible)
code::

    from past import autotranslate
    autotranslate(['mypackagename'])
    import mypackagename

This feature is experimental, and we would appreciate your feedback on
how well this works or doesn't work for you. Please file an issue `here
<https://github.com/PythonCharmers/python-future>`_ or post to the
`python-porting <https://mail.python.org/mailman/listinfo/python-porting>`_
mailing list.

For more information on the automatic translation feature, see :ref:`translation`.


Next steps
----------
For more information about writing Py3/2-compatible code, see :ref:`what-else`.

