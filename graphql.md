# [graphql](https://graphql.org/)

- GraphQL comes with a set of default scalar types out of the box:
  - Int: A signed 32‐bit integer.
  - Float: A signed double-precision floating-point value.
  - String: A UTF‐8 character sequence.
  - Boolean: true or false.
  - ID: The ID scalar type represents a unique identifier, often used to refetch an object or as the key for a cache. The ID type is serialized in the same way as a String; however, defining it as an ID signifies that it is not intended to be human‐readable.

---

## [How To GraphQL](https://www.howtographql.com)

### Introduction

- Enables declarative data fetching.
- GraphQL server exposes a single endpoint and responds to queries.
- Increased mobile usage creates need for efficient data loading.
- Variety of different front end frameworks and platforms on the client-side.
- Fast development speed and expectation for rapid feature development.
- GraphQL is not only for React developers, it can be used with any programming language and framework.

### GraphQL is the Better REST

- Gread ideas in REST: Stateless servers and structures access to resources.
- REST is a strict specification: But the concept was wildly interpreted.
- Rapidly changing requriements on client side dont go well with the static nature of REST.
- GraphQL was developed to cope with the need for more flexibiliy and efficiency in client-server communication.
- GraphQL exposes only a single endpoint, and it fetches everything you need in a single request.
- No more over-under fetching:
  - Overfetching - Downloading unnecessary data.
  - Underfetching - An endpoint doesn't return enough of the right information; need to send mulitple requests (n+1 requests problem).
- REST: Structure endpoints accoding to clients' data needs.
- No need to adjust API when product requirements and design change.
- Faster feedback cycles and product iterations.
- Fine-grained insights on what data is read by clients.
- Enables evolving API and deprecating unneeding API features.
- Great opportunities for instrumenting and performance monitoring.
- GraphQL uses strong type system to define capabilities of an API.
- Schema serves as a contract between client and server.
- Frontend and backend teams can work completely independent from each other once they both know the schema.

### Core Concepts

- The Schema Definition Language (SDL)
```
type Person {
  name: String!
  age: Int!
  posts: [Post!]!
}

type Post {
  title: String!
  author: Person!
}
```
- The `!` means the field is required.
- The `[]` indicates the value is a list (or array).
- Example client query:
```
{
  allPersons {
    name
    age
  }
}
```
- It would return a list of all the Persons' names and age from the database:
```
{
  "allPersons": [
    { "name": "Sam" , "age": 26 },
    { "name": "Kat" , "age": 25 }
  ]
}
```
- Only return the last 2 people that have been inserted into the database:
```
{
  allPersons(last: 2) {
    name
    age
  }
}
```
- Return all people's names and their list of posts:
```
{
  allPersons {
    name
    posts {
      title
    }
  }
}
```
- Changes made to the db using GraphQL are called Mutations.
- There are 3 kinds of mutations:
  - Creating new data.
  - Updating existing data.
  - Deleting existing data.
- Example Mutation:
```
mutation {
  createPerson(name: "Bob", age: 26) {
    name
    age
  }
}
```
- The server will respond with:
```
{
  "createPerson": {
    "name: "Bob",
    "age": 26
  }
}
```
- Realtime updates with Subscriptions
```
subscription {
  newPerson {
    name
    age
  }
}
```
- Whenever a `newPerson` is queried, the subscriber will be notified.
- Subscription represents a stream of data to the client.
- The GraphQL Schema.
  - Defines capabilities of the API by specifying how a client can fetch and update data.
  - Represents a contract between client and server.
  - Collection of GraphQL types with special root types.
- Root Types:
```
type Query {
  ...
}
type Mutation {
  ...
}
type Subscription {
  ...
}
```
- Creating a CRUD API:
```
// SCHEMAS
type Query {
  allPersons(last: Int): [Person!]!
  allPosts(last: Int): [Post!]!
}
type Mutation {
  createPerson(name: String!, age: String!): Person!
  updatePerson(id: ID!, name: String!, age: String!): Person!
  deletePerson(id: ID!): Person!
  createPost(title: String!): Post!
  updatePost(id: ID!, title: String!): Post!
  deletePost(id: ID!): Post!
}
type Subscription {
  newPerson: Person!
  updatedPerson: Person!
  deletedPerson: Person!
  newPost: Post!
  updatedPost: Post!
  deletedPost: Post!
}

// QUERIES
type Person {
  id: ID!,
  name: String!
  age: Int!
  posts: [Post!]!
}

type Post {
  id: ID!
  title: String!
  author: Person!
}
```

