# Encapsulation

**Encapsulation** means wrapping data and functions into a single unit (object), and controlling access to them.

---

## Private Attributes & Methods

Use a `__` (double underscore) prefix to make attributes or methods **private** — accessible only within the class.

```python
class Account:
    def __init__(self, acc_no, acc_pass):
        self.acc_no = acc_no
        self.__acc_pass = acc_pass      # private attribute

    def __sayHello(self):               # private method
        print("hello")

    def welcome(self):
        self.__sayHello()               # ✅ can call private method inside class
```

---

## Access Rules

```python
acc1 = Account("12345", "abcde")

print(acc1.acc_no)          # ✅ works — public attribute
print(acc1.__acc_pass)      # ❌ AttributeError — private, can't access outside
acc1.__sayHello()           # ❌ AttributeError — private, can't call outside
acc1.welcome()              # ✅ works — public method that calls private method internally
```

---

## The 4 Pillars of OOP

| Pillar | Description |
|--------|-------------|
| **Abstraction** | Hide implementation details — show only essential features to the user |
| **Encapsulation** | Wrap data and functions into a single unit (object) |
| **Inheritance** | A child class derives properties & methods from a parent class |
| **Polymorphism** | Same operator/function behaves differently based on context |
