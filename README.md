# mySidewalk Python Style Guide

This style guide contains up to date best practices when it comes to coding python at mySidewalk. Important context is that these guidelines are meant as an extension to [PEP8](https://www.python.org/dev/peps/pep-0008/), and so, PEP8 and the style items already espoused there will only be mentioned when they are to be a) overridden or b) clarified. Whenever a convention of PEP8 is unmentioned and no convention in the style guide overrides it, it's usage is to be assumed best practice.

## Table of Contents

* Layout
  * File layout
  * Line Length
  * Indentation
  * Blank lines
  * Multi-line statements
* Documentation
  * Comments
  * Docstrings
* Naming
* Control flow statements
* Functions
* Classes
* Patterns

## Layout

Layout is critical to a code reader's comprehension and speed. As annotation/"blame"/diff views are line based, it also key to the effectiveness of these functions.

### File layout

In keeping with PEP8, python files should be headed by documentation, followed by import statements, any relevant __all__ declaration, and followed by the declarations that will make up the module itself. As a general concept, modules should be written with an assumption that they will be read from top to bottom. This means that symbols referenced in later sections of the file should have been defined previously.

```python
# BAD
def use_a_global_defined_later():
    print some_global

some_global = 'Hello, world!'


class Foo(object):
    
    def bar(self):
        print self._a_foo_says
   
    _a_foo_says = 'What?'

# GOOD
some_global = 'Hello, world!'

def use_a_global_defined_later():
    print some_global


class Foo(object):
    _a_foo_says = 'What?'
    
    def bar(self):
        print self._a_foo_says
```

"Shebang" lines are unnecessary and can even cause issues in practice (when there are multiple python interpreters or versions, say, cpython and pypy, on one server, it is important to be explicit with which interpreter is launching the script). Do not include a "shebang".

```python
#! /usr/bin/env python
# BAD
```

### Line Length

Contrary to PEP8, lines can be up to 120 characters. Modern software engineering equipment and tools support substantially wider views than 79 characters, however, readability is still enhanced by making code fairly narrow and lines that are made long thanks to many parameter function calls or, multiple function calls, or the instantiation of primitives with many members (i.e. a long list) should be broken up even before they reach the 120 character limit. See multi-line statements for more information.

### Indentation

Contrary to PEP8, even hanging indents in a multiline statement should be indented to 4 space breaks.

```python
# BAD
foo = long_function_name(
  var_one, var_two,
  var_three, var_four)

# BETTER
foo = long_function_name(
    var_one, var_two,
    var_three, var_four,
)

# BEST
foo = long_function_name(
    var_one,
    var_two,
    var_three,
    var_four,
)
```

Code that is deeply indented (more than 12-16 spaces) is difficult to read, forces skinnier lines, incentivizes naming shortcuts and abbreviations, and likely indicates too much complexity being packed into too small a space (a complex orchestration of primitives, too large and cyclomatically complex a method, etc.). A number of methods can be used to outdent code suffering from this readability issue.

```python
# BAD
some_complicated_structure = {
    'some inner complicated structure': {
        'why not another inner complicated structure': {
            'getting past the indent equator, here; toilets flush in a reversed direction': {
                "let's throw another shrimp on the barbie!": {
                    'maybe a dingo ate your baby': {
                        'marsupials': [
                            'kangaroo',
                            'wallabie',
                            'possum',
                            'wombat',
                            'tasmanian devil',
                        ],
                    },
                },
            },
        },
    },
}

# BETTER
dingo_nonsense = {
    'maybe a dingo ate your baby': {
        'marsupials': [
            'kangaroo',
            'wallabie',
            'possum',
            'wombat',
            'tasmanian devil',
        ],
    },
}
another_inner_structure = {
    'why not another inner complicated structure': {
        'getting past the indent equator, here; toilets flush in a reversed direction': {
            "let's throw another shrimp on the barbie!": dingo_nonsense,
        },
    },
}
some_complicated_structure = {
    'some inner complicated structure': another_inner_structure,
}

class ATaleOfTwoFunctions(object):
    # BAD
    def worst_of_lines(self):
        for item in range(100):
            if item % 2:
                for inner_item in range(item):
                    print item
            elif item % 3:
                while 1:
                    sleep(1)
                    break
            else:
                print 'charcuterie.'
    
    # BETTER
    def process_tripe(self, triple):
        while 1:
            sleep(triple)
            break
    
    def process_double(self, double):
        for inner_item in range(double):
            print inner_item
    
    def process_item(self, item):
        if item % 2:
            self.process_double(item)
        elif item % 3:
            self.process_triple(item)
        else:
            print 'charcuterie.'
    
    def best_of_lines(self):
        for item in range(100):
            self.process_item(item)
```

### Blank lines

Contrary to PEP8 which states, `Use blank lines in functions, sparingly, to indicate logical sections.`, comments are preferred. The reasoning being that any logical grouping within a function that might benefit from a blank line will benefit even more from the addition of a comment to describe the logical section. A further improvement would be to factor out the logical section as its own method. The end result is that methods are much easier to identify and group as they form a solid block of code.

```python
# BAD
def spacey_method(self):
    self.kevin = Kevin()
    self.white_spacey = WhiteSpacey()

    if some_conditional():
        self.role = 'Frank'
    else:
        self.role = 'Keyser'
        
 # BETTER
def less_spacey_method(self):
    self.kevin = Kevin()
    self.white_spacey = WhiteSpacey()
    # Handle based on some conditional
    if some_conditional():
        self.role = 'Frank'
    else:
        self.role = 'Keyser'

# BEST
def handle_some_condition(self):
    if some_conditional():
        self.role = 'Frank'
    else:
        self.role = 'Keyser'

def split_spacey_method(self):
    self.kevin = Kevin()
    self.white_spacey = WhiteSpacey()
    self.handle_some_condition()
```

### Multi-line statements

Implicit line continuation is preferred over the use of backslashes. Further, if the statement can be/is scoped (has inner blocks surrounded by ```[, {, (```), it is preferred to put the elements of this scope on separate lines and indent them. Further, whenever possible, elements within a scope that have been broken out onto new lines should have a trailing comma (after the last element) to minimize the diff if a new element is added or removed to or from the end.

```python
# BAD
so_many_tuples = ((1, 2, 3, 4, 5, 6), (7, 8, 9, 10, 11), (12, 13, 14, 15), (16, 17, 18), (19, 20), (21))

# PEP8 COMPLIANT, BUT NOT GREAT
so_many_typles = ((1, 2, 3, 4, 5, 6), (7, 8, 9, 10, 11),
                  (12, 13, 14, 15), (16, 17, 18), (19, 20), (21))

# BEST
so_many_tuples = (
    (1, 2, 3, 4, 5, 6),
    (7, 8, 9, 10, 11),
    (12, 13, 14, 15),
    (16, 17, 18),
    (19, 20),
    (21), # trailing comma reduces diff if a new element is added
)

# BAD
some_string = 'concatenate' + 'a' + ' ' + 'lot' + ' ' + 'of' + 'strings' + 'together' +\
              ' ' + 'for' + ' ' + 'fun.'

# BETTER
some_string = (
    'concatenate' + 'a' + ' ' + 'lot' + ' ' + 'of' +
    'strings' + 'together' +' ' + 'for' + ' ' + 'fun.'
)
```

List comprehensions, and, more generally, generator expressions, can benefit a great deal in readability and maintainability from being split across multiple lines. A generator expression generally has 4 parts:

- The "transformation" (any logic applied to create each output element)
- The "for expression" (defining the elements that will come from the iterable)
- The "in expression" (defining the source iterable)
- The "if expression" (optional, defines a predicate for inclusion/exclusion for the elements)

For all but the simplest generator expressions, it is helpful to split the constituent elements across lines. Further, putting a generator expression on multiple lines allows the author to add helpful comments before elements of the expression.

```python
# HARD TO READ AND OVER LINE LENGTH
my_list = [(x % 3 == 0 and x % 5 == 0 and 'FizzBuzz') or (x % 5 == 0 and 'Buzz') or (x % 3 == 0 and 'Fizz') or x for x in range(100)]

# BETTER
my_list = [
    (x % 3 == 0 and x % 5 == 0 and 'FizzBuzz')
        or (x % 5 == 0 and 'Buzz')
        or (x % 3 == 0 and 'Fizz')
        or x
    for x
    in range(100)
]

# BEST
my_list = [
    # If divisible by 3 and 5, fizzbuzz
    (x % 3 == 0 and x % 5 == 0 and 'FizzBuzz')
        # If divisible by 5, buzz
        or (x % 5 == 0 and 'Buzz')
        # If divisible by 3, fizz
        or (x % 3 == 0 and 'Fizz')
        # Else the number
        or x
    # Take each integer element
    for x
    # In range 0-99
    in range(100)
]
```

For function and class signatures over the line length, prefer to split arguments with newlines and indentations. The closing `):` should be aligned with the opening `def`/`class` and on a line alone. This helps to avoid going over the line length, while still maintaining reasonable visability of the new scope.

```python
# BAD
def has_many_long_arguments(some_argument, another_argument, and_one_more_positional_one, keyword_argument='Long default value'):
    pass
   
class MyVeryComplicatedSubclass(SuperCoolFeatureMixin, KindaCoolFeatureMixin, NotReallyCoolButNeededMixin, BehemothBaseclass):
    pass

# BEST
def has_many_long_arguments(
    some_argument,
    another_argument,
    and_one_more_positional_one,
    keyword_argument='Long default value',
):
    pass
   
class MyVeryComplicatedSubclass(
    SuperCoolFeatureMixin,
    KindaCoolFeatureMixin,
    NotReallyCoolButNeededMixin,
    BehemothBaseclass,
):
    pass
```

### Import Order

Imports should be ordered into four groups:
* python standard library imports
* 3rd party imports
* sidewalk-libs imports
* imports from within the same repo

Each group should list `import' statements first (sorted alphabetically), followed by 'import-from' statements (also sorted alphabetically).

for example:
```
import logging
from datetime import datetime, timedelta
from urllib import urlencode

import requests
import simplejson as json
from django.conf import settings
from rest_framework import status

from libs.iterable_utilities import first

from routes import AUTHWALK_OAUTH_API_V1, AUTHWALK_USER_API_V1
```

### Class layout

### Function layout

## Documentation

### Comments

### Docstrings

```
""" This is a single-line docstring
"""

""" This is a multi-line docstring
    This is the second line
"""
```

## Naming

In addition to PEP8 naming conventions, names should clearly describe the entity in question (classes and references) or the action being performed (function and method names). Precision and readability are be favored over brevity. Names should not include abbreviations that make them less understandable (e.g. addr_cnty_name), although abbreviations in common use are acceptable (e.g. weight_in_kg). If an entity fits a named pattern or has a specific purpose, that should be conveyed in the name (e.g. GoogleMap < GoogleMapAdapter and hadoop < hadoop_reader)

Specific descriptions should be favored over more general ones. Choose names like "composite", "parser", "driver", "factory" or "adapter" over "handler", "processor", "helper" or "utils". If a reference, class, module or package can't be renamed to be more specific, it's an indication that it should be decomposed into more cohesive entities.  

More examples:

* o < org < organization
* convert() < serialize() < serialize_to_json()
* validate() < vaidated() < is_valid()   (where all return boolean)
* HamburgerHelper < HamburgerCondimentFactory
* ftp_file_handler < ftp_file_driver
* hadoop < hadoop_processor < hadoop_reader < hadoop_stream_reader
* ArgCtxProcessor < ArgumentContextProcessor < ArgumentContextParser + ArgumentContext + ArgumentContextSerializer
* utils.py < transforms.py + validators.py + ... 

## Functions

## Classes

## Patterns
