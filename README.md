# 1. Understanding `any`, `unknown`, and `never` in TypeScript

### `any`: Turn Off Type Checking

The `any` type tells TypeScript to stop checking the value’s type. It disables all type safety for that variable.

```ts
let data: any = "hello";
data = 42;         // ✅ allowed
data = {};         // ✅ allowed

console.log(data.foo.bar); // ✅ no error at compile time ❌ may crash at runtime
```

> ⚠️ **Avoid using `any`** unless absolutely necessary. It defeats the purpose of using TypeScript.

---

### `unknown`: Type-Safe Alternative to `any`

The `unknown` type means “we don’t know what this is yet.” You **must** narrow it down before using it.

```ts
let value: unknown = "hello";

// ✅ Type narrowing required
if (typeof value === "string") {
  console.log(value.toUpperCase());
}

// ❌ Error: Object is of type 'unknown'
console.log(value.toUpperCase());
```

> ✅ Use `unknown` when you expect the value to vary, but still want TypeScript to enforce type checks.

---

### `never`: Represents Impossible Code Paths

The `never` type is used when something should never happen:

- Functions that throw errors
- Functions that never return
- Exhaustive checks in unions

```ts
function throwError(message: string): never {
  throw new Error(message);
}

function handle(value: string | number) {
  if (typeof value === "string") {
    console.log("String");
  } else if (typeof value === "number") {
    console.log("Number");
  } else {
    // This block should be unreachable
    const _exhaustiveCheck: never = value;
  }
}
```

> ✅ `never` is useful for **exhaustiveness checking** in `switch` or `if` blocks.

---


# 2. Union vs Intersection Types in TypeScript: When to Use `|` and `&`

### Union Types (`|`): A Value of One Type or Another

Use union types when a variable can be **one of multiple types**.

```ts
type Result = "success" | "failure" | "pending";

function printStatus(status: Result) {
  if (status === "success") {
    console.log("Operation succeeded");
  }
}
```

Union types work with objects too:

```ts
type Cat = { meow: () => void };
type Dog = { bark: () => void };

function makeSound(animal: Cat | Dog) {
  if ("meow" in animal) {
    animal.meow();
  } else {
    animal.bark();
  }
}
```

---

### Intersection Types (`&`): Combine Multiple Types into One

Use intersection types when a variable must satisfy **multiple types at once**.

```ts
type User = { name: string };
type Admin = { role: string };

type AdminUser = User & Admin;

const admin: AdminUser = {
  name: "Alice",
  role: "superadmin"
};
```

> ✅ Intersection types are ideal for composing multiple interfaces into one.

---

## 🧩 Conclusion

| Type         | Description                              | Use Case Example                                    |
|--------------|------------------------------------------|-----------------------------------------------------|
| `any`        | Disables type checking                   | Worst case fallback                                 |
| `unknown`    | Requires type narrowing                  | Safer alternative to `any`                          |
| `never`      | Represents impossible code               | Exhaustiveness checks or error-throwing functions   |
| `A \| B`     | Accepts type A **or** type B             | Flexible input handling                             |
| `A & B`      | Requires both type A **and** type B      | Composing complex types                             |
