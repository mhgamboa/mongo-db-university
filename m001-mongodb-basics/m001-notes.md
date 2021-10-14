# m001: MongoDB Basics

## Chapter 1: what is MongoDB?

- MongoDB is a nosql (meaning it doesn't use tables of data like excel or sql) document database
- A **Document** is a way to store data in field-value pairs (Like a Javascript object)
  - Think of this as a row in excel
  - A **field** would be the column name and a **value** would be the cell value within a column
- A **Collection** is a group of documents
  - Think of this as an excel tab
- A **database** is a group of collections.
  - Think of this as the entire excel file
- MongoDB **Atlas** is a database as a service used for MongoDB
- **Clusters** are groups of servers that host your data
  - **Replica sets** are essentially additional copies of your data (called an instance ). This insures that everything is properly backed up in case of any damage
  - An **instance** is a single machine that stores your data in the cloud
- Atlas Interface - The left navigation pane is cluster management

## Chapter 2: Importing, Exporting, and Querying Data

- Documents look just like JSON.
  - All Keys must be in quotation marks
  - Documents can have sub-documents
  - MongoDB technically stores data as BSON (Binary JSON) instead of JSON for better performance
- BSON commands:
  - **BSON import**: `mongorestore`
  - **BSON export**: `mongodump`
- JSON commands:
  - **JSON import**: `mongoimport`
  - **JSON export**: `mongoexport`
- **URI** stands for Uniform Resource Identifier
  - TL;DR A URI is a URL for you to connect to your MongoDB database
  - MongoDB uses an **srv** string format for the URI.
  - URI format:
    - `"mongodb+srv://<your username>:<your password>@<your cluster>.mongodb.net/<database name >"`
    - If no database is specified, then it will default to `"test"`
      If you use `"admin"` You'll be given more administrative privileges (one of which is to keep track of users that have access to the database)
- Data Explorer:
  - In MongoDB Atlas click browse collections
  - In name space on the left you can search for a specific collection:
    - The format is `database_name.collection_name`
  - Searching a collection is done in JSON format at the top:
    - `{"filter": "example", "filter2": "example2"}`

### Querying Data

- Connect to the database via your URI
- `show dbs` will show list of all available databases
- `use database_name` to select a database you want to work with
- `show collections` will show all collections in the database you have navigated to
- Once you've navigated to the proper database you can use the `find` command to query your data. EX:
  - `db.collectionName.find({"filter": "example", "filter2": "example2"})`
    - **This will only show you the first 20 documents**
      - enter the command `it` to iterate through the next 20 documents in the cursor
    - You can use `db` if you already navigated to the proper database with `use database_name`
    - You can append `.count()` to count the total number of documents
    - You can append `.pretty()` to make the documents more readable
  - If you use `.find()` without a query, you will see the first 20 documents of the collection, but they will not be organized at all.
- The environment in which you can do all of this is called the **mongo shell**
  - mongo shell is also a fully functioning javascript interpreter

## Chapter 3: Creating and Manipulating Documents

- Every MongoDB document has a unique `"_id"` key value pair. The value for `_"id"` is automatically generated: this value is called `ObjectId()`
  - You can use your own unique value instead if you want
  - Duplicate documents can be created as long as the `_id` fields are not the same
- If you use `mongoimport <uri> fileName.json`, an error will be thrown for trying to import duplicate data UNLESS you use the "--drop" option at the end. EX: `mongoimport <uri> --drop fileName.json`
- You can **insert documents** by using `db.collectionName.insert(<document>)`, or by using the MongoDB Atlas
- To **insert multiple documents** pass an array into the `.insert()` method, with each array item being a document. EX:
  - `db.collectionName.insert([<document1>, <document2>, <document3>])`
  - When inserting multiple documents, the inserting process will stop if a duplicate `_id` field is inserted. To preveint this use the `{"ordered": false}` option. EX:
    - `db.collection.insert([{"object1": true}, {"object2": true}], {"ordered": false})`

### Updating Documents

- Use `updateOne()` to **update one document**
- Use `updateMany()` to **update many documents**
  - Syntax is: `db.collectionName.updateOne({<whatToFind>}, {"$inc": {"key": "valueToIncrement"}})`
- `$inc` **increments** the field value by a specified amount.
- `$set` **updates** the value of the given field with the specified value. If the field doesn't exist, it creates a new one.
- `$push` **adds items** to an array, or creates a new array if the field doesn't exist.
- Putting it all together:
  - `db.zips.updateMany({ "city": "HUDSON" }, { "$inc": { "pop": 10 } })`

### Deleting Documents

- Use `deleteOne()` to **delete one document**
- Use `deleteMany()` to **delete many documents**
- Use `db.collectionName.drop()` to **delete a collection**
- You can also use the Atlas UI to delte documets, databases, or collections
  - Deleting all collections will also remove the database

## Chapter 4: Advanced CRUD Operations

## Chapter 5: Indexing and Aggregation Pipeline

## Chapter 6: Next Steps
