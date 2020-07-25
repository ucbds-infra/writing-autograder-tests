Comparisons
===========

A fundamental aspect of doctests is that the pass/fail behavior is determined by **string comparisons**. When a doctest is run, the code provided in the Python interpreter format is executed line-by-line and then the output of that line is expected to equal the output shown in the doctest.

Return Value Types
------------------

``None``
++++++++

Statements that return the Python value ``None`` have no output. If you put ``None`` into a Python interpeter, there is no output shown:

.. code-block:: python

    >>> None
    >>> # the next interpreter prompt

Some statements that return the value ``None`` include assignment statements, import statements, calls to functions like ``print`` and ``exec``, and any functions with no ``return`` statement or a return statement that returns ``None`` itself.

.. code-block:: python

    >>> def foo(x):
    ...     return x
    >>> def bar(y):
    ...     y = "some string"
    >>> def baz(z):
    ...     return None
    >>> foo(1)
    1
    >>> bar(2)
    >>> baz(3)
    >>> foo(None)
    >>> print("some other string")
    some other string

``bool``
++++++++

Boolean values in Python are by far the easiest to check with doctests because they have a static string representation and only two possible values. The string representation of ``bool``s is the same as their variable names: ``True`` and ``False``.

.. code-block:: python

    >>> True
    True
    >>> False
    False
    >>> def is_even(x):
    ...     return x % 2 == 0
    >>> is_even(1)
    False
    >>> is_even(4)
    True

``int``, ``float``, and other numeric types
+++++++++++++++++++++++++++++++++++++++++++

Numeric types are the most difficult to test for two reasons: there are several different types of numeric values (e.g. ``int``, ``float``, and all of the NumPy types) and rounding errors can occur based on how and when students choose to round off calculations. For these reasons, unless you're working with integers, it is usually easiest to use functions that return boolean values to compare numeric values.

When working with integers, almost all data types that represent them have the same string representation. For this reason, it is a relatively easy thing to write doctests that compare integer values with each other:

.. code-block:: python

    >>> from math import factorial
    >>> factorial(4)
    24
    >>> def square(x):
    ...     return x**2
    >>> square(25)
    625

If, however, it is possible for the numbers to be floating point values, other methods of comparison are better-suited to writing doctests. One of the best examples is NumPy's ``isclose`` function, which compares two values to each other within an adjustable tolerance (which defaults to ``1e-8``). Because NumPy supports both single and double precision floating point values, rounding errors can occur even when performing the same operation on values represented in different precision data types. This is why using functions that perform comparisons and return boolean values is much more robust to all of the ways that students can format their answers.

.. code-block:: python

    >>> def divide(a, b):
    ...     return a / b
    >>> divide(divide(5, 3), 3)      # solution (a)
    0.5555555555555556
    >>> divide(5, 3)                 # solution (b)
    1.6666666666666667
    >>> divide(1.66666667, 3)        # solution (b) cont.
    0.5555555566666667

Note that while solutions (a) and (b) above are both substantially correct, the rounding in solution (b) cause the otputs to be different, so if a test using solution (a) check a student's response solution (b), the student would fail the test. Using a function like ``np.isclose``, this is avoided:

.. code-block:: python

    >>> np.iclose(
    ...     divide(divide(5, 3), 3),    # solution (a)
    ...     divide(1.66666667, 3)       # solution (b)
    ... )
    True

Because booleans have easy-to-compare string representations, this test is much more robust to all of the possible sooutions to this question, and demonstrates the best practice for comparing numeric values. (Note that NumPy also provides ``np.allclose`` for element-wise comparison of values in iterables.)

``str``
+++++++

String comparisons are relatively easy and the most straightfoward because doctests are based on stringe comparison. The main concern is to be careful of leading and trailing whitespace and to note that unless the ``'`` character appears **in** the string, Python's default string delimeters are apostrophes. If both appear, then apostrophes are used and the apostrophe in the string is escaped:

.. code-block:: python

    >>> 'some string'
    'some string'
    >>> "some'string"
    "some'string"
    >>> "some string"
    'some string'
    >>> """some string"""
    'some string'
    >>> '''some string\n'''
    'some string\n'
    >>> '''some string '"\n'''
    'some string \'"\n'

other data types
++++++++++++++++

Other data types don't have very many complexities surrounding them. For custom objects, note what their ``__repr__`` function returns and use that. When creating and testing custom classes, always use a custom ``__repr__`` function, otherwise the representation will contain the pointer to the object in memory, which changes between sessions. 

.. code-block:: python

    >>> class Point:
    ...     def __init__(self, x, y):
    ...         self.x = x
    ...         self.y = y
    >>> Point(1, 2)         # this has no __repr__, so it will have the object id 
    <__main__.Point object at 0x102cb3ac8>
    >>> class OtherPoint:
    ...     def __init__(self, x, y):
    ...         self.x = x
    ...         self.y = y
    ...     def __repr__(self):
    ...         return f"OtherPoint(x={self.x}, y={self.y})"
    >>> OtherPoint(1, 2)   # this has a __repr__, so it will be printed without the id
    OtherPoint(x=1, y=2)

**Always test your tests in a Python interpeter if you're unsure about the string representation of an object.** Don't use a Jupyter Notebook or IPython, because they don't necessary have the same output and they have different prompts.
