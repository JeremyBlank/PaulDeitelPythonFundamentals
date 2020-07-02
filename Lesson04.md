# FUNCTIONS

## Defining Functions

```
In [1]: def square(number):
   ...:     """Calculate the square of a number."""
   ...:     return number ** 2
   ...:

In [2]: square(7)
Out[2]: 49

In [3]: square(2.5)
Out[3]: 6.25

In [4]: square?
Signature: square(number)
Docstring: Calculate the square of a number.
File:      c:\windows\system32\<ipython-input-1-3583ec1e96e7>
Type:      function

In [5]: square??
Signature: square(number)
Source:
def square(number):
    """Calculate the square of a number."""
    return number ** 2
File:      c:\windows\system32\<ipython-input-1-3583ec1e96e7>
Type:      function

In [6]: import statistics

In [7]: statistics??
Type:        module
String form: <module 'statistics' from 'C:\\Users\\jrbla\\anaconda3\\lib\\statistics.py'>
File:        c:\users\jrbla\anaconda3\lib\statistics.py
Source:
"""
Basic statistics module.

This module provides functions for calculating statistics of data, including
averages, variance, and standard deviation.

Calculating averages
--------------------

==================  =============================================
Function            Description
==================  =============================================
mean                Arithmetic mean (average) of data.
harmonic_mean       Harmonic mean of data.
median              Median (middle value) of data.
median_low          Low median of data.
median_high         High median of data.
median_grouped      Median, or 50th percentile, of grouped data.
mode                Mode (most common value) of data.
==================  =============================================

Calculate the arithmetic mean ("the average") of data:

>>> mean([-1.0, 2.5, 3.25, 5.75])
2.625


Calculate the standard median of discrete data:

>>> median([2, 3, 4, 5])
3.5


Calculate the median, or 50th percentile, of data grouped into class intervals
centred on the data values provided. E.g. if your data points are rounded to
the nearest whole number:

>>> median_grouped([2, 2, 3, 3, 3, 4])  #doctest: +ELLIPSIS
2.8333333333...

This should be interpreted in this way: you have two data points in the class
interval 1.5-2.5, three data points in the class interval 2.5-3.5, and one in
the class interval 3.5-4.5. The median of these data points is 2.8333...


Calculating variability or spread
---------------------------------

==================  =============================================
Function            Description
==================  =============================================
pvariance           Population variance of data.
variance            Sample variance of data.
pstdev              Population standard deviation of data.
stdev               Sample standard deviation of data.
==================  =============================================

Calculate the standard deviation of sample data:

>>> stdev([2.5, 3.25, 5.5, 11.25, 11.75])  #doctest: +ELLIPSIS
4.38961843444...

If you have previously calculated the mean, you can pass it as the optional
second argument to the four "spread" functions to avoid recalculating it:

>>> data = [1, 2, 2, 4, 4, 4, 5, 6]
>>> mu = mean(data)
>>> pvariance(data, mu)
2.5


Exceptions
----------

A single exception is defined: StatisticsError is a subclass of ValueError.

"""

__all__ = [ 'StatisticsError',
            'pstdev', 'pvariance', 'stdev', 'variance',
            'median',  'median_low', 'median_high', 'median_grouped',
            'mean', 'mode', 'harmonic_mean',
          ]

import collections
import math
import numbers

from fractions import Fraction
from decimal import Decimal
from itertools import groupby
from bisect import bisect_left, bisect_right



# === Exceptions ===

class StatisticsError(ValueError):
    pass


# === Private utilities ===

def _sum(data, start=0):
    """_sum(data [, start]) -> (type, sum, count)

    Return a high-precision sum of the given numeric data as a fraction,
    together with the type to be converted to and the count of items.

    If optional argument ``start`` is given, it is added to the total.
    If ``data`` is empty, ``start`` (defaulting to 0) is returned.


    Examples
    --------

    >>> _sum([3, 2.25, 4.5, -0.5, 1.0], 0.75)
    (<class 'float'>, Fraction(11, 1), 5)

    Some sources of round-off error will be avoided:

    # Built-in sum returns zero.
    >>> _sum([1e50, 1, -1e50] * 1000)
    (<class 'float'>, Fraction(1000, 1), 3000)

    Fractions and Decimals are also supported:

    >>> from fractions import Fraction as F
    >>> _sum([F(2, 3), F(7, 5), F(1, 4), F(5, 6)])
    (<class 'fractions.Fraction'>, Fraction(63, 20), 4)

    >>> from decimal import Decimal as D
    >>> data = [D("0.1375"), D("0.2108"), D("0.3061"), D("0.0419")]
    >>> _sum(data)
    (<class 'decimal.Decimal'>, Fraction(6963, 10000), 4)

    Mixed types are currently treated as an error, except that int is
    allowed.
    """
    count = 0
    n, d = _exact_ratio(start)
    partials = {d: n}
    partials_get = partials.get
    T = _coerce(int, type(start))
    for typ, values in groupby(data, type):
        T = _coerce(T, typ)  # or raise TypeError
        for n,d in map(_exact_ratio, values):
            count += 1
            partials[d] = partials_get(d, 0) + n
    if None in partials:
        # The sum will be a NAN or INF. We can ignore all the finite
        # partials, and just look at this special one.
        total = partials[None]
        assert not _isfinite(total)
    else:
        # Sum all the partial sums using builtin sum.
        # FIXME is this faster if we sum them in order of the denominator?
        total = sum(Fraction(n, d) for d, n in sorted(partials.items()))
    return (T, total, count)


def _isfinite(x):
    try:
        return x.is_finite()  # Likely a Decimal.
    except AttributeError:
        return math.isfinite(x)  # Coerces to float first.


def _coerce(T, S):
    """Coerce types T and S to a common type, or raise TypeError.

    Coercion rules are currently an implementation detail. See the CoerceTest
    test class in test_statistics for details.
    """
    # See http://bugs.python.org/issue24068.
    assert T is not bool, "initial type T is bool"
    # If the types are the same, no need to coerce anything. Put this
    # first, so that the usual case (no coercion needed) happens as soon
    # as possible.
    if T is S:  return T
    # Mixed int & other coerce to the other type.
    if S is int or S is bool:  return T
    if T is int:  return S
    # If one is a (strict) subclass of the other, coerce to the subclass.
    if issubclass(S, T):  return S
    if issubclass(T, S):  return T
    # Ints coerce to the other type.
    if issubclass(T, int):  return S
    if issubclass(S, int):  return T
    # Mixed fraction & float coerces to float (or float subclass).
    if issubclass(T, Fraction) and issubclass(S, float):
        return S
    if issubclass(T, float) and issubclass(S, Fraction):
        return T
    # Any other combination is disallowed.
    msg = "don't know how to coerce %s and %s"
    raise TypeError(msg % (T.__name__, S.__name__))


def _exact_ratio(x):
    """Return Real number x to exact (numerator, denominator) pair.

    >>> _exact_ratio(0.25)
    (1, 4)

    x is expected to be an int, Fraction, Decimal or float.
    """
    try:
        # Optimise the common case of floats. We expect that the most often
        # used numeric type will be builtin floats, so try to make this as
        # fast as possible.
        if type(x) is float or type(x) is Decimal:
            return x.as_integer_ratio()
        try:
            # x may be an int, Fraction, or Integral ABC.
            return (x.numerator, x.denominator)
        except AttributeError:
            try:
                # x may be a float or Decimal subclass.
                return x.as_integer_ratio()
            except AttributeError:
                # Just give up?
                pass
    except (OverflowError, ValueError):
        # float NAN or INF.
        assert not _isfinite(x)
        return (x, None)
    msg = "can't convert type '{}' to numerator/denominator"
    raise TypeError(msg.format(type(x).__name__))


def _convert(value, T):
    """Convert value to given numeric type T."""
    if type(value) is T:
        # This covers the cases where T is Fraction, or where value is
        # a NAN or INF (Decimal or float).
        return value
    if issubclass(T, int) and value.denominator != 1:
        T = float
    try:
        # FIXME: what do we do if this overflows?
        return T(value)
    except TypeError:
        if issubclass(T, Decimal):
            return T(value.numerator)/T(value.denominator)
        else:
            raise


def _counts(data):
    # Generate a table of sorted (value, frequency) pairs.
    table = collections.Counter(iter(data)).most_common()
    if not table:
        return table
    # Extract the values with the highest frequency.
    maxfreq = table[0][1]
    for i in range(1, len(table)):
        if table[i][1] != maxfreq:
            table = table[:i]
            break
    return table


def _find_lteq(a, x):
    'Locate the leftmost value exactly equal to x'
    i = bisect_left(a, x)
    if i != len(a) and a[i] == x:
        return i
    raise ValueError


def _find_rteq(a, l, x):
    'Locate the rightmost value exactly equal to x'
    i = bisect_right(a, x, lo=l)
    if i != (len(a)+1) and a[i-1] == x:
        return i-1
    raise ValueError


def _fail_neg(values, errmsg='negative value'):
    """Iterate over values, failing if any are less than zero."""
    for x in values:
        if x < 0:
            raise StatisticsError(errmsg)
        yield x


# === Measures of central tendency (averages) ===

def mean(data):
    """Return the sample arithmetic mean of data.

    >>> mean([1, 2, 3, 4, 4])
    2.8

    >>> from fractions import Fraction as F
    >>> mean([F(3, 7), F(1, 21), F(5, 3), F(1, 3)])
    Fraction(13, 21)

    >>> from decimal import Decimal as D
    >>> mean([D("0.5"), D("0.75"), D("0.625"), D("0.375")])
    Decimal('0.5625')

    If ``data`` is empty, StatisticsError will be raised.
    """
    if iter(data) is data:
        data = list(data)
    n = len(data)
    if n < 1:
        raise StatisticsError('mean requires at least one data point')
    T, total, count = _sum(data)
    assert count == n
    return _convert(total/n, T)


def harmonic_mean(data):
    """Return the harmonic mean of data.

    The harmonic mean, sometimes called the subcontrary mean, is the
    reciprocal of the arithmetic mean of the reciprocals of the data,
    and is often appropriate when averaging quantities which are rates
    or ratios, for example speeds. Example:

    Suppose an investor purchases an equal value of shares in each of
    three companies, with P/E (price/earning) ratios of 2.5, 3 and 10.
    What is the average P/E ratio for the investor's portfolio?

    >>> harmonic_mean([2.5, 3, 10])  # For an equal investment portfolio.
    3.6

    Using the arithmetic mean would give an average of about 5.167, which
    is too high.

    If ``data`` is empty, or any element is less than zero,
    ``harmonic_mean`` will raise ``StatisticsError``.
    """
    # For a justification for using harmonic mean for P/E ratios, see
    # http://fixthepitch.pellucid.com/comps-analysis-the-missing-harmony-of-summary-statistics/
    # http://papers.ssrn.com/sol3/papers.cfm?abstract_id=2621087
    if iter(data) is data:
        data = list(data)
    errmsg = 'harmonic mean does not support negative values'
    n = len(data)
    if n < 1:
        raise StatisticsError('harmonic_mean requires at least one data point')
    elif n == 1:
        x = data[0]
        if isinstance(x, (numbers.Real, Decimal)):
            if x < 0:
                raise StatisticsError(errmsg)
            return x
        else:
            raise TypeError('unsupported type')
    try:
        T, total, count = _sum(1/x for x in _fail_neg(data, errmsg))
    except ZeroDivisionError:
        return 0
    assert count == n
    return _convert(n/total, T)


# FIXME: investigate ways to calculate medians without sorting? Quickselect?
def median(data):
    """Return the median (middle value) of numeric data.

    When the number of data points is odd, return the middle data point.
    When the number of data points is even, the median is interpolated by
    taking the average of the two middle values:

    >>> median([1, 3, 5])
    3
    >>> median([1, 3, 5, 7])
    4.0

    """
    data = sorted(data)
    n = len(data)
    if n == 0:
        raise StatisticsError("no median for empty data")
    if n%2 == 1:
        return data[n//2]
    else:
        i = n//2
        return (data[i - 1] + data[i])/2


def median_low(data):
    """Return the low median of numeric data.

    When the number of data points is odd, the middle value is returned.
    When it is even, the smaller of the two middle values is returned.

    >>> median_low([1, 3, 5])
    3
    >>> median_low([1, 3, 5, 7])
    3

    """
    data = sorted(data)
    n = len(data)
    if n == 0:
        raise StatisticsError("no median for empty data")
    if n%2 == 1:
        return data[n//2]
    else:
        return data[n//2 - 1]


def median_high(data):
    """Return the high median of data.

    When the number of data points is odd, the middle value is returned.
    When it is even, the larger of the two middle values is returned.

    >>> median_high([1, 3, 5])
    3
    >>> median_high([1, 3, 5, 7])
    5

    """
    data = sorted(data)
    n = len(data)
    if n == 0:
        raise StatisticsError("no median for empty data")
    return data[n//2]


def median_grouped(data, interval=1):
    """Return the 50th percentile (median) of grouped continuous data.

    >>> median_grouped([1, 2, 2, 3, 4, 4, 4, 4, 4, 5])
    3.7
    >>> median_grouped([52, 52, 53, 54])
    52.5

    This calculates the median as the 50th percentile, and should be
    used when your data is continuous and grouped. In the above example,
    the values 1, 2, 3, etc. actually represent the midpoint of classes
    0.5-1.5, 1.5-2.5, 2.5-3.5, etc. The middle value falls somewhere in
    class 3.5-4.5, and interpolation is used to estimate it.

    Optional argument ``interval`` represents the class interval, and
    defaults to 1. Changing the class interval naturally will change the
    interpolated 50th percentile value:

    >>> median_grouped([1, 3, 3, 5, 7], interval=1)
    3.25
    >>> median_grouped([1, 3, 3, 5, 7], interval=2)
    3.5

    This function does not check whether the data points are at least
    ``interval`` apart.
    """
    data = sorted(data)
    n = len(data)
    if n == 0:
        raise StatisticsError("no median for empty data")
    elif n == 1:
        return data[0]
    # Find the value at the midpoint. Remember this corresponds to the
    # centre of the class interval.
    x = data[n//2]
    for obj in (x, interval):
        if isinstance(obj, (str, bytes)):
            raise TypeError('expected number but got %r' % obj)
    try:
        L = x - interval/2  # The lower limit of the median interval.
    except TypeError:
        # Mixed type. For now we just coerce to float.
        L = float(x) - float(interval)/2

    # Uses bisection search to search for x in data with log(n) time complexity
    # Find the position of leftmost occurrence of x in data
    l1 = _find_lteq(data, x)
    # Find the position of rightmost occurrence of x in data[l1...len(data)]
    # Assuming always l1 <= l2
    l2 = _find_rteq(data, l1, x)
    cf = l1
    f = l2 - l1 + 1
    return L + interval*(n/2 - cf)/f


def mode(data):
    """Return the most common data point from discrete or nominal data.

    ``mode`` assumes discrete data, and returns a single value. This is the
    standard treatment of the mode as commonly taught in schools:

    >>> mode([1, 1, 2, 3, 3, 3, 3, 4])
    3

    This also works with nominal (non-numeric) data:

    >>> mode(["red", "blue", "blue", "red", "green", "red", "red"])
    'red'

    If there is not exactly one most common value, ``mode`` will raise
    StatisticsError.
    """
    # Generate a table of sorted (value, frequency) pairs.
    table = _counts(data)
    if len(table) == 1:
        return table[0][0]
    elif table:
        raise StatisticsError(
                'no unique mode; found %d equally common values' % len(table)
                )
    else:
        raise StatisticsError('no mode for empty data')


# === Measures of spread ===

# See http://mathworld.wolfram.com/Variance.html
#     http://mathworld.wolfram.com/SampleVariance.html
#     http://en.wikipedia.org/wiki/Algorithms_for_calculating_variance
#
# Under no circumstances use the so-called "computational formula for
# variance", as that is only suitable for hand calculations with a small
# amount of low-precision data. It has terrible numeric properties.
#
# See a comparison of three computational methods here:
# http://www.johndcook.com/blog/2008/09/26/comparing-three-methods-of-computing-standard-deviation/

def _ss(data, c=None):
    """Return sum of square deviations of sequence data.

    If ``c`` is None, the mean is calculated in one pass, and the deviations
    from the mean are calculated in a second pass. Otherwise, deviations are
    calculated from ``c`` as given. Use the second case with care, as it can
    lead to garbage results.
    """
    if c is None:
        c = mean(data)
    T, total, count = _sum((x-c)**2 for x in data)
    # The following sum should mathematically equal zero, but due to rounding
    # error may not.
    U, total2, count2 = _sum((x-c) for x in data)
    assert T == U and count == count2
    total -=  total2**2/len(data)
    assert not total < 0, 'negative sum of square deviations: %f' % total
    return (T, total)


def variance(data, xbar=None):
    """Return the sample variance of data.

    data should be an iterable of Real-valued numbers, with at least two
    values. The optional argument xbar, if given, should be the mean of
    the data. If it is missing or None, the mean is automatically calculated.

    Use this function when your data is a sample from a population. To
    calculate the variance from the entire population, see ``pvariance``.

    Examples:

    >>> data = [2.75, 1.75, 1.25, 0.25, 0.5, 1.25, 3.5]
    >>> variance(data)
    1.3720238095238095

    If you have already calculated the mean of your data, you can pass it as
    the optional second argument ``xbar`` to avoid recalculating it:

    >>> m = mean(data)
    >>> variance(data, m)
    1.3720238095238095

    This function does not check that ``xbar`` is actually the mean of
    ``data``. Giving arbitrary values for ``xbar`` may lead to invalid or
    impossible results.

    Decimals and Fractions are supported:

    >>> from decimal import Decimal as D
    >>> variance([D("27.5"), D("30.25"), D("30.25"), D("34.5"), D("41.75")])
    Decimal('31.01875')

    >>> from fractions import Fraction as F
    >>> variance([F(1, 6), F(1, 2), F(5, 3)])
    Fraction(67, 108)

    """
    if iter(data) is data:
        data = list(data)
    n = len(data)
    if n < 2:
        raise StatisticsError('variance requires at least two data points')
    T, ss = _ss(data, xbar)
    return _convert(ss/(n-1), T)


def pvariance(data, mu=None):
    """Return the population variance of ``data``.

    data should be an iterable of Real-valued numbers, with at least one
    value. The optional argument mu, if given, should be the mean of
    the data. If it is missing or None, the mean is automatically calculated.

    Use this function to calculate the variance from the entire population.
    To estimate the variance from a sample, the ``variance`` function is
    usually a better choice.

    Examples:

    >>> data = [0.0, 0.25, 0.25, 1.25, 1.5, 1.75, 2.75, 3.25]
    >>> pvariance(data)
    1.25

    If you have already calculated the mean of the data, you can pass it as
    the optional second argument to avoid recalculating it:

    >>> mu = mean(data)
    >>> pvariance(data, mu)
    1.25

    This function does not check that ``mu`` is actually the mean of ``data``.
    Giving arbitrary values for ``mu`` may lead to invalid or impossible
    results.

    Decimals and Fractions are supported:

    >>> from decimal import Decimal as D
    >>> pvariance([D("27.5"), D("30.25"), D("30.25"), D("34.5"), D("41.75")])
    Decimal('24.815')

    >>> from fractions import Fraction as F
    >>> pvariance([F(1, 4), F(5, 4), F(1, 2)])
    Fraction(13, 72)

    """
    if iter(data) is data:
        data = list(data)
    n = len(data)
    if n < 1:
        raise StatisticsError('pvariance requires at least one data point')
    T, ss = _ss(data, mu)
    return _convert(ss/n, T)


def stdev(data, xbar=None):
    """Return the square root of the sample variance.

    See ``variance`` for arguments and other details.

    >>> stdev([1.5, 2.5, 2.5, 2.75, 3.25, 4.75])
    1.0810874155219827

    """
    var = variance(data, xbar)
    try:
        return var.sqrt()
    except AttributeError:
        return math.sqrt(var)


def pstdev(data, mu=None):
    """Return the square root of the population variance.

    See ``pvariance`` for arguments and other details.

    >>> pstdev([1.5, 2.5, 2.5, 2.75, 3.25, 4.75])
    0.986893273527251

    """
    var = pvariance(data, mu)
    try:
        return var.sqrt()
    except AttributeError:
        return math.sqrt(var)
```

