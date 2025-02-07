# MongoDB Cheat Sheet

[ Reference - Thapa Technical and tech gun ]

Write **`monogosh`** on CMD Terminal.
( To Start MongoDB server )

**`Ctrl + D or .exit or Ctrl + C ( two times )`**
( To come out of mongosh shell. )

**`Right click`**
( To paste data on mongosh shell. )

**`Ctrl + C`**
( To copy data from mongosh shell. )

## CREATE / SWITCH / USE DATABASE:

**`use databaseName;`**
( it will create a database and switch to it, if there is no database present of that name. )

**NOTE:**
**`use databaseName;`**
( this is used to create a database as well as to switch to an existing database. )

**`db.createCollection(collectionName);`**
( To create a collection )
Eg: **`db.createCollection('posts');`**

( To Create a collection with Validation, in the below database 'teacher' is a collection. )

```
db.createCollection("teacher", {
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["name", "age"],
      properties: {
        name: {
          bsonType: "string",
          description: "Must be a string and is required"
        },
        age: {
          bsonType: "int",
          description: "Must be an integer and is required"
        }
      }
    }
  }
});
```

## INSERT INTO DATABASE:

( To insert one row / document )

Method 1:

```
db.collectionName.insert(
{field1: value1,
 field2: value2,
 ....
});

Example:
db.posts.insert({
  title: 'Post One',
  body: 'Body of post one',
  category: 'News',
  tags: ['news', 'events'],
  user: {
    name: 'John Doe',
    status: 'author'
  },
  date: Date()
});
```

Method 2:

```
db.collectionName.insertOne(
{field1: value1,
 field2: value2,
 ....
});

Example:
db.posts.insertOne({
  title: 'Post One',
  body: 'Body of post one',
  category: 'News',
  tags: ['news', 'events'],
  user: {
    name: 'John Doe',
    status: 'author'
  },
  date: Date()
});
```

( To insert multiple rows / documents. )

```
db.collectionName.insertMany([
{field1: value1,field2: value2,...},
{field2: value2,field2: value2,...}
//...
]);

Example:
db.posts.insertMany([
  {
    title: 'Post Two',
    body: 'Body of post two',
    category: 'Technology',
    date: Date()
  },
  {
    title: 'Post Three',
    body: 'Body of post three',
    category: 'News',
    date: Date()
  },
  {
    title: 'Post Four',
    body: 'Body of post three',
    category: 'Entertainment',
    date: Date()
  }
]);
```

## READ / RETRIEVE / SHOW ALL DATABASE:

**`show dbs;`** OR
**`show databases;`**
( To show databases. )

**NOTE:**
You won't see a database listed in the output of the **`show dbs`** or **`show databases`** command until that
database contains at least one 'collection' with data in it.

**`db;`**
( To show current database. )

**`show collections;`**
( To show collections )

**`db.collectionName.find();`** OR
**`db.collectionName.find().pretty();`**
( To retrieve all the documents data from a collection. )
Eg: **`db.posts.find();`** OR
**`db.posts.find().pretty();`**

**`db.collectionName.findOne();`**
( To retrieve only one document data from a collection whichever comes first it will retrieve it. )

**`db.collectionName.find().limit(number of documents you want to retrieve);`**
( To retrieve a specified number of documents data from a collection. )
Eg: **`db.collectionName.find().limit(2);`**
( it will return only the first two documents data from the collection. )

**`db.collectionName.find({name: "hameed"})`**
( To retrieve a specific documents data based on certain condition of the field from a collection. )
Eg: **`db.collectionName.find({age: 27});`**
( it will retrieve all the documents data whose age is 27. )

( To Find Specific Fields )

```
db.posts.find({ title: 'Post One' }, {
  title: 1,
  author: 1
})
```

**`db.getCollectionInfos({name:"collectionName"});`**
( To Check the Validation information of a Collection. )

**`db.posts.find().sort({ title: 1 }).pretty();`**
( To sort row or documents in ascending order )

**`db.posts.find().sort({ title: -1 }).pretty();`**
( To sort row or documents in descending order )

