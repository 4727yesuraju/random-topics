# Drizzle Database Connection

This file creates a connection to the PostgreSQL database and initializes Drizzle ORM.

```ts
import "dotenv/config";
import { drizzle } from "drizzle-orm/node-postgres";
import pg from "pg";

import * as schema from "./schema";

const pool = new pg.Pool({ connectionString: process.env.DATABASE_URL });

export const db = drizzle(pool, { schema });
```

---

# Purpose of Each Line

## 1. Load Environment Variables

```ts
import "dotenv/config";
```

### Purpose

Loads environment variables from the `.env` file.

Example `.env`

```env
DATABASE_URL=postgres://user:password@localhost:5432/mydb
```

Now you can access:

```ts
process.env.DATABASE_URL;
```

Without this import:

```ts
process.env.DATABASE_URL;
```

would be `undefined` unless it was already set in the environment.

---

## 2. Import Drizzle ORM

```ts
import { drizzle } from "drizzle-orm/node-postgres";
```

### Purpose

Imports the `drizzle()` function, which creates a Drizzle ORM instance.

Think of it as:

```
PostgreSQL Connection
        │
        ▼
   drizzle()
        │
        ▼
Database Object (db)
```

Without this, you cannot use Drizzle ORM features.

---

## 3. Import PostgreSQL Driver

```ts
import pg from "pg";
```

### Purpose

Imports the PostgreSQL client library (`pg`).

`pg` is responsible for:

- Connecting to PostgreSQL
- Sending SQL queries
- Receiving query results

Drizzle ORM **uses** `pg` internally to communicate with the database.

Think of it like this:

```
Application
     │
     ▼
Drizzle ORM
     │
     ▼
pg Driver
     │
     ▼
PostgreSQL Database
```

---

## 4. Import Database Schema

```ts
import * as schema from "./schema";
```

### Purpose

Imports all table definitions from `schema.ts`.

Example:

```ts
export const users = pgTable(...);
export const posts = pgTable(...);
```

After importing:

```ts
schema.users;
schema.posts;
```

Drizzle uses this schema to:

- Provide type safety
- Enable autocomplete
- Validate queries

---

## 5. Create a Connection Pool

```ts
const pool = new pg.Pool({
  connectionString: process.env.DATABASE_URL,
});
```

### Purpose

Creates a **connection pool** to the PostgreSQL database.

A connection pool:

- Opens and manages multiple database connections.
- Reuses existing connections instead of creating a new one for every query.
- Improves performance and scalability.

Instead of:

```
Query 1 → New Connection
Query 2 → New Connection
Query 3 → New Connection
```

A pool does:

```
Pool
 ├── Connection 1
 ├── Connection 2
 └── Connection 3

Queries reuse these connections.
```

---

## 6. Create the Drizzle Database Instance

```ts
export const db = drizzle(pool, { schema });
```

### Purpose

Creates the `db` object used throughout your application to interact with the database.

Now you can write:

```ts
await db.select().from(schema.users);
```

instead of raw SQL like:

```sql
SELECT * FROM users;
```

Benefits:

- Type safety
- Autocomplete
- Cleaner code
- Compile-time error checking

---

# Overall Flow

```
.env
 │
 ▼
DATABASE_URL
 │
 ▼
dotenv
 │
 ▼
pg.Pool
 │
 ▼
Database Connection Pool
 │
 ▼
drizzle(pool, { schema })
 │
 ▼
db Object
 │
 ▼
Application Queries
```

---

# Quick Revision

| Code                        | Purpose                                                           |
| --------------------------- | ----------------------------------------------------------------- |
| `import "dotenv/config"`    | Loads environment variables from `.env`.                          |
| `import { drizzle } ...`    | Imports Drizzle ORM to create a database instance.                |
| `import pg from "pg"`       | Imports the PostgreSQL driver used to connect to the database.    |
| `import * as schema`        | Imports all table definitions from `schema.ts`.                   |
| `new pg.Pool(...)`          | Creates a reusable pool of PostgreSQL connections.                |
| `drizzle(pool, { schema })` | Creates the `db` object for executing type-safe database queries. |

---

# Interview Tip

**Q:** Why do we use `pg.Pool` instead of creating a new connection for every query?

**Answer:**

- Reuses database connections instead of opening a new one each time.
- Reduces connection overhead.
- Improves application performance.
- Handles multiple concurrent requests efficiently.
- Prevents exhausting the database's connection limit.
