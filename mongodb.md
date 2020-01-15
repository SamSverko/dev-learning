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

## M001: MongoDB Basics

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

#### Array operators: $elemMatch

- Specify the field you want to field, specify selectors within that key that matches all the elements in `$elemMatch`: `db.movieDetails.find({boxOffice: {$elemMatch: {"country": "Germany", "revenue": {$gt: 17}}}});`.

#### $regex operator

- Regex = regular expression.
- Using regular expressions to filter results: `db.movieDetails.find({"awards.text": {$regex: /^Won .*/}}, {_id: 0, title: 1, "awards.text": 1}).pretty();`.
- Find documents that contain at least one score in the results array that is greater than or equal to 70 and less than 80: `db.scores.count({results: {$elemMatch: {$gte: 70, $lt: 80}}});`.

---

## M103: Basic cluster administration

### Chapter 1: Introduction and setup

#### Introduction to replication and sharding

- Administrative tools.
- Mongod - Request and data access.
- Authentication and database logs.
- High availability and fault tolerance.
- Scalability and horizontal sharding.

#### Setting up the vagrant environment

- Virtual environment creates a sandbox for the course.
- Avoids dependency and system troubleshooting.
- VirtualBox and Vagrant.
- Start Vagrant: `vagrant up`.
- Stop Virtual Machine (VM): `vagrant halt` for gracefully shut down, or `vagrant suspend` to suspend it.
- SSH to the VM: `vagrant ssh`.
- Exit the VM: `exit`.
- Check the health/status of the VM: `vagrant status`.

#### The mongod

- Mongod is the main daemon process of MongoDB.
- What is daemon: it's a process or programs that's meant to be run and not interacted with directly.
- Start the mongod: `mongod`.
- Connect to the MongoDB shell once mongod is running: `mongo`.
- Shutdown the MongoDB process: `db.shutdownServer();`.
- Mongod flags:
	- `--port` (default is 27017).
	- `--dbpath` specify where the db directory is located, if changed from default.
	- `--logpath` specify where the db will log informational messages.
	- `--fork` start mongod as a background process.
- Start mongoDB shell at specified port: `mongo --port 3000`.
- Create user with root access on admin database:
```shell
mongo admin --host localhost:27000 --eval '
  db.createUser({
    user: "m103-admin",
    pwd: "m103-pass",
    roles: [
      {role: "root", db: "admin"}
    ]
  })
'
```

#### Configuration file

- Include `bind_ip` to add specified network access.
- YAML - "YAML Ain't Markup Language".
- To use a config file when executing the MongoDB daemon, run: `mongod --config [or use flag -f] "/location/of/config.conf"`.
- List current mongod processes: `ps -ef | grep mongod`.
- Kill a process: `kill <pid>`.

#### File structure

- Do not interact with any `WiredTiger` files.
- The files in `journal` directory helps save checkpoints of your data.
- MongoDB uses socket connections.
- `diagnostics.data` and log files assist support in diagnostics.
- Do not modify files or folders in the MongoDB data directory.
- Change ownership of folder: `sudo chown -R vagrant:vagrant /var/mongodb`.

#### Basic commands

- Basic helper groups:
	- `db.[method]()` Controls the database.
		- `db.createUser()` and `db.dropUser()`.
		- `db.dropDatabase()`.
		- `db.createCollection()`.
		- `db.serverStatus()`.
		- `db.runCommand({[command]})`.
		- `db.commandHelp("[command]")`.
	- `db.[collection].[method]()` Collection-level methods.
		-  `db.renameCollection()`.
		- `db.collection.createIndex()`.
		- `db.collection.drop()`
	- `rs.[method]()` Controls replica sets.
	- `sh.[method]()` Controls sharded clusters.

#### Logging basics

- Process log (ACCESS, COMMAND, CONTROL, FTDC, etc.)
- Display log components: `db.getLogComponents()`. The higher the verbosity number, the more information shown.
- Verbosity:
	- `-1` Inherit from parent
	- `0` Default verbosity, to include informational messages.
	- `1-5` Increases the verbosity level to include debug messages.
- Get admin logs: `db.adminCommand({"getLog": "global"})`.
- Set the verbosity of a log category: `db.setLogLevel(0, "index")`.
- Log dissection:
	- Timestamp (`2020-01-13T20:05:50.008+000`).
	- Severity level (F-Fatal, E-Error, W-Warning, `I`-Information (0), D-Debug (1-5)).
	- Log component (`COMMAND`).
	- Connection (`[conn3]`).
	- Action and namespace (`command admin.$cmd`).
	- Application that initiated the operation (`appName: "MongoDB Shell"`).
	- The operations (`command: setParameter {setParameter: 1.0}`).
	- Operation metadata (`numYields: 0 reslen:616 locks:{} 0ms`).

#### Profiling the database

