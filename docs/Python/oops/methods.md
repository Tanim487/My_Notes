# Methods

Methods are **functions that belong to objects**.

---

## Instance Methods

Use `self` to access the object's attributes.

```python
class Student:
    def __init__(self, fullname):
        self.name = fullname

    def hello(self):
        print("hello", self.name)

s1 = Student("karan")
s1.hello()
```

---

## Static Methods

Methods that **don't use `self`** — work at the class level, not the instance level.

```python
class Student:
    @staticmethod           # decorator
    def college():
        print("ABC College")
```

!!! info "Decorators"
    Decorators **wrap a function** to extend its behavior without permanently modifying it.

---

## Class Methods

Bound to the **class itself**, not to an instance. Receives `cls` (the class) as the first argument.

```python
class Person:
    name = "anonymous"

    @classmethod
    def changeName(cls, name):
        cls.name = name

p1 = Person()
p1.changeName("rahul kumar")
print(p1.name)      # "rahul kumar"
print(Person.name)  # "rahul kumar"
```

---

## Summary: 3 Types of Methods

| Type | Decorator | First Parameter | Access Instance? | Access Class? |
|------|-----------|-----------------|-----------------|---------------|
| Instance method | — | `self` | ✅ | ✅ |
| Class method | `@classmethod` | `cls` | ❌ | ✅ |
| Static method | `@staticmethod` | — | ❌ | ❌ |

---

## `del` Keyword

```python
del s1.name     # deletes a specific attribute
del s1          # deletes the entire object
```

---

## Property Decorator

Use `@property` to access a method **like an attribute** (getter).

```python
class Student:
    def __init__(self, name):
        self.__name = name

    @property
    def name(self):
        return self.__name

s1 = Student("karan")
print(s1.name)      # accessed like an attribute, not s1.name()
```

Also available: `@name.setter` and `@name.deleter` decorators.
