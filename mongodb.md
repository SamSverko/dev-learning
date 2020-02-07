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
- Some expressions can only be used in certan stages.

---

### Chapter 1: Basic aggregation - $match and $project

#### $match: Filtering documents

- This is an aggregation operator, so we must append the `$` before it.
- Basic syntax:
```javascript
db.collection.aggregate([{
	$match: {
		type: {
			$ne: "Star"
		}
	}
}])
```
- `$match` is more of a filter, more than a find.
- `$match` uses standard MongoDB query operators (comparison, logic, arrays).
- Where cannot use the `$where` operator with `$match`.
- `$match` should always come early in the pipeline.
- `$match` does not allow for projection.
- A `$match` stage may contain a `$text` query operator, but it must be the first stage of a pipeline.

#### Lab - $match

- Help MongoDB pick a movie our next movie night! Based on employee polling, we've decided that potential movies must meet the following criteria.
	- `imdb.rating` is at least 7.
	- `genres` does not contain "Crime" or "Horror".
	- `rated` is either "PG" or "G".
	- `languages` contains "English" and "Japanese".
- Answer:
```JavaScript
// Since selectors in MongoDB filters are `AND`ed together by default, you can omit the $and operator:
var pipeline = [{
	$match: {
		"imdb.rating": { $gte: 7 },
		"genres": {$nin: ["Crime", "Horror"]},
		"rated": {$in: ["PG", "G"]},
		"languages": {$all: ["English", "Japanese"]}
	}
}];
// But if you wish to include the $and operator, you can do so:
var pipeline = [{
	$match: {
		$and: [
			{"imdb.rating": { $gte: 7 }},
			{"genres": {$nin: ["Crime", "Horror"]}},
			{"rated": {$in: ["PG", "G"]}},
			{"languages": {$all: ["English", "Japanese"]}}
		]
	}
}];
```

#### Shaping documents with $project

- We can remove and retain field, and derive entirely new field.
- `$project` is like the map function.
- Sample `$project` syntax: `db.collection.aggregate([{ $project: {} }])`.
- The `_id` field is the only field that must be explicitly removed in order for it to be removed from the query.
- Selecting subfield must be surrounded in quotes: `"imdb.rating"`.
- Reassign the value to that field: `$project: {_id: 0, name: 1, gravity: "$gravity.value"}`.
- Multiple values together: `$multiply: [gravityRatio, weightOnEarth]`, or divide (same format, it divides the first number by the second).
- Use data to be altered and then assigned to a new value:
```javascript
$project: {
	_id: 0,
	name: 1,
	myWeight: {
		$multiply: [{$divide: ["$gravity.value", 9.8]}, 86]
	}
}
```

#### Lab - Changing document shape with $project

- Using the same $match stage from the previous lab, add a $project stage to only display the the title and film rating (title and rated fields).
Answer:
```javascript
var pipeline = [
	{
		$match: {
			$and: [
				{"imdb.rating": { $gte: 7 }},
				{"genres": {$nin: ["Crime", "Horror"]}},
				{"rated": {$in: ["PG", "G"]}},
				{"languages": {$all: ["English", "Japanese"]}}
			]
		}
	},
	{
		$project: {
			_id: 0,
			title: 1,
			rated: 1
		}
	}
];
```

#### Lab - Computing fields

- Using the Aggregation Framework, find a count of the number of movies that have a title composed of one word. To clarify, "Cinderella" and "3-25" should count, where as "Cast Away" would not.
- Answer:
```JavaScript
var pipeline = [
	{
		$project: {
			"_id": 0,
			"titleSplit": {$split: ["$title", " "]}
		}
	},
	{
		$match: {
			"titleSplit": {
				$size: 1
			}
		}
	}
];
```

#### Optional Lab - Expressions with $project

