# DB schema & migrations

# PostgreSQL basics: tables, keys, indexes

Add documentation and assets for bit manipulation, dotenv usage, Git advanced commands, and rate limiting

- Added SVG asset for bit manipulation operators.
- Created .dotenv documentation explaining value storage with and without quotes.
- Introduced Git stash and rebase concepts in a new markdown file.
- Documented the importance and implementation of rate limiting, including its role in security and fairness.

## Table

- table is a collection of rows (records) and columns (fields)
- And this is how you can create a table in PostgreSQL:
    
    ```sql
    CREATE TABLE users (
    	id SERIAL PRIMARY KEY,
    	name TEXT NOT NULL,
    	email TEXT UNIQUE NOT NULL
    );
    ```
    
    - id SERIAL: auto incrementing integer
    - PRIMARY KEY: uniquely identifies each row
    - TEXT: datatype
    - NOT NULL: value is required
    - UNIQUE: no two values can be the same

## Primary Key

- Must be unique
- must be not null]
- usually called `id`

> PostgreSQL automatically creates an index on primary keys
> 
- indexes helps with fast lookups

## Foreign key

- A foreign key creates a relationship between two tables

Let’s say users can have posts.

```sql
CREATE TABLE posts (
	id SERIAL PRIMARY KEY,
	user_id INT REFERENCES users(id),
	title TEXT NOT NULL,
	body TEXT
);
```

- so the user_id must match with the users.id
- PostgreSQL will prevent invalid data

if we try to insert invalid foreign key

```sql
INSERT INTO posts(user_id, title)
VALUES (99, "Hello world");  
// throws error if no user with user.id does not exists
```

## Indexes

- An index is like a book index that helps you to find rows faster.
- so without index, postgres has to scan the entire table
- and with index, it can jump directly to the right row.

> This is how you can create an index
> 

```sql
CREATE INDEX idx_users_email ON users(email);
```

### When to use index

- searching frequently on a column
- filtering with WHERE
- using JOIN on that column
- unique columns

### When not to use index

- On small tables
- On columns rarely searched
- On columns frequently updated (indexes slows down writes)

## Example workflow

### Step 1 - create tables

```sql
CREATE TABLE users (
	id SERIAL PRIMARY KEY,
	name TEXT NOT NULL,
	email TEXT UNIQUE NOT NULL
);

CREATE TABLE posts (
	id SERIAL PRIMARY KEY,
	user_id INT REFERENCES users(id),
	title TEXT NOT NULL,
	body TEXT
);
```

### Step 2 - Insert new users

```sql
INSERT INTO users (name, email) VALUES 
('Alice', 'alice@mail.com'),
('Bob', 'bob@mail.com');
```

### Step 3 - Insert new post

```sql
INSERT INTO posts (user_id, title, body) VALUES
(1, 'First post', 'Hello'),
(1, 'Another post', 'World'),
(2, 'New post', 'Heyyy');
```

### Step 4 - Query with JOIN

```sql
SELECT users.name, posts.title
FROM users
JOIN posts 
ON users.id = posts.user_id;
```

- `SELECT users.name, posts.title` : it is like saying After you find the matches,  only show me the `name` column from the `users` and `title` column from the `posts` table
- `FROM users` : looking at the users table
- `JOIN posts` : this is the second table you want to connect ( “I want to combine `users` table with the data of `posts` table” )
- `ON [users.id](http://users.id/) = posts.user_id;` : this is one of the most important part. it is the matching rule. it tells the database how to connect the rows.
    - only combine a row from users with a row from `posts` IF the `id` value from the `users` table is exactly the same as the `user_id` value from the `posts` table.

## Database schema design: users, posts, comments

### So we are going to cover these things

1. Entities
2. Relationships
3. Table designs
4. Keys & constraints
5. Indexing strategy
6. example queries

### **Entities**

We need three core entities

1. Users
    - people using the platform