### Big Picture (Architecture)

- GraphQL is only a specification.
- Architecture has 3 use cases:
  - GraphQL server with a connected database:
    - Often used for greenfield projects.
    - Uses single web server that implements GraphQL.
    - Server resolves queries and constructs response with data that it fetches from the database.
    - GraphQL doesn't care about database type (SQL or NoSQL-based databases).
  - GraphQL server to integrate existing system:
    - Compelling use case for companies with legacy infrastuctures and many different APIs.
    - GraphQL can be used to unify existing systems and hide complexity of data fetching logic.
    - The server doesn't care about the data sources are (databases, web servers, etc.).
  - A hybrid approach with a connected database and integration of existing system.
- Resolver functions.
- GraphQL queries/mutations consist of a set of field.
- GraphQL server has one resolver function per field.
- The purpose of each resover is to retrieve the data for its corresponding field:
```
query {
  User(id: "34ku5g3") {
    name
    friends(first: 5) {
      name
      age
    }
  }
}
// resolvers executed
User(id: String!): User
name(user: User!): String!
age(user: User!): Int!
friends(first: Int, user: User!): [User!]!
```
- GraphQL Clients:
  - GraphQL is great for front end developer as data fetching complexity can be pushed to the server-side.
  - Client doesn't care where the data is coming from.
- Imperative methodology (REST):
  - From imperative to declarative data fetching.
  - Construct and send HTTP request (e.g. fetch in JavaScript).
  - Receive and parse server response.
  - Store data locally.
  - Display the data in the UI
- Declarative methodology (GraphQL):
  - Describe data requirements.
  - Display information in the UI.

- Every GraphQL schema has three special root types, these are called `Query`, `Mutation` and `Subscription`.
- The fields on these root types are called root field and define the available API operations.
- In general, when adding a new feature to the API, the process will look pretty similar every time:
  - Extend the GraphQL schema definition with a new root field (and new data types, if needed).
  - Implement corresponding resolver functions for the added fields.
- Filtering
```
// schema
type Query {
  info: String!
  feed(filter: String): [Link!]!
}

// resolver
async function feed(parent, args, context, info) {
  const where = args.filter ? {
    OR: [
      { description_contains: args.filter },
      { url_contains: args.filter },
    ],
  } : {}

  const links = await context.prisma.links({
    where
  })
  return links
}

// query
query {
  feed(filter:"QL") {
    id
  	description
    url
    postedBy {
      id
      name
    }
  }
}
```
- Pagination
```
// schema
type Query {
  info: String!
  feed(filter: String, skip: Int, first: Int): [Link!]!
}

// resolver
async function feed(parent, args, context, info) {
  const where = args.filter ? {
    OR: [
      { description_contains: args.filter },
      { url_contains: args.filter },
    ],
  } : {}

  const links = await context.prisma.links({
    where,
    skip: args.skip,
    first: args.first
  })
  return links
}

// query
query {
  feed(
    first: 1
    skip: 1
  ) {
    id
    description
    url
  }
}
```
- Sorting
```
// schema
enum LinkOrderByInput {
  description_ASC
  description_DESC
  url_ASC
  url_DESC
  createdAt_ASC
  createdAt_DESC
}

type Query {
  info: String!
  feed(filter: String, skip: Int, first: Int, orderBy: LinkOrderByInput): [Link!]!
}

// resolver
async function feed(parent, args, context, info) {
  const where = args.filter ? {
    OR: [
      { description_contains: args.filter },
      { url_contains: args.filter },
    ],
  } : {}

  const links = await context.prisma.links({
    where,
    skip: args.skip,
    first: args.first,
    orderBy: args.orderBy
  })
  return links
}

// query
query {
  feed(orderBy: createdAt_ASC) {
    id
    description
    url
  }
}
```