- `$map` lets us iterate over an array, element by element, performing some transformation on each element. The result of that transformation will be returned in the same place as the original element.
- Within `$map`, the argument to `input` can be any expression as long as it resolves to an array. The argument to `as` is the name of the variable we want to use to refer to each element of the array when performing whatever logic we want. The field `as` is optional, and if omitted each element must be referred to as `$$this`. The argument to in is the expression that is applied to each element of the input array, referenced with the variable name specified in as, and prepending two dollar signs:
```javascript
writers: {
	$map: {
		input: "$writers",
		as: "writer",
		in: {
			$arrayElemAt: [
				{
					$split: [ "$$writer", " (" ]
				},
				0
			]
		}
	}
}
```
- Find how many movies in our movies collection are a "labor of love", where the same person appears in cast, directors, and writers.
- Answer:
```javascript
db.movies.aggregate([
	{
		$match: {
			cast: { $elemMatch: { $exists: true } },
			directors: { $elemMatch: { $exists: true } },
			writers: { $elemMatch: { $exists: true } }
		}
	},
	{
		$project: {
			_id: 0,
			cast: 1,
			directors: 1,
			writers: {
				$map: {
					input: "$writers",
					as: "writer",
					in: {
						$arrayElemAt: [
							{
								$split: ["$$writer", " ("]
							},
							0
						]
					}
				}
			}
		}
	},
	{
		$project: {
			labor_of_love: {
				$gt: [
					{ $size: { $setIntersection: ["$cast", "$directors", "$writers"] } },
					0
				]
			}
		}
	},
	{
		$match: { labor_of_love: true }
	},
	{
		$count: "labors of love"
	}
]);
```

---

### Chapter 2: Basic aggregation - utility stages

#### $addFields and how it is similar to $project

- This ad fields to a document, whereas `$project` can remove and change fields.
- This appends new fields to the document.

#### geoNear stage

- Aggregation framework solution to performing geo queries within the aggregation pipeline.
- `$geoNear` must be the first query in the pipeline if used.
- This can be used on sharded collections, whereas `$near` cannot.
- Required arguments:
	- `near` - Point.
	- `distance` - Distance from MongoDB.
	- `spherical` - Boolean.
- The collection can only have one 2dsphere index.
- If using 2dsphere, the distance is returned in meters. If using legacy coordinates, the distance is returned in radians.

#### Cursor-like stages: Part 1

- These are the cursor methods:
	- The order you get documents are the "natural order", meaning when the documents were inserted into the database.
	- You can skip documents when searching for them: `db.solarSystem.find({}, {_id: 0, name: 1, numberOfMoons: 1}).skip(5).pretty();`.
	- You can limit the number of documents returned: `db.solarSystem.find({}, {_id: 0, name: 1, numberOfMoons: 1}).limit(5).pretty();`.
	- You can sort the documents returned: `db.solarSystem.find({}, {_id: 0, name: 1, numberOfMoons: 1}).sort({numberOfMoons: -1}).pretty();`. A value of `1` is ascending, a value of `-1` is descending.
- Cursor stages:
	- `$limit`, `$skip`, `$count`, `$count`.
	```javascript
	db.solarSystem.aggregate([
		{
			$project: {
				_id: 0,
				name: 1,
				numberOfMoons: 1
			}
		},
		{
			$limit: 5
		}
	]);
	```

#### Cursor-like stages: Part 2

- `$count` stage counts all incoming documents:
```javascript
db.solarSystem.aggregate([
	{
		$match: {
			type: "Terrestrial planet"
		}
	},
	{
		$project: {
			_id: 0,
			name: 1,
			numberOfMoons: 1
		}
	},
	{
		$count: "terrestrial planets"
	}
]);
```
- `$sort` Sorts the data:
```javascript
db.solarSystem.aggregate([
	{
		$project: {
			_id: 0,
			name: 1,
			numberOfMoons: 1
		}
	},
	{
		$sort: {numberOfMoons: -1}
	}
]);
```
- The sort field is not limited to one field.
- Sort aggregations are limited to 100MB of RAM, you can allow excess of this by adding: `{allowDiskUse: true}`.

#### $sample stage

- Select a set of random documents in a collection.
- First method: `{$sample: { size: < N, how many documents>}}`. When:
	- N <= 5% of number of documents in source collection, AND
	- Source collection has >= 100 documents, AND
	- $sample is the first stage.
	- Then a sudo-random cursor will select the random documents to be passed.
- Second method: `{$sample: { size: < N, how many documents>}}`. When:
	- All other conditions.
	- In-memory random sort.
- Useful when working with large amounts of data. Can be used to randomly search data for whatever reason.

#### Lab - Using cursor-like stages