2. Posts
    - the content written by the users
    - each post belongs to a user
3. Comments
    - comments written on posts
    - each comment belongs to:
        - a user
        - a post

### **Relationships**

- Let’s map the cardinality:

| Entity | Relationship |
| --- | --- |
| user → post | 1 to many |
| user → comment | 1 to many |
| post → comment | 1 to many |

![image.png](DB%20schema%20&%20migrations/06330c10-bebe-48d3-b7b7-fb4b09093564.png)

### Table designs (Schema design)

### **users table**

```sql
CREATE TABLE users (
	id SERIAL PRIMARY KEY,
	username TEXT UNIQUE NOT NULL,
	email TEXT UNIQUE NOT NULL,
	created_at TIMESTAMP DEFAULT NOW()
);
```

### posts table

```sql
CREATE TABLE posts (
	id SERIAL PRIMARY KEY,
	user_id INT NOT NULL REFERENCES users(id) ON DELETE CASCADE,
	title TEXT NOT NULL,
	body TEXT,
	created_at TIMESTAMP DEFAULT NOW()
);
```

- `ON DELETE CASCADE` → delete user → delete all their posts

### comments table

```sql
CREATE TABLE comments (
	id SERIAL PRIMARY KEY,
	post_id INT NOT NULL REFERENCES posts(id) ON DELETE ,
	user_id INT NOT NULL REFERENCES users(id) ON DELETE,
	body TEXT NOT NULL`,
	created_at TIMESTAMP DEFAULT NOW()	
);
```

- comment belongs to both post and user
- foreign keys enforces valid references
- cascading ensures no orphan comments

### Indexing strategy

Indexes that postgres creates automatically:

- Primary keys →  users(id), posts(id), comments(id)
- Foreign keys → automatically create indexes on:
    - posts(user_id)
    - comments(user_id)
    - comments(post_id)

### Additional recommended indexes:

- search posts by author

```sql
CREATE INDEX idx_posts_user_id_created
ON posts(user_id, created_at DESC);
```

- get latest comments for a post

```sql
CREATE INDEX idx_comments_post_id_created
ON comments(post_id, created_at DESC);
```

- search for user by username (login)

```sql
CREATE INDEX idx_users_username
ON users(username);
```

### Example queries

1. Get all posts with their author:

```sql
SELECT p.id, p.title, u.username
FROM posts p
JOIN users u
ON p.user_id = u.id
```

1. get comments with commenter name:

```sql
SELECT c.body, u.username
FROM comments c
JOIN users u
ON c.user_id = u.id
```

1. get all comments for a specific post (newest one first)

```sql
SELECT comments.body, users.username, comments.created_at
FROM comments
JOIN users
ON comments.user_id = users.id
WHERE comments.post_id = 1
ORDER BY comments.created_at DESC;
```

1. count comments per post

```sql
SELECT post_id, COUNT(*)
FROM comments
GROUP BY post_id;
```

1. user profile feed (user + their posts + post comment count)

```sql
SELECT 
	u.username, 
	p.title, 
	COUNT(c.id) AS comment_count
