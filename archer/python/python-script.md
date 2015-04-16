# Python

## Getting the base tutorial content

Need to ensure everyone has the python student content:

    git clone https://github.com/hpcarcher/2015-04-16-imperial-students
    

## Ways of working

### Running ipython notebooks

Need to:

```
      cd 2015-04-16-imperial-students/
      ipython notebook
```    
   and navigate to the appropriate notebook from there.
   
Possible issues:

* May have to make the notebooks trusted:

      ipython trust *.ipynb
      
* There may still be firewall issues to be sorted.
   
### Running ipython shell

Run:

    ipython

Nice things:

* Colour syntax
* Built-in python support

### Run directly through the interpreter

For the purists type `python` at the command line and then type content as you go along:

    $ python
    Python 2.7.9 (default, Feb 10 2015, 03:28:08) 
    [GCC 4.2.1 Compatible Apple LLVM 6.0 (clang-600.0.56)] on darwin
    Type "help", "copyright", "credits" or "license" for more information.
    >>> 

### Conclusion

Use:

* **I** will use the `ipython` shell
* Use the `ipython notebooks` if you are falling behind
* Use the `python` interpreter directly ... you are beyond help

Will later start using files.

# Lesson 1: some basics and numpy

## Basics

Shall use `ipython`:

    cd 2015-12-03-edinburgh-students/archer/python
    ls data
    ipython
    mbp-ma:python mario$ ipython 
    Python 2.7.6 (default, Sep  9 2014, 15:04:36) 
    Type "copyright", "credits" or "license" for more information.

     IPython 2.3.1 -- An enhanced Interactive Python.
     ?         -> Introduction and overview of IPython's features.
     %quickref -> Quick reference.
     help      -> Python's own help system.
     object?   -> Details about 'object', use 'object??' for extra details.

    
Have CSV files. Inflammation of patients - one row per patient, 60 patients one measurement per day. We want to:
 
* load data into the computer's memory,
* calculate the average daily inflammation across all patients, and
* plot the result.

Lesson objectives:

* Explain what a library is, and what libraries are used for.
* Load a Python library and use the things it contains.
* Read tabular data from a file into a program.
* Assign values to variables.
* Select individual values and subsections from data.
* Perform operations on arrays of data.
* Display simple graphs.

Start off by loading the python numerical libraries:

    import numpy                # Import the library
    numpy.<TAB><TAB>            # Find out what functions are available             
    help(numpy.empty)           # Two ways of getting help
    numpy.empty?

Let's load the data:

    numpy.loadtxt(fname="data/inflammation-01.csv",delimiter=",")
    array([[ 0.,  0.,  1., ...,  3.,  0.,  0.],
           [ 0.,  1.,  2., ...,  1.,  0.,  1.],
           [ 0.,  1.,  1., ...,  2.,  1.,  1.],
           ..., 
           [ 0.,  1.,  1., ...,  1.,  1.,  1.],
           [ 0.,  0.,  0., ...,  0.,  2.,  0.],
           [ 0.,  0.,  1., ...,  1.,  1.,  0.]])

Notes:

 * Use of ... to save space
 * Only show as much detail of the number as required
 
Need to assign it to a variable though. The way assignments are done in python:

    weight_kg=1.2
    print weight_kg
    print "My weight in kgs is ", weight_kg
    print "My weight in pounds is ", weight_kg*2.2
    
Can change the value:

    weight_kg=5.5
    print "My weight is ",weight_kg
    
Can think of a variable as a box where you store the value. Can now do:

    weight_lbs=weight_kg*2.2
    print "Weight in kg ",weight_kg," and weight in lbs ",weight_ls
    
What happens now if we change the weight:

    weight_kg=10.0
    
Can now assign:

    data = numpy.loadtxt(fname="data/inflammation-01.csv",delimiter=",")
    print data
    
Now we can start asking stuff off the data:

    print type(data)
    <type 'numpy.ndarray'>

A multidimensional array of homogeneous of fixed type items (can check this out by doing `help(numpy.ndarray)`).
    
We can also enquire the *shape* if the array:

     data.shape
     (60, 40)

