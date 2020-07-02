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

Sequence-Controlled Iteration

### fig03_01.py

```
Anaconda

"""Class average program with sequence-controlled repetition."""

# Initialization phase
total = 0 # sum of grades
grade_counter = 0
grades = [98, 76, 71, 87, 83, 90, 57, 79, 82, 94] # list of grades

# Processing phase
for grade in grades:
    total += grade # Add current grade to the running total
    grade_counter += 1 # Indicate that one more grade was processed

# Termination phase
average = total / grade_counter
print(f'Class average is {average}')
```

### To run

> (venv) C:\Users\jrbla\PycharmProjects\PaulDeitelPythonFundamentals\examples\ch02>dir
> python filename...
>

### Self Check

> Display an f-string in which you insert the values of the variables number1 (7) and number2 (5)
> and their product.
> This displayed string should be 
> 7 times 5 is 35

```

Anaconda


MY ANSWER

In [1]: number1 = 7

In [2]: number2 = 5

In [3]: answer = number1 * number2

In [4]: print(f'7 times 5 is {answer}')
7 times 5 is 35

In [5]:

PAUL'S ANSWER

In [1]: number1 = 7

In [2]: number2 = 5

In [3]: print(f'{number1} times {number2} is {number1 * number2}')
7 times 5 is 35

In [4]:
```

### Self Check 

(base) C:\Windows\system32>cd C:\Users\jrbla\PycharmProjects\PaulDeitelPythonFundamentals\examples\ch03

(base) C:\Users\jrbla\PycharmProjects\PaulDeitelPythonFundamentals\examples\ch03>dir
 Volume in drive C is Windows
 Volume Serial Number is 301B-803F

 Directory of C:\Users\jrbla\PycharmProjects\PaulDeitelPythonFundamentals\examples\ch03

06/25/2020  06:57 PM    <DIR>          .
06/25/2020  06:57 PM    <DIR>          ..
06/25/2020  01:30 PM             1,564 fig03_01.py
06/25/2020  01:30 PM             1,626 fig03_02.py
06/25/2020  06:57 PM    <DIR>          snippets_ipynb
06/25/2020  06:57 PM    <DIR>          snippets_py
               2 File(s)          3,190 bytes
               4 Dir(s)  366,054,731,776 bytes free

(base) C:\Users\jrbla\PycharmProjects\PaulDeitelPythonFundamentals\examples\ch03>python fig03_02.py
Enter grade, -1 to end: 97
Enter grade, -1 to end: 88
Enter grade, -1 to end: 72
Enter grade, -1 to end: -1
Class average is 85.67


## Built-In Function ragne: A Deeper Look

```
Anaconda

In [1]: for number in range(5, 10):
   ...:     print(number, end=' ')
   ...:
5 6 7 8 9
In [2]: for number in range(0, 10,2):
   ...:     print(number, end=' ')
   ...:
0 2 4 6 8
In [3]: for number in range(10, 0, -2):
   ...:     print(number, end=' ')
   ...:
10 8 6 4 2
In [4]:
```

### Self Check

> What happens if you try to print the items in range(10, 0, 2)?

```
In [1]: for number in range(10, 0, 2):
   ...:     print(number, end=' ')
   ...:

In [2]:   
```

> User a for statement, range and print to display on one line the sequence of values 99 88 77 66 55 44 33 22 11 0,
>each separated by one space.

```
In [1]: for number in range(99, -1, -11): # 99 is top number, -1 because we want to include 0, print in groups of -11
   ...:     print(number, end=' ')
   ...:
99 88 77 66 55 44 33 22 11 0
In [2]:   
```

> Use for and range to sum the even integers from 2 through 100, then display the sum

```
MY ANSWER

In [1]: for number in range(2, 100, 2):
   ...:     print(number, end=' ')
   ...:
2 4 6 8 10 12 14 16 18 20 22 24 26 28 30 32 34 36 38 40 42 44 46 48 50 52 54 56 58 60 62 64 66 68 70 72 74 76 78 80 82 84 86 88 90 92 94 96 98
In [2]:   
```

```
PAULS ANSWER

In [1]: total = 0

In [2]: for number in range(2, 101, 2):
   ...:     total += number
   ...: total
Out[2]: 2550

In [3]:  
```

### Using Type Decimal for Monetary Amounts

* Floating-point number fine for most applications
    * Stored in binary format, so they're only approximate

https://docs.python.org/3/library/index.html

```
In [1]: amount = 112.31

In [2]: print(amount)
112.31

In [3]: print(f'{amount:.20f}')
112.31000000000000227374

In [4]: from decimal import Decimal

In [5]: principal = Decimal('1000.00')

In [6]: principal
Out[6]: Decimal('1000.00')

In [7]: rate = Decimal('0.05')

In [8]: rate
Out[8]: Decimal('0.05')

In [9]: x = Decimal('10.5')

In [10]: y = Decimal('10.2')

In [11]: y = Decimal('2')

In [12]: x + y
Out[12]: Decimal('12.5')

In [13]: x // y
Out[13]: Decimal('5')

In [14]: x += y

In [15]: x
Out[15]: Decimal('12.5')

In [16]:  
```

