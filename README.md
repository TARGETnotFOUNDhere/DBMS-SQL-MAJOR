Absolutely! 🔥 Here’s a **clean, GitHub-ready README.md** for your **Coaching Test Management System**. You can directly copy this into your repository.

```markdown
# 🎓 Coaching Test Management System

A PostgreSQL-based database management system designed to manage coaching institute tests, students, test sections, attempts, and section-wise scores.

The project demonstrates relational database design, SQL queries, joins, aggregate functions, reporting, indexing, and query optimization using `EXPLAIN ANALYZE`.

---

## 📌 Problem Statement

Coaching institutes conduct multiple tests for students across different sections such as Physics, Chemistry, Mathematics, etc.

Managing students, tests, attempts, and section-wise scores manually can become difficult.

The **Coaching Test Management System** provides a structured PostgreSQL database to:

- Store student information
- Manage tests and test sections
- Record student attempts
- Store section-wise marks
- Calculate scores and percentages
- Identify students performing below a particular threshold
- Generate performance reports
- Analyze and optimize SQL query performance

---

## 🎯 Objectives

The main objectives of this project are:

- Design a normalized relational database.
- Maintain student and test information.
- Record test attempts.
- Store section-wise scores.
- Generate meaningful performance reports.
- Use SQL joins and aggregate functions.
- Identify students scoring below 40%.
- Rank students based on performance.
- Improve query performance using indexes.
- Analyze query execution using `EXPLAIN ANALYZE`.

---

## 🛠️ Technologies Used

| Technology | Purpose |
|---|---|
| PostgreSQL | Database Management System |
| pgAdmin 4 | Database interface |
| SQL | Database queries |
| ER Diagram | Database relationship design |
| EXPLAIN ANALYZE | Query performance analysis |

---

## 🗄️ Database Structure

The database is named:

```text
coaching_test_db
```

The project contains **6 tables**:

```text
students
tests
attempts
sections
test_sections
scores
```

### 1. Students

Stores information about students.

| Column | Type | Description |
|---|---|---|
| student_id | SERIAL | Primary Key |
| student_name | VARCHAR(100) | Student name |
| email | VARCHAR(150) | Student email |
| batch | VARCHAR(50) | Batch information |
| enrollment_date | DATE | Enrollment date |

---

### 2. Tests

Stores information about tests.

| Column | Type | Description |
|---|---|---|
| test_id | SERIAL | Primary Key |
| test_name | VARCHAR(100) | Test name |
| test_date | DATE | Test date |
| total_marks | INTEGER | Maximum marks |

---

### 3. Attempts

Stores student test attempts.

| Column | Type | Description |
|---|---|---|
| attempt_id | SERIAL | Primary Key |
| student_id | INTEGER | Foreign Key |
| test_id | INTEGER | Foreign Key |
| attempt_date | DATE | Date of attempt |

---

### 4. Sections

Stores test sections/subjects.

| Column | Type | Description |
|---|---|---|
| section_id | SERIAL | Primary Key |
| section_name | VARCHAR(100) | Section name |

Examples:

```text
Physics
Chemistry
Mathematics
```

---

### 5. Test Sections

Connects tests with their sections.

| Column | Type | Description |
|---|---|---|
| test_id | INTEGER | Foreign Key |
| section_id | INTEGER | Foreign Key |
| max_marks | INTEGER | Maximum marks for section |

This table works as a **bridge table** between `tests` and `sections`.

---

### 6. Scores

Stores marks obtained by students in individual sections.

| Column | Type | Description |
|---|---|---|
| score_id | SERIAL | Primary Key |
| attempt_id | INTEGER | Foreign Key |
| section_id | INTEGER | Foreign Key |
| marks_obtained | NUMERIC(5,2) | Marks obtained |

---

## 🔗 Entity Relationships

The main relationships are:

```text
Students
   │
   │ 1 : Many
   ▼
Attempts
   │
   │ 1 : Many
   ▼
Scores
   ▲
   │
   │ Many : 1
Sections
```

Tests are connected through attempts:

```text
Students ────────< Attempts >──────── Tests
                       │
                       │
                       ▼
                    Scores
                       ▲
                       │
                   Sections
```

Tests and sections are connected using the `test_sections` bridge table:

```text
Tests ───────< Test_Sections >─────── Sections
```

---

## 🔄 System Workflow

```text
Student Registration
        ↓
Create Test
        ↓
Create Test Sections
        ↓
Student Attempts Test
        ↓
Store Section-wise Scores
        ↓
Calculate Total Score
        ↓
Calculate Percentage
        ↓
Generate Performance Reports
        ↓
Analyze Query Performance
```

---

## 📊 SQL Features Demonstrated

The project uses several important SQL concepts:

### INNER JOIN

Used to combine related records from multiple tables.

```sql
SELECT s.student_name,
       t.test_name,
       sc.marks_obtained
FROM students s
JOIN attempts a
    ON s.student_id = a.student_id
JOIN tests t
    ON a.test_id = t.test_id
JOIN scores sc
    ON a.attempt_id = sc.attempt_id;
