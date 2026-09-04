# TypeScript Projects

## 1. TypeScript with HTML

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

## 2. Project: Task Management Application

Let's build a small task manager application.

Suppose we already have an HTML file.

## 3. `script.ts`

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

## 4. Create Sample Tasks

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

## 5. HTML Task List

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

## 6. Render Tasks

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

## 7. Add a New Task

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

## 8. Fetch API Data with a Button

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

## 9. Why Validate API Data?

There is a problem.

What if, in the future, someone adds a book with a different key-value structure?

What if there are multiple categories?

What if another key is added?

Our code assumes that the API response always has the exact structure we expect.

So whenever we use API data, we should validate the data to make sure it has the format we expect.

For data validation, we can use a library called **Zod**.

---

## 10. Zod

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

## 11. Book Schema

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

## 12. Convert the Schema into a TypeScript Type

```ts
type Book = z.infer<typeof BookSchema>
```

Now TypeScript can derive the `Book` type from the Zod schema.

---

## 13. Validate API Data with `safeParse`

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

## 14. Create Other Projects with Vite

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

## 15. How Exactly AI Can Help Us

AI can help with tasks such as:

- Converting an existing project to TypeScript
- Starting a new project from scratch

Let's see how we can use AI to convert a project into a TypeScript structure.

---

## 16. Using AI to Convert an Existing Project to TypeScript

Suppose we have one HTML file containing:

- HTML
- CSS
- JavaScript

Now we want to convert it into a proper TypeScript project structure.

We can use an AI coding tool such as **Kilo Code**.

Install the Kilo Code extension.

The class also mentions using free models, such as Tencent Hunyuan, depending on what is available in the tool.

---

## 17. Create Two Instruction Files

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

### `task.md`

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

## 18. Using Kilo Code

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

## 19. Choosing a Mode

Another important setting is the mode.

If the task is small, we can use **Code mode**.

For a larger task involving things like:

- calculation
- deeper thinking
- exploration
- complex planning

we can use **Plan mode** first.

---

## 20. Creating a New Project from Scratch with AI

We can also use AI to create a completely new project.

Again, create two files:

```text
instruction.md
task.md
```

---

## 21. `instruction.md`

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

## 22. `task.md`

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

## 23. The Main Idea Behind `instruction.md` and `task.md`

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

## 24. AI Tools Mentioned for Design

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