### Self Check

> Define a function square_root that receives a number as a parameter and returns the square root of that number.
> Determine the square root of 6.25

```

MY ANSWER

In [1]: def square_root(number):
   ...:     """Square root of a number."""
   ...:     return number ** 2
   ...:

In [2]: square_root(6.25)
Out[2]: 39.0625

In [3]:
```

```
PAULS ANSWER

In [1]: def square_root(number):
   ...:     return number ** 0.5 # or number ** (1 / 2)
   ...:

In [2]: square_root(6.25)
Out[2]: 2.5

In [3]:  
```

## Functions with Multiple Parameters

```
In [1]: def maximum(value1, value2, value3):
   ...:     """Return the maximum of the three values."""
   ...:     max_value = value1
   ...:     if value2 > max_value:
   ...:         max_value = value2
   ...:     if value3 > max_value:
   ...:         max_value = value3
   ...:     return max_value
   ...:

In [2]: maximum(12, 27, 36)
Out[2]: 36

In [3]: maximum(12.3, 45.6, 9.7)
Out[3]: 45.6

In [4]: maximum('yellow', 'red', 'orange')
Out[4]: 'yellow'

In [5]: maximum(13.5, -3, 7)
Out[5]: 13.5

In [6]: max('yellow', 'red', 'orange', 'blue', 'green')
Out[6]: 'yellow'

In [7]: min(15, 9, 27, 14)
Out[7]: 9

In [8]:  
```

