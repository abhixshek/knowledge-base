## Python's view of program execution:
your **source code** (the text statements in your .py file) is first converted to **byte code** and then routed to something called a "virtual machine".

**Compilation is simply a translation step, and byte code is a lower-level, platform-independent representation of your source code.**
(byte code is stored in `.pyc` files)

Byte code files are also one way to ship Python programs—Python is happy to run a program if all it can find are .pyc files, even if the original .py source files are absent.

> [!INFO]
below is how you start the specific python version on windows, i.e. by giving the full path to the python.exe of that version in case just $python doesnt start the interactive session of the python version you want to use.
![[Pasted image 20230305162124.png]]

*first_script.py*
o/p:
win32
1267650600228229401496703205376
Spam!Spam!Spam!Spam!Spam!Spam!Spam!Spam!

**NOTE:** that the module file is called `*first_script.py`. As for all **top-level files**, it could also be
called simply `script`, but files of code you want to import into a client have to end with
a `.py` suffix.

**NOTE:** you can route the output of a Python script to a file to save it for later use or inspection by using special shell syntax: `$python first_script.py > output_first_script.txt`
This is generally known as **stream redirection.** 

**NOTE:** the input() built in function in python supports input stream redirection, which means you can have your program read input from a file: `$python taking_user_input.py < user_inp.txt`

**NOTE:** (Windows) If your PATH env variable doesn’t include Python’s directory, and neither Python nor your script file
is in the directory you’re working in, use full paths for both:
`D:\other> C:\Python30\python c:\code\otherscript.py`

More generally, a **module** is mostly just a package of variable names, known as a **namespace**.
The names within that package are called **attributes**—an attribute is simply a variable name that is attached to a specific object (like a module). Remember, when you do:
`>>>import a_module` # this a_module is a_module is an object of type(class) 'module'. So names within this a_module will be referred as attributes of a_module in the program/module this is being imported into. This is similar to when defining a class and its variables. The variables of a class object are referred to as attributes of that class object outside the class. 

when using imports, you can access the third_level attributes in the first(top) level script.
![[Pasted image 20230313132601.png]]
**NOTE:** in the above exercise, I imported third_level.py into second_level.py and second_level.py into first_level.py. you can see in the code directory that .pyc files have been created for second and third_level module files as they were used in import statements but no .pyc file for first_level.py as it was the top level script. 


---

### Types and operations:
Python is a **dynamically typed**(it keeps track of types for you automatically instead of requiring declaration code) and **strongly typed** language (you can perform on an object
only operations that are valid for its type).
Python programs can be decomposed into **modules, statements, expressions, and objects.**

![[Pasted image 20221030142108.png]]

    >>>type(5+4j)
    <class 'complex'>

````python
import math
math.pi
3.14281568461
math.sqrt(81)
9.0
````
**math.ceil() - ceil function math module**
````python
import math
math.ceil(1.2) 
2
math.ceil(1.0001) 
2
math.ceil(1.999999)
2
math.ceil(0.999999)
1
math.ceil(-0.9999)
0
math.ceil(-2.45)
-2
````

````python
import random
random.random()
0.585101340
random.choice([3456,2,23,5]) #pass any iterable
23
````

[[strings]]
Strings are also sequence, i.e.  a **positionally** ordered collection of other objects.
Other types of sequences include lists, tuples. 
**NOTE:** Sets aren't sequences - they have no order and they can't be indexed via `set[index]` - they even don't have any kind of notion of indices. (They are `iterable`, though - you can iterate over their items.)

Among python's core data types numbers, strings, and tuples are immutable. lists and dictionaries are mutable.

    # len() fxn can be applied on any iterables, i.e. str,list, tuple,etc. i.e. it is a 
    general tool not something exclusive to strings only. what is exclusive to an object, 
    usually takes the form of a method of that object. 

