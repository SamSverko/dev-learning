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

* `psql [db-name]` Access the database via the PSQL.

* Inside PSQL: `SELECT version();` View the PostgreSQL version of your database.

* Inside PSQL: `SELECT current_date;` Prints out the current date.

* It is important to include the `;` character before hitting enter, because without, it will just make another line of SQL, rather than executing it.

* Inside PSQL: `\h` Show a list of all PSQL commands.

* Inside PSQL: `\q` Exit the PSQL.

* Inside PSQL: `\! clear` Clears the terminal.

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

// https://www.postgresql.org/docs/11/tutorial-window.html
