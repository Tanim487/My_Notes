# Classes & Objects

OOP maps **real-world scenarios** into code using objects.

| Term | Meaning |
|------|---------|
| **Class** | Blueprint for creating objects |
| **Object** | An instance of a class |

---

## Basic Class & Object

```python
class Student:
    name = "karan kumar"    # class attribute

s1 = Student()              # create object (instance)
print(s1.name)
```

---

## `__init__` Constructor

The `__init__()` method runs **automatically** whenever a class is instantiated.
Used to set object attributes.

```python
class Student:
    college = "ABC College"     # class attribute — shared by ALL objects

    def __init__(self, fullname):
        self.name = fullname    # object attribute — unique per object

s1 = Student("karan")
print(s1.name)
```

!!! note "`self`"
    `self` is a reference to the **current instance** of the class. It's how you access variables that belong to the object.

!!! tip "Object attr > Class attr"
    If an object attribute has the same name as a class attribute, the **object attribute takes priority**.
