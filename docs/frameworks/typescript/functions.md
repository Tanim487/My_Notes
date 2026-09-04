# TypeScript Functions

Functions let us group a set of instructions and run them whenever we need them.

## 1. Writing a Function

The basic syntax of a TypeScript function is:

```ts
function functionName(parameter: type): returnType {
	// function body
	return value
}
```

For example:

```ts
function greet(name: string): string {
	return `Hello, ${name}`
}

const message = greet("Tanim")

console.log(message)
```

Here:

- `name: string` means the function accepts a string parameter.
- `: string` after the closing parenthesis is the return type.
- `return` sends a value back to the place where the function was called.

## 2. Function Parameters

We should specify the type of every parameter.

```ts
function add(a: number, b: number): number {
	return a + b
}

console.log(add(5, 3))
```

The function accepts two numbers and returns a number.

If we pass a string instead of a number, TypeScript will show an error:

```ts
add("5", 3)
```

## 3. Return Type

The return type tells TypeScript what kind of value the function will return.

```ts
function multiply(a: number, b: number): number {
	return a * b
}
```

If a function returns a string, its return type should be `string`:

```ts
function getFullName(firstName: string, lastName: string): string {
	return `${firstName} ${lastName}`
}
```

## 4. A Function Without a Return Value

If a function does not return a value, we can use the `void` return type.

```ts
function showMessage(message: string): void {
	console.log(message)
}

showMessage("TypeScript is useful")
```

The function performs an action, but it does not return a value.

## 5. Optional Parameters

We can make a parameter optional by adding `?` after its name.

```ts
function greetUser(name: string, title?: string): string {
	if (title) {
		return `Hello, ${title} ${name}`
	}

	return `Hello, ${name}`
}

console.log(greetUser("Tanim"))
console.log(greetUser("Tanim", "Mr."))
```

An optional parameter must come after the required parameters.

## 6. Default Parameters

Instead of making a parameter optional, we can give it a default value.

```ts
function welcome(name: string, greeting: string = "Hello"): string {
	return `${greeting}, ${name}`
}

console.log(welcome("Tanim"))
console.log(welcome("Tanim", "Good morning"))
```

If the caller does not provide `greeting`, TypeScript uses the default value.

## 7. Arrow Functions

We can also write a function using arrow-function syntax.

```ts
const subtract = (a: number, b: number): number => {
	return a - b
}

console.log(subtract(10, 4))
```

For a one-line function, we can write it more shortly:

```ts
const square = (number: number): number => number * number

console.log(square(5))
```

## 8. Function Expressions

A function can be stored in a variable.

```ts
const divide = function (a: number, b: number): number {
	return a / b
}

console.log(divide(10, 2))
```

The parameter types and return type are written in the same way as a normal function.

## 9. Callback Functions

A callback is a function passed as an argument to another function.

```ts
function processNumber(
	number: number,
	callback: (value: number) => number
): number {
	return callback(number)
}

const result = processNumber(5, (value: number): number => value * 2)

console.log(result)
```

Here, `(value: number) => number` describes a function that accepts a number and returns a number.

## 10. Rest Parameters

Rest parameters allow a function to accept any number of arguments of the same type.

```ts
function sum(...numbers: number[]): number {
	return numbers.reduce((total, number) => total + number, 0)
}

console.log(sum(1, 2, 3))
console.log(sum(10, 20, 30, 40))
```

The `...numbers` parameter collects all the provided numbers into an array.

## 11. Function Returning a Tuple

A function can return a tuple when it returns multiple fixed-position values.

```ts
type Division = [number, number]

function divideWithRemainder(a: number, b: number): Division {
	const quotient = Math.floor(a / b)
	const remainder = a % b

	return [quotient, remainder]
}

const result: Division = divideWithRemainder(7, 2)

console.log(result)
```

The return value is a tuple containing the quotient and the remainder.
