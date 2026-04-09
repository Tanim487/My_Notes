# Functions & Recursion

## Defining a Function

```python
def func_name(param1, param2):
    # some work
    return val

func_name(arg1, arg2)   # function call
```

---

## Built-in Functions

```python
print()    # prints output
len()      # returns length
type()     # returns data type
range()    # generates a range of numbers
```

!!! tip
    In VS Code, hover over any built-in function to see its definition and parameters.

---

## Recursion

Recursion is when a function **calls itself** repeatedly.

!!! danger "Always have a Base Case"
    Without a base case, the function will call itself forever and crash.

```python
# prints n down to 1
def show(n):
    if n == 0:       # base case — stops the recursion
        return
    print(n)
    show(n - 1)      # recursive call
```
