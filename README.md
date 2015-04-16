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
    def process_item(self, item):
        if item % 2:
            for inner_item in range(item):
                print item
        elif item % 3:
            while 1:
                sleep(1)
                break
        else:
            print 'charcuterie.'
    
    def best_of_lines(self):
        for item in range(100):
            self.process_item(item)
```

### Blank lines

### Multi-line statements

### Class layout

### Function layout

## Documentation

### Comments

### Docstrings

## Naming

## Functions

## Classes
