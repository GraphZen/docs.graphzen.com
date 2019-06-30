---
title: "GraphQL 101: Understanding the basics"
draft: true
menu:
  handbook:
    name: "GraphQL 101"
    parent: "get-started"
---

The purpose of [GraphQL 101](/handbook/get-started/graphql-101) is to give an overview of the features and benefits GraphQL and the GraphQL ecosystem. If you're already familiar with GraphQL and want to get started building GraphQL APIs in .NET, check out the [Quickstart Tutorial](/tutorials/quick-start/).

## What is GraphQL?

GraphQL consists of both a query language and API server. The API server hosts a _GraphQL schema_ that represents your application's data model and the available operations (_GraphQL mutations_) that your application is capable of performing.

For fetching data or executing operations, the GraphQL query language allows clients and users to easily and simply express what part of an application's data is needed or what operation(s) to execute. The GraphQL server will execute the query and provide the results in a JSON document for ready use in the client application.

For defining the GraphQL schema (your application's data model), GraphQL provides a simple but expressive type system consisting of objects, interfaces, enums and more.

## Features and Benefits

The combination of a simple query language and a simple type system to describe your application data improves the flexibility, expressiveness, and productivty for users of your API while vastly reducing both the up-front and ongoing cost and complixity to design, implement, and document your API.

### Queries

GraphQL's query language provides a simple, powerful and flexible mechanism for fetching data. Queries are easily written and human readable. The API server type-checked by the server to ensure what is being requested is valid and provide clear and precise messages when there are any errors.

- **What you query is what you get** - By design, GraphQL queries look almost like JSON. Queries specify what field(s)/objects from the schema to select. The result from the GraphQL server is a JSON document with a symmetrical shape to the query, providing a workflow that is easy to reason about during development and when reviewing or reading code.

- **Just what you need, when you need it** - As your API grows and data types begin to contain more fields, you don't need to be concerned about the impact of bandwidth or complexity for users of your API. The results of queries remain the same, only reterning the specific objects and fields that were originally requested - keeping client code simple, focused, and efficient.

- **One request, multiple resources** - With an effective schema design (made possible by the GraphQL type system), you no longer have to peform multiple client/server round-trips to fetch the data needed to render a single component or screen. Based on the provided requestd query, the GraphQL server will dynamically join the required data types and providing a single response. Special purpose endpoints, APIs, view models, or query parameters are replaced by the expressiveness of the GraphQL query language. In essence, the GraphQL query is defining the view model or projection that works for you - often without any modifications needed on the server.

```graphql
{
  hero {
    name
    height
    mass
  }
}
```

```json
{
  "hero": {
    "name": "Luke Skywalker",
    "height": 1.72
  }
}
```

Learn more about [how to build a GraphQL schema](/handbook/concepts/schema) and [support querying operations with GraphZen](/handbook/concepts/queries).

### Mutations

Mutations provide the mechanism to perform operations on the server. They can be thought of like application commands or remote method calls and are used in most cases where an HTTP POST or PUT operation would be supported in a RESTful API.

- **Command-query responsiblity seperation (CQRS)** - While CQRS can be embodied at many different architectural layers, GraphQL queries and mutations embody the essence of CQRS at the application layer by providing a clear seperation between fetching and querying data and the operations that operate on the data.
- **Describe what your API can _do_** - in addition to _what_ data youra application has, the core capabilities of your application is often what it can _do_. Mutations provide a first class, unambiguously clear, and distinct mechanism in the GraphQL schema for doing this. No longer are users guessing between a POST/PUT, or what data is required in the URL or the HTTP body - a GraphQL mutation's name, arguments, and response are all clearly defined and inspectable.
- **Get back relevant data** - Mutations can return any output type from your schema, making it possible to both perform an operation and retrieve any data (including nested objects/resources) from the returned data type.

Learn more about [how mutations work and how to implement them in .NET with GraphZen](/handbook/concepts/mutations).

### Type System

The GraphQL type system provides a lightweight but expressive way to describe the schema (data model) for your application. The type system is composed of [objects](/handbook/type-system/objects), [interfaces](/handbook/type-system/interfaces), [union types](/handbook/type-system/interfaces), [enums](/handbook/type-system/enums), [scalars](/handbook/type-system/scalars), [input objects](/handbook/type-system/input-objects), as well as metadata in the form of [directives](/handbook/type-system/directives).

- **Describe your domain** - with the mechanisms for querying and executing operations already handled by the GraphQL query language and server, the schema design is both constrained (and free) to focus on describing your application domain. Instead of thinking about query parameters, URLs, request/response headers and formats, you are free to focus on the concepts that are closest to your domain - facilitating thought, communication, design and implementation of both client and server features.

- **Built-in API Documentation** - The schema for any API server - the types, mutations, as well as any associated directives - can be automatically introspected. Descriptions can be attached to any element in the GraphQL schema, providing documentation and clarifying comments to users of your API.

- **Backward Compatible API Evolution** - Because of the stronlgy-typed aspect of GraphQL, changes can be made while also checking to ensure it does not introduce backward compatible changes with previous versions of the schema. Continuing to support obsoleted portions of the schema is often trivial, while at the same time gracefully hiding them for new users of your API.

## History and background

GraphQL was initially conceived and developed at Facebook to address their challenges delivering concise but hetergeous result data to mobile devices. Facebook was not alone in encountering the challenges that led to GraphQL. Netflix in their effort to provide querying flexibility to their large number of clients developed a similar technology called Falcor.

Ultimately Facebook's decision to provide and open source the GraphQL specification led to broad adoption of GraphQL by providing the means to understand and implment the mechanics of GraphQL across many languages and platforms.

While GraphQL and the GraphQL sepcification was one of the first projects open-sourced by Facebook, GraphQL and the GraphQL specification are emerging as a web standard as a distinct project and instiution in the modern open source ecosystem. In March 2019, [a formal partnership between the GraphQL Foundation and the Joint Development Foundation was instituted](https://www.linuxfoundation.org/press-release/2019/03/the-graphql-foundation-announces-collaboration-with-the-joint-development-foundation-to-drive-open-source-and-open-standards/), with GraphZen as a charter member for the specification effort.

<!--
## GraphQL compared to SQL

## GraphQL vs REST

## GraphQL vs RPC

## Working with GraphQL

### GraphQL Clients

### GraphQL Servers

### Development Tools
-->

## Ready to get started?

Get an [introduction to building GraphQL APIs in C# with GraphZen](/handbook/get-started/introduction/) or head over to the [Quickstart Tutorial](/tutorials/quickstart) to build your first API.
