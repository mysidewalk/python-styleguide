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
def _not_authenticated(self):
    self._authenticator = None
    tracking_session = TrackingSessionHandler(self)
    self._user = tracking_session.user

    if api_settings.UNAUTHENTICATED_TOKEN:
        self._auth = api_settings.UNAUTHENTICATED_TOKEN()
    else:
        self._auth = None
        
 # BETTER
 def _not_authenticated(self):
    self._authenticator = None
    tracking_session = TrackingSessionHandler(self)
    self._user = tracking_session.user
    # Handle based on api_settings
    if api_settings.UNAUTHENTICATED_TOKEN:
        self._auth = api_settings.UNAUTHENTICATED_TOKEN()
    else:
        self._auth = None

# BEST
def _handle_unauthenticated_setting(self):
    if api_settings.UNAUTHENTICATED_TOKEN:
        self._auth = api_settings.UNAUTHENTICATED_TOKEN()
    else:
        self._auth = None

def _not_authenticated(self):
    self._authenticator = None
    tracking_session = TrackingSessionHandler(self)
    self._user = tracking_session.user
    self._handle_unauthenticated_setting()
```

### Multi-line statements

### Class layout

### Function layout

## Documentation

### Comments

### Docstrings

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
