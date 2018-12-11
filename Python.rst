###############################################################################
 Python Notes
###############################################################################


===============================================================================
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



===============================================================================
 Linting
===============================================================================

::

    pip install --user flake8 pydocstyle
    pip install --user mypy
    pip install --user pep8-naming
    pip install --user flake8-import-order
    pip install --user flake8-debugger



===============================================================================
 Shebang
===============================================================================

A good choice is usually::

    #!/usr/bin/env python3

which searches for the Python interpreter in the whole PATH. However, some
Unices may not have the env command, so you may need to hardcode
``/usr/bin/python3`` as the interpreter path.

Define source code encoding for 2.x::

    # -*- coding: utf-8 -*-



===============================================================================
 Pip
===============================================================================
https://pip.pypa.io/en/stable/

=========================================== ===================================
``pip install SomePackage==1.0.4``          Install specific version.
``pip install 'SomePackage>=1.0.4'``        Install minimum version.
``pip install 'SomePackage>=1.2,<1.3'``     Install within min & max versions.
``pip install -U (--upgrade) SomePackage``  Upgrade to the latest version.
``pip list -o (--outdated)``                List outdated packages.

``pip freeze -l (--local)``                 List packages in requirements format.
                                            If in a virtualenv that has global access,
                                            do not output globally-installed packages.
=========================================== ===================================

Uninstall all requirements::

    pip freeze --local > to_uninstall.txt
    pip uninstall -r to_uninstall.txt -y

or::

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


Workflow with two requirements
------------------------------

requirements-to-freeze.txt
    Is used to specify our top-level dependencies, and any explicit versions
    we need to specify.

requirements.txt
    Contains the output of ``pip freeze`` after
    ``pip install requirements-to-freeze.txt`` has been run.

Usage::

    pip install -r requirements-to-freeze.txt --upgrade
    pip freeze > requirements.txt


Bash completion for pip
-----------------------

For command line completion run::

    $ pip completion --bash >> ~/.bash_completion

or::

    $ echo 'eval "$(pip completion --bash)"' >> ~/.bash_completion



===============================================================================
 Pipenv
===============================================================================
- https://docs.pipenv.org/#pipenv-usage
- https://github.com/kennethreitz/pipenv

Pipenv is a dependency manager for Python projects. While pip can install
Python packages, pipenv is recommended as it’s a higher-level tool that
simplifies dependency management for common use cases. Pipenv manages
dependencies on a per-project basis.

Basic concepts:

- A virtualenv will automatically be created, when one doesn’t exist.
- When no parameters are passed to install, all packages [packages] specified
  will be installed.
- To initialize a specific Python venv: run ``pipenv --python 3.6``
- Otherwise, whatever virtualenv defaults to will be the default.

.. _user installation: https://pip.pypa.io/en/stable/user_guide/#user-installs

Do a `user installation`_ to prevent breaking any system-wide packages::

    $ pip install --user pipenv

To upgrade::

    $ pip install --user --upgrade pipenv

To create a new virtualenv with a specific version of Python (already installed
and on your PATH)::

    $ pipenv --python 3.6

Pipenv will automatically scan system for a Python that matches that given
version.

If you only have a requirements.txt file available when running
``pipenv install``, pipenv will automatically import the contents of this file
and create a Pipfile.

For help::

    $ pipenv [COMMAND] -h
    $ pipenv --man


Commands examples
-----------------

``pipenv install requests==2.13.0``
    To install a specific package version.

``pipenv install --dev SomePackage``
    Install package and update [dev-packages] of ``Pipfile``.

``pipenv install --dev``
    Install all dependencies, including dev.

``pipenv install --system``
    Use the system ``pip`` command to install packages into parent system. This
    is useful for Docker containers, and deployment infrastructure (e.g. Heroku
    does this).

``pipenv install --system --deploy``
    This will fail a build if the ``Pipfile.lock`` is out–of–date or Python
    version is wrong, instead of generating a new one.

