Tools and Approach
==================

.. note::
    TO CHANGE:

    * video on tox

    The choice of setup described in this page has been heavily influenced by the following references:

    * John Freeman's |project-template-python|_
    * Étienne Bersac's `Débuter avec Python en 2019`_
      (french), seen as a link from Sam (& Max)'s `Stack Python en 2019`_ (french)


TL;DR: Tools used
-----------------

Python versions installation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* |pyenv|_: used to make available various python version on the same platform.


Python dependencies, packaging, publishing
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* |poetry|_: preferred over ``pipenv`` or the combination of ``pip`` and ``setuptools``

.. note::
    The two articles that definitely tipped the balance to go with ``poetry`` are:

    * `Poetically Packaging Your Python Project`_
    * `A deeper look into Pipenv and Poetry`_


Unused ``poetry`` features
++++++++++++++++++++++++++

* *Development dependencies* are only used to the bare minimum. (|tox|_, |tox-pyenv|_,
  |commitizen|_ and |pre-commit|_) ``extras`` are used instead, alongside with
  |tox|_ for task management.
* *Version bumping* feature is rather limited compared to what is already available
  through |python-semantic-release|_.


Multi-environment testing (local) and task management
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* |tox|_: (along with |tox-pyenv|_) will be handling running the tests and qa checks,
  except for ``pre-commit`` hooks.

Testing suites
~~~~~~~~~~~~~~

* |pytest|_: for unit testing.
* |behave|_: for functional testing.


QA Tools
~~~~~~~~

* |black|_: to end arguments about code formatting.
* |isort|_: to sort import properly.
* |flake8|_: for linting. It should be less and less needed thanks to ``black``.

  * used in combination with |flake8-bugbear|_ to apply ``B950`` to follow |black's
    recommendations regarding line length handling by flake8|_.

* |pylint|_: for (further) linting. (see note below)
* |mypy|_: for static type checking.
* |coverage|_ for code coverage.

Some of these tools are called during the commit process `pre-commit hooks`_ and
on-the-fly while developing through `IDE setup`_.

.. note::
    This `flake8 vs pylint reddit thread comment`_ catches succinctly the differences
    and advantages of using both ``flake8`` and ``pylint``.



Versioning, change logs and commit messages readability
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* |python-semantic-release|_: to create the change logs and to bump versions following
  semantic versioning rules
* |commitizen|_: (the python package, not the javascript version) to create commit
  comments which are readable.
* |gitlint|_: to ensure that the commit comment effectively follow the semantic of
  ``commitizen``.


Continuous integration and continuous deployment
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* |gitlab-ci|_: for local CI and release workflow.
* |travis-ci|_: for PR checks on linux (and macOS ?).
* |appveyor|_: for PR checks on windows.


.. _pre-commit hooks:

Ensuring quality though pre-commit hooks
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* |pre-commit|_, the python tool, is used to set them up.

Hooks used
++++++++++

* |gitlint|_
* |black|_
* |blacken-docs|_
* |isort|_, helped wigth |seed-isort-config|_
* some ``pre-commit`` packaged ``pre-commit-hooks``:

  * ``trailing-whitespace``
  * ``end-of-file-fixer``
  * ``check-yaml``
  * ``debug-statements``
  * ``flake8`` with ``flake8-bugbear``

* |pyupgrade|_
* some ``pre-commit`` packaged ``pygrep-hooks``:

  * ``rst-backticks``


.. _IDE setup:

Ensuring on-the-fly QA though IDE setup
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

My IDE of choice is Pycharm/Intellij IDEA. I know that Visual Studio Code is gaining momentum,
but I am quite happy with the former.

* Set up as external tools:

  * |black PyCharm/Intellij IDEA integration|_.
  * |pylint PyCharm/Intellij IDEA integration|_.
  * |isort PyCharm/Intellij IDEA integration|_.



Other useful IDE setup
++++++++++++++++++++++

You might find these other plugins useful:

* `PyVenv Manage`_: provides a shortcut to manage the Python interpreter of
  Pycharm/Intellij IDEA projects.


Documentation
~~~~~~~~~~~~~

* |sphinx|_: to generate docs, which are then published online on `Read the docs`_.


.. |project-template-python| replace:: ``project-template-python``
.. _project-template-python: https://github.com/thejohnfreeman/project-template-python

