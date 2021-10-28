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

## Chapter 3: Admin Backend

## Chapter 4: Resiliency
