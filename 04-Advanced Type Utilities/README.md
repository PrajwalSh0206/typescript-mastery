# Module 04: Advanced Type Utilities

The goal of this module is to explore **TypeScript’s built-in utility types** and learn how to use them to **write safer, cleaner, and reusable code**.  
By the end of this module, you should be comfortable transforming and manipulating types in real-world applications.

---

## 1. `Partial<Type>`

`Partial` makes all properties of a type optional.

```ts
interface User {
  id: string;
  name: string;
  email: string;
}

const updateUser: Partial<User> = {
  name: "Updated Name"
};
```

### 2. Required<Type>

Required makes all optional properties required.

```ts
interface UserProfile {
  name: string;
  age?: number;
}

const profile: Required<UserProfile> = {
  name: "User",
  age: 30 // must include age now
};
```

### 3. Readonly<Type>

Readonly makes all properties immutable.

```ts
interface Config {
  apiUrl: string;
}

const config: Readonly<Config> = {
  apiUrl: "https://api.example.com"
};

// config.apiUrl = "new-url"; // ❌ Error
```

### 4. Pick<Type, Keys>

Pick selects specific properties from a type.

```ts
interface User {
  id: string;
  name: string;
  email: string;
}

type UserPreview = Pick<User, "id" | "name">;

const preview: UserPreview = {
  id: "1",
  name: "User"
};
```

### 5. Omit<Type, Keys>

Omit removes specific properties from a type.

```ts
type UserWithoutEmail = Omit<User, "email">;

const user: UserWithoutEmail = {
  id: "1",
  name: "User"
};
```

### 6. Record<Keys, Type>

Record creates an object type with specific keys and value types.

```ts
type Roles = "admin" | "user" | "guest";

const rolePermissions: Record<Roles, string[]> = {
  admin: ["read", "write", "delete"],
  user: ["read", "write"],
  guest: ["read"]
};
```

### 7. Mapped Types

Mapped types allow transforming all properties of a type.

```ts
type User = {
  id: string;
  name: string;
  email: string;
};

type ReadonlyUser = {
  readonly [K in keyof User]: User[K];
};
```

✅ Creates a type with all readonly properties.