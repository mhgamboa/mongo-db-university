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

## Chapter 2: User-Facing Backend

## Chapter 3: Admin Backend

## Chapter 4: Resiliency