``pipenv lock``
    To create a Pipfile.lock, which declares all dependencies and
    sub-dependencies, their latest available versions, and the current hashes
    for the downloaded files.

    We can use this to compile dependencies on our dev environment and deploy
    the compiled ``Pipfile.lock`` to all production environments
    for reproducible builds.

``pipenv install --ignore-pipfile``
    Ignore the ``Pipfile`` and install from the ``Pipfile.lock``.

``pipenv install --skip-lock``
    Ignore the ``Pipfile.lock`` and install from the ``Pipfile``. In addition,
    do not write out a ``Pipfile.lock`` reflecting changes to the ``Pipfile``.

``pipenv update --outdated``
    List out–of–date dependencies (only if version not fixed).

``pipenv run pip list -o``
    Show any outdated packages.

``pipenv update`` / ``pipenv update <pkg>``
    Update all packages or specific package.

``pipenv check``
    To scan dependency graph for known security vulnerabilities.

``pipenv run python some_script.py``
    Spawns a command installed into the virtualenv.

``pipenv uninstall --all``
    Purge all files from the virtual environment, but leave the ``Pipfile`` untouched.

``pipenv uninstall --all-dev``
    Remove all of the development packages from the virtual environment, and
    remove them from the ``Pipfile``.

``pipenv --rm``
    Remove the virtualenv.


Bash completion for pipenv
--------------------------

For command line completion run::

    $ pipenv --completion >> ~/.bash_completion

or::

    $ echo 'eval "$(pipenv --completion)"' >> ~/.bash_completion


Autoinstall Python
------------------

If ``pyenv`` is installed and configured, Pipenv will automatically ask if we
want to install a required version of Python if we don’t already have it
available on system.


Autoloading of .env
-------------------

If a ``.env`` file is present in your project, ``pipenv shell`` and ``pipenv
run`` will automatically load it.

If ``.env`` file is located in a different path or has a different name we can
set the ``PIPENV_DOTENV_LOCATION`` environment variable::

    $ PIPENV_DOTENV_LOCATION=/path/to/.env pipenv shell

To prevent pipenv from loading the ``.env`` file, set the
``PIPENV_DONT_LOAD_ENV`` environment variable::

    $ PIPENV_DONT_LOAD_ENV=1 pipenv shell


Configuration With Environment Variables
----------------------------------------
https://docs.pipenv.org/advanced.html#configuration-with-environment-variables

Pipenv options can be enabled via shell environment variables, for example:

    PIPENV_VENV_IN_PROJECT
        If set, use ``.venv`` in your project directory instead of the global
        virtualenv manager pew.

    PIPENV_NOSPIN
        Disable terminal spinner, for cleaner logs. Automatically set in CI
        environments.

Pipenv’s underlying ``pew`` dependency will automatically honor the
``WORKON_HOME`` environment variable::

    export WORKON_HOME=~/.other_venvs_location

To set environment variables on a per-project basis, we can use
`direnv project <https://direnv.net/>`_.


Working with platform-provided Python components
------------------------------------------------

It’s reasonably common for platform specific Python bindings for operating
system interfaces to only be available through the system package manager, and
hence unavailable for installation into virtual environments with pip. In these
cases, the virtual environment can be created with access to the system
site-packages directory::

    $ pipenv --three --site-packages

To ensure that all pip-installable components actually are installed into the
virtual environment and system packages are only used for interfaces that don’t
participate in Python-level dependency resolution at all, use the
PIP_IGNORE_INSTALLED setting::

    $ PIP_IGNORE_INSTALLED=1 pipenv install --dev



===============================================================================
 Local debugging servers
===============================================================================

HTTP server
-----------

.. code-block:: bash

    # 3.x
    $ python -m http.server
    # 2.x
    $ python -m SimpleHTTPServer


SMTP server
-----------

