Doctests
========

Most Python autograders run doctests against a global environment from the execution of a student's submission. These doctests are text formatted as if from a Python interpreter that tell the executor what code to run and what output to expect. An important nuance of doctests is that these test are based on :doc:`string comparison <comparison>`, so the outputs ``1`` and ``1.0`` are *not* equal.

Doctest Format
--------------

Python doctests are formatted as input and output from a Python interpeter, e.g.

.. code-block:: python

    >>> import numpy as np
    >>> np.random.seed(42)
    >>> np.random.choice([1, 2, 3])
    3

Note that Python structures that require multiple lines have ``...`` prompts, not ``>>>``.

.. code-block:: python

    >>> def foo(x):
    ...     return x
    >>> if foo(1) % 2 == 0:
    ...     print("even")
    ... else:
    ...     print("odd")

Lines that have an ellipsis prompt include ``elif``, ``else``, ``except`` and ``finally`` clauses, function bodies, continuations of escaped lines (lines after those ending with ``\``), and other indented lines (those that begin with whitespace). Note that in a normal interpreter there would be an additional empty line after an ellipsis block with an ellipsis prompt, but this can be omitted when writing doctests.

Sometimes, writing autograder tests may require the use of special characters whose meanings are changed when Python compiles the string, e.g. ``\``. If your autograder test involves these characters, when Python compiles the string, the doctest's meaning will be changed. For this reason, when writing doctests, it is always best practice to put autograder tests in raw strings, denoted by the letter ``r`` before the opening quote:

.. code-block:: python

    test = {
        "suites": [
            {
                "cases": [
                    {
                        "code": r"""
                        >>> if 4 % 2 == 0:
                        ...     print("even")
                        even
                        """     # a raw string
                    }
                ]
            }
        ]
    }

Note also that the doctest library ignores leading whitespace before prompts and outputs, e.g. as with ``textwrap.dedent``, so even though the spaces before each line in the string above are captured, they are ignored when the doctest is executed.

Running Doctests
----------------

Autograders abstract away executing doctests beyond the step of actually writing them. Once you understand the doctest format, you can write your own autograder tests and allow the autograder to take care of executing them and parsing the output. Most Python autogaders make use of Python's doctest library to perform these tests, and return results based on whether or not these pass.