- Profiler captures CRUD, admin, and cluster configuration options.
- Profiler levels
	- 0: Profiler is off and does not collect any data (default).
	- 1: Profiler collects data for operations that take longer than value of slowms(slowms is how many miliseconds the action takes, and anything that takes longer than that will be logged).
	- 2: Profile collects data for all operations.
- Show current profiling level: `db.getProfilingLevel()`.
- Set current profiling level: `db.setProfilingLevel(1)`. You can include the slowms setting: `db.setProfilingLevel(1, {slowms: 100})`. If it wasn't already there, either command will create a new collection called `system.profile` to keep the profiling data.

#### Basic security: Part 1

- Authentication
	- Verifies the identity of a user.
	- Answers the question: Who are you?
- Authorization:
	- Verifies the privileges of a user.
	- Answers the question: What do you have access to?
- Authentication methods:
	- `SCRAM` (default) - Stands for salted challenge response authentication method.
	- `X.509` - Certification for authentication. More secure yet more complicated.
	- `LDAP` (enterprise only) - Stands for Lightweight directory access protocol.
	- `KERBEROS` (enterprise only) - Works on the basis of tickets to allow nodes communicating over a non-secure network.
- Intra-cluster authentication is also available.
- Role-based access control system (RBAC).
- Each `user` has one or more `roles`.
- Each `role` has one or more `privileges`.
- A `privilege` represents a `group` of actions and the resources those actions apply to. Example:
	- DBA - Create user, create index.
	- Developer - Write data, read data.
	- Data scientist - Write data.

#### Basic security: Part 2

- Localhost exception:
	- Allows you to access MongoDB server that enforces authentication but does not yet have a configured user for you to authenticate with.
	- Must run Mongo shell from same host running the MongoDB server.
	- The localhost exception closes after you create your first user.
	- Always create a user with Administrative privileges first.
- Create a new user:
```javascript
db.createUser({
	user: "root",
	pwd: "root123",
	roles : ["root"]
})
```

#### Built-in roles: Part 1

- Role based access control.
- Database `users` are granted `roles`.
- Custom roles - Tailored roles to attend specific needs of sets of users.
- Built-in roles - Pre-packaged MongoDB roles.
- Role structure
	- Set of privileges
		- Actions performed over a resource.
			- Resources can be replica sets (specific database and collection, all databases and all collections, or specific database and any collection), shard clusters.
- Roles can inherit.
- Network authentication restrictions.
- Built-in roles:
	- Database user (`read`, `readWrite` | `readAnyDatabase`, `readWriteAnyDatabase`).
	- Database administration (`dbAdmin`, `userAdmin`, `dbOwner` | `dbAdminAnyDatabase`, `userAdminAnyDatabase`).
	- Cluster administration (`clusterAdmin`, `clusterManager`, `clusterMonitor`, `hostManager`).
	- Backup/restore (`backup`, `restore`).
	- Super user (`root`).
- All roles are on a per database system.

#### Built-in roles: Part 2

- This is the first user you should always create, because they can do everything for user management, but nothing with data management:
```javascript
db.createUser(
	{ user: "security_officer",
		pwd: "h3ll0th3r3",
		roles: [ { db: "admin", role: "userAdmin" } ]
	}
)
```
- The next user to administer the databases (DDL - data definition language):
```javascript
db.createUser(
	{ user: "dba",
		pwd: "c1lynd3rs",
			roles: [ { db: "admin", role: "dbAdmin" } ]
	}
)
```
- DML - Data modification language.
- Grant roles to user: `db.grantRolesToUser( "dba",  [ { db: "playground", role: "dbOwner"  } ] )`.
- Show role privileges: `db.runCommand( { rolesInfo: { role: "dbOwner", db: "playground" }, showPrivileges: true} )`.
- First application user:
```javascript
db.createUser(
	{ user: "m103-application-user",
		pwd: "m103-application-pass",
		roles: [ { db: "applicationData", role: "readWrite" } ]
	}
)
```

#### Server tools overview

- Mongod - Core database process.
- Mongo - Interactive MongoDB shell used to connect to Mongod.
- `mongostat` - Give quick stats on a running Mongo process.
- `mongorestore` and `mongodump` - Import and export Mongo dump files from MongoDB collections (in `BSON` format).
	- Restore: `mongorestore --drop --port 30000 dump/`.
	- Dump: `mongodump --port 30000 --db applicationData --collection products`.
	- The `bson` file is the actual database data.
	- The `json` file is meta data on the `bson` file.
- `mongoexport` and `mongoimport` - Same as restore and dump, but this uses `json` files (or `csv`).
	- Export as a `json` document: `mongoexport --port 30000 --db applicationData --collection products -o products.json`.
	- Import: `mongoimport --port 30000 products.json`.
- Add authentation to any of these commands: `-u "m103-application-user" -p "m103-application-pass" --authenticationDatabase "admin"`
- Full command to import a dataset using authentication: `mongoimport --port 27000 /dataset/products.json --db applicationData --collection products -u "m103-application-user" -p "m103-application-pass" --authenticationDatabase "admin"`.
