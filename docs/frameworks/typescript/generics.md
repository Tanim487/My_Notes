# TypeScript Generics

## 1. Generic

Generic is a powerful TypeScript feature that lets us work with a **dynamic type**.

Suppose we have:

```ts
const fruits: string[] = [
    "apple",
    "banana",
    "cherry",
    "date",
    "elderberry"
]

const numbers: number[] = [1, 2, 3, 4, 5]
```

If we write:

```ts
function getFirstItem(items: string[]): string {
    return items[0]
}
```

The function is fixed to `string`.

It accepts strings and returns a string.

If we change it to `number`, then it would work only with numbers.

But we want a way where the return type is dynamic.

In other words, if we pass strings, we want a string back. If we pass numbers, we want a number back.

### Generic solution

```ts
const fruits: string[] = [
    "apple",
    "banana",
    "cherry",
    "date",
    "elderberry"
]

const numbers: number[] = [1, 2, 3, 4, 5]

function getFirstItem<T>(items: T[]): T {
    return items[0]
}

const firstFruit = getFirstItem(fruits)
const firstNumber = getFirstItem(numbers)

console.log(firstFruit.toUpperCase())
console.log(firstNumber.toFixed(2))
```

Here we do not use a fixed data type.

`T` represents the type that is passed to the function.

The `<T>` portion identifies the generic type, and TypeScript automatically determines what `T` should be from the argument.

So:

```ts
getFirstItem(fruits)
```

makes `T` become `string`.

And:

```ts
getFirstItem(numbers)
```

makes `T` become `number`.

---

## 2. Generic with API Fetching

Let's see how we can use generics with APIs.

First, define our types:

```ts
type User = {
    id: number
    name: string
    email: string
}

type Product = {
    id: number
    name: string
    price: number
}

type Order = {
    id: number
    userId: number
    total: number,
    date: string,
    status: "pending" | "shipped" | "delivered"
}
```

Example API endpoints:

```text
/api/users
/api/products
/api/orders
```

=== "Normal TypeScript approach"

    ```ts
    async function getUsers(): Promise<User[]> {
        const data = await fetch("/api/users")
        return data.json()
    }

    async function getProducts(): Promise<Product[]> {
        const data = await fetch("/api/products")
        return data.json()
    }

    async function getOrders(): Promise<Order[]> {
        const data = await fetch("/api/orders")
        return data.json()
    }

    const users = await getUsers()
    const products = await getProducts()
    const orders = await getOrders()
    ```

    Here we need three different functions for three different types of API data.

    We can make the code simpler and shorter using a generic.

=== "Better: Generic API function"

    ```ts
    async function get<T>(apiEndpoint: string): Promise<T> {
        const data = await fetch(apiEndpoint)
        return data.json()
    }

    const users = await get<User[]>("/api/users")
    const products = await get<Product[]>("/api/products")
    const orders = await get<Order[]>("/api/orders")
    ```

    This single function works for both a single value and an array, depending on the generic type we provide.

> **Important:** Read more about generics because they are very important in TypeScript.