````python
>>> q = 'abhishek is a boy'
>>> q[2:12:2]
'hse s'
>>> len(q)
17
>>> q[2:12:1]  # 1 is the default step size in fact. so no need to pass it. 
'hishek is '
>>> q[2:12:-1] #empty output because you want to start from index 2 and end at index 12 but have given a negative increment.
''
>>> q[24:12:-1] #NOTICE although the last index for the above string (q) is 16 when we specify any number greater than that, it automatically falls down to the max possible value for the index, which in this case is 16.
'yob '
>>> q[24:]
''
>>> q[24::-1]
'yob a si kehsihba'
>>> q[24::1]
''
>>> q[0:100:2]
'ahse saby'
````

**string methods:**
These are your first line of text processing tools.

    .split(',') , .replace('a','b')  , .strip()  , .lstrip()  , .rstrip()  , .find('mang')      
    , .upper()  ,  .lower()  , .isalpha() , .isnumeric(), .capitalize()

`.strip()` removes whitespace from the beginning and end of the string. i.e. it removes new line charaters also **not just spaces.**

[.isnumeric() , isdigit() , .isdecimal() - differences](https://stackoverflow.com/questions/44891070/whats-the-difference-between-str-isdigit-isnumeric-and-isdecimal-in-pyth#:~:text=By%20definition%2C%20isdecimal()%20%E2%8A%86%20isdigit()%20%E2%8A%86%20isnumeric().%20That%20is%2C%20if%20a%20string%20is%20decimal%2C%20then%20it%27ll%20also%20be%20digit%20and%20numeric.)

````python

>>>'%s apple %s mango %s' % (q,w,r)
'abhishek mishra apple dfsdf mango 65343'
>>>'%s apple %s mango %s'%(q,w,r) #this shows giving space doesnt matter.
'abhishek mishra apple dfsdf mango 65343'
>>>'{} apple is goood. {} mango is bad. {}'.format(q,w,r) #same thing using .format()
'abhishek mishra apple is goood. dfsdf mango is bad. 65343'

````

**NOTE**: to list all the attributes (not just functions) available for an object, pass that object to dir() built in function.
    dir(string_object)
**NOTE:** to know what a function or an object method does, pass the fxn name to help() 
    w = 'ronaldo'
    help(w.replace)
    # you can also call help() on an object. but it will return a lengthy output explaining all the methods of that object. 

To do pattern matching in Python, we import a module called `re`
`>>>import re`
Pattern matching is not covered in Mark Lutz book. It is a fairly advanced concept but an essential one to learn. 


[[lists]]
### list methods:
    .append(value/object) , .pop(index), .insert(index,object/value) , .remove(value)
    sort(reverse=True/False) , .reverse()   
    #NOTE: .reverse() does not reverse in sorted manner. whatever the list is, it will 
    reverse the list so that elements from last become first and so on.    

**NOTE:** since lists are mutable objects, most list methods change the list object *in-place*, instead of creating a new one.

NOTE: to delete an item at an index of a list you can ofcourse use the list methods .pop() and .remove() but you can also use python's del operator. 
```
>>>a = [5,8,2]
>>>del(a[1]) #removes 8 from the list
>>>del a[1] #works the same way as above. i.e. in del parenthesis are not compulsory.
```


[[dictionary]]

```
>>>sorted(dict_object) #returns a sorted list of the keys of the dictionary
```




---
## Number types:
A complete inventory of Python’s numeric toolbox includes:
- Integers and floating-point numbers
- Complex numbers
- Fixed-precision decimal numbers
- Rational fraction numbers
- Sets
- Booleans
- Unlimited integer precision
- A variety of numeric built-ins and modules

Python also allows us to write integers using hexadecimal, octal, and binary literals; offers a complex number type; and allows integers to have unlimited precision (they can grow to have as many digits as your memory space allows).



.bit_length() returns the no. of bits in the memory the number would take. 
```
>>>c = 2
>>> c.bit_length() #since 2 in binary is 10 which is basically 2 bits
2
>>> c = 3
>>> c.bit_length() #since 3 in binary is 11 which is basically 2 bits
2
>>> c = 4
>>> c.bit_length() #since 4 in binary is 100 which is basically 3 bits
3
```




