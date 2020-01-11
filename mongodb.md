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

#### Databases, collections, and documents

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

---

### Chapter 2: The MongoDB query language + Atlas

#### Introduction to CRUD

- Create
- Read
- Update
- Delete

#### Installing the mongo shell (OSX / Linux)

- Start mongo without a database: `mongo --nodb`.

#### Connecting to our class Atlas cluster from the mongo shell

Example command: `mongo "mongodb://cluster0-shard-00-00-jxeqq.mongodb.net:27017,cluster0-shard-00-01-jxeqq.mongodb.net:27017,cluster0-shard-00-02-jxeqq.mongodb.net:27017/test?replicaSet=Cluster0-shard-0" --authenticationDatabase admin --ssl --username m001-student --password m001-mongodb-basics`.

#### Creating an Atlas sandbox cluster

- https://cloud.mongodb.com/

#### Connecting to your sandbox cluster from the mongo shell

- Check mongo shell version: `mongo --version`.
- Connect the mongo shell to the Atlas cluster: `mongo "mongodb+srv://sam-m001-36yjp.mongodb.net/test"  --username <username>`.

#### Loading data into your sandbox cluster

- On your local machine, navigate to where you have your `sampleData.js` file.
- Connect to your cluster (see previous lesson).
- To load data into your cluster: `load("sampleData.js")`.

#### Connecting to your sandbox cluster from compass

- On Atlas, navigate to your cluster.
- Select the primary shard, and copy the shard name (e.g. `sam-m001-shard-00-00-36yjp.mongodb.net`).
- Connect using Compass connection form.

#### Creating documents: insertOne()

- To insert something into the database: `db.collectionName.insertOne({key: value, key: value});`.

#### Creating documents: insertMany()

- To insert many things into the database: `db.moviesScratch.insertMany([{}, {}]);`. This method will stop once an error has occured.
- To insert many, but have it continue if an error occurs on one item: `db.moviesScratch.insertMany([{}, {}], {"ordered": false});`.
- We can create items by updating non-existing items, this is called upserts.

#### Reading documents: scalar fields

- Selectors in MongoDB filters are `AND`ed together by default: `{key: value, key: value}`.
- To find items in a collections: `db.movies.find({mpaaRating: "PG-13"}).pretty();`.
- To find nested items in a collection (Compass): `{"wind.direction.angle": 290}`.
- To find nested items in a collection (shell):`db.data.find({"wind.direction.angle": 290}).pretty();`.
- To count the number of items that match your query: `db.movieDetails.count({rated: "PG-13", "awards.nominations": 10});`. This returns a number of how many, no other data is returned.

#### Reading documents: array fields

- To find items that match an array exactly: `db.movies.find({cast: ["Jeff Bridges", "Tim Robbins"]}).pretty();`.
- To find items that have this item in their array field: `db.movies.find({cast: "Jeff Bridges"}).pretty();`.
- To find items that have a specific value in a specific place in their array field: `db.movies.find({"cast.0": "Jeff Bridges"}).pretty();`.

#### Cursors

- The find method returns a cursor. A cursor is a pointer to the current location in a result set.
- If there are many returned, they will be returned in batches.

#### Projections

- Projections reduce network and processing requirements by limiting the fields that are returned in results documents.
- To limit the results to only show one of their fields: `db.movies.find({genre: "Action, Adventure"}, {title: 1});`. This will also display the `_id` field, because that field is returned by default in all projections, unless explicitly exclude it.
- To not return the `_id` field in a projection: `db.movies.find({genre: "Action, Adventure"}, {_id: 0, title: 1});`.
- You can also just exclude fields as such: `db.movies.find({genre: "Action, Adventure"}, {title: 0});`.

#### Updating documents: updateOne()

- To update a document in the database:
```javascript
db.movieDetails.updateOne({
	title: "The Martian"
}, {
	$set: {
		"awards": {
			"wins": 8,
			"nominations": 14,
			"text": "Nominated for 3 Golden Globes. Another 8 wins & 14 nominations."
		}
	}
});
```

#### Operators

