# TypeScript Types

## 1. Basic Types

```ts
const country: string = "Bangladesh"
const n: number = 16

const status: boolean = true
```

If we write:

```ts
let x
```

it will be `undefined`.

We can see this by writing:

```ts
console.log(typeof x)
```

### Array type

```ts
const countries: string[] = [
    "bangladesh",
    "india",
    "US",
    "UK"
]
```

---

## 2. Arrays

```ts
const stdName: string[] = []

stdName.push("tanim")
stdName.push("manim")
stdName.push("kanim")
```

Another example:

```ts
const marks: number[] = [2, 1]

marks.push(7)
```

---

## 3. Tuple

A tuple lets us describe an array with a fixed number and order of element types.

```ts
type Point = [number, number]

const location1: Point = [12, 34]

const dhakaLocation: Point = [23.7330, 90.40]

console.log(dhakaLocation)
console.log(dhakaLocation[0])
```

Another example:

```ts
type Player = [string, string, number]

const players: Player[] = [
    ["ronaldo", "portugal", 7],
    ["messi", "argentina", 10]
]
```

Another example:

```ts
type OrderItem = [number, number]

const item: OrderItem = [7, 16]

item.push(20)

console.log(item)
```

### Interesting tuple problem

While defining `OrderItem`, we said it contains two values:

```ts
type OrderItem = [number, number]
```

But JavaScript arrays are still mutable, so `push()` can add another value:

```ts
item.push(20)
```

This can be surprising because the tuple was intended to represent exactly two values.

For this kind of situation, we can use `readonly`.

```ts
type OrderItem = readonly [number, number]

const item: OrderItem = [7, 16]

item.push(20)
```

Now the last line will show an error because we are trying to modify a readonly tuple.

---

Another Tuple Example

```ts
type Division = [number, number]

function divide(a: number, b: number): Division {
    const quotient = Math.floor(a / b)
    const remainder = a % b

    return [quotient, remainder]
}

const result: Division = divide(7, 2)

console.log(result)
```

---

## 4. Enum

Suppose we have a T-shirt product with different sizes and colors.

```ts
type TeeShirt = {
    size: number,
    color: string
}

const t1: TeeShirt = {
    size: 42,
    color: "red"
}

const t2: TeeShirt = {
    size: 38,
    color: "yellow"
}

const t3: TeeShirt = {
    size: 40,
    color: "green"
}
```

What if the product never had a yellow color, or yellow is currently unavailable? But, we still accidentally wrote `yellow` in the code.

If we want to prevent this, we can use an **enum**. 

```ts
enum TColors {
    Red = "red",
    Green = "green",
}

type TeeShirt = {
    size: number,
    color: TColors
}

const t1: TeeShirt = {
    size: 42,
    color: TColors.Red
}

const t2: TeeShirt = {
    size: 38,
    color: "yellow"
}
```

The `yellow` line will show an error.

And here, we have to use `TColors.` and then select one of the available colors.

```ts
const t3: TeeShirt = {
    size: 40,
    color: TColors.Green
}
```

---

### Another Way to Use Enum

```ts
enum Status {
    draft,
    private,
    public
}
```

Previously, we explicitly assigned values such as:

```ts
Red = "red"
```

Here we did not use `=`.

In this situation, TypeScript automatically assigns numeric values:

```text
draft   → 0
private → 1
public  → 2
```

Then:

```ts
type Article = {
    id: number;
    title: string;
    status: Status;
};

const article1: Article = {
    id: 1,
    title: "Understanding TypeScript Enums",
    status: Status.draft
};

const article2: Article = {
    id: 2,
    title: "Advanced TypeScript Types",
    status: Status.public
};
```

```ts
console.log(article2)
```

The `status` value in the output will be `2`.

That `2` is **not** the article's `id`. It is the numeric enum value for `Status.public`.

---

## 6. Map & Set

`Set` and `Map` are useful for storing:

- unique values → `Set`
- key-value pairs → `Map`

---

## 7. Set

=== "Normal JavaScript"

    ```js
    const data = new Set()

    data.add("abc")
    data.add(30)
    data.add(true)
    ```

    All these data types can be added in a normal JavaScript `Set`.

=== "TypeScript"

    We can make the type fixed:

    ```ts
    const data = new Set<string>()
    ```

    Now only string values can be added.

---

## 8. Map

=== "Normal JavaScript"

    ```js
    const players = new Map()

    players.set("Ronaldo", 7)
    players.set(10, "Messi")
    players.set("Neymar", "brazil")
    ```

=== "TypeScript"

    ```ts
    const players = new Map<string, number>()

    players.set("Ronaldo", 7)
    players.set("Messy", 10)
    ```

One important thing here is:

```ts
Map<keyType, valueType>
```

The first type is the **key**, and the second type is the **value**.

Map keys are unique.

If we use the same key again, the new value replaces the old value.

For example:

```ts
players.set("messy", 13)
players.set("messy", 10)
```

The second line rewrites the value associated with `"messy"`.

So the final value for `"messy"` is `10`.

Because a Map rewrites the value for an existing key, we can use it as a simple counting storage.

---

## 9. Practical Map Example: Juice Shop

Suppose we have a juice shop.

We want to automatically count how many times each juice has been ordered.

```ts
const orders = new Map<string, number>()

function addOrder(juice: string, count: number) {
    const quantity = orders.get(juice) || 0
    orders.set(juice, quantity + count)
}

addOrder("mango", 2)
addOrder("mango", 4)
addOrder("apple", 5)
addOrder("banana", 1)
addOrder("apple", 2)

console.log(orders)
```

The Map keeps the total quantity for each juice.
