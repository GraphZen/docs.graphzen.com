---
title: Interface Types
weight: 300
menu:
  handbook:
    name: "Interfaces"
    parent: "type-system"
---

_Interfaces_ in GraphQL describe a set of fields that objects can implement. Objects can implement multiple interfaces. Client applications can use knowledge of interfaces to build generic helpers or components that work with a variety of object types by relying on interfaces.

## Modeling Interface Types

Any C# interface that is implemented by a class in the GraphQL schema becomes an interface in the GraphQL schema. Interfaces can also be configured or excluded using the schema builder's fluent API.

### C# Conventions (Code First)

The C# following classes and interfaces will result in a similar GraphQL types:

```csharp
public interface ICharacter {
  string GivenName { get; set; }
}

public class Droid : ICharacter {
  public string GivenName { get; set; }
  public string DroidFunction { get; set; }
}

public class Human : ICharacter {
  public string GivenName { get; set; }
  public string FamilyName { get; set; }
}
```

As you can see, the mapping to a GraphQL schema is simple:

```graphql
interface ICharacter {
  givenName: String!
}

type Droid implements ICharacter {
  givenName: String!
  droidFunction: String!
}

type Human implements ICharacter {
  givenName: String!
  familyName: String!
}
```

### Schema Builder (Fluent API)

Interfaces can also be added, excluded or configured from the schema using the schema builder:

```csharp
public class BlogContext : GraphQLContext {
    public override void OnSchemaCreating(SchemaBuilder schema) {
        // Create interfaces dynamically
        schema.Interface("Named")
            .Field("name", "String");

        schema.Object("Person")
            .Interfaces("Named")
            .Field("name", "String", field => field.Resolve(() => "John"));

        // Exclude types or override conventions
        schema.Interface<IUndesiredInterface>().Ignore();
    }
}
```
