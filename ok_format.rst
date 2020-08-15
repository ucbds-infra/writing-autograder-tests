OK Test Format
==============

.. _Otter-Grader: https://otter-grader.rtfd.io/
.. _OkPy: https://okpy.github.io/documentation
.. _doctests: doctests.rst

Most autograders developed at UC Berkeley, including Otter-Grader_, rely on the OK test format, 
originally developed for OkPy_. This test format relies on :doc:`doctests <doctests>` to check that students' code
works correctly. 

The OK format is very simple: a test is defined by a single Python file that contains a global variable
called ``test`` which is assigned to a dictionary containing test configurations. The structure of this
dictionary is:

- ``test["name"]`` is the name of the test case, and should be a valid Python variable name
- ``test["points"]`` is the point value of the test case; points are assigned all-or-nothing and all
  test cases (more below) must pass to be awarded the points
- ``test["suites"]`` is a list of dictionaries that correspond to test suites, groups of test cases;
  a ``suite`` consists of:

    - ``suite["cases"]`` is a list of dictionaries that correspond to test cases (individual doctests);
      a ``case`` consists of:

        - ``case["code"]`` is a Python interpreter-formatted string of code to be executed
        - ``case["hidden"]`` is a boolean that indicates whether the test case is hidden from students
        - ``case["locked"]`` is a boolean that indicates whether the test case is locked; this configuration
          is generally only relevant to OkPy_

    - ``suite["score"]`` is a boolean that indicates whether the suite is part of the score
    - ``suite["setup"]`` is a code string that is run before the individual test cases are run
    - ``suite["teardown"]`` is a code string that is run after the individual test cases are run
    - ``suite["type"]`` is a string indicating the type of test; this is almost always going to be 
      ``"doctest"`` unless you're using OkPy_

Note that graders like Otter-Grader_ only allow test files with a single test suite.

.. code-block:: python

    test = {
        "name": "q1",
        "points": 1,
        "suites": [
            {
                "cases": [
                    {
                        "code": r"""
                        >>> foo()
                        'bar'
                        """,
                        "hidden": False,
                        "locked": False
                    }
                ],
                "scored": True,
                "setup": "",
                "teardown": "",
                "type": "doctest"
            }
        ]
