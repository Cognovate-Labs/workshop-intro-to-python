# ECE 16 – Intro to Python
## Expectations
Our goal is to introduce you to basic Python programming, execution control, and some Python specific syntax. This exersize will try to introduce these concepts, and by the end of the course you should have a good understanding of them. We will go through them in this order:
0. Variable Assignment and Python basics syntax
1. Execution control (loops, logic)
2. Try/except clauses (error catching)
3. Functions (also known as methods)

## 0. Variable Assignment and Syntax
### 0.0 Variable Types
Python is a dynamically typed language. This means you do not need to declare variable types when assigning, however there most certainly are variable types in Python (it is strongly but implicitly typed)!
For example:
```python
a = 's'
b = 1
c = 1.0
d = [1,2,3]
e = "string"
print(type(a))
print(type(b))
print(type(c))
print(type(d))
print(type(e))
```
Will print:
```
<class 'str'>
<class 'int'>
<class 'float'>
<class 'list'>
<class 'str'>
```
A couple things to note here:
1. There are no 'chars' vs. "strings". A 'char' is simply a "string" of length 1. Strings are always arrays.
  ```python
  for char in "hello":
      print(char)
  char = 'a'
  print(char[0])  # even a single character is still a string array!
  ```
2. When we typed `1.0`, Python "inferred" that we wanted a float (it could not be an int), but when we typed `1` Python verified that this could be an int so it chose that. If you want to force a type, you can do it as follows:
```
b = float(1)
c = int(1.0)  # note that this will truncate, NOT round!
```

### 0.1 List Syntax
Lists are the basic "iterable" (array) in Python. Lists are an ordered collection of other objects. These objects need not be of the same type and can even be other lists! There are other types of arrays, including strings, tuples, and sets. We will probably only ever use lists, strings, and tuples in this course, but you are encouraged to try and understand all of the data types available to you since the optimal choice depends on your specific use case.
```python
a = [1, 2.5, "string"]
b = [0, None]
c = a + b
d = [a, b]
for sublist in d:
    print("Working on sublist: " + str(sublist))
    for element in sublist:
          print(type(element))
```

Again, don't worry about the nested loops yet!
To acess elements in a list or part of a list, we use slices. Slices with postive numbers denote "from the start of the list" and negative numbers denote "counting from the end of the list".

Slicing syntax is: `list[start:stop:step]`. For example:

```python
a = [1, 2, 3, 4, ["firstelemlist2", "secondelemlist2"]]
print(a[0])
print(a[0::2])
print(a[-2])
print(a[-1][0])
```

### 0.2 Mutability
You may have heard about mutable vs. immutable objects in Python. What this means is: can we make changes to this object in memory, or do we have to re-assign it?

This may be easier to understand in practice.
```python
# Example of unmutable object
a = 1
b = a
a = 2
print(b) # prints 1

# Example of mutable object
a = [1,2]
b = a[:]
a[0] = "new_item"
print(b) # prints [1,2]
```

Floats/ints are *immutable* while lists are *mutable*. For this class, lists will be the only mutable objects you will deal with.

The other commonly used "iterators" that you may confuse with lists at first are tuples. Tuples are generally used in variable assignments, while lists are generally used in iterations:

```python
# Example use of a tuple
tup = ("a", "b", "c")
tup[1] = "d"
# An error should be raised, but if not:
print(tup)
# No changes to the items

# A common use is to assign return values with tuples:
def my_function():
    a = 1
    b = 2
    tup = (a,b)
    return tup
x, y = my_function()
print(x)
print(y)

# Note that this is a trivial function and is equivalent to:
def my_function():
    return 1, 2
```

So **why** are there mutable and inmutable objects in Python? Here is the breakdown:

Mutable:
  1. Modify: fast
  2. Read: slow

Immutable:
  1. Modify: slow (need to make a copy to edit)
  2. Read: very fast

### **0.3: Exercises**
1. Create a list called "list_1" containing the ints (integers) 1-10.
2. Create a list "list_2" containing 11-20 **as floats**.
3. Update "list_1" so that its first 3 elements get replaced with the list ```["one", "two", "three"]```.
   1. Print list_1. It should look like: ```['one', 'two', 'three', 4, 5, 6, 7, 8, 9, 10]```
4. Create a tuple containing the words "eleven", "twelve" and "thirteen". Assign it to the first three elements from 2.
   1. Print list_2. It should look like: ```['eleven', 'twelve', 'thirteen', 14.0, 15.0, 16.0, 17.0, 18.0, 19.0, 20.0]```
