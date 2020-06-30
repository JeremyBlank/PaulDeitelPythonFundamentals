## Variables and Assignment Statements

```
Anaconda

(base) C:\Users\jrbla>cd C:\Users\jrbla\PycharmProjects\PaulDeitelPythonFundamentals\examples\ch02

(base) C:\Users\jrbla\PycharmProjects\PaulDeitelPythonFundamentals\examples\ch02>dir
 Volume in drive C is Windows
 Volume Serial Number is 301B-803F

 Directory of C:\Users\jrbla\PycharmProjects\PaulDeitelPythonFundamentals\examples\ch02

06/25/2020  06:57 PM    <DIR>          .
06/25/2020  06:57 PM    <DIR>          ..
06/25/2020  01:30 PM             1,845 fig02_01.py
06/25/2020  01:30 PM             1,433 fig02_02.py
06/25/2020  06:57 PM    <DIR>          snippets_ipynb
06/25/2020  06:57 PM    <DIR>          snippets_py
               2 File(s)          3,278 bytes
               4 Dir(s)  367,785,758,720 bytes free

(base) C:\Users\jrbla\PycharmProjects\PaulDeitelPythonFundamentals\examples\ch02>
```

```
(base) C:\Users\jrbla\PycharmProjects\PaulDeitelPythonFundamentals\examples\ch02>ipython
Python 3.7.7 (default, May  6 2020, 11:45:54) [MSC v.1916 64 bit (AMD64)]
Type 'copyright', 'credits' or 'license' for more information
IPython 7.15.0 -- An enhanced Interactive Python. Type '?' for help.

In [1]: 45 + 72
Out[1]: 117

In [2]:                                                                                                                 
```
### DOCUMENTATION

https://www.python.org/
https://docs.python.org/3/library/index.html


## VARIABLES AND TYPES
```
Anaconda

In [2]: x = 7

In [3]: y = 3

In [4]: x + y
Out[4]: 10

In [5]: total = x + y

In [6]: total
Out[6]: 10

In [7]: type(x)
Out[7]: int

In [8]: type(12.7)
Out[8]: float

In [9]:       
```

### Arithmetic Operators
```
Python              Arithmetic      Algebraic       Python
Operator            Operator        Expression      Expression

Addition            +               f + 7           f + 7
Subtraction         -               p - c           p - c
Multiplication      *               b * m           b * m
Exponentiation      **              x^y             x ** y
True Division       /               x/y             x / y
Floor Division      //              |_x/y_|         x // y
Reminder (modulo)   %               r mod s         r % s
```

```
Anaconda
In [1]: 7 * 4
Out[1]: 28

In [2]: 2 ** 10
Out[2]: 1024

In [3]: 9 ** (1 / 2)
Out[3]: 3.0

In [4]: 9 ** (0.5)
Out[4]: 3.0

In [5]: 7 / 4
Out[5]: 1.75

In [6]: 7 // 4
Out[6]: 1

In [7]: 3 // 5
Out[7]: 0

In [8]: 14 // 7
Out[8]: 2

In [9]: -13 / 4
Out[9]: -3.25

In [10]: -13 // 4
Out[10]: -4

In [11]: 123 / 0
---------------------------------------------------------------------------
ZeroDivisionError                         Traceback (most recent call last)
<ipython-input-11-76e1a9ab9410> in <module>
----> 1 123 / 0

ZeroDivisionError: division by zero

In [12]: z + 7
---------------------------------------------------------------------------
NameError                                 Traceback (most recent call last)
<ipython-input-12-2ca5f6c7aca2> in <module>
----> 1 z + 7

NameError: name 'z' is not defined

In [13]: 17 % 5
Out[13]: 2

In [14]: 10 * (5 + 3)
Out[14]: 80

In [15]: 10 * 5 + 3
Out[15]: 53

In [16]:  
```

#### Self Check

Jupyter lab

## Function print and an intro to Single- and Double-Quoted Strings

