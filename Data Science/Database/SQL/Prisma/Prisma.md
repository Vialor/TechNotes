# Migration Workflow
## Concepts

> **Database migrations:** are a controlled set of changes that modify and evolve the structure of your database schema.

**Model/Entity-first migration**
![[Untitled 7.png|Untitled 7.png]]
**Database-first migration**
![[Untitled 1 2.png|Untitled 1 2.png]]
## Prisma Migration

> **Prisma Migration:** a database migration tool that supports the **model/entity-first  
>  migration  
> **pattern to manage database schemas in your local environment and in production.
1. Manually adjust your **Prisma Data Model (**`**schema.prisma**`**)**
2. Migrate your development database using the `prisma migrate dev` CLI command. This will generate:
    
    **prisma/migrations:** history of db schema change, and thereby updates the database schema
    
    **Prisma Client:** an object for backend codes to make db operations. It exists in node_modules, can also use `prisma generate` to update Prisma Client based on `schema.prisma`.
    
3. Push your changes to the feature pull request, in CI system `npx prisma migrate deploy`
4. Merge your application code from the feature branch to your main branch, in CI system `npx prisma migrate deploy`
  
**Migration Table** `**_prisma_migrations**`**:** metadata for migrations that have been applied to the database
  
# Often-Used commands
`db seed` seed data into database
### Development-focused commands
**Schema Drift:** the case where the actual schema in the live database (the actual state) is different from the source of truth (the desired state).
**Shadow Database:** is created and deleted automatically each time you run a development-focused command and is primarily used to detect problems such as schema drift. To be brief, it is a backup in case the development-focused commands break anything.
`migrate dev` for use in development environments only, requires shadow database
Apply all migrations, then create and apply any new migrations
`migrate reset` for development environments only
Reset database