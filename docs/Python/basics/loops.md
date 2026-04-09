# Loops

## While Loop

```python
while condition:
    # do something
```

---

## For Loop with `range()`

```python
for i in range(10):          # range(stop)          → 0 to 9
for i in range(2, 10):       # range(start, stop)   → 2 to 9
for i in range(2, 10, 2):    # range(start, stop, step) → 2,4,6,8
for i in range(100, 0, -1):  # going backwards
```

!!! warning
    `range(100, 1)` does **nothing** — you must provide a negative step to go backwards:
    ```python
    range(100, 0, -1)   # ✅ correct
    ```

---

## For Loop over a List

```python
for x in my_list:
    # do something
```

---

## Loop Control Statements

| Statement | Behavior |
|-----------|----------|
| `break` | Exits the loop immediately |
| `continue` | Skips current iteration, moves to next |
| `pass` | Does nothing — placeholder for future code |

### `pass` Example

`pass` is a null statement. Used when Python requires a statement syntactically but you have nothing to write yet.

```python
for x in range(10):
    pass    # placeholder — do nothing for now
```
