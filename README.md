# Zen Class MongoDB Database

## Collections:
1. **users** - Stores user details
2. **codekata** - Stores problems solved by users
3. **attendance** - Stores user attendance
4. **topics** - Stores topics covered
5. **tasks** - Stores assigned tasks
6. **company_drives** - Stores company placement drives
7. **mentors** - Stores mentor details

## Queries:

### 1. Find all topics and tasks in October 2020
```js
 db.topics.find({ date: { $gte: ISODate("2020-10-01"), $lte: ISODate("2020-10-31") } })
 db.tasks.find({ topic_id: { $in: db.topics.distinct("_id", { date: { $gte: ISODate("2020-10-01"), $lte: ISODate("2020-10-31") } }) } })
```

### 2. Find all company drives between 15-Oct-2020 and 31-Oct-2020
```js
 db.company_drives.find({ drive_date: { $gte: ISODate("2020-10-15"), $lte: ISODate("2020-10-31") } })
```

### 3. Find all company drives and students who appeared
```js
 db.company_drives.find({}, { company_name: 1, students_appeared: 1 })
```

### 4. Find number of problems solved by users in Codekata
```js
 db.codekata.find({}, { user_id: 1, problems_solved: 1 })
```

### 5. Find mentors with more than 15 mentees
```js
 db.mentors.find({ "mentees.15": { $exists: true } })
```

### 6. Find users who were absent and didn’t submit tasks between 15-Oct-2020 and 31-Oct-2020
```js
 db.users.find({
   _id: {
     $in: db.attendance.distinct("user_id", { date: { $gte: ISODate("2020-10-15"), $lte: ISODate("2020-10-31") }, status: "Absent" })
   },
   _id: {
     $in: db.tasks.distinct("user_id", { submitted: false, topic_id: { $in: db.topics.distinct("_id", { date: { $gte: ISODate("2020-10-15"), $lte: ISODate("2020-10-31") } }) } })
   }
 })
```

## Expected Output Examples:
- **Query 2 Output (Company Drives):**
```json
[
  { "company_name": "Amazon", "drive_date": "2020-10-25", "students_appeared": ["User1"] },
  { "company_name": "Google", "drive_date": "2020-10-20", "students_appeared": ["User2", "User3"] }
]
```
- **Query 4 Output (Codekata Problems Solved):**
```json
[
  { "user_id": "User1", "problems_solved": 120 },
  { "user_id": "User2", "problems_solved": 80 }
]
```

This README provides a clear guide to the MongoDB queries for the Zen Class database. 🚀

