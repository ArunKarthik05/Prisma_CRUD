# Prisma_CRUD
Prisma ORM is a powerful tool for working with databases in Node.js applications. It provides a type-safe and auto-generated query builder for TypeScript and JavaScript. Integrating Prisma ORM with Express.js is a great choice for building scalable and maintainable backend applications. Here's a basic guide to get you started:

1. **Setting Up Prisma**:
   - First, install Prisma CLI globally by running:
     ```
     npm install -g prisma
     ```
   - Initialize a new Prisma project in your project directory:
     ```
     prisma init
     ```
   - Follow the prompts to set up your database connection. Prisma supports various databases like PostgreSQL, MySQL, and SQLite.

2. **Defining Data Models**:
   - Create a `.prisma` file in your project directory. This file will contain your data models in Prisma Schema Language (PSL).
   - Define your data models like so:
     ```prisma
     // schema.prisma
     model User {
       id        Int      @id @default(autoincrement())
       username  String
       email     String   @unique
       posts     Post[]
     }

     model Post {
       id        Int      @id @default(autoincrement())
       title     String
       content   String
       author    User?    @relation(fields: [authorId], references: [id])
       authorId  Int?
     }
     ```
   - After defining your models, run `prisma generate` to generate Prisma Client.

3. **Using Prisma Client in Express.js**:
   - Install required dependencies:
     ```
     npm install @prisma/client express body-parser
     ```
   - Set up Express.js in your project if you haven't already.
   - Import and use Prisma Client in your Express.js application:
     ```javascript
     const express = require('express');
     const bodyParser = require('body-parser');
     const { PrismaClient } = require('@prisma/client');

     const prisma = new PrismaClient();
     const app = express();

     app.use(bodyParser.json());

     app.post('/users', async (req, res) => {
       const { username, email } = req.body;
       const newUser = await prisma.user.create({
         data: {
           username,
           email,
         },
       });
       res.json(newUser);
     });

     app.get('/users', async (req, res) => {
       const users = await prisma.user.findMany();
       res.json(users);
     });

     // Add other routes and logic as needed

     const PORT = process.env.PORT || 3000;
     app.listen(PORT, () => {
       console.log(`Server is running on port ${PORT}`);
     });
     ```

4. **Running Your Application**:
   - Run your Express.js application using:
     ```
     node app.js
     ```

Creating a model in Prisma involves defining the structure of your data using the Prisma Schema Language (PSL). Here are the basics you should know to create a model:

1. **Model Definition**:
   - Define a model using the `model` keyword followed by the name of the model.
   - Inside the model block, specify the fields along with their data types.

   Example:
   ```prisma
   model User {
     id        Int       @id @default(autoincrement())
     username  String
     email     String    @unique
     age       Int?
     createdAt DateTime  @default(now())
   }
   ```

2. **Field Definitions**:
   - Each field within a model represents a property of your data.
   - Specify the name of the field followed by its data type.
   - You can optionally add additional attributes like `@id`, `@unique`, `@default`, or `@relation`.

3. **Primary Key (`@id`)**:
   - Use the `@id` attribute to specify a field as the primary key.
   - Prisma requires at least one field to be marked as `@id`.

4. **Auto-increment (`@default(autoincrement())`)**:
   - Use `@default(autoincrement())` to automatically generate values for an integer primary key.

5. **Unique Constraint (`@unique`)**:
   - Use `@unique` to ensure that the values in a field are unique across all records.

6. **Optional Fields**:
   - Use the `?` suffix after the data type to denote that a field is optional.
   - Optional fields can have a value or be null.

7. **Default Values (`@default`)**
   - Use `@default(value)` to specify a default value for a field.
   - Commonly used with `DateTime` fields to set the creation/update timestamps automatically.

8. **Relationships (`@relation`)**:
   - Define relationships between models using the `@relation` attribute.
   - Specify the related field and reference field using `fields` and `references` respectively.

   Example:
   ```prisma
   model Post {
     id        Int      @id @default(autoincrement())
     title     String
     content   String
     author    User?    @relation(fields: [authorId], references: [id])
     authorId  Int?
   }
   ```

   In this example, `author` field establishes a relationship between `Post` and `User` models.

9. **Custom Types**:
   - You can define custom data types using `enum` or `type` keywords.
   - Enums represent a predefined set of values.
   - Types define reusable structures for your data.

   Example:
   ```prisma
   enum Role {
     USER
     ADMIN
   }
   ```

   ```prisma
   type Address {
     street  String
     city    String
     country String
   }
   ```

10. **Generated Prisma Client**:
    - After defining your models, run `prisma generate` to generate Prisma Client.
    - Prisma Client provides a type-safe API for interacting with your database based on your defined models.

These are the basic concepts you should know to create models using Prisma Schema Language. As you gain more experience, you can explore advanced features and optimizations offered by Prisma.
**Read & Write data into the database**
import { PrismaClient } from '@prisma/client'

const prisma = new PrismaClient()
async function main() {
  await prisma.user.create({
    data: {
      name: 'Alice',
      email: 'alice@prisma.io',
      posts: {
        create: { title: 'Hello World' },
      },
      profile: {
        create: { bio: 'I like turtles' },
      },
    },
  })

  const allUsers = await prisma.user.findMany({
    include: {
      posts: true,
      profile: true,
    },
  })
  console.dir(allUsers, { depth: null })
}
