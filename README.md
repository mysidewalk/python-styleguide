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
# WRONG
def use_a_global_defined_later():
    print some_global

some_global = 'Hello, world!'


class Foo(object):
    
    def bar(self):
        print self._a_foo_says
   
    _a_foo_says = 'What?'
```

```python
# RIGHT
some_global = 'Hello, world!'

def use_a_global_defined_later():
    print some_global


class Foo(object):
    _a_foo_says = 'What?'
    
    def bar(self):
        print self._a_foo_says
```

### Line Length

Contrary to PEP8, lines can be up to 120 characters. Modern software engineering equipment and tools support substantially wider views than 79 characters, however, readability is still enhanced by making code fairly narrow and lines that are made long thanks to many parameter function calls or, multiple function calls, or the instantiation of primitives with many members (i.e. a long list) should be broken up even before they reach the 120 character limit. See multi-line statements for more information.

### Indentation

Contrary to PEP8, even hanging indents in a multiline statement should be indented to 4 space breaks.

```python
# WRONG
foo = long_function_name(
  var_one, var_two,
  var_three, var_four)
```

```python
# RIGHT
foo = long_function_name(
    var_one, var_two,
    var_three, var_four,
)
```

Code that is deeply indented (more than 12-16 spaces) is difficult 

### Blank lines

### Multi-line statements

### Class layout

### Function layout

## Documentation

### Comments

### Docstrings

## Naming