- Using these as the favourites: `["Sandra Bullock", "Tom Hanks", "Julia Roberts", "Kevin Spacey", "George Clooney"]`, for movies released in the `USA` with a `tomatoes.viewer.rating` greater than or equal to `3`, calculate a new field called `num_favs` that represents how many favorites appear in the cast field of the movie.
- Sort your results by num_favs, tomatoes.viewer.rating, and title, all in descending order.
- What is the title of the 25th film in the aggregation result?
- Answer:
```JavaScript
// My answer
db.movies.aggregate([
	{
		$match: {
			"cast": {$in: ["Sandra Bullock", "Tom Hanks", "Julia Roberts", "Kevin Spacey", "George Clooney"]},
			"countries": {$in: ["USA"]},
			"tomatoes.viewer.rating": {$gte: 3}
		}
	},
	{
		$project: {
			"_id": 0,
			"cast": {
				$filter: {
					input: "$cast",
					as: "member",
					cond: {
						$in: ["$$member", ["Sandra Bullock", "Tom Hanks", "Julia Roberts", "Kevin Spacey", "George Clooney"]]
					}
				}
			},
			"countries": 1,
			"title": 1,
			"tomatoes.viewer.rating": 1
		}
	},
	{
		$addFields: {
			"num_favs": {
				$size: "$cast"
			}
		}
	},
	{
		$sort: {
			"num_favs": -1,
			"tomatoes.viewer.rating": -1,
			"title": -1
		}
	},
	{
		$skip: 24
	},
	{
		$limit: 1
	}
]);
// their answer
var favorites = [
	"Sandra Bullock",
	"Tom Hanks",
	"Julia Roberts",
	"Kevin Spacey",
	"George Clooney"
];

db.movies.aggregate([
	{
		$match: {
			"tomatoes.viewer.rating": { $gte: 3 },
			countries: "USA",
			cast: {
				$in: favorites
			}
		}
	},
	{
		$project: {
			_id: 0,
			title: 1,
			"tomatoes.viewer.rating": 1,
			num_favs: {
				$size: {
					$setIntersection: [
						"$cast",
						favorites
					]
				}
			}
		}
	},
	{
		$sort: { num_favs: -1, "tomatoes.viewer.rating": -1, title: -1 }
	},
	{
		$skip: 24
	},
	{
		$limit: 1
	}
])
```

#### Lab - Bringing it all together

- Calculate an average rating for each movie in our collection where English is an available language, the minimum `imdb.rating` is at least 1, the minimum `imdb.votes` is at least 1, and it was released in `1990` or after. You'll be required to rescale (or normalize) `imdb.votes`.

What film has the lowest `normalized_rating`?
- Answer:
```javascript
// my answer
db.movies.aggregate([
	{
		$match: {
			"languages": {$in: ["English"] },
			"imdb.rating": {$gte: 1},
			"imdb.votes": {$gte: 1},
			"year": {$gte: 1990}
		}
	},
	{
		$addFields: {
			"scaled_votes": {
				$add: [
					1,
					{
						$multiply: [
							9,
							{
								$divide: [
									{ $subtract: ["$imdb.votes", 5] },
									{ $subtract: [1521105, 5] }
								]
							}
						]
					}
				]
			}
		}
	},
	{
		$addFields: {
			"normalized_rating": {
				$avg: ["$scaled_votes", "$imdb.rating"]
			}
		}
	},
	{
		$project: {
			"_id": 0,
			"title": 1,
			"languages": 1,
			"normalized_rating": 1,
			"imdb.rating": 1,
			"imdb.votes": 1,
			"scaled_votes": 1,
			"year": 1
		}
	},
	{
		$sort: {
			"normalized_rating": 1
		}
	},
	{
		$limit: 1
	}
]);
// their answer
db.movies.aggregate([
	{
		$match: {
			year: { $gte: 1990 },
			languages: { $in: ["English"] },
			"imdb.votes": { $gte: 1 },
			"imdb.rating": { $gte: 1 }
		}
	},
	{
		$project: {
			_id: 0,
			title: 1,
			"imdb.rating": 1,
			"imdb.votes": 1,
			normalized_rating: {
				$avg: [
					"$imdb.rating",
					{
						$add: [
							1,
							{
								$multiply: [
									9,
									{
										$divide: [
											{ $subtract: ["$imdb.votes", 5] },
											{ $subtract: [1521105, 5] }
										]
									}
								]
							}
						]
					}
				]
			}
		}
	},
	{ $sort: { normalized_rating: 1 } },
	{ $limit: 1 }
])
```

---

