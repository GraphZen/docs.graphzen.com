---
id: unions
title: Union Types
sidebar_label: Unions
weight: 400
menu:
  handbook:
    name: "Unions"
    parent: "type-system"
---

_Union_ types provide a way to describe data contained within a field that could be any number of different object types that may or may not share common fields.

## Modeling Union Types

C# conventions provide two approaches to creating a union type: either by using a C# interface to mark members of a union type or using the base class of any types in the schema.

### C# Marker Interface

A C# marker interface with the `GraphQLUnionType` attribute can be used to indicate members of a union type. The following GraphQL Union type:

```graphql
union SearchResult = Human | Droid | Starship
```

Can be modeled with the following C#:

```csharp
[GraphQLUnionType]
interface SearchResult { }

public class Human : SearchResult {
    /* implementation elided */
}
public class Droid : SearchResult {
    /* implementation elided */
}

public class Starship : SearchResult {
    /* implementation elided */
}
```

The `SearchResult` C# interface is just a marker interface that can be used to indicate a C# class (GraphQL object) is a member of a union type. Using an interface is a good choice if you don't want to use inheritance or your classes don't form a natural inheritance hiearchy.

### C# Base Class

Alternatively, if your C# classes already share a base class it will be represented as a GraphQL union type.

```csharp
public abstract class Animal {
  public string Species { get; set; }
}

public class Dog : Animal  {
  public bool Barks { get; set; }
}

public class Cat : Animal {
  public bool Meows { get; set; }
}
```

The above C# code will result in the following GraphQL schema:

```graphql
union Animal = Dog | Cat

type Dog {
  species: String!
  barks: Boolean!
}

type Cat {
  species: String!
  meows: Boolean!
}
```

### Schema Builder (Fluent API)

Union types can also be configured via the fluent schema builder API:

```csharp
public class PetStoreContext : GraphQLContext {
    public override void OnSchemaCreating(SchemaBuilder schema) {

        schema.Union("Animal")
            .OfTypes("Dog", "Cat");
    }
}
```
