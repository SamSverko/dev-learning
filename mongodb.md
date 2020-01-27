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

---

### Chapter 2: Replication

#### What is replication?

- MongoDB uses asynchronous replication.
- Replication is the concept of maintaining multiple copies of your data.
- Availability is also known as replication.
- Databases without replication is called a stand-alone.
- Replicated systems has extra nodes that holds copies of our data.
- Replica set: A group of nodes that each have copies of the same data. All data is managed through the primary node, and the secondary nodes asynchronously copy the data from the primary.
- Failover: If the primary goes down, then a secondary node steps up as the new primary.
- The secondary nodes decide which one becomes the primary through an election (they actually vote to who should be the new primary).
- Data replication can take two forms:
	- Binary replication.
		- Assumes the OS is consistent within the set.
		- Pros: Less data, faster
	- Statement-based replication.
		- After a write is complete, the write is set to the Oplog, which is synced to the secondary nodes.
		- Uses Idempotency - You can sync the Oplog over and over without anything changing.
		- Pros: Not bound to operating system or dependency.

#### MongoDB replica sets

- Replica sets are groups of nodes that share the same data.
- A replica can act as a primary set, or secondary set.
- Asynchronous replication mechanism.
- Replica sets provide a high availability and failover.
- The read/writes are handled by the primary set.
- The secondary sets then replicate the data from the primary.
- PV1 - Protocol version 1.
- Oplog - Operation log, a statement-based log.
- The third kind of replica can be an Arbiter, this type:
	- Holds no data.
	- Can vote in an election.
	- Cannot become the new primary.
- You should always have an odd number of nodes in a replica sets.
- You can have up to 50 members.
- Only a maximum of 7 nodes can vote on an election.
- It's recommended to avoid Arbiters entirely.
- You can define a node as hidden, so that the data is hidden from the application, or even delaying the nodes to delay the replication process.

#### Setting up a replica set

- Initiate a replica set: `rs.initiate()`.
- Connect to a mongo shell by specifying the replica set: `mongo --host "m103-example/192.168.103.100:27011" -u "m103-admin" -p "m103-pass" --authenticationDatabase "admin"`.
- Get status report of the replica set: `rs.status()`.
- Add a replica to the set: `rs.add("m103:27012")`.
- Get overview of the replica set topology: `rs.isMaster()`.
- Safely step down the current primary, and create an election to elect a new primary: `rs.stepDown()`.

#### Replication configuration document

- JSON object that defines the configuration options of our replica set.
- Can be configured manually from the shell.
- There are set of mongo shell replication helper methods that make it easier to manage:
	- `rs.add`.
	- `rs.initiate`.
	- `rs.remove`.
	- `rs.reconfig`.
	- `rs.config`.
- `_id: m103-example`: The name of the replica set.
- `version: 1`: Increments whenever we update the replica set (add a replica node, or change a line of the configuration).
- `members: []`: Array of each node member.
- Setting a `priority` level of `0` to a member essentially removes that nodes chance of ever becoming a primary.

#### Replication commands

- `rs.status()`.
	- Reports health on replica set nodes.
	- Uses data from heartbeats (it may be a few seconds out of date).
- `rs.isMaster()`.
	- Describes a node's role in the replica set.
	- Shorter output than `rs.status()`.
- `db.serverStatus()['repl']`.
	- Section of the db.serverStatus() output.
	- Similar to the output of `rs.isMaster()`.
	- `rbid` - Number of rollbacks that has occured.
- `rs.printReplicationInfo()`.
	- Only returns oplog data relative to current node.
	- Contains timestamps for the fist and last oplog events, and not the statement entered.

#### Local DB: Part 1

- Databases also act as namespaces.
- A standalone has a local database, which only contains only a singular collection `startup_log`.
- A replica set has a local database, which has more collections.
- `oplog.rs` Keeps track of all statements being replicated in the set.
- A capped collection is a collection that is capped to a specified size.

#### Local DB: Part 2

- `oplog.rs` Usually takes 5% of the available disk.
- Replication window is how long it takes to fill up an oplog.
- One operation may result in many `oplog.rs` entries.
- What happens in the local database, stays in the local database.
- Return only the last entry in the oplog: `db.oplog.rs.find( { "o.msg": { $ne: "periodic noop" } } ).sort( { $natural: -1 } ).limit(1).pretty()`.

#### Reconfiguring the replica set

- Add an arbiter to the replica set: `rs.addArb("m103:28000")`.
- Remove a replica or arbiter from the replica set: `rs.remove("m103:28000")`.
- Show the configuration of the replica set: `rs.conf()`.
- Change values of the replica set configuration:
```javascript
cfg = rs.conf()
cfg.members[3].votes = 0
cfg.members[3].hidden = true
cfg.members[3].priority = 0
rs.reconfig(cfg)
```

