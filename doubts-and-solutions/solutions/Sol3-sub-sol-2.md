If you want to enforce a **schema-like structure** while using **MongoClient** (without Mongoose), you have two options:

1️⃣ **Manually Validate Data Before Inserting**  
2️⃣ **Use MongoDB Schema Validation (JSON Schema)**

---

## **1️⃣ Manually Validate Data Before Inserting**

Since MongoClient doesn't enforce a schema, **you have to manually check the data** before inserting it.

### **Example: Manual Validation in Node.js**

```javascript
const { MongoClient } = require("mongodb");

// Define a manual schema
const productSchema = {
  name: "string",
  price: "number",
};

// Function to validate data
function validateData(data, schema) {
  return Object.keys(schema).every((key) => typeof data[key] === schema[key]);
}

async function run() {
  const client = new MongoClient("mongodb://localhost:27017");
  await client.connect();

  const database = client.db("myDatabase");
  const products = database.collection("products");

  const newProduct = { name: "Laptop", price: 1000 }; // Valid data

  if (validateData(newProduct, productSchema)) {
    await products.insertOne(newProduct);
    console.log("Product inserted!");
  } else {
    console.log("Invalid data! Schema mismatch.");
  }

  client.close();
}

run().catch(console.error);
```

👉 **What happens here?**

- We defined a **manual schema** (`productSchema`).
- Before inserting, we check if the **data matches the schema** using `validateData()`.
- If it **does not match**, we reject the data.

🚀 **Advantage:** Works without Mongoose.  
🛑 **Disadvantage:** You must manually validate every insert/update operation.

---

## **2️⃣ MongoDB Built-in Schema Validation (JSON Schema)**

MongoDB allows you to **define schema rules directly in the database** using **JSON Schema validation**. This approach is closer to how Mongoose works.

### **Example: Enforcing Schema in MongoDB**

```javascript
const { MongoClient } = require("mongodb");

async function run() {
  const client = new MongoClient("mongodb://localhost:27017");
  await client.connect();

  const database = client.db("myDatabase");

  // Create a collection with schema validation
  await database.createCollection("products", {
    validator: {
      $jsonSchema: {
        bsonType: "object",
        required: ["name", "price"],
        properties: {
          name: { bsonType: "string", description: "Must be a string" },
          price: { bsonType: "number", description: "Must be a number" },
        },
      },
    },
  });

  console.log("Collection created with schema validation!");

  const products = database.collection("products");

  try {
    await products.insertOne({ name: "Laptop", price: 1000 }); // ✅ Valid
    console.log("Product inserted!");

    await products.insertOne({ title: "Book", author: "John Doe" }); // ❌ Invalid (schema mismatch)
  } catch (error) {
    console.error("Insertion failed:", error.message);
  }

  client.close();
}

run().catch(console.error);
```

👉 **What happens here?**

- We **define schema validation rules** (`bsonType`, `required`, `properties`).
- If someone tries to insert an **invalid document**, MongoDB **rejects it automatically**!

🚀 **Advantage:**  
✅ **Schema enforcement like Mongoose**  
✅ **Works directly at the database level** (doesn't depend on application code)

🛑 **Disadvantage:**

- Once the collection is created, **schema changes are harder** (you may need to modify validation rules).

---

## **🔍 Comparison Table**

| Feature                         | Manual Validation        | JSON Schema (MongoDB)             | Mongoose                                       |
| ------------------------------- | ------------------------ | --------------------------------- | ---------------------------------------------- |
| **Schema Enforced?**            | ❌ No (only in code)     | ✅ Yes                            | ✅ Yes                                         |
| **Validation Before Insert?**   | ✅ Yes (manual check)    | ✅ Yes (automatic)                | ✅ Yes                                         |
| **Requires Mongoose?**          | ❌ No                    | ❌ No                             | ✅ Yes                                         |
| **Database-Level Enforcement?** | ❌ No                    | ✅ Yes                            | ❌ No                                          |
| **Best For?**                   | Small apps, full control | Large apps, direct DB enforcement | Apps needing extra features (hooks, relations) |

---

## **📌 Summary**

1. **If you want to enforce a schema without Mongoose**, use **MongoDB JSON Schema validation** (`$jsonSchema`).
2. **If you need flexibility but still want structure**, use **manual validation in Node.js** before inserting data.
3. **If you need full ORM/ODM features**, use **Mongoose**.

🚀 Let me know if you need more clarification!

### Doubt: Since we are connecting to an existing database i.e `myDatabase` using mongoClient, but what if there is no database present already. [Sol3-sub-sol-3]
