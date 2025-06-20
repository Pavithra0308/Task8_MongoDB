# 📚 Zen Class Programme – MongoDB Project
This project demonstrates how to design and query a **MongoDB** database for a learning management system called **Zen Class**. It includes collections for users, topics, tasks, codekata, attendance, company drives, and mentors.

## 📁 Collections
- users: Information about enrolled students.
- topics: Topics taught, with date and batch.
- tasks: Assignments given to students.
- codeketa: Tracks coding practice problems solved.
- attendance: Daily attendance data.
- company_drive: Placement drive details.
- mentors: Mentor information and mentee counts.

## Code for creating Collections:

**Users**
Zen_Class_Programme> db.users.insertMany([{name: "Pavithra", email: "pavi@gmail.com",batch: "FSD-25", program: "zen class", mentor_id: 1, codeketa_score:"90", placement_status:"appeared"},{name: "Rahul", email: "rahul@gmail.com",batch: "FSD-25", program: "zen class", mentor_id: 2, codeketa_score:"80", placement_status:"appeared"},{name: "Sneha", email: "sneha@gmail.com",batch: "FSD-25", program: "zen class", mentor_id: 1, codeketa_score:"75", placement_status:"not appeared"}])

**Topics**
Zen_Class_Programme> db.topics.insertMany([{title: "React JS", date: ISODate("2020-10-12"),batch: "FSD-25"},{title:"Mongodb", date: ISODate("2020-10-22"),batch: "FSD-25"},{title: "Javascript", date: ISODate("2020-10-18"),batch: "FSD-25"}])

**Tasks**
Zen_Class_Programme> db.tasks.insertMany([{task: "Build To Do App", date:ISODate("2020-10-27"),},{task: "Movie Review App", date: ISODate("2020-10-30")}])

**Codeketa**
Zen_Class_Programme> db.codeketa.insertMany([{user_id:ObjectId('68553599ec46063dcc748a5f'), problems_solved:75, last_updated:ISODate('2020-10-25')},{user_id:ObjectId('6855365aec46063dcc748a60'),problems_solved:80, last_updated: ISODate('2020-10-27')},{user_id: ObjectId('6855365aec46063dcc748a61'), problems_solved: 110, last_updated: ISODate('2020-10-26')}])

**Attendence**
Zen_Class_Programme> db.attendence.insertMany([{user_id:ObjectId('68553599ec46063dcc748a5f'), date: ISODate('2020-10-15'),present: true},{user_id:ObjectId('6855365aec46063dcc748a60'), date: ISODate('2020-10-15'), present: false},{user_id: ObjectId('6855365aec46063dcc748a61'),date: ISODate('2020-10-20'), present: true}])

**Company_drives**
Zen_Class_Programme> db.company_drive.insertMany([{user_id:ObjectId('68553599ec46063dcc748a5f'), date: ISODate('2020-10-16'), company: 'TCS'},{user_id:ObjectId('6855365aec46063dcc748a60'), date: ISODate('2020-10-25'), company: 'Infosys'},{user_id: ObjectId('6855365aec46063dcc748a61'),date: ISODate('2020-11-01'), company: 'Wipro'}])

**Mentors**
Zen_Class_Programme> db.mentors.insertMany([{name: 'Jayanthi', mentor_id: 1, mentees: 20},{name: 'Vishnu',mentor_id: 2, mentees: 10},{name: 'Banu',mentor_id:3,mentees: 
24}])

## 🔍 Queries

**1. Topics & Tasks in October**

Zen_Class_Programme> db.topics.find({date: {$gte: ISODate('2020-10-01'), $lte: ISODate('2020-10-31')}})
Zen_Class_Programme> db.tasks.find({date: {$gte: ISODate('2020-10-01'), $lte: ISODate('2020-10-31')}})

**2. Drives between Oct 15–31**
Zen_Class_Programme> db.company_drive.find({date: {$gte: ISODate('2020-10-15'), $lte: ISODate('2020-10-31')}})

**3. Find all the company drives and students who are appeared for the placement.**
Zen_Class_Programme> db.company_drive.aggregate([{$lookup: {from: 'users',localField: 'user_id', foreignField: '_id', as: 'user_details'}},{$unwind: '$user_details'},{$match: {'user_details.placement_status': 'appeared'}}]).pretty()

**4. Find the number of problems solved by the user in codekata**
Zen_Class_Programme> db.codeketa.aggregate([{$lookup: {from: 'users',localField: 'user_id', foreignField: '_id', as: 'user'}},{$unwind: '$user'}, {$project: {_id: 0, username: '$user.name',problems_solved: 1}}]).pretty()

**5. Find all the mentors with who has the mentee's count more than 15**
Zen_Class_Programme> db.mentors.find({mentees: {$gt: 15}})

**6. Find the number of users who are absent and task is not submitted  between 15 oct-2020 and 31-oct-2020**
Zen_Class_Programme> db.attendence.aggregate([{ $lookup: { from: 'tasks', localField: 'user_id', foreignField: 'user_id', as: 'tasks' } },{$unwind: '$tasks'}, { $match: {'present': false, 'tasks.submitted': false, 'tasks.date': { $gte: ISODate('2020-10-15'), $lte: ISODate('2020-10-31') } } }])


