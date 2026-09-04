# TypeScript Notes

> **Source:** TypeScript crash course by **LearnwithHasinHayder**
>
> These are my personal notes made while following the class. I have kept the original ideas, examples, observations, and learning points, while changing the Bangla explanations into simple English and organizing the material for easier reading.

---

## 1. Setup

### Install TypeScript

```bash
npm install -g typescript
tsc --version
```

`tsc` = TypeScript Compiler.

### Needed VS Code Extension

- Code Runner

### File Format

Example:

```text
example.ts
```

---

## 2. Why TypeScript?

=== "Normal JavaScript"

    ```js
    function divide(a, b) {
        if (b == 0) {
            throw new Error("Division by zero is not allowed")
        }

        return a / b
    }

    console.log(divide(6, 3))
    ```

    Here, if we pass a value of another type, the code can crash because JavaScript does not check the types of the parameters beforehand.

=== "Better with TypeScript"

    ```ts
    function divide(a: number, b: number): number {
        if (b == 0) {
            throw new Error("Division by zero is not allowed")
        }

        return a / b
    }

    console.log(divide(6, 3))
    ```

    The third `number` is the **return type** of this function.

    Now, if we change the arguments to strings, TypeScript will automatically show an error.

---

## 3. Types

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

## 4. Run a TypeScript File

We can run a TypeScript file in the terminal using:

```bash
node example.ts
```

or:

```bash
bun example.ts
```

or:

```bash
deno example.ts
```

---

## 5. TypeScript → JavaScript Transpilation

Suppose `example.ts` contains:

```ts
function add(a: number, b: number): number {
    return a + b;
}

function subtract(a: number, b: number): number {
    return a - b;
}

function multiply(a: number, b: number): number {
    return a * b;
}

function divide(a: number, b: number): number {
    if (b === 0) {
        throw new Error("Cannot divide by zero");
    }

    return a / b;
}

console.log("add(2, 3) =", add(2, 3));
console.log("subtract(5, 2) =", subtract(5, 2));
console.log("multiply(4, 3) =", multiply(4, 3));
console.log("divide(10, 2) =", divide(10, 2));
```

Now we want to connect it with an HTML file, so we make an `index.html` file.

## `index.html`

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h1>Demo</h1>
    <script src="example.ts"></script>
</body>
</html>
```

If we run this file in a browser, we cannot see the expected output in the console because the browser does not execute `.ts` files like normal JavaScript files. `.ts` can also be associated with HLS video-stream files, which can make the browser treat the extension differently.

So we have to convert the TypeScript code into JavaScript. This process is called **transpiling**.

---

## `tsconfig.json`

Create a `tsconfig.json` file:

```json
{
    "compilerOptions": {
        "target": "esnext"
    }
}
```

### Compile

In the terminal:

```bash
tsc
```

Run this after each time the TypeScript file is changed.

Or:

```bash
tsc --watch
```

We have to run `tsc --watch` once, and it will automatically update when the TypeScript file changes. Basically, it stays in **watch mode**.

After this, a new file will automatically be generated:

```text
example.js
```

So now we have to change the script location in the HTML file too.

## Updated `index.html`

```html
<body>
    <h1>Demo</h1>
    <script src="example.js"></script>
</body>
```

Previously it had `example.ts`; now we changed it to the JavaScript file.

---

## 6. Transpile Multiple TypeScript Files into Different Folders

What if we want to transpile multiple `.ts` files and put the generated JavaScript files into a different folder?

For example:

```text
assets/
├── js/
└── ts/
    ├── ex1.ts
    ├── ex2.ts
    └── ex3.ts
