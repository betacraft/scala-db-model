Scala-db-model
==============

scala-db-model library is targeted towards creating a very flexible, automated migration , database independent ORM
library for Scala. There are quite a few ORM libraries available for scala like 'scala-migrations',
'scala-active-records', 'squirl' etc. But none of them are spot on to the solution.

scala-db-model library is inspired by Django's ORM system which does all the migration work almost automatically
without caring about the db level changes. Entire modifications have to be done on the class which is extending model
 and you are presented with ready-made scripts for migrations.

This library is in a very nascent phase so <bold>IS NOT RECOMMENDED FOR PRODUCTION USE</bold>.


## Architecture

(Initial draft)

```
                                     +------------------+
                                     |                  |
                           +---------+ MigrationManager +-------+
                           |         |                  |       |
                           |         +------------------+       |
                           |                                    |
                      +----+----+    +-------------------+      |
                      |         |    |                   |      |
                      | DBModel +----+ QueryManager      |      |
                      |         |    |                   |      |
                      +---------+    +-------+-----------+      |
                                             |                  |
                                     +-------+-----------+      |
                                     |                   |      |
                                     | DBManager         +------+
                                     |                   |
                                     +--------+----------+
                                              |
                                            +-v---+
                                            |     |
                                            |-----|
                                            |     |
                                            +-----+

```

## Components

### DBModel

Main class that will be extended by every class which is supposed to be mapped onto the database. This class will
provide basic CRUD functionality for the class with some advanced features for searching records and all. Along with
this, it will provide a feature of associating the model with other db models using standard relations
1. one-to-one field
2. one-to-many field
3. many-to-one field
4. many-to-many field
5. many-to-many-with field
with the feature of providing custom foreign key and other attributes via annotations. (Will check if possible with
macros)

### DBManager

This is the only point from where DB communication will happen. So this class will generate db dependent queries.
Initially we will start with postgres support. As the library matures we will go on adding more dbs.

### MigrationManager (The most trickiest part)

This module will be responsible for creating, updating and performing migrations on the database via DBManager. Now
as the only point of updating a table is class that is extending DBModel, it has to check before every compile that
is there any change in the models and if there are any then detect the changes and create migration.

### Query manager

To make this library db independent we will have our own custom in-between queries,
called faql (fake sql which are db independent). And they will be translated by db manager to the corresponding db
based sql.


## Current release feature list

- DBModel
- Basic annotations for fields
    * primary_key
    * unique
    * required
    * email
    * transient
- Migration manager without change detection
- DB manager supporting Postgres
- Query manager with basic CRUD functionalities


## For queries or joining project

Contact me at akshay@rainigclouds.com