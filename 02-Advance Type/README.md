# Module 02: Advanced Type System

The goal of this module is to understand how TypeScript **narrows, composes, and protects types** in real-world scenarios.  
By the end of this module, you should be comfortable handling complex data shapes safely.

---

## 1. Union Types (|)

Union types allow a value to be one of multiple types.

```ts
let value: string | number;

value = "text"; // ✅
value = 42;     // ✅
```

## 2. Discriminated Unions

Discriminated unions use a **common property** (called a *discriminator*) to safely determine the shape of an object.  
This allows TypeScript to automatically narrow the type based on that property.

```ts
type SuccessResponse = {
  status: "success";
  data: string;
};

type ErrorResponse = {
  status: "error";
  message: string;
};

type ApiResponse = SuccessResponse | ErrorResponse;

function handleResponse(response: ApiResponse) {
  if (response.status === "success") {
    console.log(response.data);
  } else {
    console.log(response.message);
  }
}
```

This pattern is heavily used in APIs and state management.

## 3. Intersection Types (&)

Intersection types combine multiple types into a single type.

```ts
type User = {
  id: number;
  name: string;
};

type Admin = {
  role: "admin";
};

type AdminUser = User & Admin;

const admin: AdminUser = {
  id: 1,
  name: "User",
  role: "admin"
};
```

## 4. unknown vs any

### any (Unsafe)
```ts
let data: any = "text";
data.toUpperCase(); // ✅ but unsafe
```

### unknown (Safe)
```ts
let data: unknown = "text";

// data.toUpperCase(); // ❌ Error

if (typeof data === "string") {
  data.toUpperCase(); // ✅
}
```
Prefer unknown when the type is not known in advance.

## 5. Type Guards

Type guards are functions that help TypeScript narrow types.

### `typeof` Type Guard

Used to check **primitive types**.

```ts
function printValue(value: string | number) {
  if (typeof value === "string") {
    console.log(value.toUpperCase());
  } else {
    console.log(value.toFixed(2));
  }
}
```

### `instanceof` Type Guard

instanceof works only with classes, not interfaces or type aliases.

```ts
class Dog {
  bark() {
    console.log("Woof");
  }
}

class Cat {
  meow() {
    console.log("Meow");
  }
}

function makeSound(animal: Dog | Cat) {
  if (animal instanceof Dog) {
    animal.bark(); // Dog
  } else {
    animal.meow(); // Cat
  }
}
```

### `in` Operator Type Guard

```ts
type Admin = {
  role: "admin";
  permissions: string[];
};

type User = {
  role: "user";
};

function checkRole(person: Admin | User) {
  if ("permissions" in person) {
    console.log(person.permissions); // Admin
  }
}
```

## 6. Optional Properties

Optional properties may or may not exist.

```ts
type UserProfile = {
  name: string;
  age?: number;
};

const profile: UserProfile = {
  name: "User"
};
```

## 7. Readonly Properties

Readonly properties cannot be reassigned.

```ts
type Config = {
  readonly apiUrl: string;
};

const config: Config = {
  apiUrl: "https://api.example.com"
};

// config.apiUrl = "new-url"; // ❌ Error
```

## 8. Function Overloading

**Function overloading** allows you to define **multiple function signatures** for the same function name.  
Each signature describes a different way the function can be called, while the implementation handles all cases.

TypeScript uses **overload signatures for type checking** and a **single implementation** at runtime.


### Basic Function Overloading

```ts
function add(a: number, b: number): number;
function add(a: string, b: string): string;

function add(a: number | string, b: number | string) {
  if (typeof a === "string" && typeof b === "string") {
    return a + b;
  }
  if (typeof a === "number" && typeof b === "number") {
    return a + b;
  }
  throw new Error("Invalid arguments");
}
```

### Overloading with Optional Parameters

```ts
function greet(name: string): string;
function greet(name: string, age: number): string;

function greet(name: string, age?: number) {
  if (age !== undefined) {
    return `Hello ${name}, age ${age}`;
  }
  return `Hello ${name}`;
}
```

## 9. Generic Types

**Generics** allow you to write **reusable and type-safe code** by defining types
that work with multiple data types instead of a single fixed type.

Instead of using `any`, generics preserve **type information**.

---


### With Generics (Safe)

```ts
function identity<T>(value: T): T {
  return value;
}

identity<string>("hello");
identity<number>(42);
```

### Generic Interfaces

```ts
interface ApiResponse<T> {
  data: T;
  success: boolean;
}

const response: ApiResponse<string> = {
  data: "OK",
  success: true
};
```

### Generic Type Aliases

```ts
type Wrapper<T> = {
  value: T;
};

const wrappedNumber: Wrapper<number> = {
  value: 42
};
```

### Multiple Generic Parameters

```ts
function merge<T, U>(a: T, b: U): T & U {
  return { ...a, ...b };
}

const result = merge({ name: "User" }, { age: 30 });
```

### Default Generic Types

```ts
type ApiResult<T = string> = {
  data: T;
  success: boolean;
};

const result: ApiResult = {
  data: "OK",
  success: true
};
```

## 10.  Generic Constraints

**Constraints** restrict the types that can be used with generics.
They ensure the generic type has certain properties or follows specific rules.

Constraints use the `extends` keyword.

---

### Basic Constraint

```ts
function getLength<T extends { length: number }>(value: T): number {
  return value.length;
}

getLength("hello");   // ✅
getLength([1, 2, 3]); // ✅
// getLength(10);     // ❌ number has no length
```

## Constraint with Interfaces

```ts
interface HasId {
  id: number;
}

function getId<T extends HasId>(obj: T): number {
  return obj.id;
}

getId({ id: 1, name: "User" }); // ✅
// getId({ name: "User" });    // ❌
```

## `keyof` Constraint

```ts
function getValue<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}

const user = { name: "User", age: 30 };

getValue(user, "name"); // string
getValue(user, "age");  // number
// getValue(user, "id"); // ❌
```