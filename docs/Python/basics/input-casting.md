# Input & Type Casting

## Input

```python
input("Enter here: ")   # the string is a prompt/placeholder
```

!!! warning "Always a String"
    The result of `input()` is **always a string**, even if the user types a number.

---

## Taking Numeric Input

Cast the result to get a number:

```python
int(input("Enter a number: "))
float(input("Enter a decimal: "))
```

---

## Type Conversion vs Casting

| Term | Meaning |
|------|---------|
| **Conversion** | Happens **automatically** in Python (implicit) |
| **Casting** | You **manually** change the type (explicit) |

---

## Casting Examples

```python
int(val)           # cast to integer
float(val)         # cast to float

# Cast dictionary keys to a list
print(list(student.keys()))
```