FROM users u
JOIN posts p
ON p.user_id = u.id
LEFT JOIN comments c
ON c.post_id = p.id
WHERE u.id = 1
GROUP BY u.username, p.title;
```

## migrations: prisma/drizzle to evolve schema safely

1. **What are migrations?**
- Migrations are controlled changes in your database schema.
    
    For example
    
    - Adding a new column
    - Rename a table
    - create or delete indexes
    - introducing new relations
- Why to even migrate in the first place?
    - So migration helps to keep the track of all the changes made and it guarantees:
        - Consistency
        - Reproducibility(it refers to the ability to achieve consistent results over time, both within a single experiment and across different labs or instruments)
        - Safe Roll-outs
        - works across multiple environments
            - (dev → staging → production)
1. **Conceptual workflow**
    
    step 1 - you modify schema
    
    - Prisma: update `schema.prisma`
    
    step 2 -  the tool generates migration SQL
    
    - Prisma: `prisma migrate dev`
    
    step 3: review and run the migration
    
    - prisma applies SQL with `migrate dev` or `migrate deploy`
    
    step 4: migration is saved in folder
    
    Example structure:
    
    ```sql
    migrations/
      20250101_init.sql
      20250105_add_comments.sql
      20250110_add_indexes.sql
    ```
    
- Each migration is permanent - it basically gives audit history of schema evolution

## Best practices for safe schema evolution

1. Never edit old migration files
    - Because they represent historical truth
2. prefer additive changes
    
    Safe:
    
    - add column
    - add table
    - add index
    
    Risky:
    
    - renaming columns
    - dropping columns
    - changing data types
3. Split risky changes into phases
- Example: `username` → `handle`
    
    **Phase 1:** Add new column
    
    ```sql
    ALTER TABLE users Add COLUMN handle text;
    ```
    
    **Phase 2:** backfill
    
    **Phase 3:** Update application to use new column
    
    **Phase 4:** drop old column later
    
1. Always test migrations in staging
2. Never run destructive migrations during peak traffic
3. Put indexes in separate migrations

## Query optimization with indexes and explain plans

### 1. How PostgreSQL executes queries

Postgres doesn’t run your SQL directly.

instead it:

- parses the SQL
- builds a plan - a strategy to retrieve data
- execute the plan

the plan depends on:

- Table size
- statistics
- indexes
- join types
- filters

### 2. The index types

B-tree (default)

used for:

- equality (=)
- ranges (>, <, BETWEEN)
- ordering (ORDER BY)

### 3. Query optimization basics

indexes speed up:

- WHERE filters
- JOIN conditions
- ORDER BY
- GROUP BY
- UNIQUE constraints

But indexes slows down when use

- INSERT
- UPDATE
- DELETE

So indexes should be applied to the column you actually run

### 4. Optimizing JOINs

```sql
SELECT u.username, p.title
FROM users u
JOIN posts p
ON p.user_id = u.id
WHERE u.id = 10;
```

OK so JOINs are only fast if both sides have the right indexes.

- users.id →  auto-indexed (primary key)
- posts.user_id → needs an index (foreign key creates an index automatically)

Now what happens if the Foreign key index is missing

```sql
Nested Loop
	-> Index scan on users
	-> Sequence scan on posts (very bad and takes a lots of time) 
```

After Indexing

```sql
Nested Loop
	-> Index scan on users
	-> Index scan on posts (good)
```

### 5. Optimizing ORDER BY

```sql
SELECT * FROM posts
WHERE user_id = 5
ORDER BY created_at DESC
LIMIT 20;
```

- Retrieve all the posts for the user with ID = 5, sorted by the latest post.

### Multi-column index to avoid sorting

```sql
CREATE INDEX idx_posts_user_created
ON posts(user_id, created_at DESC);
```

- It means

```sql
Index Scan using idx_posts_user_created on posts
```

### **6. Reading real explain plans**

- postgres plans are trees.
- it reads them from bottom → up

```sql
Nested Loop
// the first-one finds the user
-> Index Scan on users (id = 10)
// second scan finds the user's posts
-> Index Scan on posts (user_id = users.id)
```

### 7. Anti-Patterns (slow queries)

- `SELECT *` → slows down the i/o(db input and output operations) because it fetches unnecessary columns
- Functions on indexed columns

```sql
WHERE LOWER(username) = 'alice'
```

- Now this is the fix you can consider

```sql
CREATE INDEX idx_users_lower_username ON users((LOWER(username)));='
```

## **EXPLAIN plans**

`EXPLAIN`→ shows the estimated plan based on statistics

`EXPLAIN ANALYZE` → Runs the query and shows:

- actual run time
- actual rows processed
- buffers (I/O)
- what really happens behind the scenes/h3

### How to read a plan?? (structure)

- Plans are trees:
    - Bottom - nodes that actually access tables
    - top - nodes that combine results (joins, sorts,  aggregates)

Example:

```sql
Nested Loop
	-> Index Scan on users
	-> Index Scan on posts
