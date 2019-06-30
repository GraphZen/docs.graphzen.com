---
title: Introduction
weight: 100
draft: true
menu:
  handbook:
    parent: "get-started"
---

**GraphZen** is a code-first GraphQL server framework and SDK for .NET that strives to deliver a fantastic developer experience for C# developers building GraphQL APIs.

If you're unfamiliar with GraphQL, check out [GraphQL 101: Understanding the basics](/manual/get-started/graphql-101/). To get up and running quickly with GraphZen, working through the [Quickstart Tutorial](/tutorials/quickstart) is a great way to get started.

## Features

### Code First

GraphZen's powerful conventions automatically translate vanilla C# code into a GraphQL schema. This convention over configuration approach has many benefits:

1. **Express GraphQL APIs in vanilla, idomatic C#**: Continue to leverage the existing developer familiarity, IDE tooling and language features that C# provides. Basic knowledge of GraphQL and C# is all that is required to be productive (and stay productive) building GraphQL APIs with GraphZen.
2. **Zero-config schema evolution for scalable and maintainable GraphQL APIs**: As features are added and the types of data increase in a GraphQL API, having a cohesive, clear, and succinct way to define the schema becomes even more important. If adding or changing the GraphQL schema requires changes in more than one place or configuration is required by default, this slows down the development process and makes the code more difficult to understand.
3. **Familiar development style for users of Entity Framework and ASP.NET MVC**: Developers who have experience with Entity Framework or ASP.NET MVC will find themselves at home working with GraphZen to build GraphQL APIs. In a similar fashion to how Entity Framework enables developers to use C# object model to create a SQL schema, develpers can use GraphZen to translate their C# object models to GraphQL schemas. Instead of a database schema, a GraphQL schema is created with its types exposed as a GraphQL API, much like the controllers/models of an ASP.NET MVC application.

Code first example:

```csharp

public class Profile {
    public string Username { get; set; }
    public int? Age { get; set; }
    public List<Post> Posts { get; set; }
}

public class Post {
    public string Content { get; set; }
    public Profile Author { get; set; }
}

```

### Data Annotations

Code-first conventions may be overriden using data annotations or programtically using the schema builder API. For details on how code first conventions map C# code to GraphQL object types or how to progamatically construct the schema, refer to relevant aspect of the **Type System** documentation (e.g. objects, interfaces, enums, etc.).

```csharp
public class Profile {
    public string Username { get; set; }
    [GraphQLName("age")]
    public int? YearsOld { get; set; }
    public List<Post> Posts { get; set; }
    [GraphQLIgnore]
    public string PasswordHash { get; set; }
}
```

### Schema Builder

While a code first approach works well for most application development scenarios, having the ability to programatically define or manipulate the GraphQL schema - with or without corresponding C# types - provides more flexibility when needed.

Schema builder (configuration first) approach:

```csharp
public class MyApiContext : GraphQLContext {
    public override void OnSchemaCreating(SchemaBuilder schemaBuilder) {
      // This code creates the same GraphQL schema
      // as would have been created with the code-first approach above
      schemaBuilder.Object("Profile")
         .Field("username", "String!")
         .Field("age", "Int")
         .Field("posts", "[Post!]!");

      schemaBuilder.Object("Post")
         .Field("content", "String!")
         .Field("author", "Profile!");
    }
}

```

The schema builder provides a dynamic and flexible way to create and modify the GraphQL schema.

### Type System

Once a GraphQL schema is defined, GraphZen provides a robust and flexible type system in the form of an immutable schema object model. This can be used helpful introspecting your schema with C#, testing, code generation, or debugging.

```csharp
Schema schema = graphQLContext.Schema;

ObjectType profileType = schema.GetType("Profile");

Field ageField = profileType.GetField("age");
```

### Language Model

A key component of GraphQL is the GraphQL language itself. GraphZen provides a complete GraphQL abstract syntax tree, parser, printer, and syntax tree visitors for working with the GraphQL language.

```csharp
var sdl = @"
schema {
  query: Query
}

type Query {
 profiles: [Profile!]!
}

type Profile {
  username: String!
  age: Int
}
";

DocumentSyntax doc = Parser.ParseDocument(sdl);

ObjectTypeDefinitionSyntax profile = doc.Definitions
    .OfType<ObjectTypeDefinitionSyntax>()
    .Single(_ => _.Name.Value == "Profile");
```

## Getting Started: ASP.NET Core

1.  Create a new ASP.NET Core empty web project
2.  Install the the `GraphZen.AspNetCore.App` NuGet package
3.  In the `ConfigureServices` method of your `Startup` class, bootstrap the nescessary GraphQL services to your project by calling the `AddGraphQLContext()` extension method on the `IServicesCollection`.
4.  In the `Configure` method of your `Startup` class, add the GraphQL playground (interactive GraphQL query editor) and the GraphQL API endpoint to your HTTP pipeline by calling `UseGraphQLPlayground()` and `UseGraphQL()` on `IApplicationBuilder`.

    ```csharp
    public class Startup
    {
        public void ConfigureServices(IServiceCollection services)
        {
            services.AddGraphQLContext();
        }

        public void Configure(IApplicationBuilder app, IHostingEnvironment env)
        {
            app.UseGraphQLPlayground();
            app.UseGraphQL();
        }
    }
    ```

5.  Create a `Query` C# class:

    ```csharp
    public class Query {
        public string Message => "Hello world";
    }
    ```

6.  Start your web application and enter the following query into the GraphQL playground query editor:

    ```graphql
    query {
      message
    }
    ```

## Next Steps

- The [Quickstart Tutorial](/tutorials/quickstart) is a great way to get started quickly with GraphZen
- Familiarize yourself with how GraphZen handles basic GraphQL concepts such as [the GraphQL schema](/handbook/concepts/schema), [queries](/handbook/concepts/queries), and [mutations](/handbook/concepts/mutations).
- Check out the additional [tutorials](/tutorials) for more advanced examples and use cases

## Need Help?

- Community support and discussion available at [https://spectrum.chat/graphzen/ask](https://spectrum.chat/graphzen/ask)

```

```
