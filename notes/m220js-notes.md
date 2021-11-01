# M220JS: MongoDB for Javascript Developers

## Chapter 0: Introduction and Setup

- URI string Structure: `mongodb+srv://<userName>:<password>@<host>/<database>?foo`
  - `<host>` is the host name of the srv record for the MongoDB cluster that we want to connect to
    - `<host>` doesn't point to a database server. It only hosts the srv records
    - The `<database>` is the authentication database we want to connect to. The `<username>` and `<passwords>` are needed to connect to that database
  - If we want to specify additional options specify this with a question mark at the end of the URI string
- Reason to use an SRV record
  - srv records (Service records) define it's own DNS with a list of additonal hostnames (for the clusters) that you want to resolve to
  - This is great because we don't need to know where each cluster is located, even if clusters are relocated and/or changed. The SRV record takes care of that for us
- **DAO**: Data Access Objects

## Chapter 1: Driver Setup

### Mongo Client

- You use the `MongoClient` object to connect your app to the MongoDB Atlas
  - The connection is called a **database handle**
  - there is also a **collection handle**, which is the connection to the correct collection

### Asynchronous Programming

- **Callbacks** - functions passed in other functions. You can use this if you don't like promises. Example:
  `movies.findOne({ title: "Once Upon a Time in Mexico" }, function(err, doc) {})`
- **Promises** - `.then()` and `.catch()` Example:
  `movies.findOne({ title: "Once Upon a Time in Mexico" }).then().catch()`
- **Async/Await** - Use `try{}catch{}` inside of async await funcitons Example:

```
async () => {
  try {
    let { title, cast } = await movies.findOne({ title: "Once Upon a Time in Mexico" })
  } catch (e) { expect(e).toBeNull() }
}
```

- The **$all** operator will only return documents where all values specified in the query are present in the array for that field. Example:

```
let result = await movies.find({
      cast: { $all: ["Salma Hayek", "Johnny Depp"] },
    })
```

- **$in** selects documents where the value is equal to any item in an array. Example:
  `movies.find( { countries: { $in: countries } }, { projection: { title: 1 } } )`

- `.next()` iterates ot the next result in the cursor

## Chapter 2: User-Facing Backend

- The cursor methods `.limit()`, `.sort()`, and `.skip()` can also be used in an aggregation pipeline. Example:

```
movies.find({ directors: "Sam Raimi" }, { _id: 0, year: 1, title: 1, cast: 1 })
      .sort([["year", 1]])
      .skip(5)

const skippedPipeline = [{ $match: { directors: "Sam Raimi" } },
      { $project: { _id: 0, year: 1, title: 1, cast: 1 } },
      { $sort: { year: 1 } },
      { $skip: 5 },
    ]
const skippedAggregation = await movies.aggregate(skippedPipeline)
```

- `"$group"` will let you manipulate data (`$sum`, `$avg`, etc.)
- **You can use MongoDB Compass or MongoDB Atlas to create pipelines** (which can be exported to Javascript code)

### Basic Writes

- `insertOne()`
- `insertMany()`
- `updateOne()`
- `updateMany()`
  - `upsert()` Example:

```
await sessions.updateOne({ user_id: email }, { $set: { jwt: jwt } }, { upsert: true } )
```

- When a database receives a write request it only updates one of the nodes in the replica set before sending a confirmation that the write was sucessfuly performed. By using `writeConern: {w: 2}` you can specify w to state how many of the nodes need to be updated before sending the confirmation
  - You can use `writeConern: {w: majority}` as well. Takes longer, but is more secure. So do it for more important things (Like creating an account)
  - 0 will fire automatically (Fire-and-forget). If you put a number that is higher than the number of nodes, you will get an error.

### Joins

- Join merges data from two seperate collections
- `$lookup` helps you join collections.
  - `from`: What collection you want to join to your current collection
  - `let` - gives you access to you base collections data within `$lookup`. `$lookup` has access to the other collections fields (whatever is specified in `from`), but doesn't have access to your base collection (**unless you use `let`**). This value will be a two dollar sign in the `pipeline`
  - `pipeline` - When you refer to your base collections variables **use two dollar signs** instead of just one (The other collection only uses one). **The double dollar sign is defined in the `let` statement**
  - `as`

### Delete

- There's `deleteOne()` and `deleteMany()`
- If multiple documents match, then `deleteOne()` will delete whatever is next in the $natural order

## Chapter 3: Admin Backend

## Chapter 4: Resiliency