```

---

### GROUP BY

Used for grouping records and generating reports.

```sql
GROUP BY s.student_id, s.student_name;
```

---

### SUM()

Used to calculate total marks.

```sql
SUM(sc.marks_obtained)
```

---

### AVG()

Used to calculate average performance.

```sql
AVG(sc.marks_obtained)
```

---

### HAVING

Used to filter grouped results.

Example: finding students scoring below 40%.

```sql
HAVING SUM(sc.marks_obtained) < t.total_marks * 0.40;
```

---

### ORDER BY

Used for sorting results.

```sql
ORDER BY percentage;
```

---

### ROUND()

Used to format calculated values.

```sql
ROUND(AVG(sc.marks_obtained), 2)
```

---

## 📈 Reports Generated

The database can generate several useful reports:

### Student-wise Test Scores

Displays:

- Student
- Test
- Test date
- Total score

### Section-wise Average

Displays average performance for each section.

### Students Below 40%

Identifies students who scored less than 40%.

### Student Ranking

Students can be ranked according to their average performance.

### Lowest Performing Students

Helps identify students who may require additional academic support.

---

## ⚡ Query Optimization

Query optimization was also implemented using PostgreSQL indexes.

An index was created on the attempt date:

```sql
CREATE INDEX idx_attempts_attempt_date
ON attempts(attempt_date);
```

A query can then be analyzed using:

```sql
EXPLAIN ANALYZE
SELECT *
FROM attempts
WHERE attempt_date = DATE '2026-01-04';
```

A composite index was also used for combined filtering:

```sql
CREATE INDEX idx_attempts_student_date
ON attempts(student_id, attempt_date);
```

Then:

```sql
EXPLAIN ANALYZE
SELECT *
FROM attempts
WHERE student_id = 500
  AND attempt_date = DATE '2026-01-04';
```

PostgreSQL can use an **Index Scan** for selective queries, reducing the amount of data that needs to be examined.

---

## 🔍 Query Performance

`EXPLAIN ANALYZE` was used to compare different execution plans.

For example:

```text
Sequential Scan
       ↓
Bitmap Index Scan
       ↓
Index Scan
```

The actual execution plan selected by PostgreSQL depends on table size, indexes, statistics, and how selective the query condition is.

---

## 📂 Project Structure

A suggested GitHub repository structure:

```text
Coaching-Test-Management-System/
│
├── README.md
│
├── sql/
│   ├── database_schema.sql
│   ├── sample_data.sql
│   ├── reports.sql
│   └── optimization.sql
│
├── documentation/
│   └── Project_Report.pdf
│
├── screenshots/
│   ├── er_diagram.png
│   ├── database_tables.png
│   ├── query_results.png
│   └── query_optimization.png
│
└── LICENSE
```

---

## 🚀 How to Run the Project

### Step 1 — Install PostgreSQL

Install PostgreSQL on your system.

### Step 2 — Open pgAdmin 4

Open **pgAdmin 4** and connect to your PostgreSQL server.

### Step 3 — Create Database

Create a database:

```sql
CREATE DATABASE coaching_test_db;
```

### Step 4 — Open Query Tool

Select:

```text
coaching_test_db
→ Query Tool
```

### Step 5 — Create Tables

Run the database schema SQL file.

### Step 6 — Insert Data

Run the sample data SQL file.

### Step 7 — Run Reports

Execute the report queries to generate:

- Student scores
- Section averages
- Low-performing students
- Rankings
- Performance statistics

### Step 8 — Analyze Performance

Run the optimization queries using:

```sql
EXPLAIN ANALYZE
```

---

## 📸 Project Screenshots

Screenshots of the following are included in the project documentation:

- ER Diagram
- Database tables
- SQL queries
- SQL result tables
- Student performance reports
- Section averages
- Students below 40%
- Query execution plans
- Index optimization

---

## 🎓 Learning Outcomes

Through this project, the following concepts were practiced:

- Relational database design
- Primary Keys
- Foreign Keys
- Table relationships
- One-to-many relationships
- Bridge tables
- SQL joins
- Aggregate functions
- GROUP BY
- HAVING
- Sorting and filtering
- Database indexing
- Query optimization
- EXPLAIN ANALYZE
- PostgreSQL and pgAdmin 4

---

## 👨‍💻 Author

**Lakshya Purohit**

B.Tech CSE  
ITM Skills University

---

## 📌 Project Status

```text
✅ Database Design
✅ ER Diagram
✅ Table Creation
✅ Data Management
✅ SQL Queries
✅ Performance Reports
✅ Indexing
✅ Query Optimization
✅ EXPLAIN ANALYZE
✅ Documentation
```

---

## ⭐ Conclusion

The **Coaching Test Management System** provides a structured and efficient PostgreSQL database for managing coaching test activities.

The project demonstrates how relational databases can be used to store and analyze student performance while maintaining relationships between students, tests, sections, attempts, and scores.

It also demonstrates practical database optimization through indexing and `EXPLAIN ANALYZE`.

---

**Made with PostgreSQL & SQL 💙**
```
