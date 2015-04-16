# mySidewalk Python Style Guide

This style guide contains up to date best practices when it comes to coding python at mySidewalk. Important context is that these guidelines are meant as an extension to [PEP8](https://www.python.org/dev/peps/pep-0008/), and so, PEP8 and the style items already espoused there will only be mentioned when they are to be a) overridden or b) clarified. Whenever a convention of PEP8 is unmentioned and no convention in the style guide overrides it, it's usage is to be assumed best practice.

## Table of Contents

* Layout
  * File layout
  * Line Length
  * Indentation
  * Class layout
  * Function layout
  * Blank lines
  * Multi-line statements
* Documentation
  * Comments
  * Docstrings
* Naming

## Layout

Layout is critical to a code reader's comprehension and speed. As annotation/"blame"/diff views are line based, it also key to the effectiveness of these functions.

### Indentation

Contrary to PEP8, even hanging indents in a multiline statement should be indented to 4 space breaks.

```
# WRONG
foo = long_function_name(
  var_one, var_two,
  var_three, var_four)
```

```
# RIGHT
foo = long_function_name(
    var_one, var_two,
    var_three, var_four,
)
```

### Blank lines

### Multi-line statements

### File layout

### Class layout

### Function layout

## Documentation

### Comments

### Docstrings

## Naming
