# TypeScript Classes & OOP

## 1. OOP in TypeScript

Before the OOP example, it is important to understand:

- `public`
- `private`
- `protected`

---

## 2. `public`

A `public` member can be accessed from:

- inside the class
- subclasses
- outside the class

It is the normal accessible member.

Example:

```ts
class Account {
    public name: string

    constructor(name: string) {
        this.name = name
    }
}

const account = new Account("Tanim")

console.log(account.name)
```

---

## 3. `private`

A `private` member can be accessed only inside the class where it is declared.

It cannot be accessed directly:

- from outside the class
- from a subclass

Example:

```ts
class Account {
    private balance: number

    constructor(balance: number) {
        this.balance = balance
    }
}
```

This is not allowed:

```ts
const account = new Account(1000)

console.log(account.balance)
```

---

## 4. `protected`

A `protected` member can be accessed:

- inside the class
- inside subclasses

But it cannot be accessed directly from outside the class.

Example:

```ts
class Account {
    protected balance: number

    constructor(balance: number) {
        this.balance = balance
    }
}

class StudentAccount extends Account {
    showBalance() {
        console.log(this.balance)
    }
}
```

The subclass can use `this.balance`, but code outside the class hierarchy cannot directly access it.

### Quick comparison

| Modifier | Inside class | Subclass | Outside |
|---|---:|---:|---:|
| `public` | ✅ | ✅ | ✅ |
| `protected` | ✅ | ✅ | ❌ |
| `private` | ✅ | ❌ | ❌ |

---

## 5. TypeScript OOP Example

## 6. `account.ts`

```ts
class Account {
    readonly id: number;
    public name: string;
    protected balance: number;

    constructor(id: number, name: string, balance: number) {
        this.id = id;
        this.name = name;
        this.balance = balance;
    }
}
```

Here:

- `readonly id` cannot be reassigned after initialization.
- `public name` can be accessed from outside.
- `protected balance` can be accessed by the class and its subclasses.

---

## 7. Constructor Parameter Properties

We can make the class shorter using **parameter properties**:

```ts
class Account {
    constructor(
        public readonly id: number,
        public name: string,
        protected balance: number
    ) {}
}
```

TypeScript automatically creates and assigns those properties.

---

## 8. Add Methods to the Account Class

```ts
class Account {
    constructor(
        public readonly id: number,
        public name: string,
        protected balance: number
    ) {}

    deposit(amount: number): void {
        this.balance += amount
    }

    withdraw(amount: number): void {
        if (this.balance < amount) {
            throw new Error("insufficient balance")
        }

        this.balance -= amount
    }

    status(): void {
        console.log(
            `Account ${this.id} (${this.name}) has a balance of ${this.balance}`
        )
    }
}
```

---

## 9. Interface for Different Account Types

An account can have multiple types, and different account types can have different withdrawal limits.

So we can create an interface above the classes.

```ts
interface BaseAccount {
    readonly id: number;
    name: string;

    deposit(amount: number): void;
    withdraw(amount: number): void;
    status(): void;
}
```

### Important note about access modifiers in interfaces

We do not use `public`, `private`, or `protected` inside an interface.

Interfaces describe the public shape of an object.

Also, `balance` is not included in this interface because our `balance` property is protected.

```ts
// balance: number
```

We cannot describe a protected member as a public interface property.

---

## 10. Implement the Interface

```ts
class Account implements BaseAccount {
    protected isLimited: boolean = false
    protected limit: number = 0

    constructor(
        public readonly id: number,
        public name: string,
        protected balance: number
    ) {}

    deposit(amount: number): void {
        this.balance += amount
    }

    withdraw(amount: number): void {
        if (this.isLimited && amount > this.limit) {
            throw new Error(
                `You cannot withdraw more than ${this.limit}`
            )
        }

        if (this.balance < amount) {
            throw new Error("insufficient balance")
        }

        this.balance -= amount
    }

    status(): void {
        console.log(
            `Account ${this.id} (${this.name}) has a balance of ${this.balance}`
        )
    }
}
```

---

## 11. Inheritance

Now create specialized account classes.

```ts
class StudentAccount extends Account {
    protected isLimited: boolean = true
    protected limit: number = 10000
}
```

Savings account:

```ts
class SavingsAccount extends Account {
    protected isLimited: boolean = true
    protected limit: number = 50000
}
```

Current account:

```ts
class CurrentAccount extends Account {}
```

---

## 12. Use the Student Account

```ts
const studentAccount = new StudentAccount(
    153,
    "Student A",
    25000
)

studentAccount.withdraw(5000)
studentAccount.status()

studentAccount.withdraw(15000)
studentAccount.status()
```

The student account has a withdrawal limit of `10000`.

So:

```ts
studentAccount.withdraw(5000)
```

is allowed, while:

```ts
studentAccount.withdraw(15000)
```

will throw an error because it exceeds the limit.