### Chapter 3: Core aggregation - Combining information

#### The $group stage

- The only required field is the `_id` field.
- It may be necessary to sanitize incoming data.
- Group movies by their year, count how many are per year, then sort them:
```javascript
db.movies.aggregate([
	{
		$group: {
			_id: "$year",
			num_films_in_year: {
				$sum: 1
			}
		}
	},
	{
		$sort: {
			num_films_in_year: -1
		}
	}
]);
```
- Find the average metacritic score:
```javascript
db.movies.aggregate([
	{
		$match: {
			metacritic: {
				$gte: 0
			}
		}
	},
	{
		$group: {
			_id: null,
			averageMetacritic: {
				$avg: "$metacritic"
			}
		}
	}
]);
```

#### Accumulator stages in $project

- Accumulator expressions in `$project` operate over an array in the current document, they do not carry values over all documents.
- Expressions have no memory between documents.
- Get the average max high price for ice cream sales:
```javascript
db.icecream_data.aggregate([
	{
		$project: {
			_id: 0,
			max_high: {
				$max: "$trends.avg_high_tmp"
			}
		}
	}
]);
```

- Get the standard price index and standard deviation:
```javascript
db.icecream_data.aggregate([
	{
		$project: {
			_id: 0,
			average_cpi: {
				$avg: "$trends.icecream_cpi"
			},
			cpi_deviation: {
				$stdDevPop: "$trends.icecream_cpi"
			}
		}
	}
]);
```

#### Lab - $group and accumulators

- For all films that won at least 1 Oscar, calculate the standard deviation, highest, lowest, and average imdb.rating. Use the sample standard deviation expression.
- HINT - All movies in the collection that won an Oscar begin with a string resembling one of the following in their awards field: `Won 13 Oscars or Won 1 Oscar`.
- Answer:
```javascript
db.movies.aggregate([
	{
		$match: {
			awards: {
				$exists: true,
				$ne: null,
				$regex: /Won \d{1,2} Oscars?/
			}
		}
	},
	{
		$group: {
			_id: null,
			highest_rating: {
				$max: "$imdb.rating"
			},
			lowest_rating: {
				$min: "$imdb.rating"
			},
			average_rating: {
				$avg: "$imdb.rating"
			},
			deviation: {
				$stdDevSamp: "$imdb.rating"
			}
		}
	}
]);
```

#### The &unwind stage

- Unwind an array field, creating a new document for every entry where the field value is now each entry.
- Arrays are generally matches on pure equality, not equivalence.
- Short form: `$unwind: [field path]`.
- Long form:
```JavaScript
$unwind: {
	path: [field path],
	includeArrayIndex: [string],
	preserveNullandEmptyArrays: [boolean]
}
```
- Let's sort through the movie genres and return the top-rated genre per year:
```JavaScript
db.movies.aggregate([
	{ // find all the movies we are looking for
		$match: {
			"imdb.rating": {
				$gte: 0
			},
			year: {
				$gte: 2010, $lte: 2015
			},
			runtime: {
				$gte: 90
			}
		}
	},
	{ // unwind the genres as their own document
		$unwind: "$genres"
	},
	{ // group the new documents
		$group: {
			_id: {
				year: "$year",
				genre: "$genres"
			},
			average_rating: {
				$avg: "$imdb.rating"
			}
		}
	},
	{ // sort the documents from year and average_rating descending order
		$sort: {
			"_id.year": -1,
			average_rating: -1
		}
	},
	{ // now just take the first value of each year, since this will be the highest rating
		$group: {
			_id: "$_id.year",
			genre: {
				$first: "$_id.genre"
			},
			average_rating: {
				$first: "$average_rating"
			}
		}
	},
	{ // Sort by descending order
		$sort: {
			_id: -1
		}
	}
]);
```
- If the documents are long and need to use `$unwind`, this may lead to performance issues.

#### Lab - $unwind

