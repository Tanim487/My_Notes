# Dictionaries

Dictionaries are **unordered**, **mutable**, and **do not allow duplicate keys**.
They store data as `key: value` pairs. Values can be of **any type**.

---

## Creating a Dictionary

```python
info = {
    "name": "apnaCollege",
    "subjects": ["python", "C", "Java"],    # list as value
    "topics": ("dict", "set"),              # tuple as value
    "age": 35,
    "is_adult": True,
    "marks": {                              # nested dictionary
        "phy": 97,
        "chem": 98,
        "math": 95
    }
}
```

---

## Accessing & Modifying

```python
print(info["name"])              # access a value
print(info["marks"]["chem"])     # nested access

info["age"] = 40                 # change existing value
info["phone"] = "121121331"      # add new key-value pair
```

---

## Dictionary Methods

```python
myDict.keys()              # returns all keys
myDict.values()            # returns all values
myDict.items()             # returns all (key, value) pairs as tuples
myDict.get("key")          # returns value for given key
myDict.update(newDict)     # inserts all items from another dictionary
```
