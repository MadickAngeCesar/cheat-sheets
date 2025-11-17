# Prisma ORM Cheat Sheet - Beginner to Pro

## Table of Contents
- [Setup & Installation](#setup--installation)
- [Prisma Schema](#prisma-schema)
- [Models](#models)
- [Relations](#relations)
- [Migrations](#migrations)
- [Prisma Client](#prisma-client)
- [CRUD Operations](#crud-operations)
- [Queries](#queries)
- [Filtering & Sorting](#filtering--sorting)
- [Pagination](#pagination)
- [Aggregation](#aggregation)
- [Transactions](#transactions)
- [Raw Queries](#raw-queries)
- [Middleware](#middleware)
- [Seeding](#seeding)
- [Best Practices](#best-practices)

---

## Setup & Installation

### Installation
```bash
# Install Prisma CLI as dev dependency
npm install prisma --save-dev

# Install Prisma Client
npm install @prisma/client

# Initialize Prisma
npx prisma init

# This creates:
# - prisma/schema.prisma
# - .env file
```

### Project Structure
```
project/
├── prisma/
│   ├── schema.prisma
│   ├── migrations/
│   └── seed.ts
├── .env
└── src/
    └── index.ts
```

---

## Prisma Schema

### Basic Schema
```prisma
// prisma/schema.prisma

generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"  // or "mysql", "sqlite", "sqlserver", "mongodb"
  url      = env("DATABASE_URL")
}

model User {
  id        Int      @id @default(autoincrement())
  email     String   @unique
  name      String?
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}
```

### Database Providers
```prisma
// PostgreSQL
datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

// MySQL
datasource db {
  provider = "mysql"
  url      = env("DATABASE_URL")
}

// SQLite
datasource db {
  provider = "sqlite"
  url      = "file:./dev.db"
}

// SQL Server
datasource db {
  provider = "sqlserver"
  url      = env("DATABASE_URL")
}

// MongoDB
datasource db {
  provider = "mongodb"
  url      = env("DATABASE_URL")
}
```

---

## Models

### Field Types
```prisma
model Example {
  // Scalar types
  id        Int      @id @default(autoincrement())
  uuid      String   @id @default(uuid())
  email     String   @unique
  name      String?
  age       Int
  price     Float
  isActive  Boolean  @default(true)
  data      Json
  content   Bytes
  
  // Date types
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
  birthDate DateTime
  
  // BigInt
  bigNumber BigInt
  
  // Decimal (for precise decimal values)
  amount    Decimal
}
```

### Field Attributes
```prisma
model User {
  id        Int      @id @default(autoincrement())  // Primary key
  email     String   @unique                         // Unique constraint
  name      String?                                  // Optional field
  role      String   @default("user")               // Default value
  createdAt DateTime @default(now())                // Default to current time
  updatedAt DateTime @updatedAt                     // Auto-update on change
  
  @@unique([email, name])    // Composite unique constraint
  @@index([email])            // Index
  @@map("users")             // Custom table name
}
```

### Enums
```prisma
enum Role {
  USER
  ADMIN
  MODERATOR
}

model User {
  id    Int    @id @default(autoincrement())
  name  String
  role  Role   @default(USER)
}
```

---

## Relations

### One-to-Many
```prisma
model User {
  id    Int    @id @default(autoincrement())
  name  String
  posts Post[]
}

model Post {
  id       Int    @id @default(autoincrement())
  title    String
  author   User   @relation(fields: [authorId], references: [id])
  authorId Int
}
```

### One-to-One
```prisma
model User {
  id      Int      @id @default(autoincrement())
  name    String
  profile Profile?
}

model Profile {
  id     Int    @id @default(autoincrement())
  bio    String
  user   User   @relation(fields: [userId], references: [id])
  userId Int    @unique
}
```

### Many-to-Many
```prisma
// Implicit many-to-many
model Post {
  id   Int    @id @default(autoincrement())
  title String
  tags Tag[]
}

model Tag {
  id    Int    @id @default(autoincrement())
  name  String
  posts Post[]
}

// Explicit many-to-many (with join table)
model Post {
  id       Int        @id @default(autoincrement())
  title    String
  postTags PostTag[]
}

model Tag {
  id       Int        @id @default(autoincrement())
  name     String
  postTags PostTag[]
}

model PostTag {
  post      Post     @relation(fields: [postId], references: [id])
  postId    Int
  tag       Tag      @relation(fields: [tagId], references: [id])
  tagId     Int
  createdAt DateTime @default(now())
  
  @@id([postId, tagId])
}
```

### Relation Attributes
```prisma
model Post {
  id       Int    @id @default(autoincrement())
  title    String
  author   User   @relation(fields: [authorId], references: [id], onDelete: Cascade)
  authorId Int
}

// onDelete options:
// - Cascade: Delete related records
// - Restrict: Prevent deletion
// - NoAction: Do nothing
// - SetNull: Set foreign key to null
// - SetDefault: Set to default value
```

---

## Migrations

### Migration Commands
```bash
# Create migration
npx prisma migrate dev --name init

# Apply migrations
npx prisma migrate deploy

# Reset database
npx prisma migrate reset

# View migration status
npx prisma migrate status

# Create migration without applying
npx prisma migrate dev --create-only

# Generate Prisma Client
npx prisma generate

# Format schema file
npx prisma format

# Validate schema
npx prisma validate

# Open Prisma Studio
npx prisma studio
```

---

## Prisma Client

### Client Setup
```typescript
// src/db.ts
import { PrismaClient } from '@prisma/client';

const prisma = new PrismaClient();

export default prisma;

// With logging
const prisma = new PrismaClient({
  log: ['query', 'info', 'warn', 'error'],
});

// With custom log levels
const prisma = new PrismaClient({
  log: [
    { emit: 'event', level: 'query' },
    { emit: 'stdout', level: 'error' },
    { emit: 'stdout', level: 'info' },
    { emit: 'stdout', level: 'warn' },
  ],
});

prisma.$on('query', (e) => {
  console.log('Query:', e.query);
  console.log('Duration:', e.duration, 'ms');
});
```

### Connection Management
```typescript
// Connect explicitly
await prisma.$connect();

// Disconnect
await prisma.$disconnect();

// Usage pattern
async function main() {
  try {
    // Your database operations
    const users = await prisma.user.findMany();
  } catch (error) {
    console.error(error);
  } finally {
    await prisma.$disconnect();
  }
}

main();
```

---

## CRUD Operations

### Create
```typescript
// Create single record
const user = await prisma.user.create({
  data: {
    email: 'john@example.com',
    name: 'John Doe',
  },
});

// Create with relations
const user = await prisma.user.create({
  data: {
    email: 'john@example.com',
    name: 'John Doe',
    posts: {
      create: [
        { title: 'Post 1', content: 'Content 1' },
        { title: 'Post 2', content: 'Content 2' },
      ],
    },
  },
  include: {
    posts: true,
  },
});

// Create many
const users = await prisma.user.createMany({
  data: [
    { email: 'john@example.com', name: 'John' },
    { email: 'jane@example.com', name: 'Jane' },
  ],
  skipDuplicates: true,  // Skip duplicates based on unique constraints
});
```

### Read
```typescript
// Find many
const users = await prisma.user.findMany();

// Find unique
const user = await prisma.user.findUnique({
  where: { id: 1 },
});

const user = await prisma.user.findUnique({
  where: { email: 'john@example.com' },
});

// Find first
const user = await prisma.user.findFirst({
  where: { name: 'John' },
});

// Find or throw
const user = await prisma.user.findUniqueOrThrow({
  where: { id: 1 },
});

const user = await prisma.user.findFirstOrThrow({
  where: { name: 'John' },
});

// Select specific fields
const users = await prisma.user.findMany({
  select: {
    id: true,
    email: true,
    name: true,
  },
});

// Include relations
const users = await prisma.user.findMany({
  include: {
    posts: true,
    profile: true,
  },
});

// Nested includes
const users = await prisma.user.findMany({
  include: {
    posts: {
      include: {
        comments: true,
      },
    },
  },
});
```

### Update
```typescript
// Update single record
const user = await prisma.user.update({
  where: { id: 1 },
  data: {
    name: 'Jane Doe',
  },
});

// Update many
const result = await prisma.user.updateMany({
  where: { isActive: false },
  data: { isActive: true },
});

// Upsert (update or create)
const user = await prisma.user.upsert({
  where: { email: 'john@example.com' },
  update: { name: 'John Updated' },
  create: {
    email: 'john@example.com',
    name: 'John Doe',
  },
});

// Increment/Decrement
const post = await prisma.post.update({
  where: { id: 1 },
  data: {
    views: { increment: 1 },
    likes: { decrement: 1 },
  },
});
```

### Delete
```typescript
// Delete single record
const user = await prisma.user.delete({
  where: { id: 1 },
});

// Delete many
const result = await prisma.user.deleteMany({
  where: { isActive: false },
});

// Delete all
const result = await prisma.user.deleteMany();
```

---

## Queries

### Filtering
```typescript
// Equals
const users = await prisma.user.findMany({
  where: { name: 'John' },
});

// Not equals
const users = await prisma.user.findMany({
  where: { name: { not: 'John' } },
});

// In array
const users = await prisma.user.findMany({
  where: { id: { in: [1, 2, 3] } },
});

// Not in array
const users = await prisma.user.findMany({
  where: { id: { notIn: [1, 2, 3] } },
});

// Comparison operators
const users = await prisma.user.findMany({
  where: {
    age: { gt: 18 },      // Greater than
    age: { gte: 18 },     // Greater than or equal
    age: { lt: 65 },      // Less than
    age: { lte: 65 },     // Less than or equal
  },
});

// String filters
const users = await prisma.user.findMany({
  where: {
    name: { contains: 'John' },        // Contains
    name: { startsWith: 'John' },      // Starts with
    name: { endsWith: 'Doe' },         // Ends with
    name: { mode: 'insensitive' },     // Case-insensitive
  },
});

// Null checks
const users = await prisma.user.findMany({
  where: {
    name: { not: null },   // Not null
    name: null,            // Is null
  },
});
```

### Logical Operators
```typescript
// AND
const users = await prisma.user.findMany({
  where: {
    AND: [
      { age: { gte: 18 } },
      { age: { lte: 65 } },
    ],
  },
});

// OR
const users = await prisma.user.findMany({
  where: {
    OR: [
      { email: 'john@example.com' },
      { name: 'John' },
    ],
  },
});

// NOT
const users = await prisma.user.findMany({
  where: {
    NOT: {
      age: { lt: 18 },
    },
  },
});

// Complex queries
const users = await prisma.user.findMany({
  where: {
    OR: [
      { AND: [{ age: { gte: 18 } }, { age: { lte: 30 } }] },
      { AND: [{ age: { gte: 40 } }, { age: { lte: 50 } }] },
    ],
  },
});
```

### Relation Filters
```typescript
// Has relation
const users = await prisma.user.findMany({
  where: {
    posts: {
      some: {  // At least one post
        published: true,
      },
    },
  },
});

// All relations match
const users = await prisma.user.findMany({
  where: {
    posts: {
      every: {  // All posts
        published: true,
      },
    },
  },
});

// No relations match
const users = await prisma.user.findMany({
  where: {
    posts: {
      none: {  // No posts
        published: false,
      },
    },
  },
});

// Is (one-to-one)
const users = await prisma.user.findMany({
  where: {
    profile: {
      is: {
        bio: { contains: 'developer' },
      },
    },
  },
});

// Is not (one-to-one)
const users = await prisma.user.findMany({
  where: {
    profile: {
      isNot: null,
    },
  },
});
```

---

## Filtering & Sorting

### Sorting
```typescript
// Sort ascending
const users = await prisma.user.findMany({
  orderBy: { name: 'asc' },
});

// Sort descending
const users = await prisma.user.findMany({
  orderBy: { createdAt: 'desc' },
});

// Multiple sort fields
const users = await prisma.user.findMany({
  orderBy: [
    { name: 'asc' },
    { createdAt: 'desc' },
  ],
});

// Sort by relation
const users = await prisma.user.findMany({
  orderBy: {
    posts: {
      _count: 'desc',
    },
  },
});
```

---

## Pagination

### Offset Pagination
```typescript
// Skip and take
const users = await prisma.user.findMany({
  skip: 10,
  take: 10,
});

// Page-based pagination
const page = 2;
const pageSize = 10;

const users = await prisma.user.findMany({
  skip: (page - 1) * pageSize,
  take: pageSize,
});

// With total count
const [users, total] = await Promise.all([
  prisma.user.findMany({
    skip: (page - 1) * pageSize,
    take: pageSize,
  }),
  prisma.user.count(),
]);
```

### Cursor Pagination
```typescript
// First page
const firstPage = await prisma.user.findMany({
  take: 10,
  orderBy: { id: 'asc' },
});

// Next page
const lastUser = firstPage[firstPage.length - 1];
const nextPage = await prisma.user.findMany({
  take: 10,
  skip: 1,  // Skip the cursor
  cursor: { id: lastUser.id },
  orderBy: { id: 'asc' },
});
```

---

## Aggregation

### Count
```typescript
// Count all
const count = await prisma.user.count();

// Count with filter
const count = await prisma.user.count({
  where: { isActive: true },
});

// Count relations
const users = await prisma.user.findMany({
  include: {
    _count: {
      select: { posts: true },
    },
  },
});
```

### Aggregate Functions
```typescript
// Aggregate
const result = await prisma.user.aggregate({
  _count: { id: true },
  _avg: { age: true },
  _sum: { age: true },
  _min: { age: true },
  _max: { age: true },
});

// With filter
const result = await prisma.user.aggregate({
  where: { isActive: true },
  _avg: { age: true },
});
```

### Group By
```typescript
const result = await prisma.user.groupBy({
  by: ['role'],
  _count: { id: true },
  _avg: { age: true },
});

// With filter
const result = await prisma.user.groupBy({
  by: ['role'],
  where: { isActive: true },
  having: {
    age: { _avg: { gt: 25 } },
  },
  _count: { id: true },
  _avg: { age: true },
});
```

---

## Transactions

### Sequential Transactions
```typescript
// Using $transaction with array
const [user, post] = await prisma.$transaction([
  prisma.user.create({
    data: { email: 'john@example.com', name: 'John' },
  }),
  prisma.post.create({
    data: { title: 'My Post', authorId: 1 },
  }),
]);
```

### Interactive Transactions
```typescript
// Using $transaction with callback
const result = await prisma.$transaction(async (tx) => {
  const user = await tx.user.create({
    data: { email: 'john@example.com', name: 'John' },
  });
  
  const post = await tx.post.create({
    data: {
      title: 'My Post',
      authorId: user.id,
    },
  });
  
  return { user, post };
});

// With options
const result = await prisma.$transaction(
  async (tx) => {
    // Your operations
  },
  {
    maxWait: 5000,      // Max wait time to acquire a connection
    timeout: 10000,     // Max transaction execution time
    isolationLevel: 'Serializable',  // Transaction isolation level
  }
);
```

---

## Raw Queries

### Raw SQL
```typescript
// Execute raw query
const users = await prisma.$queryRaw`
  SELECT * FROM "User" WHERE age > ${18}
`;

// Execute raw (unsafe - for dynamic queries)
const tableName = 'User';
const users = await prisma.$queryRawUnsafe(
  `SELECT * FROM "${tableName}" WHERE age > $1`,
  18
);

// Execute raw (no return value)
await prisma.$executeRaw`
  UPDATE "User" SET isActive = ${true} WHERE age < ${18}
`;

// Execute raw unsafe
await prisma.$executeRawUnsafe(
  `UPDATE "User" SET isActive = $1 WHERE age < $2`,
  true,
  18
);
```

---

## Middleware

### Query Middleware
```typescript
prisma.$use(async (params, next) => {
  const before = Date.now();
  const result = await next(params);
  const after = Date.now();
  
  console.log(`Query ${params.model}.${params.action} took ${after - before}ms`);
  
  return result;
});

// Soft delete middleware
prisma.$use(async (params, next) => {
  if (params.model === 'Post') {
    if (params.action === 'delete') {
      // Change to update
      params.action = 'update';
      params.args['data'] = { deleted: true };
    }
    if (params.action === 'deleteMany') {
      params.action = 'updateMany';
      if (params.args.data !== undefined) {
        params.args.data['deleted'] = true;
      } else {
        params.args['data'] = { deleted: true };
      }
    }
  }
  return next(params);
});
```

---

## Seeding

### Seed Script
```typescript
// prisma/seed.ts
import { PrismaClient } from '@prisma/client';

const prisma = new PrismaClient();

async function main() {
  // Delete all data
  await prisma.post.deleteMany();
  await prisma.user.deleteMany();
  
  // Create users
  const user1 = await prisma.user.create({
    data: {
      email: 'john@example.com',
      name: 'John Doe',
      posts: {
        create: [
          { title: 'Post 1', content: 'Content 1' },
          { title: 'Post 2', content: 'Content 2' },
        ],
      },
    },
  });
  
  console.log({ user1 });
}

main()
  .catch((e) => {
    console.error(e);
    process.exit(1);
  })
  .finally(async () => {
    await prisma.$disconnect();
  });
```

### Package.json
```json
{
  "prisma": {
    "seed": "ts-node prisma/seed.ts"
  }
}
```

### Run Seed
```bash
npx prisma db seed
```

---

## Best Practices

### Connection Management
```typescript
// ✅ Good: Singleton pattern
// lib/prisma.ts
import { PrismaClient } from '@prisma/client';

const globalForPrisma = global as unknown as { prisma: PrismaClient };

export const prisma =
  globalForPrisma.prisma ||
  new PrismaClient({
    log: ['query'],
  });

if (process.env.NODE_ENV !== 'production') globalForPrisma.prisma = prisma;
```

### Error Handling
```typescript
// ✅ Good: Handle Prisma errors
import { Prisma } from '@prisma/client';

try {
  await prisma.user.create({
    data: { email: 'test@example.com' },
  });
} catch (error) {
  if (error instanceof Prisma.PrismaClientKnownRequestError) {
    if (error.code === 'P2002') {
      console.log('Unique constraint failed');
    }
  }
  throw error;
}
```

### Performance
```typescript
// ✅ Good: Select only needed fields
const users = await prisma.user.findMany({
  select: { id: true, name: true, email: true },
});

// ✅ Good: Use pagination
const users = await prisma.user.findMany({
  take: 10,
  skip: 0,
});

// ✅ Good: Use indexes
// In schema.prisma:
// @@index([email])
// @@index([createdAt, status])

// ✅ Good: Batch operations
const users = await prisma.user.createMany({
  data: usersData,
});
```

---

**Pro Tip:** Use Prisma Studio for database management, leverage transactions for complex operations, always use migrations for schema changes, and optimize queries with proper indexes and field selection!