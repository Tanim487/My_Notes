# Tuples

```python
tup = (87, 64, 33, 95, 76)
```

!!! warning "Tuples are Immutable"
    You **cannot** change elements after creation.
    ```python
    tup[0] = 43    # ❌ TypeError — not allowed
    ```

---

## Creating Tuples

```python
tup1 = ()           # empty tuple
tup2 = (1,)         # single-element — the trailing comma is REQUIRED
                    # without it, Python treats (1) as just the integer 1
tup3 = (1, 2, 3)    # normal tuple
```

---

## Tuple Methods

```python
tup.index(el)   # returns index of first occurrence of el
tup.count(el)   # counts total occurrences of el
```