60 rows by 60 columns.

Can dereference the data:

     print data[0,0]
     0.0
     print data[30,20]
     13.0

Start indexing from 0 like C, C++, Java and not like Fortran or MATLAB. The origin lies on the top left of the array. Can print a ***slice*** of data:

     print data[0:4,0:10]

prints from **start:end** from **start** to **end-1**. Or can do:

     print data[5:10,0:10]

Can miss the start (defaults to 0) or the end (defaults to the last index):
 
     small = data[:3,36:]
     print small

Can manipulate data:

     doubledata = 2.0 * data
     print data[:3,36:]
     print doubledata[:3,36:]
     
Also

     tripledata = doubledata+data
     print tripledata[:3, 36:]

Can perform more complex operations (methods):

     print data.mean()           # Average of all values

and there are others:

     print 'maximum inflammation:', data.max()
     print 'minimum inflammation:', data.min()
     print 'standard deviation:', data.std()

Getting partial results:

     patient_0 = data[2, :] # 2 on the first axis, everything on the second
     print 'maximum inflammation for patient 2:', patient_0.max()

Can be shortened to:

     print 'maximum inflammation for patient 2:', data[2, :].max()
     maximum inflammation for patient 2: 19.0

or

     print 'maximum inflammation for patient 2:', data[2].max()
     maximum inflammation for patient 2: 19.0
     
Similarly:

     print 'average inflammation for patient 2:', data[2].mean()
     average inflammation for patient 2: 6.1
     
Can use:

     print data.mean(axis=0)     # Average over the columns
     print data.mean(axis=1)     # Average over the rows 

Can do a quick check:

     print data.shape
     (60,40)
     print data.mean(axis=0).shape
     (40,)                             # Vector or size 40 (columns)
     print data.mean(axis=1).shape
     (60,)                             # Vector of size 60 (rows)

Can do more with slices:

     element = 'oxygen'
     print 'first three characters:', element[0:3]
     print 'last three characters:', element[3:6]
 
 What do element[-1], element[-2] and element[1:-1] give you?
 
     print element[-1]
     n
     print element[-2]
     e
     print element[1:-1]
     xyge
 
 What about:
 
     print element[3:3]
     ''

Why is that an empty string? Similarly:

    print data[3:3,4:4]
    []
    print data[3:3,:]
    []


## Plotting

Going to be using the `matplotlib`, a 2-d plotting library similar to what is available in MATLAB, for visualising the data:

    from matplotlib import pyplot
    pyplot.imshow(data)
    pyplot.show()

Blue regions in this heat map are low values, while red shows high values. As we can see, inflammation rises and falls over a 40-day period. 

Let's take a look at the average inflammation over time:

    print 'average inflammation across all patients as a function of time'
    ave_inflammation = data.mean(axis=0)
    pyplot.plot(ave_inflammation)
    pyplot.show()

Here, we have put the average inflammation value across all patients in the variable `ave_inflammation`, then asked `pyplot` to create and display a line graph of those values. 

The result is roughly a linear rise and fall, which is suspicious: based on other studies, we expect a sharper rise and slower fall. 

Let's have a look at two other statistics:

    print 'maximum inflammation value across all patients as a function of time'
    pyplot.plot(data.max(axis=0))
    pyplot.show()

and

    print 'minimum inflammation value across all patients as a function of time'
    pyplot.plot(data.min(axis=0))
    pyplot.show()

The maximum value rises and falls perfectly smoothly, while the minimum seems to be a step function. Neither result seems particularly likely, so either there's a mistake in our calculations or something is wrong with our data.

**EXERCISE**: Create a plot showing the standard deviation of the inflammation data for each day across all patients.

     pyplot.plot(data.std(axis=0))
     pyplot.show()
     
Can build more sophisticated graphs:

     import numpy as np                     # Can use aliases
     from matplotlib import pyplot as plt

     data = np.loadtxt(fname='data/inflammation-01.csv', delimiter=',')

     plt.figure(figsize=(10.0, 3.0))

     plt.suptitle("inflammation-01")

     plt.subplot(1, 3, 1)
     plt.ylabel('average')
     plt.plot(data.mean(0))

     plt.subplot(1, 3, 2)
     plt.ylabel('max')
     plt.plot(data.max(0))

     plt.subplot(1, 3, 3)
     plt.ylabel('min')
     plt.plot(data.min(0))

     plt.tight_layout()
     plt.show()

