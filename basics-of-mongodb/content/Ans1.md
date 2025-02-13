### **What is MongoDB?**

MongoDB is a **NoSQL**, document-oriented database designed for scalability, flexibility, and ease of development. Unlike traditional relational databases (SQL), MongoDB stores data in **BSON** (Binary JSON) format, allowing nested structures like arrays and objects. It is schema-less, meaning documents in the same collection can have different fields.

**Key Features**:

- **Horizontal Scalability**: Sharding distributes data across clusters.
- **High Availability**: Replica sets ensure data redundancy and failover.
- **Rich Query Language**: Supports CRUD operations, aggregation, text search, and geospatial queries.
- **Flexible Schema Design**: Documents can evolve independently.

---

### **Topics to Master MongoDB (with Examples)**

#### **1. NoSQL Basics**

- **Differences from SQL**: No fixed schema, no joins, horizontal scaling.
- **Document Model**: Data stored as JSON/BSON documents.
  ```json
  {
    "_id": ObjectId("507f191e810c19729de860ea"),
    "name": "John Doe",
    "age": 30,
    "address": {
      "street": "123 Main St",
      "city": "New York"
    }
  }
  ```

---

#### **2. MongoDB CRUD Operations**

- **Create**: `insertOne()`, `insertMany()`
  ```js
  db.users.insertOne({ name: "Alice", age: 25 });
  ```
- **Read**: `find()`, `findOne()`
  ```js
  db.users.find({ age: { $gt: 20 } });
  ```
- **Update**: `updateOne()`, `updateMany()`
  ```js
  db.users.updateOne({ name: "Alice" }, { $set: { age: 26 } });
  ```
- **Delete**: `deleteOne()`, `deleteMany()`
  ```js
  db.users.deleteMany({ age: { $lt: 18 } });
  ```

---

#### **3. Schema Design**

- **Embedding vs. Referencing**:

  - **Embedding**: Store related data in a single document (e.g., user with addresses).
    ```json
    {
      "_id": 1,
      "name": "Bob",
      "orders": [
        { "product": "Laptop", "price": 999 },
        { "product": "Mouse", "price": 20 }
      ]
    }
    ```
  - **Referencing**: Link documents using `ObjectId` (e.g., blog posts and comments).

    ```json
    // posts collection
    { "_id": 100, "title": "MongoDB Guide" }

    // comments collection
    { "post_id": 100, "text": "Great post!" }
    ```

---

#### **4. Indexing**

- **Purpose**: Speed up queries by creating indexes on frequently queried fields.
- **Types**:
  - **Single Field**: `db.users.createIndex({ age: 1 })`
  - **Compound Index**: `db.users.createIndex({ name: 1, age: -1 })`
  - **Text Index**: `db.articles.createIndex({ content: "text" })`
  - **TTL Index**: Auto-deletes documents after a time (e.g., logs).
    ```js
    db.logs.createIndex({ createdAt: 1 }, { expireAfterSeconds: 3600 });
    ```

---

#### **5. Aggregation Framework**

- **Pipeline Stages**: `$match`, `$group`, `$sort`, `$project`.
- **Example**: Calculate total sales by category.
  ```js
  db.sales.aggregate([
    { $match: { year: 2023 } },
    { $group: { _id: "$category", total: { $sum: "$amount" } } },
    { $sort: { total: -1 } },
  ]);
  ```

---

#### **6. Replication & High Availability**

- **Replica Set**: A primary node and multiple secondaries.
- **Failover**: Automatic election of a new primary if the current one fails.
  ```js
  // Initiate a replica set
  rs.initiate({
    _id: "rs0",
    members: [
      { _id: 0, host: "mongo1:27017" },
      { _id: 1, host: "mongo2:27017" },
    ],
  });
  ```

---

#### **7. Sharding (Horizontal Scaling)**

- **Shard Key**: Distributes data across shards (e.g., `user_id`).
  ```js
  sh.enableSharding("mydb");
  sh.shardCollection("mydb.users", { user_id: 1 });
  ```

---

#### **8. Security**

- **Authentication**: Enable with `--auth` flag.
  ```js
  use admin;
  db.createUser({ user: "admin", pwd: "secret", roles: ["root"] });
  ```
- **Authorization**: Role-based access control (e.g., `readWrite` role).

---

#### **9. Transactions**

- **ACID Compliance**: Multi-document transactions.
  ```js
  const session = db.getMongo().startSession();
  session.startTransaction();
  try {
    db.orders.insertOne({ item: "book", price: 20 }, { session });
    db.inventory.updateOne(
      { item: "book" },
      { $inc: { stock: -1 } },
      { session }
    );
    session.commitTransaction();
  } catch (error) {
    session.abortTransaction();
  }
  ```

---

#### **10. Tools & Ecosystem**

- **MongoDB Compass**: GUI for querying and analyzing data.
- **MongoDB Atlas**: Managed cloud database service.
- **Mongodump/Mongorestore**: Backup and restore utilities.

---

### **Advanced Topics**

1. **Change Streams**: Real-time data change notifications.
2. **Geospatial Queries**: Find locations within a radius.
3. **Performance Tuning**: Use `explain()` to analyze query plans.
4. **Schema Validation**: Enforce document structure rules.
   ```js
   db.createCollection("users", {
     validator: {
       $jsonSchema: {
         required: ["name", "email"],
         properties: { name: { type: "string" }, email: { type: "string" } },
       },
     },
   });
   ```

---

### **Learning Resources**

- **Official MongoDB University**: Free courses on MongoDB basics and advanced topics.
- **Documentation**: [MongoDB Docs](https://docs.mongodb.com/)
- **Books**: _MongoDB: The Definitive Guide_ by Shannon Bradshaw.

By mastering these topics, you’ll be equipped to design, optimize, and manage MongoDB databases for high-performance applications.
