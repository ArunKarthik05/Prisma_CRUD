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

This is just a basic example to get you started. Prisma offers more advanced features like transactions, migrations, and more, which you can explore further in their documentation.