```

For this, change `tsconfig.json`:

```json
{
    "compilerOptions": {
        "target": "esnext",
        "outDir": "assets/js",
        "rootDir": "assets/ts"
    }
}
```

Now in the terminal:

```bash
tsc
```

or:

```bash
tsc --watch
```

After that, the TypeScript files will automatically be transpiled into the `js` folder.

We mainly need to set:

- `rootDir` → where our TypeScript source files are
- `outDir` → where the generated JavaScript files should go

---

## 7. Do Not Emit JavaScript When TypeScript Has Errors

Even if there is an error in the TypeScript file, running `tsc` can still create the JavaScript file. A warning will be shown in the terminal, but this may not be what we want.

If we want TypeScript errors to prevent JavaScript output, we can configure it.

## `tsconfig.json`

```json
{
    "compilerOptions": {
        "target": "esnext",
        "noEmitOnError": true
    }
}
```

`noEmitOnError: true` means TypeScript will not generate the JavaScript output when there are TypeScript errors.

---

## 8. Variable Naming

In TypeScript we can use variable names such as:

- `name`
- `status`
- `statusbar`
- `toolbar`
- etc.

Check naming conventions separately for better knowledge.

We can also do this:

```json
{
    "compilerOptions": {
        "target": "esnext",
        "noEmitOnError": true,
        "moduleDetection": "force"
    }
}
```

With `moduleDetection: "force"`, TypeScript treats the file as a module.

This can prevent names such as `name` or `status` from causing global-scope conflicts.

For better practice, always follow good naming conventions.

---

## 9. Arrays

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

## 10. Objects

Let's go to objects.

## Using `type`

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

## Using `interface`

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

## 11. A More Complex Interface Example

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

---

## 12. Tuple & Enum

### Tuple

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

### Another Tuple Example

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

## 13. Enum

Suppose we have a T-shirt:

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

What if the product never had a yellow color, or yellow is currently unavailable?

If we want to prevent a developer from accidentally entering another color, we can use an `enum`.

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

The `yellow` line will show an error. We have to use `TColors.` and then select one of the available colors.

```ts
const t3: TeeShirt = {
    size: 40,
    color: TColors.Green
}
```

---

## Another Way to Use Enum

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

## 14. Union Type

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

## Another Union Example

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

---

## 15. Literal Types

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

---

## 16. Omit Type

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

## 17. Optional Properties

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

---

## 18. Generic

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

## 19. Generic with API Fetching

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

---

## 20. Map & Set

`Set` and `Map` are useful for storing:

- unique values → `Set`
- key-value pairs → `Map`

---

## Set

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

## Map

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

## 21. Practical Map Example: Juice Shop

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

---

## 22. TypeScript with HTML

Suppose an HTML file has:

- one input with `id="email"`
- one button with `id="send"`

=== "Normal approach"

    ```ts
    const input = document.getElementById("email") as HTMLInputElement
    const button = document.getElementById("send") as HTMLButtonElement

    button.addEventListener("click", () => {
        console.log(`Subscription complete for ${input.value}`)
    })
    ```

    `HTMLInputElement` is added because TypeScript knows that `getElementById()` can return `null`, and it does not automatically know that the returned element is specifically an input element.

    In JavaScript, we do not have to write `HTMLInputElement` or `HTMLButtonElement`.

=== "Better: Generic `querySelector`"

    We can do the same thing using a generic:

    ```ts
    const input = document.querySelector<HTMLInputElement>("#email")
    const button = document.querySelector<HTMLButtonElement>("#send")

    if (button && input) {
        button.addEventListener("click", () => {
            console.log(`Subscription complete for ${input.value}`)
        })
    }
    ```

    The generic version is better because:

    1. We can specify the element type.
    2. We can also perform a `null` check.
    3. TypeScript can understand the element type.

    So the generic version is generally much better.

---

## 23. Project: Task Management Application

Let's build a small task manager application.

Suppose we already have an HTML file.

## `script.ts`

First, define the task type:

```ts
type Task = {
    id: number;
    title: string;
    completed: boolean;
}
```

Get the HTML elements:

```ts
const form = document.getElementById("task-form") as HTMLFormElement
const input = document.getElementById("task-input") as HTMLInputElement
const list = document.getElementById("task-list") as HTMLUListElement
```

Create the task storage:

```ts
let tasks: Task[] = []
let nextId = 1
```

---

## Create Sample Tasks

Let's create a function for adding sample tasks.

```ts
function createSampleTasks(): void {
    const samples: Omit<Task, "id">[] = [
        { title: "Buy groceries", completed: true },
        { title: "Finish TypeScript homework", completed: false },
        { title: "Call the dentist", completed: false },
        { title: "Water the plants", completed: true },
    ]

    samples.forEach((sample) => {
        tasks.push({
            id: nextId++,
            title: sample.title,
            completed: sample.completed
        })
    })
}

createSampleTasks()

