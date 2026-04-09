# Inheritance & Polymorphism

## Inheritance

When a **child/derived class** inherits the properties and methods of a **parent/base class**.

```python
class Car:
    pass

class Toyota(Car):      # Toyota inherits from Car
    pass
```

### Types of Inheritance

- **Single Inheritance** — one child, one parent
- **Multi-level Inheritance** — child → parent → grandparent
- **Multiple Inheritance** — one child, multiple parents

---

## `super()` Method

Used to access methods of the **parent class** from within the child class.

```python
class Animal:
    def __init__(self, name):
        self.name = name

class Dog(Animal):
    def __init__(self, name, breed):
        super().__init__(name)    # calls Animal's __init__
        self.breed = breed
```

---

## Polymorphism: Operator Overloading

The same operator can behave **differently** depending on the types involved.
Python uses **dunder (double underscore) methods** under the hood.

| Expression | Operation | Dunder Method |
|------------|-----------|---------------|
| `a + b` | Addition | `a.__add__(b)` |
| `a - b` | Subtraction | `a.__sub__(b)` |
| `a * b` | Multiplication | `a.__mul__(b)` |
| `a / b` | Division | `a.__truediv__(b)` |
| `a % b` | Modulus | `a.__mod__(b)` |

!!! tip "Learn More"
    Visit [Python Data Model — Section 3.3.8](https://docs.python.org/3/reference/datamodel.html) for the full list of dunder methods.