**Exercise**: Modify the program to display the three plots on top of one another instead of side by side.

To Sum up:

* Import a library into a program using `import libraryname`.
* Use the `numpy` library to work with arrays in Python.
* Use `variable = value` to assign a value to a variable in order to record it in memory.
* Variables are created on demand whenever a value is assigned to them.
* Use `print` something to display the value of something.
* The expression `array.shape` gives the shape of an array.
* Use `array[x, y]` to select a single element from an array.
* Array indices start at 0, not 1.
* Use `low:high` to specify a slice that includes the indices from `low` to `high-1`.
* All the indexing and slicing that works on arrays also works on strings.
* Use `# some kind of explanation` to add comments to programs.
* Use `array.mean()`, `array.max()` and `array.min()` to calculate simple statistics.
* Use `array.mean(axis=0)` or `array.mean(axis=1)` to calculate statistics along the specified axis.

# Lesson 2: functions

## Objectives

* Define a function that takes parameters.
* Return a value from a function.
* Test and debug a function.
* Explain what a call stack is, and trace changes to the call stack as functions are called.
* Set default values for function parameters.
* Explain why we should divide programs into small, single-purpose functions.

## Defining a function

    # def to define, name of function + arguments
    def fahr_to_kelvin(temp):
        return ((temp - 32) * (5/9)) + 273.15 # Body, 4 spaces

Run the function:

    print 'freezing point of water:', fahr_to_kelvin(32)
    freezing point of water: 273.15
    
    print 'boiling point of water:', fahr_to_kelvin(212)
    boiling point of water: 273.15

## Debugging the function

Use a debugger normally but code here is pretty short. Test it with a value `test=212`:

     print "212 - 32:", 212 - 32
     212 - 32: 180
     
     print "(212 - 32) * (5/9):", (212 - 32) * (5/9)
     (212 - 32) * (5/9): 0
     
 Aha!
 
     print 5/9
     0

In Python get integer division so:

    print 10/3
    3
 
 So need to do:
 
    print 10.0/3
    3.3333333333333335
    
    print float(10)/3
    3.33333333333

Can use this for variables as well:

    a = 10
    b = 3
    print 'a/b is:', a/b
    print 'float(a)/b is:', float(a)/b

Can now fix the function:

    def fahr_to_kelvin(temp):
         return ((temp - 32) * (5.0/9.0)) + 273.15

    print 'freezing point of water:', fahr_to_kelvin(32)
    print 'boiling point of water:', fahr_to_kelvin(212)

## Composing functions

Can now write new functions:

    def kelvin_to_celsius(temp):
        return temp - 273.15

    print 'absolute zero in Celsius:', kelvin_to_celsius(0.0)

and

    def fahr_to_celsius(temp):
        temp_k = fahr_to_kelvin(temp)
        result = kelvin_to_celsius(temp_k)
        return result

## Exercises

1. In python `'a'+'b'='ab'`. Write a function that:

        print fence("name","*")
        *name*
  
  Solution:   
     
        def fence(string1,string2):
            return string2+string1+string2
             
 2. Write a function that does:
 
        print outer("helium")
        hm
 
 Solution:
 
        def outer(s):
            return s[0]+s[-1]

3. Read the section on the *call stack* in the notes.

## Testing and documenting

Write a function to centre data around a particular value:

    def center(data, desired):
        return (data - data.mean()) + desired

Can test:

     z = numpy.zeros((2,2))   # Create a 2x2 arrays of zeros
     print center(z, 3)
     
Now centre our real data:

    data = numpy.loadtxt(fname='data/inflammation-01.csv', delimiter=',')
    print center(data, 0)