- Completely replace or add each field specified: `$set`.
- Completely remove the specified field from a document: `$unset`.
- Increment the value of the field by the specified amount: `$inc`.
```javascript
db.movieDetails.updateOne({
	title: "The Martian"
}, {
	$inc: {
		"tomato.reviews": 3,
		"tomato.userReviews": 25
	}
});
```
- Add elements to an array only if they do not already exist in the set: `$addToSet`.
- More operators can be found in the [MongoDB documentation](https://docs.mongodb.com/manual/reference/operator/update/).

#### Updating documents: updateMany()

- To remove all fields that have null values:
```javascript
db.movieDetails.updateMany({
	rated: null
}, {
	$unset: {
		rated: ""
	}
});
```

#### Upserts

- Updating a non-existing document results in creating a new document.
```javascript
db.movieDetails.updateOne({
	"imdb.id": detail.imdb.id
}, {
	$set: detail
}, {
	upsert: true
});
```

#### Updating documents: replaceOne()

- `replaceOne` replaces the entire document, whereas `updateOne` only updates the specified set of information.
- Replace a document with updated information:
```javascript
// in the mongo shell, create some local data to then be used to replace the document in the db
// create filter variable for finding the document you want
let filter = {title: "House, M.D., Season Four: New Beginnings"}
// create doc variable by finding it in the db
let doc = db.movieDetails.findOne(filter);
// look at the poster field in the document
doc.poster;
// update the poster field
doc.poster = "posterUrl";
// look at the genres field
doc.genres;
// update the genres field
doc.genres.push("TV Series");
// now that the doc is the information we want, replace the doc with the current document in the database
db.movieDetails.replaceOne(filter, doc);
```

#### Deleting documents

- MongoDB supports two kinds of delete methods: `deleteOne` and `deleteMany`.
- Delete one document: `db.reviews.deleteOne({_id: ObjectId("34234532")});`.
- Delete many documents that match the field: `db.reviews.deleteMany({reviewer_id: 759723314});`.

---

### Chapter 3: Deeper dive on the MongoDB query language

#### Introduction to query operators

- CRUD operations require a filter.

#### Comparison operators

- Greater than: `$gt`, such as `db.movieDetails.find({runtime: {$gt: 90}});`.
- Less than: `$lt`, such as selecting from a range: `db.movieDetails.find({runtime: {$gt: 90, $lt: 120}}).pretty();`.
- Greater than or equal to: `$gte`.
- Less than or equal to: `$lte`.
- Find all movies with a runtime that is greater than or equal 120 minutes, and that have a "tomato.meter" score of greater than or equal to 95: `db.movieDetails.find({runtime: {$gte: 180}, "tomato.meter": {$gte: 95}}, {_id: 0, title: 1, runtime: 1});`.
- Not equal to: `$ne`. Useful in data-cleaning. Such as find all movies that don't have 'UNRATED' as their rated value: `db.movieDetails.find({rated: {$ne: "UNRATED"}}, {_id: 0, title: 1, runtime: 1});`.
- Matches the values specified, `$in`. Such as find all moves that have a rated value of either 'G' or 'PG': `db.movieDetails.find({rated: {$in: ["G", "PG"]}}, {_id: 0, title: 1, runtime: 1});`. The value of `$in` must be an array.
- Matches non of the values specified, `$nin`.

#### Element operators

- Match documents if it contains a specified key exists: `{mpaaRating: {$exists: true}}`. Change `true` to `false` to return values that do not have this field.
- Match documents where a key has a specified value: `db.movies.find({viewerRating: {$type: "int"}}).pretty();`.

#### Logical operators

- `$or` and `$and`.
```javascript
db.movieDetails.find({$or: [{"tomato.meter": {$gt: 95}},
							{"metacritic": {$gt: 88}}]},
							{_id: 0, title: 1, "tomato.meter": 1, "metacritic": 1});
```
- Comparison operators, when multiple are used, are automatically `anded` together.

#### Array operators: $all

- Match arrays that contain all elements specified: `$all`. Such as movies that have "Comedy, Crime, and Drama" in their arrays: `db.movieDetails.find({genres: {$all: ["Comedy", "Crime", "Drama"]}}, {_id: 0, title: 1, genres: 1}).pretty();`.

#### Array operators: $size

- Select documents if the array field is a specified size (or array length), `$size`. Such as: `db.movieDetails.find({countries: {$size: 1}}).pretty();`.
