# [postgresql](https://www.postgresql.org)

---

## Getting started

### Architectural fundamentals

* PostgreSQL uses a client/ server model.

* A PostgreSQL session consists of the following cooperating processes (programs):

	* A server process, which manages the database files, accepts connections to the database from client applications, and performs database actions on behalf of the clients. The database server program is called `postgres`.

	* The user's client (frontend) application that wants to perform database operations. Client applications can be very diverse in nature: a client could be a text-oriented tool, a graphical application, a web server that accesses the database to display web pages, or a specialized database maintenance tool. Some client applications are supplied with the PostgreSQL distribution; most are developed by users.

* The client/ server connections communicate over a TCP/IP network connection.

### Creating a database

* `createdb [db-name]` Creates a database.

* `dropdb [db-name]` Removes database.

### Accessing a database

* You can access a created database by running the _PostgreSQL interactive terminal program_ (PSQL).

* `psql [db-name]` Access the database via the PSQL. It will access PSQL using the default user.

* `psql -U [user-name] [database-name]` or `psql [database-name] [user-name]` Access the PSQL with specified user. By default, PostgreSQL tries to connect to a database with the same name as your user. To prevent this, specify a user and a database.

* Inside PSQL: `SELECT version();` View the PostgreSQL version of your database.

* Inside PSQL: `SELECT current_date;` Prints out the current date.

* It is important to include the `;` character before hitting enter, because without, it will just make another line of SQL, rather than executing it.

* Inside PSQL: `\h` Show a list of all PSQL commands.

* Inside PSQL: `\q` Exit the PSQL.

* Inside PSQL: `\! clear` Clears the terminal.

* `psql -h [aws-endpoint] -U [user-name] -d [database-name]` SSL connection to remote AWS RDS database. The `-h` refers to host, `-U` refers to user, and `-d` refers to database. You will then connect to the remote PSQL.

---

## The SQL Language

### Concepts

* PostgreSQL is a relational _database management system_ (RDBMS). That means it is a system for managing data stored in _relations_. Relation is essentially a mathematical term for _table_.

* Each table is a named collection of _rows_. Each row of a given table has the same set of named _columns_, and each column is of a specific data type. Whereas columns have a fixed order in each row, it is important to remember that SQL does not guarantee the order of the rows within the table in any way (although they can be explicitly sorted for display).

* Tables are grouped into _databases_, and a collection of databases managed by a single PostgreSQL server instance constitutes a _database cluster_.

* In PSQL: `\d` Displays list of relations (tables).

* In PSQL: `\d [table-name]` Displays the table schema.

* In PSQL: `\l` Displays list of databases

* In PSQL: `\c [database-name]` Disconnect from current database and connect to another database.

* When you create a local database, the database URL for the system environment should be: `postgres://{db_username}:{db_password}@{host}:{port}/{db_name}`.

### Creating a new table

* Inside PSQL: Two dashes (`--`) introduce comments.

* Inside: PSQL: `varchar(80)` specifies a data type that can store arbitrary character strings up to 80 characters in length.

* Inside PSQL: `int` is the normal integer type. `real` is a type for storing single precision floating-point numbers.

* Create a new table:
	```
	CREATE TABLE weather (
		city            varchar(80),
		temp_lo         int,           -- low temperature
		temp_hi         int,           -- high temperature
		prcp            real,          -- precipitation
		date            date
	);
	```

* In PSQL: `DROP TABLE [table-name];` Removes the table.

### Populating a table with rows

* In PSQL: `INSERT INTO [table-name] VALUES ('string', 10, 50, 0.25, '1994-11-27');` Inserts data into table.

* The syntax used so far requires you to remember the order of the columns. An alternative syntax allows you to list the columns explicitly:
	```
	INSERT INTO weather (city, temp_lo, temp_hi, prcp, date)
		VALUES ('San Francisco', 43, 57, 0.0, '1994-11-29');
	```

* In PSQL: `COPY weather FROM '/home/user/weather.txt';` You could also have used COPY to load large amounts of data from flat-text files.

### Querying a table

* To retrieve data from a table, the table is _queried_. An SQL `SELECT` statement is used to do this. The statement is divided into a select list (the part that lists the columns to be returned), a table list (the part that lists the tables from which to retrieve the data), and an optional qualification (the part that specifies any restrictions).

