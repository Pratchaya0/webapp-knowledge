Using Package:

```bash
npm i prisma --save-dev
npx prisma init --datasource-provider sqlite (sqlite is option)
```

ref: [prisma](https://www.prisma.io/docs/accelerate/getting-started)

Migration CLI:

```bash
npx prisma migrate dev --name init
```

**Example Code:**

```bash
import { PrismaClient } from "@prisma/client";
// Declare
const prisma = new PrismaClient();
```

```bash
// Create user
const user = await prisma.user.create({
    data: {
        name: "test2",
        email: "test2@gmail.com"
    }
});

// List User
const user = await prisma.user.findMany()

// Create user and post
const user = await prisma.user.create({
    data: {
        name: "test3",
        email: "test3@gmail.com",
        posts: {
            create: {
                title: "Hello, Post"
            }
        }
    }
});

// List user (Include option)
const userWithPost = await prisma.user.findMany({
    include: {
        posts: true,
    },
});

// Update user
const user = await prisma.user.update({
        data: {
        name: "John",
        posts: {
            create: {
            title: "Hello, Post update",
            },
        },
    },
    where: {
        id: 2,
        email: "test1@gmail.com"
    },
});

// Delete user
const user = await prisma.user.delete({
    where: {
        id: 1,
    },
});
```
