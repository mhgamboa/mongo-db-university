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

## Chapter 4: Advanced CRUD Operations

## Chapter 5: Indexing and Aggregation Pipeline

## Chapter 6: Next Steps