* In PSQL: `SELECT * FROM [table-name];` Retrieve all the rows from the table. Here `*` is a shorthand for “all columns”.

* A query can be “qualified” by adding a WHERE clause that specifies which rows are wanted. The WHERE clause contains a Boolean (truth value) expression, and only rows for which the Boolean expression is true are returned. The usual Boolean operators (AND, OR, and NOT) are allowed in the qualification. For example, the following retrieves the weather of San Francisco on rainy days: In PSQL:
	```
	SELECT * FROM weather
	    WHERE city = 'San Francisco' AND prcp > 0.0;
	```
* In PSQL: `SELECT * FROM weather ORDER BY city;` Results of a query will be sorted by city.

* In PSQL: `SELECT DISTINCT city FROM weather` Duplicate rows will be removed from query result.

### Joins between tables

* A query that accesses multiple rows of the same or different tables at one time is called a _join_ query.

* Since the columns all had different names, the parser automatically found which table they belong to. If there were duplicate column names in the two tables you'd need to _qualify_ the column names to show which one you meant, as in: In PSQL:
	```
	SELECT weather.city, weather.temp_lo, weather.temp_hi,
			weather.prcp, weather.date, cities.location
    	FROM weather, cities
    	WHERE cities.name = weather.city;
	```

* An alternative query form can be seen here: In PSQL: `SELECT * FROM weather INNER JOIN cities ON (weather.city = cities.name);`.

* In PSQL: `SELECT * FROM weather LEFT OUTER JOIN cities ON (weather.city = cities.name);` An _outer join_ query is if no matching row is found we want some “empty values” to be substituted.

* We can also join a table against itself. This is called a _self join_.

* You can abbreviations: In PSQL: `SELECT * FROM weather w, cities c WHERE w.city = c.name;`.

### Aggregate functions

* An aggregate function computes a single result from multiple input rows. For example, there are aggregates to compute the `count`, `sum`, `avg` (average), `max` (maximum) and `min` (minimum) over a set of rows.

* In PSQL: `SELECT max(temp_lo) FROM weather;` Fin the highest low-temperature reading.

* Select the city that had the highest low-temperature: In PSQL: `SELECT city FROM weather WHERE temp_lo = (SELECT max(temp_lo) FROM weather);`

* Get maximum low temperature observed in each city: In PSQL:
	```
	SELECT city, max(temp_lo)
	    FROM weather
	    GROUP BY city;
	```

### Updates

* Use the `UPDATE` command. Let's say we want to decrease all temperatures by 2 after November 28. In PSQL:
	```
	UPDATE weather
	    SET temp_hi = temp_hi - 2,  temp_lo = temp_lo - 2
	    WHERE date > '1994-11-28';
	```

### Deletions

* Rows can be removed from a table using the `DELETE` command. In PSQL: `DELETE FROM weather WHERE city = 'Hayward';`.
* In PSQL: `DELETE FROM tablename;` This will remove _all_ rows from the given table, leaving it empty. The system will not request confirmation before doing this!

---

## Advanced features

### Views

* If you do not want to type the query each time you need it. You can create a _view_ over the query, which gives a name to the query that you can refer to like an ordinary table. In PSQL:
	```
	CREATE VIEW myview AS
    	SELECT city, temp_lo, temp_hi, prcp, date, location
        	FROM weather, cities
        	WHERE city = name;

	SELECT * FROM myview;
	```

* Inside PSQL: `DROP VIEW [view-name]` Removes the view.

### Foreign keys

* You want to make sure that no one can insert rows in the weather table that do not have a matching entry in the cities table. This is called _maintaining the referential integrity_ of your data. In PSQL:
	```
	CREATE TABLE cities (
		city     varchar(80) primary key,
		location point
	);

	CREATE TABLE weather (
		city      varchar(80) references cities(city),
		temp_lo   int,
		temp_hi   int,
		prcp      real,
		date      date
	);
	```

### Transactions

* _Transactions_ are a fundamental concept of all database systems. The essential point of a transaction is that it bundles multiple steps into a single, all-or-nothing operation. The intermediate states between the steps are not visible to other concurrent transactions, and if some failure occurs that prevents the transaction from completing, then none of the steps affect the database at all. In PSQL:
	```
	BEGIN;
	UPDATE accounts SET balance = balance - 100.00
	    WHERE name = 'Alice';
	-- etc etc
	COMMIT;
	```

* PostgreSQL actually treats every SQL statement as being executed within a transaction. If you do not issue a `BEGIN` command, then each individual statement has an implicit `BEGIN` and (if successful) `COMMIT` wrapped around it. A group of statements surrounded by `BEGIN` and `COMMIT` is sometimes called a _transaction block_.

### Window functions

* A window function performs a calculation across a set of table rows that are somehow related to the current row.

### Inheritance

* A row of capitals _inherits_ all columns (`name`, `population`, and `altitude`) from its _parent_, `cities`. In PSQL:
	```
	CREATE TABLE cities (
		name       text,
		population real,
		altitude   int     -- (in ft)
	);

	CREATE TABLE capitals (
		state      char(2)
	) INHERITS (cities);
	```

---

## Database roles and privileges

* PostgreSQL manages database access permissions using the concept of _roles_. A role can be thought of as either a database user, or a group of database users, depending on how the role is set up. Roles can own database objects (for example, tables) and can assign privileges on those objects to other roles to control who has access to which objects. Furthermore, it is possible to grant _membership_ in a role to another role, thus allowing the member role use of privileges assigned to the role it is a member of.

* Database roles are conceptually completely separate from operating system users. In practice it might be convenient to maintain a correspondence, but this is not required. Database roles are global across a database cluster installation (and not per individual database).

* In PSQL: `\du` Display list of roles. Add a `+` for additional details.

* In PSQL: `\dp [optional-table-name]` Display list of access privileges.

### Database roles

* In PSQL: `CREATE ROLE [role-name]` or `createuser [user-name]` Create a role, or user `name` follows the rules for SQL identifiers: either unadorned without special characters, or double-quoted.

* In PSQL: `DROP ROLE [role-name]` or `dropuser [user-name]` Remove role, or user.

* If you are unable to drop a role because it has dependant objects, you can reassign what the role owns to another:

	* In PSQL: `REASSIGN OWNED BY [role-name] TO [role-name];`

	* In PSQL: `DROP OWNED BY [role-name];`

	* In PSQL: `DROP USER [role-name];`

* To determine the set of existing roles, examine the pg_roles system catalog: In PSQ: `SELECT rolname from pg_roles`;

### Role attributes

* In PSQL: `CREATE ROLE name LOGIN;` `login privilege` - Only roles that have the `LOGIN` attribute can be used as the initial role name for a database connection. A role with the `LOGIN` attribute can be considered the same thing as a "database user". In PSQL:
	```
	CREATE ROLE [role-name] LOGIN;
	CREATE USER [role-name];
	```

* In PSQL: `CREATE ROLE [role-name] SUPERUSER;` `superuser status` - A database superuser bypasses all permission checks. This is a dangerous privilege and should not be used carelessly; it is best to do most of your work as a role that is not a superuser. You must do this as a role that is already a superuser.

* In PSQL: `CREATE ROLE [role-name] CREATEDB` `database creation` - A role must be explicitly given permission to create databases (except for superusers, since those bypass all permission checks).

* IN PSQL: `CREATE ROLE [role-name] CREATEROLE` `role creation` - A role must be explicitly given permission to create more roles (except for superusers, since those bypass all permission checks). A role with `CREATEROLE` privilege can alter and drop other roles, too, as well as grant or revoke membership in them. However, to create, alter, drop, or change membership of a superuser role, superuser status is required; `CREATEROLE` is not sufficient for that.

* In PSQL: `CREATE ROLE [role-name] REPLICATION LOGIN` `initiating replication` - A role must explicitly be given permission to initiate streaming replication. A role used for streaming replication must have LOGIN permission as well.

* In PSQL: `CREATE ROLE [role-name] PASSWORD 'string'` `password` - A password is only significant if the client authentication method requires the user to supply a password when connecting to the database. The password, `md5`, and `crypt` authentication methods make use of passwords. Database passwords are separate from operating system passwords.

* In PSQL: `ALTER ROLE [role-name] ATTRIBUTE` Add attribute to existing role. Such as replacing `ATTRIBUTE` with `LOGIN` to give that role login ability.

* It is good practice to create a role that has the CREATEDB and CREATEROLE privileges, but is not a superuser, and then use this role for all routine management of databases and roles. This approach avoids the dangers of operating as a superuser for tasks that do not really require it.

### Privileges

* When an object is created, it is assigned an owner. The owner is normally the role that executed the creation statement. For most kinds of objects, the initial state is that only the owner (or a superuser) can do anything with the object. To allow other roles to use it, privileges must be granted.

* There are several different kinds of privilege:
	* `SELECT` `r` Allows `SELECT` from any column of the specified table, view, or sequence. Also allows the use of `COPY TO`. This privilege is also needed to reference existing column values in `UPDATE` or DELETE. For sequences, this privilege also allows the use of the `currval` function.

	* `INSERT` `a` Allows `INSERT` of a new row into the specified table. Also allows `COPY FROM`.

	* `UPDATE` `w` Allows `UPDATE` of any column of the specified table. (In practice, any nontrivial `UPDATE` command will require `SELECT` privilege as well, since it must reference table columns to determine which rows to update, and/or to compute new values for columns.) `SELECT ... FOR UPDATE` and `SELECT ... FOR SHARE` also require this privilege, in addition to the `SELECT` privilege. For sequences, this privilege allows the use of the `nextval` and `setval` functions.

	* `DELETE` `d` Allows `DELETE` of a row from the specified table. (In practice, any nontrivial `DELETE` command will require `SELECT` privilege as well, since it must reference table columns to determine which rows to delete.)

	* `TRUNCATE` `D` Quickly removes all rows from a set of tables. It has the same effect as an unqualified DELETE on each table, but since it does not actually scan the tables it is faster.

	* `REFERENCES` `x` To create a foreign key constraint, it is necessary to have this privilege on both the referencing and referenced tables.

	* `TRIGGER` `t` Allows the creation of a trigger on the specified table.

	* `CREATE` `C`

		* For databases, allows new schemas to be created within the database.

		* For schemas, allows new objects to be created within the schema. To rename an existing object, you must own the object and have this privilege for the containing schema.

		* For tablespaces, allows tables, indexes, and temporary files to be created within the tablespace, and allows databases to be created that have the tablespace as their default tablespace. (Note that revoking this privilege will not alter the placement of existing objects.)

	* `CONNECT` `c` Allows the user to connect to the specified database. This privilege is checked at connection startup (in addition to checking any restrictions imposed by `pg_hba.conf`).

	* `TEMPORARY` `T` or `TEMP` Allows temporary tables to be created while using the specified database.

	* `EXECUTE` `X` Allows the use of the specified function and the use of any operators that are implemented on top of the function. This is the only type of privilege that is applicable to functions. (This syntax works for aggregate functions, as well.)

	* `USAGE` `U`

		* For procedural languages, allows the use of the specified language for the creation of functions in that language. This is the only type of privilege that is applicable to procedural languages.

		* For schemas, allows access to objects contained in the specified schema (assuming that the objects' own privilege requirements are also met). Essentially this allows the grantee to "look up" objects within the schema. Without this permission, it is still possible to see the object names, e.g. by querying the system tables. Also, after revoking this permission, existing backends might have statements that have previously performed this lookup, so this is not a completely secure way to prevent object access.

		* For sequences, this privilege allows the use of the `currval` and `nextval` functions.

		* `ALL PRIVILEGES` `arwDxt` Grant all of the available privileges at once. The `PRIVILEGES` key word is optional in PostgreSQL, though it is required by strict SQL.

* To assign privileges, the `GRANT` command is used. In PSQL: `GRANT UPDATE ON [table-name] TO [user-name]`.

* The special name `PUBLIC` can be used to grant a privilege to every role on the system. Writing ALL in place of a specific privilege specifies that all privileges that apply to the object will be granted.

* In PSQL: `REVOKE ALL ON [table-name] FROM [role-name]` Revoke a privilege.

### Role membership

* It is frequently convenient to group users together to ease management of privileges: that way, privileges can be granted to, or revoked from, a group as a whole. In PostgreSQL this is done by creating a role that represents the group, and then granting membership in the group role to individual user roles.

* In PSQL: `CREATE ROLE name;` To setup a group role, first create a role.

* Once the group role exists, you can add and remove members using the GRANT and REVOKE commands: In PSQL:
	```
	GRANT group_role TO role1, ... ;
	REVOKE group_role FROM role1, ... ;
	```

### Errors

* You can view the entire [list of PostgreSQL error codes](https://www.postgresql.org/docs/10/errcodes-appendix.html).
