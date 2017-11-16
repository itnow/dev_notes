###############################################################################
Python Notes
###############################################################################


Compile Python 3.6
===============================================================================

- https://github.com/python/cpython/blob/3.6/README.rst
- https://docs.python.org/3/using/unix.html#building-python
- Python-3.6.3/README.rst


If we intend to install multiple versions of Python using the same installation
prefix (``--prefix`` argument to the configure script) we must take care that
our primary python executable is not overwritten by the installation
of a different version.

All files and directories installed using ``make altinstall``
contain the major and minor version and can thus live side-by-side.

If we intend to install multiple versions using the same prefix we must decide
which version (if any) is our "primary" version. ``make install`` also creates
``${prefix}/bin/python3`` which refers to ``${prefix}/bin/pythonX.Y`` and can
overwrite or masquerade the primary ``python3`` binary. ``make altinstall`` is
therefore recommended since it only installs ``${prefix}/bin/pythonX.Y``.

If we want to install custom version of Python: 2.7, 3.5, 3.6, with 3.6 being
the primary version, we would execute ``make install`` in our 3.6 build directory
and ``make altinstall`` in the others.


Dependencies
------------

- make
- build-essential
- libsqlite3-dev
- libbz2-dev
- zlib1g-dev
- libssl-dev
- libreadline-dev
- tk-dev
- libgdbm-dev
- liblzma-dev
- libncurses-dev
- libffi-dev


Compile steps
-------------

Get the source code::

    wget https://www.python.org/ftp/python/3.6.3/Python-3.6.3.tar.xz
    tar xvf Python-3.6.3.tar.xz
    cd Python-3.6.3

Configuration options and caveats for specific Unix platforms are extensively
documented in the README.rst.

To show helps on flags::

    ./configure --help

Configure to compile with optimizations (slow)::

    ./configure --enable-optimizations

Or just default configuration (fast)::

    ./configure

To clean already compiled files run before recompile::

    make clean

Compile, ``>`` redirects stdout, ``2>&1`` redirects stderr
to the same place as stdout::

    make > make_output.log 2>&1
    sudo make altinstall

By default files installed with ``prefix=/usr/local``. See more in Makefile.
So we can find related executables in ``/usr/local/bin/`` and run::

    /usr/local/bin/python3.6
    # or simpy
    python3.6



Linting
===============================================================================

::

    pip install flake8 pydocstyle
    pip install mypy
    pip install pep8-naming
    pip install flake8-import-order
    pip install flake8-debugger



Shebang
===============================================================================

A good choice is usually::

    #!/usr/bin/env python3

which searches for the Python interpreter in the whole PATH. However, some
Unices may not have the env command, so you may need to hardcode
``/usr/bin/python3`` as the interpreter path.

Define source code encoding for 2.x::

    # -*- coding: utf-8 -*-



PIP
===============================================================================
https://pip.pypa.io/en/stable/

=========================================== ===================================
``pip install SomePackage==1.0.4``          Install specific version.
``pip install 'SomePackage>=1.0.4'``        Install minimum version.
``pip install -U (--upgrade) SomePackage``  Upgrade to the latest version.
``pip list -o (--outdated)``                List outdated packages.

``pip freeze -l (--local)``                 List packages in requirements format.
                                            If in a virtualenv that has global access,
                                            do not output globally-installed packages.
=========================================== ===================================

Uninstall all requirements::

    pip freeze --local > to_uninstall.txt
    pip uninstall -r to_uninstall.txt -y

Another method::

    pip freeze --local | xargs pip uninstall -y


Version specifiers
------------------

A version specifier consists of a series of clauses, separated by commas::

    ~= 0.9, >= 1.0, != 1.3.4.*, < 2.0

The comma (",") is equivalent to a logical and operator: a candidate version
must match all given version clauses in order to match the specifier as a whole.
Whitespace between a conditional operator and the following version identifier
is optional, as is the whitespace around the commas.

============ ========================================
``~=``       Compatible release clause
``==``       Version matching clause
``!=``       Version exclusion clause
``<= , >=``  Inclusive ordered comparison clause
``< , >``    Exclusive ordered comparison clause
``===``      Arbitrary equality clause.
============ ========================================


pipdeptree
----------
https://github.com/naiquevin/pipdeptree

An utility to display dependency tree.



Local debugging servers
===============================================================================

HTTP server
-----------

::

    # 3.x
    python -m http.server
    # 2.x
    python -m SimpleHTTPServer


SMTP server
-----------

Run server and output received emails sent by the application to the console::
    
    python -m smtpd -n -c DebuggingServer localhost:25

Or redirect output to file::

    python -m smtpd -n -c DebuggingServer localhost:25 >> mail.log



IPython commands
===============================================================================

http://ipython.readthedocs.io/en/stable/interactive/magics.html

=============== ==========================
<object>?       Show info about object
%who / %whos    Show namespace info
%hist           History
%pbd            Activate debugger
=============== ==========================



Hints
===============================================================================

Show path of command in the current environment::

    which python

Find installation path of package::

    python -c 'import flask; print(flask.__path__)'

Virtual environment help::
    
    python3.6 -m venv -h

