In MongoDB, **you don't need to create a database manually** before using it. If the database **does not exist**, MongoDB **automatically creates it** when you insert data into a collection for the first time.

---

### **Example: Connecting to a Non-Existing Database**

```javascript
const { MongoClient } = require("mongodb");

async function run() {
  const client = new MongoClient("mongodb://localhost:27017");
  await client.connect();

  // Connecting to a database that doesn't exist yet
  const database = client.db("newDatabase"); // No need to create manually

  // Creating a collection and inserting data
  const users = database.collection("users");
  await users.insertOne({ name: "John Doe", age: 25 });

  console.log("User inserted into newDatabase!");

  client.close();
}

run().catch(console.error);
```

👉 **What happens here?**

- We connected to **`newDatabase`**, which **did not exist before**.
- When we inserted data into the **`users` collection**, MongoDB **automatically created `newDatabase`** and stored the data inside it.

---

## **🔹 When is the Database Actually Created?**

MongoDB **does not create an empty database immediately** when you connect. It is **created only when you insert data into a collection**.

### **Checking If a Database Exists**

You can check existing databases using this command in the MongoDB shell:

```shell
show dbs
```

🚀 **Newly created databases won't appear** in `show dbs` **until they have at least one document.**

---

## **📌 Summary**

✅ **No need to manually create a database**—MongoDB creates it when data is inserted.  
✅ **If a database already exists, MongoDB connects to it**.  
✅ **An empty database won't appear in `show dbs` until it has data**.

Let me know if you need more clarification!

### Doubt: `run().catch(console.error)` why you are using catch() here, and can we use catch() without try {} block?
