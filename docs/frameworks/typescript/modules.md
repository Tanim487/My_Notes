# TypeScript Modules

## 1. Script Bundle

In real projects, the entire project script is usually not written in one TypeScript file.

Different responsibilities are separated into different `.ts` files.

Then we can bundle those TypeScript files and create one JavaScript output file.

This is a more advanced process.

Let's use the same task-management project to learn bundling.

---

## 2. Create a `src` Folder

Inside `src`, create two files.

```text
src/
├── seed.ts
└── script.ts
```

---

## 3. `src/seed.ts`

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

## 4. Import from `seed.ts`

Because we moved some code from `script.ts` into `seed.ts`, we need to import those things into `script.ts`.

## 5. `src/script.ts`

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

## 6. TypeScript Modules with Node.js

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

## 7. Fixing Node's Module Setup

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
