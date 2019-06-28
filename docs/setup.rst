Full setup
==========

Environment setup
-----------------

Prerequisites
~~~~~~~~~~~~~

Install ``pyenv`` and ``pyenv-virtualenv``
++++++++++++++++++++++++++++++++++++++++++

First ``pyenv``: `install with Homebrew on macOS <https://github.com/pyenv/pyenv#homebrew-on-macos>`_
(there are also `other installation method <https://github.com/pyenv/pyenv#installation>`_
, which where not tested here)::

    # On macOS
    $ brew update
    $ brew install pyenv

To allow be able to compile the python environnent with ``pyenv`` on Mac, you will have to install
XCode command line tools. I installed them `from the developer connection <http://>`_ because of the following
`XCode command line tools installation issues <http://>`_.

You will also have to remember to add the following exports on your profile (``bash`` in my case),
otherwise you will get these `issues with pyenv's python compilation <http://>`_::

    $ echo 'export PATH="/usr/local/opt/openssl/bin:$PATH"' >> ~/.bash_profile
    $ echo 'export LDFLAGS="-L/usr/local/opt/openssl/lib -L/usr/local/opt/readline/lib"' >> ~/.bash_profile
    $ echo 'export CPPFLAGS="-I/usr/local/opt/openssl/include -I/usr/local/opt/readline/include -I$(xcrun --show-sdk-path)/usr/include"' >> ~/.bash_profile
    $ echo 'export PKG_CONFIG_PATH="/usr/local/opt/openssl/lib/pkgconfig:/usr/local/opt/readline/lib/pkgconfig"' >> ~/.bash_profile

    # Do not forget to restart your shell at some later stage to activate the changes (exec "$SHELL")

We also add the auto-completion and other features (normally optional, the ``pyenv-virtualenv``
`documantation <http://>`_ requires it)::

    $ echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\n  eval "$(pyenv init -)"\nfi' >> ~/.bash_profile

    # Do not forget to restart your shell at some later stage to activate the changes (exec "$SHELL")

.. warning::

    Adding the auto-completion features of ``pyenv`` changes your ``PATH`` environment variable.
    This is one of the reasons why installing ``tox`` with a ``brew install tox`` did not always
    quite work as expected. (even though I ended up not using it as it does not contain ``tox``'s
    latest version)

If you have not done so already, restart your shell::

    $ exec "$SHELL"

Install the versions of python you will want to work with::

    $ pyenv install 3.6.8
    [..]
    $ pyenv install 3.7.3
    [..]
    $ pyenv install 3.8-dev
    [..]

Install ``pyenv-virtualenv`` following `the documentation <http://>`_ and add the auto-activations as requested in the
doc::

    $ brew install pyenv-virtualenv
    [..]
    $ echo -e 'if which pyenv-virtualenv-init > /dev/null; then\n  eval "$(pyenv virtualenv-init -)"\nfi' >> ~/.bash_profile

    # Do not forget to restart your shell at some later stage to activate the changes (exec "$SHELL")
    $ exec "$SHELL"

Prepare your project's environment
++++++++++++++++++++++++++++++++++

Create a virtual environment for your project (here we are using python 3.6.8)::

    $ cd path/to/my-project
    $ pyenv virtualenv 3.6.8 my-project-3.6.8

Set your newly create virtual env as your project's default env, and add the other python versions
you will want ``tox`` to access to in the future::

    $ pyenv local my-project-3.6.8 3.6.8 3.7.3 3.8-dev

    # the current environment appears on the prompt
    (my-project-3.6.8) $

    (my-project-3.6.8) $ pyenv local
    my-project-3.6.8
    3.6.8
    3.7.3
    3.8-dev

Install ``tox`` and ``tox-pyenv``
+++++++++++++++++++++++++++++++++

Install ``tox`` and ``tox-pyenv`` in your project's virtual env::

    (my-project-3.6.8) $ pip install -U pip setuptools
    [..]

Install ``poetry``
++++++++++++++++++

