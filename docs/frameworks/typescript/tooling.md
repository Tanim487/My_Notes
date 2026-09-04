# TypeScript Tooling

## 1. Rollup Bundler

Now we have two TypeScript files and want to bundle them into one JavaScript file.

We will use **Rollup**.

We install packages through `package.json`, because this is the normal way to manage JavaScript project dependencies.

---

## 2. `package.json`

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

## 3. `.gitignore`

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

## 4. Install the packages

In the terminal:

```bash
npm install
```

---

## 5. `rollup.config.js`

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

## 6. Update `index.html`

Change the script location:

```html
<script src="dist/app.js"></script>
```

Instead of loading the individual TypeScript file.

---

## 7. Watch Mode

To automatically rebuild when files change:

```bash
npm run watch
```

or:

```bash
rollup -c -w
```

---

## 8. Rollup Tree Shaking

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

## 9. Node.js with TypeScript

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

## 10. Creating a Modern TypeScript Project with Vite

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

## 11. Development vs Production

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

## 12. GitHub and Deployment

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

## 13. Important `tsconfig.json` Notes

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

## 14. `moduleDetection`

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
