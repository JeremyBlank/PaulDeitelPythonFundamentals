# USING IPYTHON INTERACTIVE MODE AS A CALCULATOR

> Executes snippets
> See immediate results
> REPL - Read, Evaluate, Print, Loop
> Exiting IPython interactive mode

## OPENING CMD
> MacOS - Open a Terminal from the Applications folder's Utilities subfolder

> Windows - open the Anaconda Prompt from the start menu
>    * When doing this to update Anadonca or to install new packages, execute the Anaconda Prompt as an administrator by right-clicking, then selecting More - Run As Administrator

> Linux - opne your system's Terminal or shell


## ACTIVATING IPYTHON

```
Anaconda 
(base) C:\Windows\system32>ipython
Python 3.7.7 (default, May  6 2020, 11:45:54) [MSC v.1916 64 bit (AMD64)]
Type 'copyright', 'credits' or 'license' for more information
IPython 7.15.0 -- An enhanced Interactive Python. Type '?' for help.

In [1]:
```

```
(base) C:\Windows\system32>ipython
Python 3.7.7 (default, May  6 2020, 11:45:54) [MSC v.1916 64 bit (AMD64)]
Type 'copyright', 'credits' or 'license' for more information
IPython 7.15.0 -- An enhanced Interactive Python. Type '?' for help.

In [1]: 45 + 72
Out[1]: 117

In [2]: 5 * (12.7 - 4) / 2
Out[2]: 21.75

In [3]: 5 * 12.7 - 4 / 2
Out[3]: 61.5

In [4]: 5 * (3 + 4)
Out[4]: 35

In [5]: 5 * 3 + 4
Out[5]: 19

In [6]:

```

#### HOW TO EXIT IPYTHON

```
(base) C:\Windows\system32>ipython
Python 3.7.7 (default, May  6 2020, 11:45:54) [MSC v.1916 64 bit (AMD64)]
Type 'copyright', 'credits' or 'license' for more information
IPython 7.15.0 -- An enhanced Interactive Python. Type '?' for help.

In [1]: exit
```

or ctrl+d

```
(base) C:\Windows\system32>ipython
Python 3.7.7 (default, May  6 2020, 11:45:54) [MSC v.1916 64 bit (AMD64)]
Type 'copyright', 'credits' or 'license' for more information
IPython 7.15.0 -- An enhanced Interactive Python. Type '?' for help.

In [1]:
Do you really want to exit ([y]/n)? y
```

## EXECUTING A PYTHON PROGRAM USING IPYTHON INTERPRETER

* Changing the examples folder's ch01 subfolder
    * macOS/Linux
        * cd ~/Documents/examples/ch01
    * Windows
        * cd C: \Users\youraccount\Documents\examples\ch01
* Executing the script


```
(base) C:\Windows\system32>cd C:\Users\jrbla\PycharmProjects\PythonFundamentalsDeitel\examples\ch01

(base) C:\Users\jrbla\PycharmProjects\PythonFundamentalsDeitel\examples\ch01>dir
 Volume in drive C is Windows
 Volume Serial Number is 301B-803F

 Directory of C:\Users\jrbla\PycharmProjects\PythonFundamentalsDeitel\examples\ch01

06/25/2020  01:30 PM    <DIR>          .
06/25/2020  01:30 PM    <DIR>          ..
06/25/2020  01:30 PM    <DIR>          .ipynb_checkpoints
06/25/2020  01:30 PM             2,937 RollDieDynamic.py
06/25/2020  01:30 PM    <DIR>          snippets_ipynb
06/25/2020  01:30 PM             1,302 TestDrive.ipynb
               2 File(s)          4,239 bytes
               4 Dir(s)  368,638,001,152 bytes free

(base) C:\Users\jrbla\PycharmProjects\PythonFundamentalsDeitel\examples\ch01>ipython RollDieDynamic.py 6000 1
(base) C:\Users\jrbla\PycharmProjects\PythonFundamentalsDeitel\examples\ch01>ipython RollDieDynamic.py 6000 100
(base) C:\Users\jrbla\PycharmProjects\PythonFundamentalsDeitel\examples\ch01>ipython RollDieDynamic.py 6000 1000
```
#### Note:
- ls for macOS / Linux
- dir for Windows
- 6000 1 means you will roll 1 dice 6000 times
- 6000 100 means you will roll 100 dice 6000 times
- 6000 100 means you will roll 1000 dice 6000 times

## WRITING AND EXECUTING CODE IN A JUPYTER NOTEBOOK

