# TypeScript Type Narrowing

## 1. Union Type

A union type lets a value have more than one possible type.

```ts
type ID = string | number;

const id: ID = "g-016"

function printId(id: ID) {
    console.log(`ID: ${id}`)
}

printId("g-017")
```

---

## 2. Another Union Example

```ts
type Rectangle = {
    height: number;
    width: number;
}

type Circle = {
    radius: number;
}

type Square = {
    length: number;
}

function calculateArea(shape: Rectangle | Circle | Square): number {
    if ("radius" in shape) {
        return Math.PI * shape.radius * shape.radius
    }

    else if ("length" in shape) {
        return shape.length * shape.length
    }

    return shape.height * shape.width
}
```

We can make the union reusable:

```ts
type Shape = Rectangle | Circle | Square

function calculateArea(shape: Shape): number {
    // ...
}
```
