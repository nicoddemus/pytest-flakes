pytest-flakes
=============

py.test plugin for efficiently checking python source with pyflakes.


Usage
-----

install via::

    pip install pytest-flakes

if you then type::

    py.test --flakes

every file ending in ``.py`` will be discovered and run through pyflakes,
starting from the command line arguments.

Simple usage example
-----------------------------

Consider you have this code::

    # content of module.py

    import os
    from os.path import *

    def some_function():
        pass

Running it with pytest-flakes installed shows two issues::

    $ py.test -q --flakes
    F
    ================================= FAILURES =================================
    ______________________________ pyflakes-check ______________________________
    /tmp/doc-exec-685/module.py:2: UnusedImport
    'os' imported but unused
    /tmp/doc-exec-685/module.py:3: ImportStarUsed
    'from os.path import *' used; unable to detect undefined names
    1 failed in 0.00 seconds

These are only two of the many issues that pytest-flakes can find.

Configuring pyflakes options per project and file
-------------------------------------------------

You may configure pyflakes-checking options for your project
by adding an ``flakes-ignore`` entry to your ``setup.cfg``
or ``pytest.ini`` file like this::

    # content of setup.cfg
    [pytest]
    flakes-ignore = ImportStarUsed

This would globally prevent complaints about star imports.
Rerunning with the above example will now look better::

    $ py.test -q --flakes
    F
    ================================= FAILURES =================================
    _________________ pyflakes-check(ignoring ImportStarUsed) __________________
    /tmp/doc-exec-685/module.py:2: UnusedImport
    'os' imported but unused
    1 failed in 0.00 seconds

But of course we still would want to delete the ``import os`` line to
have a clean pass.

If you have some files where you want to specifically ignore
some errors or warnings you can start a flakes-ignore line with
a glob-pattern and a space-separated list of codes::

    # content of setup.cfg
    [pytest]
    flakes-ignore =
        *.py UnusedImport
        doc/conf.py ALL


Ignoring certain lines in files
-------------------------------

You can ignore errors per line by appending special comments to them like this::

    import sys # noqa
    app # pragma: no flakes


Running pyflakes checks and no other tests
------------------------------------------

You can restrict your test run to only perform "flakes" tests
and not any other tests by typing::

    py.test --flakes -m flakes

This will only run tests that are marked with the "flakes" keyword
which is added for the flakes test items added by this plugin.

If you are using pytest < 2.4, then use the following invocation
to the same effect::

    py.test --flakes -k flakes


Notes
-----

The repository of this plugin is at https://github.com/asmeurer/pytest-flakes

For more info on py.test see http://pytest.org

The code is partially based on Ronny Pfannschmidt's pytest-codecheckers plugin
and Holger Krekel's pytest-pep8.


Changes
=======

4.0.4 - 2021-10-26
------------------
- Fix pytest-flakes for deprecations in the upcoming pytest 7.0. [bluetech]
- Fix the pytest-flakes test suite in Python 3.10. [bluetech]
- Replace Travis CI with GitHub Actions. [bluetech]

4.0.3 - 2020-11-27
------------------

- Future proof some code against future versions of pytest. [RonnyPfannschmidt]

4.0.2 - 2020-09-18
------------------

- Fix calling pytest --flakes directly on an __init__.py file. [akeeman]

4.0.1 - 2020-07-28
------------------

- Maintenance of pytest-flakes has moved from fschulze to asmeurer. The repo
  for pytest-flakes is now at https://github.com/asmeurer/pytest-flakes/

- Fix test failures.
  [asmeurer]

- Fix deprecation warnings from pytest.
  [asmeurer]

- Fix invalid escape sequences.
  [akeeman]

4.0.0 - 2018-08-01
------------------

- Require pytest >= 2.8.0 and remove pytest-cache requirement.
  Cache is included in pytest since that version.
  [smarlowucf (Sean Marlow)]


3.0.2 - 2018-05-16
------------------

- Fix typo in name of ``flakes`` marker.
  [fschulze]


3.0.1 - 2018-05-16
------------------

- Always register ``flakes`` marker, not only when the ``--flakes`` option
  is used.
  [fschulze]


3.0.0 - 2018-05-16
------------------

- Drop support for Python 3.3. It still works so far, but isn't tested anymore.
  [fschulze]

- Add ``flakes`` marker required since pytest 3.1.
  [fschulze]

- Use pyflakes.api.isPythonFile to detect Python files. This might test more
  files than before and thus could cause previously uncaught failures.
  [asmeurer (Aaron Meurer)]


2.0.0 - 2017-05-12
------------------

- Dropped support/testing for Python 2.5, 2.6, 3.2.
  [fschulze]

- Added testing for Python 3.6.
  [fschulze]

- Fixed some packaging and metadata errors.
  [fladi (Michael Fladischer), fschulze]


1.0.1 - 2015-09-17
------------------

- Compatibility with upcoming pytest.
  [RonnyPfannschmidt (Ronny Pfannschmidt)]


1.0.0 - 2015-05-01
------------------

- Fix issue #6 - support PEP263 for source file encoding.
  [The-Compiler (Florian Bruhin), fschulze]

- Clarified license to be MIT like pytest-pep8 from which this is derived.
  [fschulze]


0.2 - 2013-02-11
----------------

- Adapt to pytest-2.4.2 using ``add_marker()`` API.
  [fschulze, hpk42 (Holger Krekel)]

- Allow errors to be skipped per line by appending # noqa or # pragma: no flakes
  [fschulze, silviot (Silvio Tomatis)]

- Python 3.x compatibility.
  [fschulze, encukou (Petr Viktorin)]


0.1 - 2013-02-04
----------------

- Initial release.
  [fschulze (Florian Schulze)]