`Install <https://poetry.eustace.io/docs/#installation>`_ ``poetry`` and `add completion
<https://poetry.eustace.io/docs/#enable-tab-completion-for-bash-fish-or-zsh>`_::

    $ curl -sSL https://raw.githubusercontent.com/sdispater/poetry/master/get-poetry.py | python
    [..]
    $ source ~/.bash_profile

    # add completion
    $ poetry completions bash > $(brew --prefix)/etc/bash_completion.d/poetry.bash-completion

.. note::

    ``poetry`` will be the only tool I add to the system's python.

I personally prefer poetry to create the virtualenvs in the project directory. They will be created in
a local ``.venv`` directory::

    $ poetry config settings.virtualenvs.in-project true

If you have publishing rights for the package, `setup repositories username and password <https://poetry.eustace.io/docs/repositories/#configuring-credentials>`_ for
``pypi``. Same for ``testpypi``, but after
`adding the url <https://poetry.eustace.io/docs/repositories/#adding-a-repository>`_::

    $ poetry config http-basic.pypi your_username your_password
    $ poetry config repositories.testpypi https://test.pypi.org/legacy/
    $ poetry config http-basic.testpypi your_username your_password

.. note::

    You are ready to go to create your project using ``tox`` and ``poetry``.

Actual setup
~~~~~~~~~~~~

Clone the project (or your fork of it) and move to the project directory::

    $ git clone https://github.com/esciara/pyteleinfo.git
    $ cd pyteleinfo

`Install the project's dependencies <https://poetry.eustace.io/docs/basic-usage/#installing-dependencies>`_::

    $ poetry install

Install ``black``'s `pre-commit hook <https://black.readthedocs.io/en/stable/version_control_integration.html>`_::

    $ pre-commit install

If you want to use your own local continuous integration server/continuous delivery pipeline,
install ``gitlab-ci`` using ``docker-compose`` (that you will have previously installed)
using code in `this repository <https://github.com/jeshan/gitlab-on-compose>`_::

    $ cd your/main/repositories/directory
    # Use gitlab_on_compose as a target cloning directory to avoid issues...
    $ git clone https://github.com/jeshan/gitlab-on-compose.git gitlab_on_compose
    $ docker-compose up

.. note:: You might want to reduce the number of gitlab-runners in you compose file to save resources.

Development tasks used/available
--------------------------------

Running tests::

    $ poetry run invoke test

Running black::

    $ poetry run black .

Running linting::

    $ poetry run invoke lint

Running generating the docs::

    $ poetry run invoke docs

Serving the generated docs to visually check them::

    $ poetry run invoke serve

Bumping version::

    $ poetry run bump2version patch    # used patch here, but use the argument your need

Building source and package distributions::

    $ poetry build

Publishing distributions to testpypi::

    $ poetry publish -r testpypi

    # If you want to build and publish in one go:
    $ poetry publish -r testpypi --build

Publishing distributions to pypi::

    $ poetry publish

.. note::

    Sphinx docs' publishing on http://readthedocs.org/ is done automatically through a ``github`` webhook setup
    from your account on the site.

Release workflow
--------------------

Reused with thanks from `Behave's repository <https://github.com/behave/behave/blob/master/tasks/release.py#L64>`_.

Pre-release checklist
~~~~~~~~~~~~~~~~~~~~~

* [ ] Everything is checked in
* [ ] All tests pass w/ tox

Release checklist
~~~~~~~~~~~~~~~~~

* [ ] Bump version to new-version and tag repository (via bump_version)
* [ ] Build packages (sdist, bdist_wheel via prepare)
* [ ] Register and upload packages to testpypi repository (first)
* [ ] Verify release is OK and packages from testpypi are usable
* [ ] Register and upload packages to pypi repository
* [ ] Push last changes to Github repository

Post-release checklist
~~~~~~~~~~~~~~~~~~~~~~

* [ ] Bump version to new-develop-version (via bump_version)
* [ ] Adapt CHANGES (if necessary)
* [ ] Commit latest changes to Github repository

IDE integration
---------------

* pylint integration (TODO: see
  https://medium.com/@wbrucek/how-i-integrated-pylint-into-my-pycharm-workflow-47047ce5e7fd ... plugin not working)
* black integration (TODO: see
  https://black.readthedocs.io/en/stable/editor_integration.html#pycharm-intellij-idea, or use plugin ?)

How to contribute
-----------------

TODO: Contribution file in repository.
