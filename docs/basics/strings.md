# Strings

Strings can be defined with single, double, or triple quotes:

```python
'hello'
"hello"
"""hello"""
```

!!! info "Strings are Immutable"
    You **cannot** change individual characters of a string in place.

---

## Escape Sequences

```python
\n    # newline
\t    # tab
```

---

## Built-in Function

```python
len(str)    # returns the length of the string
```

---

## String Slicing

```python
str = "apnaCollege"
str[1:4]      # "pna"  →  index 1 to 3 (end index is exclusive)
```

### Negative Index Slicing

```
a    p    n    a    C    o    l    l    e    g    e
-11  -10  -9  -8   -7   -6   -5   -4   -3   -2   -1
```

```python
str[-3:-1]    # "eg"
```

---

## String Methods

```python
str.endswith('x')       # True if string ends with 'x'
str.capitalize()        # Capitalizes first character
str.replace(old, new)   # Replaces all occurrences of old with new
str.find(word)          # Returns index of first occurrence
str.count('am')         # Counts occurrences of substring
```
