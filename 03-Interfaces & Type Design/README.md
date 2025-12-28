# Module 03: Interfaces & Type Design

The goal of this module is to understand how to **define, extend, and structure types** in TypeScript for scalable and maintainable code.  
By the end of this module, you should be able to design complex type systems for applications, APIs, and libraries.

---

## 1. Interfaces

Interfaces define the **shape of an object**.

```ts
interface User {
  id: number;
  name: string;
  email: string;
}
```

## 2. Extending Interfaces

Interfaces can extend other interfaces to compose types.

```ts
interface BaseEntity {
  id: string;
  createdAt: Date;
}

interface User extends BaseEntity {
  name: string;
  email: string;
}

const user: User = {
  id: "1",
  createdAt: new Date(),
  name: "User",
  email: "user@example.com"
};
```

## 3. Index Signatures

Index signatures allow dynamic property keys with a specific type.

```ts
interface ErrorBag {
  [key: string]: string;
}

const errors: ErrorBag = {
  email: "Invalid email",
  username: "Username required"
};
```

## 4. Type Aliases vs Interfaces
TypeScript provides **interfaces** and **type aliases** to define types.  
Both can describe object shapes, but there are key differences.

| Feature | Interface | Type Alias |
|---------|-----------|------------|
| Extendable | ✅ Can extend other interfaces | ❌ Cannot extend (use intersections instead) |
| Objects | ✅ | ✅ |
| Primitives | ❌ Cannot define primitive types | ✅ Can define primitives (e.g., `string`, `number`) |
| Unions/Intersections | ❌ | ✅ Can define unions or intersections |

```ts
type ID = string | number;

let userId: ID;

userId = 101;      // ✅
userId = "abc123"; // ✅
```

## 5. Index Access Types

**Index Access Types** allow you to access the type of a specific property
from another type using bracket notation (`[]`).

### Basic Index Access

```ts
type User = {
  id: number;
  name: string;
  isActive: boolean;
};

type UserName = User["name"];
```

### Accessing Multiple Properties

```ts
type UserPreview = User["id" | "name"];
```

## 6. Conditional Types

**Conditional types** allow TypeScript to choose a type
based on a condition, similar to an `if-else` statement but at the **type level**.

Syntax:

```ts
type IsString<T> = T extends string ? true : false;

type A = IsString<string>; // true
type B = IsString<number>; // false
```