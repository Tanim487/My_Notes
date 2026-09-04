# TypeScript Utility Types

## 1. Omit Type

TypeScript has many built-in utility types.

Let's first see how we would solve a problem normally.

```ts
type User = {
    id: number
    name: string
    email: string
}

const users: User[] = []

let lastId: number = 0

function addUser(name: string, email: string): User {
    const user: User = {
        id: ++lastId,
        name,
        email,
    }

    users.push(user)
    return user
}
```

Here, we want to pass only `name` and `email`. We do **not** want to pass `id`, because `id` should be generated automatically.

```ts
addUser("Alice", "alice@example.com")
addUser("Bob", "bob@example.com")

console.log(users)
```

We can solve this using TypeScript's built-in `Omit` utility type.

```ts
type User = {
    id: number
    name: string
    email: string
}

const users: User[] = []

let lastId: number = 0

function addUser(user: Omit<User, "id">): User {
    const newUser: User = {
        ...user,
        id: ++lastId
    }

    users.push(newUser)
    return newUser
}
```

`Omit<User, "id">` accepts all the properties of `User` except `id`.

The `...user` part is JavaScript's built-in **spread operator**.

Because `user` is an object, we pass the properties as key-value pairs:

```ts
addUser({
    name: "Alice",
    email: "alice@example.com"
})

addUser({
    name: "Bob",
    email: "bob@example.com"
})

console.log(users)
```

---

## 2. Optional Properties

We can solve the same problem in another way by making the `id` property optional.

```ts
type User = {
    id?: number
    name: string
    email: string
}

const users: User[] = []

let lastId: number = 0

function addUser(user: User): User {
    const newUser: User = {
        id: ++lastId,
        ...user
    }

    users.push(newUser)
    return newUser
}

addUser({
    name: "Alice",
    email: "alice@example.com"
})

addUser({
    name: "Bob",
    email: "bob@example.com"
})

console.log(users)
```

Adding `?` after a property name makes it an **optional property**.

That means the property can be provided, but it does not have to be provided.

TypeScript has many other interesting built-in utility types. Study them for better knowledge.