- What is the name, number of movies, and average rating (truncated to one decimal) for the cast member that has been in the most number of movies with English as an available language?
- Answer:
```javascript
db.movies.aggregate([
	{
		$match: {
			"cast": {
				$exists: true,
				$ne: null,
				$not: {
					$size: 0
				}
			},
			"languages": {
				$in: ["English"]
			},
			"imdb.rating": {
				$exists: true,
				$ne: null
			}
		}
	},
	{
		$unwind: "$cast"
	},
	{
		$group: {
			"_id": "$cast",
			"numFilms": {
				$sum: 1
			},
			"average": {
				$avg: "$imdb.rating"
			}
		}
	},
	{
		$project: {
			"_id": 1,
			"numFilms": 1,
			"average": {
				$divide: [{ $trunc: { $multiply: ["$average", 10] } }, 10]
			}
		}
	},
	{
		$sort: {
			"numFilms": -1,
			"average": -1
		}
	},
	{
		$limit: 1
	}
]);
```

#### The $lookup stage

- Combine information from two collections.
- It's basically a "Left Outer Join".
- The `from` field cannot be from a sharded instance, it also has to be from the same database.
- Syntax:
```javascript
$lookup: {
	from: , // collection to join
	localField: , // field from the input documents
	foreignField: , // field from the documents of the "from" collection
	as: // output array field
}
```
- If there are no matches, it will return an empty array.
- Specifying an existing field name to `as` will overwrite the the existing field.
- `$lookup` matches between `localField` and `foreignField` with an equality match.

#### Lab - Using $group

- Which alliance from `air_alliances` flies the most `routes` with either a Boeing 747 or an Airbus A380 (abbreviated 747 and 380 in `air_routes`)?
- Answer:
```javascript
db.air_routes.aggregate([
	{
		$match: {
			airplane: /747|380/
		}
	},
	{
		$lookup: {
			from: "air_alliances",
			foreignField: "airlines",
			localField: "airline.name",
			as: "alliance"
		}
	},
	{
		$unwind: "$alliance"
	},
	{
		$group: {
			_id: "$alliance.name",
			count: {
				$sum: 1
			}
		}
	},
	{
		$sort: {
			count: -1
		}
	}
]);
```

#### $graphLookup introduction

- Dynamic data structures, and scalability.
- We can have flat documents, or deeply nested schemas.
- MongoDB is designed to be a general-purpose database.
- Transitive closure.
- Syntax:
```javascript
$graphLookup: {
	from: , // lookup table
	startWith: , // expression for value to start from
	connectFromField: , // field name to connect to
	connectToField: , // field name to connect to
	as: , // field name for result array
	maxDepth: , // optional, number of iterations to perform
	depthField: , // optional, field name for number of iterations to reach this node
	restrictSearchWithMatch: , // optional, match condition to apply to lookup
}
```

#### $graphLookup: Simple lookup

- Find all those who report to Eliot:
```javascript
db.parent_reference.aggregate([
	{
		$match: {
			"name": "Eliot"
		}
	},
	{
		$graphLookup: {
			from: "parent_reference",
			startWith: "$_id",
			connectFromField: "_id",
			connectToField: "reports_to",
			as: "all_reports"
		}
	}
]).pretty();
```
- Find all the bosses of Shannon:
```JavaScript
db.parent_reference.aggregate([
	{
		$match: {
			"name": "Shannon"
		}
	},
	{
		$graphLookup: {
			from: "parent_reference",
			startWith: "$reports_to",
			connectFromField: "reports_to",
			connectToField: "_id",
			as: "bosses"
		}
	}
]).pretty();
```
- `connectToField` will be used on recursive find operations.
- `connectFromField` value will be use to match `connectToField` in a recursive match.

#### $graphLookup: Simple lookup reverse schema

- Get all reports recursively from array of direct reports:
```javascript
db.child_reference.aggregate([
	{
		$match: {
			"name": "Dev"
		}
	},
	{
		$graphLookup: {
			from: "child_reference", // self lookup (it's own collection)
			startWith: "$direct_reports", // start at the field direct_reports
			connectFromField: "direct_reports", // whenever it finds another direct_reports field, search recursively
			connectToField: "name", // save the name field data
			as: "all_reports" // save returned data inside all_reports field
		}
	}
]).pretty();
```

#### $graphLookup: maxDepth and depthField

- Depth works like array, starting at zero (zero is one level deep, and one is two levels deep, etc.).
-  `depthField` indicates how many recursive lookups were needed to find the data, `depthField: 'level'`. `'level'` refers to what the field name will be called.
- `maxDepth` only takes `$long` values.
- `depthField` determines a field, which contains the value number of documents matched by the recursive lookup.

#### $graphLookup: Cross collection lookup

- You can lookup other collections.
- Using `restrictSearchWithMatch: {"airline.name": "TAP Portugal"}`, will restrict the data returned if it matches the query.