Run server and output received emails sent by the application to the console::

    $ python -m smtpd -n -c DebuggingServer localhost:1025
    $ sudo python -m smtpd -n -c DebuggingServer localhost:25

Or redirect output to file::

    $ python -m smtpd -n -c DebuggingServer localhost:1025 >> mail.log



===============================================================================
 IPython
===============================================================================
http://ipython.readthedocs.io/en/stable/interactive/magics.html

=================== ==========================
``<object>?``       Show info about object
``%who / %whos``    Show namespace info
``%hist``           History
``%pbd``            Activate debugger
=================== ==========================



===============================================================================
 Jupyter
===============================================================================
http://jupyter.org

Install and start the notebook server::

    $ python3 -m pip install jupyter
    $ jupyter notebook



===============================================================================
 Sphinx
===============================================================================
http://www.sphinx-doc.org/en/stable/

::

    $ python3 -m pip install sphinx
    $ sphinx-quickstart



===============================================================================
 Hints
===============================================================================

Find the user base binary directory::

    python -m site --user-base

To add the installed cli tools from a pip user install to user path::

    python -c "import site; import os; print(os.path.join(site.USER_BASE, 'bin'))"

Show path for python in the current environment::

    which python

Find installation path of package::

    python -c 'import sphinx; print(sphinx.__path__)'

Virtual environment help::

    python3.6 -m venv -h



===============================================================================
 Packages overview
===============================================================================

line_profiler and kernprof
-------------------------------------------------------------------------------
https://github.com/rkern/line_profiler

**line_profiler** is a module for doing line-by-line profiling of functions.
**kernprof** is a convenient script for running either line_profiler or the
Python standard library's cProfile or profile modules, depending on what is
available.

.. code-block:: bash

    $ pip install line_profiler

.. code:: python

    @profile
    def slow_function(a, b, c):
        ...

.. code-block:: bash

    $ kernprof -l sg-sb run -p 5005 --without-threads
    $ python -m line_profiler sg-sb.lprof


pyenv
-------------------------------------------------------------------------------
https://github.com/pyenv/pyenv

- Change the global Python version on a per-user basis.
- Provide support for per-project Python versions.
- Allow to override the Python version with an environment variable.
- Search commands from multiple versions of Python at a time. This may be
  helpful to test across Python versions with tox.

It works by filling a ``shims`` directory with fake versions of the Python
interpreter (plus other tools like ``pip`` and ``2to3``). When the system looks
for a program named python, it looks inside the shims directory first, and uses
the fake version, which in turn passes the command on to pyenv. Pyenv then
works out which version of Python should be run based on environment variables,
``.python-version`` files, and the global default.


pew
-------------------------------------------------------------------------------
https://github.com/berdario/pew

**Python Env Wrapper** is a set of commands to manage multiple virtual
environments. Pew can create, delete and copy your environments, using a single
command to switch to them wherever you are, while keeping them in a single
(configurable) location.


pipsi
-------------------------------------------------------------------------------
https://github.com/mitsuhiko/pipsi

pipsi is a wrapper around virtualenv and pip which installs scripts provided by
python packages into separate virtualenvs to shield them from your system and
each other.


restview
-------------------------------------------------------------------------------
https://github.com/mgedmin/restview

A viewer for ReStructuredText documents that renders them on the fly.

Pass the document to restview, and it will launch a web server on
``localhost:random-port`` and open a web browser. You can also pass the name of
a directory, and restview will recursively look for files that end in ``.txt``
or ``.rst`` and present you with a list. ::

    $ pip install restview
    $ restview -h
    $ restview [options] filename-or-directory [...]


httpie
-------------------------------------------------------------------------------
https://github.com/jakubroztocil/httpie

HTTPie is a command line HTTP client::

    $ pip install httpie
    $ http --help
    $ http [flags] [METHOD] URL [ITEM [ITEM]]
    $ http PUT example.org X-API-Token:123 name=John
