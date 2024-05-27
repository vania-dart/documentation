---
sidebar_label: 'Migrations'
sidebar_position: 3
---


# Introduction

Migrations are like version control for your database, allowing your team to define and share the application's database schema definition. If you have ever had to tell a teammate to manually add a column to their local database schema after pulling in your changes from source control, you've faced the problem that database migrations solve.

## Generating Migrations

You may use the make:migration Vania-cli command to generate a database migration. The new migration will be placed in your database/migrations directory:

```shell
 vania make:migration create_posts_table
```

Vania-cli will use the name of the migration to attempt to guess the name of the table and whether or not the migration will be creating a new table, If Vania-cli is able to determine the table name from the migration name, Vania-cli will pre-fill the generated migration file with the specified table. Otherwise, you may simply specify the table in the migration file manually.

## Migration Structure

A migration class extends Migration base class from Vania and contains constructor, the constructor contains up method. The up method is used to add new tables, columns, or indexes to your database

## Configuring the Database Connection

In `env` file, you can specify your database information such as host, port, username, password, and database name.

```env
DB_CONNECTION=mysql # databse driver mysql or postgresql or pgsql
DB_HOST=localhost
DB_PORT=3306
DB_DATABASE=vania
DB_USERNAME=root
DB_PASSWORD=password
DB_SSL_MODE=false
DB_POOL=false
DB_POOL_SIZE=2
```

:::info

If your MySQL server doesn't support SSL mode you can add `DB_SSL_MODE= false` in the config

If you are using PostgresSql you can use DB_PREFIX and DB_SCHEMA too

:::

## Running Migrations

To run all of your outstanding migrations, execute the migrate Artisan command:

```dart
vania migrate
```

## Creating Tables

To create a new database table, use the createTableNotExists or createTable  method. These methods accepts two arguments: the first is the name of the table, while the second is a closure:

The function `createTableNotExists` first checks if the table does not exist, and then tries to create the table

The function `createTable` first drops the old table and then tries to create a new one

```dart
await createTableNotExists('TableName', () {
      id();
    });

await createTable('TableName', () {
      id();
    });
```

When creating the table, you may use any of the column methods to define the table's columns.

## Available Column Types

### bigIncrements()

The bigIncrements method creates an auto-incrementing UNSIGNED BIGINT (primary key) equivalent column:

```dart
bigIncrements('id')
```

### bigInt()

The bigInt method creates a BIGINT equivalent column:

```dart
bigInt('votes')
```

### binary()

The binary method creates a BLOB equivalent column:

```dart
binary('photo')
```

### char()

The char method creates a CHAR equivalent column with of a given length:

```dart
char('name',length: 100)
```

### dateTime()

The dateTime method creates a DATETIME equivalent

```dart
dateTime('created_at')
```

### date()

The date method creates a DATE equivalent column:

```dart
date('created_at')
```

### decimal()

The decimal method creates a DECIMAL equivalent column with the given precision (total digits) and scale (decimal digits):

```dart
decimal('amount',precision:8,scale:2)
```

### double()

The double method creates a DOUBLE equivalent column with the given precision (total digits) and scale (decimal digits):

```dart
double('amount',precision:8,scale:2)
```

### enumType()

The enum method creates a ENUM equivalent column with the given valid values:

```dart
enumType('difficulty', enumValues: ['easy', 'hard'])
```

### float()

The float method creates a FLOAT equivalent column with the given precision (total digits) and scale (decimal digits):

```dart
float('amount',precision:8,scale:2)
```

### foreign()

Creates a foreign key constraint on the specified [columnName] in the current table,
referencing the [referencesColumn] in the [referencesTable].

By default, the foreign key is not constrained, meaning it allows NULL values.
Set [constrained] to `true` to enforce the foreign key constraint.

You can specify the behavior on update and delete actions using [onUpdate] and [onDelete]
parameters. By default, both are set to 'NO ACTION'.

```dart
foreign('product_id', 'products', 'id', constrained: true, onDelete: 'CASCADE');
```

This creates a foreign key on the 'product_id' column in the current table, which
references the 'id' column in the 'products' table. The foreign key is constrained
and set to 'CASCADE' on delete.

### geometryCollection()

The geometryCollection method creates a GEOMETRYCOLLECTION equivalent column:

```dart
geometryCollection('positions')
```

### geometry()

The geometry method creates a GEOMETRY equivalent column:

```dart
geometry('positions')
```

### id()

The id method is an alias of the bigIncrements method. By default, the method will create an id column; however, you may pass a column name if you would like to assign a different name to the column:

```dart
id()
```

### integer()

The integer method creates an INTEGER equivalent column:

```dart
integer('votes')
```

### json()

The json method creates a JSON equivalent column:

```dart
json('options')
```

### lineString()

The lineString method creates a LINESTRING equivalent column:

```dart
lineString('positions')
```

### longText()

The longText method creates a LONGTEXT equivalent column:

```dart
longText('description')
```

### mediumInt()

The mediumInt method creates a MEDIUMINT equivalent column:

```dart
mediumInt('votes')
```

### mediumText()

The mediumText method creates a MEDIUMTEXT equivalent column:

```dart
mediumText('description')
```

### multiLineString()

The multiLineString method creates a MULTILINESTRING equivalent column:

```dart
multiLineString('positions')
```

### multiPolygon()

The multiPolygon method creates a MULTIPOLYGON equivalent column:

```dart
multiPolygon('positions')
```

### point()

The point method creates a POINT equivalent column:

```dart
point('positions')
```

### polygon()

The polygon method creates a POLYGON equivalent column:

```dart
polygon('positions')
```

### set()

The set method creates a SET equivalent column with the given list of valid values:

```dart
set('positions',setValues:[])
```

### smallInt()

The smallInt method creates a SMALLINT equivalent column:

```dart
smallInt('votes');
```

### softDeletes()

The softDeletes method adds a nullable deleted_at TIMESTAMP equivalent column. This column is intended to store the deleted_at timestamp:

```dart
softDeletes('deleted_at');
```

### string()

The string method creates a VARCHAR equivalent column of the given length:

```dart
string('name', length:100);
```

### text()

The text method creates a TEXT equivalent column:

```dart
text('description');
```

### time()

The time method creates a TIME equivalent column:

```dart
time('sunrise');
```

### timestamp()

The timestamp method creates a TIMESTAMP equivalent column:

```dart
timestamp('sunrise');
```

### tinyInt()

The tinyInt method creates a TINYINT equivalent column:

```dart
tinyInt('votes');
```

### tinyText()

The tinyText method creates a TINYTEXT equivalent column:

```dart
tinyText('notes');
```

### uuid()

The uuid method creates a UUID equivalent column:

```dart
uuid('id');
```

### year()

The year method creates a YEAR equivalent column:

```dart
year('birth_year');
```
