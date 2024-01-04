# MongoDB

## Installation

### Docker Installation

```bash
docker run -d -p 27017:27017 --name mongodb mongo
```

To run MongoDB in a Docker container with specific initialization parameters, use the following Docker command:

```bash
docker run -d -p 27017:27017 --name mongodb \
  -e MONGO_INITDB_ROOT_USERNAME=admin \
  -e MONGO_INITDB_ROOT_PASSWORD=<password> \
  -e MONGO_INITDB_DATABASE=<database> \
  mongo
```

### Manual Installation

For manual installation, please refer to the [official documentation](https://www.mongodb.com/docs/manual/installation/).

## Getting Started

### Connect to MongoDB

To connect to MongoDB, use the MongoDB shell or various drivers for different programming languages. Here's a basic connection string example:

```sh
mongodb://admin:<password>@localhost:27017/<database>?authSource=admin&ssl=true
```

- `admin`: The database that contains the user authentication information.
- `localhost`: The IP address of the MongoDB server.
- `27017`: The port number of the MongoDB server.
- `<database>`: The name of the database to connect to.

Connection options:

- `authSource=admin`: Specifies the authentication database.
- `ssl=true`: Enables SSL encryption for the connection.

For a full list of options, please refer to the [MongoDB documentation](https://www.mongodb.com/docs/manual/reference/connection-string/).

# Basic Operations

## Create Collection

Collections in MongoDB are created implicitly when you insert documents. However, you can explicitly create a collection using the createCollection method.

```mongosh
db.createCollection("mycollection")
```

## Insert Documents

Use the `insertOne` or `insertMany` method to insert documents into a collection.

```mongosh
db.mycollection.insertOne({ name: "John Doe", age: 30 })
```

## Query Documents

MongoDB provides powerful querying capabilities. For example, to find all documents in a collection:

```mongosh
db.mycollection.find()
```
