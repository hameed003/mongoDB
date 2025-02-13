Great question! MongoDB has a vast range of queries beyond the basic CRUD and aggregation operations. Here are **some additional advanced queries and operations** that I didn't cover earlier:

---

## **10. Additional Advanced MongoDB Queries**

### **1️⃣ Text Search (`$text`)**

MongoDB supports full-text search using indexes.

#### **Create a Text Index**

```javascript
db.products.createIndex({ name: "text", description: "text" });
```

#### **Search for a Keyword**

```javascript
db.products.find({ $text: { $search: "Product 10" } });
```

- Finds products that contain `"Product 10"` in `name` or `description`.

#### **Search for Multiple Words (Logical Operators)**

```javascript
db.products.find({ $text: { $search: "Product 10 -Samsung" } });
```

- Finds `"Product 10"` but **excludes** `"Samsung"`.

---

### **2️⃣ Regular Expressions (`$regex`)**

Used for pattern matching in string fields.

#### **Find Products with "Pro" in the Name**

```javascript
db.products.find({ name: { $regex: "Pro", $options: "i" } });
```

- `i` makes the search **case-insensitive**.

#### **Find Products Ending with "10"**

```javascript
db.products.find({ name: { $regex: "10$" } });
```

- `$` ensures it **ends** with `"10"`.

---

### **3️⃣ Find Documents Where a Field Exists (`$exists`)**

#### **Find Products That Have a `discount` Field**

```javascript
db.products.find({ discount: { $exists: true } });
```

- Returns only **products with a discount field**.

#### **Find Products That Don't Have a `discount` Field**

```javascript
db.products.find({ discount: { $exists: false } });
```

- Returns **products without a discount field**.

---

### **4️⃣ Conditional Queries (`$expr`)**

Used for **comparing** fields inside a document.

#### **Find Products Where `stock` is Greater Than `price / 10`**

```javascript
db.products.find({ $expr: { $gt: ["$stock", { $divide: ["$price", 10] }] } });
```

- **`$expr`** allows comparing **two fields** within the same document.

---

### **5️⃣ Working with Arrays (`$all`, `$size`, `$elemMatch`)**

#### **Find Products That Have "Red" and "Blue" Colors**

```javascript
db.products.find({ color: { $all: ["Red", "Blue"] } });
```

- Ensures the document contains **both** `"Red"` and `"Blue"`.

#### **Find Products That Have Exactly 2 Colors**

```javascript
db.products.find({ color: { $size: 2 } });
```

- Returns **products with exactly two colors**.

#### **Find Products That Have At Least One Color Matching a Condition**

```javascript
db.products.find({ color: { $elemMatch: { $in: ["Red", "Black"] } } });
```

- Finds products where **at least one color is "Red" or "Black"**.

---

### **6️⃣ Lookups (Joins in MongoDB)**

Used for **joining** two collections.

#### **Example: Join `products` with `orders` Collection**

```javascript
db.orders.aggregate([
  {
    $lookup: {
      from: "products", // Collection to join
      localField: "productId", // Field in orders
      foreignField: "_id", // Field in products
      as: "productDetails", // Alias for joined data
    },
  },
]);
```

- Joins the `products` collection with `orders` where `productId` matches `_id`.

---

### **7️⃣ `$where` - Use JavaScript in Queries**

#### **Find Products Where Stock is Double the Rating**

```javascript
db.products.find({ $where: "this.stock > this.rating * 2" });
```

- Uses **JavaScript** inside a query.

---

### **8️⃣ Find Most Expensive Product Per Category**

```javascript
db.products.aggregate([
  { $sort: { price: -1 } },
  { $group: { _id: "$category", mostExpensive: { $first: "$$ROOT" } } },
]);
```

- Groups products **by category** and finds the **most expensive product**.

---

### **9️⃣ Pagination (Skip & Limit with Sorting)**

Useful for implementing **"Load More" or Pagination**.

```javascript
db.products
  .find()
  .sort({ price: -1 })
  .skip(10) // Skip first 10 records
  .limit(5); // Get next 5 records
```

- Useful for **pagination systems**.

---

### **🔟 Using `$mod` for Mathematical Conditions**

#### **Find Products Where Price is Even**

```javascript
db.products.find({ price: { $mod: [2, 0] } });
```

- Finds **products with even prices**.

#### **Find Products Where Stock is Odd**

```javascript
db.products.find({ stock: { $mod: [2, 1] } });
```

- Finds **products with odd stock numbers**.

---

## **🎯 Conclusion**

✅ We covered **basic, intermediate, and advanced** MongoDB queries.  
✅ **Additional queries** include **full-text search, regex, conditional expressions, array queries, lookups, JavaScript queries, and pagination**.  
✅ MongoDB is **super flexible** and allows **complex operations without SQL-style joins**.

This should **fully** cover MongoDB queries from **basic to expert level**. Let me know if you need **more**! 🚀🔥

### Doubts: `db.products.find({}, { _id: 0, name: 1, price: 1 })` why we have used empty object `{ }` here. [View](https://github.com/hameed003/mongoDB/blob/main/doubts-and-solutions/solutions/Sol1.md)
