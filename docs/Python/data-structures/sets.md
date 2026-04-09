# Sets

A set is a collection of **unordered**, **unique** elements.

| Property | Detail |
|----------|--------|
| Elements | Must be **immutable** (int, float, string, bool, tuple ✅) |
| Duplicates | **Not allowed** — automatically removed |
| Lists/Dicts inside | ❌ Not allowed (they are mutable) |
| The set itself | ✅ Mutable — you can add/remove elements |

---

## Creating a Set

```python
nums = {1, 2, 3, 4}
set2 = {1, 2, 2, 2}    # stored as {1, 2} — duplicates removed automatically
```

---

## Empty Set

!!! warning "Don't use `{}` for an empty set!"
    ```python
    null_set = set()   # ✅ correct — creates an empty set
    empty = {}         # ❌ wrong  — this creates an empty DICTIONARY
    ```

---

## Set Methods

```python
my_set.add(el)              # adds an element
my_set.remove(el)           # removes the element
my_set.clear()              # empties the set
my_set.pop()                # removes a random element

my_set.union(set2)          # combines both sets, returns new set
my_set.intersection(set2)   # returns only common elements in both sets
```
