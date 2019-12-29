# [mongoDB](https://www.mongodb.com/)

---

## General

- Non relational database.
- Not as ACIDic as other database systems? It argues that it is.

## Setup
- Start mongod at specified location: `mongod --dbpath=/Users/samsverko/Documents/Development/data/db`.
- Access mongoDB shell once mongod is running: `mongo`.
- Clear the prompt: `cls`.
- Start mongod requiring authentication: `mongod --auth --dbpath=/Users/samsverko/Documents/Development/data/`.

## Databases
- Display all databases: `show dbs`.
- Check the current database you are using: `db`.
- To create and use a database, `use database`.


## Collections
- **Collections** are the same as relational database tables.
- Create a collection: `db.createCollection("collectionName");`.
- Display all collections: `show collections`.

## Users
- Display all users: `show users`.
- Create a new user: `db.createUser({user: "sam", pwd: "123ert", roles: [{role: "readWrite", db: "movie"}], mechanisms: ["SCRAM-SHA-1"]});`. This creates the user `sam` with encrypted password `123ert`, and allows them to read and write on the `movie` database.

## CRUD
- Items in collections are called **documents**.
- **Create:** Insert document into collection: `db.actor.insertOne({firstName: "Tom", lastName: "Hanks"});`.
- **Read:** Find all document(s) in collection: `db.actor.find();`. You can pretty-print the results by appending `.pretty()` at the end of the query (`db.actor.find().pretty();`).
- **Read:** Find specific document(s) in collection: `db.actor.find({lastName: "Hanks"})`.

---

# [MongoDB University](https://university.mongodb.com/)

---

## M001 MongoDB Basics

### Chapter 1: Introduction

#### Welcome

- Compass is a visual interface for interacting with MongoDB.
- Flexible schemas.
- Query language.
- MongoDB Atlas (MongoDB as a service platform).
- Discussion forums and TA's to help.

#### Grading and logistics

- Quizzes - ungraded.
- Labs/homework - 50%.
- Final exam - 50%.
- Chapter 1 = week 1. Chapter 2 = week 2. Chapter 3 + final = week 3.
- Must get >= 65% to receive a Statement of Completion.

#### Are you being a firewall?

- No. Hoorah!

#### Connecting to MongoDB using Compass

- Done.

#### Databases, collections, and Documents

- Clusters contain databases.
- Databases serve as namespace for collections.
- Collections store individual records, called documents.
- Specify a collection using `dbName.collectionName`.

#### Exploring datasets in compass

- Once connected, navigating to a database and collection, then viewing the Schema tab can show you how the data is organized in the collection.

#### Documents: Scalar value types

- Type of document field types include: strings, integers, dates, arrays, objects, etc.
- Use the Schema tab in Compass to get a sense of what the field values are.
- Common scalar value types: int32, double, string, and date.

#### MongoDB documents: Fields with documents as values

- Instead of a scalar value (int32, date), MongoDB supports nested Documents as the field type value. This allows for great flexibility of modeling your datasets.
- It seems that MongoDB calls the field value type of object as documents.

#### MongoDB documents: Fields with arrays as values

- It's in the title. The field type value in Documents can be an array.

#### MongoDB documents: Geospatial data

- Confirmed: In MongoDB, the terms Object and Document are used interchangeably.
- For a field type value, it can be an array of Objects. Things can nest that way.

#### Filtering collections with queries

- JSON = JavaScript object notation.
- JSON always starts and ends with curly braces. They are composed of fields. Fields have two parts, a key and a value.
- This query `{'birth year': {$gte: 1985,$lt: 1990}}` tells MongoDB: "Find all documents that have the field 'birth year' with a value that is greater than or equal to 1985 (`$gte`) and less than (`$lt`) 1990.".
- `$gte` and `$lt` keys are operators.

#### Understanding JSON

- JSON representations of MongoDB Documents.
- JSON document/object.
- JSON is easy to read and easy for computers to parse.
- Each field has a key and a value.
- JSON, in other languages are often called objects, structs, maps, or dictionaries.
- All keys must be surrounded in double quotes.
- Keys and values are separated by colons.
- Fields are separated by commas.
- Strings must be double quotes.
- Inside strings is the only time when whitespace is significant in a JSON document.
- Arrays (`[]`) and objects (`{}`) can be values and nested.
