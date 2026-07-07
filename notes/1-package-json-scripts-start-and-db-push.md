# package.json Scripts: start & db:push

## What are npm Scripts?

Scripts are shortcuts defined inside `package.json`.

Instead of typing long commands every time, you can run them using:

```bash
npm run <script-name>
```

Example:

```json
{
  "scripts": {
    "start": "node dist/index.js",
    "db:push": "drizzle-kit push"
  }
}
```

---

# 1. start Script

```json
"start": "node dist/index.js"
```

Run:

```bash
npm start
```

or

```bash
npm run start
```

Actually executes:

```bash
node dist/index.js
```

### Purpose

Starts the application by running the compiled JavaScript file.

### Flow

```
src/index.ts
      │
      ▼
Compile (tsc)
      │
      ▼
dist/index.js
      │
      ▼
npm start
      │
      ▼
Application Starts
```

---

# 2. db:push Script

```json
"db:push": "drizzle-kit push"
```

Run:

```bash
npm run db:push
```

Actually executes:

```bash
drizzle-kit push
```

### Purpose

Synchronizes the Drizzle schema with the database.

It:

1. Reads `schema.ts`
2. Connects to the database
3. Creates/updates tables and columns to match the schema

### Flow

```
schema.ts
      │
      ▼
npm run db:push
      │
      ▼
drizzle-kit push
      │
      ▼
Database Updated
```

---

# Example

### Before

```ts
export const users = pgTable("users", {
  id: serial("id").primaryKey(),
  name: text("name"),
});
```

Database:

```
users
------
id
name
```

### Add a new column

```ts
export const users = pgTable("users", {
  id: serial("id").primaryKey(),
  name: text("name"),
  email: text("email"),
});
```

Run:

```bash
npm run db:push
```

Database becomes:

```
users
------
id
name
email
```

---

# Why use npm Scripts?

Instead of remembering long commands:

```bash
node dist/index.js
```

or

```bash
drizzle-kit push
```

You simply run:

```bash
npm start
```

or

```bash
npm run db:push
```

This makes commands:

- Easier to remember
- Consistent across the team
- Faster to execute

---

# Quick Revision

| Script            | Executes             | Purpose                                          |
| ----------------- | -------------------- | ------------------------------------------------ |
| `npm start`       | `node dist/index.js` | Starts the compiled application                  |
| `npm run db:push` | `drizzle-kit push`   | Updates the database schema to match `schema.ts` |

---

# Interview Tip

**Q:** Why do we define scripts in `package.json`?

**Answer:**

- To create shortcuts for frequently used commands.
- To standardize project commands across all developers.
- To avoid typing long commands manually.
- To simplify development, testing, building, and deployment workflows.