console.log(tasks)
```

`Omit<Task, "id">` means all the properties of `Task` except `id`.

The `id` is generated automatically.

---

## HTML Task List

In `index.html`:

```html
<section>
    <h2>Tasks</h2>
    <ul id="task-list">
        <!-- All rendered task data will appear here -->
    </ul>
</section>
```

---

## 24. Render Tasks

Now we need to render the tasks into the HTML file.

Add this after the previous code in `script.ts`:

```ts
function renderTasks(): void {
    list.innerHTML = ""

    tasks.forEach((task) => {
        const li = document.createElement("li")

        const checkbox = document.createElement("input")
        checkbox.type = "checkbox"
        checkbox.checked = task.completed

        checkbox.addEventListener("change", () => {
            task.completed = checkbox.checked
        })

        const span = document.createElement("span")
        span.textContent = task.title

        li.append(checkbox, span)
        list.append(li)
    })
}
```

---

## 25. Add a New Task

Now let's add a function for adding a new task.

```ts
function addTask(title: string): void {
    tasks.push({
        id: nextId++,
        title: title,
        completed: false
    })

    renderTasks()
}
```

Then handle the form submission:

```ts
form.addEventListener("submit", (event) => {
    event.preventDefault()

    addTask(input.value)

    input.value = ""
    input.focus()
})
```

### What does `event.preventDefault()` do?

When a form is submitted, the browser has a default behavior. Usually, the browser may submit the form and reload/navigate the page.

```ts
event.preventDefault()
```

stops that default browser behavior.

This allows our TypeScript code to handle the form submission ourselves without the page being reloaded.

### What does `input.focus()` do?

```ts
input.focus()
```

puts the keyboard focus back into the input field.

For example:

1. Type a task name.
2. Press Enter.
3. The task is added.
4. The input is cleared.
5. `input.focus()` puts the cursor back into the input.
6. We can immediately type the next task without clicking the input again.

---

## 26. Script Bundle

In real projects, the entire project script is usually not written in one TypeScript file.

Different responsibilities are separated into different `.ts` files.

Then we can bundle those TypeScript files and create one JavaScript output file.

This is a more advanced process.

Let's use the same task-management project to learn bundling.

---

## Create a `src` Folder

Inside `src`, create two files.

```text
src/
├── seed.ts
└── script.ts
```

---

## `src/seed.ts`

```ts
export type Task = {
    id: number;
    title: string;
    completed: boolean;
}

export let tasks: Task[] = []

export let nextId = 1
```

We could change exported variables directly, but directly changing shared state is generally not the best practice.

So we can use a function:

```ts
export function getNextId(): number {
    return nextId++
}
```

Now the ID is changed through a function instead of directly modifying the variable.

### Sample task function

```ts
export function createSampleTasks(): void {
    const samples: Omit<Task, "id">[] = [
        { title: "Buy groceries", completed: true },
        { title: "Finish TypeScript homework", completed: false },
        { title: "Call the dentist", completed: false },
        { title: "Water the plants", completed: true },
        { title: "Refuel the scooter", completed: false },
    ]

    samples.forEach((sample) => {
        tasks.push({
            id: nextId++,
            title: sample.title,
            completed: sample.completed
        })
    })
}
```

### Why are we using `export` now?

In the earlier version, everything was inside one file, so we did not need to export anything.

Now the code is split into multiple files.

Modern TypeScript projects commonly organize code into **modules**.

So we export the things that another file needs to import.

---

## 27. Import from `seed.ts`

Because we moved some code from `script.ts` into `seed.ts`, we need to import those things into `script.ts`.

## `src/script.ts`

```ts
import {
    tasks,
    getNextId,
    createSampleTasks
} from "./seed"

