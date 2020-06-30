# CONTROL STATEMENTS

## If Statements

```
Anaconda


In [1]: grade = 85

In [2]: if grade >= 60:
   ...:     print('Passed')
   ...:
Passed

In [3]: if grade >= 60:
   ...:     print('Passed')
   ...:     print('Good job')
   ...:
Passed
Good job

In [4]: if 1:
   ...:     print('Nonzero values are True, so this will print')
   ...:
Nonzero values are True, so this will print

In [5]: if 0:
   ...:     print('Zero is False, so this will not print')
   ...:

In [6]: grade == 85
Out[6]: True

In [7]: grade == 58
Out[7]: False

In [8]: grade = 58

In [9]: x == 85
---------------------------------------------------------------------------
NameError                                 Traceback (most recent call last)
<ipython-input-9-3fee78e80568> in <module>
----> 1 x == 85

NameError: name 'x' is not defined

In [10]:
```

## if...else and if...elif... else statements

```
Anaconda


In [1]: grade = 85

In [2]: if grade >= 60:
   ...:     print('passed')
   ...: else:
   ...:     print('failed')
   ...:
passed

In [3]: grade = 57

In [4]: if grade >= 60:
   ...:     print('passed')
   ...: else:
   ...:     print('failed')
   ...:
failed

In [5]: if grade >= 60:
   ...:     result = 'passed'
   ...: else:
   ...:     result = 'failed'
   ...:

In [6]: result
Out[6]: 'failed'

In [7]: result = ('Passed' if grade => 60 else 'Failed')
  File "<ipython-input-7-fdb166dd3dab>", line 1
    result = ('Passed' if grade => 60 else 'Failed')
                                ^
SyntaxError: invalid syntax


In [8]: result = ('Passed' if grade >= 60 else 'Failed')

In [9]: result
Out[9]: 'Failed'

In [10]: grade = 49

In [11]: if grade >= 60:
    ...:     print('passed')
    ...: else:
    ...:     print('failed')
    ...:     print('You must take this course again')
    ...:
    ...:
failed
You must take this course again

In [12]: if grade >= 60:
    ...:     print('passed')
    ...: else:
    ...:     print('failed')
    ...:       print('You must take this course again')
    ...:
  File "<ipython-input-12-2760c1ee318d>", line 5
    print('You must take this course again')
    ^
IndentationError: unexpected indent


In [13]: grade = 77

In [14]: if grade >= 90:
    ...:     print('A')
    ...: elif grade >= 80:
    ...:     print('B')
    ...: elif grade >= 70:
    ...:     print('C')
    ...: elif grade >= 60:
    ...:     print('D')
    ...: else:
    ...:     print('F)
  File "<ipython-input-14-107593fbbb75>", line 10
    print('F)
             ^
SyntaxError: EOL while scanning string literal


In [15]: if grade >= 90:
    ...:     print('A')
    ...: elif grade >= 80:
    ...:     print('B')
    ...: elif grade >= 70:
    ...:     print('C')
    ...: elif grade >= 60:
    ...:     print('D')
    ...: else:
    ...:     print('F')
    ...:
C

In [16]:
```

## While statement

```

Anaconda


In [1]: product = 3

In [2]: while product <= 50:
   ...:     product = product * 3
   ...:

In [3]: product
Out[3]: 81

In [4]:
```

### Self Check

> Write a statement that determines the first power of 7 greater than 1000:

```
Anaconda

In [1]: number = 7

In [2]: while number <= 1000:
   ...:     number = number * 7
   ...:

In [3]: number
Out[3]: 2401
```

### For statement; Iterables, Lists and iterators; Built-in range Function

```

Anaconda

In [1]: for character in 'Programming':
   ...:     print(character, end=' ')
   ...:
P r o g r a m m i n g
In [2]: print(10, 20, 30, sep=', ')
10, 20, 30

In [3]: total = 0

In [4]: for number in [2, -3, 0, 17, 9]:
   ...:     total = total + number
   ...:

In [5]: total
Out[5]: 25

In [6]: for counter in range(10):
   ...:     print(counter, end=' ')
   ...:
0 1 2 3 4 5 6 7 8 9
In [7]:
```

### Self Check

> Use the range function and a for statement to calculate the total of the integers from 0 through 1,000,000.



```
Anaconda 

In [1]: total = 0

In [2]: for number in range(10000001):
   ...:     total = total + number
   ...:

In [3]: total
Out[3]: 50000005000000
```


## Augmented Assignments

Augmented       Sample          Explanation     Assigns
Assignment      Expression      

Assume: c = 3, d = 5, e = 4, f = 2, g = 9, h = 12

+=              c += 7          c = c + 7       10 to c
-=              d -= 4          d = d - 4       1 to d
*=              e *= 5          e = e * 5       20 to e
**=             f **= 3         f = f ** 3      8 to f
/=              g /= 2          g = g / 2       4.5 to g
//=             g //= 2         g = g // 2      4 to g
%=              h %= 9          h = h % 9       3 to h

```
Anaconda

In [1]: total = 0

In [2]: for number in [1, 2, 3, 4, 5,]:
   ...:     total += number
   ...:

In [3]: total
Out[3]: 15

In [4]:  
```

### Self Check

> Create a variable x with the value 12. Use an exponentiation augmented
> assignment statement to square x's value. Show x's new value. 


```
Anaconda

In [1]: x = 12

In [2]: x **= 2

In [3]: x
Out[3]: 144

In [4]:   
```