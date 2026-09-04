# TypeScript Advanced Types

## 1. Literal Types

Earlier, when we saw `Enum`, we used it to restrict values to a fixed set.

We can achieve a similar result using literal types.

The enum version would be conceptually like:

```ts
// enum Colors {
//     "Red" = "red",
//     "Green" = "green",
//     "Blue" = "blue"
// }
```

With literal types:

```ts
type Colors = "red" | "green" | "blue"

const c: Colors = "red"
```

This is useful in very common code.

For example:

```ts
type Methods = "GET" | "POST" | "DELETE"

const m: Methods = "PUT"
```

The last line will show an error because `"PUT"` is not one of the allowed values.
