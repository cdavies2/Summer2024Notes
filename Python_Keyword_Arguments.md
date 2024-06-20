# Keyword Arguments
* In Python, parameters are the input variables bounded by parentheses when defining a function
* Arguments, however, are the values assigned to parameters when passed into a function

## Types of Arguments
### Positional Arguments
* Positional arguments are values that are passed into a function based on the order in which the parameters were listed
  ```
def team(name, project):
  print(name, "is working on an", project)

team("FemCode", "Answers")
  ```
* The above code will return "FemCode is working on an Answers". If you wrote
```
team("Answers", "FemCode")
```
* Then the output would be "Answers is working on an Femcode". The order matters