Look at some stats:

    print 'original min, mean, and max are:', data.min(), data.mean(), data.max()
    original min, mean, and max are: 0.0 6.14875 20.0
    
    centered = center(data, 0)
    
    print 'centered min, mean, and and max are:', centered.min(),  centered.mean(), centered.max()
    centered min, mean, and and max are: -6.14875 -3.49054118942e-15 13.85125

Check the standard deviation:

    print 'std dev before and after:', data.std(), centered.std()

Hard to tell the difference so use:

    print 'difference in standard deviations before and after:', data.std() - centered.std()
    difference in standard deviations before and after: -3.5527136788e-15

Can document the routine internally:

    # center(data, desired): return a new array containing the original data   
    # centered around the desired value.
    def center(data, desired):
        return (data - data.mean()) + desired

or as function documentation (doctoring):

    def center(data, desired):
    '''Return a new array containing the original data centered around the desired value.'''
    return (data - data.mean()) + desired

so now doing:

     help(center)
     Help on function center in module __main__:

     center(data, desired)
         Return a new array containing the original data centered around the desired value.
    
Can also add a **docstring** that spans multiple lines:

    def center(data, desired):
    '''Return a new array containing the original data centered around the  
       desired value.
       Example: center([1, 2, 3], 0) => [-1, 0, 1]'''
    return (data - data.mean()) + desired

## Exercises
1. Write a function called `analyze` that takes a filename as a parameter and  displays the three graphs produced in the previous lesson, i.e., `analyze('data/inflammation-01.csv')` should produce the graphs already shown, while `analyze('data/inflammation-02.csv')` should produce corresponding graphs for the second data set. Be sure to give your function a `docstring`.

Solution:

```
def analyze(file):
    """
    analyze(file): read in a CSV file and plot
                   the results using matplotlib.
    """
    import numpy as np
    from matplotlib import pyplot as plt
    data = np.loadtxt(fname=file,delimiter=",")
    plt.figure(figsize=(10.0, 3.0))
    plt.suptitle(file.split("/")[1].split(".")[0])
    plt.subplot(1, 3, 1)
    plt.ylabel('average')
    plt.plot(data.mean(0))
    plt.subplot(1, 3, 2)
    plt.ylabel('max')
    plt.plot(data.max(0))
    plt.subplot(1, 3, 3)
    plt.ylabel('min')
    plt.plot(data.min(0))
    plt.tight_layout()
    plt.show()
```  


2. Write a function `rescale` that takes an array as input and returns a corresponding array of values scaled to lie in the range 0.0 to 1.0. (If `L` and `H` are the lowest and highest values in the original array, then the replacement for a value `v` should be `(v−L)/(H−L)`.) Be sure to give the function a `docstring`.

Solution:

```
def rescale(a):
    """
    rescale(array): rescale an array between a max and min.
    """
    L = a.min()
    H = a.max()
    a = (a-L)/(H-L)
    return(a)
```

3. Run the commands `help(numpy.arange)` and `help(numpy.linspace)` to see how to use these functions to generate regularly-spaced values, then use those values to test your rescale function.

       a = numpy.arange(1.0,10.0,2.0) # arange([start,] stop[, step,], dtype=None)
       print a
       array([ 1.,  3.,  5.,  7.,  9.])
       
       rescale(a)
       array([ 0.  ,  0.25,  0.5 ,  0.75,  1.  ])
       
       # linspace(start, stop, num=50, endpoint=True, retstep=False, dtype=None)
       
       a = numpy.linspace(1.0,10.0,10) # 
       print a
       array([  1.,   2.,   3.,   4.,   5.,   6.,   7.,   8.,   9.,  10.])
       
       rescale(a)
       array([ 0.        ,  0.11111111,  0.22222222,  0.33333333,  0.44444444,
        0.55555556,  0.66666667,  0.77777778,  0.88888889,  1.        ])

## Defining defaults