### Self Check

> Call function max with the list [14, 27, 5, 3] as an argument
> Call function min with the string 'orange' as an argument

```
In [1]: max([14, 27, 5, 3])
Out[1]: 27

In [2]: min('orange')
Out[2]: 'a'

In [3]:     
```

## Random-Number Generation

```
In [1]: import random

In [2]: for roll in range(10):
   ...:     print(random.randrange(1, 7), end=' ')
   ...:
1 1 1 3 2 2 3 6 5 4
In [3]: for roll in range(10):
   ...:     print(random.randrange(1, 7), end=' ')
   ...:
2 2 2 1 5 6 3 5 6 4
In [4]: run fig04_01.py
Face    Frequency
   1       998938
   2       999483
   3       998624
   4      1000972
   5       999974
   6      1002009

In [5]: run fig04_01.py
Face    Frequency
   1      1000252
   2       997385
   3       999914
   4       999799
   5      1000130
   6      1002520

In [6]:  
```

```
"""Roll a six-sided die 6,000,000 times."""

import random

# Face frequency counters
frequency1 = 0
frequency2 = 0
frequency3 = 0
frequency4 = 0
frequency5 = 0
frequency6 = 0

# 6,000,000 die rolls
for roll in range(6_000_000): # Note underscore separators
    face = random.randrange(1, 7)

    # Increment appropriate face counter
    if face == 1:
        frequency1 += 1
    elif face == 2:
        frequency2 += 1
    elif face == 3:
        frequency3 += 1
    elif face == 4:
        frequency4 += 1
    elif face == 5:
        frequency5 += 1
    elif face == 6:
        frequency6 += 1

print(f'Face{"Frequency":>13}')
print(f'{1:>4}{frequency1:>13}')
print(f'{2:>4}{frequency2:>13}')
print(f'{3:>4}{frequency3:>13}')
print(f'{4:>4}{frequency4:>13}')
print(f'{5:>4}{frequency5:>13}')
print(f'{6:>4}{frequency6:>13}')
```
### Description
> :>13 == right aligned 13 times
> :>4 == right aligned 4 times

