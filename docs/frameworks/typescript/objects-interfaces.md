# TypeScript Objects & Interfaces

## 1. Objects

Let's go to objects.

## 2. Using `type`

```ts
type User = {
    id: number,
    name: string
}

const person: User = {
    id: 16,
    name: "hasin"
}

const person2: User = {
    id: 18,
    name: "jane"
}
```

## 3. Using `interface`

We can use an `interface` too. It works similarly to `type` for this kind of object definition.

With an interface, we do not use the `=` sign.

```ts
interface User {
    id: number,
    name: string
}

const person: User = {
    id: 16,
    name: "hasin"
}

const person2: User = {
    id: 18,
    name: "jane"
}
```

---

## 4. A More Complex Interface Example

```ts
interface AppConfig {
    appName: string;
    version: string; // "1.6.0"
    debug: boolean;
    port: number;

    database: {
        host: string;
        port: number;
        username: string;
        password: string;
    };

    features: {
        registration: boolean;
        darkMode: boolean;
    };
}

const config: AppConfig = {}
```

Here, `database` and `features` are nested objects.

We can also create separate interfaces for them. That can make the code cleaner and easier to understand.

```ts
interface Database {
    host: string;
    port: number;
    username: string;
    password: string;
}

interface AppConfig {
    appName: string;
    version: string; // "1.6.0"
    debug: boolean;
    port: number;
    database: Database;

    features: {
        registration: boolean;
        darkMode: boolean;
    };
}
```

In the interface above, we could also use a `type` instead of an `interface`.

### Semicolon vs comma

Here we use `;`:

```ts
interface Database {
    host: string;
    port: number;
}
```

We could also use commas. Both are valid, but semicolons are preferable in this style.

### `type` vs `interface`

For many object-shape definitions, we can use either `type` or `interface`.