Can do:

      numpy.loadtxt('inflammation-01.csv', delimiter=',')
      
 miss out `name` but we still require `delimiter`.
 
 Can define defaults:
 
    def center(data, desired=0.0):
        '''Return a new array containing the original data centered around the desired value (0 by default).
        Example: center([1, 2, 3], 0) => [-1, 0, 1]'''
        return (data - data.mean()) + desired
 
 So can run:
 
     test_data = numpy.zeros((2, 2))
     print center(test_data, 3)
     [[ 3.  3.]
     [ 3.  3.]]
     center(dest_data)
 
 Can now see why the `delimiter` has to be specified in `loadtxt`:
 
     numpy.loadtxt?
     Signature: numpy.loadtxt(fname, dtype=<type 'float'>, comments='#', delimiter=None, converters=None, skiprows=0, usecols=None, unpack=False, ndmin=0)
     Load data from a text file.
     ...

Do not need to specify the value to centre around any more:

    more_data = 5 + numpy.zeros((2, 2))
    print 'data before centering:', more_data
    data before centering: [[ 5.  5.]
                            [ 5.  5.]]
    print 'centered data:', center(more_data)
    centered data: [[ 0.  0.]
                    [ 0.  0.]]

Makes for more user friendly functions:

    def display(a=1, b=2, c=3):
         print 'a:', a, 'b:', b, 'c:', c

    display()
    display(55)
    display(55, 66)
    display(c=42)
    
## Optional exercise

1. Rewrite the `rescale` function so that it scales data to lie between 0.0 and 1.0 by default, but will allow the caller to specify lower and upper bounds if they want. Compare your implementation to your neighbor's: do the two functions always behave the same way?

# Lesson 3: loops

Have an `analyze` function

```
import numpy as np
from matplotlib import pyplot as plt

def analyze(filename):
    data = np.loadtxt(fname=filename, delimiter=',')
    
    plt.figure(figsize=(10.0, 3.0))
    
    plt.subplot(1, 3, 1)
    plt.ylabel('average')
    plt.plot(data.mean(0))
    
    plt.subplot(1, 3, 2)
    plt.ylabel('max')
    plt.plot(data.max(0))
    
    plt.subplot(1, 3, 3)
    plt.ylabel('min')
    plt.plot(data.min(0))
    
    plt.tight_layout()
    plt.show() 
```

Can call the routine now:

    analyze('data/inflammation-01.csv')
 
Want to be able to do this for all our data files.

Suppose we want to print each character in the word "lead" on a line of its own
Could define a function:

```
def print_characters(element):
    print element[0]
    print element[1]
    print element[2]
    print element[3]
```

Then:

```
print_characters('lead')
```

This is a bad approach because?

1. ***It doesn't scale***: if we want to print the characters in a string that's hundreds of letters long, we'd be better off just typing them in.

       print_characters("hippopotamus")

2. ***It's fragile***: if we give it a longer string, it only prints part of the data, and if we give it a shorter one, it produces an error because we're asking for characters that don't exist.

       print_characters("tin")

Better approach:

```
def print_characters(element):
    for char in element:
        print char
```

Can:

* call the loop variable anything
* there must be a colon at the end of the line starting the loop
* must indent the body of the loop.

Here's another example:

```
def length(string):
    length = 0
    for letter in string:
        length = length + 1
    return length
```

Counts the number of letters in a string. You can time it using one of the *magic function* provided in *ipython*:

    %timeit length("Hippopotamus")

This is such a common function so that there is a python routine that does this:

    %timeit len("hippopotamus")
    
Note that you can re-use loop variables:

    for letter in "hippopotamus":
         print letter
         
    print letter
    
## Exercises

1. Python has a built-in function called `range` that creates a list of numbers: `range(3)` produces `[0, 1, 2]`, `range(2, 5)` produces `[2, 3, 4]`. Using `range`, write a function called `rev` that takes a string as input, and produces a new string with the characters in reverse order (As always, be sure to include a `docstring`):

       print rev('Newton')
       notweN
       
Solution:

    def rev(string):
        length=len(string)-1
        for count in string:
            print string[length]
            length=length-1

2. Exponentiation is built into Python:

       print 5**3
       125

It also has a function called `pow` that calculates the same value. Write a function called `expo` that uses a loop to calculate the same result.

## Lists

Built into the language:

    odds = [1,3,5,6,7]
    type(odds)           # Check
    print odds[0]
    print odds[-1]
    
Can loop over the elements:

    for number in odds:
        print number