.. _Débuter avec Python en 2019: https://bersace.cae.li/conseils-python-2019.html
.. _Stack Python en 2019: http://sametmax.com/stack-python-en-2019/
.. _Poetically Packaging Your Python Project:
    https://hackersandslackers.com/poetic-python-project-packaging/
.. _A deeper look into Pipenv and Poetry: https://frostming.com/2019/01-04/pipenv-poetry

.. _flake8 vs pylint reddit thread comment:
    https://
    www.reddit.com/r/Python/comments/82hgzm/any_advantages_of_flake8_over_pylint/dvai60a/

.. |black PyCharm/Intellij IDEA integration| replace::
   ``black`` PyCharm/Intellij IDEA integration
.. _black PyCharm/Intellij IDEA integration:
   https://black.readthedocs.io/en/stable/editor_integration.html#pycharm-intellij-idea
.. |pylint PyCharm/Intellij IDEA integration| replace::
   ``pylint`` PyCharm/Intellij IDEA integration
.. _pylint PyCharm/Intellij IDEA integration:
   https://plugins.jetbrains.com/plugin/11084-pylint
.. |isort PyCharm/Intellij IDEA integration| replace::
   ``isort`` PyCharm/Intellij IDEA integration
.. _isort PyCharm/Intellij IDEA integration:
   https://github.com/timothycrosley/isort/wiki/isort-Plugins

.. _PyVenv Manage: https://plugins.jetbrains.com/plugin/10085-pyvenv-manage

.. _Read the docs: https://www.readthedocs.io/

.. |black's recommendations regarding line length handling by flake8| replace::
   ``black``'s recommendations regarding line length handling by ``flake8``
.. _black's recommendations regarding line length handling by flake8:
    https://black.readthedocs.io/en/stable/the_black_code_style.html#line-length

.. |pyenv| replace:: ``pyenv``
.. _pyenv: https://github.com/pyenv/pyenv
.. |poetry| replace:: ``poetry``
.. _poetry: https://poetry.eustace.io
.. |tox| replace:: ``tox``
.. _tox: https://tox.readthedocs.io/en/latest/
.. |tox-pyenv| replace:: ``tox-pyenv``
.. _tox-pyenv: https://github.com/samstav/tox-pyenv

.. |pytest| replace:: ``pytest``
.. _pytest: http://pytest.org
.. |behave| replace:: ``behave``
.. _behave: https://behave.readthedocs.io/

.. |black| replace:: ``black``
.. _black: https://black.readthedocs.io/
.. |blacken-docs| replace:: ``blacken-docs``
.. _blacken-docs: https://github.com/asottile/blacken-docs
.. |isort| replace:: ``isort``
.. _isort: https://isort.readthedocs.io/
.. |seed-isort-config| replace:: ``seed-isort-config``
.. _seed-isort-config: https://github.com/asottile/seed-isort-config
.. |flake8| replace:: ``flake8``
.. _flake8: https://flake8.readthedocs.io/
.. |flake8-bugbear| replace:: ``flake8-bugbear``
.. _flake8-bugbear: https://github.com/PyCQA/flake8-bugbear
.. |pylint| replace:: ``pylint``
.. _pylint: https://pylint.readthedocs.io/
.. |mypy| replace:: ``mypy``
.. _mypy: https://mypy.readthedocs.io/
.. |coverage| replace:: ``coverage``
.. _coverage: https://coverage.readthedocs.io/
.. |pyupgrade| replace:: ``pyupgrade``
.. _pyupgrade: https://github.com/asottile/pyupgrade

.. |python-semantic-release| replace:: ``python-semantic-release``
.. _python-semantic-release: https://python-semantic-release.readthedocs.io/
.. |commitizen| replace:: ``commitizen``
.. _commitizen: https://woile.github.io/commitizen/
.. |gitlint| replace:: ``gitlint``
.. _gitlint: https://jorisroovers.github.io/gitlint/
.. |pre-commit| replace:: ``pre-commit``
.. _pre-commit: https://pre-commit.com

.. |sphinx| replace:: ``sphinx``
.. _sphinx: https://www.sphinx-doc.org/

.. |gitlab-ci| replace:: ``gitlab-ci``
.. _gitlab-ci: https://docs.gitlab.com/ce/ci/
.. |travis-ci| replace:: ``travis-ci``
.. _travis-ci: https://travis-ci.com
.. |appveyor| replace:: ``appveyor``
.. _appveyor: https://www.appveyor.com