```
In [1]: import random

In [2]: for roll in range(10):
   ...:     print(random.randrange(1,7), end=' ')
   ...:
2 2 2 5 2 2 5 2 3 1
In [3]: for roll in range(10):
   ...:     print(random.randrange(1,7), end=' ')
   ...:
4 1 6 5 1 6 3 5 6 6
In [4]: run fig04_01.py
Face    Frequency
   1       998966
   2      1000990
   3       999062
   4      1001866
   5       999012
   6      1000104

In [5]: run fig04_01.py
Face    Frequency
   1      1000064
   2      1000676
   3      1000448
   4       999779
   5       999372
   6       999661

In [6]: random.seed(32)

In [7]: for roll in range(10):
   ...:     print(random.randrange(1,7), end=' ')
   ...:
1 2 2 3 6 2 4 1 6 1
In [8]: for roll in range(10):
   ...:     print(random.randrange(1,7), end=' ')
   ...:
1 3 5 3 1 5 6 4 3 5
In [9]: random.seed(32)

In [10]: for roll in range(10):
    ...:     print(random.randrange(1,7), end=' ')
    ...:
1 2 2 3 6 2 4 1 6 1
In [11]:   
```

### Self Check

Requirements statement: Use a for statement, randrange, and a conditional expression
(Introduced in the preceding chapter) to simulate 20 coin flips, displaying H for heads and T for tails
all on the same line, separated by space. 