#### $graphLookup: General considerations

- Memory allocation, `$allowDiskUse`.
- Indexes, `connectToField`.
- Sharding, we cannot use a shard collection in our `from` collection.
- `$match`, unrelated match stages do not get pushed before `graphLookup` in our pipeline.
- `$graphLookup` can be used in any position of the pipeline and acts in the same way as a regular $lookup.

#### Lab: $graphLookup

- Find the list of all possible distinct destinations, with at most one layover, departing from the base airports of airlines that make part of the "OneWorld" alliance. The airlines should be national carriers from Germany, Spain or Canada only. Include both the destination and which airline services that location.
- Answer:
```javascript
db.air_alliances.aggregate([
	{ // This pipeline takes the most selective collection first, air_alliances, matching the document refering to the OneWorld alliance.
		$match: { name: "OneWorld" }
	},
	{ // It then iterates, with maxDepth 0 on the air_airlines collection to collect the details on the airlines, specially their base airport, but restricting that $lookup to airlines of the requested countries [Spain, Germany, Canada], using restrictSearchWithMatch.
		$graphLookup: {
			startWith: "$airlines",
			from: "air_airlines",
			connectFromField: "name",
			connectToField: "name",
			as: "airlines",
			maxDepth: 0,
			restrictSearchWithMatch: {
				country: { $in: ["Germany", "Spain", "Canada"] }
			}
		}
	},
	{ // We then iterate over all routes up to maximum of one layover by setting our maxDepth to 1. We find all possible destinations when departing from the base airport of each carrier by specify $airlines.base in startWith
		$graphLookup: {
			startWith: "$airlines.base",
			from: "air_routes",
			connectFromField: "dst_airport",
			connectToField: "src_airport",
			as: "connections",
			maxDepth: 1
		}
	},
	{ // We now have a document with a field named connections that is an array of all routes that are within 1 layover. We use a $project here to remove unnecessary information from the documents. We also need to include information about valid airlines that match our initial restriction and the name of the current airline.
		$project: {
			validAirlines: "$airlines.name",
			"connections.dst_airport": 1,
			"connections.airline.name": 1
		}
	},
	{ $unwind: "$connections" },
	{ // After this, we'll unwind our connections array, and then use $project to add a field representing whether this particular route is valid, meaning it is a route flown by one of our desired carriers.
		$project: {
			isValid: {
				$in: ["$connections.airline.name", "$validAirlines"]
			},
			"connections.dst_airport": 1
		}
	},
	{ $match: { isValid: true } },
	{ // Lastly, we use $match to filter out invalid routes, and then $group them on the destination.
		$group: {
			_id: "$connections.dst_airport"
		}
	}
]);
// An important aspect to this pipeline is that the first $graphLookup will act as a regular $lookup since we are setting a maxDepth to zero. The reason why we are taking this approach is due to the match restriction that $graphLookup allows, which can make this stage more efficient.
```

---

### Chapter 4: Core aggregation - Multidimensional grouping

#### Facets: introduction

- Manipulate, inspect, and analyze data.
- Application SLA = Service level agreement
- Faceting navigating - Enabling developers to creating interfaces to characterize query results.
- Faceting = Analytics capability.
- Facets are powered by the aggregate framework including:
	- Manual and auto bucketing.
	- Single facet query.
	- Rendering multiple facets.

#### Facets: Single facet query

- `$sortByCount` example:
```javascript
db.companies.aggregate([
	{
		$match: {
			"$text": {
				"$search": "network"
			}
		}
	},
	{
		"$sortByCount": "$category_code"
	}
]);
```
- Single query facets are supported by the new aggregation pipeline stage `$sortByCount`.
- As like any other aggregation pipelines, except for `$out`, we can use the output of this stage, as input for downstream stages and operators, manipulating the dataset accordingly.

#### The $bucket stage

- `$bucket` syntax:
```javascript
// syntax
$bucket: {
	groupBy: // groups on evaluated express, only 1 expression
	boundaries: // array of values with the lower inclusive group and exclusive upper bound, must use at least 2. They must all be the same type.
	default: // optional, if the value is not in the bucket, you can specify a default value to be placed
	output: {
		output1: //accumulator expression
		// ...
	}
}
// example
db.movies.aggregate([
	{
		$bucket: {
			groupBy: "$imdb.rating",
			boundaries: [0, 5, 8, Infinity],
			default: "not rated",
			output: {
				average_per_bucket: {
					$avg: "$imdb.rating"
				},
				count: {
					$sum: 1
				}
			}
		}
	}
]);
```

