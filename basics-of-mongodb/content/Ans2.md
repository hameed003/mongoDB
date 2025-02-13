In MongoDB, the **`$`** symbol is used to represent **query operators**. These operators perform comparisons, logical operations, or array operations on documents.

Here’s what some of the most commonly used **`$` operators** mean:

---

## **1️⃣ Comparison Operators**

These operators help filter data based on certain conditions.

| **Operator** | **Description**          | **Example Query**          | **Explanation**                     |
| ------------ | ------------------------ | -------------------------- | ----------------------------------- |
| **`$eq`**    | Equal to                 | `{ price: { $eq: 500 } }`  | Finds products where `price == 500` |
| **`$ne`**    | Not equal to             | `{ price: { $ne: 500 } }`  | Finds products where `price != 500` |
| **`$gt`**    | Greater than             | `{ price: { $gt: 500 } }`  | Finds products where `price > 500`  |
| **`$gte`**   | Greater than or equal to | `{ price: { $gte: 500 } }` | Finds products where `price >= 500` |
| **`$lt`**    | Less than                | `{ price: { $lt: 500 } }`  | Finds products where `price < 500`  |
| **`$lte`**   | Less than or equal to    | `{ price: { $lte: 500 } }` | Finds products where `price <= 500` |

#### **Example: Find products priced between 300 and 800**

```javascript
db.products.find({ price: { $gte: 300, $lte: 800 } });
```

---

## **2️⃣ Logical Operators**

These operators help perform complex conditions using **AND, OR, NOT** logic.

| **Operator** | **Description** | **Example Query**                                             | **Explanation**                                          |
| ------------ | --------------- | ------------------------------------------------------------- | -------------------------------------------------------- |
| **`$and`**   | Logical AND     | `{ $and: [{ price: { $gt: 300 } }, { stock: { $lt: 50 } }] }` | Finds products where `price > 300` **AND** `stock < 50`  |
| **`$or`**    | Logical OR      | `{ $or: [{ brand: "Apple" }, { brand: "Samsung" }] }`         | Finds products where brand is **Apple OR Samsung**       |
| **`$not`**   | Logical NOT     | `{ price: { $not: { $gt: 500 } } }`                           | Finds products where `price` **is NOT** greater than 500 |
| **`$nor`**   | Not OR          | `{ $nor: [{ brand: "Apple" }, { brand: "Samsung" }] }`        | Excludes **Apple & Samsung** products                    |

#### **Example: Find products that are either Electronics OR Clothing**

```javascript
db.products.find({
  $or: [{ category: "Electronics" }, { category: "Clothing" }],
});
```

---

## **3️⃣ Array Operators**

Used to filter **documents with arrays**.

| **Operator**     | **Description**                          | **Example Query**                                          | **Explanation**                                                                 |
| ---------------- | ---------------------------------------- | ---------------------------------------------------------- | ------------------------------------------------------------------------------- |
| **`$in`**        | Matches any value in a list              | `{ category: { $in: ["Electronics", "Books"] } }`          | Finds products that are **Electronics OR Books**                                |
| **`$nin`**       | Not in list                              | `{ brand: { $nin: ["Apple", "Samsung"] } }`                | Excludes **Apple & Samsung** products                                           |
| **`$all`**       | Matches all values in an array           | `{ colors: { $all: ["Red", "Blue"] } }`                    | Finds products with **both** Red & Blue                                         |
| **`$size`**      | Filters by array length                  | `{ colors: { $size: 2 } }`                                 | Finds products that have exactly **2 colors**                                   |
| **`$elemMatch`** | Matches at least one element in an array | `{ reviews: { $elemMatch: { rating: 5, user: "John" } } }` | Finds products where at least **one review** has `rating: 5` and `user: "John"` |

#### **Example: Find products with colors "Red" OR "Blue"**

```javascript
db.products.find({ color: { $in: ["Red", "Blue"] } });
```

---

## **4️⃣ Projection Operators**

Used to **select specific fields**.

| **Operator** | **Description**                 | **Example Query**                   | **Explanation**                                        |
| ------------ | ------------------------------- | ----------------------------------- | ------------------------------------------------------ |
| **`$slice`** | Returns specific array elements | `{ reviews: { $slice: 3 } }`        | Returns only the first **3 reviews** from each product |
| **`$meta`**  | Retrieves metadata              | `{ score: { $meta: "textScore" } }` | Returns text search relevance score                    |

#### **Example: Select `name` and `price`, but exclude `_id`**

```javascript
db.products.find({}, { _id: 0, name: 1, price: 1 });
```

---

## **5️⃣ Aggregation Operators**

Used in **complex queries**, such as grouping and filtering.

| **Operator** | **Description**   | **Example Query**                                                  | **Explanation**                                               |
| ------------ | ----------------- | ------------------------------------------------------------------ | ------------------------------------------------------------- |
| **`$group`** | Groups documents  | `{ $group: { _id: "$category", avgPrice: { $avg: "$price" } } }`   | Groups products by `category` and finds the **average price** |
| **`$sum`**   | Sums values       | `{ $group: { _id: "$category", totalStock: { $sum: "$stock" } } }` | Calculates **total stock per category**                       |
| **`$avg`**   | Finds the average | `{ $group: { _id: "$brand", avgRating: { $avg: "$rating" } } }`    | Calculates **average rating per brand**                       |
| **`$sort`**  | Sorts documents   | `{ $sort: { price: -1 } }`                                         | Sorts products **by price in descending order**               |
| **`$match`** | Filters documents | `{ $match: { category: "Electronics" } }`                          | Filters to include only **Electronics**                       |

#### **Example: Find the top 3 most expensive products**

```javascript
db.products.aggregate([{ $sort: { price: -1 } }, { $limit: 3 }]);
```

---

## **6️⃣ Date Queries (`$date`)**

Used to filter data by timestamps.

| **Operator** | **Description**   | **Example Query**                                 | **Explanation**                               |
| ------------ | ----------------- | ------------------------------------------------- | --------------------------------------------- |
| **`$gte`**   | Greater than date | `{ createdAt: { $gte: new Date("2024-01-01") } }` | Finds products **created after Jan 1, 2024**  |
| **`$lt`**    | Less than date    | `{ createdAt: { $lt: new Date("2023-12-01") } }`  | Finds products **created before Dec 1, 2023** |

#### **Example: Find products created in the last 7 days**

```javascript
db.products.find({
  createdAt: { $gte: new Date(new Date().setDate(new Date().getDate() - 7)) },
});
```

---

## **🎯 Summary**

- **`$eq, $gt, $lt`** are **comparison operators** used for filtering data.
- MongoDB uses **`$` operators** for **comparisons (`$eq`, `$gt`), logic (`$or`, `$and`), arrays (`$in`, `$all`), projections (`$slice`), aggregation (`$group`, `$sum`), and dates (`$date`)**.
- **Aggregation** helps in **grouping, filtering, and calculating** data.
- **Regular expressions (`$regex`) and full-text search (`$text`)** allow for **advanced searching**.

MongoDB provides a **flexible way** to query data using these operators. Let me know if you need **more details or real-world examples! 🚀🔥**
