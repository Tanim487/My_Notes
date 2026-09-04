# TypeScript Basics

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


## 3. Run a TypeScript File

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

## 4. TypeScript → JavaScript Transpilation

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

In `index.html`

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

Create a `tsconfig.json` file:

```json
{
    "compilerOptions": {
        "target": "esnext"
    }
}
```

Now the compilation part,

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

Now modify the script tag in `index.html`

```html
<body>
    <h1>Demo</h1>
    <script src="example.js"></script>
</body>
```

Previously it had `example.ts`; now we changed it to the JavaScript file.

---

## 5. Transpile Multiple TypeScript Files into Different Folders

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

## 6. Handling Errors in TypeScript

Even if there is an error in the TypeScript file, running `tsc` can still create the JavaScript file. A warning will be shown in the terminal, but this may not be what we want.

If we want TypeScript errors to prevent JavaScript output, we can configure it,

In `tsconfig.json`

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

## 7. Variable Naming Conventions

In TypeScript we can not use variable names such as:

- `name`
- `status`
- `statusbar`
- `toolbar`
- etc.

Check naming conventions separately for better knowledge.

If we want to use those names, we can also do this:

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
