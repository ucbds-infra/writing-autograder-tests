Master Notebook Tools
=====================

.. _jAssign: https://github.com/okpy/jAssign
.. _Otter Assign: https://otter-grader.rtfd.io

Most UC Berkeley autograders include tools to abstract away the generation of test files by allowing
users to define the tests along with questions and solutions in a simple notebook format. Tools like
this include jAssign_ for OkPy and `Otter Assign`_ for Otter-Grader.

Most of these tools rely on the *output* of comment-delimited code cells to generate the doctest-
formatted test cases used in test files and parse these notebooks out into two versions: a directory
containing the notebook with solutions and hidden tests, and a directory with the notebook stripped
of solutions and only public tests.

In general, it is highly recommended that you use one of these tools as they ensure the gradeability
of assignments and make the process of creating and distributing assignments much easier. `Otter Assign`_
also integrates nicely with its other tools and makes the process of using Otter with 3rd part services
much easier.
