---
id: objects
title: "Object Types"
weight: 200
menu:
  handbook:
    name: "Objects"
    parent: "type-system"
---

GraphQL _Objects_ are used to describe the concrete foundational data structures for querying data from a GraphQL API. They are analagous to C# classes or simple JSON objects. Objects can have a number of typed _fields_ that represent the contents of the data structure. Object fields are analgous to fields, getter properties, and methods in C#.

## Modeling Object Types

GraphQL object types are included or excluded from the schema using a set of conventions that automatically create objects from C# classes or by use of the fluent API available on the schema builder.

### C# Conventions (Code First)

By default, a C# class named `Query` in the same assembly as the derived `GraphQLContext` is used as the root query object type:

```csharp
public class BlogGraphQLContext : GraphQLContext {
}

public class Query {
  public BlogPost GetPostById(int id) {
    // ... implementation elided
  }
}

public class BlogPost {
    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }
}
```

The above C# code will produce the following GraphQL schema:

```graphql
schema {
  query: Query
}

type Query {
  getPostById: BlogPost!
}

type BlogPost {
  id: Int!
  title: String!
  content: String!
}
```

In the above example, the C# class `BlogPost` is automatically added to the schema because it is used as a return type on a field/method of the root `Query` type.

#### Data Annotations

Use the `GraphQLName` attribute to customize the name of an object:

```csharp
[GraphQLName("BlogComment")]
public class Comment {
    public string Comment { get; set; }
}
```

Exclude C# classes from the GraphQL schema using the `GraphQLIgnore` attribute:

```csharp
[GraphQLIgnore]
public class BlogPermission {
    public string BlogSecret { get; set; }
}
```

### Schema Builder (Fluent API)

Objects can also be added or excluded from the schema using the schema builder:

```csharp
public class BlogContext : GraphQLContext {
    public override void OnSchemaCreating(SchemaBuilder schema) {
        // Create objects dynamically
        schema.Object("CustomObject")
            .Field("message", "String", field => field.Resolve(() => "Hello World"));

        // Exclude types or override conventions
        schema.Object<BlogPermission>().Ignore();
    }
}
```
