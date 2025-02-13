To **connect to your MongoDB Atlas** from the command line (CMD), follow these steps:

---

## **Step 1: Get Your MongoDB Atlas Connection String**

1️⃣ Log in to your [MongoDB Atlas](https://www.mongodb.com/atlas).  
2️⃣ Go to **Database > Deployments** and find your cluster.  
3️⃣ Click on **"Connect"** > **"Connect using MongoDB Shell"**.  
4️⃣ Copy the connection string, which looks like this:

```shell
mongosh "mongodb+srv://your-cluster.mongodb.net/myDatabase" --username yourUsername
```

---

## **Step 2: Install MongoDB Shell (`mongosh`)**

If you haven't installed **MongoDB Shell**, download it from:  
👉 [MongoDB Shell Download](https://www.mongodb.com/try/download/shell)

---

## **Step 3: Connect to MongoDB Atlas from CMD**

1️⃣ Open **Command Prompt (CMD)** or **Terminal**.  
2️⃣ Paste and run your connection string:

```shell
mongosh "mongodb+srv://your-cluster.mongodb.net/myDatabase" --username yourUsername
```

3️⃣ Enter your **password** when prompted.

✅ **Now you're connected to MongoDB Atlas from CMD!**

---

## **Step 4: Run MongoDB Commands**

Once connected, you can run MongoDB queries just like a local database:

```shell
show dbs      # List all databases
use myDatabase  # Switch to your database
show collections  # List collections
db.users.find()  # Fetch all documents from "users" collection
```

---

## **📌 Alternative: Connect Using MongoClient (Node.js)**

If you're using **Node.js**, you can connect using `MongoClient`:

```javascript
const { MongoClient } = require("mongodb");

const uri =
  "mongodb+srv://yourUsername:yourPassword@your-cluster.mongodb.net/myDatabase";
const client = new MongoClient(uri);

async function connectDB() {
  try {
    await client.connect();
    console.log("Connected to MongoDB Atlas!");
  } catch (error) {
    console.error("Connection failed:", error);
  }
}

connectDB();
```

---

## **🚀 Summary**

- ✅ Get **MongoDB Atlas connection string** from the Atlas dashboard.
- ✅ Install **MongoDB Shell (`mongosh`)** if not installed.
- ✅ Use CMD and run the connection string.
- ✅ Execute MongoDB commands in CMD.

Let me know if you need more help! 🚀