const form = document.getElementById("task-form") as HTMLFormElement
const input = document.getElementById("task-input") as HTMLInputElement
const list = document.getElementById("task-list") as HTMLUListElement
const exportButton = document.getElementById("export") as HTMLButtonElement
```

Then:

```ts
function renderTasks(): void {
    list.innerHTML = ""

    tasks.forEach((task) => {
        const li = document.createElement("li")

        const checkbox = document.createElement("input")
        checkbox.type = "checkbox"
        checkbox.checked = task.completed

        checkbox.addEventListener("change", () => {
            task.completed = checkbox.checked
        })

        const span = document.createElement("span")
        span.textContent = task.title

        li.append(checkbox, span)
        list.append(li)
    })
}
```

Add task:

```ts
function addTask(title: string): void {
    tasks.push({
        id: getNextId(),
        title: title,
        completed: false
    })

    renderTasks()
}
```

Form handling:

```ts
form.addEventListener("submit", (event) => {
    event.preventDefault()

    addTask(input.value)

    input.value = ""
    input.focus()
})
```

---

## 28. Rollup Bundler

Now we have two TypeScript files and want to bundle them into one JavaScript file.

We will use **Rollup**.

We install packages through `package.json`, because this is the normal way to manage JavaScript project dependencies.

---

## `package.json`

Create a new `package.json`:

```json
{
    "name": "task-manager",
    "version": "1.0.0",
    "private": true,
    "type": "module",
    "scripts": {
        "build": "rollup -c",
        "watch": "rollup -c -w"
    },
    "devDependencies": {
        "@rollup/plugin-typescript": "^12.1.0",
        "rollup": "^4.24.0",
        "tslib": "^2.8.0",
        "typescript": "^5.9.0"
    }
}
```

---

## `.gitignore`

Create another file:

```text
.gitignore
```

Add:

```text
node_modules
```

### Why do we add `node_modules` to `.gitignore`?

`node_modules` contains installed dependencies.

We normally do **not** push this huge generated folder to GitHub.

Instead, we push:

- `package.json`
- `package-lock.json` (when generated)

Then another developer or the deployment platform can run:

```bash
npm install
```

and recreate `node_modules`.

---

## Install the packages

In the terminal:

```bash
npm install
```

---

## 29. `rollup.config.js`

Create:

```text
rollup.config.js
```

Add:

```js
import typescript from "@rollup/plugin-typescript";

export default {
    input: "src/script.ts",

    output: {
        file: "dist/app.js",
        format: "iife"
    },

    plugins: [
        typescript()
    ]
};
```

### Build

```bash
npm run build
```

or:

```bash
rollup -c
```

The bundled output will be:

```text
dist/app.js
```

---

## Update `index.html`

Change the script location:

```html
<script src="dist/app.js"></script>
```

Instead of loading the individual TypeScript file.

---

## Watch Mode

To automatically rebuild when files change:

```bash
npm run watch
```

or:

```bash
rollup -c -w
```

---

## 30. Rollup Tree Shaking

Tree shaking is an important built-in Rollup feature.

Suppose we have:

```ts
function deleteTask() {
    console.log("will implement")
}
```

But this function is never used.

Rollup can detect that it is unused and remove it from the final bundle.

So unused code does not need to be included in the production bundle.

This process is called **tree shaking**.

---

## 31. Node.js with TypeScript

Server-side applications such as:

- Node.js
- Bun
- Deno

can work with TypeScript.

Example `example.ts`:

```ts
function add(a: number, b: number): number {
    return a + b
}

function subtract(a: number, b: number): number {
    return a - b
}

console.log(add(10, 6))
console.log(subtract(22, 6))
```

Run:

```bash
node example.ts
```

or:

```bash
bun example.ts
```

or:

```bash
deno example.ts
```

The class notes point out that Node.js has been adding TypeScript support and that modern Node can run TypeScript files directly in supported situations.

---

## 32. TypeScript Modules with Node.js

Let's make the example a module.

Create `mathlib.ts`:

```ts
export function add(a: number, b: number): number {
    return a + b
}

export function subtract(a: number, b: number): number {
    return a - b
}
```

We can also export at the bottom:

```ts
function add(a: number, b: number): number {
    return a + b
}

function subtract(a: number, b: number): number {
    return a - b
}

export { add, subtract }
```

Then in `example.ts`:

```ts
import { add, subtract } from "./mathlib.ts"