#### Reads and writes on a replica set

- To connect directly to a secondary replica, you must exclude the replica set name in the shell command (otherwise the shell will automatically redirect you to the primary): `mongo --host "m103:27012" -u "m103-admin" -p "m103-pass" --authenticationDatabase "admin"`.
- You can only run perform read operations when connected to a secondary replica.
- Enable read commands on secondary replica: `rs.slaveOk()`.

#### Failover and elections

- Rolling upgrades happen on secondary nodes first, then the primary.
- To update all nodes in a three node replica set. Update both secondary nodes. Step down the primary node to a secondary node. The newly demoted secondary node can now be upgraded. Now all three nodes are up to date.
- Reconfiguring a replica set will always trigger an election, that may or may not elect a new primary.
- Priority is the likelihood of that node becoming a primary after election.
- Priority of zero can still vote in elections, but cannot run for election.
- Nodes that aren't eligible to run for election are labeled as `passives`.

#### Write concerns: Part 1

- A write acknowledgment mechanism that developers can ad to write operations.
- More durability definition means it takes more time.
- Write concerns:
	- `0` - Don't wait for acknowledgment.
	- `1` (default) - Wait for acknowledgment from the primary only.
	- `>= 2` - Wait for acknowledgment from the primary and one or more secondaries.
	- `majority` - Wait for acknowledgment from a majority (number of nodes in replica set / 2, rounded up) of replica set members.
- Write concern options:
	- `wtimeout` Max amount of time the application will wait for the requested write concern before marking it as failed. The operation could have been successful (after the time limit), but it failed the test you put in place.
	- `j` Each replica set node to commit the operation to the journal before returning acknowledgment.

#### Write concerns: Part 2

- Stronger level of durability comes with a tradeoff, time. It will take longer.
- MongoDB supports write concern for all cluster types: Standalone, replica set, and sharded clusters.
- Insert data with a write concern of 3 and a timeout of 1000 miliseconds: `db.new_data.insert({"m103": "very fun"}, { writeConcern: { w: 3, wtimeout: 1000 }})`.

#### Read concern

- Acknowledgment management for read operations.
- You can choose to return the most recent data in the cluster, or the data from the majority in the cluster.
- Read concern levels:
	- `local` - Default for primary members. In the one you are connected to.
	- `available` (replica sets) - Default for secondary members.
	- `majority` - When the data returned is from the majority of nodes.
	- `linearizable` - Return majority of data, but provides read on write functionality.
- Fast, safe, and latest data = Pick two.
	- Fast and latest = `local` and `available`.
	- Fast and safe = `majority`.
	- Latest and safe = `linearizable`.
- None of the read concerns require you to specify a write concern. However, reads with the read concern majority and linearizable will only return data that has been replicated to a majority of nodes in the replica set.

#### Read preferences

- Read preference is typically a driver-side setting.
- Read preference modes:
	- `primary` (default) - Routes all read operations to the primary.
	- `primaryPreferred` - Routes all read operations to the primary, but if it's unavailable, it will route reads to the secondary.
	- `secondary` - Only to the secondary
	- `secondaryPreferred` - Only to the secondary member, but if it's unavailable, it will route reads to the secondary.
	- `nearest` - Routes all read operations to the member with the lowest network latency, regardless of the member type. It also supports geographically local reads.
- Secondary reads always have a chance of returning stale data.
- To count all documents in a database: `db.collection.count()`.
- Set read preference: `db.getMongo().setReadPref('secondary')`.

---

### Chapter 3: Sharding

#### What is sharding?

- Vertical scaling: Increasing the performance such as upgrading the RAM, CPU, or disk space.
- Horizontal scaling: MongoDB method of scaling (called `Sharding`). You add more machines, and distribute the data amongst those machines.
- Together, the shards make up a sharded cluster. Each shard is a replica set.
- Mongos - Routing queries to the appropriate shard.
- Mongos uses the metadata and config servers to know where the data is. This is using the config server replica set.

#### When to shard

- You don't need to shard right off the bat.
- Before sharding, you should check these aspects of your dataset:
	- Is it still economically viable to scale vertically (CPU, Network, RAM, disk space)?
	- What would be the impact on operational tasks?
- Horizontal scales allow for process parallelization of backup, restore, and initial sync.
- General rule of thumb for maintaining datasets, the storage should be between 2-5 TB per server.
- Single thread operations and geographically distributed data.
- Zone sharding allows you to easily manage geographically distributed datasets.

#### Sharding architecture

- You can have any number amount of shards.
- Clients don't talk to the shard directly, but to the mongos `router` you set up.
- Each database will be assigned a primary shard.
- If data is found from multiple shard, the `mongos` will perform a `SHARD_MERGE` to only send one dataset back to the client.
- Non-sharded collections are placed on the primary shard.
- The role of primary shard is subject to change since we can specify the configuration.
- Shard merges are performed by the `mongos`.