```

1. PostgreSQL finds users
2. For each user → find posts via index
3. combine rows via nested loop

## **Mini-project: blog db with migrations + rollback**

## schema/migration using prisma

```sql
npm init -y
npm install prisma @prisma/client
npx prisma init
```

### Step 1 - migration 1: create users

```sql
model User {
  id        Int      @id @default(autoincrement())
  username  String   @unique
  email     String   @unique
  createdAt DateTime @default(now())

  posts     Post[]
  comments  Comment[]
}
```

```sql
npx prisma migrate dev --name init
```

### Step 2 - migration 2: create posts

```sql
model Post {
  id        Int       @id @default(autoincrement())
  title     String
  body      String?
  createdAt DateTime  @default(now())

  user      User      @relation(fields: [userId], references: [id])
  userId    Int

  comments  Comment[]
}
```

```sql
npx prisma migrate dev --name create_posts
```

### Step 3 - migration 3: create comments

```sql
model Comment {
  id        Int       @id @default(autoincrement())
  body      String
  createdAt DateTime  @default(now())

  user      User      @relation(fields: [userId], references: [id])
  userId    Int

  post      Post      @relation(fields: [postId], references: [id])
  postId    Int
}
```

```sql
npx prisma migrate dev --name create_comments
```

### Step 4: migration 4: indexes

```sql
model Post {
  id        Int      @id @default(autoincrement())
  title     String
  body      String?
  createdAt DateTime @default(now())

  @@index([userId])
  @@index([createdAt])
}

model Comment {
  id        Int      @id @default(autoincrement())
  body      String
  createdAt DateTime @default(now())

  @@index([postId])
}
```

```sql
npx prisma migrate dev --name add_indexes
```

### Step 5: migration 5: likes

```sql
model Like {
  id        Int      @id @default(autoincrement())
  createdAt DateTime @default(now())

  post      Post     @relation(fields: [postId], references: [id])
  postId    Int

  user      User     @relation(fields: [userId], references: [id])
  userId    Int

  @@unique([userId, postId])
}
```

```sql
npx prisma migrate dev --name add_likes
```

### Step 5 - Rollback Migration

Prisma does not let you automatically “Rollback”

but we can:

**Roll back last migration**

```sql
npx prisma migrate reset
```

or you can manually revert by:

- deleting last migration folder
- editing schema
- re-running migrate

### Hard reset (dev only)

```sql
npx prisma migrate reset  --force
```

## Building the Blog API with **Express + Typescript + Prisma + PostgreSQL**.

## Backend does 3 main jobs:

1. Receive the request
    - What does the user wants??
2. Do some work
    - validate inputs
    - check permissions
    - Read/write databases
    - run logic (create post,  update comment, find user etc
3. Send a response
    - send back: data, status codes, error messages, etc.

### For example: user asks to “show all its posts”

backend Steps:

- receive request
- validate → is user allowed?
- query DB → SELECT  * FROM posts
- format data
- return data in JSON format

### For example: user asks to “create a new comment”

backend steps:

- receive request
- validate → body must be not empty
- check if the post exists
- insert comment in DB
- return the created comment

### Another solid example: user hits “Like this post”

Steps:

1. Receive the request
2. Validate + authenticate (is the user logged in?)
3. check if the post exists (otherwise 404)
4. Insert a like into the database
5. return the success response

### DB data storage analogy

### users table

what it stores?

- Who is performing the action

so what are the fields that must exist?

- id
- username
- email
- password

### posts table

what it stores?

- content

fields

- id
- title
- body
- author

### likes table

stores the relation between a user and a post

fields:

- user_id
- post_id
- timestamp

```sql
user_id = 9999
post_id = 55
```

Now what if we allow this row in `likes`

> when likes.user_id always points to the real user, the db can guarantee:
> 
1. No ghost likes
    - no likes from user who don’t exist
2. No broken joins
3. safe deletion
    - if user is deleted → database automatically deletes their likes(because of `ON DELETE CASCADE`)
4. Accurate counts
    - like count always reflects the real users
5. Consistency
    - our data always makes sense

## Imagine this situation

- user tries to like the same post twice
- backend server receives two “like” requests for the same pair

The express route handler should check right, but here is a catch

- even if we check in express it doesn’t guarantee safety because
    - two requests might hit at the same time
    - race conditions can happen (it occurs when two processes access the shared resource at the same time)
    - malicious users can bypass client logic
    - backend bugs may skip the check
    - distributed systems can cause duplicates

## Now come to the main point: Building the actual Blog API using (Express + Prisma)

### Step 1 - Backend foundation: we need a database client

- Express cannot talk to the database on its own
- so we need Prisma Client as the “bridge”

Prisma = Type-safe SQL machine between express ↔ postgres

### Step 2 - Initialize Prisma client

Every express backend has one prisma client instance

because

- It manages DB connections
- creates prepared statements
- caches metadata
- reuses the connection pool

Question: Why do we should have one shared prisma client instead of creating new one for every request?

- creating many clients wastes memory
- too many DB connections → slow or crash
- Prisma recommends a singleton for performance

> Query logging: refers to the process of recording and storing information about the query executed against the DBMS. It can contain information such as query text, warning or error messages, timing
> 

## src/db/prisma.ts

```sql
import { PrismaClient } from "@prisma/client/extension"