```
In [1]: import random

In [2]: for i in range(20):
   ...:     print('H' if random.randrange(2) == 0 else 'T', end=' ')
   ...:
T H T H H T T T H H T T T H H T T T T H
In [3]:
```

```
In [1]: for i in range(20):
   ...:     if random.randrange(2) == 0:
   ...:         print('H')
   ...:     else:
   ...:         print('T')
   ...:
T
T
H
H
T
T
T
T
T
H
H
H
T
T
H
H
T
H
H
H

In [5]: 
```

## Case Study: A Game of Chance ("Craps)

* Roll two six-sided dice, each with faces containing 1 to s spots
* Calculate the sum of the spots on the two upward faces
* If the sum is 7 or 11 on the first roll, you win
* If the sum is 2, 3 or 12 on the first roll (called "craps"), you lose
* If the sum is 4, 5, 6, 8, 9 or 10 on the first roll, that
  sum becomes your "point"
* to win, you must continue rolling the dice until you "make
  your point" (i.e., roll the same point value)
* You lose by rolling a 7 before making your point.

### Self Check

> Pack a student tuple with the name 'Sue' and the list [89, 94, 85]
> display the tuple
> unpack it into variables name and grades
> display their values


```
In [1]: student = ('Sue', [89, 94, 85])

In [2]: student

Out[2]: ('Sue', [89, 94, 85])

In [3]: name, grades = student

In [4]: print(f'{name}: {grades}')
Sue: [89, 94, 85]

In [5]:
```