#### Setting up a sharded cluster

- Basic sharded cluster consists of one sharded replica set, one configuration sharded replica set (CSRS), and one mongos instance.
- Once already connected to a mongod instance, you can authenticate after connected: `db.auth("username", "password")`.
- Whenever you setup a mongod process confifuration, ensure you create the directory that it will be using: `mkdir /var/mongodb/db/[db-name]`.
- Connect to mongos once configuration file is created: `mongo --port 26000 --username m103-admin --password m103-pass --authenticationDatabase admin`.
- Start a `mongos` process: `mongos -f mongos.conf`.
- View sharding status on `mongos`: `sh.status()`.
- Add a shard to the mongos. You can specify any node because all the shards were already linked through their configuration file: `sh.addShard("m103-repl/192.168.103.100:27012")`.
- The mongos configuration file doesn't need to have a dbpath.
- The mongos configuration file needs to specify the config servers.
- Remove a directory that's not empty: `rm -r [directory]`.

#### Config db

- You should generally never write any data to this database.
- Switch to config db: `use config`.
- You can now find information on databases, collections, shards, chunks, mongo: `db.databases.find()`.

#### Shard keys

- Index field to partition data in a sharded collection and distribute it amongst the shards.
- Grouping of documents in a shard is called a `chunk`.
- A shard key must be included in every document.
- Indexes must exist first before you can select the indexed fields for your shard key.
- Sharding is a permanent operation.
	- You cannot change the shard key fields post-sharding.
	- You cannot change the values of the shard key fields post-sharding.
- Shard keys are permanent, you cannot unshard a sharded colleciton.
- How to shard:
	- Enabled sharding for the specified database: `sh.enableSharding("[database]")`.
	- Create the index for your shard keys: `db.[collection].createIndex()`.
	- Shard the collection: `sb.shardCollection("[database].[collection]", {[shard-key]})`.
- All collections have an index on `id` by default.

#### Picking a good shard key

- You enable sharding on a database.
- You shard a collection. Not all collections in sharding enabled database need to be sharded.
- What makes a good shard key?
	- High cadinality: Many possible unique shard key values.
	- Low frequency: Low repetition of a given unique shard value.
	- Non-monotonic change: Avoid shard keys that change at a steady and predictable state (non-linear changes in value).
- Read isolation = MongoDB directs targeted queries to a single shard (extremely fast query).
- Compound index has several keys rather than a single one.
- Testing sharding in a testing environment before sharding a production environment.
- Unsharding a collection is hard, you really want to avoid that.

#### Hashed shard keys

- Shard key where the underlying index is hashed.
- The actual data remains untouched, it's just the underlying index of the shard that's hashed.
- Monotonically-changing shard key values result in hotspotting (more inserts end up on a single shard).
- Hashed shard keys provide more even distribution of monotonically-changing shard keys.
- Queries on ranges of shard key values are more likely to be scatter-gather.
- Cannot support geographically isolated read operations using zoned sharding.
- Hashed indexes cannot be compounded.
- Hashed index don't support fast sorting.
- Create a hashed shard key:
	- Enabled sharding for the specified database: `sh.enableSharding("[database]")`.
	- Create the index for your shard keys: `db.[collection].createIndex({"[field]": "hashed"})`.
	- Shard the collection: `sh.shardCollection("[database].[collection]", {[shard-key-field]: "hashed"})`.

#### Chunks

- Config servers hold the cluster metadata (how many databases are sharded, which databases are sharded, etc.).
- Config servers also shown the mapping of how the shards are organized.
- Find one document chunk: `db.chunks.findOne()`.
- Chunks lower bound is inclusive, and chunks upper bound is exclusive.
- ChunkSize = default is 64MB.
- Change the chunk size on the `config db`: `db.settings.save({_id: "chunksize", value: 2})`.
- Shard key values frequency plays a role in balancing chunks.
- Jumbo chunks
	- They are larger than defined chunk size.
	- Cannot move jumbo chunks
	- Once marked as jumbo the balancer skips these chunks and avoids trying to move them.
	- In some cases these will not be able to be split.
- Increasing the maximum chunk size can help eliminate jumbo chunks.
- Chunk ranges have an inclusive minimum and an exclusive maximum.
- Locate a chunk based on a specific document: `db.chunks.find().pretty()`.

#### Balancing

- MongoDB balancing identifies which chunks have too much data, and attempts to evenly distribute them amongst the shards.
- The primary node of the config server is responsible for managing the balancer.
- A balancer round can only balance number of chunks / 2.
- Start the balancer: `sh.startBalancer(timeout, interval)`.
- Stop the balancer: `sh.stopBalancer(timeout, interval)`.
- Set the balancer: `sh.setBalancer(boolean)`.

