# Introduction and flat files

## Exploring your working directory

IPython has a bunch of cool commands, including its [magic commands](http://ipython.readthedocs.io/en/stable/overview.html). For example, starting a line with `!` gives you complete system shell access. This means that the IPython magic command `! ls` will display the contents of your current directory.

In order to import data into Python, you should first have an idea of what files are in your working directory.

## Importing entire text files

In order to read a text file in python, you could use `open()` function passing it the filename and the mode of which you want to open the file, to read a file you should pass `'r'` parameter after the filename, and assigning the result to a variable then using `read()` method of that variable to read from the file. After you finish modifying the file you should close it using `close()` method of the file variable.

You can check if the file is opened or closed using `closed` attribute of the file variable.

## Importing text files line by line

For large files, we may not want to print all of their content to the shell: you may wish to print only the first few lines. Enter the `readline()` method, which allows you to do this. When a file called `file` is open, you can print out the first line by executing `file.readline()`. If you execute the same command again, the second line will print, and so on.

You can bind a variable `file` by using a context manager construct:

```python
with open('file.txt') as file:
```

While still within this construct, the variable `file` will be bound to `open('file.txt')`; thus, to print the file to the shell, all the code you need to execute is:

```python
with open('file.txt') as file:
    print(file.readline())
```

## Why we like flat files and the Zen of Python

In Python Land, there are currently hundreds of *Python Enhancement Proposals*, commonly referred to as *PEP*s. [PEP8](https://www.python.org/dev/peps/pep-0008/), for example, is a standard style guide for Python, written by our sensei Guido van Rossum himself. It is the basis for how we here at DataCamp ask our instructors to style their code. Another one of my favorites is [PEP20](https://www.python.org/dev/peps/pep-0020/), commonly called the *Zen of Python*. Its abstract is as follows:

> Long time Pythoneer Tim Peters succinctly channels the BDFL's guiding principles for Python's design into 20 aphorisms, only 19 of which have been written down.

If you don't know what the acronym `BDFL` stands for, I suggest that you look [here](https://docs.python.org/3.3/glossary.html#term-bdfl). You can print the Zen of Python in your shell by typing `import this` into it! You're going to do this now and the 5th aphorism (line) will say something of particular interest.

## Importing flat files using NumPy

- `numpy.loadtxt(fname, dtype=<class 'float'>, delimiter=None, skiprows=0, usecols=None)`
- `numpy.genfromtxt(fname, dtype=<class 'float'>, *delimiter=None*, *skip_header=0*, *skip_footer=0*, *converters=None*, *missing_values=None*, *filling_values=None*, *usecols=None*, *names=None*, *excludelist=None*)`
- `numpy.recfromcsv()`

If we have a dataset containing different datatypes in different columns, the function `np.loadtxt()` will freak at this. Instead, the function `genfromtxt()`, which can handle such structures. If we pass `dtype=None` to it, it will figure out what types each column should be.

The argument `names` of `genfromtxt()` function tells us there is a header. Because the data are of different types, `data` is an object called a [structured array](http://docs.scipy.org/doc/numpy/user/basics.rec.html). Because numpy arrays have to contain elements that are all the same type, the structured array solves this by being a 1D array, where each element of the array is a row of the flat file imported.

`recfromcsv()` function behaves similarly to `genfromtxt()`, except that its default `dtype` is `None`, andwith a default `delimiter=","`.

## Importing flat files using pandas

The `DataFrame` object in pandas is a more appropriate structure in which to store such data and, thankfully, we can easily import files of mixed data types as DataFrames using the pandas functions `read_csv()` and `read_table()`.

- `pandas.read_csv(filepath_or_buffer, sep=NoDefault.no_default, delimiter=None, header='infer', names=NoDefault.no_default, index_col=None, usecols=None, dtype=None, skiprows=None, skipfooter=0, nrows=None, na_values=None, parse_dates=False, date_parser=None, comment=None)`

Use the `nrows` argument to specify the number of rows you want to read! Set the `header` argument to `false` if your data has no header. `na_values` argument takes a list of strings to recognize as `NA`/`NaN`.  `comment` takes characters that comments occur after in the file.

***

# Importing data from other file types

## Introduction to other file types

Python [library `os`](https://docs.python.org/2/library/os.html) consists of miscellaneous operating system interfaces, which can be used for example to print the current working directory.

```py
# Import the library
import os
# Store the current directory in wd variable        
wd = os.getcwd()
# Output the contents of the directory to the shell 
os.listdir(wd)
```

## Loading a pickled file

There are a number of datatypes that cannot be saved easily to flat files, such as lists and dictionaries. If you want your files to be human readable, you may want to save them as text files in a clever manner. JSONs, which you will see in a later chapter, are appropriate for Python dictionaries.

However, if you merely want to be able to import them into Python, you can [serialize](https://en.wikipedia.org/wiki/Serialization) them. All this means is converting the object into a sequence of bytes, or a bytestream.

To read a binary file with `open()` you need to specify the second argument:

```py
# Import pickle package
import pickle

# Open pickle file and load data: d
with open('data.pkl', 'rb') as file:
    d = pickle.load(file)
```

## Listing sheets in Excel files

We can use `pandas` to import Excel spreadsheets and how to list the names of the sheets in any loaded .xlsx file. An Excel file imported into a variable `spreadsheet`, you can retrieve a list of the sheet names using the attribute `spreadsheet.sheet_names`

```py
# Import pandas
import pandas as pd
# Assign spreadsheet filename: file
file = 'spreadsheet.xlsx'
# Load spreadsheet: xls
xls = pd.ExcelFile(file)
# Print sheet names
print(xls.sheet_names)
```

## Importing sheets from Excel files

Suppose that the spreadsheet `'battledeath.xlsx'`  Excel file contains two sheets, `'2002'` and `'2004'`. To import these sheets, there are two ways; by name and by index:

```py
# Load a sheet into a DataFrame by name: df1
df1 = xls.parse('2004')
# Load a sheet into a DataFrame by index: df2
df2 = xls.parse(0)
```