## Math Module Functions 

```
In [1]: import math

In [2]: math.pow?
Signature: math.pow(x, y, /)
Docstring: Return x**y (x to the power of y).
Type:      builtin_function_or_method

In [3]: math.sqrt(900)
Out[3]: 30.0

In [4]: math.fabs(-10)
Out[4]: 10.0

In [5]: math. # Tab button to access features
```

## Default parameter Values

```
In [1]: def rectangle_area(length=2, width=3):
   ...:     """Return a rectangle's area."""
   ...:     return length * width
   ...:

In [2]:  rectangle_area()
Out[3]: 6

In [4]: rectangle_area(10)
Out[4]: 30

In [5]: rectangle_area(10, 5)
Out[5]: 50

In [6]:
```

## Keyword Arguments

```
In [1]: def rectangle_area(length, width):
   ...:     """Return a rectangle's area."""
   ...:     return length * width
   ...:

In [2]: rectangle_area(length=10, width=3)
Out[2]: 30

In [3]: rectangle_area(width=2, length=10)
Out[3]: 20

In [4]: def rectangle_area(length=1, width=1):
   ...:     """Return a rectangle's area."""
   ...:     return length * width
   ...:

In [5]: rectangle_area()
Out[5]: 1

In [6]: rectangle_area(width=20)
Out[6]: 20

In [7]: rectangle_area(length=10)
Out[7]: 10

In [8]: def rectangle_area(length, width=1):
   ...:     """Return a rectangle's area."""
   ...:     return length * width
   ...:
```

