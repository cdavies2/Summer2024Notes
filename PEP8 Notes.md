# Consistency
Consistency within a project is important, but consistency within one module or function is most important
Despite this, style guides should sometimes be ignored, like....
  1. If applying the guideline makes code less readable (code's read more than written)
  2. To be consistent with historic, surrounding code
  3. Code predates the introduction of the guideline and has no other reason to be changed
  4. Code needs to be compatible with older versions of Python that don't support the  recommended feature from the style guide

# Code Layout
For identation, align wrapped elements using either implicit line joining (which are inside delimiters) or use a hanging indent
Add four spaces (an extra level of identation) to distinguish arguments from the rest
EX: def long_function_name(
        var_one, var_two, var_three,
        var_four):
    print(var_one)
When making an if statement where the condition must be written across multiple lines, there are several options for indentation;
  1. No extra indentation
  if (this_is_one_thing and
    that_is_another_thing):
    do_something()
  2. Add a comment between the condition and the nested code
   if (this_is_one_thing and
    that_is_another_thing):
  #both are true, so this occurs
    do_something()
  3. Add indentation on the later portion of the condition
  if (this_is_one_thing
        and that_is_another_thing):
    do_something()

Closing braces/brackets/parenthesis of multiline constructs can line up in two ways
1. Under the first non white-space character of the last line of the list
my_list = [
    1, 2, 3,
    4, 5, 6,
    ]
2. Under the first character of the line that starts the multiline construct
my_list = [
    1, 2, 3,
    4, 5, 6,
]

Spaces are the preferred indentation method
Python doesn't allow mixing of tabs and spaces for indentation

## Maximum Line Length
Limit lines to a maximum of 79 characters
The best way to wrap long lines is by breaking them over lines wrapped in parenthesis. These are preferred to using a backslash for line continuation

# Should a Line Break Before or After a Binary Operator?
It was previously recommended to break after binary operators, but this results in operators getting scattered and moved away from its operand, making it harder to read. Mathemeticians recommend breaking before binary operators for displated formulas

## Blank Lines
Top-level functions and class definitions are surrounded by two blank lines
Method definitions inside a class are surrounded by a single blank line
Blank lines can be used (sparingly) to separate groups of related functions
Blank lines can be used sparingly in functions to indicate logical sections

#Imports
Imports should usually be on separate lines 
EX:
import os
import sys

This is also ok
EX:
from subprocess import Popen, PIPE

Imports should be at the top of a file and grouped in this order...
1. Standard library imports.
2. Related third party imports.
3. Local application/library specific imports.

Absolute imports are recommended as they are more readable and usually better behaved

Wildcard imports (from <module> import *) should be avoid, as they make it hard to tell which names are present in the namespace


## Module Level Dunder Names
"Dunders" (names with two leading and two trailing underscores like __author__ or __version__ must be before any import statements except from__future__ imports. Future-imports must appear before any other code except dosctrings.

# String Quotes
When a string uses single or double quote characters, use the one not used to create the string variable to avoid backslashes and improve readability.

# Whitespace Pet Peeves
Avoid extra whitsepace immediately inside parenthesis (EX: ham[  1  ]), between a comma and parenthesis (EX: bar=(0, )), and immediately before a comma, semicolon, or colon (EX: if x==4 : print(x , y) ; x , y = y , x)
A colon in a slice should have equal amounts on each side (EX: ham[1:9])
Always surround these binary operators with a single space on either side:
1. Assignment (=,EX: i = i + 1)
2. Augmented assignment(+=, -=)
3. Comparisons (==, <, >, in, is)
4. Booleans (and, or, not)

Compound statements (multiple statements on the same line) are generally discouraged