## Compound Interest Problem

A person invests $1000.00 in a savings account yielding 5% interest. Assuming that the person leaves
all interest on deposit in  the account, calculate and display the amount of money in the account
at the end of each year for 10 years. Use the following formula for determining these amounts:

a = p(1 + r)n 

where
* p is the original amount invested (i.e., the principal), r is the annual interest
* n is the number of years
* a is the amount on deposit at the end of the nth year. 

```
In [16]: for year in range(1, 11):
    ...:     amount = principal * (1 + rate) ** year
    ...:     print(f'{year:>2}{amount:>10.2f}')
    ...:
 1   1050.00
 2   1102.50
 3   1157.62
 4   1215.51
 5   1276.28
 6   1340.10
 7   1407.10
 8   1477.46
 9   1551.33
10   1628.89
```

### Description

# Years 1 - 10 on the left
# :>2 == year > and 2 == greater than and 2 | year format is 2 decimal left and right aligned
# amount is floating point | in a field of 10 characters with .2 decimal places
    1..1050.00 = 10 character spaces

### Self Check

Assume that the tax on a restuarant bill is 6.25% and that the bill amount is $37.25. Use type Decimal to calculate
the bill total then print the result with two digits to the right of the decimal point.

```
In [1]: from decimal import Decimal

In [2]: print(f"{Decimal('37.25') * Decimal('1.0625'):.2f}")
39.58

In [3]:  
```

## Break and Continue Statements

```
In [1]: for number in range(100):
   ...:     if number == 10:
   ...:         break
   ...:     print(number, end=' ')
   ...:
0 1 2 3 4 5 6 7 8 9
In [2]: for number in range(10):
   ...:     if number == 5:
   ...:         continue
   ...:     print(number, end=' ')
   ...:
0 1 2 3 4 6 7 8 9
In [3]:
```

## Boolean Operators and, or and not

```
In [1]: gender = 'Female'

In [2]: age = 70

In [3]: if gender == 'Female' and age >= 65:
   ...:     print('Senior Female')
   ...:
Senior Female

In [4]: semester_average = 83

In [5]: final_exam = 95

In [6]: if semester_average  >= 90 or final_exam >= 90:
   ...:     print('Student gets an A')
   ...:
Student gets an A

In [7]: grade = 87

In [8]: if not grade == -1:
   ...:     print('The next grade is', grade)
   ...:
The next grade is 87

In [9]: 
```

### Self Check

> Assume that i = 1, j = 2, k = 3, and m = 2. What does each of the following conditions display?
> (i >=1) and (j < 4)
> (m <= 99) and (k < m)
> (j >= i) or (k == m)
> (k + m < j) or (3 - j >= k)
> not (k > m)


```
In [1]: i = 1

In [2]: j = 2

In [3]: k = 3

In [4]: m = 2

In [5]: (i >= 1) and (j < 4)
Out[5]: True

In [6]: (m <= 99) and (k < m)
Out[6]: False

In [7]: (j >= i) or (k == m)
Out[7]: True

In [8]: (k + m < j) or (3 - j >- k)
Out[8]: True

In [9]: not (k > m)
Out[9]: False

In [10]:  
```

## Intro to Data Science: Measures of Central Tendency - Mean, Median and Mode

* Measure of central tendency
    * single value that represents a "central" value in a set of values
    * A value which is in some sense typical of the others
* Mean
    * The average value in a set of values
* Median
    * The middle value when all the values are arranged in sorted order
* Mode
    * Most frequently occurring value

```
In [1]: grades = [85, 93, 45, 89, 85]

In [2]: sum(grades) / len(grades)
Out[2]: 79.4

In [3]: import statistics

In [4]: statistics.mean(grades)
Out[4]: 79.4

In [5]: statistics.median(grades)
Out[5]: 85

In [6]: statistics.mode(grades)
Out[6]: 85

In [7]: sorted(grades)
Out[7]: [45, 85, 85, 89, 93]

In [8]: grades
Out[8]: [85, 93, 45, 89, 85]

In [9]: 
```

### Self Check

> for the values 47, 95, 88, 73, 88 and 84, use the statistics module to calculate the mean, median and mode.

```
In [1]: import statistics

In [2]: values = [47, 95, 88, 73, 88, 84]

In [3]: statistics.mean(values)
Out[3]: 79.16666666666667

In [4]: statistics.median(values)
Out[4]: 86.0

In [5]: statistics.mode(values)
Out[5]: 88

In [6]: sorted(values)
Out[6]: [47, 73, 84, 88, 88, 95]

In [7]: values
Out[7]: [47, 95, 88, 73, 88, 84]

In [8]:  
```