```

Anaconda

(base) C:\Windows\system32>ipython
Python 3.7.7 (default, May  6 2020, 11:45:54) [MSC v.1916 64 bit (AMD64)]
Type 'copyright', 'credits' or 'license' for more information
IPython 7.15.0 -- An enhanced Interactive Python. Type '?' for help.

In [1]: print('Hello World!')
Hello World!

In [2]: print("Hello World!")
Hello World!

In [3]: print('Welcome', 'to', 'Python!')
Welcome to Python!

In [4]: print('Welcome\nto\nPython!')
Welcome
to
Python!

In [5]: print('this is a longer string, so we \
   ...: split it over two lines')
this is a longer string, so we split it over two lines

In [6]: print('Sum is', 7 + 3)
Sum is 10
```

### Escape Sequences

Escape Sequence             Description
\n                          Insert a newline character in a string. When the string is displayed,
                            for a newline, move the screen cursor to the beginning of the next line.
/t                          Insert a tab. When the string is displayed , for each tab, move the screen cursor
                            to the next tab stop.
\\                          Insert a backslash character in a string.
\"                          Insert a double quote character in a string.
\'                          Insert a single quote character in a string.

### Self Check

Jupyter Lab


## Triple-Quoted Strings

* ''' or """
* Multiline Strings
* Strings containing single or double quotes
* Docstrings

```

Anaconda

(base) C:\Windows\system32>ipython
Python 3.7.7 (default, May  6 2020, 11:45:54) [MSC v.1916 64 bit (AMD64)]
Type 'copyright', 'credits' or 'license' for more information
IPython 7.15.0 -- An enhanced Interactive Python. Type '?' for help.

In [1]: print('Display "hi" in quotes')
Display "hi" in quotes

In [2]: print('Display 'hi' in quotes')
  File "<ipython-input-2-19bf596ccf72>", line 1
    print('Display 'hi' in quotes')
                     ^
SyntaxError: invalid syntax


In [3]: print('Display \'hi\' in quotes')
Display 'hi' in quotes

In [4]: print("Display the name O'Brien")
Display the name O'Brien

In [5]: print('Display \"hi\" in quotes')
Display "hi" in quotes

In [6]: print("""Display "hi" and 'bye' in quotes""")
Display "hi" and 'bye' in quotes

In [7]: triple_quoted_string = """This is a triple-quoted
   ...: string that spans two lines"""

In [8]: print(triple_quoted_string)
This is a triple-quoted
string that spans two lines

In [9]: triple_quoted_string
Out[9]: 'This is a triple-quoted\nstring that spans two lines'

In [10]:
```

### Self Check

Jupyter Labs


## Getting input from the user

```

Anaconda

Type 'copyright', 'credits' or 'license' for more information
IPython 7.15.0 -- An enhanced Interactive Python. Type '?' for help.

In [1]: name = input("What's your name? ")
What's your name? Jeremy

In [2]: name
Out[2]: 'Jeremy'

In [3]: print(name)
Jeremy

In [4]: name = input("What's your name? ")
What's your name? 'Jeremy'

In [5]: name
Out[5]: "'Jeremy'"

In [6]: print(name)
'Jeremy'

In [7]: value1 = input('Enter first number: ')
Enter first number: 7

In [8]: value2 = input('Enter second number: ')
Enter second number: 3

In [9]: value1 + value2
Out[9]: '73'

In [10]: value = input('Enter an integer: ')
Enter an integer: 7

In [11]: value = int(value)

In [12]: value
Out[12]: 7

In [13]: another_value = int(input('Enter another integer: '))
Enter another integer: 13

In [14]: another_value
Out[14]: 13

In [15]: value + another_value
Out[15]: 20

In [16]: bad_value = int(input('Enter another integer: '))
Enter another integer: hello
---------------------------------------------------------------------------
ValueError                                Traceback (most recent call last)
<ipython-input-16-cd36e6cf8911> in <module>
----> 1 bad_value = int(input('Enter another integer: '))

ValueError: invalid literal for int() with base 10: 'hello'

In [17]: int(10.5)
Out[17]: 10

In [18]:
```

### Self check

Jupyter Lab


## Decision Making: The if Statement and Comparison Operators

## Comparison Operators

Python      Sample
Operator    Condition       Meaning

'>'         x > y           x is greater than y
'<'         x < y           x is less than y
'>='        x >= y          x is greater than or equal to y
'<='        x <= y          x is less than or equal to y
----------------------------------------------------------
==          x == y          x is equal to y
!=          x != y          x is not equal to y

```

Anaconda

Python 3.7.7 (default, May  6 2020, 11:45:54) [MSC v.1916 64 bit (AMD64)]
Type 'copyright', 'credits' or 'license' for more information
IPython 7.15.0 -- An enhanced Interactive Python. Type '?' for help.

In [1]: 7 > 4
Out[1]: True

In [2]: 7 < 4
Out[2]: False

In [3]:    
```

```

Anaconda

(base) C:\Windows\system32>cd C:\Users\jrbla\PycharmProjects\PaulDeitelPythonFundamentals\examples\ch02

(base) C:\Users\jrbla\PycharmProjects\PaulDeitelPythonFundamentals\examples\ch02>dir
 Volume in drive C is Windows
 Volume Serial Number is 301B-803F

 Directory of C:\Users\jrbla\PycharmProjects\PaulDeitelPythonFundamentals\examples\ch02

06/25/2020  06:57 PM    <DIR>          .
06/25/2020  06:57 PM    <DIR>          ..
06/25/2020  01:30 PM             1,845 fig02_01.py
06/25/2020  01:30 PM             1,433 fig02_02.py
06/29/2020  09:29 AM    <DIR>          snippets_ipynb
06/25/2020  06:57 PM    <DIR>          snippets_py
               2 File(s)          3,278 bytes
               4 Dir(s)  367,157,735,424 bytes free

(base) C:\Users\jrbla\PycharmProjects\PaulDeitelPythonFundamentals\examples\ch02>ipython fig02_01.py
Enter two integers and I will tell you the relationships they satisfy.
Enter first integer: 37
Enter second integer: 42
37 is not equal to 42
37 is less than 42
37 is less than or equal to 42

(base) C:\Users\jrbla\PycharmProjects\PaulDeitelPythonFundamentals\examples\ch02>ipython fig02_01.py
Enter two integers and I will tell you the relationships they satisfy.
Enter first integer: 7
Enter second integer: 7
7 is equal to 7
7 is less than or equal to 7
7 is greater than or equal to 7

(base) C:\Users\jrbla\PycharmProjects\PaulDeitelPythonFundamentals\examples\ch02>ipython fig02_01.py
Enter two integers and I will tell you the relationships they satisfy.
Enter first integer: 54
Enter second integer: 17
54 is not equal to 17
54 is greater than 17
54 is greater than or equal to 17

(base) C:\Users\jrbla\PycharmProjects\PaulDeitelPythonFundamentals\examples\ch02>
```
                                                                                                                  
```
Text editor code of program above

# fig02_01.py
"""Compare integers using if statements and comparison operators."""

print('Enter two integers and I will tell you',
      'the relationships they satisfy.')

# read first integer
number1 = int(input('Enter first integer: '))

# read second integer
number2 = int(input('Enter second integer: '))

if number1 == number2:
    print(number1, 'is equal to', number2)

if number1 != number2:
    print(number1, 'is not equal to', number2)

if number1 < number2:
    print(number1, 'is less than', number2)

if number1 > number2:
    print(number1, 'is greater than', number2)

if number1 <= number2:
    print(number1, 's less than or equal to', number2) 

if number1 >= number2:
    print(number1, 'is greater than or equal to', number2)
```

```

Anaconda

(base) C:\Users\jrbla\PycharmProjects\PaulDeitelPythonFundamentals\examples\ch02>ipython
Python 3.7.7 (default, May  6 2020, 11:45:54) [MSC v.1916 64 bit (AMD64)]
Type 'copyright', 'credits' or 'license' for more information
IPython 7.15.0 -- An enhanced Interactive Python. Type '?' for help.

In [1]: x = 3

In [2]: 1 <=  x <= 5
Out[2]: True

In [3]: x = 10

In [4]: 1 <=  x <= 5
Out[4]: False

In [5]:   
```

https://docs.python.org/3/reference/expressions.html#operator-precedence

### self check

Jupyter Lab


## Objects and Dynamic Typing

```

Anaconda

(base) C:\Users\jrbla\PycharmProjects\PaulDeitelPythonFundamentals\examples\ch02>ipython
Python 3.7.7 (default, May  6 2020, 11:45:54) [MSC v.1916 64 bit (AMD64)]
Type 'copyright', 'credits' or 'license' for more information
IPython 7.15.0 -- An enhanced Interactive Python. Type '?' for help.

In [1]: type(7)
Out[1]: int

In [2]: type(4.1)
Out[2]: float

In [3]: type('dog')
Out[3]: str

In [4]: x = 7

In [5]: x + 10
Out[5]: 17

In [6]: x
Out[6]: 7

In [7]: x = x + 10

In [8]: x
Out[8]: 17

In [9]: type(x)
Out[9]: int

In [10]: x = 4.1

In [11]: type(x)
Out[11]: float

In [12]: x = 'dog'

In [13]: type(x)
Out[13]: str

In [14]: 
```

### self check

Jupyter Lab


## Intro to Data Science: Basic Descriptive Statistics

* Data scientists often use statistics to describe and summarize data
* Descriptive statistics:
    * minimum - the smallest value in a collection of values
    * maximum - the largest value in a collection of values
    * range - the range of values from the minimum to the maximum
    * count - the number of values in a collection
    * sum - the total of the values in a collection
* Measures of dispersion (also called measures of variability), such as
    range, help determine how spreat out values are

* Other measures of dispersion
    * Variance and standard deviation

```

Anaconda

(base) C:\Users\jrbla\PycharmProjects\PaulDeitelPythonFundamentals\examples\ch02>ipython fig02_02.py
Enter first integer: 12
Enter second integer: 27
Enter third integer: 36
Minimum value is 12

(base) C:\Users\jrbla\PycharmProjects\PaulDeitelPythonFundamentals\examples\ch02>


(base) C:\Users\jrbla\PycharmProjects\PaulDeitelPythonFundamentals\examples\ch02>ipython fig02_02.py
Enter first integer: 27
Enter second integer: 12
Enter third integer: 36
Minimum value is 12

(base) C:\Users\jrbla\PycharmProjects\PaulDeitelPythonFundamentals\examples\ch02>


(base) C:\Users\jrbla\PycharmProjects\PaulDeitelPythonFundamentals\examples\ch02>ipython fig02_02.py
Enter first integer: 36
Enter second integer: 27
Enter third integer: 12
Minimum value is 12
```

```

fig02_02.py
"""Find the minimum of thee values."""

number1 = int(input('Enter first integer: '))
number2 = int(input('Enter second integer: '))
number3 = int(input('Enter third integer: '))

minimum = number1

if number2 < minimum:
    minimum = number2

if number3 < minimum:
    minimum = number3
print('Minimum value is', minimum)
```


```
Anaconda

(base) C:\Users\jrbla\PycharmProjects\PaulDeitelPythonFundamentals\examples\ch02>ipython
Python 3.7.7 (default, May  6 2020, 11:45:54) [MSC v.1916 64 bit (AMD64)]
Type 'copyright', 'credits' or 'license' for more information
IPython 7.15.0 -- An enhanced Interactive Python. Type '?' for help.

In [1]: min(36, 27, 12)
Out[1]: 12

In [2]: max(36, 27, 12)
Out[2]: 36

In [3]: range(36, 27, 12)

In [4]: print('Range:', min(36, 27, 12), '-', max(36, 27, 12))
```

## Functional Style Programming

* Functional-sytle can help you create more concise, clear and easier to dubug code
* Min and max are predefined reductions
    * Reduce a collection of values to a single value
* Other common reductions
    * Sum, average, standard deviation and variance





