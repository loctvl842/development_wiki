1. Up to now, `joins` is in-consistent.

Query using syntax `include` to include related table's fields, **Prisma** do not use JOIN, it use *WHERE IN (id1, id2)* in the background

2. No support using indexes

3. No migrate down (Prisma generate SQL command to upgrade)

4. Lack of models, validations, and callbacks (Prisma recommend using middleware)

5. Schema doesn't have syntax for writing check constraints.