Can change values in a list but not in a string:

    names = ['Newton', 'Darwing', 'Turing'] # typo in Darwin's name
    print 'names
    names[1] = 'Darwin' # correct the name
    print names

For example:

    name = 'Bell'
    name[0] = 'b'

Numbers and strings are ***immutable***, lists and arrays are ***mutable***,

Can change the contents of a list:

     odds.append(11)
     print odds
     
     del odds[0]
     print odds
     
     odds.reverse()
     print odds
     
     odds.<TAB><TAB>

### Exercise

1. Write a function called `total` that calculates the sum of the values in a list. (Python has a built-in function called `sum` that does this for you. Please don't use it for this exercise.)

Solution:

```
def total(array):
    tot = 0
    for num in array:
        tot += num
    return(tot)
```

## Processing multiple files

Can now process multiple files. Need:

    import glob

Provides a `glob` function that finds files whose names match a pattern.
For example:

    print glob.glob("*.ipynb")
or

    print glob.glob("data/*.csv")

Can use this to cycle through the data files:

```
filenames = glob.glob('data/*.csv')
filenames = filenames[0:3]
for f in filenames:
    print f
    analyze(f)
```

### Exercise

1. Write a function called `analyze_all` that takes a filename pattern as its sole argument and runs `analyze` for each file whose name matches the pattern.
    
# Lesson 4: Conditionals

## Conditionals

A conditional in python looks like:

    num = 37
    if num > 100:
       print "greater"
    else:
       print "not greater"
    print "done"
    
So, we can use this to write a function that tells the sign of a number:

```
def sign(num):
    if num > 0:
        return 1
    elif num == 0:
        return 0
    else:
        return -1
```

Use of `and`

```
if (1 > 0) and (-1 > 0):
    print 'both parts are true'
else:
    print 'one part is not true'
```

use of `or`:

```
if (1 < 0) or ('left' < 'right'):
    print 'at least one test is true'
```

### Exercises

1. `True` and `False` aren't the only values in Python that are true and false. In fact, any value can be used in an `if` or `elif`. After reading and running the code below, explain what the rule is for which values are considered true and which are considered false. (Note that if the body of a conditional is a single statement, we can write it on the same line as the if.)

```
if '': print 'empty string is true'
if 'word': print 'word is true'
if []: print 'empty list is true'
if [1, 2, 3]: print 'non-empty list is true'
if 0: print 'zero is true'
if 1: print 'one is true'
```

2. Write a function called near that returns True if its first parameter is within 10% of its second and False otherwise. Compare your implementation with your partner's: do you return the same answer for all possible pairs of numbers?

## Nesting

Combining ifs, loops and functions.

```
numbers = [-5, 3, 2, -1, 9, 6]
total = 0
for n in numbers:
    if n >= 0:
        total = total + n
print 'sum of positive values:', total
```

or can have a sum for positive and negative numbers:

```
pos_total = 0
neg_total = 0
for n in numbers:
    if n >= 0:
        pos_total = pos_total + n
    else:
        neg_total = neg_total + n
```

Can nest loops:

```
for consonant in'bcd':
    for vowel in 'ae':
        print consonant + vowel
```

### Exercises
1. Will changing the nesting of the loops in the code above—i.e., wrapping the loop over consonants around the loop over vowels —change the final result?
Python (and most other languages in the C family) provides in-place operators that work like this:

```
x = 1  # original value
x += 1 # add one to x, assigning result back to x
x *= 3 # multiply x by 3
print x
6
```

2. Rewrite the code that sums the positive and negative numbers in a list using in-place operators. Do you think the result is more or less readable than the original?

# Defensive programming

## Assertions

Expect that things will go wrong. Put checks, or assertions, in your programs to make sure that things are working as they should be:

```
numbers = [1.5, 2.3, 0.7, -0.001, 4.4]
total = 0.0
for n in numbers:
    assert n >= 0.0, 'Data should only contain positive values'
    total += n
print 'total is:', total
```

Broadly speaking assertions fall into 3 classes:

1. A ***precondition*** is something that must be true at the start of a function in order for it to work correctly.
2. A ***postcondition*** is something that the function guarantees is true when it finishes.
3. An ***invariant*** is something that is always true at a particular point inside a piece of code.

General syntax is:

     assertion Condition, Error on condition fail
     

Good notes on:

* Test driven development
* Debugging

Encourage you to read them.

# Lesson: command line programs

Normally use python from the command line from *scripts*. Want to write a script that will:

* Use the values of command-line arguments in a program.
* Handle flags and files separately in a command-line program.
* Read data from standard input in a program so that it can be used in a pipeline.

You will normally see at the top of script files:

```
#!/usr/bin/python                                                                                 
#!/usr/local/bin/python                                                                           
#!/usr/bin/env python
```

which tells the shell where to find the Python interpreter. The first two hard wire the location of Python while the third tells it to use the first one it finds in your path. Another way of doing the third version would be to use:

```
python filename.py
```
which achieves the same objective (well almost, there are file permissions to be taken into account as well).

Going to edit a file. In ipython type:

    !edit sys-version.py

Will fire up your favourite editor and then type:

     import sys
     print "Version of python being used ",sys.version
     
Save the file and from within `ipython` execute the code:

     %run sys-version.py

or

     !python sys-version.py

or

      !ipython sys-version.py
 
Now add the line to your script (*argument values*):
 
    print "sys.argv is ",sys.argv

Run it a couple of times with arguments. 

Note when run with `ipython` it gives the full path name to the script but not when `python` or the `%run` command is used.

Now let's create a new script `read-1.py`:

```
import sys
import numpy as np

 def main():
     script = sys.argv[0]
     filename = sys.argv[1]
     data = np.loadtxt(filename, delimiter=',')
     for m in data.mean(axis=1):
         print m

 # Call the function
 main()
```

Note:

* If you do not specify an input file you get ugly output. Could fix by adding:

```
if len(sys.argv) < 2:
   print "You must specify an input file."
   sys.exit(1)
```
* Cannot tell whether the output is correct

```
!ls data/small-*.csv
!cat data/small-01.csv
%run read-1.py data/small-01.csv
```

* should really be using Python's `argparse` library rather than handling `sys.arg` directly.

Let's persist to now try to handle multiple files:

```
!cp read-1.py read-2.py
!vi read-2.py
```
Now remember:

* `sys.arv[0]` is the script name.
* `sys.argv[1:]` captures the other command line arguments


Now want:

```
import sys
import numpy as np

def main():
    script = sys.argv[0]
    for filename in sys.argv[1:]:
        data = np.loadtxt(filename, delimiter=',')
        for m in data.mean(axis=1):
            print filename," ",m

main()
```

## Using command line flags

Should really be using the `getop` library to parse command line arguments but it is instructive to look at a couple of examples.

Want to add `--min`, `--mean`, and `--max` flags. One way to do it:

```
!cat code/readings-04.py

import sys
import numpy as np

def main():
    script = sys.argv[0]
    action = sys.argv[1]
    filenames = sys.argv[2:]

    for f in filenames:
        data = np.loadtxt(f, delimiter=',')

        if action == '--min':
            values = data.min(axis=1)
        elif action == '--mean':
            values = data.mean(axis=1)
        elif action == '--max':
            values = data.max(axis=1)

        for m in values:
            print m

main()
```

Running it:

```
%run code/readings-04.py --max data/small-01.csv
```

My colleague thought that there were a number of things wrong with the above:

1. `main` is too large to read comfortably.

2. If `action` isn't one of the three recognized flags, the program loads each file but does nothing with it (because none of the branches in the conditional match). Silent failures like this are always hard to debug.

So, some refactoring results in:

```
!cat code/readings-05.py

import sys
import numpy as np

def main():
    script = sys.argv[0]
    action = sys.argv[1]
    filenames = sys.argv[2:]
    assert action in ['--min', '--mean', '--max'], \
           'Action is not one of --min, --mean, or --max: ' + action
    for f in filenames:
        process(f, action)

def process(filename, action):
    data = np.loadtxt(filename, delimiter=',')

    if action == '--min':
        values = data.min(axis=1)
    elif action == '--mean':
        values = data.mean(axis=1)
    elif action == '--max':
        values = data.max(axis=1)

    for m in values:
        print m

main()
```

Running it:

```
%run code/readings-05.py --max data/small-01.csv
```

## Handling standard input

Can read from standard input:

```
import sys
count = 0
for line in sys.stdin:
     count += 1
Enter lines of text
as many as you want
and when you are finished type
^D
print count
```
Can put this in a file:

```
!cat code/count-stdin.py
```

and run it:

```
!python code/count-stdin.py < data/small-02.csv
2 lines in standard input
```

but note:

```
%run code/count-stdin.py < data/small-02.csv
0 lines in standard input
```

The `%run` does not understand redirection - this is the kind of stuff you can spend ages beating your head against a wall over. Also, forgetting the `<` symbol can lead to problems (program appears to stall as nothing is going in to standard input).

A final couple of examples. Converting the program to read from stdin if no files are provided:

```
!cat code/readings-06.py
```

but trying to run this:

```
!ipython code/readings-06.py --mean < data/small-01.csv
```

causes problems with `python` but not `python`.

```
!python code/readings-06.py --mean < data/small-01.csv
```

To fix the `ipython` you need to provide it with a `--` to tell it where the command line arguments so that:

```
!python code/readings-06.py --mean < data/small-01.csv
```

works ok.

# Lesson: Errors

## Traceback

An example of a traceback error:

```
from code.errors_01 import favorite_ice_cream
favorite_ice_cream()
 ---------------------------------------------------------------------------
IndexError                                Traceback (most recent call last)
<ipython-input-407-cf15931530da> in <module>()
----> 1 favorite_ice_cream()

/Users/mario/GitRepos/2015-04-16-imperial-students/python/code/errors_01.py in favorite_ice_cream()
      5         "strawberry"
      6     ]
----> 7     print ice_creams[3]

IndexError: list index out of range
```

Have two levels (number of arrows: `favorite_ice_cream()` and `print ice_creams[3]`). Last level shows where the error took place. Error message is `IndexError: list index out of range`. **Type of error** -> IndexError with an **Error message**.

Try this:

```
from code.errors_02 import print_friday_message
print_friday_message()
```

1. How many **levels** does the traceback have?
2. What is the **file name** where the error occurred?
3. What is the **function name** where the error occurred?
4. On which **line number** in this function did the error occur?
5. What is the **type of error**?
6. What is the **error message**?

## Syntax error

Interpreter has problems reading your code.

SyntaxError: Invalid syntax

```
def some_function()
    msg = "hello, world!"
    print msg
     return msg
```

IndentationError: unexpected indent

```
def some_function():
    msg = "hello, world!"
    print msg
     return msg
```

Be careful about mixing tabs and spaces.

## Variable names

NameError: name 'boot' is not defined

```
print boot
```
NameError: name 'mycount' is not defined

```
for number in range(10):
    mycount = mycount + number
print "The count is: " + str(mycount)
```

Forgetting to define/initialize a variable. Or you may have initialised it but bot the case wrong, e.g. `MyCount = 0`.

## Item errors

IndexError: list index out of range

```
letters = ['a', 'b', 'c']
print "Letter #1 is " + letters[0]
print "Letter #2 is " + letters[1]
print "Letter #3 is " + letters[2]
print "Letter #4 is " + letters[3]
```

Similarly with a `dictionary`:

KeyError: 'oregon'

```
us_state_capitals = {
    'california': 'sacramento',
    'virginia': 'richmond',
    'new york': 'albany',
    'massachusetts': 'boston'
}

print "The capital of Oregon is: " + us_state_capitals['oregon']
```

## File errors

IOError: [Errno 2] No such file or directory: 'myfile.txt'

```
file_handle = open('myfile.txt', 'r')
```

Another error:

IOError: File not open for reading

```
file_handle = open('myfile.txt', 'w')
file_handle.read()
```

# References

* [5 reasons why Python is a popular teaching language](http://radar.oreilly.com/2015/04/five-reasons-why-python-is-a-popular-teaching-language.html)
