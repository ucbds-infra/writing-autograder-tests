Seeding
=======

.. _Otter-Grader: https://otter-grader.rtfd.io/

An important aspect of writing code in the data science environment is the ability to simulate random 
processes. This presents a unique challenge to autograding assignments because the random state of 
the students' environments will be completely different from the grading environment. Some autograding 
solutions account for this issue by incorporating calls to seed random libraries during execution; 
Otter-Grader_ does this very easily using grading configurations, and this is by far the most preferable
solution if you're using Otter.

When working with other autograding solutions, seeds must be incorporated into the test files themselves
and assignments must be written in such a way as to make the seeding *before* execution possible.

Solution 1: Functions
---------------------

The first and easiest solution to this problem is to have students wrap their code involving randomness
into a function. In this way, the tests can make a seeding call before calling the function directly,
ensuring that the seed set before the code is executed. Consider the following example question and the
following test:

.. code-block:: python

    import numpy as np

    # Question 1: Define roll_die which rolls a 6-sided die using NumPy.
    def roll_die():
        return np.random.choice(np.arange(1, 7))

.. code-block:: python

    >>> np.random.seed(42)
    >>> roll_die()
    4

This approach, while easy-to-implement and effective, is generally at odds with the general autograding 
paradigm of testing global variables directly rather than encapsulting logic in needless functions. For
this reason, there is another solution that is perhaps more elegant.

Solution 2: Seeding the Students' Code
--------------------------------------

In this solution, the random libraries are seeded directly before the student writes their code in a 
manner visible to the students.

.. code-block:: python

    import numpy as np

    np.random.seed(42)

    # Question 1: Assign roll_value to the roll of a six-sided die.
    roll_value = np.random.choice(np.arange(1, 7))

.. code-block:: python

    >>> np.random.seed(42)
    >>> roll_die()
    4

This method is significantly less elegant and is susceptible to students changing the seed, which could
throw off the results of the test and fail students incorrectly. It is also important to note that if 
you're working in a notebook format, you need to include seeds im *every* randomness cell because repeated
runs of cells will change the seed value for subsequent questions, which could result in students 
failing tests.