#### Queries in a sharded cluster

- Mongos is the principle interface for handling with queries a sharded cluster.
- Scatter-gather operations is where the `mongos` needs to look in multiple shards in the cluster to find data (slow queries).
- Targeted (specified shard) versus scatter-gather (must look through multiple shards).
- Cursor operators:
	- `sort()` - `Mongos` pushes the sort to each shard and merge-sorts the results.
	- `limit()` - `Mongos` passes the limit to each targeted shard, then re-applies the limit to the merged set of results.
	- `skip()` - `Mongos` performs the skip against the merged set of results.
- For a find() operation, the `mongos` that issued the query is responsible for merging the query results.

#### Routed queries vs scatter gather: Part 1

- Each shard contains chunks of sharded data, where each chunk represents an inclusive lower bound and exclusive upper bound of the dataset.
- `minKey` = Lowest possible value (think minus infinity).
- `maxKey` = Highest possible value (think positive infinity).
- When you query with the shard index (e.g. `sku`), then mongos automatically knows which shard to direct the query to.
- When the query does not contain the shard index, it must perform the scatter-gather query and all shards must return a response in order for the `mongos` to receive the data.
- It is important that your shard index is used in the majority of your queries so as to keep up maximum efficient of queries.
- Ranged queries on a hashed index are almost always scatter-gather.
- If you are using a compound index, you can still perform a targeted query, you have to use the prefix. of it. Example, if the compound index was `{"sku": 1, "type": 1, "name", 1}`. Then `db.collection.find({"sku": ...})`, `db.collection.find({"sku": ..., "type": ...})`, and `db.collection.find({"sku": ..., "type": ..., "name": ...})` all work for targeted query. But `db.collection.find({"type": ...})` and `db.collection.find({"name": ...})` do not since they don't have the prefixes of the compound index.

#### Routed queries vs scatter gather: Part 2

- Find a document and explain how it was found: `db.products.find({"sku" : 1000000749 }).explain()`.
- `"stage": "SINGLE_SHARD"` the `mongos` was able to retrieve the data from a single shard and not merged results.
- `"stage": "SHARD_MERGE"` the `mongos` retrieved the data from a merge (multiple shards too).
- `SHARDING_FILTER`: The step performed by `mongos` used to make sure that documents fetched from a particular shard are supposed to be from that shard. To do this, mongos compares the shard key of the document with the metadata on the config servers.
- `IXSCAN`: An index scan, used to scan through index keys.
- `FETCH`: A document fetch, used to retrieve an entire document because one or more of the fields is necessary.
- The sharding filter ensures that documents returned by each shard are not orphan documents. It does this by comparing the value of the shard key to the chunk ranges inside that particular shard.
- `Mongos` will try to minimize the number of documents checked by the shard filter. To do this, `mongos` will only send the documents matching the query (i.e. are returned by the index scan) to be compared against the chunk ranges.

---

## M121: The MongoDB aggregation framework

### Chapter 0: Introduction and aggregation concepts

#### Introduction to the MongoDB aggregation framework

- Expressive filtering, powerful data transformation, statistical utilities, and data analysis.
- Much more work with a single query.

#### Atlas requirement

- We have access to 27017!
- Atlas is MongoDB's cloud hosting service.
- Connect to the class Atlas: `mongo "mongodb://cluster0-shard-00-00-jxeqq.mongodb.net:27017,cluster0-shard-00-01-jxeqq.mongodb.net:27017,cluster0-shard-00-02-jxeqq.mongodb.net:27017/aggregations?replicaSet=Cluster0-shard-0" --authenticationDatabase admin --ssl -u m121 -p aggregations --norc`

#### The concept of pipelines

- A pipeline is a composition of stages.
- Stages configurable for transformation. Stages can be arranged in any way.
- Documents flow like like an assembly line.
- Conveyor belt in a factory, along the line, there are different stages.
- Documents enter the pipeline, and go through the first stage, `$match`.
- The second stage is `$project`, and the next stage is `$group`.

#### Aggregation structure and syntax

- Pipelines may consist of one or more stages.
- Each stage is a JSON object of key value pairs.
- Each stage is either an operator or expression.
- Visit the MongoDB [Aggregation pipeline quick reference](https://docs.mongodb.com/manual/meta/aggregation-quick-reference/).
- Operators mean query operators or aggregation stages.
- Operators always appear in the key position of a document.
- Expressions act a lot like functions.
- Variable types:
	- Field path: `$fieldName`.
	- System variable: `$$UPPERCASE`.
	- User variable" `$$foo`.
- Some expressions can only be used in certian stages.

---

### Chapter 1: Basic aggregation - $match and $project

#### $match: Filtering documents

- 