const globalForPrisma = global as unknown as {
	prisma: PrismaClient | undefined
}

// log is used for query logging
export const prisma =
	globalForPrisma.prisma ??
	new PrismaClient({
	log: ["query", "info", "warn", "error"],
})

// reconnection logic
if(process.env.NODE_ENV !== "production"){
	globalForPrisma.prisma = prisma
}

// export default prisma
```

## POST /users

```sql
// src/routes/users.ts
import { Router } from "express";
import bcrypt from "bcryptjs";
import { prisma } from "../db/prisma";

const router = Router();

router.post("/", async (req, res) => {
  const { username, email, password } = req.body;

  // TODO: Add Zod validation later

  // Hash password
  const hashedPassword = await bcrypt.hash(password, 10);

  try {
    const user = await prisma.user.create({
      data: {
        username,
        email,
        password: hashedPassword,
      },
      select: {
        id: true,
        username: true,
        email: true,
        createdAt: true,
      }, // do NOT return password
    });

    res.json(user);

  } catch (err) {
    console.error(err);
    res.status(400).json({ error: "User creation failed" });
  }
});

export default router;
```

## POST /posts

```sql
// src/routes/posts.ts
import { Router } from "express";
import { prisma } from "../db/prisma";

const router = Router();

// Create a post
router.post("/", async (req, res) => {
  const { title, body, userId } = req.body;

  // simple validation for now
  if (!title || !userId) {
    return res.status(400).json({ error: "title and userId are required" });
  }

  try {
    const post = await prisma.post.create({
      data: {
        title,
        body,
        userId, // Prisma enforces FK to users.id
      },
      select: {
        id: true,
        title: true,
        body: true,
        userId: true,
        createdAt: true,
      },
    });

    res.json(post);

  } catch (err) {
    console.error(err);
    return res.status(400).json({ error: "Could not create post" });
  }
});

