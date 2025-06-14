Entity Framework is a framework that allow C# developers to apply Object Relational Mapping to database entities and work with database data as they were c# objects.

## How it Works

Entity framework works with models. Each model is composed by a context object and its entities. The context object represents a connection between the application and the database while entities represents tables inside the database.

In this case, you can use the context object alongside their entities to perform queries, save, remove or update data as they were simple c# objects.

## Setting Up

When you need to setup EF Core for a project, you need to download the EF Core package provided by the database provider. For example, PostgreSQL provides `Npgsql.EntityFrameworkCore.PostgreSQL` to establish a connection to `PostgreSQL databases`. After this, you can follow to the context creation.

You must create a derive class from `DbContext` to create your own context. Inside of it, you can declare your entities as public properties of the context. The context itself represent a connection with the database, but it isn't the real connection.
### DbContext

DbContext instances are single unit-of-works. It means that it must be created, operations must be applied and, then, it must be disposed. When using it with ASP.NET Core, you need to declare a public constructor that expects an db context option. This parameter holds all the configuration needed for connection between your application and the database, for example, the connection string. Now, you must use the asp.net container to inject the context. Just like this:

```c#
builder.Services.AddDbContext<TaskManagerDbContext>(options =>  
{  options.UseNpgsql(builder.Configuration.GetConnectionString("DefaultConnection"))
});
```

Now, you're able to use it and inject it in your controllers or other stuff.

### Entities

Entities are mapped to database tables through metadata. This metadata could be provided by data annotations or using the Fluent API. Besides, `EF Core` may also use some conventions to automatically maps properties and stuff to the database.

> FluentAPI has the biggest precedence, so, if you use both data annotations and fluent api, fluent api will override configurations done by data annotations.

Entities can be found following three methods:
- Directly attaching them to the context through `DbSet` properties.
- Specifying them through the `FluentAPI`.
- Specifying them as a property of an already mapped entity.

You can exclude these mappings through fluent api or data annotations, but I'll not cover this for now.

Each column in the table is mapped to a public property in the entity class. They must have the same name to work out of the box, or it needs to have the same name defined by the fluent api or model

### Entity Inheritance
By Default, entities that derives from a base class can be mapped without mapping the base class directly. For example, you may have a base class with some properties and a derived class with other properties. The derived class will be mapped for entity framework context if it's declared in the `DbSet` or through fluent api without mapping the base type, but it'll still applies the inheritance concept, so any fields declared and mapped by EF in the base type will be mapped as columns of the derived type.

#### Data Annotations
Data annotations must be applied directly to the entity class to configure or change the default behaviour for Entity Framework. For example, we can use `Table` in the entity class to configure the name of the table of that entity.

```c#
using System.ComponentModel.DataAnnotations.Schema;  
using Microsoft.EntityFrameworkCore;  
  
namespace TaskManager.Model;  
  
[Index(propertyName: "Username", IsUnique = true), Index(propertyName: "Email", IsUnique = true)]  
[Table("users")]  
public class User : BaseEntity  
{  
    [Column("username")]  
    public string Username { get; set; }  
    [Column("password")]  
    public string Password { get; set; }  
    [Column("email")]  
    public string Email { get; set; }  
}
```

We can also use `Column` to determine the name of the columns.