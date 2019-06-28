Searching for a sane python development environment
===================================================

Introduction
------------

As I decided to write a python package of my own, I found myself facing of a multitude of different
python tools, from packaging to development/deployment tasks managing. And I found it quite overwhelming.

As a former developer and as an agile coach, I always found it key to have a good, sane, helping set of tools
to systemically ensure quality and consistency in the code and development work. No less key is the capacity
to automate as far as possible the integration and deployment of packages and software.

.. note::
    TO CHANGE:

    * video on tox

    The choice of setup described in this page has been heavily influenced by the following references:

    * John Freeman's |project-template-python|_
    * Étienne Bersac's `Débuter avec Python en 2019`_
      (french), seen as a link from Sam (& Max)'s `Stack Python en 2019`_ (french)

    The two articles that definitely tipped the balance to go with ``poetry`` are:

    * `Poetically Packaging Your Python Project`_
    * `A deeper look into Pipenv and Poetry`_

.. |project-template-python| replace:: ``project-template-python``
.. _project-template-python: https://github.com/thejohnfreeman/project-template-python
.. _Débuter avec Python en 2019: https://bersace.cae.li/conseils-python-2019.html
.. _Stack Python en 2019: http://sametmax.com/stack-python-en-2019/
.. _Poetically Packaging Your Python Project: https://hackersandslackers.com/poetic-python-project-packaging/
.. _A deeper look into Pipenv and Poetry: https://frostming.com/2019/01-04/pipenv-poetry

Tools used
----------

Python environment management
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* |pyenv|_ used to make available various python version on the same platform.
* ``pyenv-virtualenv`` to create with- and centralise in ``pyenv`` all virtual envrionments.
  (even though the heavy use of ``tox`` and ``poetry`` will limit the usefulness of these
  virtual environments)

.. |pyenv| replace:: ``pyenv``
.. _pyenv: http://bbc.com

Python dependencies, packaging, publishing
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* ``poetry``

Available features not used:

* development dependencies (apart from installing ``tox``), which will be handled by ``tox``.
* version bumping, which is handled by ``python-semantic-release``.

Running tests and qa tasks
~~~~~~~~~~~~~~~~~~~~~~~~~~

* ``tox`` (along with ``tox-pyenv``, and ``tox-virtualenv-no-download``) will be handling the tests
  and qa tasks, except for pre-commit hooks.

Packages used for testing and qa:

* ``pytest``
* ``behave``
* ``black``
* ``isort``
* ``pyling``
* ``flake8`` (with ``flake8-bugbear``)
* ``coverage``

Documentation
~~~~~~~~~~~~~

* ``sphinx``
* Read the docs

Pre-commit hooks
~~~~~~~~~~~~~~~~

* ``pre-commit`` package is used to set them up.

Hooks used:

* ``gitlint``
* ``black``
* ``blacken-docs``
* ``isort``, helped wigth ``seed-isort-config``
* some ``pre-commit`` packaged ``pre-commit-hooks``:
    * ``trailing-whitespace``
    * ``end-of-file-fixer``
    * ``check-yaml``
    * ``debug-statements``
    * ``flake8`` with ``flake8-bugbear``
* ``pyupgrade``
* some ``pre-commit`` packaged ``pygrep-hooks``:
    * ``rst-backticks``

Commit comments readability, change logs, version bumping with semantic versioning
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* ``commitizen`` (the python package, not the javascript version) to create commit comments which are readable
* ``gitlint`` to ensure that the commit comment effectively follow the semantic of ``commitizen``
* ``python-semantic-release`` to create the change logs and to bump versions following semantic versioning rules

Continuous integration and continuous deployment
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* ``gitlab-ci`` for local CI and release workflow
* ``travis-ci`` for PR checks on linux (macOS ?)
* ``appveyor`` for PR checks on windows

IDE and it's setup
~~~~~~~~~~~~~~~~~~

My IDE of choice is Intellij/Pycharm. I know that Visual Studio Code is gaining momentum,
but I am quite happy with the former.

Plugins I have added so far:

* PyVenv Manage
* Toml
* Ini
* set up as external tools
    * ``black``
    * ``pylint``
    * ``isort``


Main tools
~~~~~~~~~~

+------------------+--------------------------------------------------------------+-----------------+
| Tool             | Used features                                                | Unused features |
+==================+==============================================================+=================+
| ``pyenv``        | install the python environments on your host.                |                 |
+------------------+--------------------------------------------------------------+-----------------+
| ``poetry``       | package dependency                                           | versioning      |
|                  |                                                              |                 |
|                  | packaging                                                    |                 |
|                  |                                                              |                 |
|                  | package publishing                                           |                 |
+------------------+--------------------------------------------------------------+-----------------+
| ``invoke``       | running linting                                              |                 |
|                  |                                                              |                 |
|                  | running tests                                                |                 |
|                  |                                                              |                 |
|                  | generating the doc (sphinx)                                  |                 |
|                  |                                                              |                 |
|                  | serve generated doc                                          |                 |
+------------------+--------------------------------------------------------------+-----------------+
| ``tox``          | running code tests in multiple pythons environments          |                 |
+------------------+--------------------------------------------------------------+-----------------+
| ``bump2version`` | updating version (together with committing and tag creation) |                 |
+------------------+--------------------------------------------------------------+-----------------+
| ``black``        | linging: enforcing conformity to PEP8 (-ish)                 |                 |
+------------------+--------------------------------------------------------------+-----------------+
| ``isort``        | sorting python imports                                       |                 |
+------------------+--------------------------------------------------------------+-----------------+
| ``pylint``       | linting (*usage planned*)                                    |                 |
+------------------+--------------------------------------------------------------+-----------------+
| ``flake8``       | linting (*usage planned*)                                    |                 |
+------------------+--------------------------------------------------------------+-----------------+
| ``mypy``         | optional static typing (*usage planned*)                     |                 |
+------------------+--------------------------------------------------------------+-----------------+
| ``pytest``       | unit testing                                                 |                 |
+------------------+--------------------------------------------------------------+-----------------+
| ``coverage``     | coverage (*usage planned*)                                   |                 |
+------------------+--------------------------------------------------------------+-----------------+
| ``gitlab-ci``    | local CI pipeline and continuous delivery pipeline           |                 |
+------------------+--------------------------------------------------------------+-----------------+
| ``travis-ci``    | pull request CI pipeline on linux and mac                    |                 |
+------------------+--------------------------------------------------------------+-----------------+
| ``appveyor``     | pull request CI pipeline on windows                          |                 |
+------------------+--------------------------------------------------------------+-----------------+

Complementary tools
~~~~~~~~~~~~~~~~~~~

+--------------------------------+------------------------------------------------+-----------------+
| Tool                           | Used features                                  | Unused features |
+================================+================================================+=================+
| ``tox-virtualenv-no-download`` | disable virtualenv (>=14)'s downloading        |                 |
|                                | behaviour when running through tox.            |                 |
+--------------------------------+------------------------------------------------+-----------------+
| ``flake8-bugbear``             | B950 to follow `black's recommendations        |                 |
|                                | regarding line length handling by flake8`_     |                 |
+--------------------------------+------------------------------------------------+-----------------+
|                                |                                                |                 |
+--------------------------------+------------------------------------------------+-----------------+

.. _black's recommendations regarding line length handling by flake8: https://black.readthedocs.io/en/stable/the_black_code_style.html#line-length