console.log(add(10, 6))
console.log(subtract(22, 6))
```

The behavior can differ depending on the runtime and its TypeScript/module support.

In the original class example:

```bash
node example.ts
```

was shown as producing an error for this module setup, while:

```bash
bun example.ts
```

and:

```bash
deno example.ts
```

were shown as working.

---

## Fixing Node's Module Setup

To fix the module setup for Node, create:

```text
package.json
```

with:

```json
{
    "type": "module"
}
```

After this, the module configuration is explicitly set to ES modules.

---

## 33. OOP in TypeScript

Before the OOP example, it is important to understand:

- `public`
- `private`
- `protected`

---

## `public`

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

## `private`

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

## `protected`

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

## 34. TypeScript OOP Example

## `account.ts`

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

## Constructor Parameter Properties

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

## 35. Add Methods to the Account Class

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

## 36. Interface for Different Account Types

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

## Implement the Interface

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

## 37. Inheritance

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

## Use the Student Account

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

---

## 38. Creating a Modern TypeScript Project with Vite

Developers do not want to waste time manually creating every folder and configuration file.

They often use **Vite** to speed up project setup and development.

In the terminal:

```bash
npm create vite@latest project_name
```

Then:

```bash
cd project_name
npm install
```

Open the project in an IDE such as VS Code.

Run:

```bash
npm run dev
```

Vite automatically sets up many things for us.

We can still modify the generated project.

---

## 39. Development vs Production

During development, we can use:

```bash
npm run dev
```

to run the development server and visually see the project.

But before deployment, we should make sure the project also works correctly as production code.

So run:

```bash
npm run build
```

Then:

```bash
npm run preview
```

`npm run preview` lets us visualize the production build.

The preview is based on the build output.

### Development

```bash
npm run dev
```

### Production build

```bash
npm run build
```

### Preview production build

```bash
npm run preview
```

---

## 40. GitHub and Deployment

While developing, we normally do not push generated folders such as:

```text
dist/
node_modules/
```

to GitHub.

So `.gitignore` should at least contain:

```text
node_modules
dist
```

We can push the whole source project to GitHub.

Then on a deployment platform:

1. Log in with GitHub.
2. Add a new project.
3. Select the repository.
4. Deploy.

For example, Vercel can automatically build and deploy the project.

---

## 41. Fetch API Data with a Button

Suppose we have a button with the ID `btn`.

```ts
const btn = document.getElementById("btn") as HTMLButtonElement

// const heading = document.getElementById("heading") as HTMLHeadingElement

let data = []

btn.addEventListener("click", async () => {
    const response = await fetch(
        "https://tsapidemo.lwhh.org/api/v1/books"
    )

    const books = await response.json()

    console.log(books)

    data = books

    printBookDetails(books[1])
})
```

Function:

```ts
function printBookDetails(book) {
    // console.clear();

    console.log("ID:", book.id)
    console.log("Title:", book.title)
    console.log("Author:", book.author)
    console.log("Category:", book.category)
    console.log("Price:", book.price)
}
```

### What did we do?

First, we fetched data from the API URL.

Then we converted the response into JSON and stored it in `books`.

Then:

```ts
printBookDetails(books[1])
```

passes one book to the function, so we can see that book's information.

---

## 42. Why Validate API Data?

There is a problem.

What if, in the future, someone adds a book with a different key-value structure?

What if there are multiple categories?

What if another key is added?

Our code assumes that the API response always has the exact structure we expect.

So whenever we use API data, we should validate the data to make sure it has the format we expect.

For data validation, we can use a library called **Zod**.

---

## 43. Zod

### Install Zod

In the terminal:

```bash
npm install zod
```

After installing it, import Zod:

```ts
import { z } from "zod"
```

Importing Zod alone is not enough.

We also need to create a schema describing the format we expect from the API.

---

## Book Schema

```ts
const BookSchema = z.object({
    id: z.number(),
    title: z.string(),
    author: z.string(),
    category: z.string(),
    price: z.number(),
})
```

This schema describes **one book**.

> The schema above is created using Zod. It is not a TypeScript type by itself.

If the API sends an array of books, we need an array schema:

```ts
const BooksSchema = z.array(BookSchema)
```

---

## Convert the Schema into a TypeScript Type

```ts
type Book = z.infer<typeof BookSchema>
```

Now TypeScript can derive the `Book` type from the Zod schema.

---

## 44. Validate API Data with `safeParse`

Now the API request:

```ts
const btn = document.getElementById("btn") as HTMLButtonElement

// const heading = document.getElementById("heading") as HTMLHeadingElement

let data = []

