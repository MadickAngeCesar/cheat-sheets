# Python Cheat Sheet - Beginner to Pro

## Table of Contents
- [Basics](#basics)
- [Variables & Data Types](#variables--data-types)
- [Operators](#operators)
- [Control Flow](#control-flow)
- [Functions](#functions)
- [Data Structures](#data-structures)
- [String Operations](#string-operations)
- [File Handling](#file-handling)
- [Exception Handling](#exception-handling)
- [Object-Oriented Programming](#object-oriented-programming)
- [Modules & Packages](#modules--packages)
- [List Comprehensions](#list-comprehensions)
- [Decorators](#decorators)
- [Generators & Iterators](#generators--iterators)
- [Context Managers](#context-managers)
- [Regular Expressions](#regular-expressions)
- [Lambda Functions](#lambda-functions)
- [Built-in Functions](#built-in-functions)
- [Best Practices](#best-practices)

---

## Basics

### Hello World
```python
print("Hello, World!")

# Multiple values
print("Hello", "World", sep=", ")

# Format strings
name = "John"
print(f"Hello, {name}!")
```

### Comments
```python
# Single line comment

"""
Multi-line
comment or
docstring
"""

'''
Also a
multi-line
comment
'''
```

### Input/Output
```python
# Input
name = input("Enter your name: ")
age = int(input("Enter your age: "))

# Output
print("Name:", name)
print(f"{name} is {age} years old")
```

---

## Variables & Data Types

### Variable Assignment
```python
# Simple assignment
x = 10
name = "John"

# Multiple assignment
a, b, c = 1, 2, 3
x = y = z = 0

# Swap variables
a, b = b, a
```

### Data Types

#### Numbers
```python
# Integer
x = 10
y = -5

# Float
pi = 3.14
e = 2.718

# Complex
z = 3 + 4j

# Type conversion
int("10")      # 10
float("3.14")  # 3.14
str(100)       # "100"

# Type checking
type(10)           # <class 'int'>
isinstance(10, int) # True
```

#### Strings
```python
# String creation
s1 = 'Hello'
s2 = "World"
s3 = """Multi
line
string"""

# String operations
len("Hello")        # 5
"Hello" + " World"  # Concatenation
"Ha" * 3           # "HaHaHa"

# Indexing and slicing
s = "Python"
s[0]         # 'P'
s[-1]        # 'n'
s[0:3]       # 'Pyt'
s[::2]       # 'Pto'
s[::-1]      # 'nohtyP' (reverse)

# String methods
s.upper()           # 'PYTHON'
s.lower()           # 'python'
s.capitalize()      # 'Python'
s.title()           # 'Python Programming'
s.strip()           # Remove whitespace
s.replace('P', 'J') # 'Jython'
s.split()           # Split into list
s.startswith('Py')  # True
s.endswith('on')    # True
s.find('th')        # 2
s.count('o')        # 1
s.isdigit()         # False
s.isalpha()         # True
```

#### Boolean
```python
true_val = True
false_val = False

# Boolean operations
bool(0)       # False
bool(1)       # True
bool("")      # False
bool("text")  # True
bool([])      # False
bool([1])     # True
```

#### None
```python
x = None
print(x is None)  # True
```

---

## Operators

### Arithmetic
```python
10 + 5    # 15 (addition)
10 - 5    # 5 (subtraction)
10 * 5    # 50 (multiplication)
10 / 5    # 2.0 (division)
10 // 3   # 3 (floor division)
10 % 3    # 1 (modulus)
10 ** 2   # 100 (exponentiation)
```

### Comparison
```python
10 == 10   # True (equal)
10 != 5    # True (not equal)
10 > 5     # True
10 < 5     # False
10 >= 10   # True
10 <= 5    # False
```

### Logical
```python
True and False  # False
True or False   # True
not True        # False

# Short-circuit evaluation
x = 10
x > 5 and x < 20  # True
```

### Identity
```python
x = [1, 2, 3]
y = x
z = [1, 2, 3]

x is y      # True (same object)
x is z      # False (different objects)
x == z      # True (same values)

x is not z  # True
```

### Membership
```python
'a' in 'apple'       # True
'x' not in 'apple'   # True
1 in [1, 2, 3]       # True
```

### Bitwise
```python
5 & 3   # 1 (AND)
5 | 3   # 7 (OR)
5 ^ 3   # 6 (XOR)
~5      # -6 (NOT)
5 << 1  # 10 (left shift)
5 >> 1  # 2 (right shift)
```

---

## Control Flow

### If/Elif/Else
```python
x = 10

if x > 0:
    print("Positive")
elif x < 0:
    print("Negative")
else:
    print("Zero")

# Ternary operator
result = "Even" if x % 2 == 0 else "Odd"

# Multiple conditions
if x > 0 and x < 100:
    print("In range")
```

### For Loop
```python
# Iterate over sequence
for i in range(5):
    print(i)  # 0, 1, 2, 3, 4

# Range with start and end
for i in range(1, 6):
    print(i)  # 1, 2, 3, 4, 5

# Range with step
for i in range(0, 10, 2):
    print(i)  # 0, 2, 4, 6, 8

# Iterate over list
fruits = ['apple', 'banana', 'cherry']
for fruit in fruits:
    print(fruit)

# Enumerate (index and value)
for i, fruit in enumerate(fruits):
    print(i, fruit)

# Iterate over dictionary
person = {'name': 'John', 'age': 30}
for key, value in person.items():
    print(key, value)
```

### While Loop
```python
count = 0
while count < 5:
    print(count)
    count += 1

# With break
while True:
    user_input = input("Enter 'quit' to exit: ")
    if user_input == 'quit':
        break
    print(f"You entered: {user_input}")

# With continue
count = 0
while count < 10:
    count += 1
    if count % 2 == 0:
        continue
    print(count)  # Only odd numbers
```

### Loop Control
```python
# Break (exit loop)
for i in range(10):
    if i == 5:
        break
    print(i)

# Continue (skip iteration)
for i in range(10):
    if i % 2 == 0:
        continue
    print(i)  # Only odd numbers

# Else clause (runs if loop completes without break)
for i in range(5):
    print(i)
else:
    print("Loop completed")

# Pass (do nothing)
for i in range(5):
    pass  # Placeholder
```

---

## Functions

### Basic Functions
```python
# Function definition
def greet(name):
    return f"Hello, {name}!"

# Function call
result = greet("John")

# Multiple parameters
def add(a, b):
    return a + b

# Default parameters
def greet(name="Guest"):
    return f"Hello, {name}!"

greet()        # "Hello, Guest!"
greet("John")  # "Hello, John!"
```

### Advanced Parameters
```python
# Keyword arguments
def describe_person(name, age, city):
    print(f"{name} is {age} years old and lives in {city}")

describe_person(name="John", age=30, city="NYC")

# *args (variable positional arguments)
def sum_all(*numbers):
    return sum(numbers)

sum_all(1, 2, 3, 4, 5)  # 15

# **kwargs (variable keyword arguments)
def print_info(**info):
    for key, value in info.items():
        print(f"{key}: {value}")

print_info(name="John", age=30, city="NYC")

# Combined
def func(a, b, *args, **kwargs):
    print(a, b)
    print(args)
    print(kwargs)

func(1, 2, 3, 4, x=5, y=6)
```

### Return Values
```python
# Single return
def add(a, b):
    return a + b

# Multiple returns
def get_info():
    return "John", 30, "NYC"

name, age, city = get_info()

# Early return
def check_positive(n):
    if n <= 0:
        return False
    return True
```

### Lambda Functions
```python
# Lambda syntax
square = lambda x: x ** 2
add = lambda a, b: a + b

# Usage
square(5)     # 25
add(3, 4)     # 7

# With map, filter, sorted
numbers = [1, 2, 3, 4, 5]
squared = list(map(lambda x: x ** 2, numbers))
evens = list(filter(lambda x: x % 2 == 0, numbers))
sorted_desc = sorted(numbers, key=lambda x: -x)
```

### Scope
```python
# Global scope
x = 10

def func():
    # Local scope
    y = 20
    print(x)  # Can access global
    print(y)

# Global keyword
count = 0

def increment():
    global count
    count += 1

# Nonlocal keyword
def outer():
    x = 10
    
    def inner():
        nonlocal x
        x += 1
    
    inner()
    print(x)  # 11
```

---

## Data Structures

### Lists
```python
# Create list
numbers = [1, 2, 3, 4, 5]
mixed = [1, "hello", 3.14, True]
empty = []

# List methods
numbers.append(6)           # Add to end
numbers.insert(0, 0)        # Insert at index
numbers.extend([7, 8])      # Extend list
numbers.remove(3)           # Remove first occurrence
popped = numbers.pop()      # Remove and return last
popped = numbers.pop(0)     # Remove at index
numbers.clear()             # Remove all
numbers.index(3)            # Find index
numbers.count(3)            # Count occurrences
numbers.sort()              # Sort in place
numbers.sort(reverse=True)  # Descending
numbers.reverse()           # Reverse in place
copy = numbers.copy()       # Shallow copy

# List operations
len(numbers)        # Length
numbers[0]          # Access by index
numbers[-1]         # Last element
numbers[1:4]        # Slicing
numbers + [6, 7]    # Concatenation
numbers * 2         # Repetition

# Check membership
3 in numbers        # True

# List unpacking
a, b, c = [1, 2, 3]
first, *rest = [1, 2, 3, 4]  # first=1, rest=[2,3,4]
```

### Tuples
```python
# Create tuple (immutable)
point = (10, 20)
single = (1,)  # Note the comma

# Tuple operations
len(point)          # 2
point[0]            # 10
point + (30, 40)    # (10, 20, 30, 40)
point * 2           # (10, 20, 10, 20)

# Tuple unpacking
x, y = point
```

### Sets
```python
# Create set (unique, unordered)
numbers = {1, 2, 3, 4, 5}
empty = set()

# Set methods
numbers.add(6)              # Add element
numbers.remove(3)           # Remove (error if not found)
numbers.discard(3)          # Remove (no error)
numbers.pop()               # Remove arbitrary element
numbers.clear()             # Remove all
numbers.copy()              # Shallow copy

# Set operations
set1 = {1, 2, 3}
set2 = {3, 4, 5}

set1 | set2         # Union: {1, 2, 3, 4, 5}
set1 & set2         # Intersection: {3}
set1 - set2         # Difference: {1, 2}
set1 ^ set2         # Symmetric difference: {1, 2, 4, 5}

# Set methods
set1.union(set2)
set1.intersection(set2)
set1.difference(set2)
set1.symmetric_difference(set2)
set1.issubset(set2)
set1.issuperset(set2)
```

### Dictionaries
```python
# Create dictionary
person = {
    'name': 'John',
    'age': 30,
    'city': 'NYC'
}

# Access values
person['name']          # 'John'
person.get('name')      # 'John'
person.get('country', 'USA')  # Default value

# Dictionary methods
person['email'] = 'john@example.com'  # Add/update
person.update({'age': 31, 'country': 'USA'})
del person['city']      # Delete
popped = person.pop('age')  # Remove and return
person.clear()          # Remove all
copy = person.copy()    # Shallow copy

# Dictionary operations
len(person)             # Number of items
'name' in person        # True
list(person.keys())     # ['name', 'age', 'city']
list(person.values())   # ['John', 30, 'NYC']
list(person.items())    # [('name', 'John'), ...]

# Dictionary iteration
for key in person:
    print(key, person[key])

for key, value in person.items():
    print(key, value)

# Dictionary comprehension
squared = {x: x**2 for x in range(5)}
```

---

## String Operations

### String Formatting
```python
name = "John"
age = 30

# f-strings (Python 3.6+)
print(f"My name is {name} and I am {age} years old")
print(f"{age + 10}")  # Expressions
print(f"{age:05d}")   # Padding: 00030
print(f"{3.14159:.2f}")  # Precision: 3.14

# format() method
"My name is {} and I am {} years old".format(name, age)
"My name is {0} and I am {1} years old".format(name, age)
"My name is {n} and I am {a} years old".format(n=name, a=age)

# % operator (old style)
"My name is %s and I am %d years old" % (name, age)
```

### String Methods
```python
s = "  Hello World  "

# Case
s.upper()           # "  HELLO WORLD  "
s.lower()           # "  hello world  "
s.capitalize()      # "  hello world  "
s.title()           # "  Hello World  "
s.swapcase()        # "  hELLO wORLD  "

# Whitespace
s.strip()           # "Hello World"
s.lstrip()          # "Hello World  "
s.rstrip()          # "  Hello World"

# Search
s.find('World')     # 8
s.index('World')    # 8 (raises error if not found)
s.count('l')        # 3
s.startswith('  H') # True
s.endswith('  ')    # True

# Replace
s.replace('World', 'Python')  # "  Hello Python  "

# Split/Join
words = s.strip().split()     # ['Hello', 'World']
' '.join(words)               # 'Hello World'

# Check
s.isalpha()         # False (has spaces)
s.isdigit()         # False
s.isalnum()         # False
s.isspace()         # False
s.islower()         # False
s.isupper()         # False
```

---

## File Handling

### Reading Files
```python
# Read entire file
with open('file.txt', 'r') as f:
    content = f.read()
    print(content)

# Read line by line
with open('file.txt', 'r') as f:
    for line in f:
        print(line.strip())

# Read lines into list
with open('file.txt', 'r') as f:
    lines = f.readlines()

# Read specific number of characters
with open('file.txt', 'r') as f:
    chunk = f.read(100)
```

### Writing Files
```python
# Write (overwrite)
with open('file.txt', 'w') as f:
    f.write('Hello World\n')
    f.write('Second line\n')

# Append
with open('file.txt', 'a') as f:
    f.write('Appended line\n')

# Write lines
lines = ['Line 1\n', 'Line 2\n', 'Line 3\n']
with open('file.txt', 'w') as f:
    f.writelines(lines)
```

### File Modes
```python
# 'r' - Read (default)
# 'w' - Write (overwrite)
# 'a' - Append
# 'x' - Exclusive creation
# 'b' - Binary mode
# 't' - Text mode (default)
# '+' - Read and write

# Examples
open('file.txt', 'r')    # Read text
open('file.txt', 'w')    # Write text
open('file.txt', 'rb')   # Read binary
open('file.txt', 'r+')   # Read and write
```

### File Operations
```python
import os

# Check if file exists
os.path.exists('file.txt')

# Check if it's a file
os.path.isfile('file.txt')

# Check if it's a directory
os.path.isdir('folder')

# Get file size
os.path.getsize('file.txt')

# Get absolute path
os.path.abspath('file.txt')

# Join paths
os.path.join('folder', 'file.txt')

# Split path
os.path.split('/path/to/file.txt')  # ('/path/to', 'file.txt')

# Get directory name
os.path.dirname('/path/to/file.txt')  # '/path/to'

# Get file name
os.path.basename('/path/to/file.txt')  # 'file.txt'

# Rename file
os.rename('old.txt', 'new.txt')

# Delete file
os.remove('file.txt')

# Create directory
os.mkdir('new_folder')
os.makedirs('path/to/folder', exist_ok=True)

# Remove directory
os.rmdir('folder')

# List directory
os.listdir('.')

# Change directory
os.chdir('/path/to/directory')

# Get current directory
os.getcwd()
```

---

## Exception Handling

### Try/Except
```python
# Basic try/except
try:
    x = 10 / 0
except ZeroDivisionError:
    print("Cannot divide by zero")

# Multiple exceptions
try:
    # Code that may raise exceptions
    pass
except ValueError:
    print("Value error")
except TypeError:
    print("Type error")
except Exception as e:
    print(f"Error: {e}")

# Else clause (runs if no exception)
try:
    x = 10 / 2
except ZeroDivisionError:
    print("Error")
else:
    print(f"Result: {x}")

# Finally clause (always runs)
try:
    file = open('file.txt', 'r')
    content = file.read()
except FileNotFoundError:
    print("File not found")
finally:
    file.close()  # Always close file
```

### Raising Exceptions
```python
# Raise exception
raise ValueError("Invalid value")

# Raise with condition
x = -1
if x < 0:
    raise ValueError("x cannot be negative")

# Re-raise
try:
    # Code
    pass
except Exception as e:
    print(f"Error occurred: {e}")
    raise  # Re-raise the exception
```

### Custom Exceptions
```python
# Define custom exception
class ValidationError(Exception):
    def __init__(self, message):
        self.message = message
        super().__init__(self.message)

# Use custom exception
def validate_age(age):
    if age < 0:
        raise ValidationError("Age cannot be negative")
    if age > 150:
        raise ValidationError("Age too high")

try:
    validate_age(-5)
except ValidationError as e:
    print(f"Validation error: {e.message}")
```

---

## Object-Oriented Programming

### Classes and Objects
```python
# Define class
class Person:
    # Class variable
    species = "Homo sapiens"
    
    # Constructor
    def __init__(self, name, age):
        # Instance variables
        self.name = name
        self.age = age
    
    # Instance method
    def greet(self):
        return f"Hello, my name is {self.name}"
    
    # Method with parameters
    def have_birthday(self):
        self.age += 1
    
    # String representation
    def __str__(self):
        return f"Person(name={self.name}, age={self.age})"
    
    # Class method
    @classmethod
    def from_birth_year(cls, name, birth_year):
        age = 2024 - birth_year
        return cls(name, age)
    
    # Static method
    @staticmethod
    def is_adult(age):
        return age >= 18

# Create object
person = Person("John", 30)

# Access attributes
print(person.name)
print(person.age)

# Call methods
print(person.greet())
person.have_birthday()

# Use class method
person2 = Person.from_birth_year("Jane", 1995)

# Use static method
print(Person.is_adult(20))
```

### Inheritance
```python
# Parent class
class Animal:
    def __init__(self, name):
        self.name = name
    
    def speak(self):
        pass

# Child class
class Dog(Animal):
    def __init__(self, name, breed):
        super().__init__(name)  # Call parent constructor
        self.breed = breed
    
    def speak(self):
        return "Woof!"

class Cat(Animal):
    def speak(self):
        return "Meow!"

# Create objects
dog = Dog("Buddy", "Golden Retriever")
cat = Cat("Whiskers")

print(dog.speak())  # "Woof!"
print(cat.speak())  # "Meow!"

# Check inheritance
isinstance(dog, Dog)     # True
isinstance(dog, Animal)  # True
issubclass(Dog, Animal)  # True
```

### Encapsulation
```python
class BankAccount:
    def __init__(self, balance):
        self.__balance = balance  # Private variable
    
    def deposit(self, amount):
        if amount > 0:
            self.__balance += amount
    
    def withdraw(self, amount):
        if 0 < amount <= self.__balance:
            self.__balance -= amount
            return True
        return False
    
    def get_balance(self):
        return self.__balance

account = BankAccount(1000)
account.deposit(500)
print(account.get_balance())  # 1500
# account.__balance  # Error: can't access private variable
```

### Properties
```python
class Person:
    def __init__(self, name, age):
        self._name = name
        self._age = age
    
    @property
    def name(self):
        return self._name
    
    @name.setter
    def name(self, value):
        if not value:
            raise ValueError("Name cannot be empty")
        self._name = value
    
    @property
    def age(self):
        return self._age
    
    @age.setter
    def age(self, value):
        if value < 0:
            raise ValueError("Age cannot be negative")
        self._age = value

person = Person("John", 30)
print(person.name)  # Uses getter
person.age = 31     # Uses setter
```

### Special Methods (Magic Methods)
```python
class Vector:
    def __init__(self, x, y):
        self.x = x
        self.y = y
    
    def __str__(self):
        return f"Vector({self.x}, {self.y})"
    
    def __repr__(self):
        return f"Vector({self.x}, {self.y})"
    
    def __add__(self, other):
        return Vector(self.x + other.x, self.y + other.y)
    
    def __sub__(self, other):
        return Vector(self.x - other.x, self.y - other.y)
    
    def __eq__(self, other):
        return self.x == other.x and self.y == other.y
    
    def __len__(self):
        return 2
    
    def __getitem__(self, key):
        if key == 0:
            return self.x
        elif key == 1:
            return self.y
        raise IndexError("Index out of range")

v1 = Vector(1, 2)
v2 = Vector(3, 4)
v3 = v1 + v2  # Uses __add__
print(v3)     # Uses __str__
print(v1[0])  # Uses __getitem__
```

---

## Modules & Packages

### Importing Modules
```python
# Import entire module
import math
print(math.pi)
print(math.sqrt(16))

# Import specific functions
from math import pi, sqrt
print(pi)
print(sqrt(16))

# Import with alias
import numpy as np
import pandas as pd

# Import all (not recommended)
from math import *

# Import from package
from datetime import datetime, timedelta
```

### Creating Modules
```python
# mymodule.py
def greet(name):
    return f"Hello, {name}!"

def add(a, b):
    return a + b

PI = 3.14159

# main.py
import mymodule

print(mymodule.greet("John"))
print(mymodule.add(5, 3))
print(mymodule.PI)
```

### Packages
```python
# Directory structure:
# mypackage/
#   __init__.py
#   module1.py
#   module2.py
#   subpackage/
#     __init__.py
#     module3.py

# Import from package
from mypackage import module1
from mypackage.subpackage import module3

# __init__.py can contain initialization code
# __init__.py
from .module1 import function1
from .module2 import function2
```

### Standard Library Modules
```python
# datetime
from datetime import datetime, timedelta
now = datetime.now()
tomorrow = now + timedelta(days=1)

# random
import random
random.randint(1, 10)
random.choice(['a', 'b', 'c'])
random.shuffle([1, 2, 3])

# os
import os
os.getcwd()
os.listdir('.')
os.path.exists('file.txt')

# sys
import sys
sys.argv  # Command line arguments
sys.exit()

# json
import json
json.dumps({'name': 'John'})  # To JSON string
json.loads('{"name": "John"}')  # From JSON string

# re (regular expressions)
import re
re.match(r'\d+', '123')
re.search(r'\d+', 'abc123')
re.findall(r'\d+', 'a1b2c3')

# collections
from collections import Counter, defaultdict, deque
Counter(['a', 'b', 'a'])  # {'a': 2, 'b': 1}

# itertools
from itertools import combinations, permutations, product
combinations([1, 2, 3], 2)  # (1,2), (1,3), (2,3)
```

---

## List Comprehensions

### Basic List Comprehension
```python
# Traditional way
squares = []
for x in range(10):
    squares.append(x ** 2)

# List comprehension
squares = [x ** 2 for x in range(10)]

# With condition
evens = [x for x in range(10) if x % 2 == 0]

# Multiple conditions
nums = [x for x in range(20) if x % 2 == 0 if x % 3 == 0]

# With transformation
upper_words = [word.upper() for word in ['hello', 'world']]
```

### Nested List Comprehension
```python
# Flatten 2D list
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
flattened = [num for row in matrix for num in row]
# [1, 2, 3, 4, 5, 6, 7, 8, 9]

# Create matrix
matrix = [[i * j for j in range(5)] for i in range(5)]
```

### Dictionary Comprehension
```python
# Create dictionary
squared = {x: x ** 2 for x in range(5)}
# {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}

# From lists
keys = ['a', 'b', 'c']
values = [1, 2, 3]
d = {k: v for k, v in zip(keys, values)}

# With condition
evens = {x: x ** 2 for x in range(10) if x % 2 == 0}

# Swap keys and values
original = {'a': 1, 'b': 2, 'c': 3}
swapped = {v: k for k, v in original.items()}
```

### Set Comprehension
```python
# Create set
squared = {x ** 2 for x in range(10)}
# {0, 1, 4, 9, 16, 25, 36, 49, 64, 81}

# With condition
evens = {x for x in range(10) if x % 2 == 0}
```

### Generator Expression
```python
# Generator (lazy evaluation)
gen = (x ** 2 for x in range(10))

# Iterate over generator
for num in gen:
    print(num)

# Convert to list
squares = list(x ** 2 for x in range(10))
```

---

## Decorators

### Basic Decorator
```python
def my_decorator(func):
    def wrapper():
        print("Before function call")
        func()
        print("After function call")
    return wrapper

@my_decorator
def say_hello():
    print("Hello!")

say_hello()
# Output:
# Before function call
# Hello!
# After function call
```

### Decorator with Arguments
```python
def repeat(times):
    def decorator(func):
        def wrapper(*args, **kwargs):
            for _ in range(times):
                func(*args, **kwargs)
        return wrapper
    return decorator

@repeat(3)
def greet(name):
    print(f"Hello, {name}!")

greet("John")
# Prints "Hello, John!" three times
```

### Function Decorator with Arguments
```python
def decorator_with_args(func):
    def wrapper(*args, **kwargs):
        print(f"Arguments: {args}, {kwargs}")
        result = func(*args, **kwargs)
        print(f"Result: {result}")
        return result
    return wrapper

@decorator_with_args
def add(a, b):
    return a + b

add(5, 3)
```

### Class Decorator
```python
class CountCalls:
    def __init__(self, func):
        self.func = func
        self.count = 0
    
    def __call__(self, *args, **kwargs):
        self.count += 1
        print(f"Call {self.count} of {self.func.__name__}")
        return self.func(*args, **kwargs)

@CountCalls
def say_hello():
    print("Hello!")

say_hello()  # Call 1 of say_hello
say_hello()  # Call 2 of say_hello
```

### Built-in Decorators
```python
class MyClass:
    @staticmethod
    def static_method():
        print("Static method")
    
    @classmethod
    def class_method(cls):
        print(f"Class method of {cls}")
    
    @property
    def my_property(self):
        return self._value
    
    @my_property.setter
    def my_property(self, value):
        self._value = value
```

---

## Generators & Iterators

### Generators
```python
# Generator function
def countdown(n):
    while n > 0:
        yield n
        n -= 1

# Use generator
for num in countdown(5):
    print(num)  # 5, 4, 3, 2, 1

# Generator expression
gen = (x ** 2 for x in range(5))

# next() function
gen = countdown(3)
print(next(gen))  # 3
print(next(gen))  # 2
print(next(gen))  # 1
```

### Custom Iterator
```python
class Counter:
    def __init__(self, max_value):
        self.max_value = max_value
        self.current = 0
    
    def __iter__(self):
        return self
    
    def __next__(self):
        if self.current < self.max_value:
            self.current += 1
            return self.current
        raise StopIteration

# Use iterator
for num in Counter(5):
    print(num)  # 1, 2, 3, 4, 5
```

### Generator with send()
```python
def echo():
    while True:
        received = yield
        print(f"Received: {received}")

gen = echo()
next(gen)  # Prime the generator
gen.send("Hello")
gen.send("World")
```

---

## Context Managers

### Using Context Managers
```python
# File handling
with open('file.txt', 'r') as f:
    content = f.read()
# File is automatically closed

# Multiple context managers
with open('input.txt', 'r') as in_file, \
     open('output.txt', 'w') as out_file:
    content = in_file.read()
    out_file.write(content)
```

### Creating Context Managers
```python
# Using class
class FileManager:
    def __init__(self, filename, mode):
        self.filename = filename
        self.mode = mode
        self.file = None
    
    def __enter__(self):
        self.file = open(self.filename, self.mode)
        return self.file
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        if self.file:
            self.file.close()

# Usage
with FileManager('file.txt', 'w') as f:
    f.write('Hello')

# Using contextlib
from contextlib import contextmanager

@contextmanager
def file_manager(filename, mode):
    f = open(filename, mode)
    try:
        yield f
    finally:
        f.close()

# Usage
with file_manager('file.txt', 'w') as f:
    f.write('Hello')
```

---

## Regular Expressions

### Basic Patterns
```python
import re

# Match pattern
re.match(r'\d+', '123abc')  # Matches at beginning
re.search(r'\d+', 'abc123')  # Searches anywhere
re.findall(r'\d+', 'a1b2c3')  # Find all matches

# Patterns
r'\d'      # Digit
r'\D'      # Non-digit
r'\w'      # Word character
r'\W'      # Non-word character
r'\s'      # Whitespace
r'\S'      # Non-whitespace
r'.'       # Any character
r'^'       # Start of string
r'$'       # End of string
r'*'       # 0 or more
r'+'       # 1 or more
r'?'       # 0 or 1
r'{n}'     # Exactly n
r'{n,}'    # n or more
r'{n,m}'   # Between n and m
```

### Common Operations
```python
# Substitute
text = "Hello 123 World 456"
result = re.sub(r'\d+', 'X', text)  # "Hello X World X"

# Split
text = "apple,banana,cherry"
fruits = re.split(r',', text)  # ['apple', 'banana', 'cherry']

# Groups
match = re.search(r'(\w+)@(\w+)\.(\w+)', 'user@example.com')
if match:
    print(match.group(0))  # Full match
    print(match.group(1))  # user
    print(match.group(2))  # example
    print(match.group(3))  # com

# Named groups
pattern = r'(?P<user>\w+)@(?P<domain>\w+\.\w+)'
match = re.search(pattern, 'user@example.com')
if match:
    print(match.group('user'))    # user
    print(match.group('domain'))  # example.com
```

---

## Lambda Functions

### Basic Lambda
```python
# Lambda syntax
add = lambda a, b: a + b
square = lambda x: x ** 2

# Usage
print(add(3, 4))     # 7
print(square(5))     # 25
```

### Lambda with Built-in Functions
```python
# map()
numbers = [1, 2, 3, 4, 5]
squared = list(map(lambda x: x ** 2, numbers))

# filter()
evens = list(filter(lambda x: x % 2 == 0, numbers))

# sorted()
words = ['apple', 'pie', 'zoo', 'a']
sorted_by_length = sorted(words, key=lambda x: len(x))

# reduce()
from functools import reduce
sum_all = reduce(lambda x, y: x + y, numbers)
```

---

## Built-in Functions

### Commonly Used Functions
```python
# Type conversion
int('10')
float('3.14')
str(100)
list((1, 2, 3))
tuple([1, 2, 3])
set([1, 2, 2, 3])

# Math
abs(-5)              # 5
round(3.14159, 2)    # 3.14
pow(2, 3)            # 8
max(1, 2, 3)         # 3
min(1, 2, 3)         # 1
sum([1, 2, 3])       # 6

# Sequences
len([1, 2, 3])       # 3
sorted([3, 1, 2])    # [1, 2, 3]
reversed([1, 2, 3])  # Iterator
enumerate(['a', 'b'])  # [(0, 'a'), (1, 'b')]
zip([1, 2], ['a', 'b'])  # [(1, 'a'), (2, 'b')]

# Range
range(5)             # 0, 1, 2, 3, 4
range(1, 6)          # 1, 2, 3, 4, 5
range(0, 10, 2)      # 0, 2, 4, 6, 8

# Boolean
all([True, True])    # True
any([False, True])   # True
bool(0)              # False

# Object
id(obj)              # Object ID
type(obj)            # Object type
isinstance(obj, int) # Type check
hasattr(obj, 'attr') # Has attribute
getattr(obj, 'attr') # Get attribute
setattr(obj, 'attr', value)  # Set attribute

# Input/Output
input("Enter: ")     # Read input
print("Hello")       # Output

# Iteration
iter([1, 2, 3])      # Create iterator
next(iterator)       # Get next item

# File
open('file.txt')     # Open file
```

---

## Best Practices

### PEP 8 Style Guide
```python
# ✅ Good: Use snake_case for variables and functions
user_name = "John"
def calculate_total(items):
    pass

# ✅ Good: Use PascalCase for classes
class UserProfile:
    pass

# ✅ Good: Use UPPER_CASE for constants
MAX_SIZE = 100
API_KEY = "abc123"

# ✅ Good: Proper indentation (4 spaces)
def function():
    if condition:
        do_something()

# ✅ Good: Blank lines
def function1():
    pass

def function2():
    pass

# ✅ Good: Import order
import os
import sys

from datetime import datetime

import numpy as np
```

### Writing Clean Code
```python
# ✅ Good: Descriptive names
def calculate_total_price(items, tax_rate):
    subtotal = sum(item.price for item in items)
    return subtotal * (1 + tax_rate)

# ❌ Bad: Unclear names
def calc(i, t):
    s = sum(x.p for x in i)
    return s * (1 + t)

# ✅ Good: Use list comprehensions
squares = [x ** 2 for x in range(10)]

# ❌ Bad: Traditional loop when not needed
squares = []
for x in range(10):
    squares.append(x ** 2)

# ✅ Good: Use with for files
with open('file.txt', 'r') as f:
    content = f.read()

# ❌ Bad: Manual file closing
f = open('file.txt', 'r')
content = f.read()
f.close()
```

### Error Handling
```python
# ✅ Good: Specific exceptions
try:
    value = int(user_input)
except ValueError:
    print("Invalid number")

# ❌ Bad: Catch all exceptions
try:
    value = int(user_input)
except:
    pass

# ✅ Good: Provide context
if age < 0:
    raise ValueError(f"Age cannot be negative: {age}")

# ✅ Good: Clean up in finally
try:
    file = open('file.txt')
    process_file(file)
finally:
    file.close()
```

### Documentation
```python
def calculate_average(numbers):
    """
    Calculate the average of a list of numbers.
    
    Args:
        numbers (list): A list of numeric values
    
    Returns:
        float: The average of the numbers
    
    Raises:
        ValueError: If the list is empty
    
    Examples:
        >>> calculate_average([1, 2, 3, 4, 5])
        3.0
    """
    if not numbers:
        raise ValueError("List cannot be empty")
    return sum(numbers) / len(numbers)
```

---

**Pro Tip:** Embrace Pythonic idioms like list comprehensions and context managers, follow PEP 8 style guidelines, use type hints for clarity, and leverage the standard library before adding external dependencies!