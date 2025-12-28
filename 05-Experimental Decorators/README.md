## Experimental Decorators

**Decorators** are a special feature in TypeScript used to
**add metadata or modify behavior** of classes, methods, properties,
or parameters.

⚠️ Decorators are **experimental** and must be explicitly enabled.

---

## 1. Enabling Decorators

In `tsconfig.json`:

```json
{
  "compilerOptions": {
    "experimentalDecorators": true,
    "emitDecoratorMetadata": true
  }
}
```

## 2. What Are Decorators?

A decorator is a function that runs at definition time, not runtime usage.

```ts
function Logger(target: Function) {
  console.log("Class created:", target.name);
}
```

## 3. Class Decorator

```ts
function LogClass(constructor: Function) {
  console.log("Logging class:", constructor.name);
}

@LogClass
class User {
  name = "User";
}
```

Runs when the class is defined.

## 4. Method Decorator

```ts
function LogMethod(
  target: any,
  propertyKey: string,
  descriptor: PropertyDescriptor
) {
  console.log(`Method: ${propertyKey}`);
}

class UserService {
  @LogMethod
  getUser() {
    return "User";
  }
}
```

## 5. Property Decorator

```ts
function Readonly(target: any, propertyKey: string) {
  Object.defineProperty(target, propertyKey, {
    writable: false
  });
}

class Config {
  @Readonly
  apiKey = "secret";
}
```

## 6. Parameter Decorator

```ts
function LogParam(
  target: any,
  propertyKey: string,
  parameterIndex: number
) {
  console.log(`Parameter at index ${parameterIndex}`);
}

class UserController {
  createUser(@LogParam name: string) {}
}
```

## 7. Decorator Factory

A decorator that accepts arguments.

```ts
function Role(role: string) {
  return function (constructor: Function) {
    console.log(`Role assigned: ${role}`);
  };
}

@Role("admin")
class AdminUser {}
```