## Arbitrary Argument Lists

```
In [1]: def average(*args):
   ...:     return sum(args) / len(args)
   ...:

In [2]: average(5, 10)
Out[2]: 7.5

In [3]: average(5, 10, 15)
Out[3]: 10.0

In [4]: average(5, 10, 15, 20)
Out[4]: 12.5

In [5]: average()
---------------------------------------------------------------------------
ZeroDivisionError                         Traceback (most recent call last)
<ipython-input-5-c4feed39dc61> in <module>
----> 1 average()

<ipython-input-1-d45655d3ef57> in average(*args)
      1 def average(*args):
----> 2     return sum(args) / len(args)
      3

ZeroDivisionError: division by zero

In [6]: grades = [88, 75, 96, 55, 83]

In [7]: average(*grades)
Out[7]: 79.4

In [8]:
```

### Self Check

> Create a function named calculate_product that receives an arbitrary argument list and returns the product
> of all the arguments.
> Call the function with the arguments 10, 20 and 30
> then with the sequence of integers produced by range(1, 6, 2)

```
In [1]: def calculate_product(*args):
   ...:     product = 1
   ...:     for value in args:
   ...:         product *= value
   ...:     return product
   ...:

In [2]: calculate_product(10, 20, 30)
Out[2]: 6000

In [3]: calculate_product(*range(1, 6, 2))
Out[3]: 15

In [4]:
```
# Methods: Functions That Belong to Objects

https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str

```
In [1]: s = 'Hello'

In [2]: s.lower()
Out[2]: 'hello'

In [3]: s.upper()
Out[3]: 'HELLO'

In [4]: 
```

## Scope Rules

* Each identifier has a scope
    * Determines where you can use it in your program
* Local Scope
    * Local variable identifier
    * "in scope" only from its definition to the end of a function's block
* Global Scope
    * Identifiers defined outside any function (or class)
    * Functions, variables and classes
    * Variables with global scope are global variables
    * Identifiers with global scope can be used in a .py file or interactive session
      anywhere after they're defined

```
In [1]: x = 7

In [2]: def access_global():
   ...:     print('x printed from global scope:', x)
   ...:

In [3]: access_global()
x printed from global scope: 7

In [4]: def try_to_modify_global():
   ...:     x = 3.5
   ...:     print('x printed from try_to_modify_global:', x)
   ...:

In [5]: try_to_modify_global()
x printed from try_to_modify_global: 3.5

In [6]: x
Out[6]: 7

In [7]: def modify_global():
   ...:     global x
   ...:     x = 'hello'
   ...:     print('x printed from modify_global:', x)
   ...:

In [8]: modify_global()
x printed from modify_global: hello

In [9]: x
Out[9]: 'hello'

In [10]: sum  = 10 + 5

In [11]: sum
Out[11]: 15

In [12]: sum([10, 5])
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-12-1237d97a65fb> in <module>
----> 1 sum([10, 5])

TypeError: 'int' object is not callable

In [13]:
Do you really want to exit ([y]/n)? y

(base) C:\Windows\system32>ipython
Python 3.7.7 (default, May  6 2020, 11:45:54) [MSC v.1916 64 bit (AMD64)]
Type 'copyright', 'credits' or 'license' for more information
IPython 7.15.0 -- An enhanced Interactive Python. Type '?' for help.

In [1]: sum([10, 5])
Out[1]: 15

In [2]: type(sum)
Out[2]: builtin_function_or_method 
```

## Import: A Deeper Look