#### Facets: Manual buckets

- Buckets are usually grouped by values between a minimum and maximum, inside the boundaries.
- All boundaries must have the same value.

#### The $bucketAuto stage

- `$bucketAuto` syntax:
```javascript
// syntax
$bucket: {
	groupBy: // groups on evaluated express, only 1 expression
	buckets: // integer to calculate the number of buckets for the script to calculate automatically what the boundaries are. This may be less than what we specify if we specify 10, but only have 8 items in the collection
	output: {
		output1: //accumulator expression
		// ...
	},
	granularity: // optional, attempt to organize using the product theories (renard, power of 2, etc.)
}
// example
db.movies.aggregate([
	{
		$match: {
			"imdb.rating": {
				$gte: 0
			}
		}
	},
	{
		$bucketAuto: {
			groupBy: "$imdb.rating",
			buckets: 4,
			output: {
				average_per_bucket: {
					$avg: "$imdb.rating"
				},
				count: {
					$sum: 1
				}
			}
		}
	}
]);
```

#### Facets: Auto buckets

- The `granularity` parameter refers to theories of number organization (R5, R20, etc.).
- Given a number of buckets, try to distribute documents evenly accross buckets.
- Adhere bucket boundaries to a numerical series set by the `granularity` option.

#### Facets: Multiple facets

- `$facet: {}`.
- Facets are sort of like sub-pipelines.
- The `$facet` stage allows several sub-pipelines to be executed to produce multiple facets.
- The `$facet` stage allows the application to generate several different facets with one single database request.

#### Lab - $facets

- How many movies are in both the top ten highest rated movies according to the imdb.rating and the metacritic fields? We should get these results with exactly one access to the database.
- Answer:
```JavaScript
db.movies.aggregate([
	{ // We begin with a $match and $project stage to only look at documents with the relevant fields, and project away needless information
		$match: {
			metacritic: { $gte: 0 },
			"imdb.rating": { $gte: 0 }
		}
	},
	{
		$project: {
			_id: 0,
			metacritic: 1,
			imdb: 1,
			title: 1
		}
	},
	{ // Next follows our $facet stage. Within each facet, we need sort in descending order for metacritic and imdb.ratting and ascending for title, limit to 10 documents, then only retain the title
		$facet: {
			top_metacritic: [
				{
					$sort: {
						metacritic: -1,
						title: 1
					}
				},
				{
					$limit: 10
				},
				{
					$project: {
						title: 1
					}
				}
			],
			top_imdb: [
				{
					$sort: {
						"imdb.rating": -1,
						title: 1
					}
				},
				{
					$limit: 10
				},
				{
					$project: {
						title: 1
					}
				}
			]
		}
	},
	{ // Lastly, we use a $project stage to find the intersection of top_metacritic and top_imdb, producing the titles of movies in both categories
		$project: {
			movies_in_both: {
				$setIntersection: ["$top_metacritic", "$top_imdb"]
			}
		}
	}
]);
```

#### The $sortByCount stage

- It takes only one argument.
- Syntax: `{$sortByCount: "$imdb.rating"}`.
- Is equivalent to a group stage to count occurrence, and then sorting in descending order.

---

### Chapter 5: Miscellaneous Aggregation

#### The $redact stage

- Syntax: `$redact: [expression]`.
- One of three expressions:
	- `$$DESCEND` - Retain this level, escept for sub-documents and arrays of documents.
	- `$$PRUNE` - Exclude all fields at the current document level without further document inspection.
	- `$$PRUNE` - Include all fields at the current document level without further document inspection.
- Example:
```JavaScript
db.employees.aggregate([
	{
		$redact: {
			$cond: [
				{
					$in: ["Management", "$acl"]
				},
				"$$DESCEND",
				"$$PRUNE"
			]
		}
	}
]).pretty();
```
- `$$KEEP` and `$$PRUNE` automatically apply to all levels below the evaluated level.
- `$$DESCEND` retains the current level and evaluates the next level down.
- `$redact` is not for restricting access to a collection.

#### The $out stage