5. Join the two lists into a new list in 2 different ways:
   1. Use the .extend() method. Give the name "joint_1" to this list.
   2. Use the "+" operator. Give the name "joint_2" to this list.
   3. Print the output. It should look like: ```['one', 'two', 'three', 4, 5, 6, 7, 8, 9, 10, 'eleven', 'twelve', 'thirteen', 14.0, 15.0, 16.0, 17.0, 18.0, 19.0, 20.0]```
6. We want to make "joint_2" a list of fixed length to which we can continously add new elements. We will create a function to do this for us. The function should accept the "fixed length" list as well as the "to append" list as arguments and output the "new list" as output. In your `list_shift()` function, include comments to explain how you arrived at your solution.
 
    The signature should be:
    ```python
    def list_shift(base_list, new_data):
        return base_list
    ```
    And the output:
    ```python
    >> fixed_length_list = [1,2,3,4]
    >> new_data = [5,6,7]
    >> list_shift(fixed_length_list, new_data)
    [4,5,6,7]
    ```
7. **Bonus**: are there any edge cases that the your function doesn't handle? Can you think how you might address them?

## 1. Execution Control
Like most other languages you have probably used, Python has while loops, for loops, and execution control (such as if/else) statements. Let's start with for loops.

### 1.0 For Loops
We assume that most are familiar with the logical operation of a for loop. The main difference Python has with other languages is the syntax of the for loops. In C you might write:

```c
char data[] = {'a','b','c'};
for (int i=0; i<3; i++) {
    printf("%c\n", data[i]);
}
``` 

If you write C-style Python, you might get something like below. It works, but it's not "Pythonic":

```python
data = ['a','b','c']
for i in [0,1,2]:
    print(data[i])
```

The "Pythonic" way to do this is to focus on simplicity and elegance, like you see below:

```python
data = ['a','b','c']
for char in data:
    print(char)
```

### 1.1 While Loops:
While loops in Python are relativley "unremarkable" compared to other Python constructs.

```python
i = 0
while i < 10:
    i += 1
```

Using more complex logic:

```python
import numpy as np

data=[]
while len(data) < 10:
    data.append(np.random.rand())
```

Note that we need to define `data` and `i` before the loop starts. Python does not offer the ability to create the counter variable in the loop syntax. In fact, the general use of counter variables is highly discouraged.

### 1.2 If Statements & Logic Control:
We have used some boolean logic in the examples above. The only clarifications to make are:
  1. In Python (as in most languages) int(0) and int(1) have boolean values (0 == False and 1 == True).
  2. True/False *must be capitalized* to be detected as booleans (to constrast with variable names).
  3. Boolean operators are:
     1. "not": `if not (2<1):`
     2. "or"   `if (a<10) or (a>20):`
     3. "and"  `if not (2<1) or (a==10 and True):
     `

Boolean logic can also be used to test existence of objects:

```python
a = None
b = []
if not a:
  print("a is boolean false, Nonetype evaluates to False")
if not b:
  print("b is boolean false, like all empty lists")
```

As for full **if conditional statements**, the syntax is:

```python
if <condition>:
  pass
elif <condition>:
  break
else:
  pass
```

Now let's look at those `pass` and `break` keywords!

### 1.3 Execution Control Keywords
There are special keywords that allow you to break out of a logic block (loop, while, if, etc.), continue onto the next iteration of a loop, skip it, etc.

```python
for number in range(10):
    if number == 2:
        pass
        print("this will get executed")
    elif number == 5:
        continue
        print("this will not get executed")
    elif number == 9:
        print("this will not get executed because of the break statement below")
    elif number == 8:
        break
    else:
        print(number)
```

### 1.4 Exercises

1. Create a list of commands. It should include "STATUS", "ADD", "COMMIT", "PUSH", along with three other commands of your choosing.
2. Create a for loop that loops over each command and prints it to the console.
3. Repeat step 2 using a while loop instead of a for loop. Compare your solutions for both approaches. Which one is more straightforward to implement?
4. Create a seperate list of strings that has the following contents in order: "PUSH FAILED", "BANANAS", "PUSH SUCCESS", "APPLES".
5. Assign the value "SUCCESS" to a variable called **text**.
6. Test the following logic statements:
    1. `"SUCCESS" in "SUCCESS"`
    2. `"SUCCESS" in "ijoisafjoijiojSUCCESS"`
    3. `"SUCCESS" == "ijoisafjoijiojSUCCESS"`
    4. `"SUCCESS" == text`
    5. `"finish" in "ijoisafjoijioj\finish"`
    6. Are these comparing the whole string or are they comparing character by character?
