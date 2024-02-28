- [Understanding Model-driven Code Generation](#understanding-model-driven-code-generation)
- [How Model-driven Code Generation Works](#how-model-driven-code-generation-works)
   * [Overview of the model-driven code generation process](#overview-of-the-model-driven-code-generation-process)
- [Benefits of Model-driven Code Generation](#benefits-of-model-driven-code-generation)
- [Tools and Frameworks for Model-driven Code Generation](#tools-and-frameworks-for-model-driven-code-generation)
   * [Prisma](#prisma)
      + [Key Features](#key-features)
      + [Using Prisma for Model-driven Code Generation](#using-prisma-for-model-driven-code-generation)
      + [Restriction](#restriction)
   * [SchemaGenius](#schemagenius)
      + [Key Features](#key-features)
      + [Using SchemaGenius for Model-driven Code Generation](#using-schemagenius-for-model-driven-code-generation)
      + [Restriction](#restriction-1)
- [Conclusion](#conclusion)

# Model-driven Code Generation

In the previous article, we discussed the concept of template-based code generation and its potential benefits. In this article, we will explore the concept of model-driven code generation, which is a key aspect of model-driven development.

# Understanding Model-driven Code Generation

Models in software development serve as abstract representations of various aspects of the system being built. These can include data models, architectural models, and behavioral models, among others.

Model-driven development (MDD) is an approach where models take center stage in the software development lifecycle, guiding the design, implementation, and maintenance phases.

# How Model-driven Code Generation Works

Model-driven code generation involves the automatic creation of source code from a higher-level model, typically representing the application's architecture, data model, or business logic.

## Overview of the model-driven code generation process

1. **Models design:** Developers create high-level models using modeling languages such as UML, DBML, ....
2. **Code Generation:** Tools analyze the model and generate code artifacts, such as database schemas, API endpoints, and user interfaces.
3. **Customization:** Generated code may require further customization or enhancement to meet specific requirements.

# Benefits of Model-driven Code Generation

Model-driven code generation offers several advantages, including:

- **Consistency and quality of code:** Since code is derived directly from the model, there is a reduced risk of inconsistencies or errors between the design and implementation phases. There will not be a typo or a missing field in the code.

- **Simplified maintenance and updates:** Changes to the application can be propagated more efficiently by updating the model and regenerating the affected code, streamlining the maintenance process.

- **Facilitation of collaboration between developers and stakeholders:** Models serve as a common language for communication between developers, designers, and stakeholders, fostering collaboration and ensuring alignment with business requirements.

# Tools and Frameworks for Model-driven Code Generation

Some tools and frameworks facilitate model-driven code generation, each offering unique features and capabilities.

## Prisma

Prisma is a modern database toolkit that simplifies database access and management by providing a type-safe and auto-generated query builder. It enables developers to define their data model using a declarative schema language and automatically generates database schemas and client libraries.

### Key Features

1. **Declarative schema language:** Prisma uses a declarative schema language to define the data model, including entities, relationships, and constraints.
2. **Type-safe query builder:** Prisma generates a type-safe query builder that allows developers to interact with the database using a strongly-typed API.
3. **Automatic migrations:** Prisma supports automatic migrations, allowing developers to evolve the database schema over time without manual intervention.
4. **Database Variants:** Prisma supports various databases, including PostgreSQL, MySQL, SQLite, and SQL Server.

### Using Prisma for Model-driven Code Generation

**Step 1: Define the Data Model**
```prisma
// schema.prisma

model User {
  id        Int      @id @default(autoincrement())
  name      String
  email     String   @unique
  posts     Post[]
}

model Post {
  id        Int      @id @default(autoincrement())
  title     String
  content   String
  author    User     @relation(fields: [authorId], references: [id])
  authorId  Int
}
```

**Step 2: Generate Prism Client**

Once the schema is defined, use the Prisma CLI to generate the Prisma Client, which provides a type-safe API for interacting with the database:

```bash
npx prisma generate
```

This command analyzes the schema and generates TypeScript/JavaScript code for the Prisma Client, including models, queries, and mutations.

**Step 3: Use the Prisma Client in the Application**
```typescript
// app.ts

import { PrismaClient } from '@prisma/client';

const prisma = new PrismaClient();

async function createUser(name: string, email: string) {
  return await prisma.user.create({
    data: {
      name,
      email
    }
  });
}

// Usage
createUser('John Doe', 'john@example.com')
  .then(user => console.log('User created:', user))
  .catch(err => console.error('Error creating user:', err))
  .finally(() => prisma.$disconnect());
```

Here, we import the Prisma Client and use its `user.create` method to create a new user in the database. Prisma handles generating the necessary SQL queries and ensuring type safety.

### Restriction

**1. Inconsistent Joins:** Currently, joins in queries using the `include` syntax to include related table fields are inconsistent. Prisma does not use JOINs but rather employs WHERE IN (id1, id2) in the background, leading to potential performance implications in certain scenarios

**2. No Support for Indexes:** Prisma lacks built-in support for defining indexes on database tables, which can impact query performance and data retrieval efficiency in large datasets.

**3. Lack of Migrate Down:** Unlike some other ORM frameworks, Prisma does not support automatic migration rollback (migrate down) out of the box. Developers need to manually handle database schema rollbacks in case of errors or changes.

**4. Lack of Models, Validations, and Callbacks:** Prisma lacks built-in support for defining models, validations, and callbacks, which are commonly found in other ORM frameworks. While Prisma recommends using middleware to address these concerns, it may require additional setup and customization.

**5. Schema Constraints Syntax:** Prisma's schema definition language does not have native syntax for writing check constraints, which are used to enforce data integrity rules at the database level.

## SchemaGenius

This is a small low-code tool built by **loctvl842** (a member at **Rockship**). SchemaGenius is a Visual Studio Code extension designed to streamline the process of generating ORM model code based on a pre-designed database schema. By leveraging the power of the popular database design tool `dbdiagram`, `SchemaGenius` allows developers to translate their database design into executable code using frameworks such as `TypeORM`, `Sequelize`.

### Using SchemaGenius for Model-driven Code Generation

**Step 1: Design the Database Schema**

Go to website `dbdiagram.io` and design the database schema. Then export the design to a file (`.dbml`).

```dbml
Table ingredients {
  id uuid [pk]
  name varchar
}

Table dishes {
  id uuid [pk]
  name varchar
}

Table recipes {
  id uuid [pk]
  name varchar [null]
  dishId uuid [ref: > dishes.id]
}

Table recipeIngredients {
  id uuid [pk]
  step integer
  amount varchar
  ingredientId uuid [ref: > ingredients.id]
  recipeId uuid [ref: > recipes.id]
}
```

**Step 2: Generate ORM Models**

- Install the `SchemaGenius` extension in Visual Studio Code.

- Press `Ctrl + Shift + P` to open the command palette, then select `SchemaGenius: Generate ORM Models`.

- Just follow the instructions and the ORM models will be generated. Read more [here](https://marketplace.visualstudio.com/items?itemName=Rockship-loctvl842.SchemaGenius)

**Step 3: Customize some entities if needed**

`dbdiagram` is a tool to design the database schema. Therefore, there is no relationship names defined. You may need to customize the generated models to have meaningful relationship names.

**Step 4: Use the ORM Models in the Application**

Just use these entities in your application like you are using TypeORM or Sequelize.

This a real project that fully used `SchemaGenius` for code generation. [Here](https://github.com/loctvl842/UCook-BE)

### Restriction

While `SchemaGenius` offers a streamlined solution for generating ORM model code from database designs, espcialy it solved the inconsistency problem of Prisma, it also has some limitations, including:

- **Limited Support for Many-to-Many Relationships:** Up to now, this tool does not support many-to-many relationships. You need to manually create the join table and the relationship between the entities.
- **Limited Maintenance and Support:** As the author of this tool, I must acknowledge that I don't have much time to maintain and support this tool.
- **Dependency on External Tools:** SchemaGenius relies heavily on external tools such as dbdiagram for database design and TypeORM or Sequelize for ORM code generation. Changes or updates to these external tools may impact the functionality and compatibility of SchemaGenius.

# Conclusion

I don't want to make comparision between tools but I want to show that there are many tools that can help you to generate code from the model. Each tool has its own strengths and weaknesses, and the choice of a tool depends on the specific requirements and constraints of the project.

If you are building small to medium-sized applications and prefer a modern, type-safe, and auto-generated query builder, Prisma is a good choice. However, if you are looking for a low-code solution to generate ORM models from a database schema, SchemaGenius is a viable option.

However, it is important to note that most low-code tools, such as Prisma and SchemaGenius, are capable of serving an application without the need to write any code. Nevertheless, it is crucial to acknowledge that the generated code may necessitate additional customization or enhancement in order to meet specific requirements.

Good luck with your code generation journey! Next blog, I will show you how I build a project without coding a single line of code.
