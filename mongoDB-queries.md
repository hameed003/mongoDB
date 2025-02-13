# MongoDB Queries: From Basic to Advanced

Let's go step by step to learn MongoDB queries from **basic to advanced** by creating a **large dataset** and demonstrating queries with proper explanations.

## 1️⃣ Setting Up MongoDB

Before running queries, ensure you have MongoDB installed and running. If you haven't installed it yet:

1. Download and install MongoDB from the **[MongoDB Download Center](https://www.mongodb.com/try/download/community)**.
2. Start MongoDB by running:
   ```bash
   mongod
   ```
3. Open MongoDB shell:
   ```bash
   mongosh
   ```

---

## 2️⃣ Creating a Large Dataset

We'll create a database named `EcommerceDB` and a collection named `products` with 1000+ sample documents.

### Insert Large Sample Data

Run this JavaScript snippet in the MongoDB shell (`mongosh`):

```javascript
use EcommerceDB  // Switch to EcommerceDB

const categories = ["Electronics", "Clothing", "Books", "Home Appliances", "Toys"];
const brands = ["Samsung", "Apple", "Nike", "Adidas", "Sony", "LG", "Dell", "HP", "Lenovo"];
const colors = ["Red", "Blue", "Green", "Black", "White", "Yellow", "Purple"];
const ratings = [1, 2, 3, 4, 5];

let bulkData = [];

for (let i = 1; i <= 1000; i++) {
    let product = {
        _id: i,
        name: `Product ${i}`,
        category: categories[Math.floor(Math.random() * categories.length)],
        brand: brands[Math.floor(Math.random() * brands.length)],
        price: Math.floor(Math.random() * 1000) + 100, // Price between 100-1100
        stock: Math.floor(Math.random() * 50) + 1, // Stock between 1-50
        rating: ratings[Math.floor(Math.random() * ratings.length)],
        color: colors[Math.floor(Math.random() * colors.length)],
        createdAt: new Date()
    };
    bulkData.push(product);
}

db.products.insertMany(bulkData); // Insert 1000+ products
```

Now, we have a large dataset (1000+ documents) in the `products` collection.

---

## 3️⃣ MongoDB Queries (Basic to Advanced)

### 1. Basic Queries

#### View All Documents

```javascript
db.products.find().pretty();
```

- Fetches all documents from the `products` collection.

- `.pretty()` formats output for better readability.

#### Find One Document

```javascript
db.products.findOne();
```

Fetches a single document from the collection.

#### Filter Data ($eq, $gt, $lt)

```javascript
db.products.find({ price: { $gt: 500 } }); // Find products with price > 500
db.products.find({ stock: { $lt: 10 } }); // Find products with stock < 10
db.products.find({ brand: "Apple" }); // Find products where brand is "Apple"
```

---

### 2. Projection (Select Specific Fields)

#### Exclude `_id` and Show `name` and `price`

```javascript
db.products.find({}, { _id: 0, name: 1, price: 1 });
```

- `_id: 0` means do not show `_id`.

- `name: 1, price: 1` means **show** `name` and `price`.

#### Include Only Specific Fields

```javascript
db.products.find({}, { name: 1, category: 1 });
```

- Shows only `name` and `category` fields.

---

### 3. Sorting and Limiting

#### Sort by Price (Ascending and Descending)

```javascript
db.products.find().sort({ price: 1 }); // Ascending Order
db.products.find().sort({ price: -1 }); // Descending Order
```

#### Limit and Skip Records

```javascript
db.products.find().limit(5); // Get only 5 records
db.products.find().skip(5).limit(5); // Skip first 5 and get next 5
```

---

### 4. Advanced Filtering

#### Find Products Between Two Prices ($gte, $lte)

```javascript
db.products.find({ price: { $gte: 300, $lte: 700 } });
```

- Finds products with **price between 300 and 700**.

#### Find Products in a List ($in)

```javascript
db.products.find({ category: { $in: ["Electronics", "Clothing"] } });
```

- Finds products **belonging to multiple categories**.

#### Find Products Not in a List ($nin)

```javascript
db.products.find({ brand: { $nin: ["Apple", "Samsung"] } });
```

- Excludes **Apple and Samsung products**.

---

### 5. Update Queries

#### Update a Single Product (`$set`)

```javascript
db.products.updateOne({ _id: 1 }, { $set: { price: 999 } });
```

Updates **price to 999** for the product with `_id: 1`.

#### Update Multiple Products (`$inc`)

```javascript
db.products.updateMany({ category: "Clothing" }, { $inc: { price: 50 } });
```

- **Increases price by 50** for all "Clothing" products.

#### Add a New Field (`$set`)

```javascript
db.products.updateMany({}, { $set: { discount: 10 } });
```

- Adds a **discount field** to all products.

---

### 6. Delete Queries

#### Delete One Product

```javascript
db.products.deleteOne({ _id: 10 });
```

- Deletes the product with `_id: 10`.

#### Delete All Products Below a Certain Price

```javascript
db.products.deleteMany({ price: { $lt: 200 } });
```

- Deletes **all products where price < 200**.

---

### 7. Aggregation (Advanced Queries)

#### Get Total Number of Products

```javascript
db.products.countDocuments();
```

#### Group by Category and Count Products

```javascript
db.products.aggregate([
  { $group: { _id: "$category", totalProducts: { $sum: 1 } } },
]);
```

- Groups products by **category** and counts how many exist in each.

#### Find Average Price of Each Category

```javascript
db.products.aggregate([
  { $group: { _id: "$category", avgPrice: { $avg: "$price" } } },
]);
```

Calculates **average price** of products in each category.

#### Find Top 5 Expensive Products

```javascript
db.products.aggregate([{ $sort: { price: -1 } }, { $limit: 5 }]);
```

- Finds the **top 5 most expensive products**.

---

### 8. Indexing (Performance Optimization)

#### Create an Index on `brand`

```javascript
db.products.createIndex({ brand: 1 });
```

**Speeds up queries** that filter by `brand`.

#### Check Indexes

```javascript
db.products.getIndexes();
```

---

### 9. Transactions (Atomic Operations)

MongoDB supports **transactions** to ensure multiple operations **succeed or fail together**.

```javascript
const session = db.getMongo().startSession();
session.startTransaction();

try {
  const products = session.getDatabase("EcommerceDB").products;
  products.updateOne({ _id: 1 }, { $inc: { stock: -1 } }); // Reduce stock
  products.updateOne({ _id: 2 }, { $inc: { stock: 1 } }); // Increase stock
  session.commitTransaction();
} catch (e) {
  session.abortTransaction(); // Rollback if anything fails
}
```

---

## 🎯 Conclusion

✅ We created a large dataset with **1000+ documents**.

✅ We explored **basic to advanced** MongoDB queries with explanations.

✅ We covered **CRUD operations, aggregation, indexing, transactions, and optimization**.

### More advance monngoDB queries: [View]()
