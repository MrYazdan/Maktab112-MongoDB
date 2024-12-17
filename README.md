# 🗃️ MongoDB Shell Commands and Notes

## Display and Select Databases

- **Show all databases:**
  ```shell
  show dbs
  ```
- **Switch to a specific database (or create one if it doesn't exist):**
  ```shell
  use maktab
  ```
- **Delete the current database:**
  ```shell
  db.dropDatabase()
  ```

## Collections (Tables in SQL)

- **Show all collections in the current database:**
  ```shell
  show collections
  ```
- **Create collections:**
  1. Explicitly:
     ```shell
     db.createCollection('admin')
     ```
  2. Implicitly (by inserting a document):
     ```shell
     db.posts.insertOne({})
     ```
- **Delete collections:**
  ```shell
  db.posts.drop()
  db.admin.drop()
  ```

## CRUD Operations

### Create

- Insert a single document:
  ```shell
  db.users.insertOne({
      name: "Reza",
      age: 30,
      skills: ["Python", "Mongo", "Django", "Linux"]
  })
  ```
- Insert multiple documents:
  ```shell
  db.users.insertMany([
      {
          name: "Ali",
          age: 25,
          skills: ["JavaScript", "Node.js"],
          phone: "09123456789"
      },
      {
          name: "Sara",
          age: 28,
          skills: ["HTML", "CSS"],
          is_active: false
      }
  ])
  ```

### Read

- Fetch all documents:
  ```shell
  db.users.find()
  ```
- Fetch documents with conditions:
  ```shell
  db.users.find({ age: { $gt: 25 } })
  ```
- Fetch a single document:
  ```shell
  db.users.findOne({ age: { $gt: 25 } })
  ```
- Access a specific field in a document:
  ```shell
  db.users.findOne({ name: "Reza" }).phone
  ```
- Transform results:
  ```shell
  db.users.find().map(i => i._id)
  db.users.find().forEach(i => print(i._id))
  ```

### Update

- Update a single document:
  ```shell
  db.users.updateOne(
      { name: "Reza" },
      { $set: { status: "young" } }
  )
  ```
- Remove a field:
  ```shell
  db.users.updateOne(
      { name: "Reza" },
      { $unset: { status: "" } }
  )
  ```
- Update multiple documents:
  ```shell
  db.users.updateMany(
      { age: { $lt: 30 } },
      { $set: { status: "young" } }
  )
  ```
- Add nested fields:
  ```shell
  db.users.updateOne(
      { name: "Reza" },
      { $set: { address: { city: "Isfahan", code: "12345" } } }
  )
  ```
  ```shell
  db.users.updateOne(
      { name: "Ali" },
      { $set: { address: { city: "Shiraz", code: "89764" } } }
  )
  ```
  ```shell
  db.users.updateOne(
      { name: "Sara" },
      { $set: { address: { city: "Hamedan", code: "98765" } } }
  )
  ```

### Delete

- Delete a single document:
  ```shell
  db.users.deleteOne({ name: "Ali" })
  ```

## Projection (Selective Fields)

- Exclude fields:
  ```shell
  db.users.find({}, { _id: 0, is_active: 0 })
  ```
- Include specific fields:
  ```shell
  db.users.find({}, { age: 1, name: 1, _id: 0 })
  db.users.find({}, { _id: 0, name: 1, address: 1 })
  db.users.find({}, { _id: 0, name: 1, "address.city": 1 })
  db.users.find({ age: { $gt: 25 } }, { _id: 0, name: 1 })
  ```
- Exclude nested fields:
  ```shell
  db.users.find({}, { name: 0, "address.code": 0 })
  ```
- Slice arrays:
  ```shell
  db.users.find({}, { _id: 0, name: 1, skills: { $slice: -1 } })
  ```

