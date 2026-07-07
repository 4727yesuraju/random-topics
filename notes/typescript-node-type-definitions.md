# TypeScript Node.js Type Definitions

## What are Type Definitions?

Type definitions tell TypeScript the **shape** of JavaScript code.

They describe:

- Variables
- Functions
- Classes
- Objects
- Modules

Type definitions help TypeScript provide:

- ✅ Type checking
- ✅ Autocomplete
- ✅ Error detection

Example:

```ts
declare const console: {
  log(message: any): void;
};
```

This tells TypeScript that:

- `console` exists.
- It has a `log()` method.
- `log()` accepts any value.
- `log()` returns `void`.

---

# What are Node.js Type Definitions?

Node.js provides built-in APIs such as:

- process
- Buffer
- fs
- path
- crypto
- http
- \_\_dirname
- \_\_filename

TypeScript doesn't know these APIs by default.

Node.js type definitions describe these APIs so TypeScript understands them.

Example:

```ts
process.env.DATABASE_URL;
```

TypeScript now knows:

- `process` exists.
- `process.env` exists.
- Environment variables are `string | undefined`.

---

# Why are they needed?

Without Node.js types:

```ts
console.log(process.env.PORT);
```

Error:

```
Cannot find name 'process'
```

With Node.js types:

```ts
console.log(process.env.PORT);
```

✅ No error.

---

# What does this line do?

```ts
/// <reference types="node" />
```

It tells TypeScript:

> "Load the Node.js type definitions for this file."

Now TypeScript recognizes:

- process
- Buffer
- \_\_dirname
- \_\_filename
- require
- setTimeout
- clearTimeout

---

# Where do these types come from?

Usually from:

```bash
npm install -D @types/node
```

`@types/node` contains all the `.d.ts` files describing Node.js APIs.

---

# Runtime vs Type Definitions

Node.js Runtime

- Executes your code.
- Provides the actual `process`, `Buffer`, `fs`, etc.

Type Definitions

- Used only by TypeScript.
- Help during development.
- Do **not** exist at runtime.

Think of it as:

Node.js = Actual Object

TypeScript Type Definitions = Blueprint/Documentation

---

# Quick Revision

- Type definitions describe JavaScript APIs.
- They enable type checking and autocomplete.
- Node.js types describe `process`, `Buffer`, `fs`, etc.
- `/// <reference types="node" />` loads Node.js types for the current file.
- `@types/node` provides these type definitions.
- Type definitions are used only by TypeScript and are not included in the compiled JavaScript.
