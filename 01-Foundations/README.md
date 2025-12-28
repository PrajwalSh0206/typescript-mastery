# Module 01: TypeScript Foundations

The goal of this module is to understand the **core TypeScript type system**, starting from primitive types and gradually moving toward basic type composition.  
By the end of this module, you should be able to read and write simple TypeScript code confidently.

---

## 1. Primitive Types

In JavaScript, variables are dynamically typed — their type can change at runtime.  
In TypeScript, once a type is assigned, it cannot change.

### Core Primitive Types

| Type | Description | Example |
|------|------------|---------|
| `string` | Textual data | `"Hello World"` |
| `number` | Integers and decimals | `42`, `3.14` |
| `boolean` | Logical values | `true`, `false` |
| `any` | Disables type checking | Avoid using this |

## 2. Type Inference

TypeScript can automatically infer the type of a variable based on its assigned value.

```ts
let greeting = "Hello"; // inferred as string
greeting = "Hi";       // ✅
// greeting = 10;      // ❌ Error
```

## 3. Arrays and Tuples

### Arrays

Arrays store multiple values of the same type.

```ts
let skills: string[] = ["TypeScript", "JavaScript"];
skills.push("Node.js"); // ✅
```

### Tuples

Tuples define a fixed-length array where each position has a specific type.

```ts
let userRole: [number, string] = [1, "Admin"];
// let invalidUserRole: [number, string] = ["Admin", 1]; // ❌
```

## 4. Functions

Functions in TypeScript should define parameter types and return types.

```ts
function greet(name: string): string {
  return `Hello, ${name}`;
}
```