If you want to sort records by the creation date or another field and only show the oldest ones, you can use sort() and limit().
**`db.collectionName.find().sort({ createdAt: 1 }).limit(10);`**
This query retrieves the 10 oldest records by sorting in ascending order of createdAt.

In MongoDB, when you use the sort() method, the number 1 and -1 indicate the order in which you want to sort the documents:

'1': Sorts in ascending order (smallest to largest). For a date, this means from the oldest to the newest.
'-1': Sorts in descending order (largest to smallest). For a date, this means from the newest to the oldest.

**`db.posts.find().limit(2).pretty();`**
( To limit rows or documents )

**`db.posts.find().limit(2).sort({ title: 1 }).pretty();`**
( To limit and sort at the same time, also known as chaining )

**`db.posts.find().count();`**
**`db.posts.find({ category: 'news' }).count();`**
( To count rows or documents )

( Using forEach method )

```
db.posts.find().forEach(function(doc) {
  print("Blog Post: " + doc.title)
})
```

( To Find By Element in Array (\$elemMatch) )

```
db.posts.find({
  comments: {
     $elemMatch: {
       user: 'Mary Williams'
       }
    }
  }
)
```

( Text Search )

```
db.posts.find({
  $text: {
    $search: "\"Post O\""
    }
});
```

( Greater & Less Than )

```
db.posts.find({ views: { $gt: 2 } })
db.posts.find({ views: { $gte: 7 } })
db.posts.find({ views: { $lt: 7 } })
db.posts.find({ views: { $lte: 7 } })
```

## UPDATE / MODIFY DATABASE:

( To udate row / documents)

```
db.posts.update({ title: 'Post Two' },
{
  title: 'Post Two',
  body: 'New body for post 2',
  date: Date()
},
{
  upsert: true
})
```

( To Update Specific Field )

```
db.posts.update({ title: 'Post Two' },
{
  $set: {
    body: 'Body for post 2',
    category: 'Technology'
  }
})
```

( To Increment Field (\$inc) )

```
db.posts.update({ title: 'Post Two' },
{
  $inc: {
    likes: 5
  }
})
```

( To Rename a Field )

```
db.posts.update({ title: 'Post Two' },
{
  $rename: {
    likes: 'views'
  }
})
```

**`db.collectionName.updateOne({name:"hameed"}, {$set:{name:"Faizan", age:30}});`**
( To update a single data or document of a collection based on certain conditions, here first parameter filter the data and second parameter replace the data. )

**`db.collectionName.updateMany({age:30}, {$set:{name:"Rohit", age:28}});`**
( To update multiple data or document of a collection based on certain conditions, here first parameter filter the data and second parameter replace the data. )

( Sub-Documents )

```
db.posts.update({ title: 'Post One' },
{
  $set: {
    comments: [
      {
        body: 'Comment One',
        user: 'Mary Williams',
        date: Date()
      },
      {
        body: 'Comment Two',
        user: 'Harry White',
        date: Date()
      }
    ]
  }
})
```

( To Add Index )
**`db.posts.createIndex({ title: 'text' })`**

## Note

commands like:  
`insert()`, `update()` are deprecated.  
So for `insert()` use `insertOne()` or `insertMany()` and for `update()` use `updateOne()` or `updateMany()`

## DELETE DATABASE:

**`db.posts.remove({ title: 'Post Four' });`**
( To delete a row or document )

**`db.dropDatabase();`**
( To delete a database. )

**`db.collectionName.drop();`**
( To delete a collection. )

**`db.collectionName.deleteOne({age:30});`**
( To delete one data or document from a collection based on certain conditions. )

**`db.collectionName.deleteMany({age:30});`**
( To delete multiple data or document of a collection based on certain conditions. )

## AUTHENTIATION / AUTHORIZATION / ROLES IN DATABASE:

**`db.createUser({user:"hameed", pwd:"123456", roles:[{role:"read", db:"school"}]});`**
( To create a user and grant permission to access the database )

**`show users;`**
( To see users in MongoDB database. )

**`show roles;`**
( To see the roles of every user in a database. )