export default router;
```

## GET posts (with pagination and indexing)

we are gonna learn these things:

- How to fetch posts?
- How to use Prisma’s findMany
- how to do pagination the correct way?
- why indexes matter for ORDER BY
- how to structure relationships in responses?

### Endpoint:

GET /posts?page=1&limit=10

### We can think everything in terms of “Features”, not Routes

- Feature: create a post
    - `POST /posts`
- Feature: list posts
    - `GET /posts`
- Feature: delete a post
    - `DELETE /posts/:id`
- Feature: like a post
    - `POST /posts/:id/like`

> Feature → Route → Code
> 

## One trick to make feature work

There are three questions we need to ask:

### Q1 → what does the client needs to send?

- `title`, `body`, `userId`

### Q2 → what database operation should happen?

- `prisma.post.create({….})`

### Q3 → what data should the backend return?

- (the newly created post)

### Let’s use this trick to create a feature/route

### POST /posts  (create a post)

---

Q1. what does the client send?

- title
- body
- userId

Q2. what DB operation?

- prisma.post.create

Q3. what needs to return?

- created post (id, title, body, userId, createdAt)

### Feature → create a comment on a post

what we need?

- Which post
- which user
- comment text/content

POST /posts/:id/comments

### Note

REST convention:

- The resource you are acting on → goes in the URL
- The data for the action → goes in the body

### For example: Create a comment for post 12

POST /posts/12/comments

- URL says which post
- body says who and the content

```sql
import { Router } from "express";
import { prisma } from "../db/prisma.ts";

const router = Router();

router.post("/posts/:id/comments", async (req, res) => {
	const postId = parseInt(req.params.id);
	const { userId, body } = req.body;

	if (!body || !userId) {
		return res.status(400).json({
			error: "Body and userId are required"
		});
	}

	try {
		const comment = await prisma.comment.create({
			data: {
				postId,
				userId,
				body,
			},
			select: {
				id: true,
				postId: true,
				userId: true,
				body: true,
				createdAt: true,
			},
		});
	} catch (error) {
		console.log(error);
		res.status(500).json({
			error: "Failed to create comment",
		});
	}
});

export default router;
```

## Feature: Like a post

POST /posts/:id/like

### Feature: Get a post by ID with all details

GET /posts/:id

things we will have

- postId
- userId
- comments
- likes
- when it was created

## Feature: Get a Single post

GET /posts/12

```sql
router.get("/posts/:id", async (req, res) => {
  const postId = parseInt(req.params.id);

  try {
    const post = await prisma.post.findUnique({
      where: { id: postId },
      select: {
        id: true,
        title: true,
        body: true,
        createdAt: true,
        user: {
          select: {
            id: true,
            username: true,
          },
        },
        _count: {
          select: {
            comments: true,
            likes: true,
          },
        },
      },
    });

    if (!post) return res.status(404).json({ error: "Post not found" });

    res.json(post);

  } catch (err) {
    console.error(err);
    res.status(500).json({ error: "Failed to fetch post" });
  }
});
```

use of “_count”

- to avoid loading huge arrays into memory
- to reduce query time
- because front-end only needs the count

## Feature: GET comments for a post

`GET /posts/:id/comments`

```sql
router.get("/posts/:id/comments", async (req, res) => {
  const postId = parseInt(req.params.id);

  try {
    const comments = await prisma.comment.findMany({
      where: { postId },
      orderBy: { createdAt: "desc" },
      select: {
        id: true,
        body: true,
        createdAt: true,
        user: {
          select: {
            id: true,
            username: true,
          },
        },
      },
    });

    res.json(comments);

  } catch (err) {
    console.error(err);
    res.status(500).json({ error: "Failed to fetch comments" });
  }
});
```

## Database connection pooling

- PostgreSQL allows only a limited number of connections(typically 20 to 100 on cloud DB)
- If our node app creates too many connections:
    - DB will crash
    - API will hang
    - we will receive ‘too many connections’ errors
    - problems occur in impossible

- in production

```sql
npm prisma migrate dev
```

- in local or development

```sql
npm prisma migrate deploy
```