- Syntax: `$out: "output_collection"`.
- Must be the last stage of the pipeline.
- Must not be part of a facet.
- Creates collections in the same database as the source collection.
- Creates a new collection for the pipeline output, or overwrite collection if specified.
- Honours indexes on existing collections.
- Will not create or overwrite data is pipeline errors.

#### Views

- From the user, views are perceived as collections.
- Views allow us to create vertical (performed through the use of a `$project` stage, modifying individual documents) and horizontal (performed through the use of a `$match` stage, changes the number of documents returned, not the shape) slices of our collections.
- Example:
```javascript
db.createView("bronze_banking", "customers", [
	{
		"$match": { "accountType": "bronze" }
	},
	{
		"$project": {
			"_id": 0,
			"name": {
				"$concat": [
					{ "$cond": [{ "$eq": ["$gender", "female"] }, "Miss", "Mr."] },
					" ",
					"$name.first",
					" ",
					"$name.last"
				]
			},
			"phone": 1,
			"email": 1,
			"address": 1,
			"account_ending": { "$substr": ["$accountNumber", 7, -1] }
		}
	}
]);
```
- Views have some restrictions:
	- No write operations.
	- No index operations.
	- No renaming.
	- Collation restrictions.
	- No `mapReduce`.
	- No `$text`.
	- No `geoNear` or `$geoNear`.
	- `find()` operations with projection operators are not permitted (`$`, `$elemMatch`, `$slice`, `$meta`).
- View definitions are public.
- Avoid referring to sensitive fields within the pipeline.
- Views contain no data themselves. They are created on demand and reflect the data in the source collection.
- View performance can be increased by creating the appropriate indexes on the source collection.

---

### Chapter 6: Aggregation performance and pipeline optimization

#### Aggregation performance

- Index usage.
- Memory constraints.
- "Realtime" processing:
	- You can never have truly realtime processing,.
	- Provide data for applications.
	- Query performance is more important.
- Batch processing:
	- Provide data for analytics.
	- Query performance is less important.
- Aggregation queries should use indexes as much as possible, to be more efficient.
- Append `{explain: true}` to the end of the aggregation pipeline to get more information on your query results.
- `$match` can use indexes, especially if it uses indexes.
- `$sort` should be placed early in the pipeline.
- If you use `$limit` and `$sort` together, they should be early on in the pipeline. `$match` then `$limit` then `$sort` is one of the most efficient aggregation pipelines used without an index.
- Results are subject to 16MB document limit. This is just the for the final returned data, the data flowing through the pipeline can be greater than 16MB.
- Use `$limit` and `$project` to keep document returned size low.
- You are limited to 100MB of RAM per stage, so use indexes.
- You can extend the RAM limit by appending `{allowDiskUse: true}` to spill usage into the disk. This should be used as a last resort only.
- `{allowDiskUse: true}` does not work with `$graphLookup`.
- When `$limit` and `$sort` are close together a very performant top-k sort can be performed.
- Transforming data in a pipeline stage prevents us from using indexes in the stages that follow.

#### Aggregation pipeline on a sharded cluster

- `sh.shardCollection('m201.restaurants', {"address.state": 1})`.
- Use the `$match` aggregation pipeline on the `address.state` to directly query a single shard.
- `$$out` and `$lookup` (but not `$group`) operators will cause a merge stage on the primary shard for a database.

#### Pipeline optimization - Part 1

- You can use a regex on `$match` without specifying regex: `{$match: {title: /^[aeiou]/i}}`.
- If the pipeline explanation shows a fetch, that means that the query had to dive into a document to get more information. Rather than skimming it because of a useful index.
- Avoid needless projects.


#### Pipeline optimization - Part 2

- One to end pattern, such as the attribute or subset pattern.

- Avoid unnecessary stages, the aggregation framework can project fields automatically if final shape of the output document can be determined from initial input.
- Use accumulator expressions, `$map`, `$filter`, `$reduce` in a project before an `$unwind`, if possible.
- Every high order array function can be implemented with `$reduce` if the provided expressions do not meet your needs.
- Causing a merge in a sharded deployment will cause all subsequent pipeline stages to be performed in the same location as the merge.
- The query in a `$match` stage can be entirely covered by an index.
- The Aggregation Framework can automatically project fields if the shape of the final document is only dependent upon those fields in the input document.
- The Aggregation Framework will automatically reorder stages in certain conditions.
