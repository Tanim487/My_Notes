# Lists

```python
my_list = [1, 2, 3, 4, 5, 6]
print(my_list)
```

!!! success "Lists are Mutable"
    Unlike strings, you **can** change individual elements of a list.

---

## List Slicing

```python
my_list[1:4]    # index 1 to 3
my_list[1:]     # index 1 to end
my_list[:4]     # start to index 3
my_list[-3:-1]  # negative index slicing
```

---

## List Methods

```python
my_list.append(4)           # adds element at the end → [2, 1, 3, 4]
my_list.sort()              # sorts ascending          → [1, 2, 3]
my_list.sort(reverse=True)  # sorts descending         → [3, 2, 1]
my_list.copy()              # returns a copy of the list
my_list.reverse()           # reverses the list        → [3, 1, 2]
my_list.insert(idx, el)     # inserts element at given index
my_list.remove(1)           # removes first occurrence of element
my_list.pop(idx)            # removes element at given index
```