7. Make a for loop that loops over the list from step 4. Print each word unless it contains the string from step 5, in which case you should exit the loop and print: "This worked!"

## 2. Error Handling

### 2.0 Try-Except Clauses

Often times you may encounter errors. Python gives us the functionality to catch these errors and deal with them accordingly. Try running this example *without* any error catching:

```python
items = [1.1,2.2,3.3,"upsie"]
for item in items:
    print(int(item))
```
Now try it *with* error catching:

```python
items = [1.1,2.2,3.3,"upsie"]
for item in items:
    try:
        print(int(item))
    except ValueError:
        print(item)
```

Note that while it is not *required* to specity which type of error to catch, it is good practice to do so to avoid catching unexpected errors. For example, see what happens if we had a typo and did not specify the type of error to catch. Here the undefined variable is caught, but then our exception fails because the error did not come from the typecasting as expected but rather from the typo in the variable name (iem instead of item):

<pre lang="python">
items = [1.1,2.2,3.3,"upsie"]
for item in items:
    try:
        print(int(<b>iem</b>))
    except:
        print(item)
</pre>

### 2.1 Assertions
Assertions are an easy way to verify function arguments are what we expect (great for test-driven design!):

```python
def add_ints(int1, int2):
  try:
      assert (isinstance(int1, int) and isinstance(int2, int)), "Inputs must be integers!"
      print("Inputs are confirmed to be ints")
      return int1+int2
  except AssertionError:
      print("Attempting to cast to int")
      try:
          int1 = int(int1)
          int2 = int(int2)
          return int1 + int2
      except ValueError:
          print("Could not cast to int, inputs are not a number")
          return None
```

Test:

```python
add_ints(1,2)
add_ints(1,2.0)
add_ints(1,"nope")
```

### 2.2 Exercises
1. Create a string containing your first name. Mine is: `name = "ramsin"`
2. Encode the string to a byte array: `byte_name =  name.encode('utf-8')`
3. Append a non-utf-8 character: `byte_name_bad = byte_name + b'\xef'` # \xef is incomplete unicode
4. Attempt to decode `byte_name_bad` back to a string using the `.decode()` method of byte types. What error did you get? Record the name of the error.
5. To get around this error, which could occur if data transmission gets garbled, we will use a try-except clause. Create a try-except clause for decoding byte types. If there is a decoding error,  set the return string to an empty string.
6. Copy this try except clause twice, once for `byte_name` and once for `byte_name_bad`. Print the result for each. `byte_name` should print the same as `name` while `byte_name_bad` should print nothing.

## 3. Functions
In Python programming, functions accept intputs, perform actions, and give outputs, but none of these are mandatory! The most basic function is:

```python
def function():
    pass

# this function call does nothing
function()
```

Notice that there is a `pass` statement in the function. Python is very demanding with indentation. If you do not put *any* statement in the function, Python does not know where your function starts/ends. This also applies to if/else statements or any other statements with indentation.

Also, note that there are no inputs or outputs in this function. Python does not require for you to assign any output value. However, you can put a `return` statement anywhere in your function to force it to return from the function with or without output values.

### 3.0 Function Syntax
Function inputs come in four basic types:
1. Required
2. Default (optional)
3. Keyword
4. Variable length

```python
def my_function(required_argument, optional_argument="default value", *extra_args):
    print(required_argument)
    print(optional_argument)
    if extra_args:
        print(extra_args)
    for extra_arg in extra_args:
        print(extra_arg)
        if extra_arg == 5:
            return "got the number "+str(extra_arg)    # this is just to demo return statements
```

Notice that required arguments must always come before optional arguments.

You can call functions using the keyword arguments or without them. Using keyword arguments allows you to switch the order of input parameters without affecting the result (i.e. you don't need to remember the order of arguments for each function).

```python
my_function("test", "okay", "extra 1", "extra 2")
my_function(optional_argument="okay", required_argument="test")
```

Note that the follwing ordering does not work.

```python
my_function(required_argument="test", "okay")
SyntaxError: positional argument follows keyword argument
```

### 3.1 Variable Scope
Unless you use the keyword global, variables are by default local. You can access global variables from inside an enclosing scope but not modify them, unless you explicitly use the `global` keyword before it.

```python
global_var1 = 123
global_var2 = 321

def function():
    local_var = 567
    
    # only accessing global_var1
    print(global_var1)
    
    # accessing and modifying global_var2
    global global_var2
    global_var2 = 42
    print(global_var2)
    return

function()

try:
    print(local_var)
except NameError:
    print("Variable does not exist!")
```

### 3.2 Exercises
No exercises here! Great job! Now you are ready to tackle the Python challenges in the rest of the class!