```
In [1]: from math import ceil, floor

In [2]: ceil(10.3)
Out[2]: 11

In [3]: floor(10.7)
Out[3]: 10

In [4]: e = 'hello'

In [5]: from math import *

In [6]: e
Out[6]: 2.718281828459045

In [7]: import statistics as stats

In [8]: grades = [85, 93, 45, 87, 93]

In [9]: stats.mean(grades)
Out[9]: 80.6

In [10]:
```

### Self Check

> Import the decimal module with the shorthand name dec,
> Create a Decimal object with the value 2.5 and square its value

```
In [1]: import decimal as dec

In [2]: dec.Decimal('2.5') ** 2
Out[2]: Decimal('6.25')

In [3]:
```

## Passing arguments to Functions: A Deeper Look

* Pass-by-value
    * called function receives a copy of the argument's value and works
      exclusively with that copy
    * changes to the function's copy do not affect the original variable's
      value in the caller
 * Pass-by-reference
     * called function can access the argument's value in the caller
       directly and modify the value if it's mutable
 * Python arguments are always passed by reference
 * Python copies the argument object's reference--not the object itself--into the
   corresponding parameter
   
 Memory Addresses and "Pointers"  
 
 Variable       Object
    x       -->  7
 
```
In [1]: x = 7

In [2]: id(x)
Out[2]: 140734765113936

In [3]: def cube(number):
   ...:     print('id(number):', id(number))
   ...:     return number ** 3
   ...:

In [4]: cube(7)
id(number): 140734765113936
Out[4]: 343

In [5]: def cube(number):
   ...:     print('number is x:', number is x) # x is a global variable
   ...:     return number ** 3
   ...:
   ...:

In [6]: cube(x)
number is x: True
Out[6]: 343

In [7]: def cube(number):
   ...:     print('id(number) before modifying number:', id(number))
   ...:     number **= 3
   ...:     print('id(number) after modying number:', id(number))
   ...:     return number
   ...:

In [8]: cube(x)
id(number) before modifying number: 140734765113936
id(number) after modying number: 1545007611088
Out[8]: 343

In [9]: id(x)
Out[9]: 140734765113936

In [10]:
```

### Self Check

> Create a variable width with the value 15.5
> Show that modifying the variable creates a new object
> Display width's id and value before and after modifying its value

```
In [1]: width = 15.5

In [2]: print('id:', id(width), ' value:', width)
id: 2279526386544  value: 15.5

In [3]: width *= 3

In [4]: print('id:', id(width), ' value:', width)
id: 2279526386000  value: 46.5

In [5]:     
```

### Functional-Style Programming

* What vs. how
* External iteration
* Internal iteration
* Declarative programming
* Pure functions


```
In [1]: values = [1, 2, 3]

In [2]: sum(values)
Out[2]: 6

In [3]: sum(values)
Out[3]: 6

In [4]: values
Out[4]: [1, 2, 3]

In [5]:   
```

## Intro to Data Science: Measures of Dispersion

* Measures of central tendency - mean, median and mode
* Categorize typical values in a group
    * mean height of your classmates
    * most frequently purchase car brand (the mode) in a given country
* An entire group is called a population
* A subset of a population is called a sample
* Measures of dispersion 
    * Also called measures of variability
    * Help you understand how spred out values are

### Variance

* We'll use the following population of 10 six-sided die rolls
    * 1, 3, 4, 2, 6, 5, 3, 4, 5, 2
* To determine the variance
    * begin with the mean of these values--3.5
    * Subtract the mean from every die value
        * -2.5, -0.5, 0.5, -1.5, 2.5, 1.5, -0.5, 0.5, 1.5, -1.5
    * Square each of these results (yielding only positives):
        * 6.25, 0.25, 0.25, 2.2.5, 6.25, 2.25, 0.25, 0.25, 2.25, 2.25
    * Calculate the mean of these squares, which is 2.25 (22.5 / 10)
    * This is the populatin variance
 * Squaring the differences emphasizes outliers-the values that are farthest from the mean
 * In data analytics, sometiems we'll want to pay careful attention to outliers,
   and sometimes we'll want to ignore them

### Standard Deviation
* The standard deviation is the square root of the variance
  (In this case, 1.5)
  * Tones down the effect of the outliers
* The smaller the variance and standard deviation are, the closer the data values are to the 
  mean and the less overall dispersion (that is, spread) there is between the values and the mean

### Advantage of Standard Deviation vs. Variance

* Standard deviation has same unites as your original measurements
* Suppose you've recorded the March Fahrenheit temperatures in your area
    * 31 numbers like 19, 32, 28, 35
    * Unites are degrees
    * When you square your temperatures to calculate the population variance,
      the unites of the population variance become "degrees squared"
    * When you take the square root of the population variance to calculate the population
      standard deviation, the units once again become degrees, which are the same 
      units as your temperatures
 