btn.addEventListener("click", async () => {
    const response = await fetch(
        "https://tsapidemo.lwhh.org/api/v1/books"
    )

    // Use another version to test validation:
    // const response = await fetch(
    //     "https://tsapidemo.lwhh.org/api/v2/books"
    // )

    const books = await response.json()

    const result = BooksSchema.safeParse(books)

    if (!result.success) {
        console.error(result.error.issues)
        return
    }

    console.log(books)

    data = books

    printBookDetails(books[1])
})
```

This line performs the validation:

```ts
const result = BooksSchema.safeParse(books)
```

If the data is valid:

```ts
result.success
```

will be `true`.

If the data is invalid, we can inspect:

```ts
result.error.issues
```

and stop processing the invalid data.

---

## 45. Create Other Projects with Vite

To create a new Vite project:

```bash
npm create vite@latest projectName
```

Vite can ask us to select a framework, such as:

- Vanilla
- React
- Vue
- and many others

Then we can select a variant/type, such as:

- TypeScript
- JavaScript
- and others

After that:

```bash
npm install
```

Then:

```bash
npm run dev
```

---

## 46. Important `tsconfig.json` Notes

So far, we manually created `tsconfig.json` files and used only a small number of options.

But if we look at a `tsconfig.json` file in a real project or on YouTube, we may see many more settings.

We can automatically create a TypeScript configuration file instead of creating it manually.

Run:

```bash
tsc --init
```

This automatically creates a `tsconfig.json` with default values and comments.

The generated file also contains documentation links that can be studied for deeper knowledge.

As a beginner, we can make the debugging-related output/source-map options disabled if we do not need them.

---

## `moduleDetection`

By default, we may see:

```json
"moduleDetection": "force"
```

We can change it to:

```json
"moduleDetection": "auto"
```

With `auto`, TypeScript can decide whether a file is a module based on its contents and configuration.

With `force`, files are treated as modules.

This changes how the generated JavaScript is structured.

---

## 47. How Exactly AI Can Help Us

AI can help with tasks such as:

- Converting an existing project to TypeScript
- Starting a new project from scratch

Let's see how we can use AI to convert a project into a TypeScript structure.

---

## 48. Using AI to Convert an Existing Project to TypeScript

Suppose we have one HTML file containing:

- HTML
- CSS
- JavaScript

Now we want to convert it into a proper TypeScript project structure.

We can use an AI coding tool such as **Kilo Code**.

Install the Kilo Code extension.

The class also mentions using free models, such as Tencent Hunyuan, depending on what is available in the tool.

---

## Create Two Instruction Files

Create:

```text
instruction.md
task.md
```

### `instruction.md`

This file contains global instructions.

```md
# Instructions

- Keep the code simple and beginner-friendly.
- Use plain TypeScript with `tsc`.
- Do not use Vite or any bundler.
- Never use `any`.
- Use strict TypeScript.
- Use modern ES modules.
- Keep the original functionality unchanged.
```

The benefit is that we do not have to repeat the same instructions in every prompt.

We can tell the AI to follow the instructions in this file.

---

## `task.md`

This file contains the specific task.

```md
# Task

Convert the provided HTML file into a working TypeScript project.

- Move CSS to `assets/css/style.css`.
- Move JavaScript to `assets/ts/main.ts`.
- Replace JavaScript with TypeScript.
- Fetch data from the API endpoint.
- Update the HTML to load `assets/css/style.css` and `assets/js/main.js`.
- Create a minimal `tsconfig.json` that compiles `assets/ts` to `assets/js`.
- Load live books data from https://tsapidemo.lwhh.org/api/v1/books
```

---

## 49. Using Kilo Code

The class workflow is:

1. Install Kilo Code from the extension/library.
2. Open the command palette.
3. Select the Kilo Code toggle command.

The class also mentions:

```text
Ctrl + Alt + B
```

as a shortcut.

Then open the Kilo Code chat window.

Use a prompt like:

```text
Implement this task @task.md - follow the instructions @instruction.md
```

Files such as:

```text
@task.md
@instruction.md
```

can be attached/referenced in the chat.

The class notes mention that files can be dragged into the chat box, and pressing `Shift` while dragging can be used to drop/reference them.

Then select the model you want to use.

The class example uses a free Tencent model.

---

## 50. Choosing a Mode

Another important setting is the mode.

If the task is small, we can use **Code mode**.

For a larger task involving things like:

- calculation
- deeper thinking
- exploration
- complex planning

we can use **Plan mode** first.

---

## 51. Creating a New Project from Scratch with AI

We can also use AI to create a completely new project.

Again, create two files:

```text
instruction.md
task.md
```

---

## `instruction.md`

```md
# Instructions

