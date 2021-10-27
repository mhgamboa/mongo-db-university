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

### Update Operators

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

- **LOGIC OPERATORS GO 1ST, QUERY OPERATORS GO 2ND.** But, **`$expr` MAKES QUERY OPERATORS GO FIRST AS WELL**

### Query Operators

- `$eq`: **equal to**
- `$ne`: **not equal to**
- `$gt`: **greater than**
- `$lt`: **less than**
- `$gte`: **greater than or equal to**
- `$lte`: **less than or equal to**
- EXAMPLE: `db.trips.find({ "tripduration": { "$lte" : 70 }, "usertype": { "$ne": "Subscriber" } }).pretty()`

### Logic Operators

- `$or`: **match one** query clause
- `$nor`: anything doat **doesn't match both** query clauses (kind of like a combination of `$not` and `$or`)
  - These three share common syntax by using an array. Example: `{"$logic":[{filter1}, {filter2}] }`
- `$and`: **match all** query clauses

  - A lot of times `$and` is used implicitly without you needing to use it. Example:
    - `{"$and": [{"student_id": {"$gt": 25} }, {"student_id": {"$lt": 100}} ]}` versus
    - `{"student_id": {"$gt": 25, "$lt": 100}}`
    - Generally speaking you only need to use `$and` when you use the same operator more than once

- `$not` - **negates** the query requriement. Example:
  - `$not` syntax example: `{"student_id": {"$not": {"$gt": 25}} }`

### $expr

- `$expr` allows the use of aggregation expressions (More on that later)
- `$expr` **allows for the use of variables**. You just have to use a `$` to denote what the variable is. Example:
  - `db.trips.find({ "$expr": { "$eq": [ "$end station id", "$start station id"] }}).count()`

### Array Operators

- `$all` helps you search withing arrays, as it does not care where the items are found in the array, as long as it exists.
  - **Without `$all` MongoDB will look for an exact array match**
- `$size` disgnates the exact number of array items that must exists to match
- Example:

```
db.listingsAndReviews.find({
  "amenities": {
    "$size": 20,
    "$all": [ "Internet", "Wifi",  "Kitchen"]
  }
}).pretty()
```

### Projections

- **Projections** tell your search query which parts of the cursor should be shown. This is the second argument of the `.find()` method and is written as a JSON object.
- The field is what you want displayed, and the value can be 0 or 1. 1 says include only these values. 0 says exclude these values, and show everything else.
  - **If you use $elemMatch, you don't use 0s or 1s.**
- You can use both 1 and 0 if the 0 is for `_id`
- `$elemMatch` can be used with either the queries or projections for .find().
  - For queries $elemMatch is similar to .find() but for finding data that is within a subdocument within an array EX:

```
db.grades.find({ "scores": { "$elemMatch": { "type": "extra credit" } }
}) //Find all documents where the student had an extra credit score:
```

### Array operators and Sub Documents

- To search through subdocuments use dot notation. This is also applicable to arrays. EX:
  - `db.<collection>.find("desired field.desired sub field": "desired sub value" )` //All spaces are included
  - `db.<collection>.find("desired field.0.desired sub field": "desired sub value" )` //The 0 is for the first array item
- $regex allows you to match parts of a string. EX:
  - `{ <field>: { $regex: /pattern/, $options: '<options>' } }`
  - `{ <field>: { $regex: 'pattern', $options: '<options>' } }`
  - `{ <field>: { $regex: /pattern/<options> } }`

## Chapter 5: Indexing and Aggregation Pipeline

- Essentially, the Aggregation Framework is just another way to query MongoDB databases, but it lets you do more than just find data.
- To use the aggregation framework use `.aggregate()` instead of `.find()`
  - The biggest difference between `.aggregate()` and `.find()` is that `.aggregate()` uses an array as the base instead of an object.
  - **The order matters in `.aggregate()`!** Data is filtered one query at a time. Here is an `.aggregate()` example:

```
db.listingsAndReviews.aggregate([
  { "$match": { "amenities": "Wifi" } }, // Filters this first
  { "$project": { "price": 1,"address": 1, "_id": 0 }} // Filters this second
]).pretty()
```

- **`$match`** filters documents so that only documents that meet the specified criteria will be passed through
- **`$project`** gets rid of all of the fields not requested

- **`$group`** is an operator that takes an incoming stream of data and siphons it into multiple distinct reserviors.
- **The first part of `$group`** creates a new cursor based on the results you want.
- `$group` syntax is `{"$group": {newFieldName: "$oldFieldNameYouWantToGroup"}}`. EXAMPLE:

```
db.listingsAndReviews.aggregate([{ "$group": { "_id": "$address.country" }}])

// Returns:
// { "_id" : "Australia" }
// { "_id" : "Brazil" }
// { "_id" : "Hong Kong" }
// etc.
```

- **The second part of `$group`** allows you to use quantitative analysis. EXAMPLE:

```
db.listingsAndReviews.aggregate([
  {"$project": { "address": 1, "_id": 0 }},
  {"$group": {
    "_id": "$address.country",
    "count": { "$sum": 1 }
  }}
])
```

- This example assigns the sum to "count" in the new cursor
- Usually you will use a value from the object instead of 1, but for this example we are just counting

### Sort & Limit

- `sort()` and `.limit()` are cursor methods just likehow .pretty() and .count() are.
  - `.limit()` limits the amount of documents shown in the cursor.
  - `.sort()` will sort the documents as specified
    - you can even sort with 2 parameters, but remember that order is important.
  - **When used together use MongoDB will always apply `.sort()` before `.limit()`, Regardless of the order you put them in.** That being said, you should still use `.sort()` before .limit() as a good practice (since order is important) EXAMPLE:

```
db.zips.find().sort({ "pop": -1 }).limit(10)
```

- This example sorts by population decending (positive 1 would be ascending) and only shows 10 documents

### Indexes

- **Indexes make your searches faster** (Like how indexes in the back of a book make your searches faster)
- **To create an index** use the `.createIndex` cursor method. Example:

```
db.trips.createIndex({ "birth year": 1 })
db.trips.createIndex({ "start station id": 1, "birth year": 1 })
```

- "1" mean in ascending order

### Data Modeling

- TL;DR Data should be stored in the way that it is queried
- As your application evolves, so should your data model

### Update and Upsert

- `.Upsert()` is a combination of update and insert.
- It will update or insert after searching the database
- It is part of the `.updateOne()` cursor method. EXAMPLE:
