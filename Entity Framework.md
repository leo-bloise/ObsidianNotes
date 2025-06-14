Entity Framework is a framework that allow C# developers to apply Object Relational Mapping to database entities and work with database data as they were c# objects.

## How it Works
Entity framework works with models. Each model is composed by a context object and its entities. The context object represents a connection between the application and the database while entities represents tables inside the database.
In this case, you can use the context object alongside their entities to perform queries, save, remove or update data as they were simple c# objects.

## Setting Up
When you need to setup EF Core for a project, you need to download the EF Core package provided by the database provider. For example, PostgreSQL provides `Npgsql.EntityFrameworkCore.PostgreSQL` to establish a connection to `PostgreSQL databases`.