- Keep the code simple and beginner-friendly.
- Use plain TypeScript with `tsc`.
- Do not use Vite or any bundler.
- Never use `any`.
- Enable strict mode.
- Use `ESNext` for `target` and `module`.
- Use modern ES modules.
- Create a clean, minimal design with ample whitespace.
```

---

## `task.md`

```md
# Task

Create a simple Book Catalog application.

Create a clean, minimal `markup.html` with ample whitespace.

Then convert it into a TypeScript project.

- Keep the original markup in `markup.html`.
- Move CSS to `assets/css/style.css`.
- Move TypeScript to `assets/ts/main.ts`.
- Compile TypeScript to `assets/js/main.js`.
- Create a minimal `tsconfig.json` using `ESNext`.
- Load live books data from https://tsapidemo.lwhh.org/api/v1/books.
- Display the books in a clean, responsive catalog.
- Update the page to load the generated CSS and JavaScript.
```

Then use the same process:

1. Open Kilo Code.
2. Use the shortcut if configured.
3. Open the chat.
4. Provide:

```text
Implement this task @task.md - follow the instructions @instruction.md
```

5. Select the tool/model.
6. Select the appropriate mode.
7. Run it.

Here we specifically told the AI to use `ESNext`, for example.

---

## 52. The Main Idea Behind `instruction.md` and `task.md`

The basic idea is:

### `instruction.md`

Keep the **global rules** here.

Examples:

- coding style
- TypeScript rules
- libraries to avoid
- whether `any` is allowed
- module system
- design preferences

### `task.md`

Keep the **specific task** here.

Examples:

- what project to create
- what files to create
- where CSS should go
- where TypeScript should go
- which API to use
- what the UI should do

The more specific and clear we can make the instructions, the better the AI can organize the code.

---

## 53. AI Tools Mentioned for Design

Some AI tools mentioned for design-related work:

- Lovable
- v0
- Bolt

---

# Quick Reference

| Topic | Main idea |
|---|---|
| Type annotations | Tell TypeScript what type a value should have |
| Arrays | `string[]`, `number[]` |
| Objects | `type` or `interface` |
| Tuple | Fixed order/number of array elements |
| Enum | Restrict values to named constants |
| Union | Allow multiple possible types |
| Literal type | Allow a fixed set of exact values |
| `Omit` | Remove selected properties from a type |
| Optional property | `property?: type` |
| Generic | Work with dynamic/reusable types |
| `Set` | Store unique values |
| `Map` | Store key-value pairs |
| `public` | Accessible everywhere |
| `protected` | Class + subclasses |
| `private` | Only the declaring class |
| `readonly` | Prevent reassignment |
| Rollup | Bundle modules into a final JavaScript file |
| Tree shaking | Remove unused code from the bundle |
| Vite | Quickly create and run modern projects |
| Zod | Runtime data validation |
| `tsconfig.json` | TypeScript compiler configuration |
| AI instructions | Separate global rules from individual tasks |

---

# Final Notes

The most important ideas from this class are:

1. TypeScript adds static type information to JavaScript.
2. `tsc` can transpile TypeScript into JavaScript.
3. `tsconfig.json` controls TypeScript compilation.
4. `type` and `interface` are both useful for describing object shapes.
5. Tuples describe fixed-position arrays.
6. Enums and literal types can restrict values.
7. Union types allow more than one possible type.
8. Utility types such as `Omit` help create reusable type definitions.
9. Optional properties use `?`.
10. Generics make functions reusable while keeping type safety.
11. `Map` and `Set` are useful TypeScript-friendly data structures.
12. DOM elements can be typed using assertions or generic DOM APIs.
13. Larger projects should separate code into modules.
14. Rollup can bundle modules and perform tree shaking.
15. TypeScript can be used for server-side applications.
16. `public`, `protected`, and `private` control class member access.
17. Vite makes project setup and development faster.
18. Production builds should be tested with `npm run build` and `npm run preview`.
19. API responses should be validated instead of blindly trusting their structure.
20. Zod can be used to validate API data at runtime.
21. AI can help convert existing projects or create new projects when given clear instructions.
22. Keeping global instructions in `instruction.md` and project-specific work in `task.md` makes AI-assisted development more organized.

---

> **End of notes — TypeScript crash course by LearnwithHasinHayder**


---
