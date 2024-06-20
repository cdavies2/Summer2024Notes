# Keyword Arguments
* In Python, parameters are the input variables bounded by parentheses when defining a function
* Arguments, however, are the values assigned to parameters when passed into a function

## Types of Arguments
### Positional Arguments
* Positional arguments are values that are passed into a function based on the order in which the parameters were listed
```
def team(name, project):
  print(name, "is working on ", project)

team("Casey", "Fibonacci Numbers")
```
* The above code will return "Casey is working on Fibonacci Numbers". If you wrote
```
team("Fibonacci Numbers", "Casey")
```
* Then the output would be "Fibonacci Numbers is working on Casey". The order matters.

### Keyword Arguments
* Keyword Arguments, or named arguments, are values that, when passed into a function, are identifiable by speific parameter names.
* A keyword argument is preceded by a parameter and the assignment operator, =.
```
def team(name, project):
    print(name, "is working on an", project)
team(project = "Fibonacci Numbers", name = 'Casey')
```
```
def team(name, project):
    print(name, "is working on an", project)
team(name = "Casey", project = 'Fibonacci Numbers')
```
* Here, despite the arguments in the code having different positions, the output is the same. With keyword arguments, as long as you assign a value to the parameter, the positions of the arguments do not matter.
* Keyword arguments have to come after positional arguments and before default/optional arguments in a function call
* Default arguments are keyword arguments whose values are assigned at the time of function definition.
* Optional arguments are default arguments that, based on the values assigned to them (EX: none or 0) can be disregarded.
```
def team(name, project, members=None):
    team.name= name
    team.project= project
    team.members= members
    print(name, "is working on an", project)
team("Casey", project = "Fibonacci Numbers")
```

## Fixed Arguments vs. Arbitrary Arguments
* With Fixed function arguments, the number of arguments passed into functions are limited to a specific quantity because we know the number of arguments needed in advance.
* Python allows for arbitrary arguments (or variable-length arguments), where parameters are not specific by distinct individual names, but rather a general term to better encapsulate the shared attribute for the type of arguments being passed
  * *args is used for non-keyworded/positional arguments
  * **kwargs is used for keyworded arguments
* Example for *args
```
def team(*members):
    for member in members:
        print(member)
team("Danielle", "Casey", "Dakota)
```
* Example for **kwargs
```
def team(**details):
    print(type(details))
    for key,value in details.items():
        print("{}: {}".format(key,value))
team(Name = "Casey", Name2= "Danielle", Project = "Fibonacci", Number = "Two Members")
```


