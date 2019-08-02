Approach
========

Some calls where made on which tools to use and how to use them.

Here is an attempt to present and explain the chosen approach, as well as other ideas
that where rejected.

General guiding principles
--------------------------

.. note::
    The tools chosen on the basis of these general guiding principles, are detailed in
    :ref:`tools`.

    The choice of these tools has been heavily influenced by the following references:

    * John Freeman's |project-template-python|_
    * Étienne Bersac's `Débuter avec Python en 2019`_ (french), seen as a link from
      Sam (& Max)'s `Stack Python en 2019`_ (french)

    The two articles that definitely tipped the balance to go with ``poetry`` are:

    * `Poetically Packaging Your Python Project`_
    * `A deeper look into Pipenv and Poetry`_


As many things as possible in as few places as possible
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

As very well illustrated in the article `Poetically Packaging Your Python Project`_,
classical packaging of your python library using ``setuptools`` forces you to define up
to *4 files* (more?)...

One of the main goal here was to have as much as possible defined
in the ``pyproject.toml`` file, which has become an accepted standard in the Python
community by way of `PEP 518`_.

Don't Repeat yourself
~~~~~~~~~~~~~~~~~~~~~

In many projects, I found things such as development dependencies repeated in multiple
places, for instance in ``setup.py``, in a ``dev-requirements.txt`` file as well as in
the ``tox.ini`` file. And this is true of even the most mature and rigorously tested
projects (see the repetition `here
<https://github.com/behave/behave/blob/121e61c5598b7967fd8a2eb1833235b282dc3ca6/setup.py#L83-L101>`__
and `here
<https://github.com/behave/behave/tree/121e61c5598b7967fd8a2eb1833235b282dc3ca6/py.requirements>`__
).

Staying `DRY`_ helps quality and prevents "wtf" bugs that are hard to catch. (amongst other things)

.. _DRY: https://en.wikipedia.org/wiki/Don%27t_repeat_yourself

Specific guiding principles
---------------------------

Let ``poetry`` handle the project's virtual env
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

``poetry`` creates a virtual env for your project by default... so why not use it and forget about
the issue.

I prefer to have the ``.venv`` directory `created in the project's directory
<https://poetry.eustace.io/docs/configuration/#settingsvirtualenvsin-project-boolean>`_ so that
it is at hand.

Then it is just a matter of using the ``poetry shell`` command to switch to your virtual env, and
that's it. all available from there.

Considered (and rejected) alternatives
++++++++++++++++++++++++++++++++++++++

Usage of ``pyenv``
------------------

.. |project-template-python| replace:: ``project-template-python``
.. _project-template-python: https://github.com/thejohnfreeman/project-template-python

.. _Débuter avec Python en 2019: https://bersace.cae.li/conseils-python-2019.html
.. _Stack Python en 2019: http://sametmax.com/stack-python-en-2019/
.. _Poetically Packaging Your Python Project:
    https://hackersandslackers.com/poetic-python-project-packaging/
.. _A deeper look into Pipenv and Poetry: https://frostming.com/2019/01-04/pipenv-poetry

.. _PEP 518: https://www.python.org/dev/peps/pep-0518/
